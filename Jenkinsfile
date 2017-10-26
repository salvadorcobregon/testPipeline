pipeline {
  agent any
  stages {
    stage('CodeDownload') {
      steps {
        git(url: 'https://github.com/salvadorcobregon/testPipeline', branch: 'master', credentialsId: '8e2c8551-b0b5-4180-8c2a-a0194c8b593d')
      }
    }
    stage('Prepare the Environment') {
      steps {
        sh 'source $JENKINS_HOME/.bash_profile'
      }
    }
    stage('Code Analysis Phase') {
      steps {
        parallel(
          "Code Analysis Phase": {
            sh 'cd $WORKSPACE && hybris_search -k $KEYS -e png -p .'
            
          },
          "Prepare the ReportUI": {
            sh 'mv $WORKSPACE/report*.json $REPORTFOLDER/'
            sh '#nohup java -jar /opt/code-scan/code-scan-2.0.0-SNAPSHOT.jar &'
            
          },
          "Restart the WebServer": {
            sh 'service httpd restart'
            
          }
        )
      }
    }
    stage('Serve the Report') {
      steps {
        echo 'Please access to the following link http://34.214.73.95/#!/files'
      }
    }
  }
  environment {
    KEYS = '/opt/analyzer/bin/oracle.key'
    WORKSPACE = '/var/lib/jenkins/workspace/AnalysisPOC'
    REPORTFOLDER = '/opt/code-scan/reports'
  }
}