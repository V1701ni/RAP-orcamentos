[README (1).md](https://github.com/user-attachments/files/35495179/README.1.md)
<div align="center">

# RAP Orçamentos

**App web para criar, enviar e acompanhar orçamentos e pedidos de aço.**

[![Acessar o app](https://img.shields.io/badge/Acessar_o_app-1D3F8F?style=for-the-badge&logo=render&logoColor=white)](https://rap-representa-es-or-amentos.onrender.com)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Firebase](https://img.shields.io/badge/Firebase-DD2C00?style=for-the-badge&logo=firebase&logoColor=white)
![PWA](https://img.shields.io/badge/PWA-5A0FC8?style=for-the-badge&logo=pwa&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=for-the-badge&logo=playwright&logoColor=white)

Projeto real, em uso no dia a dia da **RAP Representações**, representação comercial de aço em Curitiba e região metropolitana.

</div>

---

## Sobre

O representante monta o orçamento no celular ou no computador, gera um PDF com a identidade visual da empresa e envia pelo WhatsApp em poucos toques. Quando o cliente fecha, o mesmo orçamento vira pedido.

Tudo fica salvo no aparelho, funciona sem internet e sincroniza com a nuvem para aparecer em todos os aparelhos do usuário.

### O problema

Os orçamentos eram feitos à mão e enviados por WhatsApp e e-mail, sem padrão visual, sem histórico e sem controle do que estava em aberto. O usuário principal é uma pessoa mais velha, então o app precisava de letras grandes, poucos passos e nenhum caminho onde ele pudesse se perder.

---

## Telas

| Aba | Para que serve |
|---|---|
| **Início** | Resumo do mês, orçamentos vencendo e atalho para emitir pedidos. As configurações ficam na engrenagem do topo |
| **Orçamentos** | Lista com busca, filtros por período e situação |
| **Pedidos** | Vendas fechadas, com o que falta emitir e o que já foi enviado |
| **Clientes** | Cadastro e histórico de cada cliente, mesmo sem orçamento |
| **Relatório** | Últimos 12 meses por origem do cliente e motivo de perda |

---

## Funcionalidades

<details open>
<summary><b>Orçamentos</b></summary>

- Cliente com CNPJ ou CPF validado (inclusive o **CNPJ alfanumérico**, em vigor desde julho de 2026), telefone e cidade da região.
- Sugestão de clientes já atendidos, com os dados preenchidos ao escolher.
- Produtos por grupo (chapas e bobinas, perfis, tubos, barras, telhas, corte), com busca sem acento e "usados recentemente".
- Em cada item, o app lembra o último preço e as medidas já usadas para aquele produto.
- Preço com até 4 casas decimais, porque aço é vendido por kg.
- Desconto por item ou no total, em % ou R$, com contas em centavos para a soma sempre bater.
- Frete CIF, FOB ou taxa de entrega.
- Condições comerciais prontas: pagamento, prazo, validade e fornecedor.
- Duplicar, criar revisão (`2026-014-R1`) ou começar outro orçamento para o mesmo cliente.
- **Colar pedido do cliente**: o texto que o cliente mandou no WhatsApp vira itens do orçamento.

</details>

<details>
<summary><b>Calculadora de peso</b></summary>

Dentro do item, calcula o peso do aço (7,85 kg/dm³) com todas as medidas em mm:

- chapas, barras e tubos pelas fórmulas de cada formato;
- perfis dobrados (U, L, Z, cartola): `(soma das medidas − 4 × espessura) × espessura × comprimento × 7,85 ÷ 1.000.000`.

Nos perfis, o app avisa quando a medida é de **uma aba só**, para evitar erro na conta.

</details>

<details>
<summary><b>Pedidos</b></summary>

- O pedido sai de um orçamento fechado, sem digitar nada de novo.
- Numeração própria de pedido, separada da dos orçamentos.
- PDF de pedido no mesmo padrão visual.
- Aviso quando o orçamento muda depois que o pedido foi emitido.

</details>

<details>
<summary><b>PDF profissional</b></summary>

- Gerado no navegador com **jsPDF**, com as fontes da marca embutidas.
- Logo, dados do cliente, tabela com paginação automática, condições, totais, peso total e campo opcional de aceite.
- Tela **Confira antes de gerar** para revisar tudo.
- Envio pelo WhatsApp com a Web Share API. No computador, o PDF é baixado.

</details>

<details>
<summary><b>Acompanhamento</b></summary>

- Situação de cada orçamento: em aberto, fechado ou perdido, com o motivo da perda.
- Etiquetas "Vence amanhã" e "Vencido" e botão **Cobrar retorno pelo WhatsApp** com mensagem pronta.
- Resumo do mês: orçado, fechado e taxa de fechamento.
- Exportação para Excel (CSV com `;` e acentos corretos).

</details>

<details>
<summary><b>Segurança dos dados</b></summary>

- Lixeira com 30 dias para recuperar o que foi excluído.
- Cópia de segurança em arquivo, com lembrete periódico.
- Migrações por versão: atualizar o app nunca apaga dados antigos.
- Sincronização com a nuvem, explicada abaixo.

</details>

<details>
<summary><b>Uso no celular</b></summary>

- Instalável (PWA), com ícone próprio e tela cheia.
- Abre sem internet, com service worker *network-first*.
- O botão voltar do Android fecha só a janela aberta.
- Campos que avançam com Enter e opção de letra maior.

</details>

---

## Tecnologias

| Parte | Tecnologia |
|---|---|
| Interface | HTML, CSS e JavaScript puro, sem framework |
| PDF | jsPDF, com Barlow e IBM Plex Sans Condensed embutidas |
| Dados no aparelho | `localStorage` com migrações versionadas |
| Nuvem | Firebase Authentication (e-mail e senha) e Cloud Firestore (*lite*) |
| Empacotamento | esbuild, com só as funções do Firebase usadas (cerca de 220 KB) |
| Offline e instalação | Web App Manifest e Service Worker |
| Testes | Playwright, emulador do Firebase Authentication e nuvem simulada |
| Hospedagem | Render (site estático) |

O app inteiro é **um único arquivo HTML**. Fontes, logo, jsPDF e Firebase vão embutidos, então ele não depende de CDN e abre sem conexão.

---

## Sincronização

O app é *offline-first*: tudo é salvo no aparelho e a nuvem sincroniza por cima.

| Etapa | Como funciona |
|---|---|
| **Salvar** | Cada orçamento tem o campo `alterado`, com a data e a hora da última mudança |
| **Baixar** | Cada documento recebe um `serverTimestamp()` (`sinc`). O aparelho busca só o que mudou depois do último lido (`where('sinc', '>', marca)`), sem depender do relógio de cada aparelho |
| **Enviar** | O campo `_sinc` guarda a versão que já está na nuvem. Se for diferente de `alterado`, sobe. Sem internet, fica pendente para a próxima tentativa |
| **Conflitos** | Vale a versão mais recente (*last-write-wins*). Na numeração, vale sempre o maior valor |
| **Números repetidos** | Se dois aparelhos offline criam o mesmo número, o que ainda não foi enviado ao cliente pega o próximo livre |
| **Quando** | Ao abrir o app, ao voltar para ele, ao reconectar e a cada minuto |

<details>
<summary><b>Regras do Firestore e estrutura dos dados</b></summary>

Cada usuário só lê e grava os próprios dados:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /usuarios/{uid}/{documento=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

```
usuarios/{uid}/orcamentos/{id}   um documento por orçamento
usuarios/{uid}/dados/geral       configurações, produtos, preços e numeração
```

</details>

---

## Arquivos

```
index.html                 o app completo (interface, PDF, dados e sincronização)
orcamentos.webmanifest     nome, ícones e modo tela cheia
sw-orcamentos.js           service worker (abre sem internet)
icone-orcamentos-*.png     ícones (180, 192, 512 e "maskable")
```

---

## Como publicar

1. Coloque os arquivos na raiz de uma hospedagem estática (Render, GitHub Pages ou Netlify).
2. Para usar a nuvem, crie um projeto no Firebase:
   1. ative o login por **e-mail e senha**;
   2. crie o **Firestore** e publique as regras acima;
   3. registre um app da Web e coloque o `firebaseConfig` no app.
3. Abra o site, toque em **Entrar** e use o e-mail e a senha criados no Firebase.
4. No celular, use **Adicionar à tela inicial**.

Sem Firebase, o app funciona normalmente, salvando só no aparelho.

---

## Testes

Testes automatizados com **Playwright**, em celular e computador simulados:

- migração de versões antigas sem perder dados, com o PDF igual ao de antes;
- fluxo completo de orçamento e pedido, com o voltar do Android, a lixeira e a numeração;
- contas em centavos, preço com 4 casas e calculadora de peso;
- PDF com 30 itens, e-mails longos e observações;
- sincronização entre dois aparelhos, offline, com conflitos e números repetidos;
- login real do Firebase no emulador oficial;
- telas de 360 a 1366 px sem rolagem lateral e abertura sem internet.

---

<div align="center">

Desenvolvido por **Vinicius Lima Pereira**, estudante de Sistemas de Informação na Universidade Positivo, Curitiba (PR).

</div>
