# JuriDrive

# ⚖️ Sistema de Gestão e Versionamento de Processos Jurídicos

> Sistema web para organização, armazenamento e versionamento de processos jurídicos e seus respectivos documentos.


\

---

## 📌 Sobre o projeto

O projeto consiste em uma aplicação web desenvolvida para auxiliar profissionais do Direito na **organização e centralização de processos jurídicos e seus documentos**.

A proposta é oferecer um ambiente no qual cada processo funcione como uma unidade independente, contendo seus dados, partes envolvidas, documentos e histórico de alterações.

A aplicação possui uma proposta semelhante ao conceito de versionamento utilizado pelo GitHub: o processo é mantido como uma entidade central, enquanto alterações e eventos relacionados são registrados ao longo do tempo.

> **O MVP não tem como objetivo substituir sistemas jurídicos completos.**
>
> Seu foco inicial é oferecer uma ferramenta simples para organização, armazenamento e acompanhamento dos processos do usuário.

---

# 🎯 Objetivos

* Centralizar processos jurídicos em um único ambiente.
* Organizar documentos relacionados a cada processo.
* Facilitar a consulta de informações.
* Permitir controle do status dos processos.
* Registrar automaticamente alterações realizadas.
* Criar uma base arquitetural para futuras funcionalidades.

---

# 🚀 MVP

A primeira versão da aplicação será composta por três áreas principais:

### 🔐 Login

Sistema de autenticação para acesso à aplicação.

### 🗂️ Home

Tela responsável pela listagem dos processos do usuário.

Funcionalidades:

* Listagem de processos;
* Pesquisa;
* Filtro por status;
* Filtro por data de inclusão;
* Criação de novo processo;
* Acesso aos processos existentes.

### 📁 Processo

Tela individual de cada processo.

Contém:

* Nome do processo;
* Número do processo;
* Data de abertura;
* Status;
* Partes envolvidas;
* Observações;
* Documentos relacionados;
* Data da última alteração;
* Histórico de eventos.

---

# ✨ Funcionalidades

* [x] Autenticação de usuário
* [x] Cadastro de processos
* [x] Consulta de processos
* [x] Edição de processos
* [x] Alteração de status
* [x] Fechamento de processos
* [x] Reabertura de processos
* [x] Cadastro de partes
* [x] Upload de documentos
* [x] Download de documentos
* [x] Remoção de documentos
* [x] Pesquisa de processos
* [x] Filtro por status
* [x] Filtro por data
* [x] Registro automático da última alteração
* [x] Registro de eventos do processo
* [ ] Calendário
* [ ] Integração com Google Agenda
* [ ] Notificações
* [ ] Múltiplos usuários por escritório
* [ ] Sistema de permissões avançadas
* [ ] Integração com tribunais

> Os itens marcados como `[x]` representam o escopo planejado do MVP. O checklist será atualizado conforme a implementação avançar.

---

# 🏗️ Arquitetura

A aplicação utiliza uma arquitetura cliente-servidor baseada em API REST.

```text
┌─────────────────────────────────────┐
│              FRONTEND               │
│                                     │
│         React + Vite                │
│                                     │
└──────────────────┬──────────────────┘
                   │
                   │ HTTP / REST
                   ▼
┌─────────────────────────────────────┐
│              BACKEND                │
│                                     │
│              FastAPI                │
│                                     │
│  ┌─────────┐ ┌─────────┐ ┌────────┐│
│  │  Auth   │ │Processos│ │ Files  ││
│  └─────────┘ └─────────┘ └────────┘│
│                                     │
└──────────────┬───────────┬──────────┘
               │           │
               ▼           ▼
       ┌────────────┐ ┌─────────────┐
       │ PostgreSQL │ │   Storage   │
       │            │ │             │
       │ Dados      │ │ PDFs        │
       │ estruturados│ │ Imagens     │
       │            │ │ Vídeos      │
       └────────────┘ └─────────────┘
```

---

# 🧰 Tecnologias

