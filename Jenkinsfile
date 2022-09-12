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
      sh 'pyinstaller --onefile sources/add2vals.py'
      // input: 'Are You Sure?'
    }
  }
}