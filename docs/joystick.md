# Joystick (Analógicos Virtuais)

O módulo de `joystick` serve para facilitar a criação de direcionais analógicos virtuais em interfaces de jogos na Cat Engine. Em vez de calcular ângulos de toque e áreas radiais manualmente, este módulo gerenciador lida com toda a simulação física da molenga do controle na tela.

---

## Modos de Uso

### `joystick.create(x, y, radius, knobRadius, bgTex, knobTex)`
* Cria e devolve uma nova instância de Virtual Joystick. O `bgTex` (fundo) e o `knobTex` (botão central) devem ser texturas previamente carregadas em `graphics.loadTexture()`.
* **Retorno:** `JoystickObj`
* **Exemplo:** `local meu_joy = joystick.create(200, 200, 100, 40, base_img, botao_img)`

---

## Propriedades da Instância

Após criado, você deve manipular o seu `JoystickObj` com estas chamadas:

### Modificadores

* `joy:setVisuals(bgTex, knobTex)` - Altera em tempo real as imagens do analógico.
* `joy:setDynamic(enabled, areaX, areaY, areaW, areaH)` 
   * **Joystick Dinâmico**: Se habilitado com sua zona (rect), o analógico só aparece onde o dedo do usuário tocar primeiramente como um controle Flutuante (Usado em jogos Mobile modernos)!
* `joy:setDeadzone(val)` - Distância morta onde o botão não dispara movimento (Padrão: Evita leves tremores de toques não querendo mover o player).
* `joy:setAxis(axis)` - Trava o funcionamento: `both` (Padrão, Livre 360), `horizontal`, ou `vertical`.
* `joy:setSpring(enabled, speed)` - Ao soltar o controle, faz com que o botão central volte com efeito de elástico.

### Lógica (Loop)
Para que o sistema receba os toques em tela (Update) e desenhe seu frame nativo, você **deve** usar estas duas funções nas funções base da cena ou do ECS:
* **`joy:update()`** - Invocar no Loop de Update! Atualiza a posição de movimento e toque na tela do analógico.
* **`joy:draw(r, g, b, a)`** - Invocar no Loop de Draw! Desenha nativamente a interface do joystick com coloração Alpha modular.

### Captadores (Getters de Ação)
Retirando o suco que faz seu jogo andar:

* `joy:getVector()` - Retorna os eixos x,y normalizados de `-1.0` a `1.0` pra somar na velocidade do player.
   * *Exemplo:* `local x, y = joy:getVector() ; player_x = player_x + (x * velocidade)`
* `joy:getDistance()` - Distância do botão até o centro (0 até `radius`).
* `joy:getAngle()` - Retorna o Ângulo do controle em radianos! Muito útil pra definir para onde o tiro da arma irá voar.
* `joy:isActive()` - Booleano indicando se o player está segurando o analógico atualmente neste frame!
