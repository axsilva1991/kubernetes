# Kubernetes - Orquestrador de containers
É uma ferramenta capaz de criar e gerenciar uma ou mais maquinas trabalhando em conjunto (Cluster).  
* Podendo também escalar as aplicações automáticamente.
* Reiniciar aplicações caso entrem em falha.
* E executar em diferentes ambientes de cloud/sistemas operacionais, como exemplo (GCP, AWS, AZURE e etc).

## POD
* Trata-se de uma capsula que pode conter 1 ou mais container's.  
* Cada Pod terá 1 endereço de IP, posso compartilhar o mesmo endereço de ip para varios containers dentro.
* Caso os container's parem o pod é interrompido, caso o pod possua apenas um de de seus containers fora ele nao será recriado. 
* Pods são efêmeros são feitos para serem re-criados a qualquer momento.
* Pods compartilham os mesmos namespaces de rede e IPC e podem compartilhar volumes.
### Como Criar um pod:

1 - Criando o pod
```
kubectl run nginx-pod-name --image=nginx
```
2 - Verificando a criação do POD
```
kubectl get pods --watch
```
3 - Verificando detalhes sobre o pod
```
kubectl describe pod nginx-pod-name
```
4 - Editar um pod
```
kubectl edit pod nginx-pod-name
```
#### Criando Pod's de maneira declarativa:

1 - Crie o arquivo do pod .yaml
````
apiVersion: v1
kind: Pod
metadata:
  name: primeiro-pod-declarativo
  labels:
    role: myrole
spec:
  containers:
    - name: nginx-container
      image: nginx
````

2 - Execute o comando apply para criar o pod de forma declarativa
````
kubectl apply -f .\primeiro-pod.yaml
````
## Replicasets
## Deployments
## Volume
## Hpa - Horizontal Pod Autoscaler
## Pv - Persistentvolume
## Ing - Ingress
## Pvc - Persistentvolumeclaim
## Svc - Service 
## Sc - Storage Classes 
## Ds - Daemonset
## Quota

### Glossário:
###### O que é e para que serve a API do Kubernetes: 
A api tem o proposito de integrar um terminal ou cliente como kubectl com os recursos do kubernetes.  
###### Para que serve o kubectl:
Serve para integrar com os recursos de API do Kubernetes.
