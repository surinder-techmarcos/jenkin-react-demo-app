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
                echo "${env.REMOTE_TARGET}"
                echo "$WORKSPACE"
                echo sshagent(['techmarcos-ssh-key']) {
                    sh "ssh -o StrictHostKeyChecking=no -T ${env.REMOTE_USERNAME}@${env.REMOTE_HOST}"
                    // sh "ssh -v ${env.REMOTE_USERNAME}@${env.REMOTE_HOST}"
                    sh "scp -r ${WORKSPACE}/build ${env.REMOTE_USERNAME}@${env.REMOTE_HOST}:${env.REMOTE_TARGET}"
                    sh "ssh -o StrictHostKeyChecking=no -T ${env.REMOTE_USERNAME}@${env.REMOTE_HOST} /bin/bash -c 'cd ${env.REMOTE_TARGET} && chmod u+x ./docker-restart.sh && ./docker-restart.sh'"
                }
                echo "Deploy App Successfully."
            }
        }
    }
}