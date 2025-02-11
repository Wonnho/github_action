pipeline {
  agent any

  stages {  
    stage('Load Environment Variables') {
      steps {
        script {
          // Jenkins Credentials에서 Secret File 가져오기
          // credentialsId : credentials 생성 당시 작성한 ID
          // variable : 스크립트 내부에서 사용할 변수 이름
          // 환경 변수 파일 로드
          withCredentials([file(credentialsId: 'env-file', variable: 'env_file')]) {
            // 환경 변수 파일을 작업 폴더로 복사
            sh 'cp $env_file .env'

            // 환경 변수 파일 권한 설정 (읽기/쓰기)
            sh 'chmod 644 .env'

          }
        }
      }
	 }
  

		stage('Build Start') {
      steps {
        script {
          // Jenkins Credentials에서 Secret Text 가져오기
          // credentialsId : credentials 생성 당시 작성한 
          // variable : 스크립트 내부에서 사용할 변수 이름 
          withCredentials([string(credentialsId: 'discord-webhook', variable: 'discord_webhook')]) {
                // 디스코드 알림 메세지 작성 
                // description : 메세지 설명문
                // link : Jenkins BUILD 주소
                // title : 메세지 제목
                // webhookURL : 메세지 전송 URL
                discordSend description: """
                Jenkins Build Start
                """,
                link: env.BUILD_URL, 
                title: "${env.JOB_NAME} : ${currentBuild.displayName} 시작", 
                webhookURL: "$discord_webhook"
            }
        }
      }
    }

	 stage("Docker Image Build & Container Run") {
		steps {
			script {
				sh 'docker compose build'
				
				//container down and up 
				sh 'docker compose down'

				sh 'docker compose up -d'
			}
		}
	 }
	 }
  
	   
  post {
        success {
            withCredentials([string(credentialsId: 'discord-webhook', variable: 'discord_webhook')]) {
                        discordSend description: """
                        제목 : ${currentBuild.displayName}
                        결과 : ${currentBuild.currentResult}
                        실행 시간 : ${currentBuild.duration / 1000}s
                        """,
                        link: env.BUILD_URL, result: currentBuild.currentResult, 
                        title: "${env.JOB_NAME} : ${currentBuild.displayName} 성공", 
                        webhookURL: "$discord_webhook"
            }
        }
        failure {
            withCredentials([string(credentialsId: 'discord-webhook', variable: 'discord_webhook')]) {
                        discordSend description: """
                        제목 : ${currentBuild.displayName}
                        결과 : ${currentBuild.currentResult}
                        실행 시간 : ${currentBuild.duration / 1000}s
                        """,
                        link: env.BUILD_URL, result: currentBuild.currentResult, 
                        title: "${env.JOB_NAME} : ${currentBuild.displayName} 실패", 
                        webhookURL: "$discord_webhook"
            }
        }
    }
}