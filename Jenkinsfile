pipeline
{

agent {
  label 'DevServer'
}

parameters {
    // choice choices: ['dev', 'prod'], name: 'select_environment'
  choice choices: ['dev', 'prod'], description: 'Select your environment', name: 'select_environment'
}

environment{
    NAME = "chandra"
}
tools {
  maven 'mymaven'
}

stages{

    stage('build')
    {
        steps {
            // script{
            //     file = load "script.groovy"
            //     file.hello()
            // }
            sh 'mvn clean package -DskipTests=true'
            // echo "Hello ${params.LASTNAME} ${NAME}"
           
        }
    }
    stage('test')
    {
        parallel{
            stage('testA ')
            {
                agent {
                    label 'DevServer'
                }
                steps {
                    echo "this is testA"
                    sh "maven test -Dtest=TestA"
                }
            }
            stage('testB ')
            {
                agent {
                    label 'DevServer'
                }
                steps {
                    // sh 'mvn test'
                    echo "this is testB"
                    sh "maven test -Dtest=TestB"
                }
            }
        }
        post{
        success{
            // archiveArtifacts artifacts: '**/target/*.war'
            dir ('webapp/target') {
                stash name: 'maven-build-files', includes: '*.war'
            }
        }
    }
    stage('deploy_dev')
    {
        steps {
            when {
                expression { params.select_environment == 'dev' }
                beforeAgent true
            }
            agent {
                label 'DevServer'
            }
            steps
            {
                dir('var/www/html')
                {
                    unstash 'maven-build-files'
                }
                sh '''
                cd /var/www/html
                jar -xvf *.war
                '''
            }
            }
        }
    }
}
}