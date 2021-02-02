env.Version_ID = params.Version_ID
env.Build_ID = params.Build_ID
env.Client = params.Client
env.BaseEnv_ID = params.BaseEnv_ID

pipeline {
    agent any
    parameters{

    }
    stages {
        stage ("Create Build") {
            steps {
                /*For windows machine */
               //bat  'mvn clean package'

                /*For Mac & Linux machine */
                echo "Creating a Build..."
                sh  "mvn -version"
                sh  "mvn clean install -DargLine=”noverify” -DccBaseline=PIP-DEVELOP-${BUILD_NUMBER} -P deliver"
            }

            post{
                always{
                    cleanWs()
                }
                success{
                    echo "Archiving the build...."
                    archiveArtifacts artifacts : 'Deliverabl*/target/*.zip'
                }
            }
        }

        stage ("Upload Build to JFrog repository"){
            steps{
                echo "Uploading build to JFrog repository"
            }
        }

        stage ('Deploy Build in PTE Environment'){
            steps{
                    timeout (time: 5, unit:'DAYS'){
                    input message: 'Approve PTE Deployment?'
                }
                // build job : 'Deploy_Servlet_Staging_Env'
                build job : 'Deploy_PTE_env_Pipeline'

            }
            post{
                success{
                    echo 'Deployment on PTE is Successful'
                }

                failure{
                    echo 'Deployment Failure on PTE'
                }
            }
        }
/*
        stage ('Deploy Build in CTE Environment'){
            steps{

                // build job : 'Deploy_Servlet_Staging_Env'
                build job : 'Deploy_CTE_env_Pipeline'

            }
        }

        stage ('Deploy Build in PPE Environment'){
            steps{

                // build job : 'Deploy_Servlet_Staging_Env'
                build job : 'Deploy_Staging_Area_Pipeline'

            }
        }

        stage ('Deploy Build to Production'){
            steps{
                timeout (time: 5, unit:'DAYS'){
                    input message: 'Approve PRODUCTION Deployment?'
                }
                
                build job : 'Deploy_Production_Pipeline'
            }

            post{
                success{
                    echo 'Deployment on PRODUCTION is Successful'
                }

                failure{
                    echo 'Deployment Failure on PRODUCTION'
                }
            }
        }
*/        
    }
}
