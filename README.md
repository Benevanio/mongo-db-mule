# Projeto Mule + MongoDB

Aplicacao Mule 4 que expoe um endpoint HTTP para consultar documentos em uma colecao MongoDB e retornar o resultado em JSON.

## Visao geral

- Runtime Mule: 4.9.0
- Java: 17
- Endpoint HTTP: `GET /test`
- Porta padrao: `8081`
- Banco MongoDB: `Pokemon`
- Colecao consultada: `pokemon`
- Operacao usada: `find-documents`

Fluxo principal:
1. Recebe requisicao em `GET /test`.
2. Executa consulta no MongoDB (`collectionName="pokemon"`, campo `Name`).
3. Retorna payload JSON e registra log com a resposta.

## Estrutura do projeto

- `src/main/mule/global.xml`: configuracoes globais de HTTP Listener e MongoDB.
- `src/main/mule/mongo-db.xml`: fluxo `mongo-dbFlow` com listener, transformacoes e consulta no Mongo.
- `pom.xml`: dependencias e plugin Maven do Mule.
- `mule-artifact.json`: metadados do app Mule (versao minima do runtime e Java).

## Pre-requisitos

- Java 17
- Maven 3.8+
- Mule runtime 4.9.0 (via Anypoint Studio ou execucao Maven)
- MongoDB local em `localhost:27017`
- Credenciais de repositorio MuleSoft configuradas no Maven (`settings.xml`) para baixar conectores

## Configuracao do MongoDB

A conexao atual esta em:

```xml
<mongo:connection-string-connection connectionString="mongodb://localhost:27017/Pokemon" />
```

Essa configuracao fica em `src/main/mule/global.xml`.

### Exemplo de carga inicial

No Mongo shell (ou mongosh), crie a base e uma colecao de exemplo:

```javascript
use Pokemon

db.pokemon.insertMany([
  { Name: "Pikachu", Type: "Electric" },
  { Name: "Charmander", Type: "Fire" },
  { Name: "Bulbasaur", Type: "Grass" }
])
```

## Como executar

### Opcao 1: Anypoint Studio

1. Importe o projeto Maven no Studio.
2. Confirme Java 17 e Mule 4.9.0.
3. Execute a aplicacao.

### Opcao 2: Maven

No diretorio do projeto, execute:

```bash
mvn clean package
```

Depois rode no runtime Mule local (ou pelo Studio) com o artefato gerado em `target/`.

## Como testar

Com a aplicacao em execucao:

```bash
curl -X GET http://localhost:8081/test
```

Resposta esperada: lista de documentos da colecao `pokemon` em JSON.

## Observacoes

- O endpoint aceita apenas metodo `GET`.
- Se o MongoDB nao estiver disponivel, a consulta falhara no processador `mongo:find-documents`.
- Para mudar host/porta/database, ajuste a connection string no `global.xml`.
