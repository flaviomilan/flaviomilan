# professional-portfolio Specification

## Purpose

Apresentar a identidade profissional de Flavio Milan por meio de projetos, decisões técnicas e conteúdo bilíngue verificável, conectando o portfólio à publicação técnica existente.

## Requirements

### Requirement: Clear professional identity
A homepage SHALL identificar Flavio Milan, apresentar seu foco em engenharia de software, arquitetura, inteligência artificial e segurança, e oferecer acesso direto a projetos e artigos.

#### Scenario: Discover the author
- **WHEN** um visitante abre a homepage em inglês ou português
- **THEN** o hero mostra o nome do autor e uma mensagem profissional no idioma correspondente
- **AND** existem chamadas de ação para a seção de projetos e a listagem de artigos do mesmo idioma

### Requirement: Evidence based project presentation
O portfólio SHALL apresentar pelo menos um projeto verificável com problema abordado, papel do autor, decisões técnicas, tecnologias e link para código ou documentação pública.

#### Scenario: Read a project card
- **WHEN** o visitante consulta um projeto em destaque
- **THEN** sua descrição técnica e seus links estão disponíveis sem hover, toque adicional ou animação
- **AND** alegações de resultado ou autoria possuem evidência registrada na curadoria

#### Scenario: Missing optional project information
- **WHEN** um projeto não possui métricas públicas, imagem ou demonstração
- **THEN** esses campos são omitidos sem números inventados, imagens quebradas ou controles sem destino

### Requirement: Bilingual portfolio parity
O portfólio SHALL oferecer informações equivalentes em inglês e português, preservando a identidade dos projetos, links técnicos e caminhos localizados de navegação.

#### Scenario: Switch homepage language
- **WHEN** o visitante alterna entre a homepage em inglês e português
- **THEN** encontra os mesmos projetos e chamadas de ação equivalentes
- **AND** a navegação interna segue o idioma selecionado

#### Scenario: Untranslated project entry
- **WHEN** uma entrada de projeto ainda não possui os textos obrigatórios nos dois idiomas
- **THEN** a validação de conteúdo impede sua publicação como projeto em destaque

### Requirement: Verifiable career narrative
A página About SHALL preservar a apresentação pessoal e os contatos existentes e apresentar marcos cronológicos somente quando sustentados por informações profissionais verificadas.

#### Scenario: Verified milestones available
- **WHEN** existem marcos profissionais confirmados com datas e descrições
- **THEN** About os apresenta em ordem cronológica compreensível nos dois idiomas
- **AND** datas e descrições permanecem legíveis independentemente de efeitos visuais

#### Scenario: Milestone information unavailable
- **WHEN** não há marcos profissionais confirmados
- **THEN** About mantém a apresentação e as áreas de atuação verificadas
- **AND** não exibe uma timeline vazia, datas inferidas de artigos ou empresas e cargos inventados
