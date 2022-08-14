To ssh into the pod  
#For Git bash
alias kubectl='winpty kubectl'  
kubectl exec -it ${PODNAME} -- /bin/bash  
mysql -h"$HOSTNAME" -P"$PI_MARIADB_SERVICE_PORT_PI_MARIADB" -uroot -p"$MYSQL_PASSWORD"