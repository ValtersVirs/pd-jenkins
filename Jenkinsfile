pipeline {
    agent any
    triggers{ pollSCM('*/1 * * * *') }

    stages {
        stage('install-pip-deps') {
            steps {
                install()
            }
        }
        stage('deploy-to-dev') {
            steps {
                deploy("dev", 7001)
            }
        }
        stage('tests-on-dev') {
            steps {
                test("dev")
            }
        }
        stage('deploy-to-staging') {
            steps {
                deploy("stg", 7002)
            }
        }
        stage('tests-on-staging') {
            steps {
                test("stg")
            }
        }
        stage('deploy-to-preprod') {
            steps {
                deploy("preprod", 7003)
            }
        }
        stage('tests-on-preprod') {
            steps {
                test("preprod")
            }
        }
        stage('deploy-to-prod') {
            steps {
                deploy("prod", 7004)
            }
        }
        stage('tests-on-prod') {
            steps {
                test("prod")
            }
        }
    }
}

def install() {
    echo "Installing dependencies has started.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    bat "dir"
    bat "pip install -r requirements.txt"
}

def deploy(String environment, int port) {
    echo "Deployment to ${environment} has started.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    bat "pm2 delete greetings-app-${environment} & EXIT /B 0"
    bat "pm2 start app.py --name greetings-app-${environment} -- --port ${port}"
}

def test(String environment) {
    echo "Testing on ${environment} has started.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/course-js-api-framework.git'
    bat "npm install"
    bat "npm run greetings greetings_${environment}"
}