pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }

    stage('Test') {
      post {
        always {
          junit(allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml')
        }

      }
      steps {
        sh 'mvn test'
      }
    }

    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
      }
    }

  }
  
  post {
	always {
		echo '开始构建'
		script {
			jenkins_projectname=env.JOB_NAME.substring(0, env.JOB_NAME.indexof('/'))
			log_url=env.JENKINS_URL+"blue/organizations/jenkins/"+jenkins_projectname+"/detail/"+env.BRANCH_NAME+"/"+env.BUILD_NUMBER+"/pipeline#
		}
		dingTalk(accessToken: "$jenkins_dingding_token", imageUrl: 'http://icon-park.com/imagefiles/loading7_gray.gif', message: "开始构建", jenkinsUrl: "${log_url}")
		emailext(subject: '$DEFAULT_SUBJECT', body: '$DEFAULT_CONTENT', attachLog: true, compressLog: true, postsendScript: '$DEFAULT_POSTSEND_SCRIPT', presendScript: '$DEFAULT_PRESEND_SCRIPT', replyTo: '', to: '1016570682@qq.com', from: '929457098@qq.com')
	}
	
	failure {
		echo '构建失败'
		script {
			jenkins_projectname=env.JOB_NAME.substring(0, env.JOB_NAME.indexof('/'))
			log_url=env.JENKINS_URL+"blue/organizations/jenkins/"+jenkins_projectname+"/detail/"+env.BRANCH_NAME+"/"+env.BUILD_NUMBER+"/pipeline#
		}
		echo "${log_url}"
		dingTalk(accessToken: "$jenkins_dingding_token", imageUrl: 'http://www.iconsdb.com/icons/preview/soylent-red/x-mark-3-xxl.png', message: "构建失败", jenkinsUrl: "${log_url}")
	}
	
	success {
		echo '构建成功'
		script {
			jenkins_projectname=env.JOB_NAME.substring(0, env.JOB_NAME.indexof('/'))
			log_url=env.JENKINS_URL+"blue/organizations/jenkins/"+jenkins_projectname+"/detail/"+env.BRANCH_NAME+"/"+env.BUILD_NUMBER+"/pipeline#
		}
		echo "${log_url}"
		dingTalk(accessToken: "$jenkins_dingding_token", imageUrl: 'http://icons.iconarchive.com/icons/paomedia/small-n-flat/1024/sign-check-icon.png', message: "构建成功", jenkinsUrl: "${log_url}")
	}
  }
}
