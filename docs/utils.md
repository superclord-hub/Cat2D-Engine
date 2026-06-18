# Utils (Outros Sistemas)

Dentre a framework temos pequenos módulos úteis focados em funcionalidades rápidas e esporádicas de pequenos usos em jogos de altíssimo benefício vital, como Clipboard, e timers Assíncronos (async).

---

## Módulo: Clipboard (Área de Transferência)

Comunicação com o famoso "Copiar e Colar" da memória RAM do celular.

### `Clipboard.copy(text)`
* **O que isso faz?** Copia a String para dentro do sistema nativo em Android de Copiar como se o usuário estivesse lido as mensagens segurado o dedo com marcações e apertado Copiar no UI Android.
* **Exemplo:** `Clipboard.copy("https://meusite.com")`

### `Clipboard.paste()`
* **O que isso faz?** Lê o que há retido e pendurado atualmente no sistema de Colar.

---

## Módulo: Async (Temporizadores Assíncronos)

### `async.setTimeout(ms, callback)` / `async.setInterval(ms, callback)`
* **`setTimeout`**: Executa uma função ÚNICA VEZ depois da quantia de MILISSEGUNDOS passadas (`ms / 1000 = segundos`).
* **`setInterval`**: Executa uma função CONTINUAMENTE a cada X milissegundos infinitamente até que seja removido!
* Ambos retornam um `id` numérico!
* **Exemplo:**
  ```lua
  -- Avisa depois de 2 segundos passados (2000 ms)
  local meu_timer = async.setTimeout(2000, function()
      system.log("Passado!")
  end)
  ```

### `async.clear(id)`
* Para imediatamente um temporizador contínuo de `setInterval` ou um agendado por `setTimeout` que ainda não aconteceu.

### `async.loadTextureBackground(path, callback)`
* **O que isso faz?** Pra não "travar" a tela enquanto lê uma Imagem muito grande e causa os jogos pularem quadros as texturas devem usar Load nas Costas da Main Thread liberada livre da CPU.
* **Exemplo:**
  ```lua
  async.loadTextureBackground("gigante.png", function(imagem)
       minhaImagem = imagem -- Guardou seguramente.
  end)
  ```

---

## Módulo: Table (Adições Úteis)

Manipular Tabelas é o cerne do Lua. Por isso adicionamos algumas funções escritas em C++ pela performance extra.

### `table.shuffle(sua_tabela)`
* Embaralha brutalmente 100% de forma Aleatória uma Table Indexada usando funções randômicas puras C-Native. (Muito mais rápido que embaralhar no Lua puro).
* *Aviso: Modifica a tabela na própria referência (In-Place).*

### `table.getRandom(sua_tabela)`
* Sorteia e retorna um único item contido ao Acaso sem alterar a tabela. Muito útil em sistemas de Loot!



## Outros Métodos Ocultos

### `utils.async()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `async`.
* **Como usar:** `utils:async()`


### `utils.__gc()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `__gc`.
* **Como usar:** `utils:__gc()`


### `utils.__index()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `__index`.
* **Como usar:** `utils:__index()`


### `utils._resume()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `_resume`.
* **Como usar:** `utils:_resume()`


### `utils.camera()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `camera`.
* **Como usar:** `utils:camera()`


### `utils.Clipboard()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `Clipboard`.
* **Como usar:** `utils:Clipboard()`


### `utils.id()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `id`.
* **Como usar:** `utils:id()`


### `utils.basePath()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `basePath`.
* **Como usar:** `utils:basePath()`


### `utils.savePath()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `savePath`.
* **Como usar:** `utils:savePath()`


### `utils.fs()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `fs`.
* **Como usar:** `utils:fs()`


### `utils.COLOR_BUFFER_BIT()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `COLOR_BUFFER_BIT`.
* **Como usar:** `utils:COLOR_BUFFER_BIT()`


### `utils.DEPTH_BUFFER_BIT()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `DEPTH_BUFFER_BIT`.
* **Como usar:** `utils:DEPTH_BUFFER_BIT()`


### `utils.DEPTH_TEST()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `DEPTH_TEST`.
* **Como usar:** `utils:DEPTH_TEST()`


### `utils.CULL_FACE()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `CULL_FACE`.
* **Como usar:** `utils:CULL_FACE()`


### `utils.BLEND()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `BLEND`.
* **Como usar:** `utils:BLEND()`


### `utils.ARRAY_BUFFER()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `ARRAY_BUFFER`.
* **Como usar:** `utils:ARRAY_BUFFER()`


### `utils.ELEMENT_ARRAY_BUFFER()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `ELEMENT_ARRAY_BUFFER`.
* **Como usar:** `utils:ELEMENT_ARRAY_BUFFER()`


### `utils.STATIC_DRAW()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `STATIC_DRAW`.
* **Como usar:** `utils:STATIC_DRAW()`


### `utils.DYNAMIC_DRAW()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `DYNAMIC_DRAW`.
* **Como usar:** `utils:DYNAMIC_DRAW()`


### `utils.FLOAT()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `FLOAT`.
* **Como usar:** `utils:FLOAT()`


### `utils.UNSIGNED_SHORT()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `UNSIGNED_SHORT`.
* **Como usar:** `utils:UNSIGNED_SHORT()`


### `utils.UNSIGNED_INT()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `UNSIGNED_INT`.
* **Como usar:** `utils:UNSIGNED_INT()`


### `utils.TRIANGLES()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `TRIANGLES`.
* **Como usar:** `utils:TRIANGLES()`


### `utils.LINES()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `LINES`.
* **Como usar:** `utils:LINES()`


