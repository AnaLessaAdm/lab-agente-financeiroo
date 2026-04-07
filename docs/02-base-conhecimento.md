# 📊 Base de Conhecimento

## Dados Utilizados

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |
| `perfil_investidor.json` | JSON | Identificar perfil financeiro e tolerância a risco |
| `produtos_financeiros.json` | JSON | Apoiar sugestões de produtos compatíveis com o perfil |

---

## 🔧 Adaptações nos Dados

Os dados mockados foram adaptados para simular um cenário mais realista de análise financeira:

- Padronização de categorias de gastos (ex: alimentação, transporte, lazer)
- Inclusão de datas e valores consistentes para análise temporal
- Estruturação do perfil do investidor com atributos como:
  - tolerância a risco
  - objetivos financeiros
  - horizonte de investimento
- Simplificação dos produtos financeiros para facilitar a interpretação pelo modelo

---

## ⚙️ Estratégia de Integração

### Como os dados são carregados?

Os arquivos JSON e CSV são carregados no início da execução da aplicação (via Python) e armazenados em memória.

Esses dados são então organizados e preparados antes de serem enviados ao modelo.

---

### Como os dados são usados no prompt?

Os dados são inseridos dinamicamente no prompt enviado ao modelo, seguindo uma abordagem de **context injection**:

- Parte dos dados vai no **system prompt** (regras e comportamento)
- Parte vai no **user prompt** (contexto do cliente + pergunta)

O modelo **não acessa os arquivos diretamente**, apenas o contexto estruturado enviado.

---

## 🧠 Exemplo de Contexto Montado

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

## 💡 Observação

Toda a geração de resposta é feita localmente utilizando **Llama 3.2 via Ollama**, garantindo:

- 🔒 Privacidade dos dados do usuário
- ⚡ Baixa latência
- 💰 Custo zero com APIs externas
