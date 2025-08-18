pipeline {
    agent any

    parameters {
        string(name: 'DB_NAME', defaultValue: 'krishna_DB_admin', description: 'MongoDB database name')
        string(name: 'COLLECTION_NAME', defaultValue: 'dba_admin_fruits', description: 'Collection name')
        text(name: 'DATA_TO_INSERT', defaultValue: '[{"name":"mango"},{"name":"apple"},{"name":"jackfruit"}]', description: 'Data to insert as JSON array')
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/krishc145/mongo-test-pipeline', branch: 'main'
            }
        }

        stage('Prepare Insert Script') {
            steps {
                script {
                    // Validate JSON without storing parsed object
                    try {
                        new groovy.json.JsonSlurper().parseText(params.DATA_TO_INSERT)
                    } catch(Exception e) {
                        error "DATA_TO_INSERT is not valid JSON: ${e.message}"
                    }

                    // Write insert.js file directly from string
                    writeFile file: 'insert.js', text: """
db.${params.COLLECTION_NAME}.insertMany(${params.DATA_TO_INSERT});
"""
                }
            }
        }

        stage('Insert Data into Mongo') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'mongo-authentication', usernameVariable: 'MONGO_USER', passwordVariable: 'MONGO_PASS')]) {
                    bat """
                    mongosh -u %MONGO_USER% -p %MONGO_PASS% --authenticationDatabase admin %DB_NAME% insert.js
                    """
                }
            }
        }

        stage('Prepare Read Script') {
            steps {
                writeFile file: 'read.js', text: """
db.${params.COLLECTION_NAME}.find().forEach(function(doc) { printjson(doc); });
"""
            }
        }

        stage('Read Data from Mongo') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'mongo-authentication', usernameVariable: 'MONGO_USER', passwordVariable: 'MONGO_PASS')]) {
                    bat """
                    mongosh -u %MONGO_USER% -p %MONGO_PASS% --authenticationDatabase admin %DB_NAME% read.js
                    """
                }
            }
        }
    }
}
