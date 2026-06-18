# Audio (Áudio)

O módulo `Audio` é responsável pelo gerenciamento rápido e efetivo de sistemas sonoros como efeitos (SFX), músicas de fundo (BGM), controlando volumes, loops e pausas.

---

## Funções Disponíveis

### `audio.loadSound(path)`

* **O que isso faz?** Carrega um arquivo de áudio e o prepara para ser tocado. Pode carregar efeitos curtos ou músicas mais compridas em formatos populares como OGG (Sugerido para performance) ou MP3.
* **Parâmetros:**
  * `path` (string): Caminho do recurso sonoro.
* **Retorno:**
  * `Sound` - O objeto responsável pelo recurso sonoro pronto para ser invocado.
* **Exemplo:**
  ```lua
  local music = audio.loadSound("theme.ogg")
  local jumpSfx = audio.loadSound("jump.mp3")
  ```
* **O que acontece se eu usar errado?** Se o caminho não existir, a resource não será carregada e ocorrerá um erro ao ser chamada posteriormente usando algo como `music:play()`. Lembre-se também de NÃO usar essa função repetidas vezes ou inseri-la num loop.

---

### `sound:play()`

* **O que isso faz?** Inicia a reprodução de um áudio.
* **Retorno:** `Nenhum`
* **Exemplo:**
  ```lua
  -- Toca o Pulo quando solicitado
  if player_jumped then
      jumpSfx:play()
  end
  ```

---

### `sound:stop()`

* **O que isso faz?** Interrompe um som que está sendo tocado instantaneamente.
* **Exemplo:**
  ```lua
  music:stop()
  ```

---

### `sound:setVolume(volume)`

* **O que isso faz?** Ajusta o nível de ganho ou volume do seu som.
* **Parâmetros:**
  * `volume` (number): Normalizado usualmente entre `0.0` (mudo) e `1.0` (Volume total).
* **O que acontece se eu usar errado?** Modificar para valores excessivamente distantes do alcance (ex: `100.0` na onde deveria ser entre 0 e 1) pode não ter nenhum efeito adicional ou causar clipping grave.


## Outras Funções do Módulo

### `audio.__gc()` (ou `:__gc()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `audio.__index()` (ou `:__index()`)
Metamétodo interno da tabela Lua usado para conectar e buscar propriedades virtuais diretamente da Classe C++. Fica oculto do uso no dia a dia da API.

### `audio.pause()` (ou `:pause()`)
**Assinatura:**
```lua
sound:pause()
```

Pausa o som, reprodução ou sistema em sua exata posição corrente para reinício depois.

### `audio.getVolume()` (ou `:getVolume()`)
**Assinatura:**
```lua
sound:getVolume()
```

Informa aos módulos ou lua que retornam float contendo o volume setado.

### `audio.setLooping()` (ou `:setLooping()`)
**Assinatura:**
```lua
sound:setLooping(loop)
```

Se for true (verdadeiro), configurará repetição eterna do objeto até ele mandar ele pausar.

### `audio.isLooping()` (ou `:isLooping()`)
**Assinatura:**
```lua
sound:isLooping()
```

Retorna qual o estado do Loop atual de um som. (True/False)

### `audio.isPlaying()` (ou `:isPlaying()`)
**Assinatura:**
```lua
sound:isPlaying()
```

Retorna true caso o objeto atual esteja reproduzindo ativamente seu loop.

### `audio.setPitch()` (ou `:setPitch()`)
**Assinatura:**
```lua
sound:setPitch(pitch)
```

Modifica a velocidade e altura em reproduções contínuas, influenciando no pitch de timbres.

### `audio.getPitch()` (ou `:getPitch()`)
**Assinatura:**
```lua
sound:getPitch()
```

Obtem a velocidade original da gravação em uso.

### `audio.setPan()` (ou `:setPan()`)
**Assinatura:**
```lua
sound:setPan(pan)
```

Controla o balanço e pan estéreo (Left, Right) via ponto flutuante.

### `audio.getPan()` (ou `:getPan()`)
**Assinatura:**
```lua
sound:getPan()
```

Indica o estado presente do valor do Pan definido antes.

### `audio.getLength()` (ou `:getLength()`)
**Assinatura:**
```lua
sound:getLength()
```

