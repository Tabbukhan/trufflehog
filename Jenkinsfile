pipeline {
  agent any
  stages {
    stage('TruffleHog Scan') {
            steps {
                // Run TruffleHog scan
                sh 'trufflehog --regex --entropy=True .'
            }  
    }
      stage('GitGuardian Scan') {
            //agent {
                //docker { image 'gitguardian/ggshield' }
            //}
            environment {
                GITGUARDIAN_API_KEY = credentials('guardian-token')
            }
            steps {
                //sh 'ggshield secret scan ci'
                sh 'ggshield secret scan --show-secrets ci'
                script {
                    // Execute ggshield command to scan for secrets
                    def scanOutput = sh(script: 'ggshield secret scan --show-secrets --repo-path .', returnStdout: true).trim()

                    // Display the scan output
                    echo scanOutput
                }
                
            }
        }
    
  }
}
