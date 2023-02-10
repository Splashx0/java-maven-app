pipeline {
    agent any
    stages {
        stage("test"){
            steps{
                script{
                    echo "testing the application..."
                    echo "Executing pipeline for branch $BRANCH_NAME"
                }
            }
        }
        stage("build") {
                when{
                    expression{
                        BRANCH_NAME = "master"
                    }
                } 
                steps {
                    script{ 
                        gv.buildJar()
                    }
                }
            }
        stage("deploy") {
                when{
                    expression{
                        BRANCH_NAME = "master"
                        }
                    }   
                steps {
                script{
                        gv.deployApp()
                    }           
            }
        }
    }   
}
