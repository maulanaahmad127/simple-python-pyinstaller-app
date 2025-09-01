node {
    stage('Build') {
        docker.image('python:2-alpine').inside("-v ${pwd()}:/workspace -w /workspace") {
            sh "python -m py_compile sources/add2vals.py sources/calc.py"
        }
    }

    stage('Test') {
        docker.image('qnib/pytest').inside("-v ${pwd()}:/workspace -w /workspace") {
            try {
                sh "py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py"
            } finally {
                junit 'test-reports/results.xml'
            }
        }
    }

    stage('Deploy') {
        docker.image('cdrx/pyinstaller-linux:python2').inside("-v ${pwd()}:/workspace -w /workspace") {
            sh "pyinstaller --onefile sources/add2vals.py"
        }
        archiveArtifacts 'dist/add2vals'
    }
}
