pipeline {
    agent any
    environment {
        // Aseguramos que el directorio 'src' esté en el PYTHONPATH
        PYTHONPATH = "${WORKSPACE}/src"
    }
    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Clonar repositorio') {
            steps {
                git 'https://github.com/ManuelDiazNoval/Jenkins.git'
            }
        }
        stage('Crear entorno virtual') {
            steps {
                sh 'python3 -m venv venv'
            }
        }
        stage('Instalar dependencias') {
            steps {
                sh '''
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }
        stage('Ejecutar pruebas') {
            steps {
                sh '''
                . venv/bin/activate
                pytest --junitxml=test-results/results.xml
                '''
            }
        }
        stage('Publicar resultados') {
            steps {
                junit '**/test-results/results.xml'
            }
        }
    }
}
