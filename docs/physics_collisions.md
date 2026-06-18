# Physics Collisions (Eventos de Colisão)

Identificar quando uma bola toca no chão, ou uma bala acerta o jogador é essencial! A Cat Engine usa o poderoso sistema de Handlers (Eventos Manuseados) passados diretamente da linguagem C++ para o seu Lua.

---

## Preparando os "Tipos de Colisão" (CollisionType)

Antes de capturar colisões, a engine precisa saber "Quem é Quem". Crie números inteiros para identificar os grupos. Todo `Shape` pode ter um Tipo Definido:

```lua
local TIPO_JOGADOR = 1
local TIPO_MOEDA = 2

-- Define quem ele é para o Motor Físico:
shape_jogador:setCollisionType(TIPO_JOGADOR)
shape_moeda:setCollisionType(TIPO_MOEDA)
```

---

## Capturando as Colisões (`addCollisionHandler`)

Você "Ensina" o espaço (Space) a avisar você sempre que os Dois Tipos de objetos se esbarrarem!

### `space:addCollisionHandler(typeA, typeB, onBegin, onPreSolve, onPostSolve, onSeparate)`

* **O que faz?** Registra 4 funções (Callbacks) para reagirem aos momentos exatos do contato físico.
* **Parâmetros das Funções que você criar:** As funções recebem `(shapeA, shapeB, contact_points_table)`.

**Exemplo Completo:**

```lua
-- 1. Cria a Função que será executada ao Encostar
local function AoPegarMoeda(shapeA, shapeB, contatos)
    system.log("Player Tocou na Moeda!")
    -- Destruir a moeda? Marcar ela num table?
    -- obs: Nunca destrua shapes/corpos de dentro dessa função, pois a
    -- física C++ no background vai dar CRASH tentando iterar um ponteiro já morto.
    -- Salve-os numa lista `remover = {}` e remova depois do step()!

    -- Retorne true para permitir que a colisão FISICAMENTE ocorra.
    -- Retorne false se for apenas um "Fantasma" e não devem se rebater.
    return false 
end

local function AoSepararMoeda(shapeA, shapeB)
    system.log("Pararam de se encostar.")
end

-- 2. Registra no Motor da Engine para ouvir eternamente:
-- Passamos 'nil' (ou nenhuma função) para fases que não nos interessam!
meu_mundo:addCollisionHandler(
    TIPO_JOGADOR, 
    TIPO_MOEDA, 
    AoPegarMoeda, -- [Begin]: Primeiro frame do encostar
    nil,          -- [PreSolve]: Custa muito processamento!
    nil,          -- [PostSolve]: A colisão acabou de gerar força!
    AoSepararMoeda -- [Separate]: Desgrudaram!
)
```


## Outras Funções do Módulo

### `physics_collision.__gc()` (ou `:__gc()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