Tempo total até onde uma fita ou som tem limite na taxa nativa.

### `audio.getPosition()` (ou `:getPosition()`)
**Assinatura:**
```lua
sound:getPosition()
```

Recebe a posição no tempo, referencial ou localização em instâncias.

### `audio.setPosition()` (ou `:setPosition()`)
**Assinatura:**
```lua
sound:setPosition(seconds)
```

Empurra/Configura o posicionamento físico principal da entidade.

### `audio.setFade()` (ou `:setFade()`)
**Assinatura:**
```lua
sound:setFade(fade_in, fade_out)
```

Gera controles de ganho/fadings com tempo gradual no som sem travar threads principais.

### `audio.setSpatialPosition()` (ou `:setSpatialPosition()`)
**Assinatura:**
```lua
sound:setSpatialPosition(x, y, z)
```

Anexa em 3 coordenadas X, Y e Z um locutor que vai falar se o Audio emite em 3D com a distância focal.

### `audio.setSpatialPositioning()` (ou `:setSpatialPositioning()`)
**Assinatura:**
```lua
sound:setSpatialPositioning(enabled)
```

Anexa em 3 coordenadas X, Y e Z um locutor que vai falar se o Audio emite em 3D com a distância focal.

### `audio.setVelocity()` (ou `:setVelocity()`)
**Assinatura:**
```lua
sound:setVelocity(x, y, z)
```

Gera inércia aplicando vetores de velocidades em corpo dinâmico na X/Y.

### `audio.setDirection()` (ou `:setDirection()`)
**Assinatura:**
```lua
sound:setDirection(x, y, z)
```

Associa a direção para onde algo está virado.

### `audio.setGlobalVolume()` (ou `:setGlobalVolume()`)
**Assinatura:**
```lua
audio.setGlobalVolume(volume)
```

Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `audio.getGlobalVolume()` (ou `:getGlobalVolume()`)
**Assinatura:**
```lua
audio.getGlobalVolume()
```

Retorna como ponto flutuante o volume universal/mestre atual.

### `audio.setListenerPosition()` (ou `:setListenerPosition()`)
**Assinatura:**
```lua
audio.setListenerPosition(x, y, z)
```

Modifica ouvintes virtuais espacializados 3D da biblioteca auditiva. Útil para jogos de tiro/passos.

### `audio.setListenerVelocity()` (ou `:setListenerVelocity()`)
**Assinatura:**
```lua
audio.setListenerVelocity(x, y, z)
```

Modifica ouvintes virtuais espacializados 3D da biblioteca auditiva. Útil para jogos de tiro/passos.

### `audio.setListenerDirection()` (ou `:setListenerDirection()`)
**Assinatura:**
```lua
audio.setListenerDirection(x, y, z)
```

Modifica ouvintes virtuais espacializados 3D da biblioteca auditiva. Útil para jogos de tiro/passos.



## Outros Métodos Ocultos

### `audio.setLooping()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setLooping`.
* **Como usar:** `audio:setLooping()`


### `audio.setPitch()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setPitch`.
* **Como usar:** `audio:setPitch()`


### `audio.setPan()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setPan`.
* **Como usar:** `audio:setPan()`


### `audio.setPosition()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setPosition`.
* **Como usar:** `audio:setPosition()`


### `audio.setFade()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setFade`.
* **Como usar:** `audio:setFade()`


### `audio.setSpatialPosition()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setSpatialPosition`.
* **Como usar:** `audio:setSpatialPosition()`


### `audio.setSpatialPositioning()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setSpatialPositioning`.
* **Como usar:** `audio:setSpatialPositioning()`


### `audio.setDirection()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setDirection`.
* **Como usar:** `audio:setDirection()`


### `audio.setGlobalVolume()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setGlobalVolume`.
* **Como usar:** `audio:setGlobalVolume()`


### `audio.setListenerPosition()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setListenerPosition`.
* **Como usar:** `audio:setListenerPosition()`


### `audio.setListenerVelocity()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setListenerVelocity`.
* **Como usar:** `audio:setListenerVelocity()`


### `audio.setListenerDirection()`

* **O que isso faz?** Função exposta via API C++ do módulo `audio` referenciando: `setListenerDirection`.
* **Como usar:** `audio:setListenerDirection()`
