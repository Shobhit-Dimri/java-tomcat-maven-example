pipeline {
    agent any
    stages {
        stage('Cleanup Workspace') {
            steps {
                echo "Cleaned Up Workspace For Project"
                echo "Branch Name: ${env.BRANCH_NAME}"
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
                    env.pteEnv = input message: 'Select CTE env in which you want to deploy the build', ok: 'Deploy!',
                            parameters: [choice(name: 'DEPLOY BUILD', choices: 'All\nCTE-1\nCTE-2\nCTE-3\nCTE-4', description: "Which CTE environment?")]
                }
                echo "PTE environment selected ${env.pteEnv}"
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
                echo "CTE environment selected ${env.cteEnv}"
            }
        }
        stage('Deploy for PPE environment') {
            when {
                branch 'release'
            }
            steps {
                script {
                    env.ppeEnv = input message: 'Select CTE env in which you want to deploy the build', ok: 'Deploy!',
                            parameters: [choice(name: 'DEPLOY BUILD', choices: 'All\nCTE-1\nCTE-2\nCTE-3\nCTE-4', description: "Which CTE environment?")]
                }
                echo "PPE environment selected ${env.ppeEnv}"
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
