pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
			    sh 'echo "Build started at: $(date)"'
                sh 'echo "Running on: $(uname -a)"'
                sh 'rm -rf build'
                sh 'cmake -B build -S .'
                // sh 'cmake --build build'
                sh 'START=$(date +%s); cmake --build build; END=$(date +%s); echo "Build took $((END - START)) seconds."'
            }
        }
        stage('Test') {
            steps {
                sh './build/casino_game' || echo "Test failed!"'
                sh './build/test_game > test_output.log'
                archiveArtifacts artifacts: 'test_output.log', fingerprint: true

            }
        }
        stage('Deliver') {
            steps {
                // sh 'tar -czf casino_game.tar.gz build/casino_game'
                // archiveArtifacts artifacts: 'casino_game.tar.gz', fingerprint: true

                script {
                    def version = sh(script: "date +%Y%m%d%H%M%S", returnStdout: true).trim()
                    sh "tar -czf casino_game_${version}.tar.gz build/casino_game"
                    archiveArtifacts artifacts: "casino_game_${version}.tar.gz", fingerprint: true
                }
            }
        }
    }
}