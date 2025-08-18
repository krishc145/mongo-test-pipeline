pipeline {
    agent any
    stages {
        stage('Insert Data into Mongo') {
            steps {
                bat '''
                echo "db.dba_admin_fruits.insertMany([{name:'mango'},{name:'apple'},{name:'jackfruit'}])" > script.js
                mongosh krishna-DB-admin script.js
                '''
            }
        }
        stage('Read Data') {
            steps {
                bat '''
                echo "db.dba_admin_fruits.find().forEach(printjson)" > read.js
                mongosh krishna-DB-admin read.js
                '''
            }
        }
    }
}
