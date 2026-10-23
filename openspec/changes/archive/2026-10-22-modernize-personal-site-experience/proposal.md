# Proposal

## Why

A apresentação atual de flaviomilan.dev privilegia o blog e não evidencia suficientemente a identidade profissional, os projetos e as decisões de engenharia do autor. Uma experiência visual própria, inspirada no Aceternity UI, pode conectar essas evidências sem comprometer a arquitetura estática, o conteúdo bilíngue e a leitura técnica.

## What Changes

- Evoluir o design escuro existente com hierarquia tipográfica, composição ampla na homepage, grid discreto, luz localizada e motion funcional; preservar a largura editorial dos artigos.
- Introduzir Tailwind CSS gradualmente, reutilizando os tokens CSS existentes e controlando efeitos de reset e cascade.
- Compartilhar a composição da homepage entre inglês e português, apresentando Flavio Milan, especialidades, projetos selecionados e publicações.
- Adicionar dados tipados e bilíngues de projetos, com problema, papel, decisões técnicas, tecnologias e links verificáveis; apresentar trajetória em About somente com marcos comprovados.
- Preservar HTML estático e componentes Astro como padrão. Avaliar uma única interação 3D Card do Aceternity contra alternativa nativa; incorporar React apenas se o resultado justificar seu custo e cumprir os budgets.
- Refinar cards e navegação editorial preservando URLs, traduções, busca, feeds, matemática, código, metadados e integrações existentes.
- Estabelecer baseline e verificações reproduzíveis de regressão visual, acessibilidade, JavaScript e performance, distinguindo laboratório de dados reais.
- Não incluir migração para Next.js, CMS, alterações de infraestrutura, publicação, WebGL, modais, palavras rotativas ou navegação que desaparece ao rolar.

## Capabilities

### New Capabilities

- `site-design-system`: identidade visual, tokens compartilhados, tipografia, responsividade e comportamento de movimento.
- `professional-portfolio`: apresentação bilíngue de identidade, projetos e trajetória verificável.
- `technical-reading`: experiência editorial e descoberta de conteúdo com preservação dos contratos públicos existentes.
- `experience-quality`: enhancement progressivo, fronteiras de interatividade e critérios verificáveis de performance e acessibilidade.

### Modified Capabilities

Nenhuma. O inventário atual de specs está vazio; as capacidades acima formalizam tanto o comportamento novo quanto as garantias de preservação necessárias à modernização.

## Impact

A implementação afetará `site/apps/src/styles/`, componentes, layouts, homepages, About, dados de portfólio, traduções e testes. Tailwind e seu plugin Vite serão novas dependências de build; `@astrojs/react`, React/React DOM e tipos serão condicionais ao experimento aprovado pelos critérios técnicos. Motion não é requisito do 3D Card examinado.

O site continuará em Astro estático, com Markdown/MDX, Pagefind, RSS, sitemap e imagens OG existentes. Mudanças na configuração de estilos e nos gates de qualidade ocorrerão apenas durante implementação. Este planejamento não altera código, dependências instaladas, infraestrutura nem produção.
