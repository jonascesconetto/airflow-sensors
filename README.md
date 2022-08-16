## First Steps com Airflow Sensors
  Este repositório contém alguns arquivos e códigos utilizados durante curso de `Data Pipelines com Apache Airflow `.
### Requisitos
- Compreenção do sistema de Sensoriamento do Airflow.
## 💻 Principais Tecnologias, Softwares e Bibliotecas
- Docker
- Python
- Apache AirFlow (Data Pipeline)
## ⚙ Instalação e configuração de ambiente
### Docker
  - Seguir a [documentação](https://docs.docker.com/engine/install/) oficial
### Container - Apache AirFlow
  - `Observação: `Abra o terminal em um diretório raiz para subir seu container com Airflow, dessa forma não perdemos os arquivos de DAGs gerados caso seja necessário algumas reinstalação ou configuração adicional do container.
  - Crie container com seguinte comando em seu terminal:
    ```bash
    docker run -d -p 8080:8080 -v "$PWD/airflow/dags:/opt/airflow/dags/" \
    --entrypoint=/bin/bash \
    --name airflow apache/airflow:2.1.1-python3.8 \
    -c '(\
    airflow db init && \
    airflow users create --username <nome de usuário> --password <sua senha> --firstname <Seu nome> --lastname <Seu Nome> --role Admin --email <Seu e-mail>); \
    airflow webserver & \
    airflow scheduler\
    '
    ```
  - Verificando o container em execução.
    ```bash
    docker container ls
    ```
  - Verificando os logs do container
    ```bash
    docker container logs airflow
    ```
  - Se nenhum erro aparecer, acesse a interface web do Apache Airflow pelo endereço:
    
    **https://localhost:8080**
### Container - MariaDB
- `Observação: `Assim como na criação do container do Airflow é importante abrir o terminal em um diretório raiz para subir o container.
- Crie o container con o seguinte comando em seu terminal:
  ```bash
  docker run -d --name mariadb_oltp -p "3306:3306" -v "$PWD/data:/home" -e MARIADB_ROOT_PASSWORD=airflow mariadb:latest
  ```
- Verificando o container em execução.
  ```bash
  docker container ls
  ```
- Verificar o mapeamento dos volumes
  ```bash
  docker container exec -it mariadb_oltp bash
  ls /home/
  ```
- Conectar com o MySQL/MariaDB dentro do container
  ```bash
  mysql -u root -pairflow
  ```
- Listar os bancos de dados
    ```bash
  show databases;
  ```
- Instalar e configurar recursos adicionais
  - Acesse o container pelo seguinte comando
    ```bash
    docker container exec -it mariadb_oltp bash
    ```
  - Executar o seguinte comando
    ```bash
    apt-get update && apt-get install vim iputils-ping -y
    ```
#### Carregar dados 
  - Acesse o container com o seguinte comando
    ```bash
    docker container exec -it mariadb_oltp bash
    ```
  - Acessar o diretório `/home/dbs`, e executar o seguinte comando:
    ```bash
    mysql -u root -pairflow < employees.sql
    ```
  - Conectar com o MySQL/MariaDB dentro do container
    ```bash
    mysql -u root -pairflow
    ```
  - Listar os bancos de dados
    ```bash
    show databases;
    ```
  - Executar o script `create-table-sales.sql` presente no diretório `scrips` deste repositório.
#### Configuração Aiflow
- Executar o seguite comando:
  ```bash
  docker inspect <db container>
  ```
- Acessar o painel do Apache Airflow
  - Em Admin
    - Connections
        - Criar uma nova conexão

- Incluir as seguintes credenciais
  ```bash
  Conn Id: mariadb_oltp
  Conn Type: MySQL
  Description: Instancia de conexão do MySQL em ambiente OLTP
  Host: <ip advindo do inspect command>
  Schema: employee
  Login: root
  Password: airflow
  Port: 3306
  ```

## Execução dos testes/laboratórios
  - Limpar a tabela `sales_temp` do banco 'employees'
  - Executar a DAG `sensor_sql`
    - Observar o painel do Airflow
  - Inserir dados na tabela `sales_temp`
    ```sql
    INSERT INTO `sales_temp` (`id`,`data`,`valor`) VALUES (1,"17/06/21",628),(2,"04/07/21",654),(3,"22/03/21",536),(4,"09/02/21",429),(5,"07/06/21",481),(6,"22/01/21",424),(7,"09/05/21",759),(8,"30/01/21",746),(9,"17/07/21",761),(10,"02/02/21",527),(11,"21/04/21",419),(12,"02/01/21",407),(13,"17/07/21",743),(14,"10/07/21",391),(15,"19/02/21",624),(16,"16/06/21",739),(17,"07/02/21",661),(18,"01/06/21",797),(19,"17/04/21",771),(20,"19/05/21",359),(21,"15/06/21",413),(22,"02/05/21",668),(23,"04/03/21",355),(24,"24/06/21",762),(25,"29/06/21",612),(26,"06/07/21",498),(27,"21/07/21",482),(28,"04/01/21",788),(29,"09/07/21",444),(30,"31/01/21",721),(31,"18/02/21",501),(32,"09/04/21",518),(33,"01/06/21",658),(34,"24/04/21",311),(35,"12/03/21",425),(36,"05/03/21",467),(37,"24/01/21",653),(38,"06/05/21",752),(39,"23/02/21",438),(40,"12/07/21",397),(41,"16/05/21",599),(42,"04/03/21",628),(43,"30/04/21",558),(44,"13/04/21",351),(45,"12/07/21",747),(46,"02/02/21",465),(47,"16/03/21",623),(48,"27/06/21",344),(49,"17/07/21",544),(50,"02/02/21",704),(51,"06/01/21",405),(52,"10/04/21",305),(53,"12/04/21",443),(54,"13/02/21",425),(55,"18/04/21",489),(56,"06/04/21",426),(57,"18/05/21",427),(58,"07/06/21",559),(59,"26/07/21",511),(60,"01/04/21",632),(61,"28/03/21",567),(62,"12/01/21",710),(63,"03/05/21",748),(64,"16/05/21",453),(65,"19/01/21",649),(66,"10/06/21",427),(67,"21/03/21",492),(68,"11/03/21",480),(69,"31/03/21",376),(70,"12/01/21",533),(71,"22/04/21",401),(72,"04/05/21",641),(73,"11/07/21",442),(74,"21/07/21",366),(75,"07/06/21",523),(76,"30/04/21",778),(77,"02/02/21",693),(78,"05/05/21",445),(79,"05/07/21",696),(80,"24/01/21",486),(81,"09/07/21",347),(82,"14/01/21",722),(83,"11/03/21",459),(84,"08/02/21",317),(85,"12/02/21",784),(86,"21/07/21",673),(87,"18/02/21",763),(88,"02/02/21",435),(89,"23/04/21",699),(90,"27/01/21",613),(91,"22/01/21",402),(92,"01/07/21",323),(93,"16/02/21",528),(94,"03/03/21",677),(95,"11/04/21",739),(96,"22/06/21",612),(97,"27/03/21",662),(98,"21/02/21",526),(99,"29/03/21",492),(100,"18/04/21",345); 
    ```
    - Observar o painel do Airflow
