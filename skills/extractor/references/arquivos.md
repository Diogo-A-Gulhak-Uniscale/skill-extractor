# Arquivos do pacote

Pacote em `~/.skill-extractor/<tagName>/`, pasta plana. `<host>` é o `hostname` de `manifest.json`.

## Roteamento

| Pergunta | Arquivo | Tamanho típico |
| --- | --- | --- |
| "me mostra todos os domínios", panorama | `panorama.md` | < 1 KB por domínio |
| panorama com detalhe de operação | `panorama-operacional.md` | idem |
| "me mostra o domínio X" | `<host>.resumo.md` | ~1 KB |
| X com detalhe de operação | `<host>.resumo-operacional.md` | ~1 KB |
| quais páginas estão com 404, com problema, sem análise; "essa URL está ok?" | Grep em `<host>.paginas.jsonl` | ~1,3 KB por página; nunca ler inteiro |
| relatório visual de todos | `relatorio-panorama.html` | ~120 KB; só abrir no navegador |
| relatório visual de um domínio | `<host>.relatorio.html` | ~120 KB; só abrir no navegador |
| planilha de todas as páginas | `<host>.paginas.csv` | copiar para Downloads |
| planilha só de problemas reais | `<host>.problemas-reais.csv` | idem |
| planilha do que não foi validado | `<host>.nao-validadas.csv` | idem |
| planilha do que a coleta não conseguiu ler | `<host>.limites-de-coleta.csv` | idem |
| quais domínios existem, data do dado | `manifest.json` | < 1 KB |

CSV vazio traz só o cabeçalho: significa que não há linha naquele recorte. Não é erro.

## manifest.json

`schemaVersion` (1), `generatedAt` (ISO UTC), `generatedAtLabel` (Brasília, para citar), `timeZone`, `extractorCommit`, `domains[]` com `hostname`, `lastScanLabel`, `lastScanOutcomeLabel` e `files` (os arquivos do domínio).

## `<host>.paginas.jsonl`

Uma página por linha, JSON. Chaves:

- `url`: endereço completo da página;
- `caminho`: parte do endereço depois do host;
- `titulo`: título da página, ou `null`;
- `saude`: `available` (disponível), `problem` (problema real, status 400 ou mais na última validação confiável) ou `unvalidated` (ainda sem validação confiável);
- `negocio`: texto para negócio (`situacao`, `plataforma`, `seo`, `analise`, `analiseEm`, `origem`, `disponibilidade`, `avisos`, `atencao`, `sinais`, entre outros). Use por padrão;
- `operacional`: o mesmo com termos de operação (`statusHttp`, `tentativa`, `tentativaEm` etc.). Use só quando pedirem detalhe.

`unvalidated` não é "quebrada": a coleta ainda não conseguiu confirmar. Não diga que a página caiu.

## Exemplos de Grep

Uma URL (use o endereço exato que a pessoa deu):

    pattern: "url":"https://pos.faculdadeuniguacu.com.br/inscricoes-obrigada"
    (a aspa final é obrigatória: sem ela casaria também /inscricoes-obrigada-2)
    arquivo: <host>.paginas.jsonl

Páginas com problema real (ou troque `problem` por `unvalidated`):

    pattern: "saude":"problem"
    arquivo: <host>.paginas.jsonl   (limite ~20 linhas; para contar, use output count)

Páginas de um caminho:

    pattern: "caminho":"/blog
    arquivo: <host>.paginas.jsonl

Sem resultado no Grep da URL significa que a página não está no inventário: diga isso, não presuma que está no ar nem fora dele.
