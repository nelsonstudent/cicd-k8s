# Desafio Técnico – GitLab CI/CD e Kubernetes

Este projeto foi desenvolvido como solução para o **Desafio Técnico**, com o objetivo de demonstrar conhecimentos práticos em **GitLab CI/CD**, **Docker** e **Kubernetes**, simulando um cenário real de build e deploy de uma aplicação Java.

---

## 1.Objetivo

Automatizar o processo de:
1. Build da aplicação Java
2. Criação e versionamento da imagem Docker
3. Deploy da aplicação em um cluster Kubernetes
4. Validação automática do deploy

Tudo isso utilizando uma pipeline no **GitLab CI/CD**.

---

## 2.Arquitetura da Solução

- **GitLab CI/CD** para automação do pipeline
- **Docker** para empacotamento da aplicação
- **Kubernetes (kind)** como ambiente de execução
- **GitLab Agent for Kubernetes** para comunicação segura entre CI e cluster

---

## 3.Estrutura do Repositório

```text
.
├── .gitlab-ci.yml
├── Dockerfile
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
├── .gitlab/
│   └── agents/
│       └── java-agent/
│           └── config.yaml
└── README.md

```

## Pipeline GitLab CI/CD

A pipeline é composta por três estágios principais:

1) Build: 

- Executa testes da aplicação Java
- Gera a imagem Docker usando multi-stage build
- Publica a imagem no GitLab Container Registry

2) Deploy:

- Substitui dinamicamente a imagem no manifesto Kubernetes
- Realiza o deploy no cluster usando o GitLab Agent

3) Validate-deploy:

- Valida o rollout do Deployment
- Garante que a aplicação subiu corretamente

## Docker

- A imagem Docker é construída utilizando multi-stage build, separando:
- Ambiente de build (Maven)
- Ambiente de runtime (JRE)
- Isso reduz o tamanho final da imagem e melhora a segurança.

## Kubernetes

- A aplicação é executada via:
    . Deployment
    . Service (ClusterIP)

Inclui:
  . Readiness Probe
  . Labels bem definidas
  . Estrutura pronta para escalar

## GitLab Agent for Kubernetes

O deploy é realizado de forma segura utilizando o GitLab Agent, eliminando a necessidade de armazenar kubeconfig ou tokens no CI.

## Validação da Aplicação

Após o deploy:
```
kubectl get pods
kubectl port-forward svc/java-app 8080:80
curl http://localhost:8080
```

