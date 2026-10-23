# Design

## Context

Ver `proposal.md` para motivação e limites de produto. A aplicação está em `site/apps/`, com Astro 7.3.1 estático, Tailwind 4.3.3 sem Preflight, MDX, Shiki, KaTeX, Pagefind e ClientRouter. A change anterior foi arquivada e não há outra change ativa. Os contratos principais estão em `technical-reading`, `site-design-system` e `experience-quality`.

Fatos observados no código e nas capturas da validação anterior:

- Home e About já compartilham composição EN/PT-BR; as famílias editoriais ainda têm markup por idioma e estilos antigos em `global.css`.
- `/posts/` separa publicados e rascunhos e agrupa o arquivo por ano/mês. Não usa o `ArticleCard` novo da homepage; categorias/tags usam outra variante de lista e séries usam outra composição.
- `PostLayout` concentra dados estruturados, datas, tradução, sumário, navegação de série/cronológica, relacionados, compartilhamento, assinatura e comentários. Recebe `description`, mas não a apresenta no cabeçalho. O cabeçalho centralizado acumula categorias, tags e tradução.
- `TableOfContents` usa `details` nativo aberto e inclui h2/h3 quando há pelo menos três entradas. Hoje aparece antes do texto em todas as larguras.
- `ArticleCard` já usa data com `timeZone: 'UTC'`; outros locais usam o fuso da máquina. As capturas anteriores mostram diferença de dia em relação a datas autorais. Uniformizar a apresentação não requer mudar frontmatter nem caminhos.
- A coleção de séries fornece título e descrição; categorias/tags vêm dos artigos e não possuem coleção editorial própria. Categorias podem aparecer como identificadores crus, como `ai`.
- A busca usa `searchInteractions.ts`, Pagefind UI local, Promise compartilhada, limpeza de observer e destruição de instância. Já trata falha textual de carregamento, mas não oferece apresentação completa dos estados nem alternativa explícita sem JS.
- A qualidade anterior tem 1.857 testes e auditorias reutilizáveis. Os números do relatório anterior são referência histórica; não substituem uma baseline fresca do estado de trabalho atual, que pode conter alterações ainda não commitadas.

## Goals / Non-Goals

**Goals:**
- Fazer descoberta e leitura parecerem partes da mesma publicação técnica, com composição própria para cada contexto.
- Compartilhar dados e apresentação entre idiomas sem duplicar regras de publicação, datas ou ordenação.
- Dar prioridade visual a texto, assunto e continuidade de leitura, usando efeitos discretos de hover/foco.
- Reutilizar ferramentas e contratos de qualidade existentes, ampliando sua cobertura para todas as famílias afetadas.

**Non-Goals:**
- Criar um layout universal com dezenas de flags, um novo motor de busca ou um framework de filtros.
- Transformar as páginas em aplicações React, adicionar runtime de animação ou trocar fontes.
- Reorganizar conteúdo, alterar política de rascunhos, refazer a homepage/About ou modificar coleta de métricas.
- Implementar persistência nova de consulta em URL/histórico; preservar os comportamentos existentes de Pagefind e verificar navegação de retorno.

## Decisions

### 1. Direção editorial com abertura e arquivo completo

Adotar a opção recomendada na exploração como proposta revisável: abrir `/posts/` com cabeçalho alinhado à esquerda, apresentação breve, quantidade de publicados, busca e links para assuntos. Destacar exatamente o publicado mais recente do idioma; não criar curadoria ou campo de destaque novos. Manter abaixo o arquivo integral por ano/mês com datas, descrições e tempo de leitura.

O artigo destacado também aparece em sua posição no arquivo, pois este representa o inventário completo. A unicidade se aplica à seção de arquivo, não à página inteira. Nenhum card de destaque é criado se não houver publicados. Uma publicação não exige um grid com espaços vazios. Usar data decrescente e desempate determinístico por id; agrupamento e datas visíveis seguem UTC como a apresentação atual da homepage.

Alternativas: apenas redesenhar a timeline seria mais simples, mas manteria pouca hierarquia entre novidades e histórico; um mosaico de todos os artigos desperdiçaria espaço e dificultaria comparar títulos longos. A composição escolhida combina um destaque com entradas compactas.

### 2. Sistema visual editorial sem uma segunda identidade

Reutilizar os tokens existentes de fundo, painéis, texto, acento violeta e sinal ciano; não introduzir outra paleta. Inter continua responsável por títulos e corpo; JetBrains Mono fica em metadados curtos e código. Usar bordas finas, planos discretos, tipografia e espaçamento para a hierarquia. Evitar gradientes integrais, vidro, parallax, loops, revelação do corpo e inclinação.

