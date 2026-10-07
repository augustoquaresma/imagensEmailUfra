# Prática de Git com Git Flow

## 1. Objetivos

Ao final desta prática, o estudante deverá ser capaz de:

- Compreender os conceitos básicos de controle de versão;
- Inicializar e configurar um repositório Git;
- Utilizar `git status`, `git add`, `git commit`, `git diff` e `git log`;
- Compreender o papel das branches em um projeto;
- Aplicar uma estratégia baseada em **Git Flow**;
- Trabalhar com as branches `main` e `develop`;
- Criar branches do tipo `feature/*`, `release/*` e `hotfix/*`;
- Realizar merges entre branches;
- Identificar e resolver conflitos;
- Criar tags para representar versões do software;
- Visualizar graficamente a evolução do repositório.

---

# 2. Cenário da prática

Uma equipe está desenvolvendo um pequeno site institucional.

O projeto será versionado utilizando Git e uma estratégia inspirada no **Git Flow**.

Durante a prática serão desenvolvidas novas funcionalidades, preparada uma versão do sistema para publicação e realizada uma correção emergencial.

O fluxo utilizado será:

```text
                         feature/menu
                              |
                              v
main ----●---------------------|----------------------●---------
          \                    |                     /
           \                   v                    /
develop ----●------------------●---------●----------●-----------
             \                          /
              \---- feature/rodape ----/
                                      \
                                       \--- release/1.0
```

Posteriormente será criado um fluxo de correção:

```text
main ----------------●----------------------●
                      \                    /
                       \--- hotfix/1.0.1 --/
```

---

# 3. Estrutura do Git Flow

Nesta prática utilizaremos as seguintes branches:

| Branch | Finalidade |
|---|---|
| `main` | Versões estáveis e publicadas |
| `develop` | Integração das funcionalidades em desenvolvimento |
| `feature/*` | Desenvolvimento de novas funcionalidades |
| `release/*` | Preparação de uma nova versão |
| `hotfix/*` | Correções urgentes em produção |

## Regra geral

```text
Nova funcionalidade
        |
        v
   feature/*
        |
        v
     develop
        |
        v
    release/*
      /     \
     v       v
   main    develop
     |
     v
    tag
```

Para uma correção urgente:

```text
       main
        |
        v
   hotfix/*
     /     \
    v       v
  main    develop
```

> Nesta atividade, o Git Flow será executado manualmente com comandos Git. Isso ajuda a compreender o que a estratégia faz antes de utilizar extensões ou ferramentas que automatizam o fluxo.

---

# 4. Verificando e configurando o Git

Verifique se o Git está instalado:

```bash
git --version
```

Configure sua identificação:

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seuemail@email.com"
```

Confira:

```bash
git config --global --list
```

---

# 5. Criando o projeto

Crie a pasta:

```bash
mkdir pratica-git-flow
cd pratica-git-flow
```

Inicialize o repositório:

```bash
git init
```

Verifique:

```bash
git status
```

Caso a branch inicial não seja `main`, renomeie:

```bash
git branch -M main
```

---

# 6. Criando a primeira versão do projeto

Crie o arquivo `index.html`:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prática Git Flow</title>
</head>

<body>

    <h1>Projeto Desenvolvimento Web</h1>

    <p>
        Projeto utilizado para aprendizagem de Git e Git Flow.
    </p>

</body>
</html>
```

Verifique:

```bash
git status
```

Adicione:

```bash
git add index.html
```

Verifique novamente:

```bash
git status
```

Crie o primeiro commit:

```bash
git commit -m "chore: cria estrutura inicial do projeto"
```

Consulte o histórico:

```bash
git log --oneline
```

---

# 7. Criando a branch develop

No Git Flow, o desenvolvimento cotidiano não ocorre diretamente na `main`.

A branch `main` representa versões estáveis.

Crie a branch de desenvolvimento:

```bash
git switch -c develop
```

Confira:

```bash
git branch
```

Resultado esperado:

```text
* develop
  main
```

---

# 8. Feature 01 — Estilização da página

Uma nova funcionalidade deverá ser criada a partir da `develop`.

Certifique-se de estar na `develop`:

```bash
git switch develop
```

Crie:

```bash
git switch -c feature/estilizacao
```

