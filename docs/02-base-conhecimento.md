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

Existem duas possibilidades:
1. Injetar os dados diretamente no prompt (Ctrl + C, Ctrl + V)
2. Carregar os arquivos via código, conforme exemplo abaixo
```
import panda as pd
import json

#CSVs
histórico = pd.read_csv('data/historico_atendimento.csv')
transacoes = pd.read_csv('data/transacoes.csv')

#JSON
with open ('data/perfil_investidor.json', 'r', encoding='utf-8') as f:
    perfil = json.load(f)
with open ('data/produtos_financeiros.json', 'r', encoding='utf-8') as f:
    produtos = json.load(f)
```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Para simplificar, podemos simplesmente "injetar" os dados em nosso prompt, garantindo que o Agente tenha melhor contexto possível.
Lembrando que em soluções mais robustas, o ideal é que tais informações seja carregadas de maneira dinâmica a fim de ganhar maior flexibilidade.

```text
- DADOS E PERFIL DO CLIENTE (JSON - perfil_investidor.json)
{
  "nome": "João Silva",
  "idade": 32,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 5000.00,
  "perfil_investidor": "moderado",
  "objetivo_principal": "Construir reserva de emergência",
  "patrimonio_total": 15000.00,
  "reserva_emergencia_atual": 10000.00,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Completar reserva de emergência",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Entrada do apartamento",
      "valor_necessario": 50000.00,
      "prazo": "2027-12"
    }
  ]
}

- HISTORICO TRANSAÇÕES (CSV - transacoes.csv)
data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,120.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,99.00,saida
2025-10-25,Combustível,transporte,250.00,saida

- HISTORICO DE ATENDIMENTO (CSV - historico_atendimento.csv)
data,canal,tema,resumo,resolvido
2025-09-15,chat,CDB,Cliente perguntou sobre rentabilidade e prazos,sim
2025-09-22,telefone,Problema no app,Erro ao visualizar extrato foi corrigido,sim
2025-10-01,chat,Tesouro Selic,Cliente pediu explicação sobre o funcionamento do Tesouro Direto,sim
2025-10-12,chat,Metas financeiras,Cliente acompanhou o progresso da reserva de emergência,sim
2025-10-25,email,Atualização cadastral,Cliente atualizou e-mail e telefone,sim

- PRODUTOS FINANCEIROS (JSON - produtos_financeiros.json)
[
  {
    "nome": "Tesouro Selic",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% da Selic",
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva de emergência e iniciantes"
  },
  {
    "nome": "CDB Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "102% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Quem busca segurança com rendimento diário"
  },
  {
    "nome": "LCI/LCA",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "95% do CDI",
    "aporte_minimo": 1000.00,
    "indicado_para": "Quem pode esperar 90 dias (isento de IR)"
  },
  {
    "nome": "Fundo Multimercado",
    "categoria": "fundo",
    "risco": "medio",
    "rentabilidade": "CDI + 2%",
    "aporte_minimo": 500.00,
    "indicado_para": "Perfil moderado que busca diversificação"
  },
  {
    "nome": "Fundo de Ações",
    "categoria": "fundo",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil arrojado com foco no longo prazo"
  }
]
```
## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

O exemplo de contexto montado abaixo se baseia nos dados originais de base de conhecimento, mas os sitentiza deixando apenas as informações mais relevantes e otimizando assim o consumo de tokens.
Entretanto, vale lembrar que o mais importante que economizar tokens, é ter todas as informações relevantes disponíveis em seu contexto.

```
```text
==============================
CONTEXTO DO CLIENTE
==============================

DADOS DO CLIENTE
- Nome: {{nome}}
- Idade: {{idade}}
- Profissão: {{profissao}}
- Renda Mensal: {{renda_mensal}}
- Objetivo Principal: {{objetivo_principal}}
- Reserva Atual: {{reserva_emergencia_atual}}
- Patrimônio Total: {{patrimonio_total}}

RESUMO FINANCEIRO (derivado de transacoes.csv)
- Total de Receitas no mês: {{total_receitas}}
- Total de Despesas no mês: {{total_despesas}}
- Categoria com maior gasto: {{maior_categoria}}
- Média mensal de gastos: {{media_gastos}}

ÚLTIMAS TRANSAÇÕES
{{lista_transacoes_recentes}}

HISTÓRICO DE ATENDIMENTO
{{resumo_historico_atendimento}}

PRODUTOS DISPONÍVEIS NO BANCO
{{lista_produtos_financeiros}}

==============================
REGRAS DE USO DOS DADOS
==============================

1. Utilize as transações para identificar padrões de gasto e explicar de forma didática onde o cliente pode melhorar.
2. Utilize o histórico de atendimento para manter continuidade e contexto nas conversas.
3. Utilize os produtos financeiros apenas quando fizer sentido para o objetivo do cliente.
4. Priorize sempre:
   - Redução de dívidas.
   - Organização do orçamento.
   - Construção de reserva de emergência.
5. Nunca recomende produtos incompatíveis com a realidade financeira do cliente.
6. Nunca crie dados que não estejam no contexto.

==============================
ESTILO DE RESPOSTA
==============================

- Linguagem simples e amigável.
- Tom empático, como um amigo próximo.
- Explicações curtas e claras.
- Sempre oferecer um próximo passo prático.
- Evitar termos técnicos complexos.

==============================
EXEMPLO DE CONTEXTO REAL INJETADO
==============================

Dados do Cliente:
- Nome: João Silva
- Renda Mensal: R$ 5.000
- Objetivo: Construir reserva de emergência
- Reserva Atual: R$ 10.000

Resumo Financeiro:
- Total de despesas: R$ 2.488,90
- Maior gasto: Moradia
- Gasto relevante em alimentação fora de casa

Últimas Transações:
- 02/10: Aluguel - R$ 1.200
- 03/10: Supermercado - R$ 450
- 10/10: Restaurante - R$ 120
- 25/10: Combustível - R$ 250

Histórico:
- Já perguntou sobre CDB
- Acompanhou meta de reserva anteriormente

Produtos Disponíveis:
- Tesouro Selic (baixo risco)
- CDB Liquidez Diária (baixo risco)

==============================

```
