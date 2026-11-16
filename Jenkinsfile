pipeline {
    agent any

    environment {
        // Universal Docker Desktop path fallbacks for Windows
        PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;${env.LOCALAPPDATA}\\Programs\\DockerDesktop\\resources\\bin;${env.PATH}"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                bat '''
                    @echo off
                    where docker >nul 2>&1 || (
                        for /d %%i in (C:\\Users\\* D:\\Users\\*) do (
                            if exist "%%i\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" (
                                set "PATH=%%i\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;%PATH%"
                            )
                        )
                    )
                    docker build -t jenkins-flask-app .
                '''
            }
        }

        stage('Run Container') {
            steps {
                echo 'Running Docker container...'
                bat '''
                    @echo off
                    where docker >nul 2>&1 || (
                        for /d %%i in (C:\\Users\\* D:\\Users\\*) do (
                            if exist "%%i\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" (
                                set "PATH=%%i\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;%PATH%"
                            )
                        )
                    )
                    docker rm -f test-app 2>nul || ver >nul
                    docker run -d -p 5000:5000 --name test-app jenkins-flask-app
                '''
            }
        }
    }

    post {
        always {
            echo 'Cleaning up test container...'
            bat '''
                @echo off
                where docker >nul 2>&1 || (
                    for /d %%i in (C:\\Users\\* D:\\Users\\*) do (
                        if exist "%%i\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" (
                            set "PATH=%%i\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;%PATH%"
                        )
                    )
                )
                docker rm -f test-app 2>nul || ver >nul
            '''
        }
    }
}
