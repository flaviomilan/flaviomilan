# Design

## Context

Ver `proposal.md` para motivação. A aplicação está em `site/apps/`: Astro 7.3.1, output estático, MDX 8, Pagefind, RSS, sitemap, Shiki, KaTeX e geração de OG. O manifesto e o lockfile não incluem Tailwind, React, React DOM, integração React ou Motion. Versões futuras dessas adições devem ser verificadas por peer dependencies, sem atualizar Astro como parte incidental do trabalho.

`BaseLayout.astro` centraliza head, fontes, navegação e ClientRouter. `PostLayout.astro` reúne leitura e integrações editoriais. Há scripts locais e eventos `astro:after-swap`; estático não significa ausência de JavaScript. As homepages EN/PT-BR duplicam composição. As coleções atuais são posts e séries; não há modelo de projetos ou carreira.

`global.css` possui tokens dark violeta/índigo, grid, animações e regras de movimento reduzido. `.reveal` começa invisível e depende de script para mostrar conteúdo. Há pesos CSS 800 enquanto a configuração Inter vai até 700. Os gates atuais de Lighthouse têm uma execução, Performance 85 como aviso e demais categorias 90 como erro; esses limites não representam medições.

A exploração não obteve baseline visual ou Lighthouse: a ferramenta de navegador não tinha o Chrome requerido. O `dist` existente não comprova correspondência com o código ou com produção. As metas e budgets são contratos propostos, não resultados já atingidos.

## Goals / Non-Goals

**Goals:**
- Separar apresentação ampla de portfólio da coluna editorial de até 68ch.
- Reutilizar dados e composição entre idiomas, com fronteiras pequenas para efeitos opcionais.
- Manter os requisitos verificáveis mesmo se o experimento React for rejeitado.
- Tornar explícitas as evidências de conteúdo, baseline e critérios de adoção.

**Non-Goals:**
- Criar um framework genérico de efeitos, estado global de interface ou uma ilha envolvendo a página inteira.
- Reestruturar todas as rotas, converter todo o CSS, atualizar infraestrutura ou criar CMS.
- Criar páginas dedicadas de projetos neste recorte: projetos ficam na homepage, com links para código, documentação e artigos já existentes. Trajetória fica em About.
- Publicar ou alterar serviços remotos. Acompanhamento de campo é posterior e não comprova qualidade local antecipadamente.

## Decisions

### 1. Arquitetura híbrida com núcleo nativo

| Alternativa | Impacto visual | Runtime e manutenção | Decisão |
|---|---|---|---|
| Astro + Aceternity React em várias seções | Efeitos prontos, após adaptação | Mais hidratação, dependências e auditoria | Rejeitada como padrão |
| Astro + CSS e scripts mínimos | Grande liberdade de composição | Menor runtime; scripts complexos podem custar manutenção | Base da implementação |
| Astro + nativo + ilha seletiva | Preserva composição própria e permite interação especial | Custo localizado e mensurável | Selecionada |

Manter `output: static`, ClientRouter e os layouts. A integração React, caso adotada, apenas gera/hidrata componentes locais; não requer servidor ou Next.js. React estático sem `client:*` é possível, mas não oferece vantagem sobre Astro para grid e cards apresentacionais.

### 2. Tokens únicos e Tailwind incremental

Adicionar Tailwind 4 com `@tailwindcss/vite` durante apply, verificando compatibilidade. Não usar a integração legada `@astrojs/tailwind`. Manter `global.css` como entrada e fonte dos tokens para respeitar a organização atual; criar CSS de efeitos apenas quando houver reutilização concreta.

Mapear utilities para as variáveis sem duplicar paleta em constantes TS e classes. Começar com imports de tema/utilities sem Preflight global, pois já existe reset autoral. Verificar cascade layers: regras globais sem layer prevalecem sobre regras em layers; resolver conflitos seletivamente, evitando uma migração ampla ou `!important` generalizado.

Usar o fundo atual #09090b, planos elevados, texto claro, violeta/índigo e ciano pontual. Manter Inter e JetBrains Mono via Astro Fonts. Ajustar pesos a arquivos realmente fornecidos; só adicionar peso 800 se necessário e medido. Avaliar preload de mono em vez de adicionar famílias e pesos especulativamente. Definir tokens de espaçamento, foco, duração e easing; em mobile, reduzir decoração e preservar hierarquia.

Alternativas: copiar efeitos sem Tailwind reduziria dependências, mas contraria a direção solicitada; substituir todo o CSS ampliaria o risco editorial sem benefício equivalente.

### 3. Composição e conteúdo

