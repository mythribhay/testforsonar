node('master')
{
   
   stage('Git checkout')
             {
                  git 'https://github.com/mythribhay/Inglibrary.git'
              }
       

   
      stage('Deploy')
        {
             sh '/opt/maven/bin/mvn clean deploy '
         }

   stage('Run the application')
        {
             sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -Dspring.profiles.active=dev -jar $WORKSPACE/target/*.jar &'
         }
}




