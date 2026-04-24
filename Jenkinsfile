pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Alsewedy/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        SCANNER_VERSION=8.0.1.6346
                        SCANNER_DIR=$WORKSPACE/.sonar/sonar-scanner-$SCANNER_VERSION-linux-x64

                        mkdir -p $WORKSPACE/.sonar

                        if [ ! -d "$SCANNER_DIR" ]; then
                            curl -sSLo $WORKSPACE/.sonar/sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-$SCANNER_VERSION-linux-x64.zip
                            unzip -o $WORKSPACE/.sonar/sonar-scanner.zip -d $WORKSPACE/.sonar
                        fi

                        $SCANNER_DIR/bin/sonar-scanner -Dsonar.token=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
}
