
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                sh 'mvn -B clean verify'
            }
        }

        stage('Verify Artifact') {
            steps {
                sh 'ls -lh target/'
            }
        }
	
	stage('Test Jenkins Credentials') {
    		steps {
        		withCredentials([
            		string(
                credentialsId: 'demo-secret',
                variable: 'DEMO_SECRET'
            )
        ]) {
            sh '''
                if [ -n "$DEMO_SECRET" ]; then
                    echo "Credential loaded successfully!"
                else
                    echo "Credential is missing!"
                    exit 1
                fi
            '''
        }
    }
}


    }

    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar',
                             fingerprint: true
            echo 'Build completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }

        always {
            echo 'Pipeline execution finished.'
        }
    }
}

