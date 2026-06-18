# FastBatch (Renderização em Massa Extrema)

Cansado de chamar `graphics.drawRect()` para 5.000 partículas ou formatos e ver seu FPS começar a cair? O `FastBatch` envia matrizes de polígonos simples (sem textura) juntos via OpenGL em UMA única chamada VRAM de Instancing.
Isto serve para gerar partículas eficientes, chuva geométrica, Bullet Hell quadrados ou simulações numéricas. (Atenção: Diferente das Sprites, o FastBatch foca em figuras não-texturizadas renderizadas de forma super rápida!)

---

## Criando a Camada Batch

### `graphics.newFastBatch(max_sprites)`
* **O que isso faz?** Reserva memória na GPU o mais rápido possível acomodar polígonos simples coloridos instanciados.
* **Retorno:** `FastBatch`
* **Exemplo:** `mega_chuva = graphics.newFastBatch(10000)`

---

## Adicionando, Atualizando e Desenhando

Enquanto em modos tradicionais nós chamamos no `Draw`, no Batch nós Inserimos antes:

### `batch:add(x, y, w, h, r, g, b, [a], [vx], [vy])`
* Adiciona e devolve um novo bloco alocado ao buffer vazio!
* *Nota:* Diferente do Batch Clássico de Lua, aqui as Cores RGBA (0.0 a 1.0) vão direto nos parâmetros, além de aceitar velocidade X e velocidade Y como bônus (vx, vy).

### `batch:update(dt, screenW, screenH)`
* Move **todos** os objetos em massa de acordo com o `dt` (Delta Time) fornecido pela engine via as velocidades providas na adição! Ele repõe nos limites da tela se colidirem no `screenW` ou `screenH`.

### `batch:clear()`
* Excluí ou zera todas as posições salvas (Ótimo para quando muda de Fase no jogo).

### Desenhando na Tela
No momento exato de renderização gráfica:

```lua
-- Na sua função principal de loop ou cena desenhada:
mega_chuva:draw()
```


## Outras Funções do Módulo

### `fast_batch.__index()` (ou `:__index()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `fast_batch.__gc()` (ou `:__gc()`)
Acionado automaticamente pelo Coletor de Lixo (Garbage Collector) do Lua para destruir e liberar recursos complexos da memória C++. Você não precisa (e nem deve) invocar esse método manualmente.

