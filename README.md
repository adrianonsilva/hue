# Hue

- [1. Descrição](#link1)
- [2. Instalação](#link2)
- [3. Configuração](#link3)
- [4. Hue Interface](#link4)
- [5. Links](#link5)

<a id="link1"></a>
## 1. Descrição

Hue é uma Web Interface para acesso aos componentes do ecossistema Hadoop.

Permite realizar operações através da interface web, permitindo  o acesso aos componentes do cluster, sem necessidade de acessar cada cliente, nem executar comandos, aumentando a produtividade do desenvolvedor.

O aplicativos no Hue, como os editores do Hive e do Pig, dispensam a necessidade de fazer login no cluster para executar scripts interativamente usando o shell de cada aplicativo.

O Hue está presente nas instalações Cloudera e nos serviços Hadoop dos provedores de nuvem: Amazon AWS, Google Cloud Platform, and Microsoft Azure.

Features Presentes</br>
- HDFS File Browser
- Browser and Job Designer
- Editor for Hive Query
- Editor for Pig Query
- Hadoop Shell Access
- Workflows can access Oozie Interface
- Editor for Hbase

![Screenshot](/images/h00.jpg)

<a id="link2"></a>
## 2. Instalação

A instalação do Hue será realizada usando a sequinte arquitetura:

Cluster Hadoop (1 master  e 2 slaves)

* master
	* Hadoop, Yarn, Hue

* slave3
	* Hdfs, Hbase

* slave5
	* Hdfs, Hive, MySql

- Ubuntu 20.10
- Hadoop 2.6.4
- Hive 2.3.7 
- MySql 8.0.21
- Hbase 1.2.0
- Python 3.8.6

![Screenshot](/images/h01.jpg)

![Screenshot](/images/h02.jpg)

![Screenshot](/images/h03.jpg)

![Screenshot](/images/h04.jpg)

![Screenshot](/images/h05.jpg)

![Screenshot](/images/h06.jpg)

## - Instalar as dependências
```sudo apt-get update```

```sudo apt-get install git ant gcc g++ libffi-dev libkrb5-dev libmysqlclient-dev libsasl2-dev libsasl2-modules-gssapi-mit libsqlite3-dev libssl-dev libxml2-dev libxslt-dev make maven libldap2-dev python-dev python-setuptools libgmp3-dev npn python3.8-dev python3-distutils```

## - download do código fonte
```git clone https://github.com/cloudera/hue.git```

## - Python version
Como valor usar a versão 3.8, é necessário definir a versão do ambiente</br>
```export PYTHON_VER=python3.8```

## - Compilar 
```cd /home/hduser/downloads/hue```</br>
```make apps```

![Screenshot](/images/h07.jpg)

![Screenshot](/images/h08.jpg)

![Screenshot](/images/h09.jpg)

![Screenshot](/images/h10.jpg)

## - Start serviço
```cd hue```</br>
```build/env/bin/hue runserver 0.0.0.0:8000```</br>

![Screenshot](/images/h11.jpg)

![Screenshot](/images/h12.jpg)

Abrir o browser no endereço</br>
http://master:8000/</br>
usar o usuário e senha padrão</br>
user = hue</br>
password = hue</br>

![Screenshot](/images/h13.jpg)

![Screenshot](/images/h14.jpg)

Em seguida vamos criar o usuário hduser, que é o usuário padrão criado para os serviços do cluster.

![Screenshot](/images/h14a.jpg)

![Screenshot](/images/h14b.jpg)

<a id="link3"></a>
## 3. Configuração

## - Configuração dos conectores
Após a verificação que o serviço está funcionando, precisamos configurar as conexões para os componentes do cluster.

- Editar arquivo de configuração</br>
```nano desktop/conf/pseudo-distributed.ini```

![Screenshot](/images/h15.jpg)

## - Hive
O Hive é um data warehouse que permite executar comando SQL em cima do HDFS.

Localizar a sessão beeswax do arquivo hue.ini e configure as tags. Por razões práticas, serão realizadas as configurações mínimas, em um ambiente de produção entretanto devem ser consideradas todas as opções necessárias para garantir a segurança do ambiente.

- editar o arquivo pseudo-distributed.ini e acrescentar:

[beeswax]</br>
hive_server_host=slave5</br>
hive_server_port=10000</br>
thrift_version=7</br>

![Screenshot](/images/h16.jpg)

- editar o arquivo /usr/local/hive/conf/hive-site.xml, no node em que o hove está instalado e acrescentar:

![Screenshot](/images/h17.jpg)

- editar o arquivo /usr/local/hadoop/etc/hadoop/core-site.xml no namenode (master)

![Screenshot](/images/h18.jpg)

## - HDFS

O HDFS é o sistema distribuído de arquivos onde os dados são amazenados nos datanode.

- editar o arquivo pseudo-distributed.ini e acrescentar:

webhdfs_url=http://master:50070/webhdfs/v1/

![Screenshot](/images/h19.jpg)

- editar o arquivo /usr/local/hadoop/etc/hadoop/core-site.xml no namenode (master)

![Screenshot](/images/h20.jpg)


- editar o arquivo /usr/local/hadoop/etc/hadoop/hdfs-site.xml no namenode (master)

![Screenshot](/images/h21.jpg)


- editar o arquivo  /usr/local/hadoop/etc/hadoop/httpfs-site.xml

![Screenshot](/images/h22.jpg)

## - Hbase

O Apache HBase é um banco de dados NoSQL colunar que permite operações de leitura e gravação em grandes datasets.

- editar o arquivo pseudo-distributed.ini e acrescentar:</br>
```[hbase]```</br>
```hbase_clusters=(Cluster|slave3:9090)```</br>

![Screenshot](/images/hbase01.jpg)


<a id="link4"></a>
## 4. Hue Interface

## - Start dos serviços

- Hadoop</br>
```start-dfs.sh```</br>
```start-yarn.sh```</br>

![Screenshot](/images/h23.jpg)

- Hive</br>
```hive --service metastore```</br>
```hive --service hiveserver2```</br>

![Screenshot](/images/h24.jpg)

- Hbase</br>
```hbase-daemon.sh start thrift -p 9090 --infoport 9095```</br>
```start-hbase.sh```</br>
```hbase shell```</br>

![Screenshot](/images/hbase02.jpg)

![Screenshot](/images/hbase03.jpg)

![Screenshot](/images/hbase04.jpg)

![Screenshot](/images/hbase05.jpg)

![Screenshot](/images/hbase06.jpg)

- Hue</br>
```build/env/bin/hue runserver 0.0.0.0:8000```</br>

![Screenshot](/images/h25.jpg)

## - Hue Interface e clientes (shell)

- Hadoop</br>

Listar os arquivos do HDFS</br>

![Screenshot](/images/h26.jpg)

![Screenshot](/images/h27.jpg)

Interface Hue</br>

![Screenshot](/images/h28.jpg)

- Hive</br>

Lista os databases e select</br>

![Screenshot](/images/h29.jpg)

Interface Hue</br>

![Screenshot](/images/h30.jpg)

![Screenshot](/images/h31.jpg)

![Screenshot](/images/h32.jpg)

- Hbase</br>

Scan na tabela employee</br>

![Screenshot](/images/hbase07.jpg)

![Screenshot](/images/hbase08.jpg)

Interface Hue</br>

![Screenshot](/images/hbase09.jpg)

![Screenshot](/images/hbase10.jpg)

Outros conectores a serem configurados, Jobs, Pig, etc, dependendo da configuração do cluster.

<a id="link5"></a>
## 5. Links

Hue</br>
https://gethue.com/

Documentação</br>
https://docs.gethue.com/

Banco colunar</br>
https://aws.amazon.com/pt/nosql/columnar/