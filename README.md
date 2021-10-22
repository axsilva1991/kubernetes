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
<details>
<summary>passo-a-passo</summary> 

````
kubectl exec -it portal-noticias -- bash
````
</details>

#### Como deletar um pod:
<details>
<summary>passo-a-passo</summary> 


````
kubectl delete pod pod-name
kubectl delete -f ./file-name.yaml
````
Após isso podemos usar o comando `getpods` para verificar se o pod ainda existe. 

````
kubectl get pods
````
</details>

## Svc - Service 
Trata-se de um recurso do kubernetes que prove uma abstração que:
* Expoe a aplicação executando um ou mais pod's
* Prove IP's fixos para comunicação.
* Prove um DNS estável para um ou mais Pods facilitando a comunicação quando os pods cairem ou precisarem ser recriados.
* Pode fazer o balanceamento de carga entre os pods.

Os services podem ser criados de maneira declarativa.

### Tipos de serviços:
* **ClusterIP**  
Neste laboratorio precisamos criar 2 pods para validar o funcionamento do seviço:

<details>
<summary>pod-1.yaml</summary>

  ````
--- 
apiVersion: v1
kind: Pod
metadata: 
  name: criando-cluster-ip-pod1
spec: 
  containers: 
    - 
      image: "nginx:latest"
      name: criando-cluster-ip-pod1-container
      ports: 
        - 
          containerPort: 80
  ````

</details>

<details>
<summary>pod-2.yaml</summary>

  ````
--- 
apiVersion: v1
kind: Pod
metadata: 
  name: criando-cluster-ip-pod2
  labels:
    app: segundo-pod
spec: 
  containers: 
    - 
      image: "nginx:latest"
      name: criando-cluster-ip-pod2-container
      ports: 
        - 
          containerPort: 80
  ````

</details>

Após a criação dos pods podemos vincular um serviço a um dos pod's, neste caso estamos viinculando ao pod2.

<details>
<summary>svc-pod2.yaml</summary>

  ````
apiVersion: v1
kind: Service
metadata: 
  name: svc-pod2
spec: 
  type: ClusterIP
  selector: 
    app: segundo-pod
  ports: 
    - port: 80
      targetPort: 80
  ````
</details>

**port** = Porta exposta pelo serviço.<br>
**targetPort** = Porta que será exposta pelo pod e escutada pelo serviço.


* **NodePort**  
  Neste tipo de serviço que permite uma comunicação com o mundo externo, e também serve como Load Balancer Automático.
  
<details>
<summary>pod-1.yaml</summary>

  ````
--- 
apiVersion: v1
kind: Pod
metadata: 
  name: criando-cluster-ip-pod1
  labels:
    app: primeiro-pod
spec: 
  containers: 
    - 
      image: "nginx:latest"
      name: criando-cluster-ip-pod1-container
      ports: 
        - 
          containerPort: 80
  ````
</details>

<details>
<summary>svc-pod1.yaml</summary>

  ````
apiVersion: v1
kind: Service
metadata:
  name: svc-pod1
spec:
  type: NodePort
  selector:
    app: primeiro-pod
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
  ````
</details>
  
* **LoadBalancer**  
  Neste Serviço podemos utilizar os balanceadores de carga de cloud Providers, como: GCP, AMAZON e etc.
  Por Ser um LoadBalancer, também são um NodePort e ClusterIP ao mesmo tempo.

<details>
<summary>svc-pod-1-loadbalancer.yaml</summary>

  ````
apiVersion: v1
kind: Service
metadata:
  name: svc-pod-loadbalancer-1
spec:
  type: LoadBalancer
  ports:
    - port: 80
      nodePort: 30000
  selector:
    app: portal-noticias-pod
  ````
</details>

## CM - CONFIGMAP
Responsável por armazenar as configurações que são necessárias para determinados recursos. Deixando a sintaxe dos recursos limpa e deixando desacoplada as configurações, visando o re-uso para os outros recursos.

<details>
<summary>svc-pod-1-loadbalancer.yaml</summary>
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
</details>

### Glossário:
###### O que é e para que serve a API do Kubernetes: 
A api tem o proposito de integrar um terminal ou cliente como kubectl com os recursos do kubernetes.  
###### Para que serve o `kubectl`:
Serve para integrar com os recursos de API do Kubernetes.
###### Quando um pode é dado como encerrado?
Quando todos os seus containers dentro do pod param de funcionar.

