# RoteiroPratico: Orquestrando containers usando Kubernetes
Roteiro Prático Desenvolvido no âmbito da matéria de Engenharia de Software I do CEFET-MG

# Grupo:
### GUSTAVO CAMPOS PÁDUA (20203001143)
### JORGE ALIOMAR TROCOLI ABDON DANTAS (20213010469)
### SAMUEL PEDRO FERNANDES AMORIM (20213010502)

# Visão Geral
Neste guia, vamos demonstrar como orquestrar containers usando Kubernetes. Vamos criar um cluster Kubernetes local com Minikube e implantar uma aplicação simples composta por um frontend e um backend.

# Introdução
## Containers (Docker):
Antes de começar a orquestrar containers com Kubernetes, é importante entender os conceitos fundamentais de containers, especialmente usando Docker, que é uma das tecnologias de containers mais populares.
Um container é uma unidade leve e portátil que empacota uma aplicação junto com todas as suas dependências (bibliotecas, configurações, etc.) para que ela possa ser executada de forma consistente em qualquer ambiente, seja em desenvolvimento, teste ou produção. Ao contrário de máquinas virtuais (VMs), os containers compartilham o mesmo sistema operacional, tornando-os mais eficientes e rápidos.
Docker é uma plataforma que facilita a criação, execução e gerenciamento de containers. Ele fornece ferramentas e um formato padrão para empacotar software em containers, permitindo que desenvolvedores e equipes de operações trabalhem de maneira integrada.
## Conceitos Essenciais do Docker:
* **Imagem (Image):** Uma imagem é um modelo imutável que define o que será executado dentro de um container. Ela contém tudo o que sua aplicação precisa: código, bibliotecas, variáveis de ambiente, e instruções para a execução. As imagens são criadas a partir de um Dockerfile e podem ser armazenadas em registries (como o Docker Hub).
* **Container:** Um container é uma instância de uma imagem em execução. Ele é criado a partir de uma imagem e pode ser executado, parado e removido facilmente. Um container pode ser considerado como uma "mini-VP" que roda uma aplicação isolada do sistema anfitrião.
* **Dockerfile:** Um Dockerfile é um arquivo de texto que contém as instruções necessárias para construir uma imagem Docker. Ele define a base da imagem, as dependências da aplicação, as portas expostas, e o comando que deve ser executado ao iniciar o container.
* **Docker CLI (Interface de Linha de Comando):** A CLI do Docker é usada para interagir com o Docker, desde a criação de imagens até o gerenciamento de containers.
    * _Comandos básicos:_
      * **docker build:** Cria uma imagem a partir de um Dockerfile.
      * **docker run:** Executa um container a partir de uma imagem.
      * **docker ps:** Lista os containers em execução.
      * **docker stop:** Para um container em execução.
      * **docker pull:** Baixa uma imagem de um registry (por exemplo, Docker Hub).
      * **docker push:** Envia uma imagem para um registry.
* **Volumes:** Volumes são usados para persistir dados gerados ou usados pelos containers. Ao contrário do sistema de arquivos temporário dos containers, os volumes permitem que os dados sejam mantidos mesmo que o container seja removido.
* **Network:** O Docker também gerencia redes de containers, permitindo a comunicação entre diferentes containers e com o mundo externo. Ele oferece diferentes drivers de rede (bridge, host, none, etc.) para atender a necessidades específicas.

Por Que Entender Docker é Importante para Kubernetes?
Kubernetes é uma solução de orquestração de containers, e a maioria dos clusters de Kubernetes usa Docker como o runtime padrão de containers. Ao entender os conceitos básicos de Docker, você estará mais preparado para trabalhar com Kubernetes, já que muitos dos conceitos (como containers, imagens, volumes, e redes) são utilizados em ambas as tecnologias.

