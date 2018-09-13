#Setup / Install steps
###########################
kubectl apply -f namespace.yaml
kubectl apply -f server-data-persistentvolumeclaim.yaml -f server-ssl-persistentvolumeclaim.yaml -f ca-data-persistentvolumeclaim.yaml -f ca-ssl-persistentvolumeclaim.yaml -f ../storage/puppetstack-filesystem.yaml -f ../storage/puppetstack-block-rook-storageclass.yaml
kubectl apply -f puppetca-deployment.yaml -f puppetca-service.yaml
kubectl apply -f r10k-cache-persistentvolumeclaim.yaml -f r10k-env-persistentvolumeclaim.yaml -f r10k-ssl-persistentvolumeclaim.yaml && sleep 5 && kubectl apply -f r10k-deployment.yaml -f r10k-service.yaml
# Log into the r10k docker and run 
# kubectl -n puppetstack exec -it r10k-...
# r10 deploy environment

kubectl apply -f puppetserver-deployment.yaml -f puppetserver-service.yaml
kubectl apply -f haproxy-ssl-persistentvolumeclaim.yaml && sleep 5 &&  kubectl apply -f haproxy-deployment.yaml -f haproxy-service.yaml
kubectl apply -f nats-ssl-persistentvolumeclaim.yaml -f postgres-persistentvolumeclaim.yaml -f db-ssl-persistentvolumeclaim.yaml
kubectl apply -f nats-deployment.yaml -f nats-service.yaml -f postgres-deployment.yaml -f postgres-service.yaml
kubectl apply -f puppetdb-deployment.yaml -f puppetdb-service.yaml -f puppetexplorer-deployment.yaml -f puppetexplorer-service.yaml
#Finally when everything works fine and the stack is up. Run a docker client run and check puppet explorer dashboard to confirm puppet serves data
kubectl apply -f puppetclient-deployment.yaml
