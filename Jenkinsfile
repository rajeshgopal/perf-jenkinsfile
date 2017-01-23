node {
    env.GENERATE_REPORTS = 'true'
    env.CI = 'true'
    def yamlFilename = "acmeair-app-aws-cfn.yml"
    def workDir = pwd()
    def stackName = "Acmeair-dev-${env.BUILD_NUMBER}"
    def region = "us-east-1"
    
    catchError {
        stage 'Checkout'

        deleteDir()

        retry(3) {
            checkout scm
        }


        stage 'Prep'

        
        stage 'Deploy Test App - Acmeair'

        sh """aws cloudformation create-stack --stack-name ${stackName} \
                --region ${region} --template-body file:///${workDir}/${yamlFilename} \n
                
              aws cloudformation wait stack-create-complete \
                --stack-name ${stackName} --region ${region} \n
             """


        stage 'Invoke Perfaccion'

        
        
    }
}