### `utils.LEQUAL()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `LEQUAL`.
* **Como usar:** `utils:LEQUAL()`


### `utils.LESS()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `LESS`.
* **Como usar:** `utils:LESS()`


### `utils.SRC_ALPHA()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `SRC_ALPHA`.
* **Como usar:** `utils:SRC_ALPHA()`


### `utils.ONE_MINUS_SRC_ALPHA()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `ONE_MINUS_SRC_ALPHA`.
* **Como usar:** `utils:ONE_MINUS_SRC_ALPHA()`


### `utils.x()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `x`.
* **Como usar:** `utils:x()`


### `utils.y()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `y`.
* **Como usar:** `utils:y()`


### `utils.deltaX()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `deltaX`.
* **Como usar:** `utils:deltaX()`


### `utils.deltaY()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `deltaY`.
* **Como usar:** `utils:deltaY()`


### `utils.timeActive()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `timeActive`.
* **Como usar:** `utils:timeActive()`


### `utils.pressure()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `pressure`.
* **Como usar:** `utils:pressure()`


### `utils.state()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `state`.
* **Como usar:** `utils:state()`


### `utils.parse()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `parse`.
* **Como usar:** `utils:parse()`


### `utils.cat()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `cat`.
* **Como usar:** `utils:cat()`


### `utils.packFloatArray()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `packFloatArray`.
* **Como usar:** `utils:packFloatArray()`


### `utils.packUShortArray()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `packUShortArray`.
* **Como usar:** `utils:packUShortArray()`


### `utils.packUIntArray()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `packUIntArray`.
* **Como usar:** `utils:packUIntArray()`


### `utils.math_ext()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `math_ext`.
* **Como usar:** `utils:math_ext()`


### `utils.identity()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `identity`.
* **Como usar:** `utils:identity()`


### `utils.rotateX()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `rotateX`.
* **Como usar:** `utils:rotateX()`


### `utils.rotateY()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `rotateY`.
* **Como usar:** `utils:rotateY()`


### `utils.rotateZ()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `rotateZ`.
* **Como usar:** `utils:rotateZ()`


### `utils.getRandomSeed()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `getRandomSeed`.
* **Como usar:** `utils:getRandomSeed()`


### `utils.newRandomGenerator()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `newRandomGenerator`.
* **Como usar:** `utils:newRandomGenerator()`


### `utils.apply()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `apply`.
* **Como usar:** `utils:apply()`


### `utils.clone()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `clone`.
* **Como usar:** `utils:clone()`


### `utils.inverse()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `inverse`.
* **Como usar:** `utils:inverse()`


### `utils.getMatrix()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `getMatrix`.
* **Como usar:** `utils:getMatrix()`


### `utils.setMatrix()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `setMatrix`.
* **Como usar:** `utils:setMatrix()`


### `utils.setTransformation()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `setTransformation`.
* **Como usar:** `utils:setTransformation()`


### `utils.shear()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `shear`.
* **Como usar:** `utils:shear()`


### `utils.reset()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `reset`.
* **Como usar:** `utils:reset()`


### `utils.inverseTransformPoint()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `inverseTransformPoint`.
* **Como usar:** `utils:inverseTransformPoint()`


### `utils.isAffine2D()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `isAffine2D`.
* **Como usar:** `utils:isAffine2D()`


### `utils.setGravity()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `setGravity`.
* **Como usar:** `utils:setGravity()`


### `utils.shape()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `shape`.
* **Como usar:** `utils:shape()`


### `utils.point_x()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `point_x`.
* **Como usar:** `utils:point_x()`


### `utils.point_y()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `point_y`.
* **Como usar:** `utils:point_y()`


### `utils.normal_x()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `normal_x`.
* **Como usar:** `utils:normal_x()`


### `utils.normal_y()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `normal_y`.
* **Como usar:** `utils:normal_y()`


### `utils.alpha()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `alpha`.
* **Como usar:** `utils:alpha()`


### `utils.gradient_x()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `gradient_x`.
* **Como usar:** `utils:gradient_x()`


### `utils.gradient_y()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `gradient_y`.
* **Como usar:** `utils:gradient_y()`


### `utils.pointA_x()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `pointA_x`.
* **Como usar:** `utils:pointA_x()`


### `utils.pointA_y()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `pointA_y`.
* **Como usar:** `utils:pointA_y()`


### `utils.pointB_x()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `pointB_x`.
* **Como usar:** `utils:pointB_x()`


### `utils.pointB_y()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `pointB_y`.
* **Como usar:** `utils:pointB_y()`


### `utils.points()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `points`.
* **Como usar:** `utils:points()`


### `utils.__mode()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `__mode`.
* **Como usar:** `utils:__mode()`


### `utils.spatial()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `spatial`.
* **Como usar:** `utils:spatial()`


### `utils.sys()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `sys`.
* **Como usar:** `utils:sys()`


### `utils.__add()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `__add`.
* **Como usar:** `utils:__add()`


### `utils.__sub()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `__sub`.
* **Como usar:** `utils:__sub()`


### `utils.__mul()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `__mul`.
* **Como usar:** `utils:__mul()`


### `utils.__div()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `__div`.
* **Como usar:** `utils:__div()`


### `utils.__unm()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `__unm`.
* **Como usar:** `utils:__unm()`


### `utils.__eq()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `__eq`.
* **Como usar:** `utils:__eq()`


### `utils.__tostring()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `__tostring`.
* **Como usar:** `utils:__tostring()`


### `utils.video()`

* **O que isso faz?** Função exposta via API C++ do módulo `utils` referenciando: `video`.
* **Como usar:** `utils:video()`
