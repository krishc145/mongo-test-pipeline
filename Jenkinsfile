pipeline {
    agent any

    stages {
        stage('Insert Data into Mongo') {
            steps {
                bat '''
                    echo db.dba_admin_fruits.insertMany([
                        {name: "mango"},
                        {name: "apple"},
                        {name: "jackfruit"}
                    ]) > insert.js

                    mongosh krishna_DB_admin insert.js
                '''
            }
        }

        stage('Read Data') {
            steps {
                bat '''
                    echo db.dba_admin_fruits.find().forEach(doc => printjson(doc)) > read.js

                    mongosh krishna_DB_admin read.js
                '''
            }
        }
    }
}
