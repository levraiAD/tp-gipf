pipeline {
    agent any

    stages {
        stage('Build & SonarQube') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh """
                        ./gradlew clean sonar \
                          -Dsonar.projectKey=tpControle \
                          -Dsonar.projectName=tpControle \
                          -Dsonar.host.url=http://localhost:9000 \
                          -Dsonar.token=${SONAR_TOKEN ?: 'sqp_3a5f89e5e136e94a32d9edb31c6bcb064ea0d876'}
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
