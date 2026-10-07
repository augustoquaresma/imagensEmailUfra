# Prática — Docker, Virtualização, PostgreSQL, SQLite e DBeaver

## 1. Objetivos

Ao final desta prática, o estudante deverá ser capaz de:

- Diferenciar máquinas virtuais e containers;
- Compreender os conceitos de imagem, container, porta e volume;
- Executar containers utilizando Docker;
- Executar PostgreSQL em um container;
- Persistir dados utilizando Docker Volumes;
- Utilizar Docker Compose;
- Compreender as diferenças entre PostgreSQL e SQLite;
- Conectar PostgreSQL e SQLite ao DBeaver;
- Criar tabelas utilizando SQL;
- Inserir e consultar dados;
- Criar relacionamentos com chaves primárias e estrangeiras;
- Executar consultas utilizando `JOIN`.

---

# 2. Virtualização e Containers

Uma máquina virtual normalmente possui um sistema operacional completo:

```text
Hardware
   |
Sistema Operacional
   |
Hypervisor
   |
+-------------+-------------+
|    VM 01    |    VM 02    |
|-------------|-------------|
| Sistema Op. | Sistema Op. |
| Aplicação   | Aplicação   |
+-------------+-------------+
```

Containers utilizam uma abordagem diferente:

```text
Hardware
   |
Sistema Operacional
   |
Docker
   |
+---------------+---------------+
| Container 01  | Container 02  |
|---------------|---------------|
| PostgreSQL    | Aplicação     |
+---------------+---------------+
```

Containers compartilham recursos do sistema hospedeiro, sendo normalmente mais leves que máquinas virtuais completas.

### Questão

Por que um container normalmente consome menos recursos que uma máquina virtual?

---

# 3. Verificando o Docker

Execute:

```bash
docker --version
```

Consulte informações:

```bash
docker info
```

Liste containers ativos:

```bash
docker ps
```

Liste todos:

```bash
docker ps -a
```

---

# 4. Primeiro Container

Execute:

```bash
docker run hello-world
```

Fluxo simplificado:

```text
docker run
    |
    v
Existe imagem local?
    |
    +-- não --> Registro de imagens
                  |
                  v
             Download
                  |
                  v
          Criação do container
                  |
                  v
               Execução
```

Confira:

```bash
docker ps -a
```

---

# 5. Imagem × Container

Uma **imagem** funciona como um modelo para criação de containers.

```text
Imagem
  |
  +------> Container 01
  |
  +------> Container 02
  |
  +------> Container 03
```

Baixe a imagem do PostgreSQL:

```bash
docker pull postgres
```

Confira:

```bash
docker images
```

---

# 6. PostgreSQL com Docker

Crie o container:

```bash
docker run --name postgres-aula   -e POSTGRES_USER=aluno   -e POSTGRES_PASSWORD=123456   -e POSTGRES_DB=aula   -p 5432:5432   -d postgres
```

No PowerShell, pode ser utilizado em uma linha:

```powershell
docker run --name postgres-aula -e POSTGRES_USER=aluno -e POSTGRES_PASSWORD=123456 -e POSTGRES_DB=aula -p 5432:5432 -d postgres
```

## Parâmetros

| Parâmetro | Função |
|---|---|
| `--name postgres-aula` | Nome do container |
| `POSTGRES_USER` | Usuário do PostgreSQL |
| `POSTGRES_PASSWORD` | Senha |
| `POSTGRES_DB` | Banco criado inicialmente |
| `-p 5432:5432` | Mapeamento de porta |
| `-d` | Execução em segundo plano |
| `postgres` | Imagem utilizada |

> A senha `123456` é utilizada apenas para fins didáticos.

---

# 7. Entendendo a porta

O parâmetro:

```text
5432:5432
```

representa:

```text
Computador                         Container
localhost                          PostgreSQL

   5432 --------------------------> 5432
```

Formato:

```text
PORTA_HOST:PORTA_CONTAINER
```

---

# 8. Verificando o PostgreSQL

Execute:

```bash
docker ps
```

Consulte os logs:

```bash
docker logs postgres-aula
```

Para acompanhar os logs:

```bash
docker logs -f postgres-aula
```

Use `Ctrl + C` para sair da visualização contínua.