Criar `HomeContent.astro` parametrizado por idioma, consumido pelas duas rotas existentes. Hero apresenta nome, foco profissional e CTAs para `#projects` e listagem localizada de artigos. Projetos precedem publicações recentes; temas e acesso a RSS permanecem disponíveis. Usar grid assimétrico somente quando houver conteúdo suficiente: um único projeto recebe apresentação própria, sem cards fictícios para preencher espaço.

Projetos usam `src/data/projects.ts`: id estável, featured/order, nome, textos EN/PT-BR com problema, papel e decisões, lista de tecnologias, links de repositório/documentação e opcionais de imagem e resultados. Registrar fonte de evidência por entrada para revisão, sem expor metadados internos no produto. Validar no build/testes obrigatoriedade dos dois idiomas e URLs bem formadas. Não consultar GitHub no navegador para estrelas ou estatísticas.

Começar com pelo menos um projeto comprovado; Fackel é candidato porque há artigos e asset no repo, mas autoria, repositório e descrição devem ser confirmados nessas fontes antes de publicar. Não inferir empregadores ou experiência a partir de datas de posts. Se faltarem fatos de um projeto obrigatório, solicitar apenas esses dados durante a curadoria; não concluir essa entrega com placeholders.

Marcos profissionais usam dados tipados opcionais em `src/data/career.ts` com datas, descrições localizadas e evidência. Renderizar lista semântica cronológica em About quando houver dados confirmados. Caso contrário, refinar a apresentação e áreas existentes e registrar ausência de marcos, sem timeline vazia. A ausência de carreira estruturada não bloqueia o restante do redesign.

### 4. Seleção de efeitos e fronteiras

| Elemento | Escolha | Motivo / custo estimado |
|---|---|---|
| Hero | Spotlight em SVG/CSS + grid estático | Baixo; conteúdo já visível |
| Fundo com beams | Fora do primeiro recorte | Motion e animação recorrente sem ganho demonstrado |
| Texto | Frase fixa e hierarquia tipográfica | Evita espera de leitura, trocas de largura e loops |
| Gradient Animation integral | Fora do primeiro recorte | Fonte altera variáveis em document.body e utiliza filtros/interação |
| Projetos | Astro + CSS Grid, luz e hover/focus | Baixo; aproveita ideia de Bento sem runtime |
| 3D Card | Um experimento em projeto principal | React e ponteiro; fonte examinada não depende de Motion |
| Card Spotlight oficial | Não incorporar | Inclui Canvas Reveal, Three e React Three Fiber |
| Hover Effect oficial | Preferir CSS | Evita Motion para resposta simples |
| Tooltip | Labels de tecnologias visíveis | Informação disponível por teclado e toque |
| Timeline / Tracing Beam | Lista Astro e linha CSS estática | Evita medições e Motion; nenhum beam no corpo editorial |
| Moving Border / Hover Border Gradient | Borda CSS em hover e focus | Sem loop contínuo por frame |
| Floating Navbar | Refinar Header existente | Preserva navegação visível, teclado e idioma |
| Animated Modal | Não incorporar | Nenhum fluxo exige modal neste escopo |

Os custos acima são estimativas qualitativas. Preservar origem e licença ao incorporar código; distinguir componente copiado de adaptação inspirada. Não executar CLI de instalação de catálogo sem inspecionar os arquivos e dependências que ele escreveria.

Organização proposta, criada apenas quando houver consumidor:

```text
src/components/home/              HomeContent, Hero, FeaturedProjects
src/components/projects/          ProjectGrid, ProjectCard
src/components/career/            CareerTimeline
src/components/blog/              ArticleCard
src/components/effects/           HeroBackdrop, InteractiveProjectCard (adaptadores Astro)
src/components/islands/           InteractiveProjectCard.tsx (condicional)
src/components/vendor/aceternity/  3d-card.tsx (condicional)
src/data/                         projects.ts, career.ts (quando houver marcos)
```

Páginas importam componentes de apresentação. Somente o adaptador importa a ilha, e somente a ilha importa código externo. Nenhum import React/Motion no layout global ou corpo de artigos. Manter providers e componentes contextuais dentro da mesma ilha; componentes Astro passados como conteúdo não viram filhos React contextuais. Passar dados serializáveis, com links e conteúdo essencial no HTML inicial.

### 5. Experimento interativo e critérios de adoção

Depois da variante nativa estar pronta, comparar a mesma apresentação com uma única ilha 3D Card. Verificar peer dependencies de `@astrojs/react`, React/React DOM e tipos contra Astro 7; não instalar Motion para esse experimento. Inspecionar também imports e usos de APIs exclusivas de Next.js nos exemplos: substituí-los por links e assets compatíveis, sem dependência de Next.

