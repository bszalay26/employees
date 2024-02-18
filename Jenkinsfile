pipeline {
    environment {
        //DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        VERSION_NUMBER = sh (
                script: './mvnw help:evaluate -Dexpression=project.version -Dbuild.number=${BUILD_NUMBER} -q -DforceStdout',
                returnStdout: true).trim()
        IMAGE_NAME = "bszalay26/employees:${VERSION_NUMBER}"
        //SONAR_CREDENTIALS = credentials('sonar-credentials')
    }

    agent {
        dockerfile {
            filename 'Dockerfile.build'
            args '-e DOCKER_CONFIG=./docker'
        }
    }

    stages {
        stage('Commit') {
            steps {
                echo "Version number: ${VERSION_NUMBER}"
                echo "Commit stage"
                sh "./mvnw -B clean package -Dbuild.number=${build_number}"
            }
        }
        stage('Acceptance') {
            steps {
                echo "Acceptance stage"
                //sh "./mvnw -B integration-test"
            }
        }
        stage('Docker') {
            steps {
                echo "Docker stage"
                //sh "docker build -f Dockerfile.layered -t ${IMAGE_NAME} ."
                //sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u=${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                //sh "docker push ${IMAGE_NAME}"
                //sh "docker tag ${IMAGE_NAME} bszalay26/employees:latest"
                //sh "docker push bszalay26/employees:latest"
            }
        }
        stage('Quality') {
            parallel {
                stage('E2E API') {
                    steps {
                        echo "E2E test stage"
                        dir('employees-postman') {
                            sh 'rm -rf reports'
                            sh 'mkdir reports'
                            //sh 'docker compose -f docker-compose.yaml -f docker-compose.jenkins.yaml up --abort-on-container-exit'
                            //archiveArtifacts artifacts: 'reports/*.html', fingerprint: true
                        }
                    }
                }
                stage('Code quality') {
                    steps {
                        echo "Code quality (Sonar) stage"
//                sh "./mvnw sonar:sonar -Dsonar.host.url=http://host.docker.internal:9000 -Dsonar.login=${SONAR_CREDENTIALS_PSW}"
                    }
                }
            }
        }
    }
}