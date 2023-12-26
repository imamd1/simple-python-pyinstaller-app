node(){
  withDockerContainer('python:3.8-alpine') {
    stage('Build') {
      checkout scm
      sh 'python -m py_compile sources/add2vals.py sources/calc.py'
      stash includes: 'sources/*.py*', name: 'compiled-results'
    }
  }
  withDockerContainer('qnib/pytest') {
    stage('Test') {
      checkout scm
      sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
    }
  }
//   stage('Manual Approval') {
//     input message: 'Lanjut ke Tahap Berikutnya?'
//   }

  withDockerContainer('cdrx/pyinstaller-linux:python2') {
    stage('Deploy') {
    checkout scm
        dir('env.BUILD_ID') {
            sh 'docker run -v $(pwd)/sources:/src \'pyinstaller -F add2vals.py\''
            unstash 'compiled-results'
            sleep 60
            archiveArtifacts "sources/dist/add2vals" 
            sh 'docker run -v $(pwd)/sources:/src \'rm -rf build dist\''
        }
        
      }
      }
   
}
