pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Ramin619/8.2CreditDevSecOp.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        script {
          def result = bat(script: 'npm test', returnStatus: true)
          if (result != 0) {
            emailext subject: '❌ Jenkins: Tests Failed',
              body: 'The test stage failed. Please check the attached log.',
              to: 'raminsenmitha@gmail.com',
              attachLog: true
          } else {
            emailext subject: '✅ Jenkins: Tests Passed',
              body: 'The test stage passed successfully.',
              to: 'raminsenmitha@gmail.com',
              attachLog: true
          }
        }
      }
    }

    stage('Generate Coverage Report') {
      steps {
        bat 'npm run coverage || exit /b 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        script {
          def result = bat(script: 'npm audit', returnStatus: true)
          if (result != 0) {
            emailext subject: '⚠️ Jenkins: Security Vulnerabilities Found',
              body: 'Security scan completed with issues. Check attached log.',
              to: 'raminsenmitha@gmail.com',
              attachLog: true
          } else {
            emailext subject: '✅ Jenkins: Security Scan Clean',
              body: 'No vulnerabilities found in npm audit.',
              to: 'raminsenmitha@gmail.com',
              attachLog: true
          }
        }
      }
    }
  }
}
