# Instalação e Configuração Apache Nifi

- Requisitos

***Ubuntu 20.04 LTS***<br>
***Java JDK 1.8***<br>
***Apache Nifi 1.16.3***<br>

## Criando usuário para instalação e configuração

- Criando usuário ***‘nifi’***. (crie com o root)

```bash
su
adduser nifi
```

- Acesse o arquivo ***‘/etc/sudoers’*** e adicione o novo usuário com as mesmas permissões de superusuário.

```bash
su
nano /etc/sudoers
```

- Adicione o seguinte texto:

***nifi    ALL=(ALL:ALL) ALL***

Saia e salve.

- Acessando com o novo usuário.

```bash
su - nifi
```

# Instalação Apache Nifi

>***Observação:*** Inicie a instalação com usuário Nifi.

- Atualização do ambiente:

```bash
su - nifi
sudo apt update && apt upgrade -y
```

- Instalação do JAVA 1.8:

```bash
sudo apt install openjdk-8-jdk -y
```

- Verificar e capturar o diretório do JAVA instalado:

```bash
update-alternatives --config java
```

>***Observação:*** Use o local ***'/usr/lib/jvm/java-8-openjdk-amd64'*** para variável de ambiente do Java.

- Adicione a variável ***JAVA_HOME*** nos arquivos:

*- ‘.bashrc’ (Execute o ‘source .bashrc’ comando depois de sair da edição)*

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

Salve e saia do arquivo.

>***Observação:*** *‘/opt/nifi/bin/nifi-env.sh’* *(Edite esse arquivo depois que descompactar os arquivos de instalação do Nifi)*

- Verificar se a variável ***JAVA_HOME*** e o ***JAVA*** foram instalados com sucesso:

```bash
env | grep JAVA_HOME
```

***Ou***

```bash
java -version
```

- Baixando os arquivos da instalação

```bash
su - hifi
cd /opt/
sudo wget https://archive.apache.org/dist/nifi/1.16.3/nifi-1.16.3-bin.tar.gz
```

>***Observação:*** Se tiver desatualizado pegue um novo link em: *https://archive.apache.org/dist/nifi/*

- Preparando para instalação

```bash
sudo tar -zxvf nifi-1.16.3-bin.tar.gz (descompactando os arquivos)
```
```bash
sudo mv nifi-1.16.3/ nifi (renomeando a pasta para nifi) 
```
```bash
sudo chown -R nifi:nifi nifi/ (mudando a propriedade da pasta nifi para o usuário nifi)
```
```bash
cd /opt/nifi
```
```bash
nano nifi/bin/nifi-env.sh (adicione a variável ‘export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64’ as variáveis do nifi)
```
```bash
sudo bin/nifi.sh install
```

- Configurando o 1º acesso ao Nifi

Acesse o arquivo ***‘/opt/nifi/conf/nifi.properties’*** e altere as seguintes variáveis conforme a seguir:

```bash
nano /opt/nifi/conf/nifi.properties
```

***nifi.web.https.host=0.0.0.0***<br>
***nifi.web.https.port=8443***

- Ativando o serviço do Nifi

```bash
cd ~
sudo service nifi start (start|stop|restart|status)
```

```bash
service nifi status
```

![Apache Nifi](https://drive.google.com/uc?export=view&id=1Di5m_rF3wqrJzpv6qG7n5fAtyOIzBeJj)

Acesse via: https://127.0.0.1:8443/nifi/ 

- Verificando o arquivo de log do Nifi

```bash
tail /opt/nifi/logs/nifi-app.log -f
```

## Primeiro acesso ao Apache Nifi

![Apache Nifi](https://drive.google.com/uc?export=view&id=1kP83DoMyTt1BQsPSaH8D4bjd-JdvMriG)

>***Observação:*** Se você for direcionado para tela de login, as credenciais de acesso estão no arquivo de log do nifi.

```bash
nano  /opt/nifi/logs/nifi-app.log
```

Pesquise por: ***“Generated Username”*** com o comando ***“CTRL + w”***, com isso você vai localizar o usuário e senha do Admin Nifi.

### Exemplo:

```bash
Generated Username [***********************************************]
Generated Password [***********************************************]
```

### Pronto! Apache Nifi em funcionamento

![Apache Nifi](https://drive.google.com/uc?export=view&id=14jYwCSvjTskyEemHxVbsDE0eIRpOcrE9)