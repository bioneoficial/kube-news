k3d cluster create clusterNewso00 --port '8080:30000@loadbalancer' OK

docker build -t bioneoficial/kube-news . OK
docker push bioneoficial/kube-news:v1 OK
docker tag bioneoficial/kube-news:v1 bioneoficial/kube-news:latest OK
docker push bioneoficial/kube-news:latest OK
kubectl apply -f deplyment
kubectl portforward services 5432 5432
kubectl apply -f deplyment
kubectl portforward pods 8080 8080

rascunho