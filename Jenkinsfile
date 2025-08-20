pipeline
{

agent {
  label 'DevServer'
}

parameters {
    // choice choices: ['dev', 'prod'], name: 'select_environment'
    string defaultValue: 'kanth', name: 'LASTNAME'
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
            sh 'mvn clean package'
            echo "Hello ${params.LASTNAME} ${NAME}"
           
        }
    }
    stage('test')
    {
        parallel{
            stage('testA ')
            {
                steps {
                    echo "this is testA"
                }
            }
            stage('testB ')
            {
                steps {
                    // sh 'mvn test'
                    echo "this is testB"
                }
            }
        }
        post{
        success{
            archiveArtifacts artifacts: '**/target/*.war'
            }
        }
    }
}
}