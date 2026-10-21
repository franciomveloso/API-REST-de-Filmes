# 🎬 API REST de Filmes

API REST desenvolvida com **Python, Django e Django REST Framework** para cadastro, consulta, atualização e exclusão de filmes e gêneros.

O projeto utiliza SQLite como banco de dados, autenticação por token para operações de escrita e filtros para facilitar a consulta dos filmes. Também possui um script para importar dados iniciais de um arquivo CSV.

## 📌 Objetivo do projeto

Desenvolver uma API REST para gerenciamento de filmes, aplicando conceitos de desenvolvimento web com Django REST Framework, serialização de dados, operações CRUD, autenticação e filtros.

## 🛠️ Tecnologias utilizadas

- Python 3.13
- Django
- Django REST Framework
- Django REST Framework Token Authentication
- django-filter
- SQLite
- JSON
- Git e GitHub

## 📂 Estrutura do projeto

```text
projeto4/
├── manage.py
├── db.sqlite3
├── base_filmes.csv
├── importarDados.py
│
├── projeto4/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
└── filmes/
    ├── migrations/
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── serializers.py
    ├── views.py
    └── tests.py
```

### Principais arquivos

| Arquivo | Responsabilidade |
|---|---|
| `manage.py` | Executa comandos administrativos do Django. |
| `settings.py` | Configura o projeto, os aplicativos, o banco de dados e a autenticação. |
| `urls.py` | Define as rotas da API e do painel administrativo. |
| `models.py` | Define os modelos de filmes e gêneros. |
| `serializers.py` | Converte e valida os dados usados pela API. |
| `views.py` | Implementa as operações e o comportamento dos endpoints. |
| `admin.py` | Configura a administração dos dados pelo Django Admin. |
| `tests.py` | Arquivo destinado aos testes automatizados. |
| `importarDados.py` | Importa os dados iniciais do arquivo CSV. |
| `base_filmes.csv` | Contém os dados utilizados na importação inicial. |
| `db.sqlite3` | Banco de dados SQLite utilizado no projeto. |

## ⚙️ Instalação e configuração

### 1. Clonar o repositório

```bash
git clone <URL_DO_REPOSITORIO>
cd projeto4
```

Substitua `<URL_DO_REPOSITORIO>` pelo endereço real do repositório no GitHub.

### 2. Criar e ativar o ambiente virtual

Crie o ambiente:

```bash
python -m venv venv
```

No Windows, usando o Prompt de Comando:

```bat
venv\Scripts\activate
```

No Linux ou macOS:

```bash
source venv/bin/activate
```

### 3. Instalar as dependências

Com o ambiente virtual ativado:

```bash
python -m pip install django djangorestframework django-filter
```

### 4. Preparar o banco de dados

Se as migrações do aplicativo ainda não tiverem sido criadas:

```bash
python manage.py makemigrations filmes
```

Depois, execute:

```bash
python manage.py migrate
```

### 5. Criar um usuário administrador

```bash
python manage.py createsuperuser
```

Siga as instruções apresentadas no terminal.

### 6. Importar os filmes

Se `importarDados.py` e `base_filmes.csv` estiverem na pasta que contém `manage.py`, execute:

```bash
python importarDados.py
```

Execute as migrações antes da importação. O script deve apontar para o mesmo banco SQLite configurado no Django.

### 7. Iniciar o servidor

```bash
python manage.py runserver
```

Endereços locais:

- **API — rota principal:** `http://127.0.0.1:8000/api/`
- **Listagem de filmes:** `http://127.0.0.1:8000/api/filmes/`
- **Listagem de gêneros:** `http://127.0.0.1:8000/api/genero/`
- **Autenticação por token:** `http://127.0.0.1:8000/api/token/` (requisição `POST`)
- **Painel administrativo Django:** `http://127.0.0.1:8000/admin/`

> **Observação:** o servidor precisa estar em execução para acessar os endereços locais. A rota de autenticação utiliza o método `POST` para receber usuário e senha e retornar um token.

## 🔗 Endpoints da API

As rotas abaixo correspondem aos prefixos registrados no `DefaultRouter` em `projeto4/urls.py`. Os métodos disponíveis dependem da configuração dos ViewSets e das permissões.

| Método HTTP | Endpoint | Descrição |
|---|---|---|
| GET | `/api/filmes/` | Lista os filmes. |
| POST | `/api/filmes/` | Cadastra um filme. |
| GET | `/api/filmes/<id>/` | Consulta um filme pelo ID. |
| PUT | `/api/filmes/<id>/` | Atualiza um filme integralmente. |
| PATCH | `/api/filmes/<id>/` | Atualiza parcialmente um filme. |
| DELETE | `/api/filmes/<id>/` | Exclui um filme. |
| GET | `/api/genero/` | Lista os gêneros. |
| POST | `/api/genero/` | Cadastra um gênero. |
| GET | `/api/genero/<id>/` | Consulta um gênero pelo ID. |
| PUT | `/api/genero/<id>/` | Atualiza um gênero integralmente. |
| PATCH | `/api/genero/<id>/` | Atualiza parcialmente um gênero. |
| DELETE | `/api/genero/<id>/` | Exclui um gênero. |
| POST | `/api/token/` | Solicita um token de autenticação. |