Crie:

```text
css/style.css
```

Conteúdo:

```css
body {
    font-family: Arial, sans-serif;
    margin: 40px;
}

h1 {
    text-align: center;
}

p {
    line-height: 1.5;
}
```

Adicione ao `<head>` do HTML:

```html
<link rel="stylesheet" href="css/style.css">
```

Analise:

```bash
git status
git diff
```

Registre:

```bash
git add .
git commit -m "feat: adiciona estilização inicial"
```

---

# 9. Finalizando uma feature

A funcionalidade está pronta.

Retorne para:

```bash
git switch develop
```

Integre:

```bash
git merge feature/estilizacao
```

Exclua a branch finalizada:

```bash
git branch -d feature/estilizacao
```

Visualize:

```bash
git log --graph --oneline --all
```

---

# 10. Feature 02 — Criando um menu

Crie outra funcionalidade:

```bash
git switch develop
git switch -c feature/menu
```

Adicione ao `index.html`:

```html
<nav>
    <a href="#">Início</a>
    <a href="#">Sobre</a>
    <a href="#">Contato</a>
</nav>
```

Adicione estilos:

```css
nav {
    margin-bottom: 30px;
}

nav a {
    margin-right: 15px;
}
```

Registre:

```bash
git add .
git commit -m "feat: adiciona menu de navegação"
```

Finalize:

```bash
git switch develop
git merge feature/menu
git branch -d feature/menu
```

---

# 11. Feature 03 — Criando o rodapé

Crie:

```bash
git switch -c feature/rodape
```

Adicione:

```html
<footer>
    <p>Desenvolvido na disciplina de Desenvolvimento Web</p>
</footer>
```

Registre:

```bash
git add .
git commit -m "feat: adiciona rodapé"
```

Integre:

```bash
git switch develop
git merge feature/rodape
git branch -d feature/rodape
```

Consulte:

```bash
git log --graph --oneline --all
```

---

# 12. Preparando uma versão — release

As funcionalidades previstas para a versão `1.0.0` foram concluídas.

Crie uma branch de release:

```bash
git switch develop
git switch -c release/1.0.0
```

Durante uma release, devem ser priorizados:

- correções;
- ajustes de documentação;
- ajustes de versão;
- testes;
- preparação para publicação.

Não é recomendado incluir grandes funcionalidades novas.

Crie o arquivo:

```text
VERSION
```

Conteúdo:

```text
1.0.0
```

Registre:

```bash
git add .
git commit -m "chore: prepara versão 1.0.0"
```

---

# 13. Publicando a release

Com a versão validada, integre-a à `main`:

```bash
git switch main
git merge release/1.0.0
```

Crie uma tag:

```bash
git tag -a v1.0.0 -m "Versão 1.0.0"
```

Consulte:

```bash
git tag
```

Agora a mesma release deve retornar para `develop`:

```bash
git switch develop
git merge release/1.0.0
```

Depois:

```bash
git branch -d release/1.0.0
```

Visualize:

```bash
git log --graph --oneline --decorate --all
```

---

# 14. Simulando um erro em produção

Imagine que a versão `1.0.0` foi publicada e existe um erro urgente no título da página.

Como o problema está na versão em produção, a correção deve partir da `main`.

Retorne:

```bash
git switch main
```

Crie:

```bash
git switch -c hotfix/1.0.1
```

Altere:

```html
<h1>Projeto de Desenvolvimento Web</h1>
```

Registre:

```bash
git add .
git commit -m "fix: corrige título da página"
```

Atualize o arquivo `VERSION`:

```text
1.0.1
```

Registre:

```bash
git add VERSION
git commit -m "chore: atualiza versão para 1.0.1"
```

---

# 15. Finalizando o hotfix

Integre a correção à produção:

```bash
git switch main
git merge hotfix/1.0.1
```

Crie uma nova tag:

```bash
git tag -a v1.0.1 -m "Versão 1.0.1"
```

A correção também precisa chegar à `develop`:

```bash
git switch develop
git merge hotfix/1.0.1
```

Agora exclua:

```bash
git branch -d hotfix/1.0.1
```

Confira:

```bash
git log --graph --oneline --decorate --all
```

---

# 16. Prática de conflito

Crie uma nova feature:

