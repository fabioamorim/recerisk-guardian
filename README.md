<<<<<<< HEAD
# recerisk-guardian
Framework de prompt de IA para gestão de riscos em corridas de rua. Analisa dados de inscritos, altimetria e histórico de eventos para dimensionar recursos e mapear zonas de risco. Artefato do TCC — Centro Universitário Facens (2026).
=======
# 🏃 RaceRisk Guardian
### Framework de IA para Gestão de Riscos em Corridas de Rua

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![TCC Facens](https://img.shields.io/badge/TCC-Facens%202025-orange)](https://www.facens.br)
[![Status](https://img.shields.io/badge/status-pendente-brightgreen)]()

> Artefato de pesquisa do TCC *"Inteligência Artificial aplicada à Gestão de Riscos em Projetos de Corridas de Rua"*, desenvolvido no Centro Universitário Facens — Sorocaba/SP (2025/2026).

---

## 👥 Autores

| Nome | Papel |
|---|---|
| Cesar Moraes | Autor |
| Fabio Amorim | Autor |
| Renan Britoli | Autor |
| Rafael Vulcani | Autor |
| Vycenzo Martinatti | Autor |
| Prof. Tiago Scala | Orientador |

**Instituição:** Centro Universitário Facens — Sorocaba, SP, Brasil  
**Curso:** Gestão de Projetos  
**Publicação:** Janeiro de 2026

---

## 📋 Sumário

- [O que é este projeto?](#-o-que-é-este-projeto)
- [Como funciona?](#-como-funciona)
- [Estrutura do repositório](#-estrutura-do-repositório)
- [Guia Rápido — Arquivos de exemplo](#-guia-rápido--usando-com-os-arquivos-de-exemplo)
- [Guia Avançado — Seus próprios dados](#-guia-avançado--usando-com-seus-próprios-dados)
- [Detalhamento dos arquivos de prompt](#-detalhamento-dos-arquivos-de-prompt)
- [Preparando seus arquivos de dados](#-preparando-seus-arquivos-de-dados)
- [Perguntas e problemas sugeridos](#-perguntas-e-problemas-sugeridos)
- [Limitações e boas práticas](#-limitações-e-boas-práticas)
- [Como citar](#-como-citar)
- [Licença](#-licença)

---

## 🎯 O que é este projeto?

Este repositório disponibiliza um **Framework de Agente de IA** para apoiar organizadores de corridas de rua na **gestão de riscos operacionais**. O framework é composto por dois arquivos de prompt estruturados que, quando fornecidos a um modelo de linguagem (LLM), o configuram como um especialista capaz de:

- Analisar dados históricos de resultados e perfil dos inscritos
- Identificar zonas de risco no percurso com base em altimetria
- Dimensionar recursos humanos e médicos (staffs, bombeiros, ambulâncias)
- Propor estratégias de largada seguras (largada por ondas)
- Gerar recomendações fundamentadas em dados reais do evento

O framework foi desenvolvido como estudo de caso com dados de um evento real de **~3.500 participantes** em Sorocaba/SP e está integralmente documentado no artigo científico incluído neste repositório.

---

## ⚙️ Como funciona?

O framework opera em duas camadas de prompt que, juntas, configuram um agente especializado:

```
┌──────────────────────────────────────────────────────────┐
│  CAMADA 1 — Perfil do Agente  (01-perfil-ia.txt)          │
│                                                           │
│  Define identidade, expertise e metodologia do agente.   │
│  Fundamentado em PMBoK 7ª ed. e ISO 31000.               │
│  Genérico: funciona para qualquer corrida sem alteração.  │
└──────────────────────────┬───────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────┐
│  CAMADA 2 — Inputs de Contexto  (02-inputs.txt)           │
│                                                           │
│  Define os dados do evento, os problemas a resolver      │
│  e o formato de entrega das respostas.                   │
│  Específico: deve ser adaptado para cada evento.          │
└──────────────────────────┬───────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────┐
│  ARQUIVOS DE DADOS DO EVENTO                              │
│                                                           │
│  Resultados históricos · Inscritos · Altimetria          │
│  Mapa do percurso · Imagens de organização               │
└──────────────────────────────────────────────────────────┘
```

O modelo processa tudo em **5 fases encadeadas**: Coleta e Validação → Análise Quantitativa → Mapeamento de Riscos → Dimensionamento de Recursos → Recomendações e Contingências.

---

## 📁 Estrutura do repositório

```
racerisk-guardian/
│
├── prompts/
│   ├── 01-perfil-ia.txt            # Camada 1: perfil e metodologia do agente
│   └── 02-inputs.txt               # Camada 2: contexto, problemas e análises
│
├── dados-exemplo/                  # Dados reais do estudo de caso (anonimizados — LGPD)
│   ├── resultado-2024.txt          # Tempos e pace dos concluintes (edição histórica)
│   ├── inscritos-consolidado.csv   # Perfil demográfico dos inscritos (nascimento/sexo/cidade)
│   ├── altimetria-02.csv           # Altitude a cada ~90m do percurso de 5km
│   ├── home-largada-2024.png       # Foto aérea da área de largada/chegada
│   ├── percurso-gradient.png       # Mapa de inclinação por cores (Plotaroute)
│   └── 01-percurso-street-map.pdf  # Mapa georreferenciado e direções do percurso
│
├── artigo/
│   └── TCC_IA_Gestao_Riscos_Corridas.pdf   # Artigo científico completo
│
├── CITATION.cff                    # Metadados de citação acadêmica (padrão GitHub)
├── LICENSE                         # Licença CC BY 4.0
└── README.md
```

---

## 🚀 Guia Rápido — Usando com os arquivos de exemplo

Este guia permite replicar o estudo de caso descrito no artigo, utilizando os dados reais do evento.

### Pré-requisitos

- Acesso a um modelo de linguagem com suporte a **upload de múltiplos arquivos**
- **Modelos recomendados:** Claude (Anthropic) ou Gemini 1.5 Pro / 2.0 (Google)
- Janela de contexto mínima recomendada: **100k tokens**

> ℹ️ O estudo de caso original foi executado com o modelo **Gemini 1.5 Pro (2025)** via interface web do Google. O framework foi posteriormente testado também no Claude (Anthropic) com resultados equivalentes.

---

### Passo 1 — Abra o modelo de linguagem

Acesse a interface web do modelo de sua preferência:
- Claude: [claude.ai](https://claude.ai)
- Gemini: [gemini.google.com](https://gemini.google.com)

Inicie uma **nova conversa**.

---

### Passo 2 — Configure o perfil do agente (Camada 1)

Cole o conteúdo completo do arquivo `prompts/01-perfil-ia.txt` como **primeira mensagem** da conversa.

Se estiver usando a **API do modelo**, envie o conteúdo como **System Prompt**.

O modelo confirmará que assumiu o papel de **RaceRisk Guardian** e está pronto para receber os dados.

---

### Passo 3 — Faça upload dos arquivos de dados

Na mesma conversa, anexe os arquivos da pasta `dados-exemplo/`:

| Arquivo | Tipo | O que contém |
|---|---|---|
| `resultado-2024.txt` | TXT | Resultados completos da edição anterior |
| `inscritos-consolidado.csv` | CSV | Perfil demográfico dos inscritos (anonimizado) |
| `altimetria-02.csv` | CSV | Perfil de altitude do percurso de 5km |
| `home-largada-2024.png` | Imagem | Foto aérea da concentração na largada |
| `percurso-gradient.png` | Imagem | Mapa de inclinação por cor |
| `01-percurso-street-map.pdf` | PDF | Mapa, altimetria e direções do percurso |

> 💡 Se o modelo tiver limitação de arquivos simultâneos, priorize: CSV + TXT primeiro (análise quantitativa), depois adicione PDF e imagens em mensagens subsequentes.

---

### Passo 4 — Envie os inputs de contexto (Camada 2)

Cole o conteúdo completo do arquivo `prompts/02-inputs.txt` como mensagem após o upload.

---

### Passo 5 — Avalie o relatório gerado

O modelo processará todos os dados e entregará um relatório estruturado com:

- Análise quantitativa comparativa (histórico vs. edição atual)
- Mapeamento de zonas de risco no percurso
- Dimensionamento recomendado de recursos humanos e médicos
- Respostas objetivas aos problemas prioritários
- Planos de contingência para recursos críticos

---

## 🔧 Guia Avançado — Usando com seus próprios dados

Para aplicar o framework a **qualquer corrida de rua**, siga as adaptações abaixo.

### O que adaptar em `02-inputs.txt`

O arquivo `01-perfil-ia.txt` é genérico e **não requer alterações**. Já o `02-inputs.txt` contém dados específicos do estudo de caso e deve ter as seguintes seções substituídas:

#### Seção 1.1 — Dados da edição histórica

```
# Substitua pelos indicadores reais do seu evento anterior:

Total de concluintes registrados: 2.727 participantes
→ [Número de concluintes do seu evento]

Recursos médicos: 2 ambulâncias (1 fixa, 1 móvel)
→ [Recursos médicos que você utilizou]

Equipe de bombeiros: 4 voluntários em ponto fixo
→ [Equipe de segurança utilizada]

Equipe operacional: 15 staffs
→ [Número de staffs operacionais]

Incidentes Registrados:
→ [Liste os incidentes ocorridos — mesmo que "nenhum incidente registrado"]
```

#### Seção 1.2 — Dados da edição atual

```
# Substitua pelo volume real de inscritos:

Total de inscritos confirmados: 3.477 participantes
→ [Número de inscritos confirmados do seu evento]
```

#### Referências aos arquivos de dados

Ao fazer o upload, use os nomes dos seus arquivos. Nas seções que mencionam nomes como `resultado-2024.txt` ou `inscritos-consolidado.csv`, substitua pelos nomes que você deu aos seus arquivos.

---

## 📂 Preparando seus arquivos de dados

### Arquivo de Resultados Históricos *(obrigatório)*

**Formato:** TXT ou CSV

Deve conter por participante: número do peito, categoria (faixa etária + sexo), tempo de conclusão e pace.

```
# Exemplo de estrutura compatível:
NUMERO  SEXO  CATEGORIA   POSICAO  EQUIPE          TEMPO_GUN  TEMPO_CHIP  PACE
1418    F     F-30-39     1        MINHA EQUIPE     00:19:13   00:19:10    03:49
```

> ⚠️ Se o arquivo contiver múltiplas categorias com numeração reiniciando do 1 (formato comum de softwares de cronometragem), adicione esta instrução ao enviar: *"O arquivo contém múltiplas categorias. A numeração reinicia a cada categoria. Some todas as linhas para obter o total real de concluintes."*

---

### Arquivo de Inscritos *(obrigatório)*

**Formato:** CSV

Seguindo as diretrizes da **LGPD (Lei nº 13.709/2018)**, inclua **apenas dados não identificáveis**:

```csv
NASCIMENTO,SEXO,CIDADE,EQUIPE,MODALIDADE
15/07/1967,Masculino,Cidade,Nome da Equipe,10 KM
17/05/1980,Feminino,Cidade,AVULSO,5 KM
```

| ✅ Incluir | ❌ Não incluir |
|---|---|
| Data de nascimento | Nome completo |
| Sexo | CPF / RG |
| Cidade | E-mail |
| Equipe | Telefone |
| Modalidade | Endereço |

---

### Mapa do Percurso *(recomendado)*

**Formato:** PDF

Ferramentas gratuitas para gerar o mapa com altimetria e direções:

| Ferramenta | Link | Formatos de export |
|---|---|---|
| **Plotaroute** *(utilizada no estudo)* | [plotaroute.com](https://www.plotaroute.com) | PDF, GPX, CSV |
| Komoot | [komoot.com](https://www.komoot.com) | PDF, GPX |
| Strava Route Builder | [strava.com/routes/new](https://www.strava.com/routes/new) | GPX |

O PDF do Plotaroute já inclui automaticamente: mapa georreferenciado, perfil altimétrico e tabela de direções com distâncias em milhas — ideal para o framework.

---

### Dados de Altimetria *(recomendado)*

**Formato:** CSV

```csv
"Distance from Start (km)","Height Above Sea Level (m)","Comments"
0,549,""
0.09,550,""
```

Para exportar do Plotaroute: **Route Tools → Export → Elevation Data (CSV)**

---

### Imagem da Largada *(complementar)*

**Formato:** PNG ou JPG

Foto aérea (drone) da área de concentração dos participantes. Permite ao modelo estimar densidade de pessoas por m² e avaliar o posicionamento de recursos.

> 🔒 **Privacidade:** Use fotos em altitude suficiente para que participantes individuais não sejam identificáveis. Imagens de drone a partir de ~30m de altura são geralmente adequadas.

---

### Imagem do Gradiente de Inclinação *(complementar)*

**Formato:** PNG

O Plotaroute gera automaticamente uma visualização colorida da inclinação:

| Cor | Significado |
|---|---|
| 🔴 Vermelho | Subida íngreme (≥ 5%) |
| 🟡 Amarelo | Subida moderada (3–5%) |
| 🟢 Verde | Trecho plano (< 3%) |
| 🔵 Azul | Descida |

---

### Usando via API *(modo programático)*

```python
import anthropic

client = anthropic.Anthropic(api_key="sua-api-key")

with open("prompts/01-perfil-ia.txt", "r", encoding="utf-8") as f:
    perfil = f.read()

with open("prompts/02-inputs.txt", "r", encoding="utf-8") as f:
    inputs = f.read()

# Envie o perfil como system prompt e os inputs como mensagem do usuário
# Adicione os arquivos de dados conforme a documentação da API escolhida
response = client.messages.create(
    model="claude-opus-4-6",
    max_tokens=8096,
    system=perfil,
    messages=[
        {"role": "user", "content": inputs}
    ]
)

print(response.content[0].text)
```

> 📖 Documentação de upload de arquivos via API:
> - Claude: [docs.anthropic.com](https://docs.anthropic.com)
> - Gemini: [ai.google.dev](https://ai.google.dev)

---

## 🔍 Detalhamento dos arquivos de prompt

### `01-perfil-ia.txt` — Perfil do Agente

| Seção | Conteúdo |
|---|---|
| Descrição | Apresentação do agente RaceRisk Guardian |
| Áreas de Expertise | PMBoK 7ª ed., ISO 31000, Eventos Esportivos |
| Metodologia (5 Fases) | Coleta → Quantitativa → Riscos → Recursos → Recomendações |
| Formato de Entrega | Resposta objetiva + Justificativa técnica + Alternativa de baixo custo + Contingência |
| Diretrizes Obrigatórias | Fundamentar em dados, citar fontes, não exibir código Python |
| Princípios de Risco | Severidade × Probabilidade → Prioridade → Mitigação → Contingência |

**Quando personalizar:** O perfil é genérico e funciona para qualquer corrida sem alterações. Personalize apenas para adicionar expertise específica — ex: corridas de montanha (trail run), eventos noturnos ou triatlos.

---

### `02-inputs.txt` — Inputs de Contexto

| Seção | Conteúdo | Adaptar? |
|---|---|---|
| 1. Contexto Geral | Volume histórico, recursos e incidentes | ✅ Sim |
| 2. Problemas Prioritários | Perguntas sobre atendimento médico e largada | ✅ Opcional |
| 3. Metodologia de Análise | Análises obrigatórias (A–D) e complementares (E–G) | Não necessário |
| 4. Diretrizes e Restrições | Restrições operacionais e formato | Não necessário |
| 5. Arquivos Utilizados | Lista dos arquivos a processar | ✅ Sim (nomes) |

---

## ❓ Perguntas e problemas sugeridos

Com base no estudo de caso, estas são as perguntas que geraram resultados de maior valor operacional. Use-as como ponto de partida para seu próprio evento.

**Atendimento Médico**
- Qual o ponto estratégico para posicionamento de uma ambulância adicional, considerando altimetria e distribuição temporal dos corredores?
- Quantos bombeiros/socorristas são necessários para cobertura completa do percurso?
- Como identificar visualmente participantes de categorias especiais (iniciantes, idosos, condições médicas pré-existentes)?

**Gestão da Largada**
- Qual o número ideal de staffs para o volume de inscritos projetado?
- Como organizar a largada em ondas com base no pace histórico?
- Quais são os principais gargalos operacionais identificados a partir dos dados históricos?

**Análise do Percurso**
- Quais trechos apresentam risco cardíaco elevado (subidas > 8%)?
- Quais trechos apresentam risco de quedas (descidas > 10%)?
- Qual o ponto mais distante da largada e qual o tempo estimado de resposta a emergências?
- Existem sobreposições de fluxo entre participantes de 5km e 10km que representem risco?

---

## ⚠️ Limitações e boas práticas

### Limitações

- **Dependente da qualidade dos dados:** dados incompletos ou inconsistentes comprometem diretamente a qualidade das recomendações
- **Não substitui expertise humana:** os resultados devem ser validados por profissionais de segurança, medicina esportiva e organização de eventos
- **Sem monitoramento em tempo real:** o framework opera sobre dados de planejamento, não durante o evento
- **Contexto geográfico limitado:** informações sobre hospitais próximos, trânsito e clima devem ser inseridas manualmente nos inputs

### Boas Práticas

✅ **Triangule os resultados** com a experiência da equipe e referências técnicas (CBMERJ, CFM, World Athletics)

✅ **Seja específico nos inputs** — quanto mais detalhado o `02-inputs.txt`, melhores as recomendações. Inclua incidentes anteriores e restrições orçamentárias

✅ **Processe gradualmente** se o modelo tiver limitações de contexto: comece com CSV + TXT, depois adicione PDF e imagens

✅ **Documente o relatório gerado** como parte do registro formal de gestão de riscos do projeto

✅ **Verifique nomes de ruas e locais** — o modelo pode referenciar dados dos arquivos de exemplo se não forem substituídos

---

## 📖 Como citar

Se você utilizar este framework em sua pesquisa ou prática profissional, por favor cite:

**Citação do framework (repositório):**
```
MORAES, C.; AMORIM, F.; BRITOLI, R.; VULCANI, R.; MARTINATTI, V.;
SCALA, T. (orientador). RaceRisk Guardian: Framework de IA para
Gestão de Riscos em Corridas de Rua. Centro Universitário Facens,
Sorocaba/SP, 2026. Disponível em:
https://github.com/fabioamorim/racerisk-guardian
```

**Citação do artigo científico:**
```
MORAES, C.; AMORIM, F.; BRITOLI, R.; VULCANI, R.; MARTINATTI, V.;
SCALA, T. Inteligência Artificial aplicada à Gestão de Riscos: estudo
de caso em projetos de corridas de rua. Centro Universitário Facens,
Sorocaba/SP. Submetido em: 24 jan. 2026.
```

> O arquivo `CITATION.cff` neste repositório está no formato padrão reconhecido automaticamente pelo GitHub para facilitar citações acadêmicas.

---

## 📄 Licença

Este projeto está licenciado sob a **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

Você pode copiar, redistribuir, adaptar e usar este material para qualquer finalidade, incluindo comercial, desde que forneça a atribuição apropriada aos autores.

[![License: CC BY 4.0](https://licensebuttons.net/l/by/4.0/88x31.png)](https://creativecommons.org/licenses/by/4.0/)

Veja o arquivo [LICENSE](./LICENSE) para o texto completo.

---

*Desenvolvido no Centro Universitário Facens — Sorocaba, SP, Brasil — 2025/2026*
>>>>>>> 00ac8a5 (Add files)
