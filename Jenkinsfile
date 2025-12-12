pipeline {
    agent any

    environment {
        HTTP_PROXY  = 'http://proxy1-rech:3128'
        HTTPS_PROXY = 'http://proxy1-rech:3128'
    }

    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Build') {
            steps {
                sh './gradlew clean assemble --no-daemon --stacktrace'
            }
        }


        stage('SonarQube Analysis') {
            steps {
                sh '''
                    ./gradlew sonar \
                        -Dsonar.projectKey=tpControle \
                        -Dsonar.projectName="tpControle" \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.token=sqp_8ae739edc274db2b034937f19a9bdfbf2ad6d5ef
                '''
            }
        }
    }
}
