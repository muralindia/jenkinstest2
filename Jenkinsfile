// see https://dzone.com/refcardz/continuous-delivery-with-jenkins-workflow for tutorial
// see https://documentation.cloudbees.com/docs/cookbook/_pipeline_dsl_keywords.html for dsl reference
// This Jenkinsfile should simulate a minimal Jenkins pipeline and can serve as a starting point.
// NOTE: sleep commands are solelely inserted for the purpose of simulating long running tasks when you run the pipeline
node {
   // Mark the code checkout 'stage'....
   stage 'checkout'

   // Get some code from a GitHub repository
   //git url: 'https://github.com/kesselborn/jenkinsfile'
   git url: 'https://github.com/muralindia/jenkinstest1'
   sh 'git clean -fdx; sleep 4;'

   // Get the maven tool.
   // ** NOTE: This 'mvn' maven tool must be configured
   // **       in the global configuration.
  // def Maven_Home = tool '/opt/maven/apache-maven-3.5.4/bin/mvn'

   stage 'build'
   // set the version of the build artifact to the Jenkins BUILD_NUMBER so you can
   // map artifacts to Jenkins builds
   sh "/opt/maven/apache-maven-3.5.4/bin/mvn versions:set -DnewVersion=${env.BUILD_NUMBER}"
   sh "/opt/maven/apache-maven-3.5.4/bin/mvn clean package deploy"

   stage 'test'
   parallel 'test': {
     sh "/opt/maven/apache-maven-3.5.4/bin/mvn test; sleep 2;"
   }, 'verify': {
     sh "/opt/maven/apache-maven-3.5.4/bin/mvn verify; sleep 3"
   }

   stage 'archive'
   //archive 'target/*.jar'
   archiveArtifacts 'target/*.jar'
}


node {
   stage 'deploy SIT'
   sh 'echo "write your deploy code here"; sleep 2;'

   stage 'deploy Production'
   input 'Proceed?'
   sh 'echo "write your deploy code here"; sleep 3;'
   sh 'rm -rf target/prod && mkdir target/prod && cp target/*.jar target/prod'
   archiveArtifacts 'target/prod/*.jar'
}
