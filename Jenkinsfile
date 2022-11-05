pipeline {
    agent any
    tools {
        nodejs "nodejs 19.0.0"
    }
    stages {
        stage("Build") {
            steps {
                sh "npm install"
                sh "npm run build"
                echo "Build App Successfully."
            }
        }
        stage("Test") {
            steps {
                sh "npm run test"
                echo "Test App Successfully."
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploy App Successfully."
                echo "${env.REMOTE_USERNAME}, ${env.REMOTE_HOST}"
                echo sshagent(['techmarcos-ssh-key']) {
                    sh 'ssh -o StrictHostKeyChecking=no ${env.REMOTE_USERNAME}@${env.REMOTE_HOST} uptime'
                    sh 'ssh -v ${env.REMOTE_USERNAME}@${env.REMOTE_HOST}'
                    sh 'scp ./build ${env.REMOTE_USERNAME}@${env.REMOTE_HOST}:${env.REMOTE_TARGET}'
                }
                echo "Success"
                // sh "sudo rm -rf /var/www/jenkins-react-app"
                // sh "sudo cp -r ${WORKSPACE}/build/ /var/www/jenkins-react-app/"
            }
        }
    }
}