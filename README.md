# ContabGo — Motores de Cálculo RTC

Projeto de dockerização dos motores de cálculo da Reforma Tributária do Consumo (RTC), disponibilizados pela Receita Federal do Brasil.

O objetivo é simplificar a instalação e a execução das calculadoras, permitindo que sejam executadas em containers Docker, individualmente ou simultaneamente, por meio do Docker Compose.

## Motores disponíveis

| Motor | Porta local |
|---|---|
| Regime Regular | 8081 |
| Simples Nacional | 8082 |

Cada aplicação é executada em seu próprio container, com isolamento de ambiente e configuração independente.

## Tecnologias

- Docker
- Docker Compose
- Java 25
- Maven
- Spring Boot
- GitHub Container Registry (GHCR)

## Estrutura do projeto

```text
contabgo-receita-motores/
├── calculadora-rtc-regime-regular/
│   ├── Dockerfile
│   └── ...
├── calculadora-rtc-simples-nacional/
│   ├── Dockerfile
│   └── ...
└── docker-compose.yml
```

Cada motor possui seu próprio Dockerfile, responsável por compilar a aplicação e gerar uma imagem Docker pronta para execução.

O Docker Compose centraliza a configuração dos dois serviços, permitindo iniciá-los simultaneamente.

## Pré-requisitos

Para executar o projeto, é necessário ter o Docker instalado.

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) — Windows e macOS.
- [Docker Engine](https://docs.docker.com/engine/install/) — Linux.

O Docker Compose está disponível como parte do Docker Desktop ou como plugin do Docker Engine.

Não é necessário instalar Java ou Maven localmente para construir e executar as aplicações utilizando os Dockerfiles fornecidos.

## Instalação e execução

### 1. Clone o repositório

```bash
git clone https://github.com/0t4v14n0/contabgo-receita-motores.git
```

Entre no diretório do projeto:

```bash
cd contabgo-receita-motores
```

### 2. Construa e execute os dois motores

Na raiz do projeto, execute:

```bash
docker compose up -d --build
```

Esse comando constrói as imagens utilizando os Dockerfiles de cada aplicação e inicia os dois containers em segundo plano.

### 3. Acesse as aplicações

Após a inicialização, os motores ficam disponíveis nas seguintes portas:

| Aplicação | Endereço local |
|---|---|
| Regime Regular | http://localhost:8081 |
| Simples Nacional | http://localhost:8082 |

Os endereços acima correspondem às portas dos serviços. As rotas específicas das APIs dependem da implementação original de cada aplicação.

### 4. Consulte os logs

Para acompanhar a inicialização dos dois motores:

```bash
docker compose logs -f
```

Para consultar somente o Regime Regular:

```bash
docker compose logs -f motor-rtc-regime-regular
```

Para consultar somente o Simples Nacional:

```bash
docker compose logs -f motor-rtc-simples-nacional
```

### 5. Encerre os containers

Para parar e remover os containers:

```bash
docker compose down
```

As imagens construídas permanecem disponíveis localmente.

## Executando individualmente

Também é possível iniciar apenas um dos motores.

### Regime Regular

```bash
docker compose up -d --build motor-rtc-regime-regular
```

A aplicação ficará disponível em:

http://localhost:8081

### Simples Nacional

```bash
docker compose up -d --build motor-rtc-simples-nacional
```

A aplicação ficará disponível em:

http://localhost:8082

## Executando as imagens publicadas no GHCR

As imagens Docker estão configuradas para utilização do GitHub Container Registry (GHCR).

| Aplicação | Imagem |
|---|---|
| Regime Regular | `ghcr.io/0t4v14n0/contabgo-motor-rtc-regular:latest` |
| Simples Nacional | `ghcr.io/0t4v14n0/contabgo-motor-rtc-simples:latest` |

Caso as imagens já estejam publicadas no GHCR, é possível executá-las sem precisar compilar o código-fonte.

```bash
docker compose pull
docker compose up -d
```

Para utilizar essa modalidade, o arquivo `docker-compose.yml` deve conter as referências das imagens publicadas e não exigir a construção local por meio de `build`.

## Publicando as imagens no GHCR

Para publicar as imagens, é necessário autenticar-se no GitHub Container Registry.

Crie um Personal Access Token (classic) no GitHub com a permissão `write:packages`.

Realize a autenticação:

```bash
echo SEU_TOKEN | docker login ghcr.io -u 0t4v14n0 --password-stdin
```

Substitua `SEU_TOKEN` pelo token criado. Não compartilhe o token ou o adicione ao repositório.

### Construindo as imagens

Na raiz do projeto:

```bash
docker compose build
```

### Publicando as imagens

```bash
docker compose push
```

O Docker Compose utilizará os nomes definidos no arquivo `docker-compose.yml` para publicar as imagens no GHCR.

Após a publicação, outros usuários poderão baixar e executar os motores sem precisar construir as imagens localmente.

## Configuração dos containers

As aplicações possuem as seguintes configurações:

| Configuração | Valor |
|---|---|
| Perfil Spring ativo | `offline` |
| Memória inicial da JVM | 256 MB |
| Memória máxima da JVM | 512 MB |
| Porta interna | 8080 |
| Política de reinicialização | `unless-stopped` |

A variável `JAVA_TOOL_OPTIONS` configura os limites de memória da JVM.

O perfil `offline` é definido no comando de inicialização de cada container.

Os Dockerfiles também executam o Maven e as migrações Flyway durante a construção das imagens.

## Sobre o projeto

Este repositório tem como finalidade facilitar a utilização dos motores de cálculo da Reforma Tributária do Consumo por meio da tecnologia Docker.

O projeto não desenvolve nem modifica as regras de cálculo tributário das aplicações originais. Seu foco é disponibilizar uma forma simplificada de construir, executar e distribuir os motores.

Os códigos-fonte e as regras de negócio das calculadoras são de seus respectivos desenvolvedores e mantenedores.

Para informações sobre as regras tributárias, consulte a documentação disponibilizada pela Receita Federal do Brasil.

## Créditos

- Receita Federal do Brasil — motores de cálculo da Reforma Tributária do Consumo.
- [Otaviano Cruz](https://github.com/0t4v14n0) — dockerização e disponibilização das aplicações.

## Licença

Este repositório não possui uma licença de distribuição definida.

A dockerização não altera os direitos autorais ou os termos de utilização do código-fonte original das calculadoras.

Consulte os termos e as condições de distribuição do projeto original antes de redistribuir suas aplicações.