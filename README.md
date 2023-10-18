# Instalação e Configuração Apache Nifi

### Requisitos

- Ubuntu 20.04
- Apache Nifi 1.22.0 ou +
- Java OpenJDK 1.8.0_382 +
- Nano 4.8 ou +
- Wget 1.20.3 ou +
- Zip/Unzip 3.0 ou +

### Criando usuário para instalação e configuração

- Criando usuário ***nifi***. (crie com o root)

```bash
su
```

```bash
adduser nifi
```

- Adicionando o novo usuário ao grupo de superusuários (sudo).

```bash
usermod -aG sudo nifi
```

- Acessando com o novo usuário.

```bash
su - nifi
```

### Atualização do ambiente

```bash
sudo apt update && sudo apt upgrade -y
```

### Instalação do JAVA 1.8:

```bash
sudo apt install openjdk-8-jdk -y
```

- Verifique em qual diretório o JAVA está instalado:

```bash
update-alternatives --config java
```

>***Observação:*** Use o local ***/usr/lib/jvm/java-8-openjdk-amd64*** para variável de ambiente do Java.

- Adicione a variável ***JAVA_HOME*** no arquivo ***.bashrc***:

```bash
nano ~/.bashrc
```

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

Salve e saía do arquivo.

- Atualize o contexto do ambiente executando o seguinte comando:

```bash
source .bashrc
```

- Verifique se a variável ***JAVA_HOME*** e o ***JAVA*** foram configurados/instalados com sucesso:

```bash
env | grep -Ei java_home
```

***E/Ou***

```bash
java -version
```

### Baixando o Apache Nifi versão .zip

```bash
sudo wget https://archive.apache.org/dist/nifi/1.22.0/nifi-1.22.0-bin.zip
```

### Descompactando o arquivo zipado

```bash
unzip nifi-1.22.0-bin.zip
```

### Movendo e já renomeando o diretório descompactado para o /opt/

```bash
sudo mv nifi-1.22.0 /opt/nifi
```

### Mudando a propriedade do diretório da instalação para o usuário nifi

```bash
sudo chown -R nifi:nifi /opt/nifi
```

### Adicione a variável ***JAVA_HOME*** as variáveis do Apache Nifi

Adicione ao arquivo ```nifi-env.sh``` a variável ***JAVA_HOME***.

```bash
nano /opt/nifi/bin/nifi-env.sh
```

```text
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

### Executando a instalação

```bash
sudo /opt/nifi/bin/nifi.sh install
```

### Configurando o 1º acesso ao Nifi

Acesse o arquivo ***/opt/nifi/conf/nifi.properties*** e altere os seguintes parâmetros conforme a seguir:

```bash
nano /opt/nifi/conf/nifi.properties
```

```text
...
nifi.web.https.host=0.0.0.0
nifi.web.https.port=8443
...
```

### Ativando o serviço do Nifi

Opções de serviços  (start|stop|restart|status)

```bash
cd ~
```

```bash
sudo service nifi start
```

```bash
service nifi status
```

Acesse via: [https://localhost:8443/nifi/](https://localhost:8443/nifi/) 

### Primeiro acesso ao Apache Nifi

Acesse o navegador e digite a seguinte URL ou simplesmente clique em: [https://localhost:8443/nifi/](https://localhost:8443/nifi/).

Se você for direcionado para tela de login, as credenciais de acesso estão no arquivo de log do nifi.

```bash
cat /opt/nifi/logs/nifi-app.log | grep -Ei generated
```

```text
...
Generated Username [***********************************************]
Generated Password [***********************************************]
...
```

Em posse do usuário e senha é só acessar o apache Nifi.

***Pronto! Apache Nifi em funcionamento...***

### Configurando um novo usuário e senha para o Apache Nifi

- Crie o arquivo ***create-user.sh*** em ***/opt/nifi/bin***  e adicione o seguinte conteúdo:

```text
#!/bin/bash

# Criando uma senha aleatória para o novo usuário
PASSWD=$(cat /dev/urandom | tr -dc 'A-Za-z0-9' | head -c 32)

# Definindo o usuário e senha (O usuário que vai ser definido aqui vai ser o da sessão.)
./bin/nifi.sh set-single-user-credentials $USER $PASSWD

# Salvando usuário e senha do Nifi
echo "Usuário: $USER\nSenha: $PASSWD" > novas-credenciais-nifi.txt
echo "Usuário e senha Apache Nifi criados com Sucesso!!!"
```

- Dê permissão de execução para o arquivo

```bash
chmod +x create_user.sh
```

- Para criar usuário e senha execute o seguinte comando

```bash
sh ./create-user.sh
```

- Se tudo dê certo, será criado o arquivo ***novas-credenciais-nifi.txt*** com as novas credenciais de acesso ao Nifi.

```bash
cat novas-credenciais-nifi.txt
```

```
...
Usuário: nifi
Senha: ******************************************************
...
```

### Testando o novo usuário Nifi

Acesse o navegador e digite a seguinte URL ou simplesmente clique em: [https://localhost:8443/](https://localhost:8443/) (se estiver logado com o outro usuário, é só clicar em ```LOGOUT``` no canto superior direito.) e em seguida digite o novo usuário e senha, agora é só utilizar o Apache Nifi agora!!!

### Referência

Docs Nifi, **Apache Nifi**. Disponível em: <https://nifi.apache.org/docs/nifi-docs/>. Acesso em: 17 de out. de 2023.

Single User Access and HTTPS in Apache NiFi, **ExceptionFactory**. Disponível em: <https://exceptionfactory.com/posts/2021/07/21/single-user-access-and-https-in-apache-nifi/>. Acesso em: 17 de out. de 2023
