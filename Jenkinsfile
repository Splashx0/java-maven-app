def gv //global variable

pipeline {
    agent any
    parameters{
        choice(name:'VERSION',choices:['1.1.0','1.2.0','1.3.0'],description:'')
        booleanParam(name:'executeTests',defaultValue:true,description:'')
    }
    stages {
        stage("init") { 
                steps {
                    script{
                        gv = load "script.groovy" //global 
                    }
                }
            }
        stage("build") { 
            steps {
                scripts{
                     gv.buildApp()
                }
            }
        }
        stage("test") {
            steps {
             scripts{
                     gv.testApp()
                }           
          }
        }
        stage("deploy") {
            steps {
                 scripts{
                     gv.deployApp()
                }
            }
        }
    }   
}
