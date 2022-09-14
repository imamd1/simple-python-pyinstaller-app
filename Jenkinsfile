node {
  stage('Build') {
    withDockerContainer('python:2-alpine') {
      sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    }
  }
  stage('Test') {
    withDockerContainer('qnib/pytest') {
      sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
    }
  }
  stage('Deliver') {
   withDockerContainer('cdrx/pyinstaller-linux:python2') {
    dir('env.BUILD_ID') {
        sh 'docker run -d cdrx/pyinstaller-linux:python2 \'pyinstaller -F add2vals.py\''
        archiveArtifacts "${env.BUILD_ID}/sources/dist/add2vals" 
        sh 'docker run -d cdrx/pyinstaller-linux:python2 \'rm -rf build dist\''
    }
   }
  }
}
