pipeline {
  agent any
  tools {
    maven "maven"
    jdk "8"
  }

  environment {
    build_version = "${BUILD_NUMBER}"
  }

  stages {
    stage("checkout") {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/soridisaster/simple_maven.git']]])
      }
    }

    stage("build") {
      steps {
        bat "mvn compile"
      }
    }

    stage("tests") {
      steps {
        bat "mvn test"
      }
    }

    stage("jar") {
      steps {
        bat "mvn -Dbuild_version=${build_version} package"
      }
    }

    stage("publish") {
      steps {
        bat "move /Y \"${workspace}\\target\\jenkins-sample-*\" C:\\Users\\sucha\\Downloads"
      }
    }
}

  post {
    always {
      emailext body: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:\nCheck console output at $BUILD_URL to view the results.l', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!'
    }
  }
}
