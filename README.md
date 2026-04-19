🏗️ Miniguia de Estudos: Arquitetura de Agentes de IA na Cohab Minas (Agentic RAG, MCP e TurboQuant)
🎯 Contexto e Objetivos
Este caderno temático foi criado para mapear a viabilidade técnica de implementação de Inteligência Artificial Generativa na Cohab Minas. O ambiente corporativo da instituição conta com sistemas heterogêneos, incluindo dados estruturados (TOTVS Datasul, Probpms da Prodemge) e dados não-estruturados ou semi-estruturados (SEI, Office 365).
Objetivos de Estudo:
Entender como conectar Modelos de Linguagem (LLMs) a esses sistemas proprietários com segurança, resolvendo o problema de integração usando o Model Context Protocol (MCP)
.
Compreender a arquitetura de Agentic RAG para permitir que a IA tome decisões autônomas sobre onde buscar a informação
.
Analisar o uso do TurboQuant para comprimir o KV Cache, permitindo que a IA leia processos massivos do SEI ou editais complexos sem esgotar a memória dos servidores (VRAM)
.

--------------------------------------------------------------------------------
📚 Curadoria de Fontes
As seguintes fontes serviram de base para as pesquisas e foram processadas no NotebookLM para a extração de insights:
TurboQuant: Redefining AI efficiency with extreme compression (Google Research) - Estudo sobre a compressão extrema do KV Cache para 3-4 bits
.
Model Context Protocol - Wikipedia - Documentação sobre o padrão aberto da Anthropic para integração de dados
.
Retrieval - Docs by LangChain - Guias técnicos de RAG e Agentes Autônomos (Agentic RAG)
.
Retrieval-Augmented Generation for Large Language Models: A Survey - Pesquisa profunda sobre os paradigmas do RAG Modular e Avançado
.
Guia de Fine-Tuning para IA - Documentação sobre como o Fine-Tuning interage (e não anula) as técnicas de inferência
.

--------------------------------------------------------------------------------
🛠️ Engenharia de Prompts e "Cicatrizes" (Troubleshooting)
Durante a exploração do tema, vários testes de prompts foram realizados para extrair o melhor conhecimento da IA. Aqui estão as "cicatrizes" e aprendizados:
Teste 1: Conectividade de Sistemas Fechados
Prompt: "Como conectar uma IA ao TOTVS e ao SEI de forma segura?"
Resultado e Cicatriz: Inicialmente, imaginei que teríamos que construir APIs customizadas gigantescas para cada sistema (o problema N × M). A IA corrigiu essa visão me apresentando o MCP (Model Context Protocol), que padroniza a interface e mantém as permissões do usuário
.
Teste 2: Limites de Memória com Documentos Grandes (Editais)
Prompt: "Como processar editais inteiros da Cohab sem estourar a memória do servidor?"
Resultado e Cicatriz: Descobri que o gargalo não são os pesos do modelo, mas o KV Cache (memória de curto prazo)
. A solução encontrada foi o algoritmo TurboQuant, que reduz a memória necessária em 4x a 6x sem precisar treinar o modelo de novo
.
Teste 3: Conflito de Técnicas (Fine-Tuning vs. TurboQuant)
Prompt: "Tem como fazer TurboQuant com fine-tuning, ou um tira o outro?"
Resultado e Cicatriz: Havia uma dúvida se ao comprimir a memória eu perderia o treinamento específico do vocabulário da Cohab Minas. A resposta foi esclarecedora: não conflitam. O fine-tuning ajusta o conhecimento, enquanto o TurboQuant age apenas na eficiência da memória (inferência) via rotação ortogonal (PolarQuant) e correção de erros (QJL)
.

--------------------------------------------------------------------------------
📖 Miniguia de Estudo (Entrega Final)
1. Resumos Estruturados do Assunto
O que é RAG Agêntico (Agentic RAG)? Diferente de uma simples busca, o Agentic RAG usa um LLM como "cérebro" para raciocinar passo a passo. Ele avalia a pergunta do usuário e escolhe autonomamente qual ferramenta usar. Se a dúvida é financeira, ele converte texto para SQL e busca no TOTVS; se é documental, ele busca no SEI ou Office 365
.
Integração Segura com MCP O Model Context Protocol é a arquitetura cliente-servidor ideal para a Cohab. Em vez de acoplar a IA diretamente no banco de dados corporativo, o MCP atua como uma ponte universal e segura. Ele permite que a IA leia tabelas ou documentos respeitando o controle de acesso e evitando vazamentos de dados internos
.
A Revolução do TurboQuant para Infraestrutura Janelas de contexto gigantescas (como ler milhares de páginas de processos do Probpms) esgotam a memória VRAM da GPU (o KV Cache)
. O TurboQuant resolve isso em duas etapas sem exigir re-treinamento:
PolarQuant: Rotaciona os vetores para coordenadas polares, agrupando dados com alta previsibilidade
.
QJL (Quantized Johnson-Lindenstrauss): Usa apenas 1 bit extra para corrigir desvios, resultando em compressão de 3 a 4 bits por elemento com zero perda de acurácia
. Impacto: Permite rodar modelos avançados para múltiplos usuários da Cohab com um custo de infraestrutura drasticamente menor
.
2. Glossário de Conceitos
KV Cache (Key-Value Cache): A memória de curto prazo do modelo durante a inferência. Ela cresce linearmente e se torna o maior gargalo no processamento de textos longos
.
MCP (Model Context Protocol): Padrão de código aberto para conectar IA a ferramentas externas (bancos de dados, repositórios)
.
Agentic RAG: Evolução do RAG tradicional onde o agente LLM decide quando e como buscar a informação
.
TurboQuant: Algoritmo de compressão (Google Research) que encolhe o KV Cache em até 6x sem perda de precisão
.
Fine-Tuning: Treinamento adicional dado a um modelo pré-treinado para adaptá-lo a um domínio ou tarefa específica
.
3. Conjunto de Prompts Reutilizáveis (Para Revisão)
Guarde estes prompts para utilizar no ChatGPT, Claude ou NotebookLM e expandir seus estudos no futuro:
🧠 Prompt para Arquitetura de Software: "Atue como um Engenheiro de IA. Quero criar um fluxo de Agentic RAG conectando um LLM ao meu banco de dados usando o Model Context Protocol (MCP). Descreva um diagrama de arquitetura passo a passo listando os componentes necessários."
🚀 Prompt para Otimização de Hardware: "Explique, para um gerente de TI não-técnico, a diferença entre o consumo de memória dos Pesos de um Modelo (Weights) e o consumo do KV Cache. Em seguida, explique como o algoritmo TurboQuant resolve a limitação do KV Cache na leitura de grandes documentos."
📊 Prompt para Estratégia de Dados (Text-2-SQL): "Como um modelo de RAG Híbrido lida com dados estruturados (como um ERP TOTVS)? Quais ferramentas do LangChain ou LlamaIndex eu devo estudar para transformar perguntas em linguagem natural diretamente em consultas SQL seguras?"
