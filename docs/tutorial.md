# Tutoriais e Dicas Práticas

Neste documento reunimos dúvidas comuns da comunidade e as maneiras mais fáceis de resolver problemas cotidianos na framework Cat Engine, como rotacionar imagens e criar animações simples ou baseadas em spritesheets.

---

## 1. Como Rotacionar uma Sprite (Imagem)?

Muitos usuários se deparam com a necessidade de rotacionar uma imagem (como para apontar uma nave, ou uma arma). A função `graphics.draw` já possui esse suporte embutido.

```lua
local img_nave = graphics.loadTexture("assets/nave.png")
local angulo = 0

function update(dt)
    -- Gira um pouquinho a cada quadro de acordo com a velocidade do jogo (delta time)
    angulo = angulo + (1.5 * dt)
end

function draw()
    -- Uso: graphics.draw(img, x, y, angulo_radianos, escalaX, escalaY)
    graphics.draw(img_nave, 200, 200, angulo, 1, 1)
end
```

---

## 2. Como Fazer uma Animação Simples (com Imagens Separadas)

A maneira mais manual (porém funcional) de fazer uma animação com imagens separadas é guardar as imagens base em uma `Tabela` (`{}`) Lua e criar um temporizador para iterar sobre elas. Veja como é fácil:

```lua
-- Guardamos os frames separados da nossa animação em uma tabela Lua
local frames = {
    graphics.loadTexture("player_run_1.png"),
    graphics.loadTexture("player_run_2.png"),
    graphics.loadTexture("player_run_3.png")
}

local frame_atual = 1
local tempo_animacao = 0
local velocidade = 0.1 -- Tempo (em segundos) para trocar de imagem (0.1 = Rápido)

function update(dt)
    tempo_animacao = tempo_animacao + dt
    
    if tempo_animacao >= velocidade then
        tempo_animacao = 0 -- Zera nosso relógio
        frame_atual = frame_atual + 1 -- Passa pro próximo frame
        
        -- Se o quadro atual passar da quantidade limite, volta pro 1 (Looping)
        if frame_atual > #frames then
            frame_atual = 1
        end
    end
end

function draw()
    -- Desenha a imagem atual baseada no índice da tabela
    graphics.draw(frames[frame_atual], 100, 100, 0, 1, 1)
end
```

---

## 3. Animação com Spritesheet (O Jeito Mais Fácil e Correto)

Usar um **Spritesheet** (uma única imagem com todos os frames da animação agrupados, lado a lado) é a melhor escolha para performance superior. Desta forma a Placa de Vídeo trabalhará bem menos!

Com o método `:drawQuad` embutido na Imagem na Cat Engine, você não precisa ficar configurando e instanciando quads na memória para animar. O uso se dá por simplesmente desenhar através de  uma "janela de recorte" baseada em Matemática, indicando apenas onde começamos o recorte `X` e `Y` e qual será a altura e largura dele.

**Exemplo Prático**: Você tem um spritesheet de uma corrida com `400` de largura total e `100` de altura, contendo 4 frames de personagem deitados em linha reta ao longo da imagem, logo cada frame possui exatos `100` píxeis de largura (`100x100`).

```lua
local sheet = graphics.loadTexture("player_spritesheet.png")

local frame = 0
local timer = 0
local velocidade = 0.1 -- Duração de cada frame (1 décimo de segundo)

-- Qual o tamanho de apenas UM frame do personagem que será recortado?
local largura_frame = 100
local altura_frame = 100

-- Quantos frames a imagem possui dentro dessa linha reta?
local total_frames = 4

function update(dt)
    timer = timer + dt
    if timer >= velocidade then
        timer = 0
        frame = frame + 1
        
        -- Zera o frame quando tiver atingido o limite para repetir a animação!
        if frame >= total_frames then
            frame = 0 
        end
    end
end

function draw()
    -- O Segredo: Apenas mudamos o cursor de recorte 'X' atual!
    local recorteX = frame * largura_frame
    local recorteY = 0 -- Zero, pois nossa animação está toda em uma única linha reta Superior X.
    
    -- Finalmente, recortamos e desenhamos rapidamente isso por Frame
    -- texture:drawQuad(telaX, telaY, inicioRecorteX, inicioRecorteY, larRecorte, altRecorte)
    sheet:drawQuad(200, 200, recorteX, recorteY, largura_frame, altura_frame)
end
```

## Dúvidas Extras

### Posso aplicar Escala nessas animações de Quad (Spritesheet)?
Sim! O `drawQuad` da Engine tem parâmetros opcionais:
`texture:drawQuad(telaX, telaY, srcX, srcY, srcW, srcH, dstW, dstH)`