---

# 9. Entrando no Container

Execute:

```bash
docker exec -it postgres-aula bash
```

Agora execute:

```bash
psql -U aluno -d aula
```

O terminal deverá apresentar algo semelhante a:

```text
aula=#
```

---

# 10. Comandos básicos do PostgreSQL

Liste os bancos:

```sql
\l
```

Liste tabelas:

```sql
\dt
```

Inicialmente poderá aparecer:

```text
Did not find any relations.
```

---

# 11. Criando a primeira tabela

Crie:

```sql
CREATE TABLE aluno (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    curso VARCHAR(100),
    data_cadastro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Confira:

```sql
\dt
```

Visualize a estrutura:

```sql
\d aluno
```

---

# 12. Inserindo dados

Execute:

```sql
INSERT INTO aluno (nome, email, curso)
VALUES ('Ana Silva', 'ana@email.com', 'Computação');
```

```sql
INSERT INTO aluno (nome, email, curso)
VALUES ('Carlos Souza', 'carlos@email.com', 'Geografia');
```

Consulte:

```sql
SELECT * FROM aluno;
```

---

# 13. Saindo do PostgreSQL e do Container

Saia do PostgreSQL:

```text
\q
```

Saia do container:

```bash
exit
```

---

# 14. Conectando PostgreSQL ao DBeaver

Abra o DBeaver e selecione:

```text
Nova conexão
     |
     v
PostgreSQL
```

Utilize:

| Configuração | Valor |
|---|---|
| Host | `localhost` |
| Port | `5432` |
| Database | `aula` |
| Username | `aluno` |
| Password | `123456` |

Clique em **Test Connection**.

Caso o DBeaver solicite o driver PostgreSQL, permita sua instalação.

Após conectar:

```text
PostgreSQL
└── aula
    └── Schemas
        └── public
            └── Tables
                └── aluno
```

Abra a tabela e visualize seus registros.

---

# 15. Criando uma tabela pelo DBeaver

Abra o **SQL Editor**.

Execute:

```sql
CREATE TABLE disciplina (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    carga_horaria INTEGER NOT NULL,
    semestre INTEGER
);
```

Insira:

```sql
INSERT INTO disciplina
(nome, carga_horaria, semestre)
VALUES
('Desenvolvimento Web', 68, 4),
('Banco de Dados', 68, 3),
('Engenharia de Software', 68, 5);
```

Consulte:

```sql
SELECT * FROM disciplina;
```

---

# 16. Criando relacionamentos

Crie:

```sql
CREATE TABLE matricula (
    id SERIAL PRIMARY KEY,

    aluno_id INTEGER NOT NULL,
    disciplina_id INTEGER NOT NULL,

    data_matricula DATE DEFAULT CURRENT_DATE,

    FOREIGN KEY (aluno_id)
        REFERENCES aluno(id),

    FOREIGN KEY (disciplina_id)
        REFERENCES disciplina(id)
);
```

Relacionamento:

```text
ALUNO
  |
  | 1
  |
  +----------< MATRÍCULA >----------+
                                     |
                                     | N
                                DISCIPLINA
```

---

# 17. Inserindo matrículas

Execute:

```sql
INSERT INTO matricula
(aluno_id, disciplina_id)
VALUES
(1, 1),
(1, 2),
(2, 1);
```

Consulte:

```sql
SELECT * FROM matricula;
```

---

# 18. Consulta utilizando JOIN

Execute:

```sql
SELECT
    aluno.nome AS aluno,
    disciplina.nome AS disciplina,
    matricula.data_matricula
FROM matricula
INNER JOIN aluno
    ON aluno.id = matricula.aluno_id
INNER JOIN disciplina
    ON disciplina.id = matricula.disciplina_id;
```

Observe como os dados de três tabelas são combinados.

---

# 19. Ciclo de vida do Container

Pare:

```bash
docker stop postgres-aula
```

Confira:

```bash
docker ps
docker ps -a
```

Inicie novamente:

```bash
docker start postgres-aula
```

Reinicie:

```bash
docker restart postgres-aula
```

---

# 20. Persistência com Docker Volume

Containers podem ser removidos. Dados importantes não devem depender exclusivamente do ciclo de vida do container.

Crie um volume:

```bash
docker volume create postgres-dados
```

Liste:

```bash
docker volume ls
```

A arquitetura será:

```text
Container PostgreSQL
       |
       v
