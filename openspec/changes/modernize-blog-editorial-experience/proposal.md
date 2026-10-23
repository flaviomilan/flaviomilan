# Proposal

## Why

A homepage já possui uma identidade própria, mas listagens, publicações, assuntos, séries e busca ainda conservam composições do desenho anterior. Modernizar a experiência editorial completa dará continuidade visual à navegação e tornará o acervo técnico mais fácil de descobrir e ler.

## What Changes

- Transformar `/posts/` e `/pt-br/posts/` em entradas editoriais com publicação recente em destaque, acesso à busca e assuntos, mantendo o arquivo cronológico completo e acessível sem JavaScript.
- Unificar a apresentação dos índices e resultados de categorias e tags, com nomes amigáveis, contagens e descrições somente quando existirem fontes para elas.
- Apresentar séries como percursos de leitura, preservando os artigos, a ordem cronológica atual e seus destinos.
- Modernizar o cabeçalho das publicações com título, descrição disponível, autoria e metadados organizados; preservar o corpo em até 68ch e adequar o sumário ao espaço disponível.
- Refinar código, matemática, tabelas, citações, callouts e o encerramento dos artigos, incluindo navegação, relacionados, compartilhamento, newsletter e comentários existentes.
- Integrar a busca Pagefind à linguagem editorial, com estados compreensíveis de carregamento, resultados, ausência de resultados e falha; oferecer navegação alternativa sem scripts.
- Harmonizar o footer compartilhado e a apresentação da recuperação 404 com a identidade editorial, preservando seus contratos e destinos.
- Aplicar a mesma composição e qualidade nos dois idiomas, com validação visual, funcional, acessível e de performance contra o estado atual do site.

## Capabilities

### New Capabilities

Nenhuma. As capacidades existentes já abrangem apresentação, descoberta e leitura do conteúdo técnico.

### Modified Capabilities

- `technical-reading`: ampliar os contratos de descoberta do acervo, assuntos, séries e busca; definir cabeçalho editorial, sumário responsivo, apresentação técnica e continuidade da leitura.
- `site-design-system`: definir composição editorial compartilhada entre páginas internas, além da coerência do footer e da recuperação de navegação.

Os requisitos existentes de `experience-quality` continuam aplicáveis, sem alteração de metas ou exceções.

## Impact

Aplicação em `site/apps/`: páginas de posts, categorias, tags, séries, busca e 404; `PostLayout`, componentes editoriais, footer, estilos, traduções e ferramentas de validação existentes. A arquitetura permanece Astro estático, Tailwind e CSS, com Pagefind e os scripts locais necessários; não há justificativa identificada para React ou novas bibliotecas de animação.

Fora do escopo: redesenhar novamente homepage/About, reescrever artigos ou frontmatter, criar capas obrigatórias, alterar URLs, adicionar CMS, paginação, filtros client-side novos, personalização, serviços remotos ou infraestrutura. Não inclui deploy. A abertura editorial é a opção proposta para revisão; o arquivo completo permanece disponível na mesma página.
