pipeline {
    agent any

    environment {
        PYTHON_REPO = 'https://github.com/mtararujs/python-greetings'
        TEST_REPO = 'https://github.com/mtararujs/course-js-api-framework'
    }

    stages {
        stage('install-pip-deps') {
            steps {
                script {
                    echo 'Cloning Python greetings service repo...'
                    sh 'rm -rf python-greetings && git clone ${PYTHON_REPO}'
                    sh 'ls python-greetings'
                    echo 'Installing Python dependencies...'
                    sh 'pip3 install -r python-greetings/requirements.txt'
                }
            }
        }

        stage('deploy-to-dev') {
            steps {
                script {
                    deploy('dev', '7001')
                }
            }
        }

        stage('tests-on-dev') {
            steps {
                script {
                    runTests('dev')
                }
            }
        }

        stage('deploy-to-staging') {
            steps {
                script {
                    deploy('stg', '7002')
                }
            }
        }

        stage('tests-on-staging') {
            steps {
                script {
                    runTests('stg')
                }
            }
        }

        stage('deploy-to-preprod') {
            steps {
                script {
                    deploy('preprod', '7003')
                }
            }
        }

        stage('tests-on-preprod') {
            steps {
                script {
                    runTests('preprod')
                }
            }
        }

        stage('deploy-to-prod') {
            steps {
                script {
                    deploy('prod', '7004')
                }
            }
        }

        stage('tests-on-prod') {
            steps {
                script {
                    runTests('prod')
                }
            }
        }
    }
}

def deploy(env, port) {
    echo "Deploying to ${env} environment on port ${port}..."

    sh """
        rm -rf python-greetings && git clone ${PYTHON_REPO}
        pm2 delete greetings-app-${env} || true
        pm2 start python-greetings/app.py --name greetings-app-${env} -- --port ${port}
    """
}

def runTests(env) {
    echo "Running tests for ${env} environment..."

    sh """
        rm -rf course-js-api-framework && git clone ${TEST_REPO}
        cd course-js-api-framework
        npm install
        npm run greetings greetings_${env}
    """
}

