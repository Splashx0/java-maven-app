library identifier:  'jenkins-shared-library@master',retriever:modernSCM(
    [   $class:'GitSCMsource',
        remote:'https://github.com/Splashx0/jenkins-shared-library.git',
        credentials:'github-credentials'])
//@Library('jenkins-shared-library')
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
