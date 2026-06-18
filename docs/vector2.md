# Vec2 (Vetores Bidimensionais)

A Cat Engine oferece `vec2` como um Userdata nativo programado em C++ de extrema velocidade para abstrair contas vetoriais, movimentações tridimensionais/2d, forças físicas e matrizes usando operadores matemáticos base de Lua sem degradar a performance do Garbage Collector com milhares de lambdas e tabelas pesadas.

---

## Criando um Vetor

### `vec2(x, y)`
* Constrói e retorna um Objeto de C++ Vector2D.
* **Exemplo:** `local posicao = vec2(100.5, 50.0)`

## Operadores Matemáticos Nativos

Você pode usar literais e operadores lógicos naturais matemáticos do Lua entre duas ou mais instâncias de `vec2`.

* **Soma (+):** `local resultado = vet1 + vet2`
* **Subtração (-):** `local direcao = destino - origem`
* **Multiplicação (*):** Escala o vetor. `local forca = direcao * 5` (Se multiplicar dois vetores, multiplicará X por X e Y por Y).
* **Divisão (/):** `local diminuido = vet1 / 2`
* **Negação (-):** Inverte o vetor. `local oposto = -vet1`
* **Igualdade (==):** `if vetA == vetB then` (Verifica exatidão matemática).

---

## Propriedades 

Você pode acessar e editar os eixos a qualquer livre momento de forma leve.

* `meu_vetor.x` - Retorna ou Manipula o Ponto X Float.
* `meu_vetor.y` - Retorna ou Manipula o Ponto Y Float.
* **Exemplo:** `posicao.x = posicao.x + 50`

---

## Métodos Integrados

Para funções complexas adicionais não suportadas por operadores tradicionais, o Userdata acopla funções diretas.

### `vetor:length()`
* Retorna o comprimento/magnitude total (Hipotenusa) de distância até o vértice absoluto.
* **Retorno:** `number` (Float)
* **Exemplo:** `local distancia = forca_vetorial:length()`

### `vetor:normalize()`
* Normaliza o vetor em seu lugar para uma distância exata de "1", mantendo apenas o sentido e direção (Excelente para Joystick Pointers e Tiros de Bullets guiados).

### `vetor:dot(outro_vetor)`
* Calcula o Produto Escalar (Dot Product) entre os dois pontos de Vetor.
* **Retorno:** `number`

---

## Funções Multiplicadoras In-Place (Desempenho Extremo)

Quando se opera com _Operadores de Soma ou Multiplicação (+, -)_ a Engine tem a triste necessidade natural de criar um NOVO Objeto para alocar num novo espaço de memória os resultados. Para você não inflacionar milhares de vetores novos em loops extremos (como desenhar particulas e 3 mil monstros rodando ao mesmo tempo a "60 frames" em um segundo), você DEVE dar preferência aos manipuladores *In-Place* abaixo, que forçam os cálculos ocorrerem sobre a instância nativa existente sem gastar memória alocada nova!

* **`vetor:addInPlace(outro_vetor)`**
* **`vetor:subInPlace(outro_vetor)`**
* **`vetor:mulInPlace(outro_vetor)`/`vetor:mulInPlace(number)`**
* **`vetor:divInPlace(outro_vetor)`/`vetor:divInPlace(number)`**

**Exemplo Prático (A Forma Correta e Rápida):**
```lua
-- No Update (60x ao segundo)
local gravidade = vec2(0, 9.8)

function update(dt)
   -- Bom: O mesmo e antigo vec2 não é recriado, apenas modificado.
   player_velocidade:addInPlace(gravidade)
   player_pos:addInPlace(player_velocidade)
end
```


## Outras Funções do Módulo

### `vec2.__add()` (ou `:__add()`)
Adiciona componentes, quadros ou sprites novas às massas da função ou pools (ex: fast batch).

### `vec2.__sub()` (ou `:__sub()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `vec2.__mul()` (ou `:__mul()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `vec2.__div()` (ou `:__div()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `vec2.__unm()` (ou `:__unm()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `vec2.__eq()` (ou `:__eq()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `vec2.__tostring()` (ou `:__tostring()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `vec2.__index()` (ou `:__index()`)
Metamétodo interno da tabela Lua usado para conectar e buscar propriedades virtuais diretamente da Classe C++. Fica oculto do uso no dia a dia da API.

### `vec2.__newindex()` (ou `:__newindex()`)
Metamétodo interno para atribuição de valores limitados na memória nativa. Impede que lógicas Lua sobrescrevam memória read-only do motor.

