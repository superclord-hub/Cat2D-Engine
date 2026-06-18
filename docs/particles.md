# Particles (Partículas 2D)

O módulo de `Particle` proporciona uma maneira empolgante, simplista e rápida de exibir centenas a milhares de emissões simples de texturas e quadrados em padrões variados, permitindo criar magias, explosões, sangues, neves, e fogos na Cat Engine sem sacrificar muito o frame rate do aparelho através do sistema robusto em C++.

---

## Criando um Emissor Primitivo

### `particles.newEmitter([maxParticles])`
* **O que isso faz?** Declara um objeto instanciando recursos de um emissor isolado que guardará um conjunto fechado da textura ou forma designada com estado variável próprio. Permite um tamanho máximo (`maxParticles`, padrão 1000).
* **Exemplo de Criação:**
  ```lua
  local chuvaForte = particles.newEmitter(500)
  ```

---

## Usando e Comandando

Tratando a Instância via OOP (Programação Orientada a Objetos com Lua `:`):

### `emitter:update(dt)`
Atualize iterativo obrigatorio. Processa a passagem e ciclo de envelhecimento, velocidade direcional da gravidade para cada pequena faírsca gerada e exclui se tempo passou. Deve vir dentro de Update!

### `emitter:draw()`
Processo visual que pega todas as posições dos cálculos anteriores validados como Visíveis ou ativos em hardware renderizado em um único Fast Batch. Sem isso sua configuração será invisível pois falta envio Draw.

### `emitter:emit(count)`
A força o lançamento imediato pra simulações imediatas da quantia escolhida. Ex: Mande logo de cara 15 unidades para gerar impactos fortes.

## Principais Propriedades (Opcionais) de Tunning visual:

Você tem a sua disposição e o conforto para ajustar da forma criativa com os variados estilos de sets:

* **`:setPosition(x, y)`** : Coloca a base central nas posições espaciais do mundo universal da textura espalhada ou lançada pelo alvo (ex: Amarrar emissor atrás de um meteoro ou Pneus em chamas ou do mouse).
* **`:setTexture(texture)`** : Caso deseje partículas via imagem carregada e não vetorial preenchido básico. Efeitos de Nuvens utilizam muita textura de Algodão em `PNG` Alpha.
* **`:setColor(r, g, b, a, end_r, end_g, end_b, end_a)`** : Troca entre Cores Iniciais pra cores Finais durante degradações até encerrarem, baseando-se em números de 0 a 1 em vez de tabelas.
* **`:setSpeed(min, max)`** : Qual rapido as velocidades se dispersaram do meio na explosão.
* **`:setLifespan(minMax, maxD)`**: Decida de maneira aleatória entre mínimos e médios o tempo em formato SEGUNDOS Flutuantes que deveram sumir por idades após instanciar (VIda util).
* **`:setSize(startMin, startMax, endMin, endMax)`** : Agrandar ou Empequenecer, definindo o tamanho aleatório inicial e o final.
* **`:setAdditiveBlend(true)`**: Ativa a mistura aditiva, que faz as partículas brilharem intensamente como fogo ou magia quando sobrepostas!
* **`:setRadialAccel(min, max)`** e **`:setTangentAccel(min, max)`**: Adiciona forças de redemoinho (Tangencial) e atração/repulsão radial para explosões e nebulosas mágicas em curva circular constante!
* **`:getActiveCount()`**: Retorna quantas partículas estão voando ativamente naquele exato frame.

### Controlando Onde Nascem (Shapes)
Por padrão as partículas nascem exatamente de um ponto zero unificado `(:setShapePoint())`.
Mas você pode fazer nascer em espalhamento livre escolhendo áreas de nascedouros:
* **`:setShapeRect(width, height)`**: Faça partículas pingarem randomicamente num retângulo (Ótimo para Neve ou Chuva).
* **`:setShapeCircle(radius)`**: Faça as partículas brotarem num lago mágico (Gerações circulares randomizadas).


## Outras Funções do Módulo

### `particles.__index()` (ou `:__index()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `particles.__gc()` (ou `:__gc()`)
Acionado automaticamente pelo Coletor de Lixo (Garbage Collector) do Lua para destruir e liberar recursos complexos da memória C++. Você não precisa (e nem deve) invocar esse método manualmente.

### `particles.start()` (ou `:start()`)
**Assinatura:**
```lua
emitter:start()
```

Dá o estopim inicial para ativar o mecanismo (seja o gerador de partículas, som, stream e etc).

### `particles.stop()` (ou `:stop()`)
**Assinatura:**
```lua
sound:stop()
```

Para imediatamente a reprodução do áudio, partícula e etc. e volta o tempo pro zero.

### `particles.reset()` (ou `:reset()`)
**Assinatura:**
```lua
transform:reset()
```

Reseta contadores ou variáveis essenciais internas do objeto alvo devolvendo ao estado natural.

### `particles.setEmissionRate()` (ou `:setEmissionRate()`)
**Assinatura:**
```lua
emitter:setEmissionRate(rate)
```

Quantos spawn ou disparos de novos objetos em ciclos contínuos cada pool/emissor efetuará no backend.

### `particles.setAngle()` (ou `:setAngle()`)
**Assinatura:**
```lua
body:setAngle(angle)
```

Aplica angulações limites ou rotação principal no gerador e distribuidor de emissões em partícular.

### `particles.setGravity()` (ou `:setGravity()`)
**Assinatura:**
```lua
space:setGravity(x, y)
```

Determina a tração Y e X geral (vetorial) que o space vai aplicar contra corpos.

### `particles.setRotationSpeed()` (ou `:setRotationSpeed()`)
**Assinatura:**
```lua
emitter:setRotationSpeed(min, max)
```

Controla radianos de curva angular que as emissoras vão interceder durante suas rotinas.

### `particles.setDamping()` (ou `:setDamping()`)
**Assinatura:**
```lua
emitter:setDamping(damping)
```

Encerra e reseta o processador associado retornando ele para o estado inicial neutro, limpando a tarefa.



## Outros Métodos Ocultos

### `particles.setTexture()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setTexture`.
* **Como usar:** `particles:setTexture()`


### `particles.setEmissionRate()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setEmissionRate`.
* **Como usar:** `particles:setEmissionRate()`


### `particles.setLifespan()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setLifespan`.
* **Como usar:** `particles:setLifespan()`


### `particles.setAngle()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setAngle`.
* **Como usar:** `particles:setAngle()`


### `particles.setSize()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setSize`.
* **Como usar:** `particles:setSize()`


### `particles.setRotationSpeed()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setRotationSpeed`.
* **Como usar:** `particles:setRotationSpeed()`


### `particles.setDamping()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setDamping`.
* **Como usar:** `particles:setDamping()`


### `particles.setShapeRect()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setShapeRect`.
* **Como usar:** `particles:setShapeRect()`


### `particles.setShapeCircle()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setShapeCircle`.
* **Como usar:** `particles:setShapeCircle()`


### `particles.setAdditiveBlend()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setAdditiveBlend`.
* **Como usar:** `particles:setAdditiveBlend()`


### `particles.setRadialAccel()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setRadialAccel`.
* **Como usar:** `particles:setRadialAccel()`


### `particles.setTangentAccel()`

* **O que isso faz?** Função exposta via API C++ do módulo `particles` referenciando: `setTangentAccel`.
* **Como usar:** `particles:setTangentAccel()`


### `particles.getActiveCount()`
* **O que isso faz?** Retorna o número de partículas vivas.


### `particles.setShapePoint()`
* **O que isso faz?** Define a área de emissão num único ponto.
