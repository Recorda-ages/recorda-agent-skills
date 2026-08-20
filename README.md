# recorda-agent-skills

Guia centralizado de skills para os agentes de IA do projeto Recorda.

> **Skills são instruções modulares que ensinam agentes de IA a seguir metodologias de engenharia, boas práticas de código e padrões das tecnologias do projeto.**

---

## 1. Setup do Workspace

As skills devem ficar instaladas **no diretório pai (workspace)**, um nível acima dos repositórios de código. Assim:
- O agente tem visão completa de backend e frontend ao mesmo tempo.
- As skills são instaladas uma única vez e funcionam em todos os repositórios.

### Passo a passo

```bash
# 1. Crie e entre no diretório do workspace
mkdir recorda-workspace && cd recorda-workspace

# 2. Clone os repositórios de código (se já não tiver os clonado)
git clone git@github.com:Recorda-ages/recorda-frontend.git
git clone git@github.com:Recorda-ages/recorda-backend.git
```

### Estrutura resultante

```
recorda-workspace/                  ← Abra o editor AQUI
├── .claude/skills/                 ← Skills para Claude Code (gerado automaticamente)
├── .agents/skills/                 ← Skills para Codex / Antigravity (gerado automaticamente)
├── CONTEXT.md                      ← Criado automaticamente pelo agente no primeiro /grill-with-docs
├── recorda-backend/
└── recorda-frontend/
```

> [!IMPORTANT]
> **Sempre abra o editor na raiz do `recorda-workspace/`**, nunca diretamente em `recorda-backend/` ou `recorda-frontend/`. Isso garante que o agente acerte o contexto de ambos os lados e acesse as skills instaladas.

---

## 2. Instalação das Skills

Execute os comandos abaixo **a partir da raiz do seu workspace** (`recorda-workspace/`).

O instalador perguntará para qual agente você quer instalar (Claude Code, Codex, Antigravity). Selecione o seu agente.

### 2.1. Skills de Metodologia e Produtividade (Matt Pocock)

```bash
npx -y skills add mattpocock/skills --skill grill-with-docs grilling to-spec tdd code-review diagnosing-bugs research domain-modeling codebase-design resolving-merge-conflicts handoff teach wait-what
```

### 2.2. Skills Técnicas e Boas Práticas (React Native + FastAPI)

```bash
# React Native (Performance - Callstack)
npx -y skills add callstackincubator/agent-skills --skill react-native-best-practices

# React Native / Expo (Padrões e Arquitetura - Vercel)
npx -y skills add vercel-labs/agent-skills --skill vercel-react-native-skills

# FastAPI / Python / Pytest (Backend - MindRally)
npx -y skills add mindrally/skills --skill fastapi-python
```

---

## 3. Catálogo de Skills

> **User-invoked:** Você aciona explicitamente no chat quando quiser (ex: `/grill-with-docs`, `/teach`, `/to-spec`).  
> **Model-invoked:** O agente ativa automaticamente durante o desenvolvimento quando o contexto exigir.

### Metodologia & Produtividade

#### User-invoked (Acionadas por você)

| Skill | Invocação | O que faz |
|---|---|---|
| **grill-with-docs** | User-invoked | Entrevista aprofundada sobre a task, alinhando decisões e gerando/atualizando o `CONTEXT.md` e ADRs automaticamente |
| **to-spec** | User-invoked | Transforma a conversa e os requisitos discutidos em uma especificação técnica estruturada |
| **teach** | User-invoked | Ensina e explica conceitos teóricos/práticos de forma interativa para apoiar seu aprendizado |
| **wait-what** | User-invoked | Pede para o agente reformular e re-explicar a última mensagem em linguagem simples caso a resposta tenha ficado confusa |
| **handoff** | User-invoked | Resume a sessão atual em um documento de contexto para que outro agente (ou nova sessão) continue o trabalho |

#### Model-invoked (Acionadas automaticamente pelo agente)

| Skill | Invocação | O que faz |
|---|---|---|
| **grilling** | Model-invoked | Primitivo de entrevista em rounds (usado internamente pelo `grill-with-docs` e pelo agente para tirar dúvidas) |
| **tdd** | Model-invoked | Guia o desenvolvimento orientado a testes (red-green-refactor) em fatias verticais com boas práticas de mock |
| **code-review** | Model-invoked | Review automatizado do código: verifica conformidade com os padrões do projeto e com a especificação da task |
| **diagnosing-bugs** | Model-invoked | Processo disciplinado para investigar bugs: reproduzir em teste → isolar → formular hipótese → corrigir → teste de regressão |
| **research** | Model-invoked | Pesquisa aprofundada em fontes primárias (documentação oficial, código) via sub-agente em segundo plano |
| **domain-modeling** | Model-invoked | Cria e refina o vocabulário de negócio do projeto no `CONTEXT.md` conforme o desenvolvimento avança |
| **codebase-design** | Model-invoked | Orienta a criação de módulos com interfaces públicas limpas e baixo acoplamento |
| **resolving-merge-conflicts** | Model-invoked | Auxilia a resolver conflitos de Git hunk por hunk com base na intenção real de cada mudança |

### Skills Técnicas (Best Practices)

#### Model-invoked (Acionadas automaticamente pelo agente)

| Skill | Invocação | O que faz |
|---|---|---|
| **react-native-best-practices** | Model-invoked | Otimizações de performance no React Native (FPS, renderizações desnecessárias, vazamentos de memória e tamanho de bundle) |
| **vercel-react-native-skills** | Model-invoked | Padrões de arquitetura, navegação, componentes e regras recomendadas para React Native e Expo |
| **fastapi-python** | Model-invoked | Padrões de código assíncrono em FastAPI, validações Pydantic V2, queries SQLAlchemy 2.0 e fixtures de teste com pytest |

---

## 4. Fluxo de Trabalho Recomendado

Ao pegar uma task para desenvolver, siga este fluxo:

```
1. ALINHAR & DOCUMENTAR  →  /grill-with-docs
                             Discuta a tarefa com o agente. Ele fará perguntas para alinhar
                             requisitos e criará/atualizará o CONTEXT.md automaticamente.

2. TIRAR DÚVIDAS         →  /wait-what  (se a explicação do agente ficou difícil)
                             /teach      (se você quiser aprender a fundo a tecnologia antes de codar)

3. ESPECIFICAR           →  /to-spec
                             Gere a especificação técnica resumida do que será implementado.

4. DESENVOLVER           →  Implemente o código!
                             O agente aplicará automaticamente as skills de TDD, boas práticas
                             de React Native e FastAPI durante a sessão.

5. REVISAR               →  Peça: "Faça um code review das minhas alterações"
                             O agente ativará a skill de code-review para validar conformidade.

6. PASSAR O BASTÃO       →  /handoff  (ao final do dia ou se a janela de contexto estiver cheia)
```

---

## 5. Como funciona o `CONTEXT.md`?

O `CONTEXT.md` é o **glossário ubíquo do projeto** (DDD). Ele define os termos do negócio (ex: o que é um *Deck*, *Card*, *Review Session*) para que todos os agentes e desenvolvedores usem os mesmos nomes em variáveis, rotas e tabelas.

- **Criação automática:** Você não precisa criar nem copiar este arquivo manualmente.
- Ao rodar `/grill-with-docs` pela primeira vez, o agente ativará a skill `domain-modeling`, que criará o `CONTEXT.md` na raiz do workspace assim que o primeiro termo for definido.
- O glossário continuará sendo alimentado automaticamente nas próximas sessões de discussão de tasks.
