node {
  stage('Build') {
    withDockerContainer('python:2-alpine') {
      sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
    }
  }
}