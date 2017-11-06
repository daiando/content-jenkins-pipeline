pipeline {
  agent any

  stages {
    stage('build') {
      steps {
        sh 'javac -d . src/*.java'
        sh 'echo Main-Class: Rectangulator > MANIFEST.MF'
        sh 'jar -cvmf MANIFEST.MF rectangle.jar *.class'
      }
      post {
        success {
          archiveArtifacts artifacts: 'rectangle.jar', fingerprint: true
        }
      }
    }
    stage('run') {
      steps {
        sh 'java -jar rectangle.jar 7 9'
      }
    }
    stage('Promote Dev branch to Master') {
      when {
        branch 'development'
      }
      steps {
        echo 'Stashing Any Local Changes now ...'
        sh 'git stash'
        echo 'Checking Out Development Branch'
        sh 'git checkout development'
        sh 'git pull origin development'
        echo 'Checking Out Master Branch'
        sh 'git checkout master'
        sh 'git pull origin master'
        echo 'Merging Development into Master'
        sh 'git merge development'
        echo 'Pushing to Origin Master'
        sh 'git push origin master'
      }
    }
  }

}
