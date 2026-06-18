# Video (Reprodutor Interativo)

Por mais que o Lua desenhe jogos em si, as vezes é maravilhoso exibir aquela "Cutscene" no meio do jogo. A Cat Engine te permite desenhar Filmes renderizados (MP4/WebM) diretamente acima das outras janelas sem ter que recriar players complexos do zero via `video`.

---

## Modos de Uso

### `video.load(options)`
* Devolve e carrega a referência ao decodificador do MP4 (O ideal é estar nos `assets/` compactado). Passamos uma tabela com diversas configurações que constroem a camada do motor de Vídeo.
* **Opções suportadas:** `url` (String, caminho do arquivo), `isGif` (Boolean), `x` e `y` (Float, Posições), `width` e `height` (Float, Dimensões), `alpha` (Float, Transparência), `loop` (Boolean, padrão true), `volume` (Float, padrão 1.0), `fullscreen` (Boolean).
* **Retorna:** `Player`
* **Exemplo:** `minha_cutscene = video.load({ url = "cutscene01.mp4", loop = false, fullscreen = true })`

---

## Controles de Player (Play/Pause)

Após carregado no seu objeto de estado:

* `minha_cutscene:play()` - Toca / Resume o Video!
* `minha_cutscene:pause()` - Pausa no Frame atual.
* `minha_cutscene:destroy()` - Destruidor de memória, limpa a VRAM quando não quiser usar.
* `minha_cutscene:setVolume(volume)` - Float de `0.0` a `1.0`.
* `minha_cutscene:update(x, y, width, height, alpha)` - Atualiza os parâmetros visuais da caixa de renderização do vídeo em tempo real (como a posição tela ou deixar semi-transparente).

---

## Tempo e Velocidade

* `minha_cutscene:seek(time_in_seconds)` - Avança pra frente ou puxa pra trás o filme. (Ex: `minha_cutscene:seek(10)` = Ir para 10 segundos).
* `minha_cutscene:setSpeed(speed)` - Slow motion (`0.5`) ou Avanço Rápido (`2.0`). Padrão `1.0`.
* `minha_cutscene:getDuration()` - Float dos segundos totais do vídeo.
* `minha_cutscene:getCurrentTime()` - Retorna onde o ponteiro do frame se encontra em float.
* `minha_cutscene:isPlaying()` - Retorna boolean para testes lógicos de interface de "Play/Pause Button".

---

## Avisos Mágicos
> O Módulo de vídeo desenha a janela **por cima de toda sua engine**. Durante ou num update de fase. Ele não é instanciado na mesa nativa `graphics.draw()` como os Sprites para usar escalas mirabolantes e etc, ele possui Player Nativo C++ Android de máxima performance de HW Acceleration (Acoplado pelo código interno). Pense nele como cenas exclusivas em Tela Cheia e não uma textura mapeada num cubo!


## Outras Funções do Módulo

### `video.__gc()` (ou `:__gc()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `video.__index()` (ou `:__index()`)
Metamétodo interno da tabela Lua usado para conectar e buscar propriedades virtuais diretamente da Classe C++. Fica oculto do uso no dia a dia da API.