`<id>` representa o identificador do registro.

## 🔐 Autenticação por token

Quando a API estiver configurada com permissões de leitura pública e escrita autenticada, as consultas podem ser feitas sem token. Para criar, editar ou excluir registros, envie um token válido.

### Solicitar um token

Envie uma requisição `POST` para `/api/token/` com o usuário e a senha cadastrados:

```http
POST /api/token/
Content-Type: application/json
```

Corpo da requisição:

```json
{
  "username": "seu_usuario",
  "password": "sua_senha"
}
```

Exemplo ilustrativo de resposta:

```json
{
  "token": "TOKEN_RETORNADO_PELA_API"
}
```

### Enviar o token

Inclua o token no cabeçalho das requisições que alteram os dados:

```http
Authorization: Token SEU_TOKEN
```

**Segurança:** não publique senhas nem tokens reais no README ou no repositório.

## 🔎 Filtros de consulta

A listagem de filmes pode aceitar filtros por parâmetros na URL, conforme os campos configurados em `views.py`.

| Parâmetro | Exemplo |
|---|---|
| `titulo` | `/api/filmes/?titulo=Matrix` |
| `ano_lancamento` | `/api/filmes/?ano_lancamento=1999` |
| `genero` | `/api/filmes/?genero=2` |
| `duracao` | `/api/filmes/?duracao=136` |
| `descricao` | `/api/filmes/?descricao=aventura` |

É possível combinar filtros:

```text
/api/filmes/?ano_lancamento=1999&genero=2
```

Os parâmetros acima refletem os campos descritos na documentação anterior. Confirme os nomes e o comportamento no código atual antes de considerar esta lista definitiva.

## 📥 Exemplos de requisições

Os exemplos desta seção são ilustrativos. Os campos e valores devem corresponder aos definidos nos modelos e serializers do projeto.

### 1. Listar filmes

```http
GET /api/filmes/
```

Exemplo de resposta:

```json
[
  {
    "id": 1,
    "titulo": "Matrix",
    "genero": 1,
    "descricao": "Um hacker descobre a verdade sobre a realidade simulada.",
    "ano_lancamento": 1999,
    "duracao": 136
  }
]
```

### 2. Cadastrar um filme

```http
POST /api/filmes/
Content-Type: application/json
Authorization: Token SEU_TOKEN
```

Corpo da requisição:

```json
{
  "titulo": "Interestelar",
  "genero": 1,
  "descricao": "Uma missão espacial em busca da sobrevivência humana.",
  "ano_lancamento": 2014,
  "duracao": 169
}
```

O gênero informado deve existir no banco de dados. Consulte `/api/genero/` para verificar os gêneros cadastrados.

### 3. Consultar um filme

```http
GET /api/filmes/1/
```

### 4. Atualizar parcialmente um filme

```http
PATCH /api/filmes/1/
Content-Type: application/json
Authorization: Token SEU_TOKEN
```

Corpo da requisição:

```json
{
  "duracao": 170
}
```

### 5. Excluir um filme

```http
DELETE /api/filmes/1/
Authorization: Token SEU_TOKEN
```

### 6. Consultar gêneros

```http
GET /api/genero/
```

## 🗄️ Banco de dados e importação

O projeto utiliza SQLite para armazenar os filmes e gêneros. O arquivo `base_filmes.csv` fornece os dados iniciais e `importarDados.py` realiza a importação para o banco.

Antes de executar o script, confirme se as tabelas foram criadas e se o script utiliza o mesmo arquivo de banco configurado no Django.

## 🧪 Testes

O arquivo `tests.py` pode ser utilizado para implementar testes automatizados dos modelos, endpoints, autenticação e validação dos dados.

Para executar os testes do aplicativo:

```bash
python manage.py test filmes
```

A existência do arquivo não significa que todos os cenários já estejam testados; a cobertura depende dos testes implementados.

## 🚀 Possíveis melhorias

- Ampliar a cobertura dos testes automatizados.
- Documentar todos os campos e validações dos serializers.
- Adicionar paginação, se necessário.
- Incluir documentação interativa da API.
- Configurar variáveis de ambiente e cuidados adicionais para produção.

## 👩‍💻 Autoria

**Sueny**

Projeto acadêmico desenvolvido para praticar Python, Django e desenvolvimento de APIs REST.
