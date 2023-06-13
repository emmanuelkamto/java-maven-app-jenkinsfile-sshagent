// #!/usr/bin/env groovy

// library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
//     [$class: 'GitSCMSource',
//      remote: 'https://gitlab.com/nanuchi/jenkins-shared-library.git',
//      credentialsId: 'gitlab-credentials'
//     ]
// )

// pipeline {
//     agent any
//     tools {
//         maven "maven-3.9"
//     }
//     environment {
//         IMAGE_NAME = 'nanajanashia/demo-app:java-maven-2.0'
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
//                    def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
//                    def ec2Instance = "ec2-user@18.234.217.253"

//                    sshagent(['ec2-server-key']) {
//                        sh "scp -o StrictHostKeyChecking=no server-cmds.sh ${ec2Instance}:/home/ec2-user"
//                        sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ec2Instance}:/home/ec2-user"
//                        sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}"
//                    }
//                 }
//             }
//         }
//     }
// }


// #!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/emmanuelkamto/java-maven-app-shared-library.git',
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
        stage('deploy') {
            steps {
                script {
                   echo 'deploying docker image to EC2...'
                   def dockerComposeCmd = "docker-compose -f docker-compose.yaml up --detach"
                   sshagent(['ec2-server-key']) {
                        sh "scp -o docker-compose.yaml ec2-user@18.234.217.253:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@18.234.217.253 ${shellCmd}"
                    }
                    
                }
            }
        }
    }
}
                    



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