```bash
git switch develop
git switch -c feature/novo-titulo
```

Altere:

```html
<h1>Aprendendo Git Flow</h1>
```

Registre:

```bash
git add .
git commit -m "feat: altera título principal"
```

Retorne para `develop`:

```bash
git switch develop
```

Altere a mesma linha para:

```html
<h1>Projeto Prático de Git</h1>
```

Registre:

```bash
git add .
git commit -m "refactor: modifica título da aplicação"
```

Tente integrar:

```bash
git merge feature/novo-titulo
```

O Git deverá apresentar um conflito.

O arquivo terá marcações semelhantes a:

```text
<<<<<<< HEAD
<h1>Projeto Prático de Git</h1>
=======
<h1>Aprendendo Git Flow</h1>
>>>>>>> feature/novo-titulo
```

Resolva para:

```html
<h1>Projeto Prático — Aprendendo Git Flow</h1>
```

Remova os marcadores e finalize:

```bash
git add index.html
git commit -m "fix: resolve conflito no título"
```

Depois:

```bash
git branch -d feature/novo-titulo
```

---

# 17. Visualizando o fluxo completo

Execute:

```bash
git log --graph --oneline --decorate --all
```

Observe:

- onde as features foram criadas;
- onde foram integradas;
- onde surgiu a release;
- onde surgiu o hotfix;
- quais commits possuem tags;
- como `main` e `develop` evoluíram.

---

# 18. Fluxo conceitual

## Desenvolvimento de funcionalidade

```text
develop
   |
   +---- feature/minha-feature
             |
             | desenvolvimento
             |
             v
           commit
             |
             v
          develop
```

## Release

```text
develop
   |
   +---- release/1.0.0
             |
             +--------> main
             |           |
             |           +--> tag v1.0.0
             |
             +--------> develop
```

## Hotfix

```text
main
 |
 +---- hotfix/1.0.1
            |
            +--------> main
            |           |
            |           +--> tag v1.0.1
            |
            +--------> develop
```

---

# 19. Boas práticas de commits

Evite mensagens como:

```text
alteração
mudança
teste
arrumei
final
agora vai
```

Prefira mensagens que expressem a intenção:

```text
feat: adiciona formulário de contato

fix: corrige validação do e-mail

docs: atualiza documentação

style: ajusta espaçamento do menu

refactor: reorganiza estrutura do código

test: adiciona testes do cadastro

chore: atualiza versão do projeto
```

Alguns prefixos comuns:

| Prefixo | Uso |
|---|---|
| `feat` | Nova funcionalidade |
| `fix` | Correção de erro |
| `docs` | Documentação |
| `style` | Formatação/estilo |
| `refactor` | Refatoração |
| `test` | Testes |
| `chore` | Manutenção/configuração |

---

# 20. Desafio final

Considere que a equipe recebeu a seguinte demanda:

> Criar uma seção de contato para a próxima versão do sistema.

Realize o processo completo:

1. Certifique-se de estar na `develop`;
2. Crie `feature/contato`;
3. Adicione um formulário com:
   - nome;
   - e-mail;
   - assunto;
   - mensagem;
   - botão de envio;
4. Faça pelo menos **dois commits** durante o desenvolvimento;
5. Integre a feature à `develop`;
6. Exclua a branch da feature;
7. Crie `release/1.1.0`;
8. Atualize o arquivo `VERSION`;
9. Integre a release à `main`;
10. Crie a tag `v1.1.0`;
11. Integre a release de volta à `develop`;
12. Exclua a branch de release.

Ao final execute:

```bash
git log --graph --oneline --decorate --all
```

---

# 21. Questões para reflexão

1. Qual a diferença entre `main` e `develop`?
2. Por que uma feature normalmente nasce da `develop`?
3. Por que não é recomendado desenvolver diretamente na `main`?
4. Qual a finalidade de `feature/*`?
5. Qual a finalidade de `release/*`?
6. Qual a finalidade de `hotfix/*`?
7. Por que um hotfix deve ser integrado tanto à `main` quanto à `develop`?
8. Para que servem as tags?
9. Qual a diferença entre um commit e uma tag?
10. O que pode causar um conflito?
11. Como o Git Flow ajuda uma equipe de desenvolvimento?
12. Em quais tipos de projeto esse fluxo pode se tornar excessivamente complexo?

