# Tasks

## 1. Baseline editorial e cobertura de auditoria

- [x] 1.1 Registrar o estado de trabalho e hash das fontes/config/lockfile em `site/docs/blog-editorial-validation.md`, gerar build fresco e executar code-quality e Vitest; verificar que a baseline corresponde ao estado atual e preserva alterações anteriores, sem depender apenas de HEAD.
- [x] 1.2 Ampliar a matriz e o coletor existentes para índices/detalhes de categorias, tags e séries EN/PT-BR, incluindo série vazia real e regressão home/About/404; verificar que cada rota selecionada existe e que os mesmos comandos funcionarão para baseline e candidato.
- [x] 1.3 Preservar o build inicial e capturar a matriz nas cinco larguras, inventário de recursos e três execuções Lighthouse desktop/mobile por rota; registrar medianas, dispersão, ambiente, consentimento e isolamento de terceiros no relatório, sem executar build ou testes concorrentes durante medição.

## 2. Fundamentos editoriais compartilhados

- [x] 2.1 Criar helpers compartilhados de seleção de publicados, destaque mais recente, agrupamento, contagens, sequência e apresentação UTC de datas; verificar com testes de idiomas, rascunhos, empate de datas, datas próximas à meia-noite e coleções vazias.
- [x] 2.2 Criar cabeçalho editorial e entradas compactas reutilizáveis com CSS escopado e tokens existentes; adaptar ArticleCard somente onde necessário para headings semânticos, verificando build, ordem de headings e ausência de regressão na composição da homepage.
- [x] 2.3 Adicionar labels e estados editoriais às duas traduções, com fallback para termos técnicos desconhecidos e sem alterar URLs; verificar paridade de chaves e renderização de textos longos, ausência de descrição e índice vazio.
- [x] 2.4 Documentar no guia editorial a responsabilidade dos componentes, regras de datas/contagens, fallback de assuntos e limites de CSS global; verificar que os exemplos correspondem às APIs efetivamente criadas e não exigem mudar conteúdo ou instalar dependências.

## 3. Listagens, assuntos e séries

- [x] 3.1 Migrar as duas listagens para composição compartilhada com abertura editorial, destaque mais recente, acesso à busca/assuntos e arquivo integral por ano/mês; verificar em HTML gerado o inventário exato de publicados, unicidade dentro do arquivo, destaque correto e comportamento de zero/um item por fixtures.
- [x] 3.2 Modernizar índices de categorias/tags EN/PT-BR com labels, contagens e destinos existentes; verificar contra as coleções que rascunhos não entram nas contagens, termos desconhecidos preservam seu nome e não há descrições inventadas.
- [x] 3.3 Modernizar detalhes de categorias/tags com contexto, breadcrumb e lista compartilhada; verificar artigos esperados, data decrescente, idioma dos links e preservação de caminhos/metadados em testes do site gerado.
- [x] 3.4 Modernizar catálogo/detalhe de séries com descrições disponíveis, contagens e sequência numerada; verificar ordem crescente, CTA inicial válido, omissão de séries vazias no catálogo e recuperação das rotas vazias existentes.
- [x] 3.5 Validar cada família migrada nas cinco larguras e com teclado/sem JS nos dois idiomas, incluindo títulos longos e estados vazios; registrar capturas e decisões visuais no guia e corrigir overflow, hierarquia ou foco antes de avançar.

## 4. Publicação e navegação de leitura

