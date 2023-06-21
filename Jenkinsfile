#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/emmanuelkamto/jenkins-shared-library.git',
     credentialsId: 'github-credential'
    ]
)

pipeline {
    agent any
    tools {
        maven "maven-3.9"
    }
    environment {
        IMAGE_NAME = 'emmanuelkamto/demo-app:jma-1.0'
    }
    stages {
        stage('build app') {
            steps {
               script {
                  echo 'building application jar...'
                  buildJar()
               }
            }
        }
        stage('build image') {
            steps {
                script {
                   echo 'building docker image...'
                   buildImage(env.IMAGE_NAME)
                   dockerLogin()
                   dockerPush(env.IMAGE_NAME)
                }
            }
        }
        stage('provision server') {
            environment {
                AWS_ACCESS_KEY_ID = credentials('jenkins_aws_access_key_id')
                AWS_SECRET_ACCESS_KEY = ('jenkins_aws_secret_access_key')
                TF_VAR_env_prefix = 'test'
            }
            steps {
                script {
                    dir('terraform') {
                        sh "terrafrom init"
                        sh "terraform apply --auto-approve"
                        EC2_PUBLIC_IP = sh(
                            script: "terraform output ec2_public_ip"
                            returnStdout: true
                            ).trim()
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {

                    echo 'waiting for EC2 server to initialize'
                    sleep(time: 90; unit:"SECONDS")

                    echo 'deploying docker image to EC2...'
                    echo '${EC2_PUBLIC_IP}'

                    def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
                    def ec2Instance = "ec2-user@${EC2_PUBLIC_IP}"

                    sshagent(['server-ssh-key']) {
                       sh "scp -o StrictHostKeyChecking=no server-cmds.sh ${ec2Instance}:/home/ec2-user"
                       sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ec2Instance}:/home/ec2-user"
                       sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}"
                   }
                }
            }
        }
    }
}


// #!/usr/bin/env groovy

// library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
//     [$class: 'GitSCMSource',
//      remote: 'https://github.com/emmanuelkamto/jenkins-shared-library.git',
//      credentialsId: 'github-credential'
//     ]
// )

// pipeline {
//     agent any
//     tools {
//         maven "maven-3.9"
//     }
//     environment {
//         IMAGE_NAME = 'emmanuelkamto/demo-app:jma-1.0'
//     }
//     stages {
//         stage('build app') {
//             steps {
//                script {
//                   echo 'building application jar...'
//                   buildJar()
//                }
//             }
//         }
//         stage('build image') {
//             steps {
//                 script {
//                    echo 'building docker image...'
//                    buildImage(env.IMAGE_NAME)
//                    dockerLogin()
//                    dockerPush(env.IMAGE_NAME)
//                 }
//             }
//         }
//         stage('deploy') {
//             steps {
//                 script {
//                    echo 'deploying docker image to EC2...'
//                    def dockerComposeCmd = "docker-compose -f docker-compose.yaml up --detach"
//                    sshagent(['ec2-server-key']) {
//                         sh "scp -o docker-compose.yaml ec2-user@18.234.217.253:/home/ec2-user"
//                         sh "ssh -o StrictHostKeyChecking=no ec2-user@18.234.217.253 ${shellCmd}"
//                     }
                    
//                 }
//             }
//         }
//     }
// }
                    


// #!/usr/bin/env groovy

// library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
//     [$class: 'GitSCMSource',
//      remote: 'https://github.com/emmanuelkamto/jenkins-shared-library.git',
//      credentialsId: 'github-credential'
//     ]
// )

// pipeline {
//     agent any
//     tools {
//         maven "maven-3.9"
//     }
//     environment {
//         IMAGE_NAME = 'emmanuelkamto/demo-app:jma-1.0'
//     }
//     stages {
//         stage('build app') {
//             steps {
//                script {
//                   echo 'building application jar...'
//                   buildJar()
//                }
//             }
//         }
//         stage('build image') {
//             steps {
//                 script {
//                    echo 'building docker image...'
//                    buildImage(env.IMAGE_NAME)
//                    dockerLogin()
//                    dockerPush(env.IMAGE_NAME)
//                 }
//             }
//         }
//         stage('deploy') {
//             steps {
//                 script {
//                    echo 'deploying docker image to EC2...'
//                    def dockerCmd = "docker run -p 8080:8080 -d ${IMAGE_NAME}"
//                    sshagent(['ec2-server-key']) {
//                         sh "ssh -o StrictHostKeyChecking=no ec2-user@18.234.217.253 ${shellCmd}"
//                     }
                    
//                 }
//             }
//         }
//     }
// }
     



// #!/usr/bin/env groovy

// pipeline {
//     agent any
//     stages {
//         stage('build') {
//             steps {
//                 script {
//                     echo "Building the application..."
//                 }
//             }
//         }
//         stage('test') {
//             steps {
//                 script {
//                     echo "Testing the application..."
//                 }
//             }
//         }
//         stage('deploy') {
//             steps {
//                 script {
//                     def shellCmd = 'docker run -p 3080:3080 -d emmanuelkamto/demo-app:jma-1.0'
//                     sshagent(['ec2-server-key']) {
//                       sh "ssh -o StrictHostKeyChecking=no ec2-user@18.234.217.253 ${shellCmd}"
//                    }
//                 }
//             }
//         }
//     }
// }
