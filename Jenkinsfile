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
        nexusURL = '172.31.94.66:8081'
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
         stage('Build') {
            steps {
                sh """
                    ls -la
                    zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                    ls -ltr
                """
            }
        }
        stage('Publish Artifact') {
            steps {
                 nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusURL}",
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus_auth',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }
        stage ('Invoke_pipeline') {
            steps {
                script{
                    def params = [
                    string(name: 'version', value: "${packageVersion}"),
                    string(name: 'environment', value: "dev")
                ]
                build job: 'catalogue-deploy', wait = true, parameters: params
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