Cabeçalhos de descoberta usam largura ampla dentro do wrapper existente. O cabeçalho de artigo pode ser mais largo que o corpo para acomodar títulos técnicos; o texto permanece em até 68ch. Decoração é estática e secundária. Revisar a capitular automática existente: retirar a aplicação indiscriminada ao primeiro parágrafo para que aberturas técnicas, links e callouts tenham leitura previsível. Não adicionar capas obrigatórias nem usar OG com texto como imagem de abertura automática.

Comparação considerada: expandir diretamente todas as regras globais é rápido, mas tende a atingir homepage/About e outros consumidores. Preferir CSS escopado de composição e manter em `global.css` tokens, conteúdo renderizado de MDX e padrões realmente globais. Remover regras antigas apenas quando seus consumidores tiverem sido migrados e verificados. Respeitar a precedência existente entre CSS sem layer e utilities; não habilitar Preflight nem espalhar `!important`.

### 3. Dados comuns, componentes com responsabilidades pequenas

As páginas preservam `getStaticPaths`, destinos e passagem de SEO. Extrações propostas, criadas somente quando houver consumidor real:

```text
src/components/editorial/
  EditorialHeader.astro       titulo, descricao, metadados e acoes
  ArchiveContent.astro        abertura e arquivo compartilhados EN/PT-BR
  PublicationList.astro      entradas compactas com headings adequados
  TopicIndex.astro           categorias/tags com contagens
  TopicContent.astro         contexto e publicacoes de um assunto
  SeriesIndex.astro          catalogo de series
  SeriesContent.astro        sequencia numerada
  SearchContent.astro        abertura, estado e navegacao alternativa
  PublicationHeader.astro    abertura do artigo
  PublicationEnding.astro    composicao dos componentes existentes
src/components/blog/
  ArticleCard.astro           destaque reutilizado, sem mudar a homepage
src/lib/
  editorialContent.ts         selecao, contagens, agrupamento e datas
```

`editorialContent.ts` recebe coleções já carregadas, idioma e parâmetros explícitos; usa os helpers existentes para URLs e idioma. Não consulta rede nem mantém cache global de conteúdo. Publicados são filtrados antes de contagens e seleção. Datas usam um formatador compartilhado com UTC e locale atual; timestamps estruturados continuam os autorais. Não alterar Markdown, frontmatter ou ids para corrigir apresentação.

Metadados visuais de categorias conhecidas podem usar as traduções existentes ou um mapa local pequeno por id estável. Tags técnicas preservam grafia apropriada; não traduzir identificadores em URLs. Descrições só vêm de dados editoriais já presentes; caso não existam, omitir. Reusar `ArticleCard` para destaque; permitir heading semântico adequado ao contexto se necessário, conservando a API atual da homepage. Não forçar cada variante de lista a ser um card grande nem introduzir abstração sem reutilização.

### 4. Assuntos e séries preservam seus contratos

Índices de categorias/tags usam nomes, contagens e links, com maior hierarquia visual. Detalhes apresentam contexto, quantidade, breadcrumb e a lista de publicados em data decrescente. Não criar filtros interativos: links estáticos para rotas existentes resolvem a descoberta solicitada.

Séries usam título e descrição da coleção; catálogo continua mostrando apenas séries com publicados. Rotas existentes de séries vazias permanecem geradas, com estado vazio e sem CTA inválido. Sequências mantêm a ordenação crescente de data atual; desempate determinístico por id. Mostrar posição e uma ação para o primeiro item quando existir. Não inferir uma ordem curricular nem criar propriedade de ordem nova.

### 5. Cabeçalho e corpo do artigo

Extrair apenas a apresentação do cabeçalho de `PostLayout`. Manter nele o cálculo/passe de metadados, tradução, rascunho, dados estruturados e destinos. Cabeçalho alinhado à esquerda com assunto primário, h1 expressivo, descrição opcional e uma faixa concisa de autor/data/leitura. Assuntos adicionais, tags, série e tradução permanecem acessíveis com menor peso visual, sem lista oculta dependente de hover. Autoria é Flavio Milan, já declarada no site; não adicionar foto, cargo ou credenciais inventadas.

