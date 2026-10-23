# Spec Delta

## ADDED Requirements

### Requirement: Shared editorial page composition
Listagens, assuntos, séries, busca e publicações SHALL compartilhar linguagem editorial de tipografia, cores, superfícies, espaçamento, metadados e foco, com composição adequada à descoberta ou à leitura e equivalente nos dois idiomas.

#### Scenario: Navigate across editorial page families
- **WHEN** o visitante percorre listagem, assunto, série, busca e publicação
- **THEN** reconhece os mesmos padrões visuais de cabeçalho, links e metadados
- **AND** páginas de descoberta usam o espaço para comparação de entradas enquanto o corpo dos artigos permanece em até 68ch
- **AND** a hierarquia é perceptível sem depender de hover, animação, capas ou cores isoladas

#### Scenario: Narrow layout and long localized text
- **WHEN** essas páginas são exibidas em 320, 390, 768, 1280 e 1536 pixels CSS nos dois idiomas
- **THEN** títulos longos, contagens, descrições e controles se adaptam sem sobreposição ou overflow da página
- **AND** hierarquia de headings e ordem de foco permanecem significativas
- **AND** texto e controles essenciais ficam disponíveis sem JavaScript e com movimento reduzido, ressalvada a consulta dinâmica da busca com alternativa de navegação

### Requirement: Coherent footer and recovery presentation
O footer compartilhado e a página 404 SHALL acompanhar a identidade editorial, mantendo links, informações existentes e caminhos claros de navegação e recuperação.

#### Scenario: Reach the shared footer
- **WHEN** o visitante chega ao footer de uma página editorial, homepage ou About
- **THEN** distingue navegação editorial, RSS e perfis externos sem perda dos destinos existentes
- **AND** links internos seguem o idioma atual e os controles têm foco visível
- **AND** a composição se adapta às cinco larguras de referência

#### Scenario: Recover from a missing route
- **WHEN** o visitante recebe a página 404
- **THEN** identifica o estado de erro e acessa os caminhos existentes para início e publicações
- **AND** a apresentação conserva noindex e o comportamento atual de recuperação por idioma
- **AND** a modernização visual não altera a resolução de rotas ou o status HTTP configurado na publicação