Usar `client:media="(hover: hover) and (pointer: fine) and (prefers-reduced-motion: no-preference)"` para o efeito opcional, mantendo HTML estático nas demais condições. O componente também deve observar mudanças de preferência e desativar comportamento já hidratado. Não combinar diretivas no mesmo nó. `client:visible` permanece alternativa para futuras ilhas abaixo da dobra, mas não pausa loops ao sair da tela. Não usar `client:only` para conteúdo.

Critérios cumulativos: ganho perceptível documentado em comparação visual; equivalência estática; teclado/toque corretos; no máximo +90 KiB gzip de JS próprio sobre a variante nativa; metas Lighthouse e mobile atendidas; nenhuma nova dependência React/Motion em carregamento direto de artigos. Contabilizar cada recurso do grafo adicional uma vez, incluindo renderer, runtime, shared chunks e imports adiados, não apenas bytes requisitados no primeiro viewport. Registrar separadamente código inline que mude.

Se algum critério falhar, entregar variante nativa, retirar dependências/arquivos exclusivos do experimento e registrar a decisão. Esse é um resultado previsto, sem necessidade de ampliar a change. Não acrescentar efeitos só para justificar dependências.

### 6. Enhancement e ciclo de navegação

Conteúdo deve nascer visível: aplicar estados de reveal somente após confirmar suporte e inicialização, com fallback sem JS. Menu mobile deve manter links acessíveis sem script, sem alterar desnecessariamente o comportamento interativo existente.

Tratar mount/unmount por navegação: registrar inicializações idempotentes, limpar observers, listeners, timers e frames antigos, e atualizar referências ao DOM. Preservar eventos de navegação do Astro já usados. Medições DOM somente no cliente; estados iniciais determinísticos; ids SVG únicos se reutilizados.

Movimento reduzido deve afetar CSS, scripts e chamadas explícitas de scroll suave. Efeitos contínuos, se algum sobreviver à seleção, devem pausar fora da tela e com aba oculta. Preferir transform/opacity e limitar áreas com filtros; não manter `will-change` global. Dimensões e proporções de assets são explícitas; imagens abaixo da dobra são lazy, sem lazy loading do recurso LCP.

### 7. Validação e interpretação das métricas

A spec `experience-quality` contém os limites. A baseline deve usar build fresco e registrar revisão ou hash do snapshot de fonte, versões de Node/navegador/Lighthouse, rota, viewport, rede/CPU e estado de consentimento. Não confundir `dist` antigo ou configuração CI com evidência. Usar três execuções frias por URL/perfil, mediana e dispersão; reproduzir condições no candidato. Não reduzir metas silenciosamente se a baseline não as atingir: corrigir, ou explicitar a limitação e discutir uma revisão antes de declarar aceite.

Matriz: home, About, posts, busca, 404 e artigos com matemática, MDX, código, tabelas e imagens; cobrir EN/PT-BR. Categorias, tags e séries continuam na suite de invariantes. Lighthouse desktop >=95 em todas as categorias aplicáveis, mantendo exceção SEO somente para 404. Mobile: queda máxima de 3 pontos Performance, aumento máximo de 10% LCP/TBT e 0,02 CLS; referência zero não admite aumento. Não tratar TBT como INP medido.

Screenshots nas larguras 320, 390, 768, 1280 e 1536; fontes estabilizadas, animações congeladas e terceiros mascarados/mocados na comparação visual, com smoke separado das integrações. Aprovar explicitamente alterações intencionais de baseline; não atualizar snapshots para esconder regressão. Verificar movimento em sessão própria, além dos snapshots.

Revisar teclado, nomes, foco, contraste, alvos, zoom/reflow, toque, sem JS, movimento reduzido e navegação repetida cinco vezes. Verificar no HTML e nos recursos que artigos não adquiriram runtime de portfólio. Usar navegador automatizado para screenshot e fluxos; escolher a ferramenta mínima compatível com CI, registrando sua versão na implementação. Reutilizar Vitest de conteúdo/site em vez de substituir a suite.

RUM: auditar `WebVitals.astro` e cobertura existente do GA4, unidades, consentimento e limites de medição após navegação. Manter coleta atual; não ampliar finalidade ou configurar serviços remotos. Preparar acompanhamento de 28 dias com p75 por dispositivo e suficiência documentada (contagem de observações e páginas, cobertura e lacunas). Quando disponível, confrontar CrUX; quando ausente, não inferir aprovação. Publicação e acesso a analytics exigem seu contexto/autorização próprios; a change pode concluir implementação e laboratório com campo explicitamente pendente.

