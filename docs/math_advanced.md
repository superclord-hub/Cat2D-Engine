# Math Avançado (Vetores, Matrizes e Ruídos)

Para aprofundar as manipulações 2D, cat engine provê ferramentas ricas para lógicas procedurais (Random) e cálculos espaciais diretos (Vetores/Matrizes).

---

## Números Aleatórios (Random & Noise)

### `math.random(min, max)` / `math.randomNormal(stddev, mean)` / `math.setRandomSeed(seed)`
* **O que isso faz?** Fornece algorítmos pra selecionar números dentro de margens. O Normal gera randomização através da curva e distribuição "Sino" perfeita (Guassiana).

### `math.noise(x, y, z)` e `math.fbm(x, y, ...)`
* **O que isso faz?** Geração de ruído de grade "Perlin / Simplex Noise" que resulta numa "aleatoriedade orgânica". Utilizadíssimo na criação procedural criativa dos mundos como planícies/terrenos do Minecraft.
* **Exemplo:**
  ```lua
  local alturaDoTerreno = math.noise(pos_x * 0.1, 0, 0) * 100
  ```

### Extras de Sorte & Random (`uuid`, `chance`, `randomVector`)
* `math.chance(probability)`: Joga o Dado de forma fácil, passando valor como `0.25` ele retornará booleano `true` informando se você acertou esse número na mosca nos 25% de taxa de acerto. Bom para drops / loots!
* `math.uuid()`: Cansado de tentar criar identidades pra salvar instâncias sem sobrescrever e bugar chaves globais? Chama esse método pra ele te vomitar uma string Hexadecimal complexa garantida como nunca duplicada na História do Universo, servindo pra atrelar id em Multiplayer ou Componentes.
* `math.randomVector()`: Devolve `x, y` normalizados de -1 a 1 de forma esférica. Ótimo pra espalhar coisas como uma bacia estourando aleatoriamente num círculo perfeito.

---

## Manipulação Randômica (Tabela)

Apesar de existirem globalmente para `table`, estas funções usam matemática de semente aleatória.

### `table.shuffle(tbl)` e `table.getRandom(tbl)`
* `shuffle`: Mistura aleatoriamente as posições de itens dentro de uma array/tabela. Excelente pra fazer cartas de baralho.
* `getRandom`: Sorteia e retorna um valor que exista dentro daquela matriz.

---

## Estruturas Auxiliares

### `math.randomWeighted(table)` e `math.uuid()`
* `randomWeighted`: Sorteia itens usando um peso como chance (Ex: `math.randomWeighted({0.1, 0.8, 0.1})` retornará `2` em 80% das vezes!).
* `uuid`: Retorna um identificador único rápido para bancos de dados ou instâncias de internet `"550e8400-e29b..."`.

### `math.clamp`, `math.lerp`, `math.pingpong`, `math.approach`
* Ferramentas matemáticas utilitárias que interpolam (`lerp`), limitam (`clamp`), rebotam (`pingpong`) ou chegam a um alvo controladamente (`approach`). Execução extrema em C++.

---

## Transformações e Matrizes (Matrix 4x4)

### `math.mat4()` / `math.newTransform()`
* **O que isso faz?** Declara planilhas Matriciais pra usos brutos avançados no pipeline de Câmera da cena ou intercâmbio pesado em GLSL multiplicando os vértices.
* **Modificadores do Transform:**
  * **`mat:translate(x, y)`** (Desloca para a posição)
  * **`mat:scale(sx, sy)`** (Alarga a matriz de pontos)
  * **`mat:rotateX/Y/Z(angle)`** (Rotaciona tridimensionalmente ou no eixo alvo!)
  * **`mat:transformPoint(x, y)`** (Multiplica e devolve qual será o resultado dessa matriz num ponto final do plano virtual).
  * **`mat:ortho()`** / **`mat:perspective()`** (Visões focadas de Câmeras puras pra 2D e 3D).


## Outras Funções do Módulo

### `math_ext.sign()` (ou `:sign()`)
Retorna o sinal do valor fornecido (1, -1 ou 0).

### `math_ext.distance()` (ou `:distance()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `math_ext.dist2()` (ou `:dist2()`)
Retorna a distância quadrada (evita a raiz quadrada, sendo mais eficiente) entre dois pontos.

### `math_ext.distanceSquared()` (ou `:distanceSquared()`)
Retorna a distância quadrada entre dois pontos.

### `math_ext.normalize()` (ou `:normalize()`)
Retorna o vetor (normalizado) com comprimento 1.

### `math_ext.dot()` (ou `:dot()`)
Calcula o produto escalar (dot product) entre dois vetores.

### `math_ext.cross()` (ou `:cross()`)
Calcula o produto vetorial (cross product) entre dois vetores 2D.

### `math_ext.direction()` (ou `:direction()`)
Retorna um vetor (x, y) apontando para a direção do ângulo fornecido.

### `math_ext.round()` (ou `:round()`)
Arredonda o valor para o número inteiro mais próximo.

### `math_ext.fract()` (ou `:fract()`)
Retorna a parte fracionária de um número.

### `math_ext.lerpAngle()` (ou `:lerpAngle()`)
Interpola linearmente entre dois valores baseado em um peso t (0 a 1).

### `math_ext.smoothstep()` (ou `:smoothstep()`)
Interpolação no estilo curva em S suave, comum em atenuação.

### `math_ext.remap()` (ou `:remap()`)
Mapeia um valor de uma faixa (in_min, in_max) para outra faixa (out_min, out_max).

### `math_ext.map()` (ou `:map()`)
Mapeia um valor de um intervalo para outro de forma linear.

### `math_ext.wrap()` (ou `:wrap()`)
Envolve (loop) o valor mantendo-o estritamente entre o mínimo e o máximo fornecidos.

### `math_ext.snap()` (ou `:snap()`)
Arredonda/Alinha o valor ao múltiplo mais próximo do "step" fornecido.

### `math_ext.moveTowards()` (ou `:moveTowards()`)
Muda um valor ou vetor em direção ao alvo, não passando dele.

### `math_ext.length()` (ou `:length()`)
Calcula o comprimento (magnitude) de um vetor.

### `math_ext.packFloatArray()` (ou `:packFloatArray()`)
Empacota uma tabela do lua contendo floats em um array nativo C para envio à GPU ou otimizações.

### `math_ext.packUShortArray()` (ou `:packUShortArray()`)
Empacota uma tabela contendo ints 16bits (unsigned short) para envio à GPU.

### `math_ext.packUIntArray()` (ou `:packUIntArray()`)
Empacota uma tabela contendo ints 32bits sem sinal.

### `math_ext.triangulate()` (ou `:triangulate()`)
Transforma um polígono genérico (uma lista de pontos) em uma lista de triângulos rendenderizáveis.

