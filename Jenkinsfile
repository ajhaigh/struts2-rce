def policyEvaluation

pipeline {
  agent {
    docker {
      label 'docker'
      image 'maven:latest'
      args '-v $HOME/.m2:/root/.m2'
         // import groovy.json.*
      }
   }

   stages {
//      stage('Preparation') {
//         checkout scm
//      }

      stage('Build') {
         steps {
            // Run the maven build
            sh 'mvn --version'
            // sh 'mvn -Dmaven.test.failure.ignore clean'
            // install'
//            sh 'npm install'
         }
      }

      stage('IQ Policy Check') {
         steps {
            policyEvaluation = nexusPolicyEvaluation iqStage: 'build', iqApplication: 'struts2-rce__ajhaigh', 
               iqScanPatterns: [[scanPattern: 'target/struts2-rest-showcase-2.5.10.war']],
               enableDebugLogging: true, failBuildOnNetworkError: true
            sh "echo ${policyEvaluation.applicationCompositionReportUrl}"
         }
      }

       stage('Create TAG') {
          steps {
             echo "Create a Tag"
//        createTag nexusInstanceId: 'nxrm3', tagAttributesJson: '''{
//            "IQScan": "\${policyEvaluation.applicationCompositionReportUrl}",
//            "JenkinsBuild": "\${BUILD_URL}"
//          }''', tagName: "IQ-Policy-Evaluation_${currentBuild.number}"
          }
       }

       stage('Publish') {
          steps {
             echo "Publish build"
//        nexusPublisher nexusInstanceId: 'nxrm3', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: ["./target/struts2-rest-showcase.war"], mavenCoordinate: [artifactId: 'struts2-rest-showcase', groupId: 'org.apache.struts', packaging: 'war', version: '2.5.10']]], tagName: "IQ-Policy-Evaluation_${currentBuild.number}"
          }
       }
   }
}  

