# Controle de PJ - IPCA

App web com login (administrador e usuário padrão), dados compartilhados e atualização em tempo real entre vários usuários.

## Como colocar online (Render, passo a passo)
1. Crie um repositório no GitHub e envie estes arquivos (a pasta `node_modules` e `data` não vão, o `.gitignore` já cuida disso).
2. Em https://render.com crie uma conta, clique em **New > Blueprint** e escolha o repositório. O arquivo `render.yaml` configura tudo (servidor Node, disco para guardar os dados e chave de sessão).
3. Aguarde o deploy. O Render dá um link `https://....onrender.com`: é o link que você passa para todos.
4. No primeiro acesso, a tela pede para criar o **administrador**. Nesse momento a base atual de colaboradores é carregada.
5. Na aba **Usuários**, o admin cadastra nome e sobrenome (o login `nome.sobrenome` é gerado sozinho) e define a senha padrão. No primeiro acesso a pessoa é obrigada a criar a própria senha.

Observação: o disco persistente exige plano pago do Render (o `render.yaml` usa `starter`). Sem disco, os dados se perdem a cada novo deploy. Alternativas como Railway e Fly.io também funcionam, desde que o `DATA_DIR` aponte para um volume persistente.

## Rodar no seu computador
```
npm install
npm start
```
Abra http://localhost:3000

## Como funciona
- `server.js`: servidor Express. Guarda tudo em `db.json` (pasta `DATA_DIR`), valida o login no servidor (senhas com scrypt, cookie assinado) e avisa todos os navegadores abertos quando algo muda.
- `public/index.html`: o aplicativo.
- Backup: o botão "Backup dos dados" baixa um JSON. Copie também o arquivo `db.json` do disco periodicamente.

## Limites
- Uma instância só (não escale para várias), pois os dados ficam em arquivo.
- Se duas pessoas editarem o mesmo colaborador ao mesmo tempo, vale a última gravação.
- Use sempre HTTPS (o Render já entrega).
