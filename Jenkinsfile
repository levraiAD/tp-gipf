pipeline {
    agent any
    environment {
        HTTP_PROXY = 'http://proxy1-rech:3128'
        HTTPS_PROXY = 'http://proxy1-rech:3128'
        GRADLE_OPTS = '-Dhttp.proxyHost=proxy1-rech -Dhttp.proxyPort=3128 -Dhttps.proxyHost=proxy1-rech -Dhttps.proxyPort=3128'
        JAVA_TOOL_OPTIONS = '-Dhttp.proxyHost=proxy1-rech -Dhttp.proxyPort=3128 -Dhttps.proxyHost=proxy1-rech -Dhttps.proxyPort=3128'
    }
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Build') {
            steps {
                sh './gradlew build --no-daemon --stacktrace'
            }
        }
        stage('Jacoco') {
            steps {
                sh 'mvn test'
                }
            }
        stage('Jacoco Report') {
            steps {
                jacoco execPattern: 'target/jacoco.exec'
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
    post {
        always {
            junit 'build/test-results/test/*.xml'
            archiveArtifacts artifacts: 'build/reports/**, build/test-results/**/*.xml', fingerprint: true
        }
    }
}
