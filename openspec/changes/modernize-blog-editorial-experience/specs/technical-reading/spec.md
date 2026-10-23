# Spec Delta

## ADDED Requirements

### Requirement: Editorial archive entry
A listagem de publicações SHALL oferecer destaque para a publicação mais recente, acesso à busca e aos assuntos, seguido de arquivo cronológico completo no idioma atual, sem exigir scripts para descobrir ou abrir artigos publicados.

#### Scenario: Browse the complete archive
- **WHEN** um visitante abre a listagem em inglês ou português
- **THEN** identifica a publicação mais recente, a busca e os assuntos disponíveis
- **AND** encontra cada publicação daquele idioma exatamente uma vez no arquivo por ano e mês, da mais recente para a mais antiga
- **AND** a publicação destacada também permanece no arquivo completo
- **AND** datas, descrições disponíveis e tempo de leitura ajudam a comparar entradas

#### Scenario: Small or empty archive
- **WHEN** o acervo publicado do idioma contém uma única publicação ou nenhuma
- **THEN** a apresentação se adapta sem destaque fictício ou seções vazias
- **AND** a ausência de publicações recebe mensagem localizada com navegação alternativa
- **AND** rascunhos não são promovidos ao destaque nem ao arquivo público

### Requirement: Discoverable topic indexes
Índices de categorias e tags SHALL apresentar assuntos publicados no idioma atual com nome legível, contagem de publicações e link para o destino existente, sem alterar identificadores públicos ou inventar descrições.

#### Scenario: Browse topics
- **WHEN** o visitante consulta categorias ou tags
- **THEN** encontra nomes compreensíveis e contagens consistentes com os artigos publicados daquele idioma
- **AND** assuntos conhecidos possuem apresentação localizada e termos desconhecidos conservam seu nome de origem
- **AND** o destino mantém o caminho público já existente

#### Scenario: Topic metadata unavailable
- **WHEN** um assunto não possui descrição editorial verificada ou o índice está vazio
- **THEN** descrições ausentes são omitidas e o índice vazio informa essa condição no idioma atual
- **AND** nenhum texto, contador ou card fictício preenche o espaço

### Requirement: Contextual topic results
Páginas de uma categoria ou tag SHALL mostrar contexto do assunto e publicações correspondentes em ordem cronológica decrescente, com a mesma hierarquia editorial da listagem e acesso de retorno ao índice correto.

#### Scenario: Read a topic archive
- **WHEN** o visitante segue um assunto a partir de uma publicação ou índice
- **THEN** reconhece o assunto, a quantidade e os artigos publicados correspondentes no idioma atual
- **AND** cada artigo apresenta título, data, descrição quando disponível e destino de leitura
- **AND** breadcrumb ou navegação equivalente permite retornar ao índice do mesmo tipo e idioma

### Requirement: Ordered series presentation
O catálogo e as páginas de séries SHALL apresentar título, descrição disponível, quantidade de artigos publicados e sequência cronológica de leitura, preservando os destinos e a associação existente entre artigos e séries.

#### Scenario: Follow a reading sequence
- **WHEN** um visitante abre uma série com artigos publicados
- **THEN** encontra os artigos do idioma atual em ordem cronológica crescente e com posição explícita
- **AND** consegue começar pelo primeiro artigo ou abrir qualquer outro diretamente
- **AND** a quantidade exibida corresponde à sequência disponível

#### Scenario: Series has no published articles
- **WHEN** uma série existente não tem artigos publicados no idioma atual
- **THEN** ela mantém sua rota com mensagem localizada e navegação de recuperação
- **AND** não oferece uma ação para começar sem destino
- **AND** o catálogo público continua omitindo séries sem artigos publicados

### Requirement: Integrated search states
A busca SHALL apresentar estados localizados e acessíveis de preparação, pronta para consulta, resultados, nenhum resultado e indisponibilidade, mantendo a consulta digitada durante a atualização e oferecendo caminhos alternativos para descobrir conteúdo.

#### Scenario: Search and refine
- **WHEN** um visitante consulta e refina uma busca
- **THEN** o campo permanece identificado e a consulta não é apagada pela atualização dos resultados
- **AND** consegue abrir resultados com teclado ou toque
- **AND** o estado sem resultados informa o ocorrido e permite refazer a consulta
- **AND** mudanças relevantes de estado são perceptíveis por tecnologia assistiva sem deslocar o foco à força

#### Scenario: Search assets fail or scripts are disabled
- **WHEN** o carregamento da busca falha ou JavaScript está desativado
- **THEN** o visitante encontra explicação adequada e links para publicações e assuntos do idioma atual
- **AND** uma falha de carregamento não deixa apenas uma área vazia nem remove a navegação alternativa

