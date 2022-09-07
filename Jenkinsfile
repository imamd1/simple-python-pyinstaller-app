node {
  stage('Build') {
    docker.image('python:2-alpine') {
      sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
    }
  }
}