---

# 22. Entrega

O estudante deverá entregar:

1. A pasta completa do projeto;
2. A saída do comando:

```bash
git log --graph --oneline --decorate --all
```

3. A saída de:

```bash
git branch
```

4. A saída de:

```bash
git tag
```

5. Uma explicação, com suas próprias palavras, sobre:

- `main`;
- `develop`;
- `feature`;
- `release`;
- `hotfix`;
- merge;
- conflito;
- tag.

---

# 23. Resultado esperado

Ao concluir a prática, o estudante deverá compreender que Git Flow não é apenas um conjunto de comandos, mas uma **estratégia de organização do processo de desenvolvimento**.

O fluxo resumido será:

```text
                 +--> feature/* ----+
                 |                  |
                 |                  v
main <-------- release/* <------- develop
  ^                |                ^
  |                |                |
  +--- hotfix/* ---+----------------+
  |
  +--> tags: v1.0.0, v1.0.1, v1.1.0
```

A ideia central é separar:

```text
main     = produção / versões estáveis
develop  = integração do desenvolvimento
feature  = novas funcionalidades
release  = preparação de versões
hotfix   = correções urgentes
tag      = identificação de versões publicadas
```

---

# 24. Parte online — GitHub

Até este ponto, o histórico do projeto está armazenado localmente no computador. Agora o repositório será associado ao **GitHub**, permitindo armazenar uma cópia remota, compartilhar o código e trabalhar de forma colaborativa.

## 24.1. Criando um repositório no GitHub

Acesse o GitHub:

```text
https://github.com/
```

Crie um novo repositório com o nome:

```text
pratica-git-flow
```

Como o projeto já foi criado localmente, prefira criar o repositório remoto **vazio**, sem adicionar automaticamente `README`, `.gitignore` ou licença.

Após a criação, o GitHub apresentará uma URL semelhante a:

```text
https://github.com/SEU-USUARIO/pratica-git-flow.git
```

Substitua `SEU-USUARIO` pelo seu usuário do GitHub nos comandos seguintes.

---

# 25. Associando o repositório local ao GitHub

Dentro da pasta do projeto, confira se você está realmente em um repositório Git:

```bash
git status
```

Adicione o repositório remoto:

```bash
git remote add origin https://github.com/SEU-USUARIO/pratica-git-flow.git
```

Confira:

```bash
git remote -v
```

Resultado semelhante a:

```text
origin  https://github.com/SEU-USUARIO/pratica-git-flow.git (fetch)
origin  https://github.com/SEU-USUARIO/pratica-git-flow.git (push)
```

O nome `origin` é uma convenção utilizada para identificar o repositório remoto principal.

---

# 26. Enviando a branch main

Certifique-se de estar na `main`:

```bash
git switch main
```

Envie-a para o GitHub:

```bash
git push -u origin main
```

A opção `-u` cria uma associação entre a branch local e a branch remota.

Depois disso, normalmente será suficiente executar:

```bash
git push
```

para enviar novos commits dessa branch.

---

# 27. Enviando a branch develop

A branch `develop` também deverá existir no GitHub.

Execute:

```bash
git switch develop
git push -u origin develop
```

Acesse o GitHub e verifique se estão disponíveis:

```text
main
develop
```

---

# 28. Entendendo local e remoto

Agora existem duas cópias relacionadas do projeto:

```text
Computador do estudante                    GitHub
-----------------------                    ------

main ------------------------------------> main

develop ---------------------------------> develop

feature/* -------------------------------> feature/*

        git push  --------------------->

        git pull  <---------------------

        git fetch <---------------------
```

É importante compreender que **commit e push são operações diferentes**.

```text
git commit
```

registra uma versão no repositório **local**.

```text
git push
```

envia commits locais para o repositório **remoto**.

Portanto:

```text
Arquivo alterado
      |
      v
git add
      |
      v
Staging Area
      |
      v
git commit
      |
      v
Repositório local
      |
      v
git push
      |
      v
GitHub
```

---

# 29. Publicando uma feature no GitHub

Agora será simulada uma funcionalidade desenvolvida por um membro da equipe.

Atualize primeiro a `develop`:

