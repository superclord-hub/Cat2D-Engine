# Physics (Física Base)

A Cat Engine foi construída sobre o poderoso Chipmunk2D (Simulador Rígido altamente profissional). Em motores como este, nós não usamos classes genéricas que fazem tudo. O Mundo se separa em **Espaços**, **Corpos Médicos** (peso/rotação), e **Shapes (Formas)** que são as carnes reais que colidem.

---

## 1. O Espaço (Space)
Toda simulação corre dentro de um espaço (um contêiner de mundo).
### `physics.newSpace()`
* **O que isso faz?** Cria um universo isolado.
* **Retorno:** `Space`
* **Exemplo de uso:**
  ```lua
  meu_mundo = physics.newSpace()
  meu_mundo:setGravity(0, 900) -- Gravidade puxando para baixo
  ```

### `space:step(dt)`
* **O que isso faz?** Manda a simulação "andar no tempo" processando a física de todos. Deve ser chamada no seu Loop de atualização contínuo.
  ```lua
  meu_mundo:step(system.getDeltaTime())
  ```

---

## 2. Massas & Corpos (Bodies)
Um corpo (Body) é como uma "Aura" ou um ponto no espaço que tem massa, inércia (momento) e carrega gravidade, mas **NÃO TEM FORMA AINDA**, logo, ele não colide sem um Shape (Forma) afixado.

### `physics.newBody(mass, moment)`
* Corpo Dinâmico (Sofre atuação natural de forças, peso e pancada).
* `moment`: A inércia de giro (resistência a ser rotacionado). Pode ser calculado com `physics.momentForBox(mass, w, h)`.
### `physics.newStaticBody()`
* Corpos rígidos, peso infinito, imóveis! Ideais pra chão e paredes.
### `physics.newKinematicBody()`
* Corpos "Fantasmas" e infinitos porem movem se você setar sua velocidade manualmente (Plataformas Moveis de Super Mario voadoras).

---

## 3. Formas de Colisão (Shapes)
As formas dão "Carne" ao seu esqueleto (Corpo) para poder esbarrar no mundo!

### `physics.newBoxShape(body, width, height, radius)`
### `physics.newCircleShape(body, radius, offset_x, offset_y)`
* Instancia uma geometria atrelada a uma Espinha Dorsal Mestra (O Corpo).

---

## 4. Conectando tudo para o Espaço agir!
Para a física acontecer, os Corpos **E** os Shapes precisam viver dentro do **Space**.

```lua
-- Exemplo de Criação de uma Caixa Caindo Clássica
meu_mundo = physics.newSpace()
meu_mundo:setGravity(0, 900)

-- Corpo dinâmico de Massa 1.0
local massa = 1.0
local momento = physics.momentForBox(massa, 50, 50)
meu_corpo = physics.newBody(massa, momento)
meu_corpo:setPosition(100, -100)

-- Carne quadrada cobrindo esse corpo
minha_forma = physics.newBoxShape(meu_corpo, 50, 50, 0.0)
minha_forma:setFriction(0.7)

-- ENTREGANDO AO MUNDO PRA SIMULAR AGORA:
meu_mundo:addBody(meu_corpo)
meu_mundo:addShape(minha_forma)
```

**Importante:** Nunca adicione os Objetos Estáticos `StaticBody` no Mundo pela função `addBody`, pois o mundo ignora gravidades nele já na raiz natural. Estáticos e Cinemáticos são processados de fora à parte em Chipmunk2D! Para eles adicione APENAS a Forma (`addShape`).


## Outras Funções do Módulo

### `physics.__gc()` (ou `:__gc()`)
Função utilitária interna ou embutida que acessa o motor nativo em C++. Fornece manipulação de estado direto da Engine de hardware.

### `physics.__index()` (ou `:__index()`)
Metamétodo interno da tabela Lua usado para conectar e buscar propriedades virtuais diretamente da Classe C++. Fica oculto do uso no dia a dia da API.

### `physics.removeBody()` (ou `:removeBody()`)
**Assinatura:**
```lua
space:removeBody(body)
```

Remove sem destruir um Corpo inserido no contexto daquele espaço e parando as interações no espaço.

### `physics.removeShape()` (ou `:removeShape()`)
**Assinatura:**
```lua
space:removeShape(shape)
```

Suspende colisões mundanas do Shape especificado no step da física.

### `physics.momentForCircle()` (ou `:momentForCircle()`)
**Assinatura:**
```lua
physics.momentForCircle(m, r1, r2, ox, oy)
```

Computa inércia/momento m estático da geometria Circular sólida pra definir corpos de newBody de rodas reais estáveis no Physics.

### `physics.momentForSegment()` (ou `:momentForSegment()`)
**Assinatura:**
```lua
physics.momentForSegment(m, ax, ay, bx, by, r)
```

Gerador de inércia pro chipmunk manter coesos rotações em eixos geométricos de segmentos delgados.

