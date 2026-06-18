# Graphics (Gráficos)

O módulo `Graphics` é responsável por tudo que é desenhado na tela, desde imagens e texturas simples até geometrias e renderização avançada.

---

## Funções Disponíveis

### `graphics.loadTexture(path)`

* **O que isso faz?** Carrega uma imagem a partir do armazenamento (PNG, JPG).
* **Parâmetros:**
  * `path` (string): O caminho para o arquivo de imagem.
* **Retorno:**
  * `Texture` - O objeto de textura que pode ser desenhado na tela.
* **Exemplo:**
  ```lua
  local myTexture = graphics.loadTexture("assets/hero.png")
  ```
* **O que acontece se eu usar errado?** Se o arquivo não existir ou o caminho estiver incorreto, a engine não criará a textura, o retorno será nulo, que acarretará um erro ao tentar desenhar.

---

### `texture:getWidth()` / `texture:getHeight()`
* Retorna os tamanhos originais da imagem (largura e altura).
* **Exemplo:** `local w = myTexture:getWidth()`

### `texture:setFilter(min, mag)` e `texture:setWrap(u, v)`
* O Filtro Padrão geralmente deixa as imagens Anti-Serrilhadas (Nearest, Linear). Mas para Jogos em Pixel Art 2D puros você deve passar `0` (Nearest) para desativar o esfarelamento HD. O Wrap te permite ditar se a textura deve se clonar infinitamente (`1`) se for esticada via Shader!

---

### `graphics.draw(texture, x, y, rotation, scaleX, scaleY)`

* **O que isso faz?** Desenha uma imagem previamente carregada na posição e dimensões desejadas.
* **Parâmetros:**
  * `image` (Image): O objeto de imagem gerado pelo `graphics.loadTexture()`.
  * `x` (number): A posição horizontal na tela.
  * `y` (number): A posição vertical na tela.
  * `rotation` (number) [Opcional]: Rotação em radianos. Padrão = 0.
  * `scaleX` (number) [Opcional]: Escala horizontal. Padrão = 1.
  * `scaleY` (number) [Opcional]: Escala vertical. Padrão = 1.
* **Retorno:**
  * `Nenhum`
* **Exemplo:**
  ```lua
  -- Desenha o herói no centro (x=200, y=200) com tamanho real
  graphics.draw(myTexture, 200, 200, 0, 1, 1)
  ```
* **O que acontece se eu usar errado?** Passar uma imagem nula causará um erro fatal. Omitir argumentos obrigatórios como `x` ou `y` poderá resultar na aplicação fechar por conta do interpretador Lua falhar em localizar estes parâmetros numéricos.

---

### `graphics.newQuad(x, y, w, h, sw, sh)` e `texture:drawQuad(x, y, srcX, srcY, srcW, srcH, dstW, dstH)`
* **O que faz?** Corta dinamicamente partes de sprites e desenha na tela! Modos como `drawQuad` não usam objeto da memóra do novo Quad, mas pedem diretamente as coordenadas literais.
* `srcX, srcY, srcW, srcH`: A região da imagem fonte que você quer recortar.
* `dstW, dstH`: A escala de destino daquele corte na tela. Opcional, vai copiar o src.

### `graphics.draw9Slice(texture, x, y, w, h, left, right, top, bottom)`
* **O que faz?** Desenha interfaces, janelas, botões que esticam, porem preservando as bordas físicas sem deformar o arredondamento!

---

## Renderização Avançada e Textos

### `graphics.loadFont(path, size)` e `graphics.print(font, txt, x, y, r, g, b, a)`
* **O que faz?** Cria e escreve textos usando fontes OTF/TTF personalizadas em tempo real.
* **Exemplo:**
  ```lua
  local myFont = graphics.loadFont("assets/Roboto.ttf", 32)
  graphics.print(myFont, "Olá Mundo!", 100, 100, 1.0, 1.0, 1.0, 1.0)
  ```
