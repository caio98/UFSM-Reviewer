# UFSM-Reviewer

> **Plataforma Multi-Agente Local para Apoio à Produção e Revisão Científica Universitária**

[![Status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)](https://github.com)
[![Licença](https://img.shields.io/badge/licença-MIT-blue)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.11%2B-brightgreen)](https://python.org)
[![CrewAI](https://img.shields.io/badge/framework-CrewAI-orange)](https://crewai.com)
[![Ollama](https://img.shields.io/badge/LLMs-Ollama-black)](https://ollama.com)

Desenvolvido pelo **Grupo Verdi** — Universidade Federal de Santa Maria (UFSM), 2026.
Coordenação: Prof.ª Andrea Schwertner Charão.

---

## Sumário

- [Sobre o Projeto](#sobre-o-projeto)
- [Arquitetura](#arquitetura)
- [Stack Tecnológica](#stack-tecnológica)
- [Requisitos de Hardware](#requisitos-de-hardware)
- [Instalação](#instalação)
- [Uso](#uso)
- [Roteiro de Desenvolvimento](#roteiro-de-desenvolvimento)
- [Resultados Esperados](#resultados-esperados)
- [Equipe](#equipe)
- [Como Contribuir](#como-contribuir)
- [Referências](#referências)
- [Licença](#licença)

---

## Sobre o Projeto

O **UFSM-Reviewer** é uma plataforma multi-agente de revisão científica automatizada executada **inteiramente de forma local**, sem envio de dados a servidores externos. O sistema simula o processo de revisão por pares sobre manuscritos acadêmicos produzidos por alunos de graduação e pós-graduação, fornecendo feedback estruturado, contextualizado e pedagogicamente explicado.

### Problema que resolve

A relação orientador-orientando frequentemente não comporta revisões detalhadas e iterativas de múltiplos rascunhos. Com isso, muitos artigos chegam à submissão com deficiências evitáveis:

- Ausência de comparações com o estado da arte
- Alegações de novidade não fundamentadas na literatura recente
- Inconsistências entre objetivos e resultados
- Estrutura argumentativa frágil

### Diferenciais

| Característica | UFSM-Reviewer | Sistemas similares (ex.: ScholarPeer) |
|---|---|---|
| Público-alvo | Estudantes em formação | Pesquisadores experientes |
| Execução | 100% local (Ollama) | APIs externas (OpenAI, etc.) |
| Camada pedagógica | ✅ Explica *por que* cada crítica é relevante | ❌ Não inclusa |
| Privacidade de manuscritos | ✅ Garantida — sem envio externo | ⚠️ Dados enviados à nuvem |
| Custo de operação | Gratuito (open-source) | Dependente de APIs pagas |

---

## Arquitetura

O sistema é composto por **cinco agentes especializados**, orquestrados via CrewAI sobre modelos locais via Ollama:

```
Artigo do Aluno (PDF / DOCX / LaTeX)
         │
         ▼
┌─────────────────────────────────────┐
│  Agente 1 — Compressor de Contexto  │  gemma3:12b
│  Extrai: claims · método · evidências│
└──────────────┬──────────────────────┘
               │
       ┌───────┴────────┐
       ▼                ▼
┌──────────────┐  ┌──────────────────┐
│  Agente 2    │  │   Agente 3       │
│  Historiador │  │  Caçador de      │
│  de Domínio  │  │  Baselines       │
│  qwen3:14b   │  │  qwen3:14b       │
│  [Semantic   │  │  [Semantic       │
│   Scholar]   │  │   Scholar]       │
└──────┬───────┘  └────────┬─────────┘
       │                   │
       └─────────┬─────────┘
                 ▼
       ┌─────────────────────┐
       │   Agente 4          │
       │   Motor de QA       │
       │   Crítico           │
       │   deepseek-r1:14b   │
       └──────────┬──────────┘
                  ▼
┌────────────────────────────────────────────┐
│  Agente 5 — Gerador de Revisão             │  gemma3:12b
│  + Camada Pedagógica Explicativa           │
│  Síntese estruturada com links à literatura│
└──────────────────┬─────────────────────────┘
                   ▼
         Relatório Final ao Aluno
  ┌──────────────────────────────────┐
  │ • Pontos fortes                  │
  │ • Fraquezas explicadas           │
  │ • Baselines ausentes identificados│
  │ • Sugestões acionáveis           │
  └──────────────────────────────────┘
```

### Camada Pedagógica Explicativa

O Agente 5 transforma cada crítica técnica em **três componentes** didáticos:

1. **Descrição** — qual é o problema identificado no manuscrito
2. **Fundamentação** — por que isso é um problema científico (com referência à literatura)
3. **Sugestão** — como o aluno pode corrigi-lo concretamente

---

## Stack Tecnológica

Todas as tecnologias são **gratuitas e open-source**:

| Componente | Tecnologia | Licença |
|---|---|---|
| Execução de LLMs | [Ollama](https://ollama.com) | MIT |
| Orquestração de agentes | [CrewAI](https://crewai.com) (Python) | MIT |
| Busca de literatura | [Semantic Scholar API](https://api.semanticscholar.org) | Gratuita |
| Embeddings vetoriais | `nomic-embed-text` via Ollama | Apache 2.0 |
| Banco vetorial | PostgreSQL + pgvector | MIT |
| Interface web | Streamlit | Apache 2.0 |
| Backend API | FastAPI (Python) | MIT |
| Containerização | Docker + Docker Compose | Apache 2.0 |
| Parsing de documentos | PyMuPDF + Pandoc | Open-source |
| Controle de versão | Git + GitHub/GitLab | Gratuito |

### Modelos LLM por Agente

| Agente | Modelo | VRAM mínima |
|---|---|---|
| Compressor de Contexto (Ag. 1) | `gemma3:12b` | 8 GB |
| Historiador de Domínio (Ag. 2) | `qwen3:14b` | 10 GB |
| Caçador de Baselines (Ag. 3) | `qwen3:14b` | 10 GB |
| Motor de QA Crítico (Ag. 4) | `deepseek-r1:14b` | 10 GB |
| Gerador de Revisão (Ag. 5) | `gemma3:12b` | 8 GB |
| Embeddings | `nomic-embed-text` | 1 GB |

---

## Requisitos de Hardware

### Configuração Mínima

- GPU NVIDIA com **16 GB VRAM** (ex.: RTX 3080, RTX 4080)
- 32 GB RAM
- 500 GB SSD NVMe
- CPU: 8 cores ou mais

### Configuração Recomendada

- GPU NVIDIA com **24 GB VRAM** (ex.: RTX 3090, RTX 4090 ou A5000)
- 64 GB RAM
- 1 TB SSD NVMe
- CPU: 16 cores ou mais
- Rede: 1 Gbps (acesso interno)

### Alternativa sem GPU

É viável operar com modelos menores (`gemma3:4b`, `phi4-mini`) em CPU, com throughput reduzido (~1 artigo/hora). Adequado para fase de testes.

---

## Instalação

### Pré-requisitos

- [Docker](https://docs.docker.com/get-docker/) e Docker Compose instalados
- [Ollama](https://ollama.com) instalado e configurado com suporte CUDA (para GPU NVIDIA)
- Python 3.11+

### 1. Clone o repositório

```bash
git clone https://github.com/ufsm/ufsm-reviewer.git
cd ufsm-reviewer
```

### 2. Suba os serviços via Docker Compose

```bash
docker compose up -d
```

Isso inicializa: PostgreSQL + pgvector, Streamlit (interface) e FastAPI (backend).

### 3. Baixe os modelos via Ollama

```bash
ollama pull gemma3:12b
ollama pull qwen3:14b
ollama pull deepseek-r1:14b
ollama pull nomic-embed-text
```

> ⚠️ Certifique-se de ter espaço suficiente em disco (~30 GB para todos os modelos).

### 4. Configure as variáveis de ambiente

```bash
cp .env.example .env
# Edite .env conforme necessário (portas, paths, etc.)
```

### 5. Acesse a interface

Abra o navegador em: [http://localhost:8501](http://localhost:8501)

---

## Uso

1. Acesse a interface web via Streamlit
2. Faça upload do manuscrito (`.pdf`, `.docx` ou `.tex`)
3. Selecione a área temática do artigo (opcional, melhora a busca de literatura)
4. Aguarde o pipeline multi-agente processar o artigo
5. Receba o **Relatório de Revisão Pedagógica** com:
   - Pontos fortes identificados
   - Fraquezas com explicação científica fundamentada
   - Baselines e trabalhos relacionados ausentes
   - Sugestões acionáveis de melhoria

Todo o processamento ocorre localmente. **Nenhum dado é enviado a servidores externos.**

---

## Roteiro de Desenvolvimento

| Fase | Período | Descrição |
|---|---|---|
| **Fase 1** | Meses 1–2 | Infraestrutura, Prova de Conceito (Agentes 1 e 4, interface básica) |
| **Fase 2** | Meses 3–4 | Integração da Semantic Scholar API, Agentes 2, 3 e 5, banco vetorial |
| **Fase 3** | Meses 5–6 | Validação por simulação de revisão por pares (50 artigos, experimento pedagógico) |
| **Fase 4** | Meses 7–8 | Documentação, publicação do artigo científico, implantação institucional |

---

## Resultados Esperados

- **Plataforma funcional** implantada na infraestrutura da UFSM, acessível via interface web
- **H-Max Score médio > 4,5** nas avaliações por especialistas humanos
- **Melhoria estatisticamente significativa** na qualidade dos artigos do grupo experimental vs. grupo controle
- **Privacidade garantida**: 100% do processamento local, auditável internamente
- **Artigo científico** submetido a periódico/conferência em Informática na Educação ou IA
- **Corpus anonimizado** de revisões em português para pesquisas futuras em PLN

---

## Equipe

| Função | Perfil | Dedicação |
|---|---|---|
| Coordenadora | Prof.ª Andrea Schwertner Charão (IA/PLN) | 4 h/semana |
| Desenvolvedor Principal | Aluno de mestrado/doutorado em Computação | 20 h/semana |
| Desenvolvedor Auxiliar | Aluno de Iniciação Científica | 12 h/semana |
| Especialistas de Domínio | Professores das áreas participantes (×2) | 2 h/semana |
| Avaliadores Externos | Pesquisadores revisores para validação (×5) | Pontual (Fase 3) |

---

## Como Contribuir

Contribuições são bem-vindas! Para contribuir:

1. Faça um fork do repositório
2. Crie uma branch para sua feature (`git checkout -b feature/minha-feature`)
3. Commit suas alterações (`git commit -m 'feat: adiciona minha feature'`)
4. Push para a branch (`git push origin feature/minha-feature`)
5. Abra um Pull Request

Por favor, leia o arquivo `CONTRIBUTING.md` antes de contribuir.

---

## Referências

- Goyal, P. et al. **ScholarPeer: A Context-Aware Multi-Agent Framework for Automated Peer Review**. arXiv:2601.22638v1, 2026.
- Jin, Z. et al. **AgentReview: Exploring peer review dynamics with LLM agents**. arXiv:2406.12708, 2024.
- Weng, Y. et al. **CycleReviewer: Exploring the potential of large language models in automated peer review**. arXiv:2412.18037, 2024.
- Garg, M. K. et al. **ReviewEval: An evaluation framework for AI-generated reviews**. arXiv:2502.11736, 2025.
- Liang, W. et al. **Can large language models provide useful feedback on research papers?** NEJM AI, 2024.
- [Ollama](https://ollama.com) — Execução local de LLMs
- [Semantic Scholar API](https://api.semanticscholar.org) — Allen Institute for AI

---

## Licença

Este projeto é distribuído sob a licença **MIT**. Consulte o arquivo [LICENSE](LICENSE) para mais detalhes.

---

<p align="center">
  Desenvolvido com ❤️ pelo <strong>Grupo Verdi</strong> — UFSM, Santa Maria/RS, 2026
</p>
