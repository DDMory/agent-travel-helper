
# AeroPlan - Agente inteligente de planejamento para voo

### Contexto

O **AeroPlan** é um assistente virtual especializado em logística pré-voo para viagens domésticas cuja função é organizar as etapas burocráticas e práticas antes do embarque, garantindo que o usuário tenha a mala adequada, os documentos certos e não perca o prazo do check-in.

Em organização de viagens, é comum as pessoas realizarem consultas em diversos dominios buscando maneiras de se preparar ao ambiente do destino de viagem, elas buscam informações como: **Tempo**, **Clima** e **indicação de objetos** que deveria levar junto fora os itens essenciais.

O AeroPlan surge como uma solução centralizada para falicitar essas pesquisas usando uma interface de chatbot para simplicidade e familiaridade, ela combina **inteligencia generativa (LLMs)** e **recuperação de informação de bases de documentos (RAG)**.

### Comunicação

O agente deve solicitar os dados necessários (origem, destino e data) caso o usuário envie informações incompletas, ou processar a solicitação diretamente se todos os parâmetros forem informados na primeira mensagem.

```mermaid
flowchart TD
    User([Usuário]) --> Chat[n8n: Chat Trigger]
    Chat --> Agent[n8n: AI Agent]
    
    Agent <--> Memory[(Memória)]
    
    Agent --> Check{Dados completos?}
    Check -- Não --> Ask[Pede dado faltante] --> User
    
    Check -- Sim --> Tools[Executa Tools]
    
    subgraph Tools [Tools no n8n]
        T1[API Clima]
        T2[RAG: Regras e Malas]
        T3[Agendador de Lembrete -48h]
    end
    
    Tools --> Agent
    Agent --> Response[Resposta Final: Clima + Mala + Check-in] --> User
```

**Critérios de Resposta**

- **Tom de voz:** Claro, direto e sem formalismo excessivo.
  
- **Nível de detalhamento:** Conciso, contendo apenas uma breve justificativa para as roupas sugeridas de acordo com o clima.
  
- **Formatação:** Estruturada estritamente em Markdown (tópicos e listas de verificação).

**Regras de Fluxo e Validação**

- **Coleta progressiva:** Se faltar qualquer dado essencial (origem, destino ou data), o agente deve perguntar objetivamente pelo dado pendente antes de acionar qualquer ferramenta.
  
- **Limite de escopo:** Se o usuário solicitar itens fora do domínio (como dicas de hotéis, passeios ou roteiros turísticos), o agente deve recusar de forma educada, reforçando que atua exclusivamente na logística de embarque pré-voo.
  
- **Retenção de contexto:** O agente deve manter a memória da conversa para permitir alterações de dados já informados (ex: mudar apenas a data ou o destino) sem reiniciar o fluxo do zero.

### Casos de teste

TODO: elabora casos de teste 