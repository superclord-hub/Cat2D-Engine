# Physics Avançado (Joints, Shapes, Raycasts)

Sistemas da mecânica universal se dividem em **Corpos** (Massa e Eixo - A Espinha) e **Geometrias / Shapes** (A carne que colide e atrita).

---

## Estrutura Física: Corpos Rígidos (Bodies)
* **`physics.newBody(mass, moment)`**: Se movimenta e interage inteiramente influenciado. O 'moment' é a resistência rotacional, calcule com `physics.momentFor...`!
* **`physics.newStaticBody()`**: Congelado no espaço, não se move, afeta os dinâmicos como chão e paredes inamovíveis.
* **`physics.newKinematicBody()`**: Pode se movimentar se mandarmos pela programação via `body:setVelocity(vx, vy)`, não tem gravidade e empurra os dinâmicos como as clássicas plataformas móveis do Mário.

## Formas de Colisão (Shapes)
As formas ditam AONDE ocorre as colisões de rebatida pra física agir. Você deve atrelar aos Corpos listados acima.
### `physics.newBoxShape(body, width, height, radius)`
* Declara um retângulo colisor afixado ao corpo referenciado. `radius` arredonda as pontas!

### `physics.newCircleShape(body, radius, offset_x, offset_y)`
* Declara uma esfera colisor circular afixada. `offset` desloca ele do centro original do corpo.

### `physics.newSegmentShape(body, ax, ay, bx, by, thick)` e `physics.newPolyShape(body, vertices, radius)`
* Criação livre para linhas puras esmagadoras e polígonos triangulares arbitrários!

### Opções Interativas da Carne (Shapes)
Com o colisor feito, decida os materiais do projeto pra simular vida!
* **`shape:setFriction(0.5)`**: Como escorregadio é isso? Gelo é `0.1`, Borracha `1.0`.
* **`shape:setElasticity(1.0)`**: Fator KIKAR ou Rebater estilo Bola de Tênis.
* **`shape:setSensor(true)`**: Transforma em Alma / Sensor. Corpos passarão por ele sendo avisados porém NÃO rebatidos por força natural. Ótimo pra criar ZONAS DE PERIGO ou Colecionáveis onde atravessamos!

---

## Lançamento de Raios Laser e Consultas Espaciais (Raycasts & Queries)
* **O que isso faz?** Excelente forma de ver se "Estou vendo o player?" traçando uma linha cega direta de A a B. Lógicas clássicas para Linhas de Visão de Tiro ou I.A. 

### `physics.raycastFirst(x1, y1, x2, y2, radius, filter)`
Busca e retorna o PRIMEIRO corpo encostado que o "Laser" colidiu.
  ```lua
  local hit = physics.raycastFirst(xdoInimigo, ydoInimigo, xdoPlayer, ydoPlayer, 1.0, nil)
  if hit then 
      system.log("Visão obstruida pela forma com ID x/y: " .. hit.point_x)
  end
  ```

### `physics.pointQuery(x, y, maxDistance, filter)` e `physics.bbQuery(left, bottom, right, top...)`
Enquanto o `raycast` testa LINHAS, o `pointQuery` pega quem tá tocando ou no raio próximo de um "Ponto" puro na tela (Ex: Qual corpo o mouse tocou?), e o `bbQuery` levanta CADA CORPO inserido dentro de um retângulo/BoundingBox (Ótimo pra bombas que quebram tudo na área).

---

## Articulações e Restrições (Constraints & Joints)
Amarrações ou Cordas entre dois corpos diferentes (Ex: Pneus atrelados ao Carro).
* **`physics.newPinJoint(bodyA, bodyB)`**: Une como se pregasse com alfinete!
* **`physics.newDampedSpring(...)`**: Simulação completa e divertida de efeito MOLEJO (Molões) que atraí e retrai com as físicas naturais de pulo das molas de amortecedores automotivos.
* **`physics.newSimpleMotor(bodyA, bodyB, rate)`**: Roda agressivamente um lado causando o movimento forçado das turbinas.

**Aviso de Otimização das Articulações:** Se adicionar muitas pernas soltas como "Ragdoll", sempre adicione um limite nas quebras de ligações ou elas contorceram seus corpos voando aos ares e causando erros no simulador devido ao Overcharge de cálculos.
