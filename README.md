# DevSecOps Pipeline com Jenkins e SonarQube

Este repositório contém os arquivos e configurações necessários para implementar um pipeline DevSecOps completo utilizando Jenkins, SonarQube e Docker.

## Objetivo

Este projeto demonstra a implementação de práticas DevSecOps, integrando análise de segurança diretamente no processo de CI/CD. O pipeline construído:

1. Extrai o código-fonte do repositório GitHub
2. Realiza análise estática de segurança com SonarQube
3. Verifica se o código passa nos critérios de qualidade
4. Constrói uma imagem Docker da aplicação
5. Publica a imagem no Docker Hub

## Pré-requisitos

- AWS EC2 t3.medium ou equivalente com Ubuntu
- Portas 8080 (Jenkins) e 9000 (SonarQube) abertas
- Docker instalado
- Conta no Docker Hub
- Java 17 ou superior

## Estrutura do Projeto

O repositório contém:

- **Jenkinsfile**: Definição do pipeline CI/CD
- **Dockerfile**: Configuração para construção da imagem Docker
- **pom.xml**: Arquivo de configuração Maven para o projeto Spring Boot
- **src/**: Código-fonte da aplicação Spring Boot

## Configuração do Ambiente

### 1. Instalar Jenkins

```bash
sudo apt update
sudo apt install openjdk-17-jre
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

### 2. Instalar SonarQube

```bash
sudo adduser sonarqube
sudo su - sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.7.96285.zip
unzip sonarqube-9.9.7.96285.zip
chmod -R 755 /home/sonarqube/sonarqube-9.9.7.96285
cd sonarqube-9.9.7.96285/bin/linux-x86-64/
./sonar.sh start
```

### 3. Instalar Docker

```bash
sudo apt update
sudo apt install docker.io
sudo usermod -aG docker $USER
sudo usermod -aG docker jenkins
sudo systemctl restart docker
```

## Configuração do Pipeline

### 1. Plugins Necessários no Jenkins

- Docker Pipeline
- SonarQube Scanner

### 2. Credenciais Necessárias

Configurar no Jenkins:
- `docker-cred`: Credenciais do Docker Hub
- `sonar`: Token de acesso ao SonarQube
- `git`: Token de acesso ao GitHub

### 3. Configurar o Pipeline no Jenkins

1. Criar novo item do tipo Pipeline
2. Definir o pipeline para usar o Jenkinsfile deste repositório

## Executando o Pipeline

Após a configuração, você pode executar o pipeline diretamente no Jenkins. O processo:

1. Faz o checkout do código
2. Executa análise de segurança no SonarQube
3. Aguarda aprovação do Quality Gate
4. Constrói e publica a imagem Docker

## Resultados da Análise de Segurança

Os resultados da análise de segurança podem ser visualizados no SonarQube, acessando:
`http://[IP-DO-EC2]:9000`

## Imagem Docker

A imagem Docker gerada é publicada no Docker Hub em:
`danielmelo5627/ultimate-cicd`

## Artigo Detalhado

Para uma explicação mais detalhada deste projeto, consulte o [artigo completo no Medium](https://medium.com/@dreimao4/devsecops-na-pr%C3%A1tica-pipeline-de-seguran%C3%A7a-com-jenkins-e-sonarqube-3aa5f072a05a).

## Contribuições

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues ou enviar pull requests.

## Licença

Este projeto está licenciado sob a licença MIT.
