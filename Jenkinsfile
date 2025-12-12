pipeline {
    agent any

    stages {
        stage('Build & SonarQube') {
            steps {
                withSonarQubeEnv('TPControle') {
                    sh """
                        ./gradlew clean test sonarqube \
                          -Dsonar.projectKey=tpControle \
                          -Dsonar.projectName=tpControle \
                          -Dsonar.host.url=http://localhost:9000
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
