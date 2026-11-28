# Ligar o sistema ao Firebase (passo a passo)

Você já criou o projeto **Gamboa Construttor** (plano Spark). Falta ligar 5 coisas e colar os dados no `config.js`.

## 1. Ativar o login por e-mail e senha
Firebase → **Authentication** → *Começar* → aba **Sign-in method** → **E-mail/senha** → Ativar → Salvar.
(Opcional: ativar também **Google** para quem preferir entrar com a conta Google.)

## 2. Criar o banco de dados
Firebase → **Firestore Database** → *Criar banco de dados* → local **southamerica-east1 (São Paulo)** → modo **produção** → Criar.

## 3. Publicar as regras de segurança (é aqui que o corte de contrato é imposto)
Firestore Database → aba **Regras** → apague tudo → cole o conteúdo do arquivo **firestore.rules** → **Publicar**.

## 4. Autorizar o endereço do site
Authentication → **Configurações** → **Domínios autorizados** → *Adicionar domínio* → `pedrinhopp.github.io`.

## 5. Copiar os dados do app e colar no config.js
Firebase → ⚙ **Configurações do projeto** → rolar até **Seus apps** → ícone **</>** (Web) → apelido "sistema" → *Registrar app* → copie os 4 valores de `firebaseConfig` (apiKey, authDomain, projectId, appId) e cole em **config.js**:

```js
window.SCF_CONFIG = {
  firebase: { apiKey: "...", authDomain: "...", projectId: "...", appId: "..." },
  googleClientId: "",   // ID do Google Drive (veja abaixo) — pode ficar vazio por enquanto
};
```
Esses valores identificam o projeto e não são senha; quem protege os dados são as regras do passo 3.

## 6. Publicar no GitHub
Envie para o repositório (substituindo os atuais): **index.html** (sistema do cliente), **admin.html** (Administração geral), **config.js**, **favicon.png** e os `.png`.

## 7. Criar o Administrador geral (faça logo, uma única vez)
Abra `https://pedrinhopp.github.io/Sistema-Gamboa-Construttor/admin.html` → aparece **Primeiro acesso** → crie sua conta. Depois disso essa tela some para sempre.
Dentro dele: **Nova empresa** → informe a empresa, o administrador do cliente (e-mail e senha), o prazo do contrato e, se quiser, os produtos iniciais de pré-moldados. Clique em **Gerenciar** para cadastrar usuários, ver acessos, contrato e configurações.
O cliente entra em `.../Sistema-Gamboa-Construttor/` (index.html) com o e-mail e a senha que você definiu.

## 8. Google Drive (backup do cliente)
1. console.cloud.google.com → criar projeto → **APIs e serviços → Biblioteca** → ativar **Google Drive API**.
2. **Tela de consentimento OAuth**: tipo *Externo*; adicione os e-mails dos clientes como *usuários de teste* (ou publique o app).
3. **Credenciais → Criar credenciais → ID do cliente OAuth → Aplicativo da Web**; em *Origens JavaScript autorizadas* coloque `https://pedrinhopp.github.io`.
4. Copie o ID (`...apps.googleusercontent.com`) para o `googleClientId` do config.js (vale para todos) ou para o cliente em *Gerenciar → Configurações*.

## Observações
- Redefinir senha de usuário no modo online é por **e-mail de redefinição** (o sistema envia o link).
- Qualquer pessoa com a chave do app pode tentar criar conta no Firebase Auth, mas **sem perfil cadastrado pelo Administrador geral nenhum dado é lido** (as regras bloqueiam).
- Para testar sem Firebase: acrescente `#/demo` ao endereço (dados só naquele navegador). Em `admin.html`, use `admin.html?app=master#/demo`.
