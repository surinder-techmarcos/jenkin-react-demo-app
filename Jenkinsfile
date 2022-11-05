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
                echo "Deployment starting...."
                echo "REMOTE_USERNAME : ${env.REMOTE_USERNAME}, REMOTE_HOST: ${env.REMOTE_HOST}"
                echo "$WORKSPACE"
                echo sshagent(['techmarcos-ssh-key']) {
                    sh 'ssh -o StrictHostKeyChecking=no -T ${env.REMOTE_USERNAME}@${env.REMOTE_HOST}'
                    // sh 'ssh -v ${env.REMOTE_USERNAME}@${env.REMOTE_HOST}'
                    // sh 'scp $WORKSPACE/build ${env.REMOTE_USERNAME}@${env.REMOTE_HOST}:${env.REMOTE_TARGET}'
                }
                echo "Deploy App Successfully."
                // sh "sudo rm -rf /var/www/jenkins-react-app"
                // sh "sudo cp -r ${WORKSPACE}/build/ /var/www/jenkins-react-app/"
            }
        }
    }
}