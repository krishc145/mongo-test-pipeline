pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'
        PATH = "${env.JAVA_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Run Mongo Connector') {
            steps {
                bat '''
                groovy -cp "K:\\1.Rushi technologies - 6AM - May 2025 batch\\shared library\\shared-lib-mongo-connector\\lib\\mongodb-driver-sync-5.5.0.jar;K:\\1.Rushi technologies - 6AM - May 2025 batch\\shared library\\shared-lib-mongo-connector\\scripts" K:\\1.Rushi technologies - 6AM - May 2025 batch\\shared library\\shared-lib-mongo-connector\\scripts\\mongo-connector.groovy
                '''
            }
        }
    }
}
