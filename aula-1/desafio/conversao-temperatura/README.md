# conversao-temperatura

## Como usar?

> Base de dados usado: https://github.com/KubeDev/conversao-temperatura

1. vá ate a pasta conversao-temperatura, onde estar o Dockerfile e rode:
   1. `docker image build -t felippedeiro/conversao-temperatura:v1 .`
   1. `docker container run -d -p 8080:8080 felippedeiro/conversao-temperatura:v1`
1. abra o navegador cole a url `localhost:8080`
