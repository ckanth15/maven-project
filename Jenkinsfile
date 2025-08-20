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
            sh 'mvn clean package -DskipTests=true'
            echo "Hello ${params.LASTNAME} ${NAME}"
           
        }
        post {
        success{
            archiveArtifacts artifacts: '**/target/*.war'
            }
        }
    }
}
}