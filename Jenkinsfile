node(){
  stage('Build') {
    withDockerContainer('python:3.8-alpine') {
      sh 'python -m py_compile sources/add2vals.py sources/calc.py'
      stash includes: 'sources/*.py*', name: 'compiled-results'
    }
  }
  stage('Test') {
    withDockerContainer('qnib/pytest') {
      sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
    }
  }
//   stage('Manual Approval') {
//     input message: 'Lanjut ke Tahap Berikutnya?'
//   }
//   stage('Deploy') {
//     dir('env.BUILD_ID') {
//         sh 'docker run -v $(pwd)/sources:/src cdrx/pyinstaller-linux:python2 \'pyinstaller -F add2vals.py\''
//         unstash 'compiled-results'
//         sleep 60
//         archiveArtifacts "sources/dist/add2vals" 
//         sh 'docker run -v $(pwd)/sources:/src cdrx/pyinstaller-linux:python2 \'rm -rf build dist\''
//     }
// //    withDockerContainer('cdrx/pyinstaller-linux:python2') {
     
// //    }
//   }
}