```bash
git switch develop
git pull origin develop
```

Crie:

```bash
git switch -c feature/contato
```

Adicione ao `index.html`:

```html
<section id="contato">

    <h2>Contato</h2>

    <form>

        <label for="nome">Nome:</label>
        <input type="text" id="nome">

        <label for="email">E-mail:</label>
        <input type="email" id="email">

        <label for="mensagem">Mensagem:</label>
        <textarea id="mensagem"></textarea>

        <button type="submit">Enviar</button>

    </form>

</section>
```

Verifique:

```bash
git status
git diff
```

Registre:

```bash
git add .
git commit -m "feat: adiciona formulário de contato"
```

Até aqui a branch existe apenas localmente.

Envie-a:

```bash
git push -u origin feature/contato
```

Agora a estrutura será semelhante a:

```text
LOCAL                         GITHUB

main                          main
develop                       develop
feature/contato ------------> feature/contato
```

---

# 30. Pull Request

Em um projeto colaborativo, uma feature normalmente não deve ser integrada diretamente à `develop` sem revisão.

No GitHub, crie um **Pull Request**:

```text
feature/contato
       |
       | Pull Request
       v
    develop
```

No Pull Request:

1. Verifique a branch de origem;
2. Verifique a branch de destino;
3. Informe um título claro;
4. Descreva o que foi desenvolvido;
5. Revise os arquivos alterados;
6. Verifique os commits;
7. Realize o merge quando a alteração estiver aprovada.

Exemplo de título:

```text
feat: adiciona formulário de contato
```

Exemplo de descrição:

```text
Implementa a seção de contato da aplicação.

Alterações:
- adiciona campo nome;
- adiciona campo e-mail;
- adiciona campo mensagem;
- adiciona botão de envio.
```

---

# 31. Atualizando o repositório local após o Pull Request

Depois que o Pull Request for integrado no GitHub, a `develop` remota estará mais atualizada que a sua `develop` local.

Execute:

```bash
git switch develop
```

Depois:

```bash
git pull origin develop
```

Agora exclua a feature local:

```bash
git branch -d feature/contato
```

Caso a branch remota ainda exista e deva ser removida:

```bash
git push origin --delete feature/contato
```

---

# 32. Git pull

O comando:

```bash
git pull
```

é utilizado para buscar alterações do repositório remoto e integrá-las à branch local.

Exemplo:

```bash
git switch develop
git pull origin develop
```

Antes de começar uma nova funcionalidade, é uma boa prática atualizar a branch base:

```bash
git switch develop
git pull
git switch -c feature/nova-funcionalidade
```

Isso reduz a possibilidade de iniciar um trabalho sobre uma versão desatualizada.

---

# 33. Git fetch

Também podemos consultar alterações remotas utilizando:

```bash
git fetch
```

O `fetch` baixa informações do remoto, mas não realiza automaticamente o merge na branch atual.

Compare:

```text
git fetch
    |
    +--> busca informações do remoto
    +--> não integra automaticamente

git pull
    |
    +--> busca alterações
    +--> integra as alterações
```

---

# 34. Clone — obtendo um projeto existente

Até agora criamos o projeto localmente e depois o enviamos ao GitHub.

Em outro computador, normalmente começamos utilizando:

```bash
git clone https://github.com/SEU-USUARIO/pratica-git-flow.git
```

Entre na pasta:

```bash
cd pratica-git-flow
```

Consulte:

```bash
git status
git branch
git remote -v
```

Para visualizar branches remotas:

```bash
git branch -a
```

Caso precise trabalhar com a `develop`:

```bash
git switch develop
```

---

# 35. Clone, pull e push

Os três comandos possuem finalidades diferentes:

| Comando | Finalidade |
|---|---|
| `git clone` | Obtém inicialmente uma cópia de um repositório |
| `git pull` | Atualiza o repositório local com alterações remotas |
| `git push` | Envia commits locais para o remoto |

Fluxo típico:

```text
Primeira vez:

GitHub
  |
  | git clone
  v
Computador


Durante o desenvolvimento:

GitHub
  |
  | git pull
  v
Computador
  |
  | alterações
  | git add
  | git commit
  |
  | git push
  v
GitHub
```

---

# 36. Enviando tags ao GitHub

