# RoteiroPratico_Orquestra-o-de-Containers-usando-Kubernetes
Roteiro Prático Desenvolvido no âmbito da matéria de Engenharia de Software I do CEFET-MG

# Grupo:
### GUSTAVO CAMPOS PÁDUA (20203001143)
### JORGE ALIOMAR TROCOLI ABDON DANTAS (20213010469)
### SAMUEL PEDRO FERNANDES AMORIM (20213010502)

# Visão Geral
Neste guia, vamos demonstrar como orquestrar containers usando Kubernetes. Vamos criar um cluster Kubernetes local com Minikube e implantar uma aplicação simples composta por um frontend e um backend.

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
#### _**A) Windows**_
*  Baixar o Executável
Vá para a página de lançamentos do Kubernetes.
Baixe o arquivo kubectl para Windows. Você pode baixar o binário diretamente ou usar o instalador via choco (Chocolatey) se estiver disponível.
Usando o Instalador Chocolatey:

Se você tem o Chocolatey instalado, execute o seguinte comando no PowerShell:

powershell
Copiar código
choco install kubernetes-cli
Usando o Binário Direto:

Baixe o arquivo do binário e coloque-o em um diretório no seu PATH.
Adicionar ao PATH

Se você baixou o binário diretamente, adicione o diretório ao PATH. Isso pode ser feito no Painel de Controle -> Sistema e Segurança -> Sistema -> Configurações avançadas do sistema -> Variáveis de ambiente, e adicione o caminho do diretório onde kubectl.exe está localizado.
Verificar a Instalação

Abra o Prompt de Comando ou PowerShell e execute:

powershell
Copiar código
kubectl version --client
Isso deve exibir a versão do kubectl instalada.

1.2. macOS
Passos de Instalação
Usar Homebrew

Se você não tem o Homebrew instalado, você pode instalá-lo com o comando:

bash
Copiar código
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
Instale o kubectl usando Homebrew:

bash
Copiar código
brew install kubectl
Verificar a Instalação

Abra o Terminal e execute:

bash
Copiar código
kubectl version --client
Isso deve exibir a versão do kubectl instalada.

1.3. Linux
Ubuntu/Debian
Instalar o kubectl usando o Repositório APT

Atualize o índice de pacotes e adicione a chave do repositório:

bash
Copiar código
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl
Adicione o repositório do Kubernetes:

bash
Copiar código
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo sh -c 'echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list'
Instale o kubectl:

bash
Copiar código
sudo apt update
sudo apt install -y kubectl
Verificar a Instalação

Abra o Terminal e execute:

bash
Copiar código
kubectl version --client
Isso deve exibir a versão do kubectl instalada.

#### Node.js e npm:

1.2. Inicialização do Minikube
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
