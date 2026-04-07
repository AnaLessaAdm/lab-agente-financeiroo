# 📊 Avaliação e Métricas

## 🧪 Como Avaliar seu Agente

A avaliação foi realizada de duas formas:

1. **Testes estruturados:** Cenários simulados com base nos dados de transações e perfil do cliente;
2. **Testes manuais:** Interações reais com usuários para validar comportamento e clareza das respostas.

---

## 📏 Métricas de Qualidade

| Métrica | O que avalia | Resultado |
|---------|--------------|----------|
| **Assertividade** | O agente respondeu corretamente com base nos dados | Alta |
| **Segurança** | Evitou inventar informações fora do contexto | Alta |
| **Coerência** | Respostas alinhadas ao perfil financeiro | Média-Alta |

---

## 🧠 Exemplos de Cenários de Teste

### ✅ Teste 1: Consulta de gastos
- **Pergunta:** "Quanto gastei com alimentação?"
- **Resposta esperada:** Valor baseado no `transacoes.csv`
- **Resultado:** ✔️ Correto  

---

### ⚠️ Teste 2: Recomendação de produto
- **Pergunta:** "Qual investimento você recomenda para mim?"
- **Resposta esperada:** Compatível com perfil do cliente
- **Resultado:** ✔️ Parcialmente correto  
> O agente respondeu corretamente, mas poderia ser mais específico

---

### ✅ Teste 3: Pergunta fora do escopo
- **Pergunta:** "Qual a previsão do tempo?"
- **Resposta esperada:** Recusar e redirecionar
- **Resultado:** ✔️ Correto  

---

### ✅ Teste 4: Informação inexistente
- **Pergunta:** "Quanto rende o produto XYZ?"
- **Resposta esperada:** Admitir falta de informação
- **Resultado:** ✔️ Correto  

---

## 📈 Resultados

### ✅ O que funcionou bem:

- O agente respeita bem o contexto fornecido
- Baixa incidência de alucinação graças às regras no prompt
- Boa capacidade de explicar gastos de forma simples
- Respostas rápidas devido ao uso do modelo local (Ollama)

---

### ⚠️ O que pode melhorar:

- Respostas poderiam ser mais analíticas (ex: comparar períodos)
- Falta de maior personalização baseada em histórico completo
- Limitação do modelo leve (1B) em respostas mais complexas
- Não há validação automática da resposta (apenas via prompt)

---

## 🚀 Métricas Avançadas (Opcional)

Mesmo sendo uma aplicação local, algumas métricas podem ser observadas:

- **Latência média:** ~1-3 segundos por resposta (modelo local)
- **Custo:** Zero (sem uso de API externa)
- **Taxa de erro:** Baixa (eventuais falhas de conexão com Ollama)

---

## 💡 Próximos Passos

- Implementar validação automática das respostas
- Melhorar análise financeira (ex: insights por categoria)
- Testar modelos maiores (ex: 7B) para maior qualidade
- Adicionar logs de interação para análise contínua
