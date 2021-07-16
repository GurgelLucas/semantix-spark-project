# Projeto Final de Spark

NEste projeto foi usado <b>Docker</b> com <b>WSL2</b> para construir a infraestrutura de clusters. Para aplicar o projeto em seu computador local será necessário a instalação do <b>Docker</b> e <b>Docker Compose</b>.

- Docker: https://docs.docker.com/get-docker/
- Docker Compose: https://docs.docker.com/compose/install/
  - SO
    - Windows
      - Docker Desktop (Hyper-V ou WSL2)
      - Docker Toolbox (VirtualBox)
    - Linux
      - Docker Engine
      - Docker Compose
- Baixar conteudo do Cluster

  ```bash
        git clone https://github.com/rodrigo-reboucas/docker-bigdata.git spark
  ```

- Baixar Cluster de Big Data - Parcial
  Arquivo: <u>docker-compose-parcial.yml</u>
- Baixar as imagens

  ```bash
      docker-compose -f docker-compose-parcial.yml pull
  ```

- Listar as imagens

  ```bash
    docker image ls
  ```

- Iniciar todos os serviços
  ```bash
    docker-compose -f docker-compose-parcial.yml up –d
  ```
- Configurar o jar do spark para aceitar o formato parquet em tabelas Hive

  ```bash
      curl -O https://repo1.maven.org/maven2/com/twitter/parquet-hadoop-bundle/1.6.0/parquet-hadoop-bundle-1.6.0.jar
  ```

  ```bash
  docker cp parquet-hadoop-bundle-1.6.0.jar jupyter-spark:/opt/spark/jars
  ```

### Base de Dados

https://mobileapps.saude.gov.br/esus-vepi/files/unAFkcaNDeXajurGB7LChj8SgQYS2ptm/04bd3419b22b9cc5c6efac2c6528100d_HIST_PAINEL_COVIDBR_06jul2021.rar

### Descompactar Dados localmente

- Windows:
  Winrar ou 7zip

- Linux:
  Instalar pacotes unrar, caso não tenha em seu SO, executar o comando:
  ```bash
    sudo apt install unrar
  ```
  Criar uma pasta para descompactar os arquivos,
  ```bash
    mkdir covid
  ```
  Descompactando...
  ```bash
  unrar x 04bd3419b22b9cc5c6efac2c6528100d_HIST_PAINEL_COVIDBR_06jul2021.rar ~/spark/covid
  ```

* No <b>HDFS</b> criar diretório e enviar dados

  Criando diretório

```bash
    docker exec -i namenode hdfs dfs -mkdir /user/datacovid
```

- Enviar os dados para o hdfs
  ```bash
      docker cp covid namenode:/
  ```
- Enviando os dados servidor Hadoop

  ```bash
        docker exec -i namenode hdfs dfs -put covid/HIST_PAINEL_COVIDBR_* /user/datacovid/
  ```