/var/lib/postgresql/data
       |
       v
+----------------------+
|    Docker Volume     |
|   postgres-dados     |
+----------------------+
```

Para recriar o exercício usando volume, remova primeiro o container anterior quando ele não for mais necessário:

```bash
docker stop postgres-aula
docker rm postgres-aula
```

Crie novamente:

```bash
docker run --name postgres-aula   -e POSTGRES_USER=aluno   -e POSTGRES_PASSWORD=123456   -e POSTGRES_DB=aula   -p 5432:5432   -v postgres-dados:/var/lib/postgresql/data   -d postgres
```

PowerShell:

```powershell
docker run --name postgres-aula -e POSTGRES_USER=aluno -e POSTGRES_PASSWORD=123456 -e POSTGRES_DB=aula -p 5432:5432 -v postgres-dados:/var/lib/postgresql/data -d postgres
```

---

# 21. Docker Compose

Crie:

```text
docker-compose.yml
```

Conteúdo:

```yaml
services:

  postgres:

    image: postgres

    container_name: postgres-aula

    environment:
      POSTGRES_USER: aluno
      POSTGRES_PASSWORD: 123456
      POSTGRES_DB: aula

    ports:
      - "5432:5432"

    volumes:
      - postgres-dados:/var/lib/postgresql/data

volumes:
  postgres-dados:
```

> Se já existir um container chamado `postgres-aula`, remova-o antes de iniciar a composição para evitar conflito de nomes.

Execute:

```bash
docker compose up -d
```

Confira:

```bash
docker compose ps
```

Pare:

```bash
docker compose stop
```

Inicie:

```bash
docker compose start
```

Remova os containers da composição:

```bash
docker compose down
```

Para acompanhar logs:

```bash
docker compose logs -f
```

---

# 22. PostgreSQL × SQLite

PostgreSQL utiliza uma arquitetura cliente-servidor:

```text
DBeaver
   |
   | localhost:5432
   v
PostgreSQL
   |
   v
Banco aula
```

SQLite utiliza outra abordagem:

```text
DBeaver
   |
   v
SQLite
   |
   v
aula.db
```

O SQLite armazena o banco diretamente em um arquivo.

---

# 23. Comparação

| Característica | PostgreSQL | SQLite |
|---|---|---|
| Arquitetura | Cliente-servidor | Banco baseado em arquivo |
| Servidor | Sim | Não |
| Porta | Geralmente 5432 | Não utiliza |
| Usuários | Sim | Não da mesma forma |
| Senha | Sim | Não por padrão |
| Docker | Muito comum | Geralmente desnecessário |
| Armazenamento | Servidor/volume | Arquivo `.db` |
| Aplicações Web | Excelente | Possível, com limitações |
| Aplicações locais | Possível | Excelente |
| DBeaver | Conecta ao servidor | Abre o arquivo |

---

# 24. Criando um banco SQLite

Crie uma pasta:

```bash
mkdir sqlite
cd sqlite
```

O banco será:

```text
aula.db
```

Caso o executável SQLite esteja instalado:

```bash
sqlite3 aula.db
```

---

# 25. Criando tabela no SQLite

Execute:

```sql
CREATE TABLE produto (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT NOT NULL,
    descricao TEXT,
    preco REAL NOT NULL,
    quantidade INTEGER DEFAULT 0
);
```

Insira:

```sql
INSERT INTO produto
(nome, descricao, preco, quantidade)
VALUES
('Notebook', 'Notebook para desenvolvimento', 4500.00, 5);
```

```sql
INSERT INTO produto
(nome, descricao, preco, quantidade)
VALUES
('Mouse', 'Mouse sem fio', 120.00, 10);
```

Consulte:

```sql
SELECT * FROM produto;
```

Saia:

```text
.quit
```

Estrutura:

```text
sqlite/
└── aula.db
```

---

# 26. SQLite pelo DBeaver

No DBeaver:

```text
Nova conexão
     |
     v
SQLite
```

Em vez de configurar host, porta, usuário e senha, selecione o arquivo:

```text
aula.db
```

Arquitetura:

```text
DBeaver
   |
   v
