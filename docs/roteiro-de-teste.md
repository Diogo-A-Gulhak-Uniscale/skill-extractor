# Roteiro de teste — Extractor no Claude

Uns 15 minutos. Instale antes seguindo o [README](../README.md#instalar-2-minutos) e abra o Claude
Code (`claude`) em qualquer pasta. Faça as perguntas na ordem; a coluna da direita é o que deve vir.

Na primeira pergunta o Claude Code pede para usar a skill do Extractor: aprove.

| # | Pergunte | Deve vir |
| --- | --- | --- |
| 1 | o que você sabe fazer com o Extractor? | Tabela "O que o Extractor responde aqui no chat", com os exemplos de pergunta. |
| 2 | quais domínios vocês monitoram? | "15 domínios monitorados pelo Extractor", com a data e o resultado da última análise de cada um, e o convite para pedir um domínio novo. |
| 3 | me mostra o domínio www.uniguacu.com.br | Data do dado; tabela de indicadores (páginas encontradas, no ar, com erro, ainda não analisadas, Google, tecnologia, o que mudou, última análise); até 20 páginas com as com erro primeiro; menu "Exportar" de 1 a 5. |
| 4 | 2 | "Planilha salva em …\Downloads\www.uniguacu.com.br.paginas.csv". Abra no Excel: acentos certos, uma linha por página. |
| 5 | 1 | O relatório com o timbrado da UniScale abre no navegador. |
| 6 | quais páginas do uniguacu.com.br estão com erro? | Tabela das páginas com erro (ou "Nenhuma página … está com erro"). Funciona sem o `www`. |
| 7 | a página https://www.uniguacu.com.br/ está no ar? | Ficha da página: título, situação, Google, tecnologia, última análise. |
| 8 | relatório de todos os domínios | Panorama de todos os domínios e o menu com a opção 1 (relatório com timbrado). |
| 9 | como está o site exemplo.com.br? | "O domínio exemplo.com.br não é monitorado pelo Extractor", a lista dos monitorados e como pedir a inclusão. |
| 10 | quero incluir o domínio teste.uniguacu.com.br, é para uma campanha | Link "Enviar pedido de inclusão…". Clique: abre um e-mail já preenchido para diogo.arthur@uniscale.tec.br com o domínio e o motivo. **Não precisa enviar.** |
| 11 | varre o www.uniguacu.com.br agora | Para quem não mantém o Extractor: diz que só quem mantém dispara e que a atualização automática é em dia útil, às 10h. (Quem mantém dispara uma varredura de verdade: pule esta.) |

## O que observar em todas

- A resposta usa só o que o Extractor publicou: não inventa página, número nem domínio.
- Toda resposta de domínio mostra a data do dado.
- Nenhuma resposta fala em termos técnicos (códigos internos, nomes em inglês em caixa alta).
- Cada resposta sai em poucos segundos.

## Como reportar

Para cada problema: o número da linha, o que você perguntou, o que veio (print) e o que esperava.
Mande para diogo.arthur@uniscale.tec.br.
