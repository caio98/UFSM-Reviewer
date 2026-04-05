---
stepsCompleted: ['step-01-init', 'step-02-discovery', 'step-02b-vision', 'step-02c-executive-summary-draft']
inputDocuments: ['docs/UFSM-Reviewer.pdf']
workflowType: 'prd'
briefCount: 0
researchCount: 0
brainstormingCount: 0
projectDocsCount: 1
classification:
  projectType: 'Plataforma multi-agente com interface web'
  domain: 'edtech — formação de pesquisadores / escrita científica assistida por IA'
  complexity: 'alta'
  projectContext: 'greenfield'
  keyDifferentiator: 'Camada pedagógica explicativa + execução local com privacidade total'
  criticalTopics:
    - 'Proposta de valor tangível para o aluno além de privacidade e aprovação institucional'
    - 'Risco de qualidade dos LLMs locais + plano de mitigação/fallback'
    - 'Sustentabilidade operacional pós-projeto: hardware, governança, custo'
    - 'Métrica de aprendizado real — validar diferencial pedagógico diretamente'
  personas:
    - 'Aluno (usuário primário): submete manuscritos, recebe feedback, itera em espaço seguro'
    - 'Orientador (usuário secundário configurável): recebe relatórios de evolução por escolha própria'
    - 'Administrador institucional: configura políticas, acessa métricas agregadas anonimizadas'
  roadmap:
    mvp1: 'Revisão pedagógica com pipeline multi-agente local e ciclo de maturação protegida'
    mvp2: 'Inteligência institucional: rastreamento de submissões, taxa de aceite, Qualis, política de ciclo completo'
    mvp3: 'Aprendizado de viés por venue: perfil por evento/periódico, RAG sobre revisões históricas'
  privacyPrinciple: 'Trajetória individual pertence ao aluno; trajetória institucional pertence à UFSM'
  institutionalKPI: 'Qualis como métrica central de impacto'
---

# Product Requirements Document - UFSM-Reviewer

**Author:** Caio
**Date:** 2026-04-05

## Sumário Executivo

O **UFSM-Reviewer** é uma plataforma multi-agente de tutoria científica executada inteiramente na infraestrutura local da Universidade Federal de Santa Maria, projetada para romper o isolamento que paralisa pesquisadores em formação. O problema central não é ausência de feedback — é que alunos de graduação e pós-graduação comparam sua produção interna caótica com a produção externa aparentemente perfeita de seus pares, criando vergonha que paralisa a escrita e deteriora a relação de orientação desde os primeiros rascunhos.

A plataforma opera como **sala de ensaio científico**: o aluno interage com o sistema antes de expor seu trabalho ao orientador, recebendo revisões pedagógicas fundamentadas na sabedoria coletiva e anônima de ciclos anteriores — não apenas críticas técnicas, mas evidências de que outros pesquisadores passaram pelo mesmo ponto e avançaram. Isso normaliza o processo e substitui a paralisia por confiança ativa.

A execução 100% local via Ollama garante privacidade total de manuscritos pré-publicação — requisito institucional inegociável, não diferencial opcional. O orientador participa do ciclo de forma configurável, recebendo relatórios de evolução em vez de rascunhos brutos, o que eleva a qualidade das reuniões de orientação e reduz o atrito inicial da relação.

O impacto institucional é medido em Qualis: maior volume de produções de maior qualidade eleva o posicionamento da UFSM no cenário científico nacional, valoriza orientadores e egressos. O modelo de dados segue o princípio: **trajetória individual pertence ao aluno; trajetória institucional pertence à UFSM** — com anonimização rigorosa em todos os níveis.

### O que torna especial

O diferencial do UFSM-Reviewer não é a qualidade técnica do feedback — é a **humanização do processo de aprendizado científico**. Enquanto ferramentas genéricas de IA entregam críticas isoladas, o UFSM-Reviewer contextualiza cada apontamento com evidências do processo coletivo: frequência do problema em trabalhos similares, caminhos que outros pesquisadores tomaram, literatura que fundamenta a correção. O aluno não recebe um julgamento — recebe um mapa de onde está e para onde pode ir.

O sistema aprende com cada ciclo completo: alunos informam o resultado de suas submissões (aceite, apresentação, publicação, Qualis do veículo), alimentando um corpus institucional que torna as revisões progressivamente mais precisas e contextualmente alinhadas aos venues de interesse da instituição.

## Classificação do Projeto

| Dimensão | Valor |
|---|---|
| **Tipo** | Plataforma multi-agente com interface web (Streamlit + FastAPI + CrewAI) |
| **Domínio** | Edtech — formação de pesquisadores / escrita científica assistida por IA |
| **Complexidade** | Alta — múltiplos LLMs especializados, execução local, validação científica rigorosa |
| **Contexto** | Greenfield com proposta de pesquisa detalhada como base |
| **Usuários primários** | Alunos de graduação e pós-graduação da UFSM |
| **Usuários secundários** | Orientadores (participação configurável), Administração institucional |
| **KPI institucional** | Qualis médio das produções e taxa de aceite por programa |
| **Privacidade** | 100% local — nenhum manuscrito enviado a servidores externos |
