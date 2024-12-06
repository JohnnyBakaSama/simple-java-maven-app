pipeline {
    agent {
        docker {
            image 'maven:3.9.2'
            args '-v /root/.m2:/root/.m2' // Ensures Maven dependency cache is shared
        }
    }
    stages {
        stage('Build') {
            steps {
                // Clean and build the Maven project, skipping tests
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                // Run unit tests
                sh 'mvn test'
            }
            post {
                always {
                    // Archive test results even if tests fail
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                // Execute the delivery script
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
            // Clean up the workspace after the pipeline finishes
            cleanWs()
        }
    }
}
