# ECS (Entity / Component / System)

O módulo `ECS` na Engine fornece um sistema flexível onde herança não é a base vital, e ao invés disso as Identidades (Entidades Universais vazias) contêm partes (Componentes / Dados e Variáveis em estado), as quais são puramente modificadas iterativamente iterando sob seu processador iterador natural (Sistema).

---

## Gerenciando Identidades Simples:

### `ecs.createEntity()`
* **O que isso faz?** Retorna um número único Inteiro (`id`) reservando um espaço que a compõe na engine de Componentes anexáveis.
* **Exemplo:**
  ```lua
  local my_enemy_bot = ecs.createEntity()
  ```

### `ecs.destroyEntity(id)`
* **O que isso faz?** Aniquila sem piedade todos os dados acoplados em cascata da memória e liberta IDs para economizar RAM de hardware durante a execução do motor.
* **Exemplo:** `ecs.destroyEntity(my_enemy_bot)`

### `ecs.disableEntity(id)` / `ecs.enableEntity(id)` / `ecs.isActive(id)`
* **O que isso faz?** Alternativa viável e eficiente a Destruição e Re-criação inata contínua (Poolings). Desativando ele e componentes todos se inativam do sistema visual do Motor sumindo, mas ainda vivendo inalterados ocultamente aguardando repousos reacionários sendo ligados ativamente no Enable! `isActive` verifica o status atual da entidade no ciclo de vida. 

## Acoplando e Manipulando Dados:

### `ecs.addComponent(id, nome_tabela, tabela_lua)`
* **O que isso faz?** Anexa informações passadas livres como referências que poderão ditar ações nas entrelinhas para as identidades e componentes de estados.
* **Parâmetros:**
  * `id` (int): Retornado via `createEntity()`.
  * `nome_tabela` (string): Identificador fácil e humano deste componente em Tabela.
  * `tabela_lua` (table) A lista e pares que dita comportamentos do escopo passado aos motores físicos e lógicas do sistema do player.
* **Exemplo de uso completo no código do Player:**
  ```lua
  local hero_id = ecs.createEntity()
  -- Criando um Estado Simples de Personagem da mente da Pessoa:
  ecs.addComponent(hero_id, "Transform", {x = 150, y = 150})
  ecs.addComponent(hero_id, "Health", {hp = 100, maxHp = 100})
  ```

### `ecs.getComponent(id, nome)`
* **O que isso faz?** Uma forma direta de achar e editar, processar a base exata em tempo de atualização sem rebuscar dados espalhados nulos para efetuar ações diretas durante execuções longínquas dentro da própria estrutura sem bagunças e lixos residuais globais pelo app.
* **Retorno:** A tabela vinculada que representa estados exatos do componente alvo pedido perfeitamente sincronizada em forma exata local e global contendo os índices primitivos intactos para atualização local de tempo real, caso falhe `nil`.
* **Exemplo:**
  ```lua
  local posicao = ecs.getComponent(hero_id, "Transform")
  if posicao ~= nil then
      posicao.x = posicao.x + 10 -- Andou 10 pixels
  end
  ```


## Outras Funções do Módulo

### `ecs.__index()` (ou `:__index()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `ecs.destroy()` (ou `:destroy()`)
**Assinatura:**
```lua
video:destroy()
```

Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `ecs.disable()` (ou `:disable()`)
**Assinatura:**
```lua
entity:disable()
```

Muda states no Contexto OpenGL para forçar desligamento GL enum (Ex. depth, cut, stencil).

### `ecs.enable()` (ou `:enable()`)
**Assinatura:**
```lua
entity:enable()
```

Modifica o Contexto nativo state da Render Pipeline para ligar Capabilities de buffers ex GL_BLEND, GL_DEPTH_TEST e gl macro.

