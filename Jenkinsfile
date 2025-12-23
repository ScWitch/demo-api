pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build All Maven Projects') {
            steps {
                script {
                    // Сначала контракты, потом остальные сервисы
                    def projects = [
                        'books-api-contract',
                        'events-contract',
                        'analytics-service',
                        'audit-service',
                        'demo-rest',
                        'ws'
                    ]

                    echo "Найдено проектов: ${projects.size()} — ${projects}"

                    for (project in projects) {
                        echo "→ Сборка: ${project}"
                        dir(project) {
                            sh 'mvn clean install -DskipTests --batch-mode'
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            echo "✅ Все проекты успешно собраны! Артефакты сохранены."
        }
        failure {
            echo "❌ Один или несколько проектов не собрались."
        }
    }
}