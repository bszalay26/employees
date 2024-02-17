pipeline {
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        VERSION_NUMBER = sh (
                script: './mvnw help:evaluate -Dexpression=project.version -Dbuild.number=${BUILD_NUMBER} -q -DforceStdout',
                returndStdout: true).trim()
//        IMAGE_NAME = "bszalay26/employees:${VERSION_NUMBER}"
    }

    agent any /*{
        dockerfile {
            filename 'Dockerfile.build'
            args '-e DOCKER_CONFIG=./docker'
        }
    }*/

    stages {
        stage('Commit') {
            steps {
                echo "Version number: ${VERSION_NUMBER}"
                echo "Commit stage"
                //sh "./mvnw -B clean package -Dbuild.number=${build_number}"
            }
        }
        stage('Acceptance') {
            steps {
                echo "Acceptance stage"
                //sh "./mvnw -B integration-test"
            }
        }
/*        stage('Docker') {
            steps {
                sh "docker build -f Dockerfile.layered -t ${IMAGE_NAME} ."
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u=${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                sh "docker push ${IMAGE_NAME}"
                sh "docker tag ${IMAGE_NAME} bszalay26/employees:latest"
                sh "docker push bszalay26/employees:latest"
            }
        }*/
    }
}