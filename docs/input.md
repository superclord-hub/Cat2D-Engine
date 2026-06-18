# Input (Entrada de Dados)

O módulo `Input` é responsável por gerenciar tudo relacionado à interação do usuário com o dispositivo, como toques na tela (touch), digitação no teclado, e leituras dos sensores (giroscópio, acelerômetro, gestos).

---

## Funções Disponíveis (Touch/Toques)

### `input.isTouched()` / `input.isJustTouched()` / `input.isTouchReleased()`
* **`isTouched()`**: Verifica se a tela está sendo tocada ou pressionada naquele exato frame. (true contínuo)
* **`isJustTouched()`**: Retorna verdadeiro apenas no exato momento inicial (frame único) do toque. Útil para botões de interface.
* **`isTouchReleased()`**: Retorna verdadeiro apenas no exato momento (frame unico) em que o dedo solta a tela (levantou o dedo).
* **Exemplo:**
  ```lua
  if input.isJustTouched() then
      player.pular() -- Só pula uma vez por toque
  end
  ```

### `input.getTouchX()` / `input.getTouchY()` / `input.getTouchPressure()`
* **O que isso faz?** Retorna as coordenadas X e Y do primeiro dedo na tela. O Pressure te diz a "força" física esmagada real do dedo (Hardware dependente).
* **Retorno:** `number`
* **Exemplo:**
  ```lua
  if input.isTouched() then
      local tx = input.getTouchX()
      local ty = input.getTouchY()
      graphics.drawRect(tx - 25, ty - 25, 50, 50, 1, 0, 0, 1)
  end
  ```

### `input.getTouches()` e `input.getTouch(id)`
* **O que isso faz?** O Android suporta Multi-Touch (múltiplos toques ao mesmo tempo). Essas funções capturam dados de todos os dedos.
* **Retorno `getTouches()`:** Array contendo as Tabelas de todos os toques ativos na tela.
* **Retorno `getTouch(id)`:** Tabela Única com `{id, x, y, deltaX, deltaY, timeActive, pressure, state}`

---

## Funções Disponíveis (Teclado e Texto)

### `input.startTextInput(texto_inicial)` / `input.stopTextInput()`
* **O que isso faz?** Mostra e oculta o teclado virtual da tela.
* **Exemplo:**
  ```lua
  input.startTextInput("Digite seu nome...")
  ```

### `input.getText()` / `input.setText(text)`
* **O que isso faz?** Retorna ou configura o texto que o usuário digitou no teclado nativo.
* **Retorno `getText()`:** `String`.

### `input.getKeyboardHeight()`
* **O que isso faz?** Retorna a altura real tomada (em pixels) pelo teclado na janela atual, permitindo que você suba os inputs visuais para não serem obstruídos pela base virtual!

### `input.wasBackPressed()`
* **O que isso faz?** Checa se o usuário retornou, deslizando do canto da tela ou botões de voltar virtuais no botão inferior. Útil pra menus.
* **Retorno:** `Booleano`.

---

## Sensores e Gestos do Aparelho

### `input.getAccelerometer()` e `input.getGyroscope()`
* **O que isso faz?** Capturam os ângulos de rotação do celular e forças atuantes nele.
* **Retorno:** Retornam variáveis x, y e z.
* **Exemplo:**
  ```lua
  local ax, ay, az = input.getAccelerometer()
  player.x = player.x + ax * delta
  ```

### `input.getPinchScale()` / `input.getRotationAngle()`
* **O que isso faz?** Detecta ações nativas rápidas como gesto de pinça (Zoom em mapa) ou giros com 2 dedos.
* **Retorno:** `number` (escala ou ângulo).

### `input.getFlingVelocity()`
* **O que isso faz?** Identifica quão rápido a tela foi "arrastada e soltada" fisicamente. Devolve os valores de `velocidadeX` e `velocidadeY` que você pode usar para rolar Mapas no inércia da física (Igual o arrastar do Instagram e etc).

### `input.wasDoubleTapped()` / `input.wasSwiped()`
* **O que isso faz?** Captura duplo-toques na tela ou deslizadas rápidas puras diretas (swipe simples).


## Outras Funções do Módulo

### `Clipboard.copy()` (ou `:copy()`)
**Assinatura:**
```lua
Clipboard.copy(text)
```

Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `Clipboard.paste()` (ou `:paste()`)
**Assinatura:**
```lua
Clipboard.paste()
```

Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

