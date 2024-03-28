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
        nexusURL = '172.31.88.3:8081'
    }
    parameters {
        booleanParam(name: 'Deploy', defaultValue: false, description: 'Toggle this value')
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
    }
    // Post Build
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
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