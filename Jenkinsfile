pipeline {
    agent {
        docker {
            image 'maven:3.9.2'
            args '-u root -v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh '''
                if [ -f ./jenkins/scripts/deliver.sh ]; then
                    ./jenkins/scripts/deliver.sh
                else
                    echo "Delivery script not found!"
                    exit 1
                fi
                '''
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
