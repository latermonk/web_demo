pipeline {
   agent any

   stages {
      stage('pull code') {
         steps {
            git 'https://github.com/latermonk/web_demo.git'
         }
      }
      stage('code checking') {
         steps {

            script {
                 //引入SonarQubeScanner工具
                scannerHome = tool 'sonar-scanner'
            }
            //引入SonarQube的服务器环境
            withSonarQubeEnv('sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner"
            }
         }
      }
      stage('build project') {
         steps {
            sh 'mvn clean package'
         }
      }
      stage('publish project') {
         steps {
            deploy adapters: [tomcat8(credentialsId: 'fc23e5b7-9930-4dfb-af66-a2a576be52fb', path: '', url: 'http://192.168.66.102:8080')], contextPath: null, war: 'target/*.war'
         }
      }
   }
   post {
         always {
            emailext(
               subject: '构建通知：${PROJECT_NAME} - Build # ${BUILD_NUMBER} - ${BUILD_STATUS}!',
               body: '${FILE,path="email.html"}',
               to: '1014671449@qq.com'
            )
         }
   }
}
