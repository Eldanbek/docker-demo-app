pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'docker-demo-app'
        DOCKER_TAG = "${env.BUILD_NUMBER}"
    }
    
    triggers {
        githubPush() 

    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                   sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }                                                                       
        }
        
        stage('deploy compose file') {
            steps {
                withCredentials ([
                                 string  ( credentialsid: 'mongo-db-username', variable: 'MONGO_DB_USERNAME'),
                                 string  ( credentialsid: 'mongo-db-password', variable: 'MONGO_DB_PASSWORD')
                                 ]) {
                                     sh '''
                                        export DOCKER_TAG=${DOCKER_TAG}
                                        export MONGO_DB_USERNAME=${MONGO_DB_USERNAME}
                                        export MONGO_DB_PWD=${MONGO_DB_PWD}
                                        docker-compose -f compose.yml up -d
                                        '''
                }
            }
        }

    }
}
          