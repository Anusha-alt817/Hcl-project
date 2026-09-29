pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Check Java and Maven') {
            steps {
                sh '''
                    echo "===== JAVA ====="
                    echo $JAVA_HOME
                    which java
                    java -version

                    echo "===== JAVAC ====="
                    which javac
                    javac -version

                    echo "===== MAVEN ====="
                    which mvn
                    mvn -version
                '''
            }
        }

        stage('Build') {
            steps {
                echo 'Building application with Maven...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
        }

        stage('Success') {
            steps {
                echo 'CI Pipeline completed successfully!'
            }
        }
    }
}