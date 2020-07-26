def mvnHome

node('node'){
   stage('git checkout'){
      try{
      git credentialsId: 'jenkins', url: 'https://github.com/thiringai-evans/CI_CD_Integration'
      } catch(err){
         sh "echo error in checkout"
      }
   }

   stage('maven test'){
      try{
         mvnHome=tool'maven-3.6.3'
         sh "$mvnHome/bin/mvn --version"
         sh "$mvnHome/bin/mvn clean test surefire-report:report"
      } catch(err) {
         sh "echo error in defining maven"
      }
   }

   stage('test case and report'){
      try {
      echo "executing test cases"   
      junit allowEmptyResults: true, testResults: '/addressbook_main/target/surefire-reports'
      publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'addressbook_main/target/site', reportFiles: 'surefire-report.html', reportName: 'SureFireReportHTML', reportTitles: ''])
      
      } catch(err){
         throw err
      }
   }

   stage('package and generate artifacts'){
      try{
         sh "$mvnHome/bin/mvn clean package -DskipTests=true"
      } catch(err) {
         sh "echo error in packaging and generating artifacts"
      }
   }
}