### `physics.momentForPoly()` (ou `:momentForPoly()`)
**Assinatura:**
```lua
physics.momentForPoly(m, verts, r)
```

Vetoriza formula inercial sobre qualquer Poligono convexo passando as vertentes numéricas retornando Moment ideal.

### `physics.getPosition()` (ou `:getPosition()`)
**Assinatura:**
```lua
sound:getPosition()
```

Recebe a posição no tempo, referencial ou localização em instâncias.

### `physics.setAngle()` (ou `:setAngle()`)
**Assinatura:**
```lua
body:setAngle(angle)
```

Aplica angulações limites ou rotação principal no gerador e distribuidor de emissões em partícular.

### `physics.getAngle()` (ou `:getAngle()`)
**Assinatura:**
```lua
body:getAngle()
```

Obtém instante da Rotação física e base rad do objeto no chipmunk e transforms das entidades e componentes em ECS / math.

### `physics.setVelocity()` (ou `:setVelocity()`)
**Assinatura:**
```lua
sound:setVelocity(x, y, z)
```

Gera inércia aplicando vetores de velocidades em corpo dinâmico na X/Y.

### `physics.getVelocity()` (ou `:getVelocity()`)
**Assinatura:**
```lua
body:getVelocity()
```

X e Y numéricas e ativas de pulso ou força resultante dos forces das dinâmicas impulsionada agora do centro geométrico.

### `physics.setAngularVelocity()` (ou `:setAngularVelocity()`)
**Assinatura:**
```lua
body:setAngularVelocity(w)
```

Desanexa um corpo físico e rígido permanentemente do espaço e de suas colisões ativas. Lembre-se, use isso quando um objeto for destruído/morto no seu jogo.

### `physics.getAngularVelocity()` (ou `:getAngularVelocity()`)
**Assinatura:**
```lua
body:getAngularVelocity()
```

Busca as grandezas de velocidades giro/rad actuantes na dinâmica interna giratória gerando inércias rebatendo objetos tangenciais na Engine 2D.

### `physics.setMass()` (ou `:setMass()`)
**Assinatura:**
```lua
body:setMass(mass)
```

Retorna em valores de ponto flutuante o vetor ou velocidade de inércia atual/angular do objeto que está girando ou se movimentando pela engine física plana.

### `physics.getMass()` (ou `:getMass()`)
**Assinatura:**
```lua
body:getMass()
```

Checa qual proporção kgs (float) internal do corpo ativo atrelado.

### `physics.setMoment()` (ou `:setMoment()`)
**Assinatura:**
```lua
body:setMoment(moment)
```

Devolve o cálculo de massa bruta do objeto que interfere nas transferências de colisões densas.

### `physics.getMoment()` (ou `:getMoment()`)
**Assinatura:**
```lua
body:getMoment()
```

Grau do Momento de Inércia usado em motores 2D para decidir a estabilidade da velocidade rotacional e de pêndulo.

### `physics.applyForce()` (ou `:applyForce()`)
**Assinatura:**
```lua
body:applyForce(fx, fy)
```

Aplica os valores ou transformações de um objeto no objeto atual.

### `physics.applyImpulse()` (ou `:applyImpulse()`)
**Assinatura:**
```lua
body:applyImpulse(ix, iy)
```

Aplica os valores ou transformações de um objeto no objeto atual.

### `physics.setTorque()` (ou `:setTorque()`)
**Assinatura:**
```lua
body:setTorque(torque)
```

Aplica uma força física impulsionadora bruta imediata no sistema, empurrando o objeto vetorialmente. Ótimo para impulsos de pulos rígidos.

### `physics.setElasticity()` (ou `:setElasticity()`)
**Assinatura:**
```lua
shape:setElasticity(elasticity)
```

Define a elasticidade (rebatimento ou bounce) da forma física (Ex: 1.0 quica infinitamente mantendo a força, 0.0 não quica absorvendo a queda).

### `physics.setSensor()` (ou `:setSensor()`)
**Assinatura:**
```lua
shape:setSensor(isSensor)
```

Define se essa física deve apenas disparar EVENTOS DE TOQUE mas não agir como parede rígida. (Ex: True = Pode atravessar e captar a colisão, como Moedas, False = Parede).

### `physics.setCollisionType()` (ou `:setCollisionType()`)
**Assinatura:**
```lua
shape:setCollisionType(typeId)
```

Dita a ID na tabela de identificação dessa camada ou tipo de choque físico. Ajudando a motor engine saber se Fogo (ID 1) bate na Água (ID 2).

### `physics.setDensity()` (ou `:setDensity()`)
**Assinatura:**
```lua
shape:setDensity(density)
```

Calcula matematicamente a inércia, massa versus a forma geométrica configurada na área da engine (calculo automático para Body).

### `physics.getDensity()` (ou `:getDensity()`)
**Assinatura:**
```lua
shape:getDensity()
```

Muda a densidade ou rigidez local que forma o objeto. Maior densidade exige maior força gravitacional externa para locomovê-la.

