# Redator de Novelas

Editor de textos para escrever e organizar capítulos de uma novela. A interface funciona no navegador, salva um rascunho localmente e permite criar uma conta para guardar documentos em uma API com banco de dados SQLite.

## Funcionalidades

- Editor com título, texto, contexto e personagens.
- Salvamento automático do rascunho no `localStorage` do navegador.
- Cadastro e login, com documentos associados ao usuário e salvos na API.
- Temas claro e escuro, download do texto em `.txt` e ditado por voz quando o navegador oferece reconhecimento de fala.

## Tecnologias

O front-end usa HTML, CSS e JavaScript sem framework. O back-end usa Python, FastAPI, SQLAlchemy, SQLite e autenticação por JWT.

## Executar localmente

Você precisa de Python 3 e de um navegador moderno. Abra dois terminais na raiz do projeto.

**1. Inicie a API:**

```bash
cd back-end
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python main.py
```

A API estará em `http://localhost:8000` e sua documentação interativa em `http://localhost:8000/docs`. Por padrão, o banco será criado em `back-end/redator.db` quando a API for iniciada a partir desse diretório.

**2. Sirva a interface:**

```bash
python3 -m http.server 5500
```

Abra `http://localhost:5500` no navegador. O front-end está configurado em `front-end/api.js` para acessar `http://localhost:8000`; altere `API_URL` nesse arquivo se a API usar outro endereço.

O rascunho local pode ser usado sem conta. Para salvar e listar documentos na API, crie uma conta pela interface e faça login. O token da sessão fica no `localStorage` do navegador.

## Estrutura do projeto

| Caminho | Conteúdo |
| --- | --- |
| `index.html` | Página principal do editor |
| `front-end/app.js` | Interface, edição e salvamento |
| `front-end/api.js` | Cliente HTTP da API |
| `front-end/style.css` | Estilos e temas |
| `back-end/main.py` | Rotas FastAPI |
| `back-end/auth.py` | Autenticação e tokens |
| `back-end/models.py` e `back-end/database.py` | Modelos e conexão com o banco |

## API

Após iniciar o back-end, consulte `http://localhost:8000/docs` para testar as rotas. As principais são:

| Método | Rota | Uso |
| --- | --- | --- |
| `POST` | `/auth/register` | Criar usuário |
| `POST` | `/auth/login` | Entrar e receber um token |
| `GET` | `/auth/me` | Consultar o usuário autenticado |
| `GET`, `POST` | `/documents` | Listar ou criar documentos |
| `GET`, `PUT`, `DELETE` | `/documents/{document_id}` | Consultar, atualizar ou excluir um documento |
| `POST` | `/documents/save-or-update` | Criar ou atualizar pelo título |

As rotas de documentos exigem `Authorization: Bearer <token>`. O login recebe os campos `username` e `password` como formulário. Consulte também [a documentação do back-end](back-end/README.md) para exemplos com `curl`.

## Antes de publicar

Este projeto está configurado para desenvolvimento local. Em `back-end/auth.py`, substitua a chave JWT fixa por um segredo fornecido pelo ambiente. Em `back-end/main.py`, restrinja as origens CORS ao domínio da interface. Use HTTPS e configure o endereço da API para o ambiente de publicação.