* **Filtros e Sombras:** Possuímos ainda `graphics.printOutline` e `graphics.printShadow` para criar letras com contornos e fundos dinâmicos!
* **Medição:** `graphics.measureText(font, "text")` retorna Array com `w` (largura) e `h` (altura) que o texto tomaria antes mesmo de desenhá-lo!

---

### `graphics.newCanvas(w, h)` / `graphics.setCanvas(canvas)`
* Cria uma tela fantasma em memória VRAM e te permite desenhar coisas secretamente nela durante o frame, para depois poder desenhar a tela final inteira girando, ou aplicando Pós Processamentos pesados. Lembre-se, use `setCanvas(nil)` para voltar à tela normal.

---

### `graphics.push()` e `graphics.pop()`
* Salva e Restaura o Estado Global (Bordas, Câmera atual, Rotações Globais GL).

---

### `graphics.drawRect(x, y, w, h, r, g, b, [a])`

* **O que isso faz?** Desenha um retângulo simples (Forma Geométrica primitiva).
* **Parâmetros:**
  * `x`, `y` (number): Coordenadas da forma.
  * `w`, `h` (number): Largura e altura.
  * `r`, `g`, `b`, `a` (number): Cores numéricas de 0.0 a 1.0. (Opacidade `a` é 1.0 por padrão).
* **Retorno:** `Nenhum`
* **Exemplo:**
  ```lua
  graphics.drawRect(50, 50, 100, 200, 1.0, 1.0, 1.0, 1.0)
  ```

---

### `graphics.drawCircle(x, y, radius, r, g, b, [a])`

* **O que isso faz?** Desenha um círculo preenchido.
* **Parâmetros:**
  * `x`, `y` (number): Ponto central do círculo.
  * `radius` (number): Distância entre o centro e a borda.
  * `r`, `g`, `b`, `a` (number): Cores de 0.0 a 1.0.
* **Exemplo:**
  ```lua
  graphics.drawCircle(100, 100, 50, 0.0, 1.0, 0.0, 1.0)
  ```

---

### `graphics.clear(r, g, b, [a])`

* **O que isso faz?** Limpa a tela inteira preenchendo-a com uma cor base. Essencial para redesenhar quadros. Normalmente chamado logo no início da função `draw()`.
* **Parâmetros:**
  * `r`, `g`, `b`, `a` (number): Valores de cor variando em decimal entre 0.0 a 1.0.
* **Exemplo:**
  ```lua
  graphics.clear(0.0, 0.0, 0.0, 1.0) -- Fundo preto
  ```

---

## Conceitos Relacionados

**Delta Time** e **Renderização Base:**  
Desenhos a cada quadro necessitam limpar a tela usando `graphics.clear` e redesenhar os objetos. Para obter os melhores resultados, evite instanciar tabelas e imagens a cada quadro.


## Outras Funções do Módulo

### `camera.setPosition()` (ou `:setPosition()`)
**Assinatura:**
```lua
sound:setPosition(seconds)
```

Empurra/Configura o posicionamento físico principal da entidade.

### `camera.getPosition()` (ou `:getPosition()`)
**Assinatura:**
```lua
sound:getPosition()
```

Recebe a posição no tempo, referencial ou localização em instâncias.

### `camera.setZoom()` (ou `:setZoom()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.getZoom()` (ou `:getZoom()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.setRotation()` (ou `:setRotation()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.getRotation()` (ou `:getRotation()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.setBounds()` (ou `:setBounds()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.clearBounds()` (ou `:clearBounds()`)
Zera todos pixels rastreados para cor pré definida no buffer no OpenGL com chamada de enum glClear.