## Frontend

<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" width="70" alt="React"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/vitejs/vitejs-original.svg" width="70" alt="Vite"/>
</p>

| Tecnologia            | Utilização                          |
| --------------------- | ----------------------------------- |
| **React**             | Construção da interface             |
| **Vite**              | Build e ambiente de desenvolvimento |
| **React Router**      | Navegação entre páginas             |
| **Styled Components** | Estilização da aplicação            |

---

## Backend

<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" width="70" alt="Python"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/fastapi/fastapi-original.svg" width="70" alt="FastAPI"/>
</p>

| Tecnologia     | Utilização                        |
| -------------- | --------------------------------- |
| **Python**     | Linguagem principal do backend    |
| **FastAPI**    | Desenvolvimento da API REST       |
| **Pydantic**   | Validação e serialização de dados |
| **JWT**        | Autenticação                      |
| **SQLAlchemy** | ORM                               |
| **Alembic**    | Migrações do banco                |

---

## Banco de Dados

<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/postgresql/postgresql-original.svg" width="80" alt="PostgreSQL"/>
</p>

**PostgreSQL**

Responsável pelo armazenamento dos dados estruturados da aplicação.

Entre os dados armazenados estão:

* Usuários;
* Processos;
* Partes;
* Documentos e seus metadados;
* Histórico de alterações.

---

## Armazenamento de arquivos

O conteúdo binário dos documentos não será armazenado diretamente no PostgreSQL.

A aplicação utilizará um serviço de armazenamento de objetos.

Opções previstas:

* Supabase Storage;
* Amazon S3;
* MinIO.

O banco armazenará apenas os metadados e a referência do arquivo.

---

# 🗃️ Modelo de domínio

```text
                    ┌──────────────┐
                    │    Usuário   │
                    └──────┬───────┘
                           │
                           │ 1:N
                           ▼
                    ┌──────────────┐
                    │   Processo   │
                    └──────┬───────┘
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             │ 1:N         │ 1:N         │ 1:N
             ▼             ▼             ▼
       ┌──────────┐  ┌────────────┐ ┌──────────────┐
       │  Partes  │  │ Documentos │ │  Histórico   │
       └──────────┘  └────────────┘ └──────────────┘
```

---

# 🗄️ Estrutura do banco

### `usuarios`

```text
id
nome
email
senha_hash
criado_em
atualizado_em
```

### `processos`

```text
id
usuario_id
numero_processo
nome_processo
status
data_abertura
observacoes
criado_em
atualizado_em
```

### `partes`

```text
id
processo_id
nome
tipo
criado_em
atualizado_em
```

### `documentos`

```text
id
processo_id
nome_original
nome_storage
tipo_arquivo
tamanho
caminho_storage
enviado_em
```

### `historico_alteracoes`

```text
id
processo_id
usuario_id
tipo_alteracao
descricao
criado_em
```

---

# 🔄 Versionamento

O sistema utiliza um modelo de **versionamento baseado em eventos**.

Cada alteração relevante gera um registro no histórico do processo.

Exemplo:

```text
22/09/2026 14:30
Processo criado.

22/09/2026 14:42
Parte adicionada.

22/09/2026 15:03
Documento "peticao.pdf" adicionado.

22/09/2026 15:21
Dados do processo alterados.

22/09/2026 16:10
Processo alterado para FECHADO.
```

Além do histórico, cada processo possui os campos:

```text
criado_em
atualizado_em
```

O campo `atualizado_em` é atualizado automaticamente sempre que ocorrer uma alteração relevante.

---

# 📊 Status do processo

No MVP serão utilizados apenas dois estados:

```text
┌─────────────┐
│    ABERTO   │
└──────┬──────┘
       │
       │ fechar
       ▼
┌─────────────┐
│   FECHADO   │
└──────┬──────┘
       │
       │ reabrir
       ▼
┌─────────────┐
│    ABERTO   │
└─────────────┘
```

Estados mais específicos poderão ser adicionados futuramente.