## O que é Kubernetes:
Kubernetes (também conhecido como K8s) é uma plataforma open-source projetada para automatizar a implantação, escalonamento e gerenciamento de aplicações containerizadas. Inicialmente desenvolvido pelo Google e atualmente mantido pela Cloud Native Computing Foundation (CNCF), Kubernetes permite que você gerencie clusters de containers em uma infraestrutura distribuída. Ele organiza e coordena como os containers são executados em um ambiente de produção, simplificando operações complexas como alta disponibilidade, escalabilidade e recuperação de falhas.
## Principais componentes do Kubernetes:
* **Pod:** A menor unidade no Kubernetes, que agrupa um ou mais containers e compartilha recursos como rede e armazenamento.
* **Node:** Uma máquina virtual ou física que executa Pods.
* **Cluster:** Conjunto de Nodes gerenciados pelo plano de controle do Kubernetes.
* **Service:** Um recurso que define como expor um grupo de Pods como um serviço de rede.
  ![image](https://github.com/user-attachments/assets/ad3a160f-4cf3-4cd7-853b-8a2f25988332)

## Vantagens de usar Kubernetes para orquestração de containers.
O Kubernetes é uma das soluções mais populares e robustas para gerenciar aplicações baseadas em containers em escala, proporcionando maior controle, automação e confiabilidade. Dentre suas vantagens, tem-se:
* **Automatização e Escalabilidade:** Kubernetes permite que você escale automaticamente suas aplicações, ajustando o número de réplicas de containers conforme a carga de trabalho aumenta ou diminui. Suporta escalonamento horizontal (adicionar mais réplicas de Pods) com facilidade.
* **Autocorreção e Alta Disponibilidade:** Kubernetes detecta falhas de containers e automaticamente reinicia, substitui ou redistribui os Pods conforme necessário, garantindo que suas aplicações permaneçam disponíveis. Utiliza balanceamento de carga para distribuir o tráfego de forma eficiente.
* **Gerenciamento Simplificado de Configurações:** Permite o gerenciamento centralizado de configurações sensíveis e variáveis de ambiente usando ConfigMaps e Secrets. Facilita a atualização de aplicações com "rolling updates" sem tempo de inatividade perceptível.
* **Orquestração Multi-Plataforma:** Kubernetes é independente de plataforma, podendo ser executado em ambientes on-premises, em várias nuvens (multicloud) ou em arquiteturas híbridas. Suporta integrações com diferentes soluções de armazenamento e rede.
* **Implantação Consistente e Portável:** As aplicações containerizadas são facilmente portáveis entre diferentes ambientes, desde desenvolvimento até produção. Kubernetes oferece uma forma padronizada de gerenciar e implantar aplicações, independentemente de onde estejam rodando.
* **Eficiência no Uso de Recursos:** Kubernetes distribui e otimiza automaticamente os recursos entre os containers, garantindo o uso eficiente da infraestrutura subjacente.
* **Gerenciamento de Tráfego e Networking:** Suporta roteamento avançado de tráfego, com controle granular sobre o tráfego entre serviços usando Ingress e políticas de rede. Permite a implementação de balanceamento de carga interno e externo.
* **Comunidade e Ecossistema Vibrante:** Kubernetes possui uma comunidade ativa e um vasto ecossistema de ferramentas e extensões (como Helm, Prometheus, Istio), facilitando a adoção e expansão de sua infraestrutura.


# Pré-requisitos
* **Minikube:** Para criar e gerenciar um cluster Kubernetes local.
* **Kubectl:** Ferramenta de linha de comando para interagir com o cluster Kubernetes.

# Passos
## 1. Configuração do Ambiente
### 1.1. Instalação de Ferramentas
#### Minikube:
- Acesse https://github.com/kubernetes/minikube/releases
- Baixe e instale versão de acordo com seu Sistema Operacinal
#### Kubectl:
- Acesse https://kubernetes.io/docs/tasks/tools/
- Baixe e instale versão de acordo com seu Sistema Operacinal

### 1.2. Inicialização do Minikube

Para iniciar o cluster Kubernetes local, abra um terminal com acesso de administrador (mas não como root) e execute:

```bash
minikube start
```

Se o Minikube não iniciar corretamente, consulte a [página de drivers](https://minikube.sigs.k8s.io/docs/drivers/) para obter ajuda sobre como configurar um gerenciador de contêineres ou máquina virtual compatível.

## 2. Interaja com Seu Cluster

Se você já tem o `kubectl` instalado, pode usá-lo para acessar seu novo cluster:

```bash
kubectl get po -A
```

Como alternativa, o Minikube pode baixar a versão apropriada do `kubectl` para você, permitindo o uso do comando:

```bash
minikube kubectl -- get po -A
```

Para facilitar ainda mais, adicione o seguinte comando à configuração do seu shell (para mais detalhes, consulte a [documentação do kubectl](https://kubernetes.io/docs/reference/kubectl/)):

```bash
alias kubectl="minikube kubectl --"
```

Inicialmente, alguns serviços como o `storage-provisioner` podem não estar em estado `Running`. Isso é normal durante a inicialização do cluster e deve se resolver em breve. Para obter mais informações sobre o estado do seu cluster, o Minikube inclui o Kubernetes Dashboard, que permite explorar facilmente seu ambiente:

```bash
minikube dashboard
```

## 3. Implante Aplicações

### 3.1. Serviço Simples

Crie uma implantação de exemplo e exponha-a na porta 8080:

```bash
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
kubectl expose deployment hello-minikube --type=NodePort --port=8080
```

Pode levar algum tempo, mas sua implantação aparecerá quando você executar:

```bash
kubectl get services hello-minikube
```

A maneira mais fácil de acessar este serviço é deixar o Minikube abrir um navegador da web para você:

```bash
minikube service hello-minikube
```

Como alternativa, use o `kubectl` para encaminhar a porta:

```bash
kubectl port-forward service/hello-minikube 7080:8080
```

Sua aplicação agora está disponível em `http://localhost:7080/`. Experimente alterar o caminho da solicitação e observe as mudanças na saída da aplicação.

### 3.2. LoadBalancer

Para acessar uma implantação LoadBalancer, use o comando `minikube tunnel`. Aqui está um exemplo de implantação:

```bash
kubectl create deployment balanced --image=kicbase/echo-server:1.0
kubectl expose deployment balanced --type=LoadBalancer --port=8080
```

Em outra janela, inicie o túnel para criar um IP roteável para a implantação `balanced`:

```bash
minikube tunnel
```

Para encontrar o IP roteável, execute este comando e examine a coluna `EXTERNAL-IP`:

```bash
kubectl get services balanced
```

Sua implantação agora está disponível em `<EXTERNAL-IP>:8080`.

### 3.3. Ingress

Habilite o complemento de Ingress:

```bash
minikube addons enable ingress
```

O exemplo a seguir cria serviços `echo-server` simples e um objeto Ingress para roteá-los.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: foo-app
  labels:
    app: foo
spec:
  containers:
    - name: foo-app
      image: 'kicbase/echo-server:1.0'
---
kind: Service
apiVersion: v1
metadata:
  name: foo-service
spec:
  selector:
    app: foo
  ports:
    - port: 8080
---
kind: Pod
apiVersion: v1
metadata:
  name: bar-app
  labels:
    app: bar
spec:
  containers:
    - name: bar-app
      image: 'kicbase/echo-server:1.0'
---
kind: Service
apiVersion: v1
metadata:
  name: bar-service
spec:
  selector:
    app: bar
  ports:
    - port: 8080
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
    - http:
        paths:
          - pathType: Prefix
            path: /foo
            backend:
              service:
                name: foo-service
                port:
                  number: 8080
          - pathType: Prefix
            path: /bar
            backend:
              service:
                name: bar-service
                port:
                  number: 8080
```

Aplicar o conteúdo:

```bash
kubectl apply -f https://storage.googleapis.com/minikube-site-examples/ingress-example.yaml
```

Aguarde pelo endereço de ingress:

```bash
kubectl get ingress
```

Para usuários do Docker Desktop:
Para que o ingress funcione, você precisará abrir uma nova janela de terminal e executar `minikube tunnel`. No passo seguinte, use `127.0.0.1` no lugar de `<ip_from_above>`.

Agora, verifique se o ingress funciona:

```bash
curl <ip_from_above>/foo
```

```bash
curl <ip_from_above>/bar
```

## 4. Gerencie Seu Cluster

Pause o Kubernetes sem impactar as aplicações implantadas:

```bash
minikube pause
```

Despausa uma instância pausada:

```bash
minikube unpause
```

Interrompa o cluster:

```bash
minikube stop
```

Altere o limite de memória padrão (requer reinicialização):

```bash
minikube config set memory 9001
```

Navegue pelo catálogo de serviços Kubernetes facilmente instalados:

```bash
minikube addons list
```

Crie um segundo cluster executando uma versão mais antiga do Kubernetes:

```bash
minikube start -p aged --kubernetes-version=v1.16.1
```

Exclua todos os clusters Minikube:

```bash
minikube delete --all
```

