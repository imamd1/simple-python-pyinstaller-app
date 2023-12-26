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
    stage('Deploy') {
      withEnv(['VOLUME = \'$(pwd)/sources:/src\'', 'IMAGE = \'cdrx/pyinstaller-linux:python2\'']) {
      dir('env.BUILD_ID') {
          sh 'docker run -v ${VOLUME} ${IMAGE} pyinstaller -F add2vals.py'
          unstash 'compiled-results'
          sleep 60
          archiveArtifacts "sources/dist/add2vals" 
          sh 'docker run -v ${VOLUME} ${IMAGE} rm -rf build dist'
      }
    }
  }
}
