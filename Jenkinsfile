node {
    stage('Checkout') {
        // ambil seluruh isi repo, bukan cuma Jenkinsfile
        checkout scm
        sh 'ls -R .'
    }
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside {
            try {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } finally {
                junit 'test-reports/results.xml'
            }
        }
    }
    stage('Deploy') {
    docker.image('python:2-alpine').inside('--user root') {
        sh '''
            apk add --no-cache gcc musl-dev libffi-dev make binutils
            pip install --no-cache-dir "pyinstaller==3.6"
            pyinstaller --onefile sources/add2vals.py
        '''
    }
    archiveArtifacts 'dist/add2vals'
}
}
