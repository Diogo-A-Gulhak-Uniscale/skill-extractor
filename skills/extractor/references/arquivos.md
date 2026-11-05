# Arquivos do pacote

Pacote em `~/.skill-extractor/<tagName>/`, pasta plana. `<host>` é o `hostname` de `manifest.json`.

## Roteamento

| Pergunta | Arquivo | Tamanho típico |
| --- | --- | --- |
| "me mostra todos os domínios", panorama | `panorama.md` | < 1 KB por domínio |
| panorama com detalhe de operação | `panorama-operacional.md` | idem |
| "me manda tudo do domínio X", "me mostra o domínio X" | `<host>.chat.md` + menu de exportação da skill | ≤ 4 KB |
| resumo curto do domínio X, sem menu | `<host>.resumo.md` | ~1 KB |
| X com detalhe de operação | `<host>.resumo-operacional.md` | ~1 KB |
| quais páginas estão com 404, com problema, sem análise; "essa URL está ok?" | Grep em `<host>.paginas.jsonl` | ~1,3 KB por página; nunca ler inteiro |
| relatório visual de todos | `relatorio-panorama.html` | ~120 KB; só abrir no navegador |
| relatório visual de um domínio | `<host>.relatorio.html` | ~120 KB; só abrir no navegador |
| planilha de todas as páginas | `<host>.paginas.csv` | copiar para Downloads |
| planilha só das páginas com erro | `<host>.com-erro.csv` | idem |
| planilha do que não foi validado | `<host>.nao-validadas.csv` | idem |
| planilha do que a coleta não conseguiu ler | `<host>.limites-de-coleta.csv` | idem |
| quais domínios existem, data do dado | `manifest.json` | < 1 KB |

CSV vazio traz só o cabeçalho: significa que não há linha naquele recorte. Não é erro.

## `<host>.chat.md`

Resposta pronta para o chat: linha do domínio com a data do dado, tabela de indicadores (páginas encontradas, no ar, com erro, ainda não analisadas, liberadas para o Google, tecnologia, o que mudou, última análise) e tabela de páginas (com erro primeiro, até 20). Até 4 KB. **Não traz o menu de exportação**: a skill o acrescenta (ver `SKILL.md`, passo 6). Mostre como está, sem reescrever número nem rótulo.

## Planilhas (CSV)

As quatro planilhas (`paginas`, `com-erro`, `nao-validadas`, `limites-de-coleta`) têm colunas em português, na mesma ordem:

`Página, Caminho, Título, Situação, Tecnologia, Indexação, Última confirmação, Última tentativa, Origem, Primeira vez em, Última vez em, Atenção`

Têm BOM UTF-8 (abrem com acento correto no Excel). `com-erro.csv` substitui o antigo `problemas-reais.csv`.

## manifest.json

`schemaVersion` (1), `generatedAt` (ISO UTC), `generatedAtLabel` (Brasília, para citar), `timeZone`, `extractorCommit`, `panoramaFiles` (os arquivos de panorama) e `domains[]` com `hostname`, `lastScanLabel`, `lastValidScanAt` (ISO UTC da última análise válida do domínio, ou `null` se nunca teve), `lastScanOutcomeLabel` e `files` (os arquivos do domínio, incluindo o relatório HTML). O pacote pode ser novo e o dado de um domínio velho: confira `lastValidScanAt`. A cópia em cache está completa quando existem `manifest.json`, todos os `panoramaFiles` e todos os `domains[].files`.

## `<host>.paginas.jsonl`

Uma página por linha, JSON. Chaves:

- `url`: endereço completo da página;
- `caminho`: parte do endereço depois do host;
- `titulo`: título da página, ou `null`;
- `saude`: `available` (disponível), `problem` (problema real, status 400 ou mais na última validação confiável) ou `unvalidated` (ainda sem validação confiável);
- `coleta`: `"limitada"` quando a coleta não conseguiu ler a página (limite de coleta, não página quebrada), ou `null`;
- `negocio`: texto para negócio (`situacao`, `plataforma`, `seo`, `analise`, `analiseEm`, `origem`, `disponibilidade`, `avisos`, `atencao`, `sinais`, entre outros). Use por padrão;
- `operacional`: o mesmo com termos de operação (`statusHttp`, `tentativa`, `tentativaEm` etc.). Use só quando pedirem detalhe.

`unvalidated` não é "quebrada": a coleta ainda não conseguiu confirmar. Não diga que a página caiu.

## Exemplos de Grep

Uma URL (use o endereço exato que a pessoa deu):

    pattern: "url":"https://pos.faculdadeuniguacu.com.br/inscricoes-obrigada"
    (a aspa final é obrigatória: sem ela casaria também /inscricoes-obrigada-2)
    arquivo: <host>.paginas.jsonl

Se não achar, procure pelo caminho, também com a aspa final (home é `/`), com e sem barra final:

    pattern: "caminho":"/inscricoes-obrigada"
    pattern: "caminho":"/inscricoes-obrigada/"

Páginas com problema real (ou troque `problem` por `unvalidated`):

    pattern: "saude":"problem"
    arquivo: <host>.paginas.jsonl   (limite ~20 linhas; para contar, use output count)

Páginas de um caminho:

    pattern: "caminho":"/blog
    arquivo: <host>.paginas.jsonl

Só depois de a URL exata e o caminho (com e sem barra final) não acharem nada, a conclusão é que a página não está no inventário: diga isso, não presuma que está no ar nem fora dele.
