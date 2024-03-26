pipeline {
    agent { 
        node { 
            label 'AGENT-1' 
            } 
        }
    options {
        ansiColor('xterm')
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
    }
    environment { 
        packageVersion = ''
        nexusURL = "http://44.203.40.204:8081/repository/catalogue/"
    }
    // Build
    stages {
        stage('Get the app version') { 
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'  // variable initialisation using def
                    packageVersion = packageJson.version
                    echo "application_version: $packageVersion"
                }
            }
        }
        stage('Install dependencies') { 
            steps {
                sh """
                   npm install
                """
            }
        }
        stage('Build a zip file') { 
            steps {
                sh """
                   ls -la

                   ls -ltr
                """
            }
        }
        // stage('destroy') {
        //     when {
        //         expression {
        //             params.option == "destroy"
        //         }
        //     }
        //     input {
        //         message "Should we continue?"
        //         ok "Yes, we should."
        //     }
        //     steps {
        //         sh """
        //            cd 01-vpc
        //            terraform destroy -auto-approve
        //         """
        //     }
        // }
    }
    // Post Build
    post { 
        always { 
            echo 'I will always say Hello again!'
            // deleteDir()
        }
        failure { 
            echo 'I will run when the job has failed!'
        }
        success { 
            echo 'I will run when the job is success!'
        }
        aborted { 
            echo 'I will run when the job is aborted manually!'
        }
    }
}