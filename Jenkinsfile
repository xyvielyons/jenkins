def remote = [:]
remote.name = 'digitalocean'
remote.host = '198.199.72.62'
remote.allowAnyHosts = true

pipeline {
    agent any
    environment {
        PI_CREDS=credentials('c4770b8f-e01f-4ee1-9235-666f7ef58c23')
        DOCKER_HOST = 'tcp://docker:2375'
        IMAGE_NAME = "Jengaplan"
        TAG = "38"
    }
    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('remote ssh') {
            steps {
                echo env.GIT_AUTHOR_NAME
                echo env.JOB_NAME
                script {
                    remote.user = env.PI_CREDS_USR
                    remote.password=env.PI_CREDS_PSW
                }
               
                sshCommand(remote: remote, command: """
                    cd .. && \
                    ls && \
                    cd ${IMAGE_NAME} && \
                    ls && \
                    lscpu && \
                    exit \
                    
                """)

                echo "Building.."
                sh '''
                '''
            }
        }
        stage('Test') {
            steps {
                echo "Testing.."
                sh '''
                cd myapp
                python3 hello.py
                '''
            }
        }
        stage('Deliver') {
            steps {
                echo 'Deliver....'
                input message: "Deploy to production?", ok: "Yes, Deploy",submitter: 'admin'
                sh '''
                echo "doing delivery stuff.."
                '''
            }
        }
    }
}