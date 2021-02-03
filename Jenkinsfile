env.Version_ID = params.Version_ID
env.Build_ID = params.Build_ID
env.Client = params.Client
env.BaseEnv_ID = params.BaseEnv_ID

pipeline {
    agent any
    stages {
        stage('Cleanup Workspace') {
            steps {
                echo "Cleaned Up Workspace For Project"
            }
        }
        stage ("Code Checkout") {
            steps {
                git credentialsId: 'git_credentials', url: "https://github.com/Shobhit-Dimri/java-tomcat-maven-example.git"
            }
        }
        stage(' Unit Testing') {
            steps {
                echo "Running Unit Tests"
            }
        }
        stage('Code Analysis') {
            steps {
                echo "Running Code Analysis"
            }
        }
        stage ("Build Artifactory") {
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
        stage('Deploy for PTE environment') {
            when {
                branch '^feature.*$'
            }
            steps {
                script {
                    env.cteEnv = input message: 'Select CTE env in which you want to deploy the build', ok: 'Deploy!',
                            parameters: [choice(name: 'DEPLOY BUILD', choices: 'All\nCTE-1\nCTE-2\nCTE-3\nCTE-4', description: "Which CTE environment?")]
                }
                echo "Version ID: ${env.Version_ID}"
                echo "Build ID: ${env.Build_ID}"
                echo "Client ID: ${env.Client}"
                echo "BaseEnv ID: ${env.BaseEnv_ID}"
                echo "CTE environment selected ${env.cteEnv}"
            }
        }
        stage('Deploy for CTE environment') {
            when {
                branch 'develop'
            }
            steps {
                script {
                    env.cteEnv = input message: 'Select CTE env in which you want to deploy the build', ok: 'Deploy!',
                            parameters: [choice(name: 'DEPLOY BUILD', choices: 'All\nCTE-1\nCTE-2\nCTE-3\nCTE-4', description: "Which CTE environment?")]
                }
                echo "Version ID: ${env.Version_ID}"
                echo "Build ID: ${env.Build_ID}"
                echo "Client ID: ${env.Client}"
                echo "BaseEnv ID: ${env.BaseEnv_ID}"
                echo "CTE environment selected ${env.cteEnv}"
            }
        }
        stage('Deploy for PPE environment') {
            when {
                branch 'release'
            }
            steps {
                script {
                    env.cteEnv = input message: 'Select CTE env in which you want to deploy the build', ok: 'Deploy!',
                            parameters: [choice(name: 'DEPLOY BUILD', choices: 'All\nCTE-1\nCTE-2\nCTE-3\nCTE-4', description: "Which CTE environment?")]
                }
                echo "Version ID: ${env.Version_ID}"
                echo "Build ID: ${env.Build_ID}"
                echo "Client ID: ${env.Client}"
                echo "BaseEnv ID: ${env.BaseEnv_ID}"
                echo "CTE environment selected ${env.cteEnv}"
            }
        }
        stage('Deliver master environment') {
            when {
                branch 'master'
            }
            steps {
                script {
                    env.masterEnv = input message: 'No environment for master !!!', ok: 'Deploy!'
                }
            }
        }
    }
}
