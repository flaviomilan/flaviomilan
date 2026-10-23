# Tasks

Todos os itens abaixo são trabalho futuro de implementação. Executar comandos da aplicação em `site/apps/`. Os requisitos estão nas quatro specs; as escolhas técnicas, critérios do experimento e os resultados condicionais estão em `design.md`.

## 1. Baseline reproduzível e curadoria

- [x] 1.1 Preparar navegador e ferramentas locais de auditoria com versões fixas, sem alterar produção; verificar que o navegador abre o preview e registrar comandos e versões em `site/docs/visual-experience-validation.md`.
- [x] 1.2 Gerar build fresco antes de alterações visuais e executar code-quality e Vitest; registrar revisão/hash das fontes, inventário de rotas/recursos e falhas pré-existentes no relatório de baseline.
- [x] 1.3 Capturar screenshots da matriz definida no design e três execuções Lighthouse por URL/perfil desktop e mobile; verificar que relatórios registram medianas, dispersão, viewport, rede/CPU e consentimento, sem reutilizar dist antigo como baseline.
- [x] 1.4 Confirmar pelo menos um projeto por fontes do repositório e/ou documentação pública, verificando autoria, papel, decisões e destino; registrar evidências e textos EN/PT-BR e solicitar somente fatos obrigatórios que estiverem ausentes, sem placeholders.
- [x] 1.5 Verificar disponibilidade de marcos profissionais confirmados e registrar a decisão entre timeline e apresentação sem marcos; concluir somente com fontes ou ausência explícita, sem inferir datas de publicações.

## 2. Design system e estilos

- [x] 2.1 Adicionar Tailwind 4 e plugin Vite compatíveis com Astro 7 durante apply, sem Preflight global; verificar build e type-check sem mudança de output estático ou introdução de React.
- [x] 2.2 Evoluir tokens de cor, planos, espaçamento, foco, motion e largura em global.css e mapear utilities à mesma fonte; verificar cascade com artigos, listas, tabelas, formulários e navegação nas capturas de regressão.
- [x] 2.3 Refinar hierarquia com Inter/JetBrains Mono existentes, resolver peso 800 versus fontes disponíveis e revisar preloads; verificar fontes/requisições no navegador e ausência de saltos de layout atribuíveis à mudança.
- [x] 2.4 Tornar conteúdo .reveal visível por padrão e aplicar enhancement somente após inicialização; verificar homepage, About, listagem e artigo com JavaScript desativado e com inicializador indisponível.
- [x] 2.5 Implementar movimento reduzido em CSS, efeitos e scroll explícito; verificar preferência ativa antes da carga e mudança em tempo de execução, preservando visibilidade e ações.
- [x] 2.6 Documentar tokens, integração Tailwind sem Preflight e limites de efeitos no guia de validação; verificar exemplos e capturas nas cinco larguras da spec antes de expandir componentes.

## 3. Dados e homepage bilíngue

- [x] 3.1 Criar modelo tipado de projetos com textos EN/PT-BR, campos técnicos, links, opcionais e referência de evidência; verificar testes de entrada completa, tradução obrigatória ausente e opcionais omitidos, além de falha de validação para destaque inválido.
- [x] 3.2 Criar composição HomeContent compartilhada pelas duas rotas usando helpers i18n existentes; verificar testes de HTML para paridade de projetos, datas, links e destinos localizados.
- [x] 3.3 Implementar hero com identidade, frase fixa, CTAs para #projects e posts, Spotlight SVG/CSS e grid estático; verificar HTML inicial, teclado e capturas sem atrasar H1 ou CTAs para animar.
- [x] 3.4 Implementar ProjectCard/ProjectGrid Astro nativos com descrições legíveis, labels e links independentes de hover; verificar conteúdo real, imagens dimensionadas quando presentes, uma única entrada sem preenchimento fictício e layout nas cinco larguras.
- [x] 3.5 Reintegrar publicações recentes, temas e RSS na homepage; verificar invariantes dos idiomas e snapshots, documentando a curadoria de projetos e revisão de alegações no guia.

## 4. About e avaliação da ilha opcional