### `camera.setDeadzone()` (ou `:setDeadzone()`)
**Assinatura:**
```lua
joy:setDeadzone(val)
```

Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.clearDeadzone()` (ou `:clearDeadzone()`)
Zera todos pixels rastreados para cor pré definida no buffer no OpenGL com chamada de enum glClear.

### `camera.setFollowLerp()` (ou `:setFollowLerp()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.follow()` (ou `:follow()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.zoomAt()` (ou `:zoomAt()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.shake()` (ou `:shake()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.attach()` (ou `:attach()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.detach()` (ou `:detach()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.screenToWorld()` (ou `:screenToWorld()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.isPointVisible()` (ou `:isPointVisible()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `camera.isRectVisible()` (ou `:isRectVisible()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.



## Outras Funções do Módulo

### `graphics.setColor()` (ou `:setColor()`)
**Assinatura:**
```lua
graphics.setColor(r, g, b, a)
```

Indica as cores de blend passadas e primitivas para a instâncias da renderização atual rgba ou de fading partículas.

### `graphics.drawLine()` (ou `:drawLine()`)
**Assinatura:**
```lua
graphics.drawLine(x1, y1, x2, y2, r, g, b, a)
```

Invoca as chamadas gráficas da GPU para as primitivas e objetos instanciados desenharem no buffer atual.

### `graphics.drawTriangle()` (ou `:drawTriangle()`)
**Assinatura:**
```lua
graphics.drawTriangle(x1, y1, x2, y2, x3, y3, r, g, b, a)
```

Retorna o ângulo (em radianos) entre dois pontos.

### `graphics.drawTexturedTriangle()` (ou `:drawTexturedTriangle()`)
**Assinatura:**
```lua
graphics.drawTexturedTriangle(texture, x1, y1, u1, v1, x2, y2, u2, v2, x3, y3, u3, v3, r, g, b, a)
```

Retorna o ângulo (em radianos) entre dois pontos.

### `graphics.setDepthTest()` (ou `:setDepthTest()`)
**Assinatura:**
```lua
graphics.setDepthTest(enable)
```

Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `graphics.clearDepth()` (ou `:clearDepth()`)
**Assinatura:**
```lua
graphics.clearDepth()
```

Zera todos pixels rastreados para cor pré definida no buffer no OpenGL com chamada de enum glClear.

### `graphics.setScissor()` (ou `:setScissor()`)
**Assinatura:**
```lua
graphics.setScissor(x, y, w, h)
```

Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `graphics.clearScissor()` (ou `:clearScissor()`)
**Assinatura:**
```lua
graphics.clearScissor()
```

Zera todos pixels rastreados para cor pré definida no buffer no OpenGL com chamada de enum glClear.

### `graphics.translate()` (ou `:translate()`)
**Assinatura:**
```lua
graphics.translate(tx, ty)
```

Desloca a posição pela matriz ou gráfico atual.

### `graphics.rotate()` (ou `:rotate()`)
**Assinatura:**
```lua
graphics.rotate(angle)
```

Rotaciona o objeto, matriz ou canvas num determinado ângulo (em radianos).

### `graphics.scale()` (ou `:scale()`)
**Assinatura:**
```lua
graphics.scale(sx, sy)
```

Aumenta ou diminui a escala pela matriz, vetor ou na tela.



## Outros Métodos Ocultos (Auto-Documentados)

Abaixo mapeamos automaticamente novas funções da API C++ que haviam ficado sem menção detalhada.

### `camera.setZoom()` (ou `:setZoom()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `setZoom`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:setZoom()
  ```

### `camera.getZoom()` (ou `:getZoom()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `getZoom`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:getZoom()
  ```

### `camera.setRotation()` (ou `:setRotation()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `setRotation`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:setRotation()
  ```

### `camera.getRotation()` (ou `:getRotation()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `getRotation`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:getRotation()
  ```

### `camera.setBounds()` (ou `:setBounds()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `setBounds`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:setBounds()
  ```

### `camera.clearBounds()` (ou `:clearBounds()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `clearBounds`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:clearBounds()
  ```

### `camera.clearDeadzone()` (ou `:clearDeadzone()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `clearDeadzone`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:clearDeadzone()
  ```

### `camera.setFollowLerp()` (ou `:setFollowLerp()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `setFollowLerp`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:setFollowLerp()
  ```

### `camera.follow()` (ou `:follow()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `follow`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:follow()
  ```

### `camera.zoomAt()` (ou `:zoomAt()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `zoomAt`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:zoomAt()
  ```

### `camera.shake()` (ou `:shake()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `shake`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:shake()
  ```

### `camera.attach()` (ou `:attach()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `attach`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:attach()
  ```

### `camera.detach()` (ou `:detach()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `detach`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:detach()
  ```

### `camera.screenToWorld()` (ou `:screenToWorld()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `screenToWorld`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:screenToWorld()
  ```

### `camera.isPointVisible()` (ou `:isPointVisible()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `isPointVisible`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:isPointVisible()
  ```

### `camera.isRectVisible()` (ou `:isRectVisible()`)

* **O que isso faz?** Função específica da engine principal (`camera`) para interagir com o parâmetro/estado `isRectVisible`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  camera:isRectVisible()
  ```
### `graphics.printOutline()` (ou `:printOutline()`)

* **O que isso faz?** Função específica da engine principal (`graphics`) para interagir com o parâmetro/estado `printOutline`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  graphics:printOutline()
  ```

### `graphics.printShadow()` (ou `:printShadow()`)

* **O que isso faz?** Função específica da engine principal (`graphics`) para interagir com o parâmetro/estado `printShadow`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  graphics:printShadow()
  ```

### `graphics.measure()` (ou `:measure()`)

* **O que isso faz?** Função específica da engine principal (`graphics`) para interagir com o parâmetro/estado `measure`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  graphics:measure()
  ```

### `graphics.drawArc()` (ou `:drawArc()`)

* **O que isso faz?** Função específica da engine principal (`graphics`) para interagir com o parâmetro/estado `drawArc`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  graphics:drawArc()
  ```

### `graphics.drawEllipse()` (ou `:drawEllipse()`)

* **O que isso faz?** Função específica da engine principal (`graphics`) para interagir com o parâmetro/estado `drawEllipse`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  graphics:drawEllipse()
  ```

### `graphics.drawPolygon()` (ou `:drawPolygon()`)

* **O que isso faz?** Função específica da engine principal (`graphics`) para interagir com o parâmetro/estado `drawPolygon`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  graphics:drawPolygon()
  ```

### `graphics.drawPolyLine()` (ou `:drawPolyLine()`)

* **O que isso faz?** Função específica da engine principal (`graphics`) para interagir com o parâmetro/estado `drawPolyLine`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  graphics:drawPolyLine()
  ```

### `graphics.setDefaultFilter()` (ou `:setDefaultFilter()`)

* **O que isso faz?** Função específica da engine principal (`graphics`) para interagir com o parâmetro/estado `setDefaultFilter`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  graphics:setDefaultFilter()
  ```

### `graphics.drawTexture()` (ou `:drawTexture()`)

* **O que isso faz?** Função específica da engine principal (`graphics`) para interagir com o parâmetro/estado `drawTexture`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  graphics:drawTexture()
  ```


## Outros Métodos Ocultos

### `graphics.setColor()`

* **O que isso faz?** Função exposta via API C++ do módulo `graphics` referenciando: `setColor`.
* **Como usar:** `graphics:setColor()`


### `graphics.drawLine()`

* **O que isso faz?** Função exposta via API C++ do módulo `graphics` referenciando: `drawLine`.
* **Como usar:** `graphics:drawLine()`


### `graphics.drawTriangle()`

* **O que isso faz?** Função exposta via API C++ do módulo `graphics` referenciando: `drawTriangle`.
* **Como usar:** `graphics:drawTriangle()`


### `graphics.drawTexturedTriangle()`

* **O que isso faz?** Função exposta via API C++ do módulo `graphics` referenciando: `drawTexturedTriangle`.
* **Como usar:** `graphics:drawTexturedTriangle()`


### `graphics.setDepthTest()`

* **O que isso faz?** Função exposta via API C++ do módulo `graphics` referenciando: `setDepthTest`.
* **Como usar:** `graphics:setDepthTest()`


### `graphics.setScissor()`

* **O que isso faz?** Função exposta via API C++ do módulo `graphics` referenciando: `setScissor`.
* **Como usar:** `graphics:setScissor()`


### `graphics.rotate()`

* **O que isso faz?** Função exposta via API C++ do módulo `graphics` referenciando: `rotate`.
* **Como usar:** `graphics:rotate()`
