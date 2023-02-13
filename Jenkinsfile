pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage("incrmeent version") {
                    steps {
                        script{
                            echo 'incrementing app version...'
                            sh 'mvn build-helper:parse-version versions:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} versions:commit'
                            def matcher = readFile('pom.xml') =~ '<version>(.+)</version>' //parse xml file and read value from it
                            def version = matcher[0][1]     //match every version tag in the pom.xml so we need the first one
                            env.IMAGE_NAME="$version-$BUILD_NUMBER"   //BUILD_NUMBER env variable from jenkins
                        }                                             //one of common ways of versionnig docker image
                    
                    }
                }
        stage("build app") {
                steps {
                    script{
                       echo "building the application..."
                       sh 'mvn clean package' //builds jar file with version from pom.xml
                                              // mvn clean package : clean the target folder with old jar file so the new one will be created with a single jar file
                    }
                }
            }
        stage("build image") {
            steps {
                script{
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t splashdocker1/java-maven-app:$IMAGE_NAME ."
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push splashdocker1/java-maven-app:$IMAGE_NAME"}
                }
            }
        }
        stage("deploy") {
            steps {
             script{
                   echo 'deploying docker image to EC2..'
                }
          }
        }
        stage("commit version update in git repo") { //access git and commit changes to it
            steps {
             script{
                 withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                    sh  'git config --global user.email "jenkins@example.com"' //globalby for all the projects  
                    sh  'git config --global user.name "jenkins"' // wee could too ssh to jenkins server and set the config

                    sh 'git status'
                    sh 'git branch'
                    sh 'git config --list' 

                    sh "git remote set-url origin https://${USER}:${PASS}@github.com/Splashx0/java-maven-app.git"  //jenkins need to connect to  git repo with credentials
                    sh 'git add .'
                    sh 'git commit -m "ci: version bump"'
                    sh 'git push origin HEAD:jenkins-versionning'
                 }
                }
          }
        }
    }   
}
