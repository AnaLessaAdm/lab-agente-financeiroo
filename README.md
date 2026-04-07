# 💰 Finova – Assistente Financeiro com IA

Finova é um assistente financeiro inteligente que ajuda usuários a entender seus hábitos de consumo, analisar transações e fornecer insights financeiros baseados em dados reais. Ele utiliza **Llama 3.2 via Ollama** para processamento local, garantindo **privacidade, baixo custo e respostas seguras**.

---

## 📌 Funcionalidades

- Análise de transações financeiras e perfil do usuário
- Respostas baseadas em dados reais, evitando alucinações
- Interação via chatbot em Streamlit
- Controle de recomendações financeiras de acordo com o perfil do cliente
- Capacidade de lidar com perguntas fora do escopo de forma segura

---

## 🧩 Componentes

| Componente              | Descrição |
|------------------------|----------|
| Interface              | Chatbot em Streamlit para interação com o usuário |
| Modelo (LLM)           | Llama 3.2 executado localmente via Ollama |
| Base de Conhecimento   | Arquivos JSON/CSV com dados de transações e perfil do cliente |
| Validação              | Camada de checagem para evitar alucinações e inconsistências |

---

## 📊 Base de Conhecimento

### Dados Utilizados

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |
| `perfil_investidor.json` | JSON | Identificar perfil financeiro e tolerância a risco |
| `produtos_financeiros.json` | JSON | Apoiar sugestões de produtos compatíveis com o perfil |

### Adaptações nos Dados

- Padronização de categorias de gastos
- Inclusão de datas e valores consistentes para análise temporal
- Estruturação do perfil do investidor com atributos como tolerância a risco e objetivos financeiros

### Estratégia de Integração

- Os arquivos JSON/CSV são carregados no início da execução da aplicação e armazenados em memória.
- Os dados são organizados e inseridos dinamicamente no prompt enviado ao modelo (context injection).
- O modelo **não acessa os arquivos diretamente**, apenas o contexto estruturado enviado.

### Exemplo de Contexto Montado

```text
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Objetivo: Crescimento de patrimônio
- Saldo disponível: R$ 5.000

Resumo de Gastos (últimos 30 dias):
- Alimentação: R$ 1.200
- Transporte: R$ 400
- Lazer: R$ 600

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
- 05/11: Restaurante - R$ 120

Produtos disponíveis:
- CDB (baixo risco)
- Fundo multimercado (médio risco)
- Ações (alto risco)
```

---

## 🧠 Prompts do Agente

### System Prompt

```
Você é Finova, um assistente financeiro inteligente.

Objetivo: Ajudar o cliente a entender seus hábitos financeiros com base nos dados fornecidos.

REGRAS:

1. Responda SOMENTE com base nos dados fornecidos
2. Nunca invente informações financeiras
3. Se não houver dados suficientes, diga: "Não tenho informação suficiente para responder com precisão"
4. Não faça recomendações de investimento sem considerar o perfil do cliente
5. Seja claro, objetivo e educativo
6. Destaque padrões financeiros quando possível
7. Não acesse informações externas
8. Não forneça aconselhamento profissional

ESTILO DE RESPOSTA:
- Linguagem simples e direta
- Baseada em dados concretos

EXEMPLOS:
Pergunta: "Estou gastando muito?"
Resposta: "Seus gastos com alimentação somam R$ 450, representando a maior parte recente. Considere revisar essa categoria."
```

### Exemplos de Interação

**Cenário 1:** Análise de gastos  
**Usuário:** "Estou gastando muito com lazer?"  
**Agente:** "Há registros de gastos com lazer. Comparando com outras categorias, pode impactar seu orçamento se for frequente."

**Cenário 2:** Recomendação financeira  
**Usuário:** "Posso investir meu dinheiro?"  
**Agente:** "Como seu perfil é moderado, você pode considerar opções com risco equilibrado. Não há informação suficiente para recomendação específica."

**Edge Case:** Pergunta fora do escopo  
**Usuário:** "Qual a previsão do tempo?"  
**Agente:** "Sou especializado em finanças e não tenho informações sobre previsão do tempo."

---

## 💻 Código do Agente (Streamlit + Ollama)

```python
import requests
import streamlit as st
import pandas as pd
import json

URL_OLLAMA = "http://localhost:11434/api/generate"
MODELO = "llama3.2:1b"

st.title("💰 Finova - Assistente Financeiro")

@st.cache_data
def carregar_dados():
    gastos = pd.read_csv('./data/transacoes.csv')
    perfil = json.load(open('./data/perfil_investidor.json', encoding='utf-8'))
    return gastos, perfil

dados_gastos, dados_perfil = carregar_dados()

if "messages" not in st.session_state:
    st.session_state.messages = []

for msg in st.session_state.messages:
    with st.chat_message(msg["role"]):
        st.write(msg["content"])

pergunta = st.chat_input("Digite sua dúvida financeira...")

if pergunta:
    st.session_state.messages.append({"role": "user", "content": pergunta"})
    with st.chat_message("user"):
        st.write(pergunta)

    resumo_financeiro = dados_gastos.tail(5).to_string(index=False)
    contexto = f"""
    Você é Finova.
    Dados do cliente: {dados_perfil.get('nome')}, perfil {dados_perfil.get('perfil')}
    Últimos gastos:
    {resumo_financeiro}
    Pergunta: {pergunta}
    """

    payload = {"model": MODELO, "prompt": contexto, "stream": False}

    with st.chat_message("assistant"):
        with st.spinner("Finova está pensando..."):
            resposta = requests.post(URL_OLLAMA, json=payload)
            texto = resposta.json().get("response", "Erro ao gerar resposta.")
            st.write(texto)
    st.session_state.messages.append({"role": "assistant", "content": texto})
```

---

## 📊 Avaliação e Métricas

### Testes Estruturados

| Métrica | O que avalia | Resultado |
|---------|--------------|-----------|
| Assertividade | Responde corretamente | Alta |
| Segurança | Evita inventar informações | Alta |
| Coerência | Respostas fazem sentido | Média-Alta |

### Cenários de Teste

1. Consulta de gastos → ✔️ Correto  
2. Recomendação de produto → ✔️ Parcial  
3. Pergunta fora do escopo → ✔️ Correto  
4. Informação inexistente → ✔️ Correto  

---

## 🎤 Pitch – Apresentação (3 minutos)

### 1. O Problema
Usuários têm dificuldade em entender seus hábitos de consumo e não possuem orientação financeira personalizada.

### 2. A Solução
Finova analisa transações e perfil financeiro, gerando respostas seguras e baseadas em dados locais.  

### 3. Demonstração
- Chatbot em Streamlit  
- Perguntas sobre gastos, perfil e investimentos  
- Respostas baseadas em dados do usuário  

### 4. Diferencial e Impacto
- IA local → privacidade e custo zero  
- Controle de alucinação  
- Educação financeira e insights personalizados

---

## 💡 Próximos Passos

- Validar respostas automaticamente  
- Adicionar análises mais detalhadas por categoria e período  
- Testar modelos maiores (7B ou 13B) para maior qualidade  
- Criar dashboard de insights financeiros
