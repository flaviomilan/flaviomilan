# site-design-system Specification

## Purpose

Oferecer uma identidade visual consistente para apresentação profissional e leitura técnica, com profundidade discreta, hierarquia clara e adaptação acessível a diferentes dispositivos.

## Requirements

### Requirement: Consistent dark visual hierarchy
O site SHALL apresentar fundos escuros em planos distinguíveis, tipografia hierárquica e acentos luminosos discretos, mantendo aparência consistente entre páginas e idiomas.

#### Scenario: Transition between portfolio and article
- **WHEN** o visitante navega da homepage para um artigo ou About
- **THEN** cores, tipografia, foco e elementos de navegação permanecem visualmente coerentes
- **AND** o artigo usa uma composição de leitura própria, sem herdar a largura ampla do hero

### Requirement: Responsive composition
O site SHALL apresentar conteúdo e controles utilizáveis nas larguras de 320, 390, 768, 1280 e 1536 pixels CSS, sem rolagem horizontal da página.

#### Scenario: Narrow viewport
- **WHEN** a homepage ou About é exibida com largura de 320 pixels CSS
- **THEN** títulos, cards, controles e textos não se sobrepõem nem são cortados
- **AND** a ordem visual preserva a ordem significativa do conteúdo

#### Scenario: Locally scrollable technical content
- **WHEN** um artigo contém código, fórmula ou tabela maior que a largura disponível
- **THEN** o conteúdo permanece acessível por adaptação ou rolagem local sem alargar a página

### Requirement: Functional and restrained motion
O site SHALL usar animação somente como decoração secundária, resposta a interação ou indicação de estado, sem atrasar a leitura nem ocultar conteúdo essencial.

#### Scenario: Initial hero rendering
- **WHEN** a homepage é carregada
- **THEN** nome, mensagem profissional e chamadas de ação estão visíveis antes de qualquer animação terminar
- **AND** existe no máximo um tratamento decorativo animado dominante no hero

### Requirement: Reduced motion experience
O site SHALL respeitar a preferência de movimento reduzido em efeitos CSS, efeitos executados por script e rolagem, preservando todo o conteúdo e as ações.

#### Scenario: Reduced motion enabled before loading
- **WHEN** o visitante carrega uma página com preferência de movimento reduzido ativa
- **THEN** efeitos de inclinação, parallax e loops decorativos permanecem desativados
- **AND** entradas animadas não deixam elementos invisíveis

#### Scenario: Preference changes during the visit
- **WHEN** o visitante ativa movimento reduzido durante a sessão
- **THEN** os efeitos decorativos ativos param e a interface permanece utilizável
