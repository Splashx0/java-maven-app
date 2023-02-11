@Library('jenkins-shared-library')
def gv

pipeline {
    agent any
    tools {
        maven 'Maven'
    }

    stages {
        stage("init"){
            steps{
                script{
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") { 
                steps {
                    script{ 
                        buildJar()
                    }
                }
            }
        stage("build  and push image") { 
            steps {
                script{
                    buildImage 'splashdocker1/java-maven-app:3.0'
                    dockerLogin()
                    dockerPush 'splashdocker1/java-maven-app:3.0'
                }
            }
        }
        stage("deploy") {

            steps {
             script{
                    gv.deployApp()
                }           
          }
        }
    }   
}