Uma tag criada localmente não é necessariamente enviada automaticamente.

Confira:

```bash
git tag
```

Para enviar uma tag específica:

```bash
git push origin v1.0.0
```

Para enviar outra:

```bash
git push origin v1.0.1
```

Ou envie todas:

```bash
git push origin --tags
```

No GitHub, verifique se as versões estão disponíveis.

---

# 37. Release online

Quando uma `release/*` estiver concluída, o fluxo conceitual será:

```text
develop
   |
   v
release/1.1.0
   |
   +--------------------> main
   |                       |
   |                       v
   |                    v1.1.0
   |
   +--------------------> develop
```

Após os merges locais, envie:

```bash
git switch main
git push origin main
```

Depois:

```bash
git switch develop
git push origin develop
```

E:

```bash
git push origin --tags
```

Em equipes, esses merges também podem ser realizados por **Pull Requests**, de acordo com as regras definidas para o projeto.

---

# 38. Hotfix online

Imagine que a versão `1.1.0` está publicada e foi encontrado um problema urgente.

Atualize a `main`:

```bash
git switch main
git pull origin main
```

Crie:

```bash
git switch -c hotfix/1.1.1
```

Faça a correção.

Depois:

```bash
git add .
git commit -m "fix: corrige problema identificado em produção"
```

Publique:

```bash
git push -u origin hotfix/1.1.1
```

A branch estará disponível para revisão no GitHub.

O fluxo será:

```text
                    hotfix/1.1.1
                         |
                         |
                         v
main <-------------------+------------------> tag v1.1.1
                         |
                         v
                      develop
```

A correção deve chegar tanto à `main` quanto à `develop`, evitando que o problema reapareça em versões futuras.

---

# 39. Autenticação no GitHub

Dependendo da configuração do ambiente, o GitHub poderá solicitar autenticação ao realizar operações como:

```bash
git push
```

A autenticação pode ser realizada utilizando mecanismos suportados pelo GitHub, como credenciais configuradas no ambiente, GitHub CLI, token ou SSH.

Para verificar o endereço remoto atual:

```bash
git remote -v
```

Se o projeto utilizar HTTPS, será semelhante a:

```text
https://github.com/SEU-USUARIO/pratica-git-flow.git
```

Se utilizar SSH, será semelhante a:

```text
git@github.com:SEU-USUARIO/pratica-git-flow.git
```

> Não compartilhe senhas, tokens ou chaves privadas no repositório.

---

# 40. Arquivo .gitignore

Nem todos os arquivos devem ser enviados ao GitHub.

Crie:

```text
.gitignore
```

Exemplo:

```gitignore
# Dependências
node_modules/

# Arquivos de ambiente
.env

# IDE
.vscode/
.idea/

# Sistema operacional
.DS_Store
Thumbs.db

# Logs
*.log
```

Registre:

```bash
git add .gitignore
git commit -m "chore: adiciona gitignore"
git push
```

O `.gitignore` ajuda a evitar o versionamento de arquivos desnecessários ou sensíveis.

---

# 41. README.md

Crie um arquivo:

```text
README.md
```

Exemplo:

```markdown
# Prática Git Flow

Projeto desenvolvido durante a disciplina de Desenvolvimento Web.

## Tecnologias

- HTML
- CSS
- Git
- GitHub

## Estratégia de branches

- main
- develop
- feature/*
- release/*
- hotfix/*
```

Registre:

```bash
git add README.md
git commit -m "docs: adiciona documentação do projeto"
git push
```

---

# 42. Fluxo completo: Git + Git Flow + GitHub

O processo completo estudado nesta prática pode ser representado por:

```text
                    DESENVOLVIMENTO LOCAL

                         develop
                            |
              +-------------+-------------+
              |                           |
              v                           v
       feature/menu                feature/contato
              |                           |
              +-------------+-------------+
                            |
                            v
                         develop
                            |
                            v
                      release/1.0.0
                       /          \
                      v            v
                    main        develop
                      |
                      v
                   v1.0.0
                      |
                      v
                    GitHub
```

Em caso de erro urgente:

```text
                         main
                          |
                          v
                    hotfix/1.0.1
                     /         \
                    v           v
                  main       develop
                    |
                    v
                 v1.0.1
```

