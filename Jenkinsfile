pipeline {
    agent any
    
    stages {
        stage('1. Ambil Kode') {
            steps {
                checkout scm
            }
        }

        stage('2. Analisis Keamanan (SAST)') {
            steps {
                script {
                    // Panggil alat Scanner secara manual agar anti-error
                    def scannerHome = tool 'SonarScanner' 
                    
                    // Jalankan scan
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('3. Quality Gate (HARD BLOCKING)') {
            steps {
                timeout(time: 10, unit: 'MINUTES') { 
                    script {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "SKRIPSI ALERT: Pipeline diblokir! Status: ${qg.status}"
                        }
                    }
                }
            }
        }
    }
}
