# skill-extractor

## O que é

Plugin do Claude Code que responde, no chat, perguntas sobre os domínios e as páginas da UniScale monitorados pelo Extractor. Exemplos: "me mostra o domínio X", "relatório de todos os domínios", "quais páginas do X estão com 404?", "essa URL está ok?". Também abre o relatório visual, entrega planilhas (CSV) e, para quem mantém o Extractor, dispara "varrer agora".

Ele só baixa e lê o pacote de dados que o Extractor publica nos dias úteis, às 10h (Brasília). Não precisa de Node nem de Docker.

## Pré-requisitos

Só o Claude Code. Não precisa de conta no GitHub, de login nem de acesso a repositório. No Windows,
o Claude Code usa o Git for Windows; se ele ainda não estiver instalado, rode `winget install Git.Git`
no PowerShell e reabra o terminal.

## Instalação

No terminal:

```powershell
claude plugin marketplace add Diogo-A-Gulhak-Uniscale/skill-extractor
claude plugin install skill-extractor@skill-extractor
```

O primeiro nome é o plugin, o segundo é o marketplace; ambos são `skill-extractor`. Dentro do
Claude Code dá no mesmo: `/plugin marketplace add Diogo-A-Gulhak-Uniscale/skill-extractor` e
`/plugin install skill-extractor@skill-extractor`.

No Cowork o plugin ainda não foi testado.

Depois, abra o Claude Code com `claude` e escreva, por exemplo:

```text
me mostra o domínio pos.faculdadeuniguacu.com.br
```

Na primeira pergunta o plugin baixa o pacote do dia para `~/.skill-extractor/`. Nas seguintes ele reaproveita, até sair uma Release nova.

## Uso

- "quais domínios vocês monitoram?": lista os domínios do inventário, com a última análise de cada um;
- "quero incluir o domínio X": monta um e-mail já preenchido para quem mantém o Extractor; é só clicar no link e enviar. Depois de incluído, o domínio aparece na atualização seguinte;
- "relatório de todos os domínios": resumo no chat, com menu para abrir o relatório com timbrado;
- "me manda tudo o que o domínio X tem": resposta pronta no chat (indicadores e páginas) e, abaixo, um menu de exportação numerado: 1 relatório com timbrado (abre no navegador), 2 planilha de todas as páginas, 3 só páginas com erro, 4 ainda não analisadas, 5 limites de coleta. Responda com o número; a planilha é copiada para Downloads e o caminho é informado;
- "quais páginas do X estão com 404?" / "essa URL está ok?": consulta o inventário do domínio;
- "varre o X agora": dispara uma nova coleta. Só funciona para quem mantém o Extractor; para os demais, a próxima atualização é no dia útil seguinte, às 10h.

Toda resposta começa com a data do dado. Se o dado tiver mais de 96 horas, a resposta avisa que está atrasado.

Por trás, a skill roda um comando só, `extractor-consulta` (em `skills/extractor/scripts/`), que vem com o plugin: confere quem
publicou, baixa o pacote quando ele muda e devolve a resposta pronta. Por isso cada pergunta leva
poucos segundos e gasta pouco da sua cota do Claude. Na primeira
pergunta o Claude Code pede para usar a skill do Extractor: aprove, e o comando roda sem novas
perguntas.

## Erros comuns

| O que aparece | O que é | O que fazer |
| --- | --- | --- |
| "não consegui ler a publicação dos dados" | Sem internet, ou a rede bloqueia `github.com` | Confira a conexão; na rede da empresa, peça a liberação de `github.com` e `api.github.com` |
| "a publicação mais recente não veio de Diogo-A-Gulhak-Uniscale" | A Release não foi publicada pela conta esperada | Avise quem mantém o Extractor |
| "atualize o plugin" | O formato do pacote mudou | `claude plugin update skill-extractor@skill-extractor` |
| "varrer agora" recusado | Só quem mantém o Extractor pode disparar | Aguarde a atualização automática |
| O Claude não acha o comando do Extractor | Plugin desatualizado ou Claude Code sem reiniciar | `claude plugin update skill-extractor@skill-extractor` e reabra o Claude Code |

## Privacidade

Este repositório e os dados que ele publica são públicos: são os números do inventário em linguagem
de negócio, os mesmos que o Extractor Insights mostra sem login. Diagnóstico técnico da coleta não
entra no pacote.

O conteúdo lido é enviado à API do Claude como parte da conversa. Os arquivos ficam em `~/.skill-extractor/` e em `~/Downloads/` (CSVs que você pedir).
