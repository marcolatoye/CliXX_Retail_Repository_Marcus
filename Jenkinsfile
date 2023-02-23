pipeline {
    agent any
    environment {
        VERSION = "1.0.${BUILD_NUMBER}"
        PATH = "${PATH}:${getSonarPath()}:${getDockerPath()}"
        RUNNER = "Marcus"
    }
    stages {
//         stage ('Sonarcube Scan') {
//         steps {
//           slackSend (color: '#FFFF00', message: "STARTING SONARQUBE SCAN - '${env.RUNNER}': Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
//          script {
//           scannerHome = tool 'sonarqube'
//         }
//         withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]){
//         withSonarQubeEnv('SonarQubeScanner') {
//           sh " ${scannerHome}/bin/sonar-scanner \
//           -Dsonar.projectKey=CliXX-App-Marcus   \
//           -Dsonar.login=${SONAR_TOKEN} "
//         }
//         }
//         }
// }
//  stage('Quality Gate') {
//             steps {
//               slackSend (color: '#FFFF00', message: "WAITING FOR QUALITY GATE REPORT - '${env.RUNNER}': Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
//                 timeout(time: 3, unit: 'MINUTES') {
//                     waitForQualityGate abortPipeline: true
//             }
//             }
//         }

          stage ('Build Docker Image') {
          steps {
            slackSend (color: '#FFFF00', message: "BUILDIN THE DOCKER IMAGE - '${env.RUNNER}': Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
            // script{
            //  dockerHome= tool 'docker-inst'
            // }
            //  sh "${dockerHome}/bin/docker build . -t clixx-image:$VERSION "
            sh "docker build . -t clixx-image:$VERSION "
          }
        }
  // stage ('Starting Docker Image') {
  //         steps {
  //           slackSend (color: '#FFFF00', message: "STARTING DOCKER IMAGE - '${env.RUNNER}': Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
  //             sh '''
  //             if ( docker ps|grep clixx-cont ) then
  //                echo "Docker image exists, killing it"
  //                docker stop clixx-cont
  //                docker rm clixx-cont
  //                docker run --name clixx-cont  -p 80:80 -d clixx-image:$VERSION
  //             else
  //                docker run --name clixx-cont  -p 80:80 -d clixx-image:$VERSION 
  //             fi
  //             '''
  //         }
  //       }
  // stage ('Restore CliXX Database') {
  //         steps {
  //           slackSend (color: '#FFFF00', message: "RESTORING CLIXX DATABASE - '${env.RUNNER}': Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
  //             sh '''
  //           python3 -m venv python3-virtualenv
  //           source python3-virtualenv/bin/activate
  //           python3 -m pip install --upgrade pip
  //           python3 --version
  //           pip3 install boto3 botocore boto ansible
  //           ansible-playbook $WORKSPACE/deploy_db_ansible/deploy_db.yml
  //           deactivate
  //             '''
  //         }
  //       }
  // stage ('Configure DB Instance') {
  //         steps {
  //             slackSend (color: '#FFFF00', message: "CONFIGURE DATABASE INSTANCE - '${env.RUNNER}': Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
  //             script {
  //          def userInput = input(id: 'confirm', message: 'Is DB creation complete?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Complete?', name: 'confirm'] ])
  //       }

  //        withCredentials([
  //                    string(credentialsId: 'user_name', variable: 'user_name'),
  //                    string(credentialsId: 'pass_word', variable: 'pass_word'),
  //                    string(credentialsId: 'dbname', variable: 'dbname'),
  //                    string(credentialsId: 'server_instance', variable: 'server_instance'),
  //                    string(credentialsId: 'ecs_database', variable: 'ecs_database')
  //                    ]) {

  //        sh '''
  //              USERNAME="${user_name}"
  //              PASSWORD="${pass_word}"
  //              DBNAME="${dbname}"
  //              SERVER_IP=$(curl -s http://checkip.dyndns.org | sed -e 's/.*Current IP Address: //' -e 's/<.*$//')
  //              SERVER_INSTANCE="${ecs_database}"
  //              echo "use '$DBNAME';" > $WORKSPACE/db.setup
  //              echo "UPDATE wp_options SET option_value = '$SERVER_IP' WHERE option_value LIKE 'http%';">> $WORKSPACE/db.setup
  //              mysql -u $USERNAME --password=$PASSWORD -h $SERVER_INSTANCE  -D  $DBNAME < $WORKSPACE/db.setup
  //             '''
  //      }
  //         }
  //       }
      // stage ('Tear Down CliXX Docker Image and Database') {
      //     steps {
      //       slackSend (color: '#FFFF00', message: "TEARING DOWN CLIXX DOCKER IMAGE AND DATABASE - '${env.RUNNER}': Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
      //        script {
      //           def userInput = input(id: 'confirm', message: 'Tear Down Environment?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Tear Down Environment?', name: 'confirm'] ])
      //        }
      //         sh '''
      //       python3 -m venv python3-virtualenv
      //       source python3-virtualenv/bin/activate
      //       python3 --version
      //       pip3 install boto3 botocore boto
      //       ansible-playbook $WORKSPACE/deploy_db_ansible/delete_db.yml
      //       deactivate
      //       docker stop clixx-cont
      //       docker rm  clixx-cont
      //         '''
      //     }
      //   }
        stage ('Log Into ECR and push the newly created Docker') {
          steps {
            slackSend (color: '#FFFF00', message: "PUSHING DOCKER IMAGE INTO ECR - '${env.RUNNER}': Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
             script {
                def userInput = input(id: 'confirm', message: 'Push Image To ECR?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Push to ECR?', name: 'confirm'] ])
             }
             withCredentials([
                     string(credentialsId: 'ecr_url', variable: 'ecr_url')
                     ])
                  {
              sh '''
                ECR_URL="${ecr_url}"
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $ECR_URL
                docker tag clixx-image:$VERSION $ECR_URL:clixx-image-$VERSION
                docker tag clixx-image:$VERSION $ECR_URL:latest
                docker push $ECR_URL:clixx-image-$VERSION
                docker push $ECR_URL:latest
              '''
              }
          }
        }
    }
}
def getSonarPath(){
        def SonarHome= tool name: 'sonarqube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
        return SonarHome
    }
def getDockerPath(){
        def DockerHome= tool name: 'docker-inst', type: 'dockerTool'
        return DockerHome
    }
    