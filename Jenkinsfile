node{
    
    stage('get code'){
        git 'https://github.com/sherinstephen87/devops-e1-insure-me.git'
    }
    
    stage('build package'){
        sh 'mvn clean package'
    }
    
    stage('containerize'){
         sh 'docker build -t sherinstephen87/devops-insure-me:2.0 .'
    }
    
    stage('push to docker'){
         sh "docker login -u sherinstephen87 -p dckr_pat_70iJuI44OtSwB-9zkgUXWsDFHgU"
        //withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerhub-pwd')]) {
          //  sh "docker login -u sherinstephen87 -p ${dockerhub-pwd}"
            sh 'docker push sherinstephen87/devops-insure-me:2.0'
        //}
    }
    
    stage('deploy to test'){
        ansiblePlaybook become: true, credentialsId: 'ansible-aws-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    
    stage('get testing code'){
        git 'https://github.com/sherinstephen87/devops-insure-me-testing.git'
    }
    
    stage('build test code'){
        sh 'mvn clean package assembly:single'
    }
    
    stage('test the app'){
        sh 'java -jar target/my-assignment-0.0.1-SNAPSHOT-jar-with-dependencies.jar'
    }
    
    stage('get app code again'){
        git 'https://github.com/sherinstephen87/devops-e1-insure-me.git'
    }
    
    stage('build app code'){
        //
    }
    
    stage('deploy to prod'){
        ansiblePlaybook become: true, credentialsId: 'ansible-aws-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }
    
}
