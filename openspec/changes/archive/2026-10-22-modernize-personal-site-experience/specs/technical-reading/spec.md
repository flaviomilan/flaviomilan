# Spec Delta

## Purpose

Preservar a leitura confortável e a descoberta do conteúdo técnico durante a evolução visual do site, mantendo os contratos de URLs, traduções, indexação e distribuição existentes.

## ADDED Requirements

### Requirement: Comfortable long form reading
Artigos SHALL manter o corpo de leitura limitado a 68ch, hierarquia de títulos, matemática e código legíveis, sem fundos animados, inclinação ou efeitos de rastreamento ao longo do texto.

#### Scenario: Read a technical article
- **WHEN** o visitante abre um artigo longo com fórmulas, código, imagens e tabelas
- **THEN** esses elementos permanecem legíveis em desktop e mobile
- **AND** o texto não recebe animações de revelação por parágrafo nem depende delas para aparecer

### Requirement: Accessible article discovery
Listagens e cards SHALL apresentar título, data, descrição quando disponível e destino de leitura com acesso por teclado, ponteiro e toque.

#### Scenario: Open a publication from the homepage
- **WHEN** o visitante percorre os cards com teclado ou toque
- **THEN** identifica o destino e abre o artigo sem depender de hover
- **AND** os artigos recentes continuam acessíveis na homepage e na listagem do idioma

### Requirement: Stable public content contracts
O site SHALL preservar URLs publicadas, relações entre traduções, categorias, tags, séries, RSS por idioma e conteúdo indexável da busca estática.

#### Scenario: Existing article URL
- **WHEN** um leitor acessa uma URL datada publicada antes da modernização
- **THEN** encontra o mesmo artigo no idioma esperado
- **AND** links de tradução, categorias, tags e séries continuam válidos

#### Scenario: Discovery and distribution
- **WHEN** o build de produção é validado
- **THEN** feeds, sitemap, índice de busca, imagens sociais e exportação de conteúdo para agentes continuam presentes
- **AND** rascunhos preservam as regras existentes de listagem e indexação

### Requirement: Preserved search and social metadata
Páginas SHALL preservar título, descrição, canonical, hreflang, metadados sociais e dados estruturados aplicáveis, usando a origem canônica https://www.flaviomilan.dev.

#### Scenario: Inspect a translated article
- **WHEN** o HTML de um artigo e de sua tradução é inspecionado
- **THEN** canonical e alternates apontam para os destinos corretos
- **AND** metadados sociais e dados estruturados conservam título, idioma e datas autorais

#### Scenario: Missing route
- **WHEN** um visitante acessa uma rota inexistente
- **THEN** a experiência de erro mantém navegação de recuperação e a diretiva noindex

### Requirement: Preserved editorial interactions
Artigos SHALL manter sumário, navegação entre publicações e séries, relacionados, cópia de código e link, newsletter e comentários, conforme sua disponibilidade e configuração existentes.

#### Scenario: Navigate between articles without full reload
- **WHEN** o visitante abre um artigo, navega para outro e retorna
- **THEN** controles editoriais e integrações continuam associados à publicação atual
- **AND** uma ação do visitante não dispara handlers duplicados introduzidos pela modernização
