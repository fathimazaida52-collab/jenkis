pipeline {
    agent any
    environment {
        GITHUB_CREDS = credentials('github-packages-cred')
        JAVA_HOME = tool name: 'kooi'
        MAVEN_HOME = tool name: 'maven---1'
        PATH = "${JAVA_HOME}\\bin;${env.PATH}"
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Build & Deploy') {
            steps {
                configFileProvider([configFile(fileId: 'maven-github-settings', variable: 'MAVEN_SETTINGS')]) {
                    bat """
                        set GH_USER=%GITHUB_CREDS_USR%
                        set GH_TOKEN=%GITHUB_CREDS_PSW%
                        "%MAVEN_HOME%\\bin\\mvn.cmd" -s "%MAVEN_SETTINGS%" -B clean package
                        "%MAVEN_HOME%\\bin\\mvn.cmd" -s "%MAVEN_SETTINGS%" -B deploy
                    """
                }
            }
        }
    }
    post {
        success {
            echo "Build and deployment completed successfully."
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
