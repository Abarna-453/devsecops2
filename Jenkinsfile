pipeline {
  agent any
  tools { 
        maven 'Maven_3_2_5'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=devsecops-buggyapp_testing -Dsonar.organization=devsecops-buggyapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=4ac72bc77db33f644f2e03f1f871a3c8f2417fa2'
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=devsecops-abz_devsecops-abarna -Dsonar.organization=devsecops-abz -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=46130ba6ed15074ef6ff4b8e7d7b20b26bba6144'
			}
        } 
  }
