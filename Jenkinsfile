pipeline {
    agent any

    stages {
        stage('Build & SonarQube') {
            steps {
                withSonarQubeEnv('My SonarQube Server') {
                    sh """
                        ./gradlew clean sonar \
                          -Dsonar.projectKey=tpControle \
                          -Dsonar.projectName=tpControle \
                          -Dsonar.host.url=http://localhost:9000 \
                          -Dsonar.token=${SONAR_TOKEN ?: '........'}
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