#### Scenario: Return to search through client navigation
- **WHEN** o visitante abre um resultado e retorna à busca usando a navegação do site
- **THEN** o campo e os resultados permanecem operáveis
- **AND** não aparecem interfaces duplicadas, callbacks de páginas abandonadas ou erros introduzidos pela modernização

### Requirement: Informative publication masthead
A abertura de uma publicação SHALL apresentar título, descrição quando disponível, autoria, data, tempo de leitura e contexto aplicável de atualização, assunto, série e tradução, com hierarquia clara e sem depender de animação ou imagem de capa.

#### Scenario: Open a publication
- **WHEN** um leitor abre um artigo publicado em qualquer idioma
- **THEN** identifica título, autor, data e tempo estimado de leitura antes do corpo
- **AND** a descrição autoral existente aparece como introdução visual sem alterar o texto da publicação
- **AND** atualização, série e tradução são exibidas somente quando aplicáveis e levam aos destinos corretos

#### Scenario: Long title or absent description
- **WHEN** uma publicação tem título longo, muitas tags ou não possui descrição nem capa
- **THEN** o cabeçalho se adapta sem corte, sobreposição ou rolagem horizontal da página
- **AND** não substitui informações ausentes por descrição, imagem ou crédito inventado

#### Scenario: Consistent authored dates
- **WHEN** a mesma publicação aparece no destaque, arquivo, assunto, série e cabeçalho do artigo
- **THEN** a data visível representa o mesmo dia autoral em todos esses locais
- **AND** o fuso da máquina de build não altera esse dia nem as datas estruturadas de publicação e atualização

### Requirement: Adaptive reading navigation
Publicações com pelo menos três títulos de nível dois ou três SHALL oferecer sumário acessível aos mesmos destinos do texto, adaptando sua posição ao espaço disponível sem reduzir a coluna de leitura a uma largura desconfortável.

#### Scenario: Read with a lateral table of contents
- **WHEN** há espaço suficiente para corpo e sumário lado a lado
- **THEN** o sumário lateral permanece utilizável durante a leitura sem sobrepor texto ou controles
- **AND** todos os seus links continuam alcançáveis quando o sumário é maior que a altura disponível
- **AND** âncoras de destino não ficam ocultas pelo cabeçalho do site

#### Scenario: Read on a narrow screen without scripts
- **WHEN** o espaço não comporta a apresentação lateral, inclusive com JavaScript desativado
- **THEN** o sumário permanece disponível em controle nativo recolhível antes do texto
- **AND** abrir, fechar e seguir suas âncoras funciona por teclado e toque
- **AND** artigos com menos de três títulos elegíveis não exibem sumário vazio

### Requirement: Readable technical surfaces
Código, fórmulas, tabelas, imagens, citações e callouts SHALL manter hierarquia, contraste e espaçamento coerentes com a apresentação editorial, preservando seu conteúdo, semântica e controles aplicáveis.

#### Scenario: Read dense technical content
- **WHEN** o visitante lê conteúdo com código longo, tabela larga, fórmula ou imagem
- **THEN** o corpo permanece limitado a 68ch e elementos maiores se adaptam ou possuem rolagem local
- **AND** código pode ser selecionado e seus controles de cópia permanecem visíveis ao receber foco
- **AND** a decoração não cobre texto, sintaxe, legendas ou controles

### Requirement: Coherent publication continuation
O encerramento de uma publicação SHALL distinguir continuidade de leitura, compartilhamento, assinatura e comentários, preservando as ações existentes e evitando controles vazios ou concorrentes sem contexto.

#### Scenario: Continue a series or chronological reading
- **WHEN** o leitor chega ao final de uma publicação
- **THEN** identifica a navegação da série quando houver destinos ou a navegação cronológica aplicável fora de séries
- **AND** relacionados disponíveis permanecem acessíveis e não incluem a própria publicação
- **AND** limites de sequência não produzem botões sem destino
- **AND** os destinos de compartilhamento existentes, newsletter e comentários continuam disponíveis conforme sua configuração

#### Scenario: Repeat editorial interactions
- **WHEN** o leitor alterna entre duas publicações e idiomas e usa cópia de código ou link
- **THEN** a ação corresponde à publicação atual e executa uma única vez
- **AND** textos de interface e feedback seguem o idioma atual
- **AND** reorganizar a apresentação não dispara envio de formulário, comentário ou compartilhamento por conta própria