Os últimos parâmetros (`dstW` e `dstH`) controlam o tamanho que esse quadro será renderizado na tela. Se você quer exibir aquele nosso frame de escala "100x100" ampliado 2 vezes o tamanho dele. Bastaria ditar que as margens finais representariam "200" em proporção: `sheet:drawQuad(200, 200, rectX, rectY, 100, 100, 200, 200)`

> **Quer a biblioteca de animação definitiva?** A Cat Engine aceita muito código Lua clássico! Há bibliotecas que tornam as animações muito rápidas de se programar. Em projetos mais robustos, recomendamos adaptar bibliotecas famosas de animação como o framework clássico para lua **Anim8**.

---

## 4. Criando Jogos Retro 3D (Raycaster & Mode 7)

O módulo nativo `retro` transforma o plano 2D da Cat Engine em um motor Falso-3D na placa de vídeo, rodando na velocidade da luz. Ideal para jogos estilo Doom (Raycast) e Mario Kart (Mode 7). 

### Exemplo Base: Andando em um Labirinto (Raycaster)
Neste exemplo, vamos carregar as texturas, desenhar sobre um `Canvas` de 320x240 (para dar aquela aura 'Pixel Art' de PS1/SNES e economizar muita GPU) e navegar com um sistema de colisão básica usando as novas funções matemáticas!

```lua
local canvas = graphics.newCanvas(320, 240)
local texTijolo

-- Câmera e Jogador
local camX, camY = 2.5, 2.5
local camZ = 0.5
local angulo = 0

-- Nosso pequeno Mapa 5x5
local map_width, map_height = 5, 5
local mapa = {
    1, 1, 1, 1, 1,
    1, 0, 0, 0, 1,
    1, 0, 0, 0, 1,
    1, 0, 0, 0, 1,
    1, 1, 1, 1, 1
}

function load()
    texTijolo = graphics.loadTexture("assets/tijolo.png")
    
    retro.setMap(map_width, map_height, mapa)
    retro.setWalls({texTijolo})
end

function update(dt)
    -- Rotação
    if input.isKeyDown("left") then angulo = angulo - 2 * dt end
    if input.isKeyDown("right") then angulo = angulo + 2 * dt end

    -- Movimentação Sólida (Com checagem de Colisão Avançada)
    local moveVel = 3 * dt
    if input.isKeyDown("up") then
        local novoX = camX + math.cos(angulo) * moveVel
        local novoY = camY + math.sin(angulo) * moveVel
        
        -- Checa se o chão está livre (0 = nada, >0 = parede) usando retro.getBlock
        if retro.getBlock(math.floor(novoX), math.floor(camY)) == 0 then camX = novoX end
        if retro.getBlock(math.floor(camX), math.floor(novoY)) == 0 then camY = novoY end
    end
end

function draw()
    -- Passo 1: Liga a TV antiga (Canvas Baixa Resolução)
    graphics.setCanvas(canvas)
    graphics.clear(0.2, 0.2, 0.2) -- Cor simples do chão e teto
    
    -- Passo 2: Renderiza o mundo mágico!
    -- camX, camY, camZ(padrão da câmera humana de jogo fps = 0.5), ângulo.
    retro.drawRaycast(camX, camY, 0.5, angulo)
    
    -- Passo 3: Volta o monitor pra vida real HD e estica a "TV antiga" pra caber na Tela inteira.
    graphics.setCanvas()
    graphics.draw(canvas, 0, 0, 0, graphics.getWidth()/320, graphics.getHeight()/240)
end
```

### Exemplo: Personagens "Vivos" olhando para você (Billboarding)
Um truque fundamental da era 2.5D, era fazer os personagens (Moedas, Inimigos) acompanharem a câmera 3D sem precisar modelar em 3D. Fazemos isso usando o `retro.project`.

```lua
local inimigo_img = graphics.loadTexture("assets/monstro.png")
local monstroX, monstroY = 3.5, 3.5 -- Posição dele solto no mapa

function draw()
    graphics.setCanvas(canvas)
    
    -- Desenha a fase primeiro (Exemplo Mode 7 - Corrida Livre, Sem paredes)
    retro.drawMode7(camX, camY, 0.5, angulo)

    -- Projeta onde os "olhos 3D do mundo" imaginam o monstro
    local telaX, telaY, objEscala, objDist = retro.project(
        camX, camY, 0.5, angulo, 0, 0.66, 
        monstroX, monstroY, 0.5, 
        320, 240
    )

    -- Só gasta processamento pra desenhar se ele estiver NA NOSSA FRENTE e VIVO (Nao ta atras da câmera! > 0)
    if objDist > 0.1 then 
        -- Ele cresce perto, e diminui longe! E acompanha sua rotação perfeitamente!
        local fatorCrescimento = 1 / objDist
        graphics.draw(inimigo_img, telaX, telaY, 0, fatorCrescimento, fatorCrescimento)
    end
    
    graphics.setCanvas()
    -- Esticaremos ele como no exemplo anterior (...)
end
```

