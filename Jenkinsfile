pipeline {
    agent any

    // Ask user for inputs
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
                    // Validate JSON input
                    def insertData
                    try {
                        insertData = readJSON text: params.DATA_TO_INSERT
                    } catch (Exception e) {
                        error("DATA_TO_INSERT must be valid JSON array!")
                    }

                    // Write insert.js
                    writeFile file: 'insert.js', text: """
db.${params.COLLECTION_NAME}.insertMany(${groovy.json.JsonOutput.toJson(insertData)});
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
                script {
                    // Write read.js
                    writeFile file: 'read.js', text: """
db.${params.COLLECTION_NAME}.find().forEach(function(doc) { printjson(doc); });
"""
                }
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
