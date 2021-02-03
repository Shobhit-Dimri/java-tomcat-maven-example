env.Version_ID = params.Version_ID
env.Build_ID = params.Build_ID
env.Client = params.Client
env.BaseEnv_ID = params.BaseEnv_ID

pipeline {
    agent any
    stages {
        stage ("Updating code from SCM") {
            steps {
                git credentialsId: 'git_credentials', url: "https://github.com/Shobhit-Dimri/java-tomcat-maven-example.git"
            }
        }
        stage ("Creating Build") {
            steps {
                echo "Creating a Build..."
                sh  "mvn -version"
                sh  "mvn clean install -DargLine=”noverify” -DccBaseline=PIP-DEVELOP-${BUILD_NUMBER} -P deliver"
                echo "BUILD# ${BUILD_NUMBER}"
            }
            post{
                always{
                    cleanWs()
                }
            /*    success{
                    echo "Archiving the build...."
                    archiveArtifacts artifacts : "build/target/*war"
                }*/
            }
        }
        stage ("Upload build to Artifactory") {
            steps {
                echo "Upload artifactory to JFrog."
            }
        }
        stage('Select Environment') {
            when {
                branch 'feature*'
            }
            steps {
                script {
                    env.pteEnv = input message: 'Select PTE env in which you want to deploy the build', ok: 'Deploy!',
                            parameters: [choice(name: 'DEPLOY BUILD', choices: 'All\nPTE-1\nPTE-2\nPTE-3\nPTE-4', description: "Which PTE environment?")]
                }
                echo "Version ID: ${env.Version_ID}"
                echo "Build ID: ${env.Build_ID}"
                echo "Client ID: ${env.Client}"
                echo "BaseEnv ID: ${env.BaseEnv_ID}"
                echo "PTE environment selected ${env.pteEnv}"
            }
        }
    }
}
