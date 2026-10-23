# experience-quality Specification

## Purpose

Manter uma experiência rápida, acessível e resiliente a falhas de scripts, com critérios reproduzíveis que distinguem qualidade medida em laboratório de resultados observados entre usuários reais.

## Requirements

### Requirement: Essential content without client scripts
Identidade, projetos, artigos e navegação básica SHALL estar disponíveis no HTML inicial e permanecer visíveis e utilizáveis com JavaScript desativado ou com falha no carregamento de enhancement.

#### Scenario: JavaScript disabled
- **WHEN** um visitante abre homepage, About, listagem ou artigo com JavaScript desativado
- **THEN** consegue ler o conteúdo principal e seguir links de navegação
- **AND** seções de conteúdo não permanecem transparentes ou escondidas

### Requirement: Keyboard and assistive access
A interface modernizada SHALL atender WCAG 2.2 AA, oferecer ordem de foco significativa, foco visível não encoberto e controles com nomes acessíveis, sem depender exclusivamente de ponteiro.

#### Scenario: Keyboard navigation
- **WHEN** o visitante percorre a página e abre e fecha o menu usando teclado
- **THEN** todos os controles são alcançáveis e operáveis
- **AND** o foco permanece visível e retorna ao acionador ao fechar o menu

#### Scenario: Contrast and reflow audit
- **WHEN** são auditados texto, controles, zoom e reflow nas páginas representativas
- **THEN** texto normal alcança contraste de 4,5:1 e texto grande de 3:1
- **AND** contraste não textual aplicável, alvos de interação e demais critérios AA são verificados
- **AND** a revisão combina automação sem violações sérias ou críticas e inspeção manual

### Requirement: Bounded optional interactivity
Efeitos opcionais SHALL carregar somente nas rotas e condições que os utilizam, preservar a apresentação estática e interromper trabalho decorativo contínuo quando a página está oculta ou o efeito fora da tela.

#### Scenario: Read an article directly
- **WHEN** um visitante carrega diretamente um artigo
- **THEN** efeitos exclusivos do portfólio não introduzem seu runtime de interface ou animação nessa rota

#### Scenario: Leave an interactive effect
- **WHEN** um efeito decorativo ativo deixa a área visível ou a aba fica oculta
- **THEN** seu loop decorativo contínuo para
- **AND** ao retornar, a apresentação e os links continuam corretos

### Requirement: Measured resource budget
O experimento interativo SHALL adicionar no máximo 90 KiB gzip de JavaScript próprio por rota consumidora em relação à variante nativa equivalente, contando chunks compartilhados e adiados uma vez por recurso.

#### Scenario: Evaluate the interactive variant
- **WHEN** a variante interativa é comparada ao mesmo build com cards nativos
- **THEN** o relatório contabiliza o grafo completo de recursos adicionais, inclusive carregamentos adiados
- **AND** a variante é descartada se exceder o orçamento ou falhar nos demais critérios de qualidade

### Requirement: Reproducible laboratory quality
Páginas representativas SHALL alcançar mediana de pelo menos 95 em Performance, Accessibility, Best Practices e SEO no Lighthouse desktop, em três execuções por URL com ambiente fixo. A página 404 SHALL ser dispensada somente da meta SEO por ser intencionalmente noindex.

#### Scenario: Compare baseline and candidate
- **WHEN** a homepage, About, listagem, busca e artigos representativos são auditados nos dois idiomas
- **THEN** baseline e candidato usam a mesma versão de navegador, Lighthouse, perfil de rede e CPU
- **AND** o relatório registra notas, dispersão e limitações sem confundir limites configurados com resultados

#### Scenario: Mobile regression
- **WHEN** as mesmas rotas são auditadas com perfil mobile fixo
- **THEN** a mediana Performance não cai mais de 3 pontos contra a baseline
- **AND** medianas LCP e TBT não pioram mais de 10%, nem CLS mais de 0,02
- **AND** quando o valor de referência é zero, o candidato não introduz regressão nessa métrica

### Requirement: Field metrics and honest evidence
O acompanhamento real SHALL usar metas p75 de LCP ≤2,5 s, INP ≤200 ms e CLS ≤0,1 por dispositivo, sem apresentar resultados de laboratório como comprovação de campo.

#### Scenario: Sufficient field observations
- **WHEN** existe uma janela de 28 dias com cobertura e amostra suficientes documentadas
- **THEN** o acompanhamento calcula p75 separado por dispositivo e identifica versão, período e limitações de coleta

#### Scenario: Insufficient or unavailable field data
- **WHEN** publicação ainda não foi autorizada ou os dados reais são insuficientes
- **THEN** o relatório declara as metas de campo não verificadas e registra o plano de acompanhamento
- **AND** não inventa resultados nem condiciona a conclusão dos checks locais a dados futuros

### Requirement: Stable repeated navigation
A navegação entre páginas SHALL manter interações funcionais sem listeners duplicados, observers abandonados, erros de hidratação ou efeitos que continuem ativos após sair da página.

#### Scenario: Repeat a bilingual navigation cycle
- **WHEN** o visitante repete cinco vezes o fluxo homepage, artigo, busca, troca de idioma e retorno
- **THEN** controles continuam corretos, não há novos erros de console atribuíveis à mudança e handlers não se multiplicam
