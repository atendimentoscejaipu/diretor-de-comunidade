# Diretor de Comunidade — CEJA Dr. Gerardo Camelo Madeira

Sistema de monitoramento e busca ativa de alunos evadidos (protótipo funcional),
com sincronização em tempo real via **Firebase Firestore**.

## Estrutura de arquivos

```
diretor-de-comunidade/
├── index.html          → aplicativo completo (HTML + CSS + JS)
├── assets/
│   └── ceja-logo.webp  → logo da escola
├── firebase.json        → configuração do Firebase Hosting
├── .firebaserc          → id do projeto Firebase (edite antes de usar)
├── firestore.rules      → regras de acesso ao banco de dados
├── .gitignore
└── README.md
```

## 1. Colocar no Firebase

### 1.1 Criar o projeto
1. Acesse https://console.firebase.google.com e clique em **Adicionar projeto**.
2. Dê um nome (ex: `diretor-de-comunidade`) e finalize a criação.

### 1.2 Ativar o Firestore
1. No menu lateral, vá em **Firestore Database** → **Criar banco de dados**.
2. Escolha o modo (produção ou teste) e a região mais próxima (ex: `southamerica-east1`).

### 1.3 Criar o app Web e obter as chaves
1. Em **Configurações do projeto → Geral → Seus apps**, clique no ícone **`</>`** (Web).
2. Registre o app e copie o objeto `firebaseConfig` gerado.
3. Abra `index.html`, localize o bloco `const firebaseConfig = { ... }` (perto do início do
   `<script type="module">`) e substitua os valores de exemplo pelos valores reais copiados.

### 1.4 Instalar o Firebase CLI e publicar
```bash
npm install -g firebase-tools
firebase login
cd diretor-de-comunidade
firebase use --add          # selecione o projeto que você criou
firebase deploy
```
Ao final, o terminal mostra a URL pública (algo como
`https://SEU_PROJETO.web.app`).

### 1.5 Regras do Firestore
O arquivo `firestore.rules` já vem com uma regra aberta (qualquer pessoa com o
link pode ler/gravar), suficiente para testar o protótipo. Antes de usar com
dados reais de alunos, implemente o **Firebase Authentication** e restrinja as
regras (há um exemplo comentado dentro do próprio arquivo). Para publicar as
regras:
```bash
firebase deploy --only firestore:rules
```

## 2. Colocar no GitHub

```bash
cd diretor-de-comunidade
git init
git add .
git commit -m "Diretor de Comunidade - versão inicial"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/SEU_REPOSITORIO.git
git push -u origin main
```

Se preferir, crie o repositório primeiro em https://github.com/new e copie a
URL exibida no lugar de `SEU_USUARIO/SEU_REPOSITORIO`.

### Publicar direto do GitHub (opcional)
Você também pode conectar o Firebase Hosting ao GitHub Actions para fazer
deploy automático a cada `push`:
```bash
firebase init hosting:github
```
O comando cria um workflow em `.github/workflows/` que publica automaticamente
no Firebase sempre que houver um push para o repositório.

## 3. Observações importantes

- **Login simulado**: as senhas de Secretária/Professores são guardadas no
  `localStorage` do navegador (não é autenticação real). Para uso em produção
  com dados reais de alunos, o ideal é migrar para o **Firebase Authentication**.
- **Sincronização em tempo real**: estudantes, professores, rematrículas e o
  Farol de Rematrícula já usam `onSnapshot` do Firestore — qualquer alteração
  salva por um usuário aparece automaticamente para os demais.
- **Exportações**: Excel/CSV e PDF são geradas no próprio navegador (sem
  serviços externos); a exportação "Word" gera um arquivo `.doc` compatível
  com o Microsoft Word.
