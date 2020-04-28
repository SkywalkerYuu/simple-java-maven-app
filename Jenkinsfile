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
		dingTalk(
			robot: '35ec4006-298f-41da-ab4d-8595198496b3',
			type: 'TEXT'
			text:[
				"开始构建",
				"这里是构建信息"
			],
			at: [
				'15223487404'
			],
			picUrl: 'http://icon-park.com/imagefiles/loading7_gray.gif'
		)
		emailext(
			subject: '$DEFAULT_SUBJECT',
			body: '$DEFAULT_CONTENT',
			attachLog: true,
			compressLog: true,
			postsendScript: '$DEFAULT_POSTSEND_SCRIPT',
			presendScript: '$DEFAULT_PRESEND_SCRIPT',
			replyTo: '',
			to: '1016570682@qq.com',
			from: '929457098@qq.com'
		)
	}
	
	failure {
		echo '构建失败'
		dingTalk(
			robot: "35ec4006-298f-41da-ab4d-8595198496b3",
			type: 'LINK',
			title: '构建失败',
			text: [
				'测试链接型的信息',
				'分行信息'
			],
			messageUrl: 'http://www.baidu.com',
			picUrl: 'http://www.iconsdb.com/icons/preview/soylent-red/x-mark-3-xxl.png'
		)
	}
	
	success {
		echo '构建成功'
		dingTalk(
			robot: "35ec4006-298f-41da-ab4d-8595198496b3",
			type: 'MARKDOWN',
			title: '构建成功',
			text: [
				'# 构建成功的标题',
				'消息正文：测试MARKDOWN类型的消息',
				'---',
				'## 二级标题',
				'### 三级标题',
				'#### 四级标题',
				'* 列表1',
				'* 列表2',
				'* 列表3',
				'* 列表4',
				'[链接测试](http://www.baidu.com)',
				'![图片链接](http://5b0988e595225.cdn.sohucs.com/images/20190112/a5bb4d5f621d4a15a42b54f3817ae9ad.jpeg)',
				'**<font color='red' size=4>格式文字</font>**'
			],
			picUrl: 'http://icons.iconarchive.com/icons/paomedia/small-n-flat/1024/sign-check-icon.png',
	}
  }
}