---

# 🔐 Segurança

A aplicação utilizará autenticação baseada em **JWT**.

```text
Usuário
   │
   │ Login
   ▼
FastAPI
   │
   │ Validação
   ▼
JWT
   │
   ▼
Frontend
   │
   │ Bearer Token
   ▼
API protegida
```

O backend deverá validar:

* autenticação;
* autorização;
* propriedade do processo;
* validade do token;
* permissões de acesso;
* integridade dos dados.

Um usuário não poderá acessar processos pertencentes a outro usuário.

---

# 📂 Estrutura do projeto

## Backend

```text
backend/
│
├── app/
│   │
│   ├── auth/
│   ├── usuarios/
│   ├── processos/
│   ├── partes/
│   ├── documentos/
│   ├── historico/
│   │
│   ├── core/
│   │   ├── config.py
│   │   ├── database.py
│   │   ├── security.py
│   │   └── storage.py
│   │
│   ├── models/
│   ├── schemas/
│   ├── repositories/
│   ├── services/
│   └── routers/
│
├── migrations/
├── tests/
└── main.py
```

## Frontend

```text
frontend/
│
├── src/
│   │
│   ├── assets/
│   │
│   ├── components/
│   │   ├── Sidebar/
│   │   ├── ProcessCard/
│   │   ├── SearchBar/
│   │   ├── UploadArea/
│   │   └── Modal/
│   │
│   ├── pages/
│   │   ├── Login/
│   │   ├── Home/
│   │   └── Processo/
│   │
│   ├── contexts/
│   │   └── AuthContext/
│   │
│   ├── services/
│   ├── hooks/
│   ├── routes/
│   └── styles/
│
└── main.jsx
```

---

# 🌐 API

## Autenticação

| Método | Endpoint         | Descrição |
| ------ | ---------------- | --------- |
| `POST` | `/auth/register` | Cadastro  |
| `POST` | `/auth/login`    | Login     |

## Processos

| Método   | Endpoint                 | Descrição         |
| -------- | ------------------------ | ----------------- |
| `GET`    | `/processos`             | Lista processos   |
| `GET`    | `/processos/{id}`        | Consulta processo |
| `POST`   | `/processos`             | Cria processo     |
| `PUT`    | `/processos/{id}`        | Edita processo    |
| `PATCH`  | `/processos/{id}/status` | Altera status     |
| `DELETE` | `/processos/{id}`        | Arquiva processo  |

## Partes

| Método   | Endpoint                 | Descrição      |
| -------- | ------------------------ | -------------- |
| `GET`    | `/processos/{id}/partes` | Lista partes   |
| `POST`   | `/processos/{id}/partes` | Adiciona parte |
| `PUT`    | `/partes/{id}`           | Edita parte    |
| `DELETE` | `/partes/{id}`           | Remove parte   |

## Documentos

| Método   | Endpoint                     | Descrição          |
| -------- | ---------------------------- | ------------------ |
| `POST`   | `/processos/{id}/documentos` | Upload             |
| `GET`    | `/processos/{id}/documentos` | Lista documentos   |
| `GET`    | `/documentos/{id}`           | Consulta documento |
| `DELETE` | `/documentos/{id}`           | Remove documento   |

---

# 📁 Armazenamento

Os arquivos serão organizados no Storage utilizando uma estrutura baseada no usuário e no processo:

```text
storage/
│
└── usuario_uuid/
    │
    ├── processo_uuid/
    │   ├── peticao_inicial.pdf
    │   ├── documento.jpg
    │   └── contrato.pdf
    │
    └── outro_processo_uuid/
        └── documento.pdf
```

O PostgreSQL armazenará somente os metadados e o caminho do arquivo.

---

# 🧪 Testes

A aplicação deverá possuir testes para as principais regras de negócio.

### Backend

* testes unitários;
* testes de integração;
* autenticação;
* autorização;
* CRUD de processos;
* upload;
* alteração de status;
* isolamento entre usuários.

