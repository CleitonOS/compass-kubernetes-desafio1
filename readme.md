
<p align="center">
  <a href="" rel="noopener">
 <img max-width=400px height=100px src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/45/Logo_CompassoUOL_Positivo.png/1200px-Logo_CompassoUOL_Positivo.png" alt="Project logo"></a>
</p>

<h1 align="center">Instalando o minikube e as ferramentas necessárias para utilizar o Kubernetes no Oracle Linux</h1> 
<p align="center"><i>Esse será um passo a passo da instalação do Minikube e kubectl, utilizando uma VM do Oracle Linux</i></p>

## 📝 Tabela de conteúdos
- [Instalando tudo que é necessário (Passo 1)](#step1)
- [Instalando kubectl (Passo 2)](#step2)
- [ Instalando o minikube (Passo 3)](#step3)
- [Referências](#documentation)

## 📑 Requisitos
Esses são os requisitos de uso do minikube:
- 2 CPUs ou mais
- 2GB de memória livre
- 20GB de disco livre
- Conexão á internet
- Container ou Gerenciador de 'Virtual Machine', como: Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, ou VMware Fusion/Workstation

## 🖥️ Instalando tudo que é necessário (Passo 1)<a name = "step1"></a>

Diferente do que faço em meus repositórios, destinarei essa seção para download de outros utilitários necessários para o bom funcionamento do minikube.

- Precisaremos de um driver para executar o minikube, tal driver permite que o minikube crie e gerencie máquinas virtuais que executam os clusters Kubernetes locais. 

- O driver a ser utilizado, pode ser: VirtualBox, KVM, Docker, VMware etc. (Fica a sua escolha)

- Nesse repositório vou utilizar o docker como driver.

#### Instalando o Docker:

- Instalando a partir de um repositório RPM
- Para começar, instale o pacote **yum-utils** (que fornece o **yum-config-manager** como utilitário) e configure o repositório.

    ```
    $ sudo yum install -y yum-utils
    $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    ```

#### Instalando o Docker Engine:

- Para instalar a versão mais recente utilize esse comando:

    ```
    $ sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin --allowerasing --nobest
    ```

    - Observações: Se for solicitado a aceitar a chave GPG, verifique se a impressão digital corresponde "060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35" e, em caso afirmativo, aceite-a.
    
    - Além disso, este comando instala o Docker, mas não inicia o Docker. Ele também cria um grupo "docker", mas não adiciona nenhum usuário ao grupo por padrão.

#### ⚙️ Inicializando o Docker <a name = "step2"></a>

- Inicie o Docker.

    ```
    $ sudo systemctl start docker
    ```


## 🔽 Instalando `kubectl` (Passo 2)<a name = "step2"></a>

O `kubectl` é a ferramenta de linha de comando do Kubernetes que permite interagir com o cluster.

1. Para começar, baixe a última versão estável com esse comando (Para arquitetura x86-64).

    ```
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    ```

2. Valide o arquivo binário que foi baixado.

- Baixe o 'kubectl checksum file'
    ```
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
    ```

- Vamos validar o arquivo binário agora:

    ```
    echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
    ```

- Se foi validado com sucesso, a resposta será essa:
    ```
    kubectl: OK
    ```

- Caso a verificação falhe, essa provavelmente será a resposta:
    ```
    kubectl: FAILED
    sha256sum: WARNING: 1 computed checksum did NOT match
    ```

    >Nota: Baixe a mesma versão do binário (arquivo do kubectl) e o checksum.

3. Instalando o `kubectl`

    ```
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    ```

4. Teste para garantir que a versão que você instalou está atualizada:

    ```
    kubectl version --client
    ```

- Feito isso, concluímos essa etapa.

## 🔽 Instalando o minikube (Passo 3)<a name = "step3"></a>
Instalando a versão para o Linux (x86-64)

1. Baixando o minikube

    ```
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    ```

2. Instalando o minikube

    ```
    sudo install minikube-linux-amd64 /usr/local/bin/minikube
    ```

    - Caso queira instalar outro tipo de versão, acesse o site da documentação do [minikube](https://minikube.sigs.k8s.io/docs/start/).

- Para prosseguir, será necessário ter instalado o Docker, como mencionado na seção de instalação acima - [Instalando tudo que é necessário (Passo 1)](#step1).

3. Inicializando seu cluster com o minikube

- Com o docker baixado e instalado, automaticamente ele irá inicializar com o Docker como driver.

    ```
    minikube start
    ```

- Caso Tenha problemas para iniciar com o Docker, utilize esse comando:

    ```
    minikube start --driver=docker
    ```

    - Para problemas relacionados as permissões do docker (Problema comum):

        ```
        sudo usermod -aG docker $USER && newgrp docker
        ```
    - Depois de executar esse comando, execute o comando anterior.

- Interaja com o seu cluster e veja os componentes rodando:

    ```
    kubectl get po -A
    ```

    <img src="./Screenshots/VerificandoComponentes-k8s.png" min-width="80%">
    
- ### Feito isso a instalação foi realizada com sucesso!

## Referências utilizadas:<a name="documentation"></a>
- [Kubernetes Docs - Install Kubectl Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

- [Minikube Docs - Install Minikube](https://minikube.sigs.k8s.io/docs/start/)
