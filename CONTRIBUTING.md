# Como contribuir

O apd4devs é um guia aberto mantido pela comunidade dev angolana. Qualquer ajuda conta.

## Formas de ajudar

- **Dar uma estrela** ao repositório, para que mais devs o encontrem.
- **Fazer uma pergunta**: abre uma [issue "Pergunta"](https://github.com/DumiJDev/apd4devs/issues/new?template=pergunta.yml). As perguntas aparecem no site, e as boas respostas passam para o guia.
- **Reportar uma correção ou novidade** (multa nova, mudança na lei, link partido): abre uma [issue "Correção"](https://github.com/DumiJDev/apd4devs/issues/new?template=correcao.yml) com a fonte.
- **Editar o guia diretamente**: todo o conteúdo está em [`index.html`](index.html). Faz fork, edita e abre um pull request.

## Regras do conteúdo

1. **Cita a fonte.** Cada facto novo (multa, artigo, prazo) precisa de um link: deliberação da APD, Diário da República, AngoLEX ou imprensa.
2. **Escreve para devs.** Diz o que fazer no código, no schema ou na infraestrutura, não só o que diz a lei.
3. **Português de Angola**, tom direto, tratamento por "tu".
4. **Nada de dados pessoais reais** nas issues nem nos exemplos.
5. **Não é aconselhamento jurídico.** Evita afirmações categóricas onde a lei é ambígua e diz quando é uma interpretação.

## Testar localmente

É um único ficheiro HTML estático, sem build:

```bash
python3 -m http.server 8000   # abre http://localhost:8000
```

Ao adicionar uma secção, junta-a também ao índice (`nav.toc`). Se for uma pergunta frequente, atualiza o bloco JSON-LD `FAQPage` no `<head>`.
