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

#### Como Criar um pod:
<details>
<summary>passo-a-passo</summary>

1 - Criando o pod
```
kubectl run nginx-pod-name --image=nginx
```
2 - Verificando a criação do POD

**Visualização continua do status do pod**
```
kubectl get pods --watch
```
**Visualização detalhada:**
  ```
kubectl get pods -o wide
```  

3 - Verificando detalhes sobre o pod
```
kubectl describe pod nginx-pod-name
```
4 - Editar um pod
```
kubectl edit pod nginx-pod-name
```
</details>

#### Criando Pod's de maneira declarativa:
<details>
<summary>passo-a-passo</summary>  

  
1 - Crie o arquivo do pod .yaml
````
apiVersion: v1
kind: Pod
metadata:
  name: primeiro-pod-declarativo
spec:
  containers:
    - name: nginx-container
      image: nginx
````

2 - Execute o comando apply para criar o pod de forma declarativa
````
kubectl apply -f .\primeiro-pod.yaml
````
</details>

#### Como acessar o pod via terminal?
````
kubectl exec -it portal-noticias -- bash
````

#### Como deletar um pod:
````
kubectl delete pod pod-name
kubectl delete -f ./file-name.yaml
````
Após isso podemos usar o comando `getpods` para verificar se o pod ainda existe. 

````
kubectl get pods
````

## Svc - Service 
Trata-se de um recurso do kubernetes que prove uma abstração que:
* Expoe a aplicação executando um ou mais pod's
* Prove IP's fixos para comunicação.
* Prove um DNS para um ou mais Pods facilitando a comunicação quando os pods cairem ou precisarem ser recriados.
* Pode fazer o balanceamento de carga entre os pods.

### Tipos de serviços:
* ClusterIP
* NodePort
* LoadBalancer

<details>
<summary>passo-a-passo</summary> 
<details>

## Replicasets
## Deployments
## Volume
## Hpa - Horizontal Pod Autoscaler
## Pv - Persistentvolume
## Ing - Ingress
## Pvc - Persistentvolumeclaim
## Sc - Storage Classes 
## Ds - Daemonset
## Quota

### Glossário:
###### O que é e para que serve a API do Kubernetes: 
A api tem o proposito de integrar um terminal ou cliente como kubectl com os recursos do kubernetes.  
###### Para que serve o `kubectl`:
Serve para integrar com os recursos de API do Kubernetes.
###### Quando um pode é dado como encerrado?
Quando todos os seus containers dentro do pod param de funcionar.

