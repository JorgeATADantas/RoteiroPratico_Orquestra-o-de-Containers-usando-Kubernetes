# RoteiroPratico_Orquestra-o-de-Containers-usando-Kubernetes
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
* **Git:** Para clonar o repositório.
* **Docker:** Para construir e rodar containers localmente.
* **Minikube:** Para criar e gerenciar um cluster Kubernetes local.
* **Kubectl:** Ferramenta de linha de comando para interagir com o cluster Kubernetes.
* **Node.js e npm:** Necessário para a aplicação de exemplo (Frontend e Backend).

# Passos
## 1. Configuração do Ambiente
### 1.1. Instalação de Ferramentas
#### Docker:
- Acesse https://www.docker.com/ 
- Baixe e instale versão de acordo com seu Sistema Operacinal
#### Minikube:
- Acesse https://github.com/kubernetes/minikube/releases
- Baixe e instale versão de acordo com seu Sistema Operacinal
#### Kubectl:
- Acesse https://kubernetes.io/docs/tasks/tools/
- Baixe e instale versão de acordo com seu Sistema Operacinal
#### Node.js e npm:
- Acesse https://nodejs.org/pt
- Baixe e instale versão de acordo com seu Sistema Operacinal

### 1.2. Inicialização do Minikube

bash
Copiar código
minikube start
2. Configuração do Projeto
2.1. Estrutura do Repositório
Clone o repositório com o exemplo prático:

bash
Copiar código
git clone https://github.com/seu-usuario/exemplo-kubernetes.git
cd exemplo-kubernetes
2.2. Estrutura do Projeto
Dentro do repositório, você deve encontrar a seguinte estrutura:

css
Copiar código
exemplo-kubernetes/
├── frontend/
│   ├── Dockerfile
│   ├── src/
│   └── package.json
├── backend/
│   ├── Dockerfile
│   ├── src/
│   └── package.json
├── kubernetes/
│   ├── frontend-deployment.yaml
│   ├── frontend-service.yaml
│   ├── backend-deployment.yaml
│   └── backend-service.yaml
└── README.md
3. Construção e Execução dos Containers
3.1. Construir Imagens Docker
Navegue para as pastas frontend e backend e construa as imagens:

bash
Copiar código
cd frontend
docker build -t meu-frontend:1.0 .
cd ../backend
docker build -t meu-backend:1.0 .
3.2. Testar Localmente
Você pode testar as imagens localmente para garantir que tudo está funcionando:

bash
Copiar código
docker run -p 3000:3000 meu-frontend:1.0
docker run -p 5000:5000 meu-backend:1.0
4. Deploy no Kubernetes
4.1. Aplicar Configurações Kubernetes
Aplique os arquivos de configuração Kubernetes:

bash
Copiar código
kubectl apply -f kubernetes/frontend-deployment.yaml
kubectl apply -f kubernetes/frontend-service.yaml
kubectl apply -f kubernetes/backend-deployment.yaml
kubectl apply -f kubernetes/backend-service.yaml
4.2. Verificar o Status
Verifique se os pods estão em execução:

bash
Copiar código
kubectl get pods
Verifique os serviços expostos:

bash
Copiar código
kubectl get services
5. Testar a Aplicação
5.1. Acessar a Aplicação
Para acessar a aplicação, use o comando:

bash
Copiar código
minikube service frontend-service
Isso abrirá a aplicação frontend no navegador padrão. O frontend se comunicará com o backend, que também deve estar acessível através de um serviço Kubernetes.

6. Conclusão
Neste guia, você aprendeu como orquestrar containers usando Kubernetes. Você construiu e implantou uma aplicação simples com um frontend e um backend, e acessou a aplicação rodando em um cluster Kubernetes local.

Links Úteis
Documentação do Kubernetes
Documentação do Minikube
Documentação do Docker