- [x] 4.1 Refinar About EN/PT-BR preservando apresentação e contatos; quando houver marcos confirmados, adicionar dados tipados e lista cronológica, e caso contrário manter apresentação sem timeline; verificar os dois estados com testes e capturas.
- [x] 4.2 Revalidar fonte, licença e dependências do 3D Card e versões compatíveis da integração React; registrar inventário de imports e confirmar que o candidato não exige Next.js, Motion ou WebGL antes de incorporá-lo ao experimento.
- [x] 4.3 Montar variante experimental com uma única ilha React, adaptador Astro e código externo isolado, usando a condição de media definida no design; verificar SSR determinístico, conteúdo sem JS, ponteiro preciso, toque, teclado e mudança de movimento reduzido.
- [x] 4.4 Comparar variante nativa e experimental com a mesma apresentação, contabilizar runtime/chunks adicionais gzip e executar auditorias; verificar limite de +90 KiB, metas de qualidade, ausência de React/Motion em artigos e ganho visual documentado.
- [x] 4.5 Registrar a decisão de adoção: manter somente a ilha que cumprir todos os critérios ou remover código e dependências exclusivos do experimento e entregar a versão nativa; verificar manifesto, lockfile, configuração, grafo final e evidências da decisão.

## 5. Leitura e navegação editorial

- [x] 5.1 Refinar ArticleCard e listagens reutilizando conteúdo/i18n existentes, sem efeitos no corpo; verificar título, data, descrição, teclado/toque e coluna de leitura de até 68ch com capturas desktop/mobile.
- [x] 5.2 Validar e ajustar matemática, código, tabelas e imagens somente onde o novo estilo exigir; verificar artigos MDX/Markdown representativos sem overflow da página e registrar capturas locais de elementos largos.
- [x] 5.3 Refinar Header mantendo navegação sticky visível e links utilizáveis sem JS, com menu acessível e foco restaurado; verificar testes de teclado, Escape, toque e idioma, incluindo viewport 320 px.
- [x] 5.4 Revisar lifecycle dos inicializadores afetados e limpar listeners/observers/timers; verificar cinco ciclos home/artigo/busca/idioma/retorno sem handlers duplicados, erros de hidratação ou efeitos abandonados.
- [x] 5.5 Preservar SEO e contratos de conteúdo executando os testes existentes de rotas, traduções, links, feeds, sitemap, assets e metadados após build; verificar também sumário, relacionados, série, cópia, comentários e newsletter sem enviar formulários ou mensagens reais.
- [x] 5.6 Documentar manutenção da fronteira editorial e inicialização/cleanup no guia de validação; verificar que a documentação acompanha os componentes efetivamente modificados, sem reescrever conteúdo dos artigos.

## 6. Integração e evidências de aceite

- [x] 6.1 Consolidar os checks visuais/a11y/recursos das fases anteriores no CI existente e configurar Lighthouse desktop com três execuções, medianas >=95 e exceção SEO apenas para 404; verificar uma execução local equivalente aos comandos CI e critérios mobile da spec.
- [x] 6.2 Executar revisão integrada WCAG 2.2 AA com automação sem achados sérios/críticos e inspeção manual de contraste, nomes, foco, alvos, zoom/reflow, teclado e toque; registrar resultados por rota e corrigir falhas atribuíveis à change.
- [x] 6.3 Executar matriz visual final nas cinco larguras, dois idiomas, menu aberto, sem JS e movimento reduzido; verificar diffs intencionais separadamente de regressões e registrar aceite visual e inspeção própria de motion/CPU/GPU.
- [x] 6.4 Auditar coleta existente de Web Vitals, unidades, consentimento e limitações após navegação, sem configurar serviços remotos; entregar procedimento de observação de 28 dias com p75 por dispositivo, contagens/cobertura e estado explícito de campo não verificado.
- [x] 6.5 Executar code-quality, build, Vitest, validação HTML e todas as auditorias novas no resultado final; entregar relatório com comparação à baseline, grafo de recursos, decisão React e pendências reais, sem declarar metas não medidas como aprovadas.

## Workflow follow-up

- Revisar os artefatos e iniciar explicitamente o workflow apply antes de executar qualquer tarefa acima.
- Publicar somente mediante autorização posterior, pelo fluxo de release existente; usar o rollback existente se houver regressão, sem alterar infraestrutura.
- Após publicação autorizada e acesso aos dados, observar a janela de campo e registrar metas atingidas ou resultado inconclusivo conforme cobertura; esta observação futura não integra os checkboxes locais.
- Arquivar a change somente quando o trabalho de implementação e seus critérios locais forem concluídos; o planejamento pronto não significa implementação concluída.
