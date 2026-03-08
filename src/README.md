# Código da Aplicação

Esta pasta contém o código da aplicação AMIF — Amigo Financeiro, um assistente virtual de educação financeira que utiliza IA generativa rodando localmente para responder dúvidas de clientes com base em dados estruturados.

A interface foi construída com Streamlit e o modelo de linguagem é executado localmente via Ollama.

## Estrutura Sugerida

```
Lab_DIO_BIA_Futuro/
│
├── src/
│   ├── app.py                # Aplicação principal Streamlit
│   └── requirements.txt      # Dependências da aplicação
│
├── data/
│   ├── historico_atendimento.csv
│   ├── perfil_investidor.json
│   ├── produtos_financeiros.json
│   └── transacoes.csv
│
│
├── requirements.txt          # Dependências gerais do projeto
└── README.md
```
## Descrição dos Componentes
app.py

Aplicação principal que:
 - carrega os dados do cliente
 - constrói o contexto financeiro
 - envia o prompt para o modelo
 - renderiza a interface conversacional

data/

Contém os dados simulados utilizados pelo assistente:

| Arquivo                     | Descrição                           |
| --------------------------- | ----------------------------------- |
| `perfil_investidor.json`    | Informações do cliente              |
| `produtos_financeiros.json` | Catálogo de produtos do banco       |
| `transacoes.csv`            | Histórico recente de transações     |
| `historico_atendimento.csv` | Interações anteriores com o cliente |

Esses dados são utilizados para personalizar as respostas da IA.


## Exemplo de requirements.txt

```
streamlit
pandas
requests
```
Instalação:
```pip install -r requirements.txt```

## Como Rodar

```bash
# 1 Instalar dependências
pip install -r requirements.txt

# 2️ Iniciar o modelo local
Projeto utiliza Ollama
ollama run gpt-oss:20b-cloud

# Rodar a aplicação
Na raiz do projeto:
streamlit run src/app.py

A aplicação abrirá automaticamente em:
http://localhost:8501
```
#Funcionalidades do Assistente
O agente AMIF é capaz de:
 - analisar o perfil financeiro do cliente
 - interpretar histórico de transações
 - explicar produtos financeiros
 - orientar organização financeira
 - promover educação financeira responsável

Tudo respeitando regras de:
 - segurança da informação
 - conformidade regulatória
 - transparência financeira

#Tecnologias Utilizadas
 - Python
 - Streamlit
 - Ollama
 - pandas
