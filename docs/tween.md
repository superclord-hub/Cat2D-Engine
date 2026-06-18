# Tween (Animações Interpoladas)

O módulo de `tween` implementa cálculos de interpolação matemática em propriedades de Tabelas Lua. Basicamente, transita suavemente os valores ao invés do clássico "teletransporte". Sendo extremamente requisitado para criar botões de UI com saltos, aberturas de portas em mapas e animações cinemáticas.

---

## Modos de Uso

### `tween.to(obj, props, duration, easeName)`
* Construtor disparador. Injeta o modificador e os cálculos diretos no motor Tween ativo internamente. Não pausa a thread atual (diferente de `thread.tween`).
* **obj:** A Tabela alvo a ser animada. Ex: `local porta = {x = 0, y = 0}`.
* **props:** Outra tabela contendo a nova numeração pra onde as propriedades antigas irão.
* **duration:** Em Segundos inteiros de float.
* **easeName:** Uma string que define a Suavização Linear para dar sensação de inércia da física.
  * _Opções suportadas: "linear", "quadOut", "quadIn", "quadInOut", "cubicIn", "cubicOut", "cubicInOut", "bounceOut" e "elasticOut"._
* **Exemplo:**
  ```lua
  local meu_quadrado = {x = 10, y = 10, size = 50}
  
  -- Ao chamar isso, ele vai mover o quadrado para X=500 num esticão elástico
  -- durante 2.0 Segundos!
  tween.to(meu_quadrado, {x = 500, size = 150}, 2.0, "elasticOut")
  ```

---

## Lógica Interna

### `tween.update(dt)`
Para os valores realmente saírem da fase inicial e progredirem os floats do alvo, você **deve** atualizar a mesa global do módulo com esta chamada mandando o tempo dos frames dentro do ciclo de seu jogo:

**Exemplo Master:**
```lua
function update(dt)
    tween.update(dt) 
end

function draw()
    graphics.drawRect(meu_quadrado.x, meu_quadrado.y, meu_quadrado.size, meu_quadrado.size, 1, 0, 0, 1)
end
```
> Obs: Como as tabelas em Lua viajam sob referências puras da RAM, você não capta o retorno, a biblioteca se encarregará de mudar os itens dinamicamente na raiz da sua tabela fornecida! Mágica limpa.


## Outros Métodos Ocultos (Auto-Documentados)

Abaixo mapeamos automaticamente novas funções da API C++ que haviam ficado sem menção detalhada.

### `tween.linear()` (ou `:linear()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `linear`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:linear()
  ```

### `tween.easeInQuad()` (ou `:easeInQuad()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `easeInQuad`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:easeInQuad()
  ```

### `tween.easeOutQuad()` (ou `:easeOutQuad()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `easeOutQuad`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:easeOutQuad()
  ```

### `tween.easeInOutQuad()` (ou `:easeInOutQuad()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `easeInOutQuad`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:easeInOutQuad()
  ```

### `tween.easeInCubic()` (ou `:easeInCubic()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `easeInCubic`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:easeInCubic()
  ```

### `tween.easeOutCubic()` (ou `:easeOutCubic()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `easeOutCubic`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:easeOutCubic()
  ```

### `tween.easeInOutCubic()` (ou `:easeInOutCubic()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `easeInOutCubic`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:easeInOutCubic()
  ```

### `tween.easeInQuart()` (ou `:easeInQuart()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `easeInQuart`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:easeInQuart()
  ```

### `tween.easeOutQuart()` (ou `:easeOutQuart()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `easeOutQuart`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:easeOutQuart()
  ```

### `tween.easeInOutQuart()` (ou `:easeInOutQuart()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `easeInOutQuart`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:easeInOutQuart()
  ```

### `tween.easeOutBounce()` (ou `:easeOutBounce()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `easeOutBounce`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:easeOutBounce()
  ```

### `tween.onComplete()` (ou `:onComplete()`)

* **O que isso faz?** Função específica da engine principal (`tween`) para interagir com o parâmetro/estado `onComplete`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  tween:onComplete()
  ```
