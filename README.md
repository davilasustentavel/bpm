# BPM d'Avila — Mapa de Processos

Sistema web para gestão e visualização dos processos internos da **d'Avila Soluções Sustentáveis**. Permite cadastrar grupos de processos, os processos dentro de cada grupo e as atividades de cada processo, com controle de responsáveis, instruções de trabalho e estado de conclusão.

---

## Como acessar

Acesse diretamente pelo navegador em **[https://davilasustentavel.github.io/bpm/](https://davilasustentavel.github.io/bpm/)** ou abra o arquivo `index.html` localmente. O sistema se conecta automaticamente ao banco de dados em nuvem (Supabase) e exibe os dados em tempo real.

Ao carregar, o indicador no canto superior direito mostrará **● Online** (azul) quando a conexão for estabelecida, ou **● Offline** em caso de falha de rede.

---

## Estrutura dos dados

O sistema organiza as informações em três níveis hierárquicos:

```
Grupo de Processos
└── Processo
    └── Atividade
```

- **Grupo de Processos** — a categoria maior, como "Gestão Comercial" ou "Gestão Financeira"
- **Processo** — uma frente de trabalho dentro do grupo, como "Triagem de Oportunidades"
- **Atividade** — uma tarefa específica dentro do processo, com responsável, frequência, local, justificativa, passo a passo e resultado esperado

---

## Navegação

### Barra lateral (esquerda)

A barra lateral lista todos os grupos de processos cadastrados. Há duas abas:

- **Processos** — exibe a lista de grupos, com filtros por categoria
- **Pessoas** — exibe todos os colaboradores cadastrados no sistema

#### Filtros de categoria

- **Atividade Fim** — processos diretamente ligados à entrega de valor ao cliente
- **Apoio** — processos de suporte interno (financeiro, RH, TI, etc.)

Clique em um grupo para carregar seus processos e atividades na área principal.

### Área principal (direita)

Exibe os processos do grupo selecionado, cada um com sua tabela de atividades. No topo aparecem três indicadores rápidos: total de processos, total de atividades e quantidade de responsáveis distintos.

---

## Padrão de cores dos grupos

A cor de fundo de cada grupo na barra lateral indica seu estado:

| Cor | Significado |
|-----|-------------|
| **Branco** (padrão) | Grupo em andamento, sem pendências identificadas |
| **Amarelo** (`#fef3c7`) | Existe pelo menos um processo sem nenhuma atividade cadastrada |
| **Verde** (`var(--green-pale)`) | Grupo marcado como completo pelo gestor e sem processos vazios |

O grupo atualmente selecionado é indicado pelo **nome em negrito** — o fundo não muda com a seleção, preservando a leitura do estado real do grupo.

### Cor do processo na área principal

Quando um processo não possui nenhuma atividade cadastrada, seu cabeçalho recebe fundo amarelo e borda superior laranja — chamando atenção para a pendência.

---

## Operando o sistema

### Criar um grupo de processos

Clique em **+ Novo Grupo de Processos** no rodapé da barra lateral. Preencha:

- **Número** — código do grupo (ex: `01`). Será ajustado automaticamente ao reordenar
- **Categoria** — Atividade Fim ou Processo de Apoio
- **Nome** — nome do grupo em letras maiúsculas

### Editar ou excluir um grupo

Passe o mouse sobre o grupo na barra lateral para ver os botões de edição (✏️) e exclusão (🗑). A exclusão remove também todos os processos e atividades do grupo — a ação pede confirmação e não pode ser desfeita.

### Marcar um grupo como completo

Somente disponível ao **editar** um grupo existente. A opção aparece com um checkbox no modal de edição. Condições:

- O checkbox só fica disponível quando **todos os processos do grupo possuem pelo menos uma atividade**
- Se ainda houver processos vazios, o checkbox aparece desabilitado com uma mensagem explicativa
- Ao marcar como completo, o grupo recebe fundo verde e entra em **modo somente leitura**

### Modo somente leitura (grupo completo)

Quando um grupo está marcado como completo, **nenhuma alteração é permitida**:

- Não é possível adicionar novos processos
- Não é possível adicionar, editar ou excluir atividades
- Os botões de ação e os handles de reordenação são ocultados
- Um badge **✓ Grupo Completo** aparece no cabeçalho da área principal
- O botão **✏️ Editar Grupo** permanece visível — é por ele que o gestor desmarca a conclusão e retorna o grupo ao modo editável

### Criar um processo

Com um grupo selecionado, clique em **+ Novo Processo** no cabeçalho da área principal. Preencha nome, número e, opcionalmente, o dono do processo.

### Reordenar processos

Arraste o ícone ⠿ à esquerda do nome do processo para reposicioná-lo. Ao soltar, o sistema renumera automaticamente todos os processos do grupo em cascata.

### Criar uma atividade

Clique em **+ Atividade** no cabeçalho do processo desejado. O formulário aceita:

- **O Que** *(obrigatório)* — descrição da tarefa
- **Quem** — responsável (lista de pessoas cadastradas)
- **Quando** — frequência ou gatilho
- **Onde** — sistema ou local onde ocorre
- **Por Que** — justificativa
- **Como** — passo a passo
- **Saída** — resultado ou entrega esperada
- **Instrução de Trabalho** — código do documento de referência (ex: `IT-01`)

### Reordenar atividades

Arraste o ícone ⠿ da linha da atividade para reposicioná-la dentro do mesmo processo. A nova ordem é salva automaticamente.

### Recolher e expandir processos

Clique no chevron **›** à direita do cabeçalho do processo para recolhê-lo. O botão **⊟ Colapsar tudo / ⊞ Expandir tudo** no cabeçalho da página opera todos de uma vez. O estado de recolhimento é salvo localmente no navegador.

---

## Gerenciando pessoas

Na aba **Pessoas** da barra lateral é possível cadastrar, editar e excluir colaboradores. Cada pessoa tem nome, e-mail e função.

Ao excluir uma pessoa, ela é removida como responsável de todas as atividades em que estava vinculada — as atividades permanecem, apenas sem responsável.

---

## Notificações do sistema

Todas as operações de salvar, editar e excluir geram uma notificação discreta no canto inferior direito da tela:

| Cor da borda | Tipo |
|---|---|
| Azul | Sucesso |
| Vermelho | Erro |
| Laranja | Aviso |

As notificações desaparecem automaticamente após 3,5 segundos.

---

## Tecnologias utilizadas

- **HTML/CSS/JavaScript** puro — sem frameworks ou dependências de build
- **Supabase** — banco de dados PostgreSQL em nuvem com API REST
- **Google Fonts** — tipografia Nunito Sans
- Funciona em qualquer navegador moderno

---

*d'Avila Soluções Sustentáveis — Documento interno*
