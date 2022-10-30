def gv

libary = library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'git@github.com:CharlieBravoCode/react-nodejs-example.git',
    credentialsId: 'GitHub',
    ]
)

pipeline {
    agent any
    environment {
        IMAGE_NAME = 'charliebravocode/react-nodejs-example'
    }
    stages {
        stage('build docker image') {
            steps {
                script {
                    echo "building docker image..."
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo "deploying docker image to EC2"
                    def dockerCmd = 'docker run -p 3080:3080 -d ${IMAGE_NAME}'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@3.72.15.155 ${dockerCmd}"
                    }

                }
            }
        }
    }   
}
