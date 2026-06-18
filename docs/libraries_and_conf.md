# Bibliotecas da Cat Engine e Configurações

Esta seção da documentação explica como o sistema de **Bibliotecas Padrão** funciona na Cat Engine, detalhando a biblioteca visual embutida (`ui.lua`) e o poderoso arquivo de configurações globais `conf.lua`.

---

## O Arquivo `conf.lua` (Configurações da Engine)

Quando você inicia sua aplicação pela primeira vez, a Cat Engine cria e configura um projeto base no armazenamento local do dispositivo. Uma parte crucial dessa infraestrutura é a criação da pasta `catconfig` contendo o arquivo `conf.lua`.

### Para que serve o `conf.lua`?

O arquivo `conf.lua` age como a porta de entrada para configurações profundas e vitais da engine antes que qualquer ambiente gráfico seja de fato levantado.

Exemplo padrão de um `conf.lua` recém-criado:

```lua
-- catgame/catconfig/conf.lua
-- Configurações gerais da engine

-- Define se a engine deve criar/recriar a pasta catlibs e seus arquivos (como ui.lua)
recreate_catlibs = true

-- Define se a engine deve recriar o arquivo ui.lua caso seja apagado
recreate_ui_lua = true

-- Nível de anti-aliasing ou suavização de pixels (0 = desativado, 2, 4, 8)
-- Desative se o jogo aparenta ser pixel ou não possui muitos detalhes curvos.
msaa = 4

-- Orientação da tela do jogo.
-- Mude esta opção para travar a tela na posição desejada.
-- "sensor"    -> A tela gira livremente conforme você vira o celular.
-- "landscape" -> A tela fica deitada (paisagem), ótimo para jogos de plataforma 2D.
-- "portrait"  -> A tela fica em pé (retrato), ideal para jogos como Flappy Bird.
orientation = "sensor"
```

### Explicação das Flags do Sistema:

*   **`recreate_catlibs`**: Gerencia se as pastas cruciais do sistema de componentes podem ser restauradas.
*   **`recreate_ui_lua`**: É uma camada de proteção. Se você acidentalmente deletar a sua `catlibs/ui.lua`, a engine gera uma nova na próxima inicialização, garantindo que projetos que dependam de interface nunca quebrem abruptamente de forma irremediável.
*   **`msaa`**: Multisampling Anti-Aliasing limpa "serrilhados" de objetos na tela. Alto custa performance mas melhora qualidade. Para jogos totalmente em *Pixel Art*, é recomendado mudar o `msaa` para `0` (desativado).
*   **`orientation`**: Define o sistema direcional de todo seu App. Se definido como `portrait` ou `landscape`, o aplicativo forçará e permanecerá unicamente na rotação estipulada em nível de sistema Android.

---

## Criando e Importando Bibliotecas

Criar sua própria biblioteca na Cat Engine permite separar melhor as responsabilidades e distribuir código. O processo de separação é natural a Lua.

1.  Crie um arquivo `.lua` (ex: `meus_calculos.lua`) na sua estrutura de arquivos dentro de `catgame/` (ex: `catgame/catlibs/meus_calculos.lua`).
2.  Construa sua API de Retorno.

**meus_calculos.lua:**
```lua
local calc = {}

function calc.somar(a, b)
    return a + b
end

function calc.subtrair(a, b)
    return a - b
end

-- Retornamos a tabela para quem fez o require
return calc
```

3. No seu `main.lua` ou de outro script:

```lua
local meus_calculos = require("catlibs.meus_calculos")

function update(dt)
   local soma = meus_calculos.somar(10, 5)
   system.log("Total: " .. soma)
end
```

---

## A Biblioteca Padrão `ui.lua`

A Cat Engine já envia embutida com o seu primeiro levantamento de projeto a poderosa **`ui.lua`**, localizada em `catgame/catlibs/ui.lua`. 
A finalidade desta biblioteca oficial é permitir criar interfaces (botões, painéis, visualizadores de scroll, checkbox, barras de progresso, etc) sem dor de cabeça e de forma otimizada com poucas linhas de código.

### Como usar a UI

Primeiro faça o import em seu jogo:
```lua
local ui = require("catlibs.ui")
```

A `ui.lua` é um paradigma desenhado para reter estado e se desenhar com uma única chamada em seu loop principal.

**Exemplo Completo:**
```lua
local ui = require("catlibs.ui")

function load()
    -- Configurando a fonte global para os elementos UI
    local font = graphics.loadFont("minha_fonte.ttf", 24)
    if font then
        ui.setFont(font)
    end

    -- Cria um painel base no canto (10,10) com dimensão (300,400)
    ui.newPanel("meu_painel", 10, 10, 300, 400)
    
    -- Cria um botão e especifica uma Callback para o evento de Clique
    ui.newButton("btn_iniciar", 30, 30, 200, 60, "Iniciar Jogo", function()
        system.log("O botão INICIAR foi pressionado!")
    end)
    
    -- Cria Checkbox
    ui.newCheckbox("som_ativo", 30, 120, 40, true, function(checked)
        system.log("Estado do Som: " .. tostring(checked))
    end)
end

function update(dt)
    -- Verifica as interações/toques nos elementos para acionar as Callbacks de clique e scroll
    ui.update(dt)
end

function draw()
    -- Renderiza todos osc elementos na tela de acordo com sua ordem/estado!
    ui.draw()
end
```

### Estrutura Interna da interface

Os principais controles disponíveis na `ui.lua`:
- `ui.newPanel(id, x, y, w, h)`: Um quadrado gráfico container de visualização.
- `ui.newButton(id, x, y, w, h, text, callback)`: Botão reativo ao toque que aceita funções anônimas.
- `ui.newCheckbox(id, x, y, size, checked, callback)`: Marcador do tipo boleano ativo/inativo.
- `ui.newScrollView(id, x, y, w, h)` / `ui.beginScroll(id)` / `ui.endScroll()`: Área mascarada para itens em deslizamento (Scissor Test na placa). Use-as envolvendo botões renderizados numa visualização limitada.
- `ui.newProgressBar(id, x, y, w, h, max_value)`: Sistema barra interativa ou informacional. Use `bar.value` para manipulá-la.

Todos os elementos retornam uma estrutura `Tabela` em Lua. Logo, você pode alterá-los dinamicamente (visibilidade, cor, textura, etc) resgatando através do comando:

```lua
local btn = ui.get("btn_iniciar") 
btn.visible = false -- Oculta o objeto e impede interação temporalmente

-- É possível mudar cores para RGBA ({r=0, g=1, b=0, a=1})
btn.color = {r=1, g=0, b=0, a=1}
```

Para customização aprofundada, leia diretamente o código que compõe o módulo local `catgame/catlibs/ui.lua`, ele foi feito sob medida para que desenvolvedores modifiquem à vontade!
