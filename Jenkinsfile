pipeline {
    // Usando uma imagem Docker compatível com sua versão do Playwrightt
    agent {
        docker {
            image 'mcr.microsoft.com/playwright/javascript:v1.47.0-jammy'
            args '-u root'
        }
    }

    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Instalando dependências com NPM CI...'
                // 'npm ci' é a melhor prática para CI/CD: é mais rápido e confiável que 'npm install'
                sh 'npm ci'
            }
        }

        stage('Run Unit Tests (Jest)') {
            steps {
                echo 'Executando os testes unitários com Jest...'
                // Executa o script "test" do seu package.json
                sh 'npm run test'
            }
        }

        stage('Run E2E Tests (Playwright)') {
            steps {
                echo 'Executando os testes E2E com Playwright...'
                // Executa o script "e2e" do seu package.json
                sh 'npm run e2e'
            }
        }
    }

    post {
        always {
            echo 'Coletando relatórios de teste...'
            
            // Publica os resultados dos testes E2E do Playwright
            junit 'results.xml'

            // Arquiva o relatório HTML do Playwright
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true
        }
    }
}