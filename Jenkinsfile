pipeline {
  agent any
  tools { 
    maven 'Maven_3_2_5'  
  }
  environment {
    JIRA_URL = 'https://abarnask2003.atlassian.net'
    JIRA_PROJECT_KEY = 'DAI'
  }
  stages {
    stage('Compile and Run Sonar Analysis') {
      steps {
        script {
          // Run Sonar Analysis
          sh '''
          mvn clean verify sonar:sonar \
          -Dsonar.projectKey=devsecops-abz_devsecops-abarna \
          -Dsonar.organization=devsecops-abz \
          -Dsonar.host.url=https://sonarcloud.io \
          -Dsonar.token=46130ba6ed15074ef6ff4b8e7d7b20b26bba6144
          '''

          // Fetch Blocker Issues from SonarQube API
          def sonarResponse = sh(
            script: '''
            curl -s -u 46130ba6ed15074ef6ff4b8e7d7b20b26bba6144: \
            "https://sonarcloud.io/api/issues/search?componentKeys=devsecops-abz_devsecops-abarna&severities=BLOCKER"
            ''',
            returnStdout: true
          )

          // Parse SonarQube API response
          def sonarIssues = readJSON text: sonarResponse

          // Check if there are blocker issues
          if (sonarIssues.total > 0) {
            echo "Blocker issues found: ${sonarIssues.total}"
            
            // Loop through the issues and create Jira tickets
            sonarIssues.issues.each { issue ->
              def jiraPayload = """
              {
                "fields": {
                  "project": {
                    "key": "${env.JIRA_PROJECT_KEY}"
                  },
                  "summary": "SonarQube Blocker: ${issue.message}",
                  "description": "SonarQube reported a blocker issue in ${issue.component}.\\n\\nMore details: ${issue.key}",
                  "issuetype": {
                    "name": "Bug"
                  }
                }
              }
              """

              // Create Jira issue using API
              withCredentials([string(credentialsId: 'JIRA_API_TOKEN', variable: 'JIRA_API_TOKEN')]) {
                sh """
                curl -X POST -H "Authorization: Bearer ${JIRA_API_TOKEN}" \
                -H "Content-Type: application/json" \
                -d '${jiraPayload}' \
                "${env.JIRA_URL}/rest/api/2/issue/"
                """
              }
            }
          } else {
            echo "No blocker issues found."
          }
        }
      }
    }
