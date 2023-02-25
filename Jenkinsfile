pipeline {
    agent any
    stages {
        stage("test"){
            steps{
                script{
                    echo "testing the trigger for multibranch-pipeline"
                    echo "testing the application..."
                    echo "Executing pipeline for branch $BRANCH_NAME"
                }
            }
        }
        stage("build") {
  
                steps {
                    script{ 
                        echo "building the application..."
                    }
                }
            }
        stage("deploy") {  
                steps {
                script{
                        echo "deploying the application..."
                        def dockerCmd="docker run -p 3080:80 -d rizkyaditomo/react-nodejs-example:v1"
                        sshagent(['ec2-server-key']){
                            sh "ssh -o StrictHostKeyChecking=no ec2-user@3.238.138.75 ${dockerCmd}"
                        }
                    }           
            }
        }
    }   
}
