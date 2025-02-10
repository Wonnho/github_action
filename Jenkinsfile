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
}