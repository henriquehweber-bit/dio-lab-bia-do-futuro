# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Para que server no Amif |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores ou seja dar continuidade a um atendimento mais eficiente, amigável e coerente com as necessidades e histórico do cliente. |
| `perfil_investidor.json` | JSON | Não será usado pelo Amif, pois ele será um amigo íntimo do cliente que ajudará em sua saúde financeira. |
| `produtos_financeiros.json` | JSON | Conhecer os produtos financeiros oferecidos pela instituição bancária para que possam ser explicados e recomendados de maneira eficiente para o cliente. |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente e utilizar estas informações de forma didática para que consiga dar conselhos financeiros que atendam as reais necessidades dos clientes. |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

A base perfil_investidor.json não será utilizada, por isso foi retirada das bases de dados do Amif. Isso ocorreu devido ao perfil do cliente que o produto atendrá, uma vez que está focado em ajudar as pessoas em sua saúde financeira. 

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.
```
import panda as pd
import json

#CSVs
histórico = pd.read_csv('data/historico_atendimento.csv')
transacoes = pd.read_csv('data/transacoes.csv')

#JSON
with open ('data/produtos_financeiros.json', 'r', encoding='utf-8') as f:
    produtos = json.load(f)
```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Existem duas possibilidades: injetar os dados diretamente no prompt ou carregar os arquivos via código, conforme o exemplo abaixo

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
