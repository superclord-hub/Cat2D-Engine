# Shaders (Efeitos Visuais Avançados)

O módulo `Shader` permite que você manipule os pixels e os vértices diretamente na placa de vídeo (GPU) através de GLSL, criando efeitos como Reflexos na Água, Distorções, Sombreados complexos, Iluminações e muito mais!

---

## Criando um Shader

### `graphics.newShader(vertexCode, fragmentCode)`

* **O que isso faz?** Compila o código fonte em texto GLSL e devolve um objeto `Shader` pronto para o uso e envio para Placa de Vídeo. Você pode passar `nil` em um deles se quiser usar o Shader padrão da engine para aquela etapa.
* **Parâmetros:**
  * `vertexCode` (string): Código GLSL do Vertex Shader responsável pelas posições geométricas.
  * `fragmentCode` (string): Código GLSL do Fragment Shader (Pixel) responsável pelas cores finais de cada pontinho na tela.
* **Retorno:** `Shader`
* **Exemplo de Shader que inverte as cores (Negativo):**
  ```lua
  local invertCode = [[
  vec4 effect(vec4 color, Image texture, vec2 texture_coords, vec2 screen_coords) {
      vec4 pixel = Texel(texture, texture_coords);
      return vec4(1.0 - pixel.r, 1.0 - pixel.g, 1.0 - pixel.b, pixel.a) * color;
  }
  ]]
  local meuShader = graphics.newShader(nil, invertCode)
  ```

---

## Usando e Aplicando Shaders

### `graphics.setShader(shader)`

* **O que isso faz?** Ativa o shader alvo! A partir do momento em que for chamado, tudo desenhado a seguir na tela passará por ele, até que você o desative passando `nil`.
* **Como e pra que eu uso?**
  ```lua
  graphics.setShader(meuShader)
  graphics.draw(my_image, 100, 100)
  graphics.setShader(nil) -- Desliga o efeito para que os próximos desenhos não fiquem invertidos!
  ```

---

## Modificando Shaders em Tempo-Real (Uniforms)

Frequentemente precisamos alterar forças e números do Shader de dentro do mundo para o mundo do GLSL (Ex: Passar o tempo contínuo para movimentar ondas, mandar cor da luz). Isso é feito "Evniando" variáveis uniformes.

### `shader:sendInt(name, value)` / `shader:sendFloat(name, value)`
* **O que isso faz?** Manda variáveis reais via Lua diretamente para as aberturas `uniform int/float variavel;` alocadas no código GLSL compilado.
* **Exemplo: Animando Ondulações baseadas num tempo!**
  ```lua
  local meu_tempo = meu_tempo + system.getDeltaTime()
  meuShader:sendFloat("iTime", meu_tempo)
  ```

### `shader:sendMatrix(name, value)` / `shader:sendTexture(name, texture)`
* **O que isso faz?** Passam Matrizes transformacionais ou Imagens de Máscaras extras (como Mapas de Normais) que a engine possuí pro código GLSL calcular.


## Outras Funções do Módulo

### `shader.__gc()` (ou `:__gc()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `shader.__index()` (ou `:__index()`)
Metamétodo interno da tabela Lua usado para conectar e buscar propriedades virtuais diretamente da Classe C++. Fica oculto do uso no dia a dia da API.