---

# 43. Rotina recomendada para uma nova funcionalidade

Antes de iniciar:

```bash
git switch develop
git pull origin develop
```

Crie:

```bash
git switch -c feature/minha-feature
```

Desenvolva e registre:

```bash
git status
git add .
git commit -m "feat: implementa minha funcionalidade"
```

Publique:

```bash
git push -u origin feature/minha-feature
```

Depois:

```text
GitHub
   |
   +--> abrir Pull Request
   |
   +--> revisar alterações
   |
   +--> merge em develop
```

Atualize o ambiente local:

```bash
git switch develop
git pull origin develop
```

Remova a branch:

```bash
git branch -d feature/minha-feature
```

---

# 44. Desafio final online

Agora realize o fluxo completo sem copiar diretamente uma sequência pronta.

## Situação

A equipe deseja lançar a versão `1.1.0` contendo uma nova página **Sobre o Projeto**.

## Tarefas

1. Atualize sua `develop` local;
2. Crie `feature/sobre`;
3. Implemente a seção;
4. Faça pelo menos dois commits;
5. Envie `feature/sobre` ao GitHub;
6. Abra um Pull Request para `develop`;
7. Revise os arquivos modificados;
8. Faça o merge;
9. Atualize sua `develop` local;
10. Crie `release/1.1.0`;
11. Atualize `VERSION`;
12. Finalize a release;
13. Atualize `main` e `develop`;
14. Crie `v1.1.0`;
15. Envie as branches e tags ao GitHub;
16. Confira o histórico completo.

Execute:

```bash
git log --graph --oneline --decorate --all
```

---

# 45. Questões finais

Responda:

1. Qual a diferença entre Git e GitHub?
2. Qual a diferença entre repositório local e remoto?
3. Para que serve `git remote`?
4. Qual a diferença entre `commit` e `push`?
5. Qual a diferença entre `clone` e `pull`?
6. Qual a diferença entre `fetch` e `pull`?
7. O que significa `origin`?
8. Para que serve um Pull Request?
9. Por que uma feature deve ser revisada antes do merge?
10. Por que `main` e `develop` possuem funções diferentes no Git Flow?
11. Quando utilizar `release/*`?
12. Quando utilizar `hotfix/*`?
13. Para que servem as tags?
14. Qual a importância do `.gitignore`?
15. Por que arquivos como `.env` não devem ser enviados para um repositório público?

---

# 46. Entrega final

O estudante deverá entregar:

- Link do repositório no GitHub;
- Projeto completo;
- `README.md`;
- `.gitignore`;
- branches `main` e `develop`;
- histórico de commits organizado;
- evidência de utilização de `feature/*`;
- pelo menos um Pull Request;
- tags das versões criadas.

Também deverá apresentar o resultado de:

```bash
git log --graph --oneline --decorate --all
```

```bash
git branch -a
```

```bash
git tag
```

```bash
git remote -v
```

## Critérios sugeridos de avaliação

| Critério | Pontuação |
|---|---:|
| Organização do repositório | 1,0 |
| Commits claros e coerentes | 1,5 |
| Uso correto de `main` e `develop` | 1,5 |
| Uso de `feature/*` | 1,0 |
| Uso de `release/*` | 1,0 |
| Uso de `hotfix/*` | 1,0 |
| Pull Request | 1,0 |
| Tags/versionamento | 1,0 |
| README e `.gitignore` | 1,0 |
| **Total** | **10,0** |

---

# 47. Síntese

Ao final da prática, o estudante deverá compreender o fluxo:

```text
Editar
  |
  v
git add
  |
  v
git commit
  |
  v
Repositório local
  |
  v
git push
  |
  v
GitHub
  |
  v
Pull Request
  |
  v
Revisão
  |
  v
Merge
```

Integrado ao Git Flow:

```text
feature/*
    |
    v
 develop
    |
    v
release/*
  /     \
 v       v
main   develop
 |
 v
tag
```

E, para problemas urgentes:

```text
      main
       |
       v
   hotfix/*
    /     \
   v       v
 main   develop
   |
   v
  tag
```

Assim, a prática percorre o ciclo completo de **Git local, estratégia de branches com Git Flow e colaboração remota utilizando GitHub**.
