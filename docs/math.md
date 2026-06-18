# Math (Matemática)

O módulo `Math` tem a finalidade de otimizar e fornecer atalhos velozes para cálculos e algoritmos comuns aos desenvolvedores de jogos 2D, variando de limitações simplórias a obtenção rápida geométrica. 

> **Nota Importante:** Todas as funções matemáticas padrões e originais da própria linguagem Lua (`math.cos`, `math.sin`, `math.pi`, `math.rad`, `math.deg`, `math.abs`, `math.min`, `math.max`, etc.) estão **nativa e completamente disponíveis** na framework, em conjunto com estas extensões personalizadas abaixo criadas especificamente pela Engine para Game Dev.

---

## Funções Disponíveis

### `math.distance(x1, y1, x2, y2)`

* **O que isso faz?** Fornece o atalho pra fórmula básica de Distância Espacial "Pitágoras", entregando qual espaço absoluto existe separando os eixos de 2 instâncias diferentes.
* **Parâmetros:**
  * `x1`, `y1` (number): Coordenadas da primeira instância.
  * `x2`, `y2` (number): Posições do segundo objeto.
* **Retorno:**
  * `number` - O comprimento absoluto separando o Ponto A e B em double precisions.
* **Exemplo:**
  ```lua
  -- O inimigo o enxergou a tempo?
  if math.distance(player.x, player.y, enemy.x, enemy.y) < 150 then
      enemy:atacar()
  end
  ```

---

### `math.clamp(value, min, max)`

* **O que isso faz?** Utilizado extensivamente para limitar movimentações ou escalas, prevenindo que um valor ultrapasse limites rigorosos por engano mantendo suas proporções em cheques constantes. Repõe limites automaticamente.
* **Parâmetros:**
  * `value` (number): O valor flutuante testado.
  * `min` (number): O limite de segurança mínimo do elemento.
  * `max` (number): O limitador supremo de subida de segurança.
* **Retorno:**
  * `number` normalizado e estabilizado respeitando de perto esses horizontes.
* **Exemplo:**
  ```lua
  -- Não permita ao HP ultrapassar o nível 100 de vida total 
  -- ou que a vida termine em níveis negativos:
  vidaJogador = math.clamp(vidaJogador - danoSofido, 0, 100)
  ```


## Outras Funções do Módulo

### `math.lerp()` (ou `:lerp()`)
**Assinatura:**
```lua
math.lerp(a, b, t)
```

Interpola linearmente entre dois valores baseado em um peso t (0 a 1).

### `math.sign()` (ou `:sign()`)
**Assinatura:**
```lua
math.sign(val)
```

Retorna o sinal do valor fornecido (1, -1 ou 0).

### `math.dist2()` (ou `:dist2()`)
**Assinatura:**
```lua
math.dist2(x1, y1, x2, y2)
```

Retorna a distância quadrada (evita a raiz quadrada, sendo mais eficiente) entre dois pontos.

### `math.distanceSquared()` (ou `:distanceSquared()`)
**Assinatura:**
```lua
math.distanceSquared(x1, y1, x2, y2)
```

Retorna a distância quadrada entre dois pontos.

### `math.angle()` (ou `:angle()`)
**Assinatura:**
```lua
math.angle(x1, y1, x2, y2)
```

Retorna o ângulo (em radianos) entre dois pontos.

### `math.normalize()` (ou `:normalize()`)
**Assinatura:**
```lua
math.normalize(x, y)
```

Retorna o vetor (normalizado) com comprimento 1.

### `math.dot()` (ou `:dot()`)
**Assinatura:**
```lua
math.dot(x1, y1, x2, y2)
```

Calcula o produto escalar (dot product) entre dois vetores.

### `math.cross()` (ou `:cross()`)
**Assinatura:**
```lua
math.cross(x1, y1, x2, y2)
```

Calcula o produto vetorial (cross product) entre dois vetores 2D.

### `math.direction()` (ou `:direction()`)
**Assinatura:**
```lua
math.direction(angle, length)
```

Retorna um vetor (x, y) apontando para a direção do ângulo fornecido.

### `math.round()` (ou `:round()`)
**Assinatura:**
```lua
math.round(val)
```

Arredonda o valor para o número inteiro mais próximo.

### `math.fract()` (ou `:fract()`)
**Assinatura:**
```lua
math.fract(val)
```

Retorna a parte fracionária de um número.

### `math.lerpAngle()` (ou `:lerpAngle()`)
**Assinatura:**
```lua
math.lerpAngle(a, b, t)
```

