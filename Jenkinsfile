pipeline {
  agent any
  stages {

    stage('git') {
      steps {
        git url: 'https://github.com/michaelparfait/tp-continuous-deployment.git'
      }
    }

    stage('build') {
      steps {
        sh 'docker build -t michael/hashservice .'
      }
    }

    stage('run') {
      steps {
        sh 'docker run -d --name hs -p 3000:3000 --rm michael/hashservice'
      }
    }

    stage('test and stop') {
      steps {
        sh 'sleep 4'
        sh 'curl http://192.168.33.111:3000/md5/hello | grep 5d41402abc4b2a76b9719d911017c592'
        sh 'echo $?'
        sh 'sleep 4'
        sh 'docker stop hs'
      }
    }

    stage('deploy') {
      steps {
        sh 'ansible-playbook -i hosts playbook.yml'
      }
    }

  } 
}