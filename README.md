# BPM d'Avila — Mapa de Processos · REV 02.1

Sistema web para gestão e visualização dos processos internos da **d'Avila Soluções Sustentáveis**. Permite cadastrar grupos de processos, processos e atividades, com controle de responsáveis, instruções de trabalho, campos descritivos de entrada/saída, painel de detalhes lateral, estado de conclusão e controle de acesso por perfil de usuário.

---

## Como acessar

Acesse pelo navegador em **[https://davilasustentavel.github.io/bpm/](https://davilasustentavel.github.io/bpm/)** ou abra o arquivo `index.html` localmente.

O sistema se conecta automaticamente ao banco de dados em nuvem (Supabase). Ao carregar, o indicador no canto superior direito mostrará **● Online** quando a conexão for estabelecida, ou **● Offline** em caso de falha de rede.

**Qualquer pessoa pode visualizar o mapa de processos sem precisar de login.** O login é necessário apenas para realizar alterações.

---

## Situação atual do banco de dados

| Item | Quantidade |
|---|---|
| Grupos de processos | 17 |
| Processos cadastrados | 55 |
| Atividades cadastradas | 185 |
| Grupos marcados como completos | 2 (Gestão de Propostas · Gestão de Contas a Pagar) |

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

### Campos de cada processo

| Campo | Obrigatório | Descrição |
|---|---|---|
| **Número** | Sim | Código sequencial (ex: `01.1`). Ajustado automaticamente ao reordenar |
| **Nome** | Sim | Nome do processo |
| **Dono** | Não | Responsável pela gestão do processo |
| **Descrição** | Não | Texto livre explicando o objetivo e contexto do processo |
| **Entrada** | Não | O que inicia ou alimenta o processo |
| **Saída** | Não | O produto ou resultado gerado pelo processo |

### Campos de cada atividade

| Campo | Obrigatório | Descrição |
|---|---|---|
| **O Que** | Sim | Descrição da tarefa |
| **Quem** | Não | Responsável (lista de pessoas cadastradas) |
| **Quando** | Não | Frequência ou gatilho |
| **Onde** | Não | Sistema ou local onde ocorre |
| **Por Que** | Não | Justificativa da atividade |
| **Como** | Não | Passo a passo de execução |
| **Saída** | Não | Resultado ou entrega esperada |
| **Instrução de Trabalho** | Não | Código do documento de referência (ex: `IT-01`) |

---

## Controle de acesso

### Perfis de usuário

| Perfil | O que pode fazer |
|---|---|
| **Visitante** (sem login) | Visualizar grupos, processos e atividades. A aba Pessoas fica oculta |
| **Usuário ativo** | Visualizar + criar, editar e excluir grupos, processos e atividades |
| **Administrador** | Tudo acima + gerenciar a tabela de pessoas (cadastrar, editar, desabilitar, promover a admin) |

### Como fazer login

Clique em **🔐 Entrar** no canto superior direito do cabeçalho. Informe o e-mail e a senha cadastrados no sistema. Pressionar **Enter** no campo de senha também confirma o acesso.

Ao entrar, o nome do usuário aparece no cabeçalho e os controles de edição ficam visíveis. Para sair, clique em **Sair**.

### Segurança no banco de dados

O banco utiliza **Row Level Security (RLS)** com as seguintes regras:

- **Leitura** — liberada para qualquer visitante, sem autenticação
- **Escrita em grupos, processos e atividades** — requer usuário autenticado com `ativo = true`
- **Escrita na tabela de pessoas** — requer usuário com `admin = true`

---

## Navegação

### Barra lateral (esquerda)

A barra lateral lista todos os grupos de processos cadastrados. Há duas abas:

- **Processos** — exibe a lista de grupos, com filtros por categoria
- **Pessoas** — exibe todos os colaboradores cadastrados (visível apenas quando logado)

#### Filtros de categoria

- **Atividade Fim** — processos diretamente ligados à entrega de valor ao cliente
- **Apoio** — processos de suporte interno (financeiro, RH, TI, etc.)

Clique em um grupo para carregar seus processos e atividades na área principal. O grupo selecionado é indicado pelo **nome em negrito** — a cor de fundo não muda com a seleção, preservando a leitura do estado real do grupo.

### Área principal (direita)

Exibe os processos do grupo selecionado, cada um com sua tabela de atividades. No topo aparecem três indicadores rápidos: total de processos, total de atividades e quantidade de responsáveis distintos.

### Painel lateral de detalhes

Ao clicar no **nome de um processo** ou no **nome de uma atividade** (sublinhado pontilhado), um painel desliza da direita exibindo todos os campos daquele item:

- **Clique no nome do processo** — exibe grupo de origem, dono, descrição, entrada e saída
- **Clique no nome da atividade** — exibe processo de origem, quem, quando, onde, por quê, como, saída e instrução de trabalho
- Clicar no mesmo item fecha o painel (toggle)
- Clicar em outro item abre o novo diretamente
- Pressionar **Esc** ou clicar fora do painel também fecha

---

## Padrão de cores dos grupos

A cor de fundo de cada grupo na barra lateral indica seu estado:

| Cor | Significado |
|---|---|
| **Branco** (padrão) | Grupo em andamento, sem pendências identificadas |
| **Amarelo** | Existe pelo menos um processo sem nenhuma atividade cadastrada |
| **Verde** | Grupo marcado como completo pelo gestor e sem processos vazios |

### Cor do processo na área principal

Quando um processo não possui nenhuma atividade cadastrada e o grupo está em modo editável, seu cabeçalho recebe fundo amarelo e borda superior laranja — chamando atenção para a pendência.

---

## Operando o sistema

> Os controles de edição abaixo só aparecem para usuários autenticados e ativos.

### Criar um grupo de processos

Clique em **+ Novo Grupo de Processos** no rodapé da barra lateral. Preencha:

- **Número** — código do grupo (ex: `01`). Será ajustado automaticamente ao reordenar
- **Categoria** — Atividade Fim ou Processo de Apoio
- **Nome** — nome do grupo em letras maiúsculas

### Editar ou excluir um grupo

Passe o mouse sobre o grupo na barra lateral para ver os botões de edição (✏️) e exclusão (🗑). A exclusão remove também todos os processos e atividades do grupo — a ação pede confirmação e não pode ser desfeita.

### Reordenar grupos

Arraste o ícone ⠿ à esquerda do nome do grupo para reposicioná-lo entre os demais. O sistema renumera automaticamente todos os grupos e atualiza em cascata os números dos processos filhos.

### Marcar um grupo como completo

Disponível apenas ao **editar** um grupo existente. A opção aparece como um checkbox no modal de edição, com as seguintes condições:

- O checkbox só fica habilitado quando **todos os processos do grupo possuem pelo menos uma atividade**
- Se ainda houver processos vazios, o checkbox aparece desabilitado com uma mensagem explicativa
- Ao marcar como completo, o grupo recebe fundo verde e entra em **modo somente leitura**

### Modo somente leitura (grupo completo)

Quando um grupo está marcado como completo, nenhuma alteração é permitida:

- Não é possível adicionar novos processos ou atividades
- Os botões de edição, exclusão e os handles de reordenação ficam ocultos
- Um badge **✓ Grupo Completo** aparece no cabeçalho da área principal
- O botão **✏️ Editar Grupo** permanece visível — é por ele que o gestor desmarca a conclusão e retorna o grupo ao modo editável

### Criar um processo

Com um grupo selecionado, clique em **+ Novo Processo** no cabeçalho da área principal. Preencha nome, número e, opcionalmente, dono, descrição, entrada e saída do processo.

### Reordenar processos

Arraste o ícone ⠿ à esquerda do nome do processo para reposicioná-lo. Ao soltar, o sistema renumera automaticamente todos os processos do grupo.

### Criar uma atividade

Clique em **+ Atividade** no cabeçalho do processo desejado. O formulário aceita:

- **O Que** *(obrigatório)* — descrição da tarefa
- **Quem** — responsável (lista de pessoas cadastradas)
- **Instrução de Trabalho** — código do documento de referência (ex: `IT-01`)
- **Quando** — frequência ou gatilho
- **Onde** — sistema ou local onde ocorre
- **Por Que** — justificativa
- **Como** — passo a passo
- **Saída** — resultado ou entrega esperada

### Reordenar atividades

Arraste o ícone ⠿ da linha da atividade para reposicioná-la dentro do mesmo processo. A nova ordem é salva automaticamente.

### Recolher e expandir processos

Clique no chevron **›** à direita do cabeçalho do processo para recolhê-lo ou expandi-lo. O botão **⊟ Colapsar tudo / ⊞ Expandir tudo** no cabeçalho da página opera todos de uma vez. O estado de recolhimento é salvo localmente no navegador entre sessões.

---

## Gerenciando pessoas

A aba **Pessoas** na barra lateral exibe todos os colaboradores cadastrados e fica visível apenas para usuários autenticados.

> O gerenciamento de pessoas (cadastrar, editar, excluir) é restrito a **administradores**.

### Campos de cada pessoa

| Campo | Descrição |
|---|---|
| **Nome** | Nome completo |
| **E-mail** | Endereço de e-mail (visível apenas no modal de edição) |
| **Função** | Cargo ou função na organização |
| **Usuário habilitado** | Quando marcado, a pessoa pode fazer login e editar o sistema |
| **Administrador** | Quando marcado, a pessoa também pode gerenciar a tabela de pessoas |

### Ícones de acesso (visíveis apenas quando logado)

Ao lado do nome de cada pessoa, podem aparecer os seguintes ícones:

| Ícone | Significado |
|---|---|
| 🔐 | Administrador do sistema |
| 🔑 | Usuário com acesso ao sistema |
| *(sem ícone)* | Pessoa sem login vinculado |

### Excluir uma pessoa

Ao excluir uma pessoa, ela é removida como responsável de todas as atividades em que estava vinculada. As atividades permanecem, apenas sem responsável atribuído.

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

## Arquivos do repositório

| Arquivo | Descrição |
|---|---|
| `index.html` | Aplicação completa (SPA) |
| `favicon.svg` | Ícone da empresa exibido na aba do navegador |
| `README.md` | Este documento |
| `mapa_processos_davila.md` | Base de conhecimento completa dos processos para uso em agentes de IA |
| `Atributos_de_Processo_BPM_dAvila.pdf` | Análise de atributos típicos de processo — documento para a diretoria |
| `gerar_pdf_atributos.py` | Script Python que gerou o PDF acima |

---

## Tecnologias utilizadas

- **HTML/CSS/JavaScript** puro — sem frameworks ou dependências de build
- **Supabase JS v2** — cliente oficial para autenticação e acesso ao banco
- **Supabase** — banco de dados PostgreSQL em nuvem com RLS e API REST
- **Google Fonts** — tipografia Nunito Sans
- Funciona em qualquer navegador moderno

---

## Histórico de versões

| Versão | Principais alterações |
|---|---|
| REV 02 | Autenticação, RLS, perfis de acesso, aba Pessoas, ícones de usuário, sincronização auth.users ↔ pessoas |
| REV 02.1 | Painel lateral de detalhes (processos e atividades), campos Descrição/Entrada/Saída nos processos, correção do link "+ Adicionar" para visitantes, base de conhecimento para agente IA |

---

*d'Avila Soluções Sustentáveis — Documento interno*