Interpola linearmente entre dois valores baseado em um peso t (0 a 1).

### `math.smoothstep()` (ou `:smoothstep()`)
**Assinatura:**
```lua
math.smoothstep(min, max, val)
```

Interpolação no estilo curva em S suave, comum em atenuação.

### `math.remap()` (ou `:remap()`)
**Assinatura:**
```lua
math.remap(val, in_min, in_max, out_min, out_max)
```

Mapeia um valor de uma faixa (in_min, in_max) para outra faixa (out_min, out_max).

### `math.map()` (ou `:map()`)
**Assinatura:**
```lua
math.map(val, in_min, in_max, out_min, out_max)
```

Mapeia um valor de um intervalo para outro de forma linear.

### `math.pingpong()` (ou `:pingpong()`)
**Assinatura:**
```lua
math.pingpong(val, max)
```

Retorna um valor que "quica" para frente e para trás entre 0 e um valor máximo.

### `math.wrap()` (ou `:wrap()`)
**Assinatura:**
```lua
math.wrap(val, min, max)
```

Envolve (loop) o valor mantendo-o estritamente entre o mínimo e o máximo fornecidos.

### `math.snap()` (ou `:snap()`)
**Assinatura:**
```lua
math.snap(val, step)
```

Arredonda/Alinha o valor ao múltiplo mais próximo do "step" fornecido.

### `math.approach()` (ou `:approach()`)
**Assinatura:**
```lua
math.approach(val, target, max_delta)
```

Muda o valor gradualmente em direção a um alvo com velocidade constante (max_delta).

### `math.moveTowards()` (ou `:moveTowards()`)
**Assinatura:**
```lua
math.moveTowards(val, target, max_delta)
```

Muda um valor ou vetor em direção ao alvo, não passando dele.

### `math.length()` (ou `:length()`)
**Assinatura:**
```lua
math.length(x, y)
```

Calcula o comprimento (magnitude) de um vetor.

### `math.packFloatArray()` (ou `:packFloatArray()`)
**Assinatura:**
```lua
math.packFloatArray(table)
```

Empacota uma tabela do lua contendo floats em um array nativo C para envio à GPU ou otimizações.

### `math.packUShortArray()` (ou `:packUShortArray()`)
**Assinatura:**
```lua
math.packUShortArray(table)
```

Empacota uma tabela contendo ints 16bits (unsigned short) para envio à GPU.

### `math.packUIntArray()` (ou `:packUIntArray()`)
**Assinatura:**
```lua
math.packUIntArray(table)
```

Empacota uma tabela contendo ints 32bits sem sinal.

### `math.triangulate()` (ou `:triangulate()`)
**Assinatura:**
```lua
math.triangulate(polygon_points)
```

Transforma um polígono genérico (uma lista de pontos) em uma lista de triângulos rendenderizáveis.

### `math.identity()` (ou `:identity()`)
**Assinatura:**
```lua
mat:identity()
```

Redefine a matriz/transformação para a identidade (sem deformação/posição [0,0]).

### `math.translate()` (ou `:translate()`)
**Assinatura:**
```lua
mat:translate(tx, ty, tz)
```

Desloca a posição pela matriz ou gráfico atual.

### `math.scale()` (ou `:scale()`)
**Assinatura:**
```lua
mat:scale(sx, sy, sz)
```

Aumenta ou diminui a escala pela matriz, vetor ou na tela.

### `math.rotateX()` (ou `:rotateX()`)
**Assinatura:**
```lua
mat:rotateX(angle)
```

Rotaciona a matriz ao redor do eixo X.

### `math.rotateY()` (ou `:rotateY()`)
**Assinatura:**
```lua
mat:rotateY(angle)
```

Rotaciona a matriz ao redor do eixo Y.

### `math.rotateZ()` (ou `:rotateZ()`)
**Assinatura:**
```lua
mat:rotateZ(angle)
```

Rotaciona a matriz ao redor do eixo Z.

### `math.transformPoint()` (ou `:transformPoint()`)
**Assinatura:**
```lua
mat:transformPoint(x, y, z)
```

Transforma a coordenada x,y,z com a matriz local ou transformador, retornando os novos valores.

### `math.ortho()` (ou `:ortho()`)
**Assinatura:**
```lua
mat:ortho(left, right, bottom, top, near, far)
```

Instaura a visão da matriz ortográfica 2D baseada nos parâmetros left, right, bottom, etc.

### `math.perspective()` (ou `:perspective()`)
**Assinatura:**
```lua
mat:perspective(fovY, aspect, near, far)
```

Transforma a matriz usando os dados de projeção perspectiva 3D.

