
pipeline {
    agent any
	
	parameters {
	    choice(
	        name: 'BUILD_ENV',
	        choices: ['DEV', 'QA', 'PROD'],
	        description: 'Select the build environment'
	    )
	
	    string(
	        name: 'APP_VERSION',
	        defaultValue: '1.0.0',
	        description: 'Application version'
	    )
	}
	
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
		stage('Show Build Parameters') {
		    steps {
		        echo "Selected Environment: ${params.BUILD_ENV}"
		        echo "Application Version: ${params.APP_VERSION}"
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
