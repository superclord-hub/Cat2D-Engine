# Índice da Documentação - Cat Engine

Bem-vindo à documentação oficial da **Cat Engine**, uma framework em Lua focada em simplicidade e desempenho para desenvolvimento de jogos 2D no Android.

## Introdução

Esta documentação foi criada com o objetivo de responder rapidamente três perguntas:
1. O que isso faz?
2. Como eu uso?
3. O que acontece se eu usar errado?

### Por onde começar?
- **[Guia de Início Rápido](#guia-de-inicio-rapido):** Crie algo funcionando em poucos minutos.
- **[Módulos da API](#modulos-da-api):** Aprenda a usar cada módulo separadamente.
- **[Boas Práticas](#boas-praticas):** Dicas para manter seu código limpo e performático.

---

## Guia de Início Rápido

Criar seu primeiro jogo com a Cat Engine é muito fácil.
Tudo que você precisa fazer é programar em Lua usando nossos módulos integrados.

**Exemplo básico - Exibindo uma imagem:**

```lua
-- Carrega a imagem apenas uma vez no início (não dentro do loop de desenho)
local playerImage = graphics.loadTexture("player.png")

function draw()
    -- Define a cor de fundo (Opcional)
    graphics.clear({r=0, g=0, b=0, a=255})
    
    -- Desenha a textura na posição x=100, y=100
    graphics.draw(playerImage, 100, 100, 0, 1, 1)
end
```
**Resultado esperado:**
Isso exibirá a imagem `player.png` na posição (100, 100).

---

## Módulos da API

Abaixo você encontra a documentação detalhada para cada módulo base da engine:

* [**Tutorial (Animações e Dicas Rápidas)**](tutorial.md) - Aprenda de forma fácil como rotacionar sprites, criar animações simples e usar spritesheets.
* [**Graphics (Gráficos)**](graphics.md) - Desenho de imagens, formas texturas diversas.
* [**Audio (Áudio)**](audio.md) - Tocar efeitos sonoros e músicas de fundo (BGM/SFX).
* [**Input (Entrada)**](input.md) - Toques em tela simultâneos, Gestos, e sensores de Mapeamento, teclado Virtual de celular.
* [**System (Sistema)**](system.md) - Time step da Engine de forma rápida e Logs.
* [**Math (Matemática)**](math.md) - Distâncias de colisão, limitador, Matemática Simples.
* [**Math/Avançado (Vetores, Ruído e Matriz)**](math_advanced.md) - Construções avançadas Random, Transformações XYZ.
* [**Physics (Física Base)**](physics.md) - Lentes Gravitacionais e interações para Corpos rígidos, dinâmicos e estáticos.
* [**Physics/Colisões (Eventos)**](physics_collisions.md) - Catchbacks Limpos de Quando as coisas se chocam de verdade.
* [**Physics/Avançado (Raycasts/Articulações)**](physics_advanced.md) - Queries aprofundadas Lasers, Constraints e Formas de materiais.
* [**Particles (Partículas)**](particles.md) - Massivo de sistemas visuais pesados simultâneos de C++.
* [**Filesystem (Arquivos)**](filesystem.md) - Motor Sandbox para leitura fácil.
* [**Filesystem/Avançado (Zip/Leituras Otimizadas)**](filesystem_advanced.md) - Manipulações via Zip Modding Engine.
* [**ECS (Entity Component System)**](ecs.md) - Atribui modularmente em blocos tabelados estados.
* [**Video (Reprodutor Interativo)**](video.md) - Cutscenes Renderizadas direto na tela principal rodando em Background.
* [**FastBatch (Emissor Visuais de Estremos)**](fast_batch.md) - Desvios Absolutos para Desenhar Bullets ou Mundos grandes em +20k Draw Calls num Único Frame.
* [**Shaders (Efeitos em Placa de Vídeo)**](shaders.md) - Programe Efeitos Visuais via GLSL (Relevo, Negativo, Lentes D'agua).
* [**OpenGL Nativo (Low-Level)**](gl_lowlevel.md) - Manipulações da mesa gráfica Bruta direta via Hardware GLES2+.
* [**SpatialHash (Colisões Rápida sem Física)**](spatial_hash.md) - Buscadores Rápidos C++ para I.A e radares em Grandes Mapas.
* [**Utils (Utilitários Variados)**](utils.md) - Ferramentas Assincronicidades Rápidas.
* [**System/UI Nativa (Interface de Hardware)**](native_ui.md) - Controle do Telefone: Abrir Url, Vibrações sensoriais, Exploradores de Diretório da Galeria.
* [**Bibliotecas Customizadas & conf.lua (Configurações)**](libraries_and_conf.md) - Entenda o ecossistema de configs e como usar a ui.lua nativa em componentes visuais.
* [**Vec2 (Vetores Bidimensionais)**](vector2.md) - Objeto Matemático C++ para Posições, Velocidades e Álgebra Linear de Alta Performance.
* [**JSON (Processamento)**](json.md) - Manipulação, Criptografia leve de Decodes em Texto de configuração JSON.
* [**Joystick (Controles Virtuais)**](joystick.md) - Simulador de Hardware de Analógicos Interativos de Tela Direcional.
* [**Tween (Animações e Interpolações)**](tween.md) - Deslizador suave de Propriedades Lineares e Elásticas Dinamicamente.
* [**Thread (Assincronia Corpos)**](thread.md) - Subsistemas de repousos corrotineiros (Asyn/Await behavior).
* [**Events & Plugins**](events_plugins.md) - Intermédios de Listeners Desacoplados nativos para GameStates.
* [**Retro3D (Modo 7 & Raycast)**](retro3d.md) - Motor Pseudo-3D Acelerado via Shader para Doom Clones.
* [**Engine3D (Experimental)**](engine3d.md) - Pseudo-Renderizador 3D para leitura de Malhas OBJs em perspectivas Z.

---

## Boas Práticas

Siga estas dicas para evitar problemas de desempenho e organização:

- **✅ Reutilize Imagens e Fontes Carregadas:** Carregue recursos fora do loop principal, preferencialmente na inicialização do script, e apenas desenhe/use-os dentro de eventos como `update` ou `draw`.
- **❌ Não carregue arquivos pesados em loops:** Nunca utilize `graphics.loadTexture` ou `audio.loadSound` dentro de funções contínuas de loop (ex. `update`, `draw`). Fazer isso destruirá a taxa de quadros e causará estouro de memória.

## FAQ (Perguntas Frequentes)

**Por que minha imagem não aparece?**
- O caminho fornecido em `graphics.loadTexture(path)` está correto?
- Verifique se a variável salvou corretamente a imagem e se `graphics.draw` está sendo chamada dentro da função correta de desenho (`draw()`).

**Por que o som não toca?**
- O arquivo `.ogg` ou `.mp3` existe no diretório correto?
- Verifique se você chamou `sound:play()` usando a source retornada.

---
_Aviso: Consulte também o arquivo `api_reference.txt` para uma lista rápida com as assinaturas completas de cada função da engine._

