pipeline {
    agent any

    stages {
        stage('Информация о дисках (Disk Usage)') {
            steps {
                echo '=== 1. РАЗМЕРЫ РАЗДЕЛОВ ДИСКА (df -h) ==='
                sh 'df -h'

                echo ''
                echo '=== 2. РАЗМЕР ТЕКУЩЕЙ РАБОЧЕЙ ДИРЕКТОРИИ (du -sh .) ==='
                sh 'du -sh .'

                echo ''
                echo '=== 3. ТОП-5 САМЫХ БОЛЬШИХ ПАПОК ВНУТРИ ==='
                sh 'du -h --max-depth=1 . 2>/dev/null | sort -hr | head -5'
            }
        }

        stage('Процесс-монстр (Top Memory Process)') {
            steps {
                echo '=== 4. ПРОЦЕСС, ЗАНИМАЮЩИЙ БОЛЬШЕ ВСЕГО ОЗУ (RAM) ==='
                sh '''
                    echo "Список топ-5 процессов по памяти:"
                    ps aux --sort=-%mem | head -n 6 | awk '{print "PID: " $2 " | MEM%: " $4 " | RSS: " $6 " KB | CMD: " $11}'
                    
                    echo ""
                    echo "ПОБЕДИТЕЛЬ (самый жирный процесс):"
                    ps aux --sort=-%mem | head -n 2 | tail -n 1 | awk '{print "Имя: " $11 " | Память: " $4 "% | PID: " $2}'
                '''
            }
        }
    }

    post {
        // Эта секция выполнится всегда после сборки
        always {
            echo 'Проверка дисков и памяти завершена.'
        }
    }
}
