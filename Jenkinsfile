		
pipeline {
    agent any
    tools {
        // Define global tools such as Maven and JDK (optional based on your setup)
        maven 'maven3'
        jdk 'java17'
        
    }
    environment {
        // SonarQube environment variables
        SONARQUBE_SERVER = tool 'sonarqube' // Replace with your SonarQube server name
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out the code...'
                // Pull code from repository
                git branch: 'main', url: 'https://github.com/guna222ls/Petclinic.git'
                sh "pwd"
            }
        }
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Execute the build (e.g., Maven)
                sh 'mvn clean install'
            }
        }
       
        stage('SonarQube Scan') {
            steps {
                echo 'Running SonarQube analysis...'
                // SonarQube scanner for static code analysis
                withSonarQubeEnv('sonarqube') { // Replace with the name of your SonarQube configuration
                    sh '''$SONARQUBE_SERVER/bin/sonar-scanner -Dsonar.projectName=petclinic \
                    -Dsonar.java.binaries=.  \
                    -Dsonar.projectKey=petclinic '''
                }
            }
        }
         stage('OWASP Check') {
            steps {
               
               dependencyCheck additionalArguments: '--scan target/ --out .', nvdCredentialsId: 'owasp_API_KEY', odcInstallation: 'owaspcheck' 
               
                }
            }
        
        
        
        
        stage('Quality Gate Check') {
            steps {
                echo 'Waiting for SonarQube Quality Gate...'
                // Check the status of the Quality Gate
                script {
                    def qualityGate = waitForQualityGate()
                    if (qualityGate.status != 'OK') {
                        error "SonarQube Quality Gate failed with status: ${qualityGate.status}"
                    }
                }
            }
        }
       /* stage('Evaluate OWASP Results') {
            steps {
                echo 'Evaluating OWASP Dependency-Check report...'
                script {
                    // Parse the OWASP Dependency-Check report (example logic)
                    def reportFile = readFile 'dependency-check-report/dependency-check-report.json'
                    def vulnerabilities = reportFile.contains("VULNERABILITY")
                    if (vulnerabilities) {
                        error 'OWASP Dependency-Check found critical vulnerabilities. Deployment held!'
                    }
                }
            }
        }*/
        stage('Deploy') {
            when {
                expression {
                    // Only proceed if all previous checks pass
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                echo 'Deploying application...'
                // Deployment script
                sh './deploy.sh'
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution complete.'
        }
        failure {
            echo 'Pipeline failed. Please review the logs and resolve issues before retrying.'
        }
    }
}



		
pipeline {
    agent any
    tools {
        // Define global tools such as Maven and JDK (optional based on your setup)
        maven 'maven3'
        jdk 'jdk21'
    }
   
    
    stages {
       stage('Checkout Code') {
            steps {
                echo 'Checking out the code...'
                // Pull code from repository
                git branch: 'main', url: 'https://github.com/guna222ls/Petclinic.git'
                sh "pwd"
                
            }
        }
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Execute the build (e.g., Maven)
                sh 'mvn install'
            }
        }
       /*
        stage('SonarQube Scan') {
            steps {
                echo 'Running SonarQube analysis...'
                // SonarQube scanner for static code analysis
                withSonarQubeEnv('sonarqube') { // Replace with the name of your SonarQube configuration
                    sh '''$SONARQUBE_SERVER/bin/sonar-scanner -Dsonar.projectName=petclinic \
                    -Dsonar.java.binaries=.  \
                    -Dsonar.projectKey=petclinic '''
                }
            }
        }*/
         stage('OWASP Check') {
            steps {
               
               dependencyCheck additionalArguments: '--scan target/ --out .', nvdCredentialsId: 'nvdkey', odcInstallation: 'owaspcheck' 
               
                }
            }
        
        
        /*
        
        stage('Quality Gate Check') {
            steps {
                echo 'Waiting for SonarQube Quality Gate...'
                // Check the status of the Quality Gate
                script {
                    def qualityGate = waitForQualityGate()
                    if (qualityGate.status != 'OK') {
                        error "SonarQube Quality Gate failed with status: ${qualityGate.status}"
                    }
                }
            }
        }
		stage('Evaluate OWASP Results') {
            steps {
                echo 'Evaluating OWASP Dependency-Check report...'
                script {
                    // Parse the OWASP Dependency-Check report (example logic)
                    def reportFile = readFile 'dependency-check-report/dependency-check-report.json'
                    def vulnerabilities = reportFile.contains("VULNERABILITY")
                    if (vulnerabilities) {
                        error 'OWASP Dependency-Check found critical vulnerabilities. Deployment held!'
                    }
                }
            }
        }*/
        
        
        
        
    }
    }