A largura de corpo de 68ch permanece um limite máximo, não uma largura mínima forçada em mobile. Refinar os estilos atuais de headings, listas, código, tabelas, KaTeX, imagens, blockquotes e `Callout`, preservando sintaxe, seleção, conteúdo e semântica. Código/fórmulas/tabelas largos têm overflow local. Controles de cópia aparecem também por foco; decoração não pode cobri-los. Não mudar Shiki, processamento de Markdown ou renderização matemática nesta change.

### 6. Sumário lateral como enhancement de layout

Manter o mesmo sumário nativo e os mesmos ids dos headings. Em telas largas, posicioná-lo ao lado do corpo com CSS Grid e sticky limitado ao contêiner de leitura. Começar a avaliar o modo lateral em 1200px, somente se o wrapper comportar corpo, gap e aproximadamente 14rem de sumário. Se não couber, manter composição empilhada; o breakpoint final é resultado da matriz de layout.

O sumário tem altura máxima em função da viewport e rolagem interna, foco visível e espaço para o header fixo. No modo empilhado, `details` fica antes do texto, operável sem JS; pode começar aberto como hoje. Não manter duas cópias de links para desktop/mobile. A ordem do DOM deve continuar coerente com a leitura. Não adicionar rastreamento de seção ativa, observer ou animação de scroll: sticky e âncoras nativas bastam. Manter `scroll-margin` para evitar headings ocultos pelo header.

### 7. Encerramento, footer e recuperação

Organizar o fim do artigo em três blocos reconhecíveis: continuar leitura; compartilhar; acompanhar/discutir. Reutilizar `SeriesNav`, `PostNav` e `RelatedPosts`, mantendo a seleção e os links existentes. Limites de sequência omitem o destino inexistente. Manter todos os destinos de compartilhamento atuais, permitindo quebra de linha; não instalar SDKs nem enviar eventos/ações automaticamente.

Newsletter e Giscus preservam integração, configuração e consentimento existentes. Não adicionar uma caixa de autor com alegações novas. Footer agrupa navegação editorial, RSS e perfis externos de modo responsivo; seus destinos atuais permanecem. A mudança do footer atinge também homepage/About, por isso entra na revisão visual dessas páginas. A 404 recebe somente harmonização visual e de recuperação, preservando noindex, links e mecanismo atual de seleção por idioma; não criar novas rotas de erro nem alterar infraestrutura.

### 8. Busca com Pagefind e estados acessíveis

Manter Pagefind UI e `searchInteractions.ts` como fronteira de cliente; não reimplementar o motor ou sua paginação/filtros. Estilizar por variáveis e seletores locais suportados pelos assets instalados. Mensagens de preparação/falha e links para `/posts/` e `/categories/` localizados existem fora do contêiner que Pagefind substitui. Incluir explicação `noscript`; links alternativos permanecem acessíveis mesmo se o script falhar.

Separar carregamento inicial dos resultados vazios e do erro de assets. Aproveitar estados acessíveis do Pagefind e evitar regiões live duplicadas. Não mover foco a cada resultado nem recriar a instância para estilizar. Preservar consulta digitada em atualizações da mesma instância. Manter Promise de carregamento, `destroy()`, desconexão de observer e guarda de callback assíncrono após navegação. Não adicionar promessa de filtro por idioma ou persistência de consulta que o comportamento atual não garanta; revisar que a UI e a navegação alternativa seguem o idioma atual.

### 9. Validação e critérios de aceite

As metas de `experience-quality` permanecem integrais: três execuções frias por URL/perfil; mediana desktop >=95 em Performance, Accessibility, Best Practices e SEO, com exceção SEO apenas para 404. Mobile: queda máxima de 3 pontos Performance, aumento máximo de 10% de LCP/TBT e 0,02 de CLS; referência zero não admite aumento. Não usar TBT como comprovação de INP nem transformar o resultado anterior em baseline deste trabalho.

Antes da alteração visual, criar build fresco do estado atual e registrar hash das fontes/config/lockfile, versões, ferramenta, servidor, CPU/rede, consentimento e isolamento de terceiros. Um SHA de HEAD só identifica a baseline se a árvore correspondente estiver limpa. Usar a mesma revisão do coletor para base e candidato, executar sequencialmente e evitar build ou testes concorrentes durante Lighthouse. Se um conjunto for invalidado por carga externa, registrar motivo e repetir o perfil completo, sem selecionar apenas melhores execuções.

Ampliar `experience-routes.mjs` e o runner existente para contemplar:

