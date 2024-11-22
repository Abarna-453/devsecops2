pipeline {
  agent any
  tools { 
        maven 'Maven_3_2_5'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=devsecops-abz_devsecops-abarna -Dsonar.organization=devsecops-abz -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=46130ba6ed15074ef6ff4b8e7d7b20b26bba6144'
			}
        } 
  }
}
