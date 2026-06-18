# System (Sistema Core)

O módulo `System` auxilia com tarefas essenciais interativas de baixo nível com a própria engine, manipulações de vida (GC), hardware e notificações.

---

## Tempo e Monitoramento

### `system.getTime()`
* Retorna o tempo em segundos desde uma época fixa (Epoch Unix / High resolution clock). Pode ser utilizado de forma similar ao func de Os para calcular deltas super precisos!

### `system.getDeltaTime()`
* Traz o tempo transcorrido no último quadro. Permite que as Físicas de seu Game ocorram a um tempo constante independente de lag (Taxas Frame-Rate Independentes).

### `system.getFPS()` e `system.getDrawCalls()`
* Retorna o Frame-Rate do jogo, e as Chamadas de Renderização (Quantidade de vezes requeridas à Placa de Vídeo em um frame para desenhar suas coisas).

### `system.getScreenWidth()` e `system.getScreenHeight()`
* Traz a largura e altura totais do quadro limpo!

---

## Armazenamento Leve Local

### `system.write(key, value)` e `system.read(key)`
* **O que faz?** Uma mão na roda pra salvar e recuperar progresso rápido (Score, Dinheiro, Status). Não precisa utilizar o `Filesystem` pesado. Ele salva nas Preferências Ocultas Nativas da base Android em um HashMap imutável.

---

## Hardware e Básico

### `system.exit()`
* Dá "Quit". Fecha o jogo imediatamente finalizando de vez o processo nativo.

### `system.getDensity()`
* Retorna a densidade de pixels global (DPI scaling) da tela do seu dispositivo de Hardware!

### `system.scheduleNotification(id, title, message, delay)`
* **O que faz?** Promove Retenção do Player! Dispara um pop-up clássico Android empurrado, trazendo de volta o usuário ao Jogo depois de "delay" milisegundos com "Sua roça está pronta!".
* **Para cancelar:** `system.cancelNotification(id)`

### `system.hapticImpact(style)`
* Mais sutil e refinado que um "vibrate" gigante que balança partes soltas do aparelho, os Haptics variam o `style` (`0` leve, `1` médio, `2` pesado, `3` tick rígido) de motor linear em aparelhos de nova geração em toques satisfatórios de UI.

---

## Memória e GC (Garbage Collector)

### `system.getLuaMemoryUsage()`
* Mede quantos KB o Lua e tabelas comem de fato.

### `system.gcCollect()`, `system.gcPause()`, `system.gcResume()`
* Pausa e resume o coletor pra não engasgar o jogo durante cenas onde alocar/limpar faria frames perderem sincronia na tela (Stuttering Famoso).

---

## Threads (Assincronicidade Própria)

### `system.runAsync(func)`
* Desvia um bloco de Lua e o joga na CPU secundária nos bastidores nativos da Engine. Ideal para descompactar ZIP, ou interpretar mapas sem Freezar o display!

### `system.log(message)`
* É o clássico `print()`. Imprime no console nativo (Logcat).


## Outras Funções do Módulo

### `async.setTimeout()` (ou `:setTimeout()`)
**Assinatura:**
```lua
async.setTimeout(ms, callback)
```

Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `async.setInterval()` (ou `:setInterval()`)
**Assinatura:**
```lua
async.setInterval(ms, callback)
```

Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `async.clear()` (ou `:clear()`)
**Assinatura:**
```lua
batch:clear()
```

Zera todos pixels rastreados para cor pré definida no buffer no OpenGL com chamada de enum glClear.

### `async.loadTextureBackground()` (ou `:loadTextureBackground()`)
**Assinatura:**
```lua
async.loadTextureBackground(path, callback)
```

Arredonda o valor para o número inteiro mais próximo.

