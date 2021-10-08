# Aula 2 - Kubernetes

## Conteudos

- [Kubernetes](#kubernetes)
- [Comandos](#comandos)

## Kubernetes

- caso um container esteja com um comportamento inesperado, ele precisa ser encerrado e outro precisa ser criado no lugar
- conforme a demanda na aplicacao for aumentando, precisa replicar o container e criar varias copias p distribuir as cargas de requisicoes entre eles
- nao da pra parar tudo porque tem uma nova versao que vai sair, tem que atualizar com tudo no ar, sem esperar uma janela de deploy

- balanceamento de carga: distribui as cargas entre as replicas conforme a demanda
- service discovery: cataloga o endereco pra onde cada processo foi enviado, o que evita ficar procurando onde esta os processos.
- self healing: verifica o estado do container, caso algum nao esteja saudavel, ele restarta o container pra tornar ele saudavel de novo.
- o kubernetes nao derruba a aplicacao pra poder atualizar, ele fornece estrategia de atualizacao quando a aplicacao ainda esta no ar. Favorece aplicações que precisa de alta disponibilidade.

- Cluster: conjuntos de máquinas
   - Kubernetes Control Pane: gerencia os nodes e orquestra todo o cluster
      - Kube Api Server: recebe toda a comunicação com o cluster. Quando executa um kubectl, a comunicação é feita com ele.
      - ETCD: banco de chave e valor que armazena todos os dados do kubernetes, seu acesso é só pelo Kube Api Server.
      - Kube Scheduler: organiza onde cada processo vai ser executado. Ele analisa as especificações, verifica quais nodes podem atender a demanda e define aonde vai ser executado.
      - Kube Controller Manager: executa e gerencia todos os controladores do kubernetes. Administra as tomadas de decisão do kubernetes como autenticação, autorização e admissão.
   - Kubernetes Nodes: executa os containers das aplicações e que executa toda a carga.
      - Kubelet: agente de inspeção do node. Ele é reponsavel por garantir a execução dos containers e interagir com o Kube Api Server pra reportar o status.
      - Kube Proxy: comunicação de redes com o cluster
      - Container Runtime (CRI): as especificações necessarias pra que o container runtime seja usado dentro do kubernetes pra executar os containers.

- Labels e selectors: pra um pod, ou outro objeto, consiga interagir com outro, precisa selecionar e pra isso existe os labels e selectors. o label marca o objeto e atraves do select que seleciona os objetos

- Pod: onde os containers são executados. Pode ter mais de 1 container dentro do pod, compartilhando o mesmo ip e filesystem, mas não é o ideal usar dessa forma e sim pra cada pod colocar um pedaço da aplicação pra quando a demanda aumentar, fica facil do kubernetes escalar a aplicação.
- ReplicaSet: controlador que gerencia os pods no cluster kubernetes. ele que permite que haja disponibilidade e resiliencia. se um pod estiver ruim, o replicaset derruba ele e sobe um outro, assim garantido resiliencia/confiabilidade. garante que o numero de replicas definido nele seja igual ao numero de replicas em execução.
- Deployment: faz o gerenciamento de versoes dos pods e replicasets. quando ha uma nova versão, ele automaticamente derruba os pods de forma progressiva e inicia de novo. se tiver problema na nova versao do replicaset, da pra fazer rollback e voltar a versao antiga.
- Service: meio de comunicacao pros pods, faz o balanceamento de carga. 
   - ClusterIP: gera comunicacao interna. uma api precisa acessar um banco de dados, so acessa dentro do kubernetes.
   - NodePort: utiliza uma porta do cluster kubernetes que atraves dele da o acesso ao service q ira levar ao pod. portas entre 30000 e 32767.
   - LoadBalancer: cria um load balancer na frente do service, gerando um ip publico e a partir dele, acessa o service.

## Comandos

- `kind create cluster --name <name-cluster>`
- `kind create cluster --name meucluster`
- `kind create cluster --name meucluster --config cluster.yaml`
- `kind get clusters`
- `kubectl cluster-info --context kind-meucluster`
- `kind delete cluster --name meucluster`

- `kubectl get nodes`
- `kubectl delete all --all`: deleta todos os objetos existentes
- `kubectl delete -f k8s/deployment.yaml`: deleta todos os objetos criados pelo arquivo
- `kubectl get all`: mostra todos os objetos

- `kubectl api-resources`: lista os objetos e grupos de api que da pra usar

- `kubectl create -f meupod.yaml`: aplica o objeto, manifesto no kubernetes
- `kubectl get pods`: lista todos os pods
- `kubectl describe pod <name-pod>`: detalhes sobre o pod
- `kubectl port-forward pod/meupod 8080:80`: redireciona a porta local pro do container
- `kubectl delete pod meupod`
- `kubectl get pods -l <chave>=<valor>`: pega o pod pelo label
- `kubectl get pods -l app=web`

- `kubectl apply -f meureplicaset.yaml`: cria os objetos que estão definidos no arquivo
- `kubectl get replicaset`
- `kubectl describe repicaset meureplicaset`
- `kubectl delete meureplicaset.yaml`

- `kubectl apply -f meudeployment.yaml`
- `kubectl get meudeployment`
- `kubectl describe repicaset meudeployment`
- `kubectl rollout history deployment meudeployment`: mostra as versão do deployment
- `kubectl rollout undo deployment meudeployment`: volta ao deployment antigo

- `watch 'kubectl get all'`: atualiza em tempo real o comando
