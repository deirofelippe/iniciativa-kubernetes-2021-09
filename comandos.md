# Iniciativa Kubernetes 2021/09 (Fabrício Veronez)

## COMANDOS BÁSICOS

- `docker container run <imagem>`: 
- `docker container run --name <nome> <imagem>`:
	- `docker container run --name meucontainer hello-world`
- `docker container la -a`: 
- `docker container rm <container-id-ou-name>`:
- `docker container run --name meucontainer --rm hello-world`: --rm remove o container quando ele é fechado
- `docker container run -it ubuntu /bin/bash`: -t habilita tty, -i modo iterativo pra usar o terminal
- `docker container run -d nginx`: executa em modo deamon, em segundo plano, sem prender o terminal
- `docker container run -d -p <porta-local>:<porta-container> <imagem>`: redireciona a porta local para a porta do container
	- `docker container run -d -p 8080:80 nginx`
rm -f: quando esta em execucao, tem q usar a remocao forçada
- `docker container run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=mongouser -e MONGO_INITDB_ROOT_PASSWORD=mongopwd mongo`: -e passa as variaveis de ambiente, 

docker image ls: lista as imagens
docker image prune: elimina as images que nao tem mais referencia
docker image rm: remove a imagem
docker image inspect: dados da imagem
docker image history: dados de criacao da imagem

## TROUBLESHOOTING

- `docker container inspect <id-ou-name>`: informacoes do container (estado do container, elementos de rede, volumes)
- `docker container exec -it <id-ou-name>`: abre o terminal do container que ja esta em execucao
- `docker stop <id-ou-name>`
- `docker container start <id-ou-name>`
- `docker container logs <id-ou-name>`: consegue ver o stdout do container
- `docker container -n 5 <id-ou-name>`: mostra as ultimas 5 linhas do log
- `docker container -f <id-ou-name>`: -f follow, acompanha em tempo real o log
- `docker container -t`: mostra as horas de cada log

## CRIAR IMAGEM

### Má prática -> usar o commit

**Commit é trabalhoso e não é versionavel**

docker commit <container-id> <nome-desejado>

### Boa pŕatica -> usar Dockerfile

./Dockerfile

```Dockerfile
# a partir de qual imagem va ser criada a atual
FROM ubuntu 

# executa a instrucao
# o ideal é usar os comandos concatenado com '&&' em caso de atualizacao de pacotes ou repositorios, pois caso tenha feito cache do 'apt-get update', alguns pacotes podem ficar desatualizados usando repositorios antigos
RUN apt-get update && apt-get install curl --yes && apt-get install vim --yes
```

-t <nome-container>: 
`docker image build -t docker-curl-file .`:

## Dockerfile

[](./img-1.png)