- [x] 4.1 Extrair a apresentação do cabeçalho de PostLayout com alinhamento editorial, descrição opcional, autor e metadados hierárquicos; verificar campos presentes/ausentes, datas consistentes entre contextos, traduções, rascunhos e preservação de schema/OG/canonical sem editar Markdown.
- [x] 4.2 Implementar composição de leitura com até 68ch e sumário nativo único, lateral quando couber e empilhado nas demais larguras; verificar limiar de três headings, teclado, âncoras, offset do header, sumário maior que a viewport e reflow/zoom sem sobreposição.
- [x] 4.3 Refinar headings, parágrafos, listas, código, tabelas, KaTeX, imagens, citações e callouts, retirando a capitular automática indiscriminada; verificar screenshots de conteúdo denso, seleção/cópia e overflow local sem alterar processamento ou texto dos artigos.
- [x] 4.4 Reorganizar navegação, relacionados, compartilhamento, newsletter e comentários em encerramento coerente; verificar primeiro/último item de série, artigo fora de série, ausência de relacionados e todos os destinos existentes, sem enviar formulários ou ações reais.
- [x] 4.5 Adicionar invariantes e fluxos de leitura aos testes existentes e documentar a composição/cleanup no guia; verificar artigos diretos e navegação ClientRouter com cópia associada à publicação atual e sem handlers duplicados.

## 5. Busca integrada

- [x] 5.1 Compartilhar composição da busca EN/PT-BR e estilizar Pagefind com estados de preparação, pronta, resultados e ausência de resultados, mantendo consulta e foco durante atualizações; verificar o comportamento com assets instalados e testes de teclado/idioma em navegador.
- [x] 5.2 Apresentar falha de carregamento e alternativa noscript com links estáticos localizados fora do contêiner substituído; verificar bloqueio simulado de assets, contexto sem JS e saída/retorno durante carregamento sem UI duplicada ou callbacks obsoletos.
- [x] 5.3 Incorporar os estados e navegação repetida da busca ao runner existente e documentar sua fronteira de cliente e limitações; verificar testes com resultados/sem resultados/falha e que nenhum filtro, persistência ou motor novo foi introduzido.

## 6. Footer e recuperação de navegação

- [x] 6.1 Refinar a composição do footer agrupando navegação editorial, RSS e perfis sem perder destinos; verificar links/idiomas e capturas nas cinco larguras inclusive home/About, com foco visível e sem sobreposição ao consentimento.
- [x] 6.2 Harmonizar a apresentação 404 e ações de recuperação sem alterar noindex, rotas ou comportamento atual por idioma; verificar HTML e navegação de recuperação em contextos EN/PT-BR e preservar o contrato HTTP da infraestrutura existente.
- [x] 6.3 Cobrir footer/404 com invariantes de destinos e fluxos de teclado e documentar os efeitos globais intencionais; verificar ausência de mudança de serviços, conteúdo e navegação principal.

## 7. Aceite integrado e evidências finais

- [x] 7.1 Executar code-quality, build fresco, Vitest, Nu HTML, sitemap HTTP local e inventário de recursos; registrar resultados e comprovar preservação de conteúdo, rotas, traduções, feeds, índice, OG e rascunhos, sem runtime React/Motion novo.
- [x] 7.2 Executar matriz final de screenshots nas cinco larguras e dois idiomas, estados de busca e sumário, movimento reduzido e sem JS; combinar axe sem violações sérias/críticas com revisão manual WCAG 2.2 AA de contraste, nomes, headings, teclado, foco, alvos e reflow, registrando aceite visual separado de regressões.
- [x] 7.3 Executar cinco ciclos completos listagem/artigo/assunto/série/busca/idioma/retorno, com integrações e clipboard mockados; verificar ausência de erros novos, handlers duplicados e trabalho abandonado, além de destinos e cópia corretos sem envios externos reais.
- [x] 7.4 Executar três medições frias por rota/perfil do candidato com o mesmo coletor e condições da baseline, exigir os limites desktop/mobile de experience-quality e integrar a matriz ao CI; registrar conjunto completo, medianas, dispersão e qualquer repetição justificada sem reduzir metas ou selecionar melhores execuções isoladas.
- [x] 7.5 Entregar `site/docs/blog-editorial-results.md` com comparação de baseline, capturas, recursos, verificações, limitações e pendências reais; verificar que nenhum resultado de campo p75 é declarado medido e que não houve deploy, mantendo o procedimento de observação futura já existente.

## Workflow follow-up

- Revisar os artefatos e iniciar apply somente mediante solicitação explícita.
- Após implementação e revisão, sincronizar specs e arquivar mediante solicitação; publicação continua sendo uma operação separada.