SQLite Driver
   |
   v
aula.db
   |
   └── produto
```

---

# 27. Criando uma tabela SQLite pelo DBeaver

Abra o SQL Editor da conexão SQLite.

Execute:

```sql
CREATE TABLE categoria (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT NOT NULL UNIQUE
);
```

Insira:

```sql
INSERT INTO categoria (nome)
VALUES ('Informática');
```

```sql
INSERT INTO categoria (nome)
VALUES ('Periféricos');
```

Consulte:

```sql
SELECT * FROM categoria;
```

---

# 28. Desafio — Banco Acadêmico

Crie no PostgreSQL um pequeno banco acadêmico.

## Tabela curso

```text
CURSO
├── id
├── nome
└── carga_horaria
```

## Tabela aluno

```text
ALUNO
├── id
├── nome
└── email
```

## Tabela matricula

```text
MATRICULA
├── id
├── aluno_id
├── curso_id
└── data_matricula
```

## Requisitos

1. Execute PostgreSQL utilizando Docker;
2. Utilize Docker Volume;
3. Conecte pelo DBeaver;
4. Crie as três tabelas;
5. Defina as chaves primárias;
6. Defina as chaves estrangeiras;
7. Cadastre pelo menos 3 alunos;
8. Cadastre pelo menos 3 cursos;
9. Cadastre pelo menos 5 matrículas;
10. Faça uma consulta utilizando `JOIN`.

Depois, reproduza as três tabelas em um banco SQLite e compare as duas implementações.

---

# 29. Exemplo de consulta do desafio

```sql
SELECT
    aluno.nome AS aluno,
    curso.nome AS curso,
    matricula.data_matricula
FROM matricula
INNER JOIN aluno
    ON aluno.id = matricula.aluno_id
INNER JOIN curso
    ON curso.id = matricula.curso_id;
```

---

# 30. Questões para reflexão

1. Qual a diferença entre máquina virtual e container?
2. Qual a diferença entre imagem e container?
3. Para que serve `docker run`?
4. Para que serve `docker ps`?
5. O que significa `5432:5432`?
6. Para que serve um Docker Volume?
7. O que acontece com os dados quando um container sem volume é removido?
8. Qual a diferença entre PostgreSQL e SQLite?
9. Por que SQLite não precisa de servidor?
10. Qual a função do DBeaver?
11. O que é uma chave primária?
12. O que é uma chave estrangeira?
13. Para que serve um `JOIN`?
14. Qual a vantagem do Docker Compose?
15. Por que SQLite geralmente não precisa ser executado em um container?

---

# 31. Entrega

Organize o projeto:

```text
pratica-docker/
│
├── docker-compose.yml
├── README.md
│
├── scripts/
│   ├── postgresql.sql
│   └── sqlite.sql
│
└── sqlite/
    └── aula.db
```

O estudante deverá apresentar evidências de:

```bash
docker ps
```

```bash
docker volume ls
```

```bash
docker compose ps
```

No PostgreSQL:

```sql
SELECT * FROM aluno;
```

```sql
SELECT * FROM disciplina;
```

```sql
SELECT * FROM matricula;
```

No SQLite:

```sql
SELECT * FROM produto;
```

```sql
SELECT * FROM categoria;
```

Também deverá apresentar a consulta com `JOIN`.

---

# 32. Síntese da prática

Fluxo PostgreSQL:

```text
Virtualização
      |
      v
Containers
      |
      v
Docker
      |
      v
Imagem PostgreSQL
      |
      v
Container PostgreSQL
      |
      +----------> Docker Volume
      |
      v
localhost:5432
      |
      v
DBeaver
      |
      v
SQL
      |
      v
Tabelas
      |
      v
PK + FK + JOIN
```

Fluxo SQLite:

```text
SQLite
   |
   v
arquivo aula.db
   |
   v
DBeaver
   |
   v
SQL
   |
   v
Tabelas
```

Ao final, o estudante deverá compreender a diferença prática entre um **SGBD cliente-servidor**, como PostgreSQL, e um **banco de dados embarcado baseado em arquivo**, como SQLite, além de compreender como Docker pode ser utilizado para criar ambientes de infraestrutura reproduzíveis.
