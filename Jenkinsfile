def gv
pipeline {
    agent any
    parameters{
       choice(name:"VERSION",choices:['1.1.0','1.2.0'])
       booleanParam(name:"executeTests", defaultValue:false,description:'')
    }
    

    stages {
       stage('init') {
            steps {script{
                gv = load "script.groovy"
                }}
        }
        stage('Build') {
            steps {
                script{ gv.buildApp()
                    }
               
                }
        }

        stage('test') {
            when{
                expression{
                    params.executeTests == true
                }
                
            }
                    steps {
                        script{
                          gv.testApp()
                        }
                    }
        }
        stage('deploy') {
        
                    input{
                        message "choose the envrironment to deploy to"
                        ok "Done"
                        parameters{
                            choice(name:"ENV",choices:['prod','dev'])
                        }
                    }
                    steps {
                        script{
                          env.ENV2 = input message: "to select the param and store it we have to write it in script ", ok "done", parameters: choice(name:"ENV",choices:['prod','dev'])
                          gv.deployApp()
                          echo "deploying to ${ENV} and ${ENV2}"
                        }
                  
                    }
    
            
        }
    }
}
