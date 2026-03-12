pipeline {
    agent {
        docker {
            image 'python:3.10'
        }
    }

    stages {
        stage('Install') {
            steps {
                echo 'Installation des dépendances'
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Script') {
            steps {
                echo 'Exécution du script Python'
                sh 'python app.py'
            }
        }

        stage('Tests') {
            steps {
                echo 'Lancement des tests'
                sh 'pytest'
            }
        }
    }
}