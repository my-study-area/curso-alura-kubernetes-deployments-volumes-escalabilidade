# curso-alura-kubernetes-deployments-volumes-escalabilidade
Curso de Kubernetes: Deployments, Volumes e Escalabilidade

## Anotações
```bash
# inicia um cluster kubernetes
minikube start --vm-driver=virtualbox

# exibe a versão do client
kubectl version --client

# exibe os nodes
kubectl get nodes

# cria um pod
kubectl run nginx-pod --image=nginx:latest

# lista os pods
kubectl get pods

# lista os pods assistindo as modificações
kubectl get pods --watch

# mostra os detalhes do pod nginx-pod
kubectl describe pod nginx-pod

# edita as propriedade de um pod
kubectl edit pod nginx-pod

# cria um pod declarativo
kubectl apply -f ./primeiro-pod.yaml

# deleta pod nginx-pod
kubectl delete pod nginx-pod

# deleta pod declarativamente
kubectl delete -f ./primeiro-pod.yaml

#
kubectl apply -f ./portal-noticias.yaml

# acessa terminal
kubectl exec -it portal-noticias -- bash

# lista os pods no formato wide
kubectl get pods -o wide

# lista os services
kubectl get svc

# removes todos os pods
kubectl delete pods --all

# removes todos os services
kubectl delete services --all

# instala ping dentro do container
apt-get update && apt-get install -y iputils-ping

# instala mysqlclient no container
apt-get install -y default-mysql-client

# lista os resulsets
kubectl get rs

# lista os deployments
kubectl get deployments

# lista as revisões
kubectl rollout history deployment nginx-deployment

# gera uma revisão
kubectl -f deployments --record=true

# adiciona uma descrição na evisão
kubectl annotate deployment nginx-deployment \
kubernetes.io/change-cause="Definindo a imagem com versão latest"

# volta para a revisão 2
kubectl rollout undo deployment nginx-deployment --to-revision=2
```

Exemplo de ReplicaSet:
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: portal-noticias-replicaset
spec:
  template:
    metadata:
      name: porta-noticias
      labels:
        app: portal-noticias
    spec:
      containers:
        - name: portal-noticias-container
          image: aluracursos/portal-noticias:1
          ports:
            - containerPort: 80
          envFrom:
            - configMapRef:
                name: portal-configmap
  replicas: 3
  selector:
    matchLabels:
      app: portal-noticias
```

Exemplo de Deployment:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  template:
    metadata:
      name: nginx-pod
      labels:
        app: nginx-pod
    spec:
      containers:
        - name: nginx-container
          image: nginx:1
          ports:
            - containerPort: 80
  selector:
    matchLabels:
      app: nginx-pod
```
