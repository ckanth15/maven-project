pipeline {
    agent {
        label 'DevServer'
    }

    parameters {
        choice(
            name: 'select_environment',
            choices: ['dev', 'prod'],
            description: 'Select your environment'
        )
    }

    environment {
        NAME = "chandra"
    }

    tools {
        maven 'mymaven'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests=true'
                echo "Build complete for ${params.select_environment} environment"
            }
        }

        stage('Test') {
            parallel {
                stage('TestA') {
                    agent { label 'DevServer' }
                    steps {
                        echo "Running TestA"
                        sh "mvn test"
                    }
                }
                stage('TestB') {
                    agent { label 'DevServer' }
                    steps {
                        echo "Running TestB"
                        sh "mvn test"
                    }
                }
            }
            post {
                success {
                    dir('webapp/target') {
                        stash name: 'maven-build-files', includes: '*.war'
                    }
                }
            }
        }

        stage('Deploy to Dev') {
            when {
                expression { params.select_environment == 'dev' }
            }
            agent { label 'DevServer' }
            steps {
                dir('/var/www/html') {
                    unstash 'maven-build-files'
                }
                sh '''
                    cd /var/www/html
                    jar -xvf *.war
                '''
            }
        }

        // stage('Deploy to Prod') {
        //     when {
        //         expression { params.select_environment == 'prod' }
        //     }
        //     agent { label 'ProdServer' }
        //     steps {
        //         echo "Deploying to Production..."
        //         // Add prod deployment logic here
        //     }
        // }
    }
}
