# skill-extractor

## O que é

Plugin do Claude Code que responde, no chat, perguntas sobre os domínios e as páginas da UniScale monitorados pelo Extractor. Exemplos: "me mostra o domínio X", "relatório de todos os domínios", "quais páginas do X estão com 404?", "essa URL está ok?". Também abre o relatório visual, entrega planilhas (CSV) e, para quem mantém o Extractor, dispara "varrer agora".

Ele só baixa e lê o pacote de dados que o Extractor publica nos dias úteis, às 10h (Brasília). Não precisa de Node nem de Docker.

## Pré-requisitos

`git` e `gh` (GitHub CLI), com `gh` autenticado numa conta que tenha acesso a este repositório. O repositório é privado.

## Passo a passo no Windows, do zero

No PowerShell:

**1. Peça acesso.** O dono do repositório precisa te adicionar em Settings, Collaborators. Sem isso o passo 4 falha ao clonar.

**2. Instale o que falta.** Se algum já estiver instalado, o comando avisa e segue.

```powershell
winget install Git.Git
winget install GitHub.cli
```

Feche e reabra o PowerShell para o `PATH` atualizar.

**3. Autentique no GitHub.**

```powershell
gh auth login
```

Responda **GitHub.com**, **HTTPS**, **sim** para "Authenticate Git with your GitHub credentials", e conclua no navegador.

**4. Instale o plugin.**

```powershell
claude plugin marketplace add Diogo-A-Gulhak-Uniscale/skill-extractor
claude plugin install skill-extractor@skill-extractor
```

O primeiro nome é o plugin, o segundo é o marketplace; ambos são `skill-extractor`.

**5. Pergunte.** Abra o Claude Code com `claude` e escreva, por exemplo:

```text
me mostra o domínio pos.faculdadeuniguacu.com.br
```

Na primeira pergunta o plugin baixa o pacote do dia para `~/.skill-extractor/`. Nas seguintes ele reaproveita, até sair uma Release nova.

No macOS o fluxo é o mesmo, com `brew install git gh` no passo 2.

## Uso

- "relatório de todos os domínios": resumo no chat, com menu para abrir o relatório com timbrado;
- "me manda tudo o que o domínio X tem": resposta pronta no chat (indicadores e páginas) e, abaixo, um menu de exportação numerado: 1 relatório com timbrado (abre no navegador), 2 planilha de todas as páginas, 3 só páginas com erro, 4 ainda não analisadas, 5 limites de coleta. Responda com o número; a planilha é copiada para Downloads e o caminho é informado;
- "quais páginas do X estão com 404?" / "essa URL está ok?": consulta o inventário do domínio;
- "varre o X agora": dispara uma nova coleta. Só funciona para quem mantém o Extractor; para os demais, a próxima atualização é no dia útil seguinte, às 10h.

Toda resposta começa com a data do dado. Se o dado tiver mais de 96 horas, a resposta avisa que está atrasado.

Por trás, a skill roda um comando só, `extractor-consulta`, que vem com o plugin: confere quem
publicou, baixa o pacote quando ele muda e devolve a resposta pronta. Por isso cada pergunta leva
poucos segundos e gasta pouco da sua cota do Claude (a skill responde no modelo Sonnet). Se o
Claude Code pedir permissão para rodar `extractor-consulta`, escolha a opção de não perguntar de
novo.

## Erros comuns

| O que aparece | O que é | O que fazer |
| --- | --- | --- |
| Falha ao clonar, ou pedido de usuário e senha | Sem acesso ao repositório, ou Git não autenticado | Refaça os passos 1 e 3 |
| `gh` não é reconhecido | GitHub CLI fora do `PATH` | Reabra o terminal; se persistir, reinstale |
| "pacote não veio do publicador oficial" | A Release não foi publicada pela conta esperada | Avise quem mantém o Extractor |
| "atualize o plugin" | O formato do pacote mudou | `claude plugin update skill-extractor@skill-extractor` |
| "varrer agora" recusado | Só quem mantém o Extractor pode disparar | Aguarde a atualização automática |
| `extractor-consulta: command not found` | Plugin desatualizado ou Claude Code sem reiniciar | `claude plugin update skill-extractor@skill-extractor` e reabra o Claude Code |

## Privacidade

O conteúdo lido é enviado à API do Claude como parte da conversa. Os arquivos ficam em `~/.skill-extractor/` e em `~/Downloads/` (CSVs que você pedir).