### Frontend

* renderização dos componentes;
* navegação;
* autenticação;
* formulários;
* filtros;
* upload;
* tratamento de erros.

---

# 🛣️ Roadmap

## MVP 1.0

* [x] Autenticação
* [x] Processos
* [x] Partes
* [x] Documentos
* [x] Upload
* [x] Filtros
* [x] Status
* [x] Versionamento básico
* [x] Histórico de eventos

## V1.1 — Histórico avançado

* [ ] Timeline completa
* [ ] Identificação do usuário responsável
* [ ] Detalhamento das alterações
* [ ] Comparação de alterações

## V1.2 — Calendário

* [ ] Calendário interno
* [ ] Eventos
* [ ] Audiências
* [ ] Prazos
* [ ] Compromissos
* [ ] Integração com Google Agenda

## V1.3 — Notificações

* [x] Lembretes
* [ ] Notificações de prazos
* [ ] Notificações de eventos

## V2.0 — Gestão de escritórios

* [ ] Múltiplos usuários
* [ ] Escritórios
* [ ] Permissões
* [ ] Compartilhamento de processos
* [ ] Perfis de acesso
* [ ] Auditoria avançada

---

# 📋 Critérios de aceitação do MVP

O MVP será considerado concluído quando o usuário conseguir:

* [x] Fazer login;
* [x] Criar um processo;
* [x] Visualizar seus processos;
* [x] Pesquisar processos;
* [x] Filtrar processos;
* [x] Visualizar um processo;
* [x] Editar seus dados;
* [x] Cadastrar partes;
* [x] Adicionar documentos;
* [x] Visualizar documentos;
* [x] Baixar documentos;
* [x] Remover documentos;
* [x] Fechar processos;
* [x] Reabrir processos;
* [x] Visualizar a data da última alteração;
* [x] Ter seus processos isolados dos demais usuários.

---

# 🗺️ Roadmap arquitetural

```text
                    MVP
                     │
                     ▼
          ┌─────────────────────┐
          │ Gestão de Processos │
          └──────────┬──────────┘
                     │
          ┌──────────▼──────────┐
          │ Histórico avançado  │
          └──────────┬──────────┘
                     │
          ┌──────────▼──────────┐
          │      Calendário     │
          └──────────┬──────────┘
                     │
          ┌──────────▼──────────┐
          │    Notificações     │
          └──────────┬──────────┘
                     │
          ┌──────────▼──────────┐
          │ Gestão de Escritório│
          └─────────────────────┘
```

---

# 🧱 Princípios arquiteturais

O desenvolvimento deverá seguir alguns princípios:

* Separação de responsabilidades;
* API REST;
* Backend independente do frontend;
* Banco relacional;
* Arquivos separados do banco;
* Autenticação e autorização no backend;
* Validação no backend;
* Versionamento de banco através de migrations;
* Identificadores UUID;
* Registro de eventos importantes;
* Exclusão lógica quando aplicável;
* Preparação para evolução do sistema.

---

# 📌 Escopo resumido

O MVP pode ser resumido como:

> **Uma plataforma web autenticada para centralizar processos jurídicos, seus dados, partes e documentos, oferecendo organização, controle de status e registro das alterações realizadas em cada processo.**

O sistema será propositalmente enxuto na primeira versão, priorizando a construção de uma **base sólida de arquitetura, dados e segurança** que permita adicionar posteriormente calendário, notificações, integração com serviços externos e recursos para escritórios.

---

# 👨‍💻 Status do projeto

**Status:** 🟡 Modelagem / Arquitetura

**Versão:** `1.0.0-MVP`

**Próxima etapa:**

```text
Modelagem
    ↓
C4 Architecture
    ↓
UML
    ↓
ERD
    ↓
Dicionário de dados
    ↓
Contrato da API
    ↓
Wireframes
    ↓
Implementação
```

---

## 📄 Licença

Projeto privado / proprietário.

A definição da licença de distribuição deverá ser realizada posteriormente.
