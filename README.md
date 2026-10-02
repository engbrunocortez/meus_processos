# Meus Processos — painel pessoal SVLMAT

Painel que mostra só os processos de compra ligados a você na planilha
"EEL - Execução das Compras": aqueles em que você é o **Responsável** e aqueles
em que seu nome aparece no **Histórico** (ex.: "Repasse ao Bruno para pesquisa de preços").

O repositório guarda apenas o código. Os dados continuam na planilha privada e
chegam ao painel por um script que roda na sua conta Google, protegido por uma chave.

## 1. Criar a "ponte" (Google Apps Script) — uma vez só

1. Acesse https://script.google.com e clique em **Novo projeto**.
2. Apague o conteúdo e cole o arquivo `Code.gs`.
3. Em `CONFIG.CHAVE`, troque `TROQUE-POR-UMA-CHAVE-LONGA` por uma senha longa só sua.
4. Selecione a função **testar** e clique em **Executar**. Autorize o acesso quando o Google pedir.
   No log deve aparecer "Processos encontrados: N".
5. Clique em **Implantar → Nova implantação → Tipo: App da Web**:
   - Executar como: **Eu**
   - Quem pode acessar: **Qualquer pessoa**
6. Copie a URL que termina em `/exec`.

> "Qualquer pessoa" é necessário para o GitHub Pages conseguir buscar os dados.
> Sem a chave correta, o script responde apenas "Chave inválida".

Sempre que alterar o `Code.gs`, faça **Implantar → Gerenciar implantações → Editar → Nova versão**
para a URL continuar a mesma.

## 2. Publicar o painel no GitHub Pages

1. Crie um repositório (ex.: `meus-processos`) e envie o `index.html`.
2. Em **Settings → Pages**, escolha *Deploy from a branch*, branch `main`, pasta `/ (root)`.
3. Abra o endereço `https://SEU-USUARIO.github.io/meus-processos/`.
4. Clique em ⚙, cole a URL `/exec` e a chave. Elas ficam salvas só no seu navegador
   (repita esse passo uma vez em cada aparelho).

## Como o painel lê a planilha

- Abas lidas: **Licitações** e **Dispensas** (cabeçalho na linha 2).
- **Em aberto**: qualquer status que não seja Concluído, Cancelado, Anulado, Revogado,
  Fracassado, Deserto ou Transferido.
- **Dias parado**: contados a partir da última data do Histórico. Datas sem ano
  (ex.: `24/11`) são lidas de trás para frente: a mais recente é a última vez que
  aquela data ocorreu até hoje, e as anteriores recuam um ano quando o calendário vira.
  Cartões ficam amarelos a partir de 15 dias e vermelhos a partir de 30
  (ajuste em `ALERTA` e `CRITICO` no `index.html`).
- Valores com várias linhas (um por empenho) são somados.
- O painel se atualiza sozinho a cada 10 minutos enquanto estiver aberto.
