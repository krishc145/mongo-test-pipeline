pipeline {
    agent any

    environment {
        MONGO_USER = 'krishna-admin-group'
        MONGO_DB   = 'krishna_DB_admin-pp'
        COLLECTION_NAME = 'dba_admin_fruits-pp'
        // Example JSON data to insert
        DATA_TO_INSERT = '[{"name":"mango"},{"name":"apple"},{"name":"jackfruit"}]'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/krishc145/mongo-test-pipeline',
                    credentialsId: 'b0725751-8901-49f7-b432-5a0f0168ad35'
            }
        }

        stage('Prepare Insert Script') {
            steps {
                script {
                    writeFile file: 'insert.js', text: """
db["${env.COLLECTION_NAME}"].insertMany(${env.DATA_TO_INSERT});
"""
                }
            }
        }

        stage('Insert Data into Mongo') {
            steps {
                withCredentials([string(credentialsId: 'mongo-password-id', variable: 'MONGO_PASS')]) {
                    bat """
mongosh -u ${env.MONGO_USER} -p %MONGO_PASS% --authenticationDatabase admin ${env.MONGO_DB} insert.js
"""
                }
            }
        }

        stage('Prepare Read Script') {
            steps {
                script {
                    writeFile file: 'read.js', text: """
db["${env.COLLECTION_NAME}"].find().forEach(function(doc) { printjson(doc); });
"""
                }
            }
        }

        stage('Read Data from Mongo') {
            steps {
                withCredentials([string(credentialsId: 'mongo-password-id', variable: 'MONGO_PASS')]) {
                    bat """
mongosh -u ${env.MONGO_USER} -p %MONGO_PASS% --authenticationDatabase admin ${env.MONGO_DB} read.js
"""
                }
            }
        }
    }
}
