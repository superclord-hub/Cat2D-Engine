# OpenGL ES (Nativo Baixo-Nível)

As funções tradicionais de desenho em tela cobrem 99% dos casos, porém certas implementações gráficas monstruosas requerem chamar OpenGL GLES2.0/3.0 nativamente e puramente pra injetar Buffers e Triângulos (Ex: Desenhar 3D e Modelos complexos direto)!

A Engine Cat expõe globalmente a mesa do C/C++ livre de abstrações: `gl`.

---

## Variáveis (Enumeradores de Estado GL)
* **Padrões Buffer:** `gl.COLOR_BUFFER_BIT`, `gl.DEPTH_BUFFER_BIT`
* **Testes de Estado:** `gl.DEPTH_TEST`, `gl.CULL_FACE`, `gl.BLEND`
* **Primitivas:** `gl.TRIANGLES`, `gl.LINES`
* **Formatos de Dados:** `gl.FLOAT`, `gl.UNSIGNED_SHORT`... etc

---

## Funções Comuns
* `gl.clearColor(r, g, b, a)`: Altera a cor de fundo bruta da janela/buffer ativo.
* `gl.clear(mascara)`: Limpa cor e profundidade nativa (Ex: `gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT)`). Porem o bitwise no LUA se usa Operador Matemático ou a Engine tem conversão. Na engine Lua puro, não temos bitwise, mas isso existe na API C++.
* `gl.enable(estado)` / `gl.disable(estado)`: Liga testes globais nativos. (ex: `gl.enable(gl.DEPTH_TEST)`)
* `gl.viewport(x, y, w, h)`: Fixando Lentes.

### Buffers (Malhas de Vértices)
* `gl.createBuffer(is_element_array, array_vertices)`
* `gl.bindBuffer(target, buffer)`
* `gl.deleteBuffer(buffer)`
* `gl.bufferData(...)`

### Atributos e Desenho Puro Moderno M2
* `gl.enableVertexAttribArray(index)` e `gl.vertexAttribPointer(...)`
* `gl.useProgram(shader_bruto)`
* `gl.drawArrays(modo, indice_inicial, total)`
* `gl.drawElements(modo, total, tipo_variavel, buffer_ponteiro)`

**AVISO EXTREMO:** NUNCA quebre as regras do State-Machine ao usar `gl`. Você trancará a Engine na Thread inteira caso falhe ou desconfigure sem retornar.


## Outras Funções do Módulo

### `gl.ARRAY_BUFFER()` (ou `:ARRAY_BUFFER()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `gl.ELEMENT_ARRAY_BUFFER()` (ou `:ELEMENT_ARRAY_BUFFER()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `gl.STATIC_DRAW()` (ou `:STATIC_DRAW()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `gl.DYNAMIC_DRAW()` (ou `:DYNAMIC_DRAW()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `gl.UNSIGNED_INT()` (ou `:UNSIGNED_INT()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `gl.LEQUAL()` (ou `:LEQUAL()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `gl.LESS()` (ou `:LESS()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `gl.SRC_ALPHA()` (ou `:SRC_ALPHA()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `gl.ONE_MINUS_SRC_ALPHA()` (ou `:ONE_MINUS_SRC_ALPHA()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `gl.depthFunc()` (ou `:depthFunc()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `gl.blendFunc()` (ou `:blendFunc()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `gl.disableVertexAttribArray()` (ou `:disableVertexAttribArray()`)
**Assinatura:**
```lua
gl.disableVertexAttribArray(index)
```

Muda states no Contexto OpenGL para forçar desligamento GL enum (Ex. depth, cut, stencil).



## Outros Métodos Ocultos (Auto-Documentados)

Abaixo mapeamos automaticamente novas funções da API C++ que haviam ficado sem menção detalhada.

### `gl_lowlevel.depthFunc()` (ou `:depthFunc()`)

* **O que isso faz?** Função específica da engine principal (`gl_lowlevel`) para interagir com o parâmetro/estado `depthFunc`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  gl_lowlevel:depthFunc()
  ```

### `gl_lowlevel.blendFunc()` (ou `:blendFunc()`)

* **O que isso faz?** Função específica da engine principal (`gl_lowlevel`) para interagir com o parâmetro/estado `blendFunc`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  gl_lowlevel:blendFunc()
  ```


## Outros Métodos Ocultos

### `gl_lowlevel.disableVertexAttribArray()`

* **O que isso faz?** Função exposta via API C++ do módulo `gl_lowlevel` referenciando: `disableVertexAttribArray`.
* **Como usar:** `gl_lowlevel:disableVertexAttribArray()`