### `math.mat4()` (ou `:mat4()`)
**Assinatura:**
```lua
math.mat4()
```

Cria um novo objeto Matriz 4x4 (Math.mat4).

### `math.apply()` (ou `:apply()`)
**Assinatura:**
```lua
transform:apply(transform2)
```

Aplica os valores ou transformações de um objeto no objeto atual.

### `math.clone()` (ou `:clone()`)
**Assinatura:**
```lua
transform:clone()
```

Retorna uma cópia exata do objeto atual.

### `math.inverse()` (ou `:inverse()`)
**Assinatura:**
```lua
transform:inverse()
```

Inverte a matriz / transform e altera seu valor ou retorna dados inversos.

### `math.getMatrix()` (ou `:getMatrix()`)
**Assinatura:**
```lua
transform:getMatrix()
```

Retorna os valores empacotados da matriz interna deste objeto transformador.

### `math.setMatrix()` (ou `:setMatrix()`)
**Assinatura:**
```lua
transform:setMatrix(m11, m12, ...)
```

Sobrescreve os dados numéricos internos dessa transformação/matriz com valores especificados.

### `math.setTransformation()` (ou `:setTransformation()`)
**Assinatura:**
```lua
transform:setTransformation(x, y, angle, sx, sy, ox, oy, kx, ky)
```

Configura múltiplas informações (posições X, Y, escalonamento, pinos de rotação etc) de uma vez na Transform ou Matriz.

### `math.rotate()` (ou `:rotate()`)
**Assinatura:**
```lua
transform:rotate(angle)
```

Rotaciona o objeto, matriz ou canvas num determinado ângulo (em radianos).

### `math.shear()` (ou `:shear()`)
**Assinatura:**
```lua
transform:shear(kx, ky)
```

Aplica inclinação (shear/skew) na Transform atual em X e Y.

### `math.reset()` (ou `:reset()`)
**Assinatura:**
```lua
transform:reset()
```

Reseta contadores ou variáveis essenciais internas do objeto alvo devolvendo ao estado natural.

### `math.inverseTransformPoint()` (ou `:inverseTransformPoint()`)
**Assinatura:**
```lua
transform:inverseTransformPoint(x, y)
```

Inverte a matriz / transform e altera seu valor ou retorna dados inversos.

### `math.isAffine2D()` (ou `:isAffine2D()`)
**Assinatura:**
```lua
transform:isAffine2D()
```

Checa internamente e diz verdadeiro ou falso (boolean) se a matriz em questão funciona validamente via Affine Transformations puras no espaço 2D.

### `math.newTransform()` (ou `:newTransform()`)
**Assinatura:**
```lua
math.newTransform()
```

Devolve o novo Transform(). É uma alternativa moderna contendo métodos para criar árvores hierárquicas e deformações espaciais eficientes que substituem matrizes brutas e funções manuais.



## Outros Métodos Ocultos

### `math.lerp()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `lerp`.
* **Como usar:** `math:lerp()`


### `math.sign()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `sign`.
* **Como usar:** `math:sign()`


### `math.dist2()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `dist2`.
* **Como usar:** `math:dist2()`


### `math.distanceSquared()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `distanceSquared`.
* **Como usar:** `math:distanceSquared()`


### `math.angle()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `angle`.
* **Como usar:** `math:angle()`


### `math.cross()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `cross`.
* **Como usar:** `math:cross()`


### `math.direction()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `direction`.
* **Como usar:** `math:direction()`


### `math.round()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `round`.
* **Como usar:** `math:round()`


### `math.fract()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `fract`.
* **Como usar:** `math:fract()`


### `math.lerpAngle()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `lerpAngle`.
* **Como usar:** `math:lerpAngle()`


### `math.smoothstep()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `smoothstep`.
* **Como usar:** `math:smoothstep()`


### `math.remap()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `remap`.
* **Como usar:** `math:remap()`


### `math.map()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `map`.
* **Como usar:** `math:map()`


### `math.pingpong()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `pingpong`.
* **Como usar:** `math:pingpong()`


### `math.wrap()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `wrap`.
* **Como usar:** `math:wrap()`


### `math.snap()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `snap`.
* **Como usar:** `math:snap()`


### `math.approach()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `approach`.
* **Como usar:** `math:approach()`


### `math.moveTowards()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `moveTowards`.
* **Como usar:** `math:moveTowards()`


### `math.triangulate()`

* **O que isso faz?** Função exposta via API C++ do módulo `math` referenciando: `triangulate`.
* **Como usar:** `math:triangulate()`
