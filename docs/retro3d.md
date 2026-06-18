# Retro3D (Raycasting & Mode 7)

O módulo `retro` fornece um motor Fake 3D incrivelmente rápido acelerado por GPU (OpenGL ES 3.2 Shaders).
Com ele, você pode criar jogos estilo *Doom*, *Wolfenstein*, *Mario Kart* e *F-Zero* na Cat Engine, desenhando através do Raycaster (para paredes e labirintos) ou Mode 7 (para chão e teto).

## Como importar

O módulo já está nativamente embutido.

```lua
local mapa = {
    1,1,1,1,1,
    1,0,0,0,1,
    1,0,2,0,1,
    1,0,0,0,1,
    1,1,1,1,1
}

function load()
    texTijolo = graphics.loadTexture("tijolo.png")
    texPedra = graphics.loadTexture("pedra.png")
    texChao = graphics.loadTexture("chao.png")

    retro.setMap(5, 5, mapa)
    retro.setWalls({texTijolo, texPedra})
    retro.setFloor(texChao)
end

function draw()
    -- X, Y, Ângulo de Rotação, Pitch (Olhar pra cima/baixo), Campo de Visão (Opcional, Padrão 0.66)
    retro.drawRaycast(2.5, 2.5, math.pi / 2, 0, 0.66)
end
```

## Funções Disponíveis

### `retro.setMap(largura, altura, tabela)`
Define a malha (mapa 2D) de blocos do nível. O parâmetro `tabela` deve ser um array 1D contendo números inteiros de 0 a 8.
- **0:** Espaço vazio.
- **1 a 8:** Parede texturizada (índice da textura fornecida em `setWalls` começando do 1).

### `retro.setWalls(texturas)`
Define as texturas usadas para as paredes (até 8 texturas máximas). O argumento `texturas` deve ser uma listagem de objetos `Texture`.

### `retro.setFloor(textura)`
Define a textura usada para o chão em Raycast e Mode 7. O argumento `textura` deve ser um objeto `Texture`.

### `retro.setCeiling(textura)`
Define a textura usada para o teto. O argumento `textura` deve ser um objeto `Texture`.

### `retro.setSkybox(textura)`
Define um mapa circular ao fundo das paredes/teto transparente.

### `retro.setFog(r, g, b, min, max)`
Habilita uma névoa ambiente de cor RGB.
`min` (Padrão 1.0) Até qual proximidade a névoa ignora esconder tudo.
`max` (Padrão 12.0) Distância máxima para os objetos estarem 100% tomados pela névoa.

### `retro.setFloorColor(r, g, b)`
Define a cor sólida do chão se você não definir nenhuma Textura para ele.

### `retro.setCeilingColor(r, g, b)`
Define a cor sólida do teto caso você decida fazer o teto liso sem textura.

### `retro.setAmbientLight(r, g, b)`
Define a cor e a intensidade da iluminação global ambiente. Valores RGB vão de 0.0 a 1.0 (onde 1, 1, 1 é um local claro e sem sombras, e 0.2, 0.2, 0.3 é uma noite escura e sombria). Perfeito para combinar com a Névoa (`setFog`) e dar profundidade e atmosfera assustadora pro seu labirinto, precisando ser iluminada por luzes extras!

### `retro.addPointLight(x, y, r, g, b, [intensity], [radius])`
Adiciona uma lâmpada luminosa (Point Light) nas posições de mapa do mundo (`x` e `y`) com a cor emissiva RGB (`r, g, b`).
Você opcionalmente ainda pode definir sua intensidade (O Padrão é `1.0`), ou o raio de propagação físico sobre esse espaço de distância (O padrão é chegar até uma distância de `5.0`). Você pode colocar até **16** Point Lights em cena simulâneas. Ela iluminará as texturas em sua volta nas quais a luz encosta e perderá sua força baseada no seu Raio e Distância dando um visual mágico ao 3D!

### `retro.clearLights()`
Simplesmente desliga e destrói todas as luzes que você adicionou num passe de mágica! Perfeito para atualizar as luzes dinâmicas como se fosse um abajur que pisca sem dor de cabeça, ou ao mudar de cenário.

### `retro.project(camX, camY, camZ, angulo, pitch, fov, objX, objY, objZ, maxScreenW, maxScreenH)`
Projeta uma Coordenada 3D do Mundo para uma TELA 2D!
Você pode usar a Coordenada `x, y, scale, dist` retornadas por esta função para desenhar Sprites convencionais (com `graphics.draw` ou `FastBatch`) posicionando eles no mundo e os escalando de acordo com a distância! Se `dist < 0`, o objeto está atrás da câmera e não deve ser desenhado.

### `retro.setBlock(x, y, id)`
Atualiza dinamicamente um bloco do mapa (perfeito para abrir portas, destruir paredes ou criar mecânicas tipo "bomba"/"minecraft").

### `retro.getBlock(x, y)`
Retorna o código (`0` a `8`) do bloco no mapa nesta posição exata (perfeito para implementar sistema de colisão e física!).

### `retro.raycast(originX, originY, dirX, dirY, [maxDist])`
Dispara um raio de precisão matemática e simula fisicamente o mesmo percurso que o renderizador enxerga. Perfeito para "hitscan" (Dar tiros no inimigo) ou então checar "linha de visão" (Saber se o inimigo tem você no campo de visão e pode atirar, ou se há uma parede no meio!).
*Retorna 6 valores*: `bateu (bool), distancia, hitX, hitY, blocoID (qte atingida), ladoAtingido (0=X, 1=Y)`.
- Se `bateu` for falso, significa que não atingiu nenhum bloco sólido dentro do `maxDist`.

