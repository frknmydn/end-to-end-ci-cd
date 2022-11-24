pipeline{ 
    agent any 
    stages{ 
        stage{'Git checkout'}{ 
            steps{ 
                git branch: 'main', url: 'https://github.com/frknmydn/end-to-end-ci-cd.git' 
            } 
        } 
    } 
}
