node('master')
{
   
   stage('Git checkout')
             {
                  git 'https://github.com/mythribhay/Inglibrary.git'
              }
       
   stage('SonarQube analysis') 
         {
                  withSonarQubeEnv('sonar') 
                  {
                        sh '/opt/maven/bin/mvn clean package sonar:sonar -Dsonar.password=admin -Dsonar.login=admin'
                  } // SonarQube taskId is automatically attached to the pipeline context
         }
  
   stage("Quality Gate")
         {
                  timeout(time: 1, unit: 'HOURS') 
                  { // Just in case something goes wrong, pipeline will be killed after a timeout
                        def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                        if (qg.status != 'OK') 
                        {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                  }
         }
   stage('Build')
         {
             sh '/opt/maven/bin/mvn clean install'
         }

   stage('Execution')
        {
             sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -Dspring.profiles.active=dev -jar $WORKSPACE/target/*.jar &'
         }
   
   stage('Deploy')
        {
             sh '/opt/maven/bin/mvn clean deploy '
         }
}




