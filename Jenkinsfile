node("kube2"){
    git 'git@github.com:roachmaster/raspberry-pi-mariadb-deployment.git'
    String tempString;
    withCredentials([usernamePassword(credentialsId: '8047ae57-cfa7-4ee1-86aa-be906b124593', passwordVariable: 'credPw', usernameVariable: 'credName')]) {
        tempString = sh(returnStatus: true, script: 'kubectl get secrets | grep -c mysql-pass')
        if(tempString.trim().equals("1")){
            println("Adding Secret");
            sh "kubectl create secret generic mysql-pass --from-literal=password=${credPw}"
        }
    }

    tempString = sh(returnStatus: true, script: 'kubectl get deployments | grep -c pi-mariadb')
    if(tempString.trim().equals("1")){
         tempString = sh(returnStatus: true, script: 'kubectl get configmap | grep -c mysql-initdb-config')
         if(!tempString.trim().equals("1")){
             println("Removing mysql-initdb-config configmap");
             sh "kubectl delete configmap mysql-initdb-config"
         }
         println("Adding mysql-initdb-config configmap");
         sh "kubectl create -f k3s/configmap.yml"

        tempString = sh(returnStatus: true, script: 'kubectl get pvc | grep -c mariadb-pv-claim')
        if(!tempString.trim().equals("1")){
            println("Removing mariadb-pv-claim pvc");
            sh "kubectl delete pvc mariadb-pv-claim"
        }
        println("Adding mariadb-pv-claim pvc");
        sh "kubectl create -f k3s/pvc.yml"
        tempString = sh(returnStatus: true, script: 'kubectl get svc | grep -c pi-mariadb')
        if(!tempString.trim().equals("1")){
            println("Removing pi-mariadb svc");
            sh "kubectl delete svc pi-mariadb"
        }
        println("Removing pi-mariadb svc");
        sh "kubectl create -f k3s/service.yml"

        println("Adding pi-mariadb deployment");
        sh "kubectl create -f k3s/deployment.yml"
    }
}
