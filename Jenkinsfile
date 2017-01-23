node {
    env.GENERATE_REPORTS = 'true'
    env.CI = 'true'
    def yamlFilename = "main-${environment}.yml"
    def workDir = pwd()
    def stackName = "${clusterName}-${environment}"
    def fullStackName = "${stackName}-${env.BUILD_NUMBER}"

    catchError {
        stage 'Checkout'

        deleteDir()

        retry(3) {
            checkout scm
        }


        stage 'Prep'

        sh "aws cloudformation validate-template --template-body file:///${workDir}/${yamlFilename} --region ${region}"
        
        stage 'Deploy'

        sh """aws cloudformation create-stack --stack-name ${fullStackName} \
                --region ${region} --template-body file:///${workDir}/${yamlFilename} \
                --parameters \
                    ParameterKey=MinNumInstances,ParameterValue=1 \
                    ParameterKey=MaxNumInstances,ParameterValue=1 \
                    ParameterKey=DesiredNumInstances,ParameterValue=1 \
                    ParameterKey=ImageID,ParameterValue=${amiId} \
                    ParameterKey=EC2InstanceType,ParameterValue=${instanceType} \
                    ParameterKey=KeyPairName,ParameterValue=${key} \
                    ParameterKey=VPCId,ParameterValue=${vpc_id} \
                    ParameterKey=SubnetsToUse,ParameterValue='\"${subnets}\"' \
                    ParameterKey=GaleraClusterName,ParameterValue=${clusterName} \
                    ParameterKey=PackageVersion,ParameterValue=${version} \
                    ParameterKey=ConsulServerAddress,ParameterValue=${consulServer} \
                    ParameterKey=Environment,ParameterValue=${environment} \
                    ParameterKey=SnapshotID,ParameterValue=${snapshotId} \
                    ParameterKey=SnapshotVolumeSize,ParameterValue=${snapshotVolumeSize} \
                    ParameterKey=SessionTimeout,ParameterValue=${sessionIdleTimeout} \n
                    
              aws cloudformation wait stack-create-complete \
                --stack-name ${fullStackName} --region ${region} \n
             """


        stage 'Verify'

        stage 'Promote'

        
    }
}
