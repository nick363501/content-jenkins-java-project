pipeline  {
  agent { label 'master' }
  environment {
      DEPLOY_DIR = "/tmp/awstest"
      MAJOR_VERSION =1
    }
  tools {
    ant "ant-1.10.1"
    jdk "sun-jdk-8u31"
	maven "maven3"
    }
  stages {
    stage( 'checkout' ) {
        agent {
          label 'master'
        }
        steps {
          checkout scm
        }
    }

    stage ('build') {
      agent {
       label 'master'
      }
      steps {
        sh 'ant -f build.xml -v'
      }
    }

    stage ('envNames'){
      agent {
         label 'master'
      }
      steps {
        sh "env"
      }
    }

    stage ('deploy') {
      agent {
        label 'master'
      }
      steps {
         sh "mkdir -p ${DEPLOY_DIR}"
         sh "cp ${env.WORKSPACE}/dist/rectangle_${env.MAJOR_VERSION}.${BUILD_NUMBER}.jar ${DEPLOY_DIR}"
      }
     }
    stage ('Unit Test') {
      agent {
        label 'master'
        }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage ("Test on Linux") {
      agent {
        label 'master'
      }
      steps {
        sh "java -jar ${DEPLOY_DIR}/rectangle_${env.MAJOR_VERSION}.${BUILD_NUMBER}.jar 3 4"
      }
    }
    stage ("Promote to Green") {
      agent {
        label 'master'
      }
      steps {
        sh "mkdir -p ${DEPLOY_DIR}/green"
        sh "cp -rf ${DEPLOY_DIR}/rectangle_${env.MAJOR_VERSION}.${BUILD_NUMBER}.jar ${DEPLOY_DIR}/green/rectangle_${env.MAJOR_VERSION}.${BUILD_NUMBER}.jar"
      }
    }
    }
}