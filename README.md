# Extractor no Claude

Pergunte no chat do Claude sobre os sites da UniScale e da UniGuaçu: quais páginas existem, quais
estão no ar, quais estão com erro, quais aparecem no Google. A resposta vem pronta, em tabela, com
opção de baixar planilha ou abrir um relatório com o timbrado da UniScale.

Os dados vêm do **Extractor**, a ferramenta interna que analisa os sites em dias úteis, às 10h
(horário de Brasília). Nada acessa o site na hora da pergunta: a resposta é a última análise, e a
data dela aparece em toda resposta.

## Instalar (2 minutos)

Você precisa do **Claude Code**. Não precisa de conta no GitHub nem de login em nada além do Claude.

1. No Windows, se ainda não tiver o Git for Windows (o Claude Code usa), abra o PowerShell e rode
   `winget install Git.Git`. Depois feche e abra o terminal de novo.
2. No terminal, rode os dois comandos:

   ```
   claude plugin marketplace add Diogo-A-Gulhak-Uniscale/skill-extractor
   claude plugin install skill-extractor@skill-extractor
   ```

3. Abra o Claude Code (`claude`) e pergunte: **o que você sabe fazer com o Extractor?**

Na primeira pergunta, o Claude Code pede autorização para usar a skill do Extractor. Aprove: daí em
diante ela roda sem novas perguntas.

Dentro do Claude Code, o passo 2 também funciona como `/plugin marketplace add
Diogo-A-Gulhak-Uniscale/skill-extractor` e `/plugin install skill-extractor@skill-extractor`.

## O que perguntar

Escreva do seu jeito; estes são exemplos.

| Pergunte | O que vem |
| --- | --- |
| "quais domínios vocês monitoram?" | a lista dos domínios, com a data e o resultado da última análise |
| "me mostra o domínio www.uniguacu.com.br" | indicadores do domínio, as páginas (com erro primeiro) e o menu de exportação |
| "relatório de todos os domínios" | o panorama de todos e o relatório com timbrado |
| "quais páginas do uniguacu.com.br estão com erro?" | as páginas que devolvem erro na última análise |
| "a página https://www.uniguacu.com.br/ está no ar?" | a situação daquela página: no ar, título, Google, tecnologia |
| "essa página aparece no Google?" | se ela está liberada ou bloqueada para o Google |
| "quero incluir o domínio novo.uniguacu.com.br" | um e-mail pronto pedindo a inclusão |

O domínio pode ir com ou sem `www`, com `https://` ou com o caminho da página.

### O que cada indicador quer dizer

- **No ar**: a página respondeu normalmente na última análise.
- **Com erro**: a página devolveu erro (por exemplo, página não encontrada).
- **Ainda não analisadas**: o Extractor achou a página, mas ainda não conseguiu confirmar a situação.
- **Liberadas para o Google / Bloqueadas (noindex)**: se a página permite ou não aparecer na busca.
- **Limites de coleta**: o site limitou o acesso do Extractor naquela hora. Isso **não** quer dizer
  que a página está quebrada.

## Exportar

Depois de ver um domínio, o Claude mostra um menu numerado. Responda só com o número:

1. **Relatório com timbrado**: abre no navegador (dá para imprimir ou salvar em PDF).
2. **Planilha de todas as páginas**.
3. **Só as páginas com erro**.
4. **As ainda não analisadas**.
5. **Os limites de coleta**.

As planilhas vão para a pasta **Downloads** e abrem no Excel. No relatório de todos os domínios, a
opção é só a 1.

## Domínio que não está na lista

Pergunte "quais domínios vocês monitoram?" para ver a lista. Se o site que você precisa não está
nela, diga "quero incluir o domínio X" (pode dizer o motivo). O Claude monta um e-mail já preenchido
para quem mantém o Extractor; é só clicar no link e enviar. Depois de incluído, o domínio aparece na
atualização seguinte.

## Atualização dos dados

- Automática, em dias úteis, às 10h (Brasília).
- Se o dado tiver mais de 4 dias, a resposta começa com um aviso de atraso. Avise quem mantém o
  Extractor.
- "Varre o domínio X agora" só funciona para quem mantém o Extractor.

## Problemas comuns

| O que aparece | O que fazer |
| --- | --- |
| "não consegui ler a publicação dos dados" | Confira a internet. Na rede da empresa, peça a liberação de `github.com` e `api.github.com`. |
| "não consegui conferir quem publicou os dados" | Espere alguns minutos e pergunte de novo. |
| "a publicação mais recente não veio de Diogo-A-Gulhak-Uniscale" | Não use os dados; avise quem mantém o Extractor. |
| "os dados estão num formato novo" | Rode `claude plugin update skill-extractor@skill-extractor` e reabra o Claude Code. |
| O Claude não reconhece a pergunta ou não acha o comando do Extractor | Rode o mesmo `claude plugin update` e reabra o Claude Code. |
| O link do e-mail de pedido não abre | Mande um e-mail para diogo.arthur@uniscale.tec.br com o domínio e o motivo. |

Para reportar um problema: o que você perguntou, o que veio (um print ajuda) e o que esperava, para
diogo.arthur@uniscale.tec.br.

## Privacidade e custo

- Este repositório e os dados são públicos: são os números do inventário em linguagem de negócio, os
  mesmos que o Extractor Insights mostra sem login. Diagnóstico técnico da coleta não entra.
- O que o plugin lê vai para o Claude como parte da conversa. Os arquivos ficam em
  `~/.skill-extractor/` (dados do dia) e em `~/Downloads/` (planilhas que você pedir).
- Cada pergunta leva poucos segundos e gasta pouco da sua cota do Claude. A coleta roda numa máquina
  da UniScale, sem custo de servidor.

## Teste guiado

Roteiro com o resultado esperado de cada pergunta: [`docs/roteiro-de-teste.md`](docs/roteiro-de-teste.md).

## Para quem mantém

A publicação, o agendamento e a inclusão de domínios estão no runbook do Extractor
(`docs/operations/skill-publish.md`, no repositório do Extractor). O plugin roda um comando só,
`skills/extractor/scripts/extractor-consulta`; os arquivos do pacote estão descritos em
`skills/extractor/references/arquivos.md`.