### `physics.setFilter()` (ou `:setFilter()`)
**Assinatura:**
```lua
texture:setFilter(min, mag)
```

Informa filtros para desenhar texturas/físicas ou aplicar máscaras de luz sobre a colisão.

### `physics.newSegmentShape()` (ou `:newSegmentShape()`)
**Assinatura:**
```lua
physics.newSegmentShape(body, ax, ay, bx, by, radius)
```

Constrói instâncias matemáticas de polígonos ou linhas densas em Box2D e Chipmunk baseando-se num Corpo para testar colisões.

### `physics.newPolyShape()` (ou `:newPolyShape()`)
**Assinatura:**
```lua
physics.newPolyShape(body, vertices, radius)
```

Constrói instâncias matemáticas de polígonos ou linhas densas em Box2D e Chipmunk baseando-se num Corpo para testar colisões.



## Outros Métodos Ocultos (Auto-Documentados)

Abaixo mapeamos automaticamente novas funções da API C++ que haviam ficado sem menção detalhada.

### `physics.setMaxForce()` (ou `:setMaxForce()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `setMaxForce`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:setMaxForce()
  ```

### `physics.setErrorBias()` (ou `:setErrorBias()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `setErrorBias`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:setErrorBias()
  ```

### `physics.setMaxBias()` (ou `:setMaxBias()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `setMaxBias`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:setMaxBias()
  ```

### `physics.setCollideBodies()` (ou `:setCollideBodies()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `setCollideBodies`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:setCollideBodies()
  ```

### `physics.newPivotJoint()` (ou `:newPivotJoint()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `newPivotJoint`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:newPivotJoint()
  ```

### `physics.newSlideJoint()` (ou `:newSlideJoint()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `newSlideJoint`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:newSlideJoint()
  ```

### `physics.newGrooveJoint()` (ou `:newGrooveJoint()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `newGrooveJoint`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:newGrooveJoint()
  ```

### `physics.newDampedRotarySpring()` (ou `:newDampedRotarySpring()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `newDampedRotarySpring`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:newDampedRotarySpring()
  ```

### `physics.newRotaryLimitJoint()` (ou `:newRotaryLimitJoint()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `newRotaryLimitJoint`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:newRotaryLimitJoint()
  ```

### `physics.newRatchetJoint()` (ou `:newRatchetJoint()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `newRatchetJoint`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:newRatchetJoint()
  ```

### `physics.newGearJoint()` (ou `:newGearJoint()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `newGearJoint`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:newGearJoint()
  ```

### `physics.addConstraint()` (ou `:addConstraint()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `addConstraint`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:addConstraint()
  ```

### `physics.removeConstraint()` (ou `:removeConstraint()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `removeConstraint`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:removeConstraint()
  ```

### `physics.raycast()` (ou `:raycast()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `raycast`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:raycast()
  ```

### `physics.pointQueryNearest()` (ou `:pointQueryNearest()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `pointQueryNearest`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:pointQueryNearest()
  ```

### `physics.shapeQuery()` (ou `:shapeQuery()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `shapeQuery`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:shapeQuery()
  ```

### `physics.getInRadius()` (ou `:getInRadius()`)

* **O que isso faz?** Função específica da engine principal (`physics`) para interagir com o parâmetro/estado `getInRadius`.
* **Como utilizar:**
  ```lua
  -- Exemplo básico de assinatura
  physics:getInRadius()
  ```


## Outros Métodos Ocultos

### `physics.addBody()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `addBody`.
* **Como usar:** `physics:addBody()`


### `physics.addShape()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `addShape`.
* **Como usar:** `physics:addShape()`


### `physics.removeBody()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `removeBody`.
* **Como usar:** `physics:removeBody()`


### `physics.removeShape()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `removeShape`.
* **Como usar:** `physics:removeShape()`


### `physics.momentForCircle()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `momentForCircle`.
* **Como usar:** `physics:momentForCircle()`


### `physics.momentForSegment()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `momentForSegment`.
* **Como usar:** `physics:momentForSegment()`


### `physics.momentForPoly()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `momentForPoly`.
* **Como usar:** `physics:momentForPoly()`


### `physics.setAngularVelocity()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `setAngularVelocity`.
* **Como usar:** `physics:setAngularVelocity()`


### `physics.setMass()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `setMass`.
* **Como usar:** `physics:setMass()`


### `physics.setMoment()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `setMoment`.
* **Como usar:** `physics:setMoment()`


### `physics.applyForce()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `applyForce`.
* **Como usar:** `physics:applyForce()`


### `physics.applyImpulse()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `applyImpulse`.
* **Como usar:** `physics:applyImpulse()`


### `physics.setTorque()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `setTorque`.
* **Como usar:** `physics:setTorque()`


### `physics.setCollisionType()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `setCollisionType`.
* **Como usar:** `physics:setCollisionType()`


### `physics.setDensity()`

* **O que isso faz?** Função exposta via API C++ do módulo `physics` referenciando: `setDensity`.
* **Como usar:** `physics:setDensity()`
