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
                sh 'ggshield secret scan ci'
                
            }
        }
    stage('Scan for Secrets') {
            steps {
                script {
                    // Execute ggshield command to scan for secrets
                    def scanOutput = sh(script: 'ggshield secret scan --json --repo-path .', returnStdout: true).trim()

                    // Parse the JSON output to extract detected secrets
                    def detectedSecrets = readJSON(text: scanOutput)

                    // Display detected secrets
                    echo "Detected Secrets:"
                    detectedSecrets.each { secret ->
                        echo "- Type: ${secret.type}, Line: ${secret.line}, Path: ${secret.path}"
                    }
                }
            }
        }
  }
}
