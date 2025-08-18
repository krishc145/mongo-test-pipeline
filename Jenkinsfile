pipeline {
    agent any

    // Ask user for inputs
    parameters {
        string(name: 'DB_NAME', defaultValue: 'krishna_DB_admin', description: 'MongoDB database name')
        string(name: 'COLLECTION_NAME', defaultValue: 'dba_admin_fruits', description: 'Collection name')
        text(name: 'DATA_TO_INSERT', defaultValue: '[{"name":"mango"},{"name":"apple"},{"name":"jackfruit"}]', description: 'Data to insert (JSON array or CSV)')
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
                    def rawData = params.DATA_TO_INSERT
                    def collectionName = params.COLLECTION_NAME

                    // Try parsing user input as JSON
                    def jsonArray
                    try {
                        jsonArray = new groovy.json.JsonSlurper().parseText(rawData)
                    } catch(Exception e) {
                        // If parsing fails, treat input as CSV
                        def items = rawData.split(',').collect { it.trim() }
                        jsonArray = items.collect { [name: it] }
                    }

                    // Write insert.js
                    writeFile file: 'insert.js', text: """
db.${collectionName}.insertMany(${groovy.json.JsonOutput.toJson(jsonArray)});
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
                    def collectionName = params.COLLECTION_NAME

                    // Create read.js
                    writeFile file: 'read.js', text: """
db.${collectionName}.find().forEach(function(doc) { printjson(doc); });
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