## Risks / Trade-offs

- [CSS global e Tailwind em layers diferentes] -> Começar sem Preflight e validar seletores de artigo, formulários e navegação antes de expandir utilities.
- [Runtime maior que o benefício visual] -> Comparação nativa/React com grafo completo e retirada do experimento se reprovar.
- [Conteúdo profissional insuficiente] -> Curadoria com fontes; pelo menos um projeto comprovado; carreira opcional sem invenção.
- [Handlers acumulados no ClientRouter] -> Cleanup e ciclo repetido de navegação, incluindo retorno e troca de idioma.
- [Blur, repaint e movimento excessivos] -> Limitar superfície e quantidade de efeitos, medir CPU/GPU e desativar em movimento reduzido.
- [Lighthouse variável ou terceiros instáveis] -> Ambiente fixo, três execuções, dispersão registrada e testes visuais isolados dos terceiros.
- [Pouco tráfego e consentimento limitam RUM] -> Relatar cobertura e inconclusão sem afirmar p75 aprovado.
- [Código incorporado exige manutenção própria] -> Poucos componentes, origem/licença registradas e nenhuma promessa de compatibilidade integral do catálogo.

## Migration Plan

| Fase | Dependência | Entrega e aceite | Manutenção / risco |
|---|---|---|---|
| 0 Baseline | Navegador disponível | Relatórios, screenshots e inventário reproduzíveis | Baixa; ambiente e dist desatualizado |
| 1 Design system | Baseline | Tokens/Tailwind, tipografia e regressão editorial preservada | Média inicialmente; cascade |
| 2 Homepage | Fase 1 + curadoria | Composição EN/PT-BR, hero visível e projetos reais | Baixa/média; paridade |
| 3 Projetos e About | Dados tipados e fase 2 | Cards, carreira condicional e decisão do experimento | Média no experimento; editorial contínua |
| 4 Blog | Tokens estáveis | Cards/leitura com SEO e integrações preservados | Baixa/média; MDX/KaTeX |
| 5 Refinamento | Fases anteriores | Acesso, budgets, checks e evidências finais | Média; automação visual |

Cada fase permanece revisável e pode ser integrada sem remover conteúdo existente. Rodar comandos da aplicação em `site/apps`; usar `npm ci` somente durante implementação quando necessário, seguido de code-quality, build e testes. Acrescentar checks visuais/a11y/bundle aos gates existentes com escopo proporcional.

Entrega desta change termina no resultado local revisável. Não publicar automaticamente. Em uma publicação posteriormente autorizada, usar o fluxo de release e rollback existente baseado no SHA completo, sem alterar Terraform/DNS. Se houver regressão em produção, restaurar a release anterior pelo procedimento existente; o site não adquire migrações de dados ou serviços novos. A observação de campo permanece um acompanhamento pós-release separado.

## Open Questions

- Quais outros projetos, além do primeiro comprovado, merecem destaque? Isso ajusta a curadoria, não a arquitetura ou os requisitos mínimos.
- Há marcos profissionais confirmados com datas para a timeline? A alternativa sem marcos já está definida na spec.
- O efeito 3D oferece ganho suficiente na comparação? O protocolo e ambos os resultados possíveis estão definidos; nenhuma dependência é obrigatória antes da avaliação.

## References

Fontes consultadas na exploração anterior; revalidar imports/versões no experimento, pois registros podem mudar:
- [Catálogo Aceternity](https://ui.aceternity.com/components) e [catálogo para agentes](https://ui.aceternity.com/ai-recommendations).
- [3D Card: fonte](https://ui.aceternity.com/registry/3d-card.json), [Card Spotlight: fonte](https://ui.aceternity.com/registry/card-spotlight.json), [Canvas Reveal: fonte](https://ui.aceternity.com/registry/canvas-reveal-effect.json).
- [Spotlight](https://ui.aceternity.com/components/spotlight), [Bento Grid: fonte](https://ui.aceternity.com/registry/bento-grid.json), [Gradient Animation: fonte](https://ui.aceternity.com/registry/background-gradient-animation.json).
- [Astro React](https://docs.astro.build/en/guides/integrations-guide/react/), [diretivas](https://docs.astro.build/en/reference/directives-reference/), [Tailwind no Astro](https://docs.astro.build/en/guides/styling/#tailwind), [Preflight](https://tailwindcss.com/docs/preflight).
- [WCAG 2.2](https://www.w3.org/TR/WCAG22/) e [Web Vitals](https://web.dev/articles/vitals).
