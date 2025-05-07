pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/ManuelDiazNoval/Jenkins.git', branch: 'main'
            }
        }

        stage('Crear entorno virtual') {
            steps {
                sh 'python3 -m venv $VENV_DIR'
            }
        }

        stage('Instalar dependencias') {
            steps {
                sh '''
                    source $VENV_DIR/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Ejecutar pruebas') {
            steps {
                sh '''
                    source $VENV_DIR/bin/activate
                    pytest --junitxml=test-results/results.xml || true
                '''
            }
        }

        stage('Publicar resultados') {
            steps {
                junit 'test-results/results.xml'
            }
        }
    }

    post {
        success {
            echo 'Pipeline ejecutado correctamente.'
        }
        failure {
            echo 'El pipeline falló. Revisa los logs.'
        }
    }
}
