# Studio Model AI

Estúdio de criação de imagens com perfis de modelos, referências separadas (identidade, pose, roupa e cenário), prompt editável, geração/edição por Gemini e galeria local.

> **Segurança importante:** não use uma chave que foi compartilhada em uma conversa ou publicada. Revogue-a/rotacione-a no painel do serviço e crie uma nova chave Gemini API diretamente em https://aistudio.google.com/apikey. A chave fornecida na conversa não foi inserida neste pacote. O prefixo informado também não corresponde ao formato usual de chaves Gemini (`AIza...`), então não presuma que seja uma chave Gemini válida.
>
> **Importante:** a aplicação não finge gerar imagens. O botão de geração chama o endpoint `/api/generate`, que usa a API oficial Google Gemini. Sem `GEMINI_API_KEY`, a tela funciona, mas a geração real retorna uma mensagem pedindo a configuração da chave.

## Requisitos

- Node.js 22 ou superior (recomendado para as versões atuais do SDK Google Gen AI).
- npm 10 ou superior.
- Uma chave Gemini API: https://aistudio.google.com/apikey
- Projeto Google com acesso/faturamento conforme a modalidade de uso da API.

## Executar localmente

1. Extraia o ZIP do projeto.
2. Abra a pasta `studio-model-ai` no terminal ou editor de código.
3. Instale as dependências:

   ```bash
   npm install
   ```

4. Copie `.env.example` para `.env.local`.
5. Abra `.env.local` e informe sua chave:

   ```env
   GEMINI_API_KEY=sua_chave_google_aqui
   MAX_REFERENCE_MB=18
   ```

6. Inicie o site:

   ```bash
   npm run dev
   ```

7. Acesse `http://localhost:3000`.

**Segurança:** nunca coloque a chave em uma variável `NEXT_PUBLIC_*`, no código do navegador, em repositório público ou em mensagens. O endpoint lê `GEMINI_API_KEY` somente no servidor.

## Publicar na internet (Vercel)

**Esta publicação exige acesso à sua conta GitHub e Vercel.** Não consigo entrar nessas contas ou publicar em seu nome sem uma conexão/autorização de conta. Não compartilhe senhas nem tokens aqui.

1. Crie um repositório privado no GitHub e envie os arquivos deste projeto.
2. Entre em https://vercel.com e importe o repositório.
3. Nas configurações do projeto na Vercel, adicione a variável de ambiente `GEMINI_API_KEY` com sua chave.
4. Adicione também `MAX_REFERENCE_MB` com valor `18` (opcional; limites de payload da hospedagem podem ser menores).
5. Faça o deploy e abra o endereço gerado.

Em hospedagens serverless, há limites próprios para tamanho de requisição, memória, duração e armazenamento. Para muitos arquivos de referência ou tráfego compartilhado, use armazenamento remoto privado (por exemplo, Supabase Storage) e envie ao modelo referências recuperadas no servidor. Este pacote mantém as fotos no navegador e as envia ao endpoint quando você gera.

## Como usar

1. Abra **Minhas modelos** e cadastre um nome, uma descrição visual opcional e até cinco fotos de referência da mesma personagem.
2. Volte a **Criar imagem** e selecione a personagem cadastrada.
3. Escolha uma pose da biblioteca ou envie uma foto de pose.
4. Escolha categoria, descrição e cor da roupa; opcionalmente, envie a peça como referência.
5. Escolha cenário, iluminação, estilo, enquadramento e ângulo.
6. Escreva ou componha o prompt e revise o texto final.
7. Escolha Nano Banana 2 ou Nano Banana Pro, proporção e resolução. O Nano Banana Pro pode gerar até 4K; a disponibilidade efetiva depende do modelo, da conta e da API.
8. Clique em **Gerar imagem**. Salve o resultado na galeria para mantê-lo neste aparelho.
9. Para editar uma imagem anterior, abra-a na galeria, ajuste o prompt e use **Variação / editar**. A imagem carregada será encaminhada como referência de edição.

## Modelos usados

- `gemini-3.1-flash-image` — Nano Banana 2.
- `gemini-3-pro-image` — Nano Banana Pro.

Os nomes, disponibilidade, resolução suportada, preços, cotas e políticas dos modelos podem mudar. Consulte:

- Geração de imagem: https://ai.google.dev/gemini-api/docs/image-generation
- Modelos: https://ai.google.dev/gemini-api/docs/models
- Preços: https://ai.google.dev/gemini-api/docs/pricing

## Dados e privacidade nesta versão

- Perfis, referências e galeria ficam no IndexedDB do navegador do aparelho.
- Os dados não são sincronizados entre dispositivos e não existe login nesta versão.
- Ao limpar os dados do navegador, trocar de navegador ou aparelho, os perfis e a galeria locais podem ser perdidos. Exporte/baixe os resultados importantes.
- Quando você clica em gerar, as imagens de referência e o prompt são enviados ao Google Gemini pela API. Consulte os termos e controles de dados do provedor antes de enviar imagens pessoais.
- Não envie imagens de terceiros sem autorização. Use apenas modelos adultos e imagens com consentimento. O aplicativo não implementa mecanismos para contornar filtros ou regras do provedor.

## Limitações conhecidas / evolução recomendada

Esta entrega é uma primeira versão funcional, local-first, com geração real via API. Para transformar em uma plataforma multiusuário pública, implemente autenticação, banco de dados e armazenamento privado remoto (por exemplo Supabase), limites por conta, proteção contra abuso, controle de custos, exclusão remota de arquivos e processamento assíncrono. Isso exige credenciais e configuração do projeto de hospedagem.

A preservação de identidade é probabilística: imagens de referência claras e variadas ajudam, mas nenhum modelo garante identidade ou anatomia perfeita em todas as gerações. A API pode rejeitar solicitações conforme suas políticas.

## Estado de validação desta entrega

O código-fonte e a configuração foram inspecionados. A instalação de dependências e o build de produção não foram concluídos neste ambiente por limite de tempo; execute `npm install` e `npm run build` no ambiente de publicação e corrija eventuais avisos/erros antes de divulgar o site. Nenhuma geração real foi executada porque não foi configurada uma chave Gemini válida no servidor.

## Scripts

```bash
npm run dev    # desenvolvimento
npm run build  # compilar para produção
npm run start  # executar compilação de produção
```

## Estrutura

```text
src/
  app/
    api/generate/route.ts  # geração real no servidor via Gemini
    globals.css            # interface responsiva
    layout.tsx
    page.tsx
  components/
    studio-client.tsx      # estúdio, formulários, modelos, prompt e galeria
  lib/
    db.ts                  # persistência local IndexedDB
    types.ts               # tipos compartilhados
.env.example
```