| Família | Cobertura |
|---|---|
| Publicações | listagem EN/PT-BR, código, matemática e artigo longo com imagens/tabelas |
| Assuntos | índices de categorias e tags; detalhes representativos de cada um, EN/PT-BR |
| Séries | catálogo e detalhe com artigos nos dois idiomas; rota vazia para estado sem publicados |
| Busca | página nos dois idiomas; pronta, resultados, sem resultados e falha de assets em fluxos específicos |
| Regressão global | home/About EN/PT-BR, header/menu, footer e 404 |

Selecionar rotas reais das coleções durante a preparação da baseline; não usar slugs fictícios. Casos de coleção vazia, título longo e campos ausentes podem usar fixtures de testes sem publicar conteúdo fictício. Os estados dinâmicos da busca são testados em navegador; Lighthouse usa carga direta consistente da página, não interações diferentes entre base e candidato.

Capturas nas larguras 320, 390, 768, 1280 e 1536, fontes estabilizadas, movimento reduzido e terceiros isolados. Revisar screenshots de cada família e idioma, inclusive sumário aberto, foco e estados de busca. Reusar Playwright, axe e Nu instalados. Exigir zero overflow de página, zero violações sérias/críticas e inspeção manual WCAG 2.2 AA: contraste de texto/controles, nomes, ordem de headings, teclado, foco não encoberto, alvos, reflow e texto a 200%. Testar sumário alto e zoom que faça o layout lateral voltar ao modo empilhado.

Sem JS, arquivo, assuntos, séries e artigo publicado continuam navegáveis; a busca explica a limitação e oferece alternativas. Rascunhos conservam o comportamento de preview e noindex, sem entrar no acervo público. Executar cinco ciclos de listagem/artigo/assunto/série/busca/idioma/retorno para verificar cleanup, cópia e ausência de handlers duplicados. Mockar clipboard e integrações; não submeter newsletter, comentário ou compartilhamento real.

Reutilizar testes de rotas, conteúdo, SEO, feeds, sitemap, assets e traduções. Adicionar invariantes semânticas de inventário, destaque, contagens, sequência, datas e campos opcionais; não congelar toda a estrutura CSS em testes. Rodar `code-quality`, build, Vitest, Nu e inventário de recursos. Não adicionar runtime React/Motion a nenhuma rota. Registrar bytes gzip e diferenças de recursos; não confundir chunks emitidos com todos os serviços externos ou código inline. A observação de campo de 28 dias já documentada permanece futura, sem deploy nesta change.

## Risks / Trade-offs

- [CSS global compartilhado afeta homepage/About] -> Escopar composições; revisar consumers antes de excluir regras e capturar as páginas preservadas.
- [Destaque aparece duas vezes na listagem] -> Identificar visualmente a abertura e manter o inventário completo; testar unicidade dentro do arquivo.
- [Tags sem descrição ou nomes traduzidos] -> Manter grafia original como fallback e omitir descrição sem fonte; nunca alterar slug para melhorar o label.
- [Sumário longo ou sticky encobre foco] -> Limitar altura, permitir scroll interno e retornar ao layout empilhado quando não houver espaço.
- [Mudança de wrappers quebra seletores de scripts] -> Preservar ou atualizar explicitamente os seletores em `articleInteractions`, EngagementTracker e testes no mesmo passo.
- [Pagefind injeta markup e CSS próprios] -> Personalizar sua fronteira local e validar assets reais em vez de depender de classes não verificadas ou reconstruir o componente.
- [Datas mudam com fuso do build] -> Centralizar formato UTC e testar o mesmo dia em todas as apresentações, preservando timestamps e URLs.
- [Baseline por HEAD perde alterações anteriores não commitadas] -> Identificar snapshot real por hash e preservar o build antes de editar; não restaurar nem limpar alterações existentes.
- [Benchmark variável em máquina ocupada] -> Execução sequencial, registro de dispersão e repetição documentada do conjunto invalidado; não flexibilizar metas silenciosamente.

## Migration Plan

1. Preservar estado de trabalho e baseline; preparar a matriz expandida sem alterar produção.
2. Entregar componentes editoriais e dados compartilhados, depois listagem/assuntos/séries nos dois idiomas.
3. Refinar publicação e sumário; validar conteúdo técnico e integrações antes de modernizar busca e footer/404.
4. Executar a matriz integrada e registrar aceite visual intencional separado de regressões, comparação de performance e limitações reais.

Cada etapa usa as mesmas rotas e coleções. Não há migração de dados, instalação prevista ou serviço novo. Rollback consiste em reverter apenas os commits/patches desta change, preservando a modernização anterior e alterações alheias; não usar reset destrutivo. Uma futura publicação exige solicitação separada e segue o procedimento de deploy existente.
