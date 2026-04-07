# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

A Finova é uma inteligência artificial que transforma dados bancários "frios" (como o seu extrato) em decisões inteligentes.
- Fim do "Dinheiro Parado": Ela olha o seu perfil e avisa: "Ei, você tem R$ 500 dando bobeira na conta, por que não coloca no CDB que rende mais?"
- Alerta de Gastos: Ela percebe se você exagerou no iFood ou no lazer antes do mês acabar, agindo como um freio de mão preventivo.
- Tradução do "Economês": Em vez de falar siglas difíceis, ela explica de um jeito simples o que você deve fazer com o seu dinheiro.

### Solução
> Como o agente resolve esse problema de forma proativa?

A Finova resolve o problema analisando continuamente os arquivos de dados locais. Ela utiliza Gatilhos de Insight para alertar sobre desvios no orçamento e sugerir produtos de investimento que combinam com o perfil de risco do usuário, antes mesmo do dinheiro ficar desvalorizado na conta corrente

### Público-Alvo
> Quem vai usar esse agente?

SPoupadores Iniciantes: Pessoas que têm dinheiro parado na conta corrente e não sabem por onde começar a investir.

---

## Persona e Tom de Voz

### Nome do Agente
FINOVA

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

Consultiva, Educativa e Proativa.
A Finova se comporta como um especialista financeiro que não apenas guarda o seu dinheiro, mas ensina você a lidar com ele. Ela é atenciosa aos detalhes (como aumentos de gastos) e encorajadora quando o assunto é começar a investir.

### Tom de Comunicação
> Formal, informal, técnico, acessível?

Acessível e Seguro.
O tom é profissional (para passar confiança, já que lida com dinheiro), porém utiliza uma linguagem simples e direta. Evita termos técnicos complexos sem explicação, focando sempre na clareza e na utilidade da informação.

### Exemplos de Linguagem
-Saudação: "Olá! Sou a Finova, sua assistente para uma vida financeira mais inteligente. Notei algumas oportunidades nos seus dados de hoje, vamos conferir?"

-Confirmação: "Entendido! Analisando seu perfil de investidor e seu histórico de transações agora mesmo para te dar a melhor sugestão."

-Erro/Limitação: "No momento, meu acesso está restrito aos dados locais fornecidos. Não consigo realizar transações externas, mas posso te ajudar a planejar seu próximo passo com o que temos aqui!"

---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Cliente] -->|Pergunta via Streamlit| B[Interface]
    B -->|Busca contexto| D[Base de Dados CSV/JSON]
    D -->|Dados de transacoes e perfil| C[LLM Llama 3.2 via Ollama]
    C -->|Gera insight financeiro| E[Validacao de Seguranca]
    E -->|Resposta personalizada| F[Resposta no Chat]
```

## 🧩 Componentes

| Componente              | Descrição |
|------------------------|----------|
| Interface              | Chatbot em Streamlit para interação com o usuário |
| Modelo (LLM)           | Llama 3.2 executado localmente via Ollama |
| Base de Conhecimento   | Arquivos JSON/CSV com dados de transações e perfil do cliente |
| Validação              | Camada de checagem para evitar alucinações e inconsistências |

---

## 🔐 Segurança e Anti-Alucinação

### Estratégias Adotadas

- O agente responde **apenas com base nos dados fornecidos localmente**
- Não há envio de dados para APIs externas (processamento local via Ollama)
- Quando não há informação suficiente, o agente **admite incerteza**
- Não realiza recomendações financeiras sem dados completos do perfil
- As respostas são baseadas em dados estruturados (JSON/CSV)

---

### ⚠️ Limitações Declaradas

O agente **não faz**:

- Acesso à internet ou dados em tempo real
- Previsões financeiras garantidas ou promessas de retorno
- Recomendações de investimento sem perfil detalhado do cliente
- Substituição de aconselhamento profissional (consultor financeiro)
- Interpretação de dados incompletos como conclusivos
- Execução de ações reais (ex: movimentar dinheiro, realizar investimentos)

---