### `retro.movePlayer(x, y, radius, dx, dy)`
Resolve o problema de colisão de forma polida e precisa! Em vez de bugar nas paredes, o seu jogador vai deslizar ("Slide") suavemente nos cantos.
- **x, y**: A posição atual do jogador na malha.
- **radius**: O "raio" gordinho (tamanho físico) do corpo do jogador (ex: 0.3).
- **dx, dy**: Para onde ele quer andar (Ex: joystick).
- **Retorna**: O novo `X` e `Y` confirmados pós-colisão.
Exemplo de uso: `px, py = retro.movePlayer(px, py, 0.3, jx * speed, jy * speed)`

### `retro.drawRaycast(camX, camY, camZ, angulo, [pitch, fov])`
Desenha a cena usando o motor Raycaster. Ideal para jogos de tiro e labirintos.
- **camX, camY:** Posição da câmera no mundo na malha horizontal (`X` e `Y`).
- **camZ:** A altura da Câmera (ex: `0.5` mantém os olhos na metade da altura da parede, e usar `1.0` levanta a altura até o topo da parede, servindo para agachar e pular).
- **angulo:** Rotação radiana (onde `math.pi/2` olha para o norte).
- **pitch:** Afeção do ângulo vertical de olhar (ex: -0.2 para olhar um pouco pra cima. Padrão: 0).
- **fov:** Amplitude visual (Padrão: 0.66).

### `retro.drawMode7(camX, camY, camZ, angulo, [pitch, fov])`
Desenha a cena sem paredes usando o Mode 7. Ele projeta unicamente a textura de Chão (`setFloor`) e o Teto (`setCeiling`) com rotação visual livre e névoa direcional inteligente, perfeito para Mário Kart e jogos de pilotagem!
- **camZ:** A altura tridimensional da Câmera (ex: 0.5 mantem o jogador cravado no meio do chão/teto).

## 💡 Dicas de Ouro: Criando o Jogo 3D Perfeito

Agora que você conheceu as funções, veja como elas atuam em conjunto para criar qualquer gênero dentro do motor 3D:

### 1. Sistema FPS Físico e Tiros (Hitscan)
Com as funções `retro.getBlock` e `retro.raycast`, nós saímos dos gráficos limitados e damos a base matemática para mecânicas profissionais:
- **Movimento Sólido:** Antes de andar, você checa `retro.getBlock(novoX, novoY)` para garantir que o seu jogador vai colidir de forma natural com as paredes.
- **Tiros, Armas e Lasers (Hitscan):** Disparar a função invisível `retro.raycast` simula fisicamente um feixe na mesma velocidade da bala. Ele te responde na mesma hora se você acertou uma parede ou se acertaria a distância em que um monstro está da sua tela!
- **Inteligência Artificial (Linha de Visão):** Monstros podem usar isso para checar "Tem alguma parede entre mim e o jogador?". Se o feixe acertar o vazio antes de chegar em você, ele atira! Senão, ele recua e muda de rota pelo labirinto.

### 2. Interação e Puzzles (Estilo Minecraft ou Chaves)
A função `retro.setBlock(x, y, id)` atualiza de forma assíncrona o mapeamento da textura na memória C++, isso permite:
- **Portas e Chaves Secretas:** Se o jogador tiver uma chave na frente de uma porta (Bloco de ID `2`), rodar o comando `setBlock(portaX, portaY, 0)` vai destruir a porta permanentemente e desenhar o caminho aberto no mundo.
- **Labirintos Dinâmicos:** Mudar paredes de lugar durante o jogo sem interrupções.

### 3. Sistema de Entidades, Sprites e Atores 2.5D
Como não estamos limitando o desenvolvedor a objetos 3D puros trancados no motor e exigindo programação complexa de shader, a incrivel função `retro.project` entrega os atores para a poderosa e simples Renderização 2D nas suas mãos:
- **Inimigos, Carrinhos Rivais e Moedas:** Você simplesmente passa a posição mundo em que eles estão e ela retornará não somente um eixo de posição X/Y mas a **Escala** respectiva da distância, e você desenha a partir disso com `graphics.draw` (podendo até desenhar barras de vida seguindo a altura na cabeça dos inimigos!).

### 5. O que NÃO fazer (Práticas Ruins a Evitar) 🚫
- **Fugir da Canvas Padrão**: Renderizar `drawRaycast` e `drawMode7` em altíssima resolução nativa Full HD vai perder completamento o charme "Retrô Pixel Arte" e vai fazer a placa de vídeo desenhar centenas de milhares de linhas e calcular luzes desnecessariamente. Prefira sempre renderizar em uma Canvas pequena (ex: 320x240) e esticá-la para a tela!
- **Girar as Sprites (Inimigos) no Eixo Errado**: Ao projetar Sprites na tela (`retro.project`), desenhe-as normalmente como imagens. Não passe rotações estranhas no `graphics.draw`. Se quiser que o "papelão do inimigo pareça torto ou deitado", deixe assim, mas lembre-se que Billboarding perfeito em jogos anos 90 é sempre deixar o objeto alinhado com a tela, apenas mudando o seu Tamanho de acordo com a distância (ajustando a Escala `escX` e `escY`).
- **Limites Físicos e Hitscan Gigantes**: Não chame `retro.raycast` e passe `maxDist` muito exorbitantes (como milhares e milhares de blocos) a todo minuto para múltiplos inimigos caso não seja necessário, limite a visão de `maxDist` para que seja em coerência com o tamanho renderizado do mapa!

Para conferir o código funcional e rodável usando Mode 7 e Raycaster completo com Movimentação, mire em **`tutorial.md`**!
