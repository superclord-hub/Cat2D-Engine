# Engine3D (Mundo Diferencial Experimental)

Embora a Cat Engine seja fundamentalmente programada a vida inteira sobre pilares da Ortoprojetada OpenGL ES 3.2 do Android e o amor eterno por Pixel Art bidimensional, o motor esconde em seu baú um injetor de API simplificado 3D pseudo-experimental (`engine3d`).

---

## Modos de Uso Rápido e Limitações

A renderização utiliza Z-Buffering em Background e projeta Perspectiva na Câmera nativa base com shaders de profundidade sem iluminação.

* **Aviso Absoluto:** Esse módulo **NÃO** foi construído para criar jogos colossais de RPG mundo aberto com montanhas e física complexa tridimensional, e sim mini-games experimentais de fliperamas do zero, Mode7 (Mario Kart SNES) em perspectiva e objetos rotativos em Menus da UI.

---

## Funções Disponíveis

### `engine3d.camera`
Tabela global de controle da câmera no vetor tridimensional, possuindo:
```lua
engine3d.camera.x = 0
engine3d.camera.y = 5
engine3d.camera.z = -10
engine3d.camera.pitch = 0  -- Inclinação
engine3d.camera.yaw = 0    -- Rotação de lado
```

### `engine3d.project(x, y, z)`
Dado os eixos em float do volume 3D, ele faz o cálculo matricial da Projection View de acordo com a Câmera atual devolvendo no final três itens do espaço visível plano:
* **Retorna:** `tela_X`, `tela_Y`, `escala_Z`
* _Útil para você grudar lógicas antigas do `graphics.draw` em 2D por cima e dar escala menor quanto mais longe tiver._ 

### `engine3d.drawTexturedCube(texture, x, y, z, size, rx, ry, rz)`
* **texture:** Um resource lido em `graphics.loadTexture()`.
* Desenha um Quadrado em bloco tridimensional do qual todas as normais reagem as angulações de Rotações com Wrap Repeats em UV baseados nesta textura do Motor Gráfico Oficial.

### Suporte a `.obj` Meshes
Leia malhas sólidas simplistas exportadas pelo Blender ou Magicavoxel em formato ASCII Wavefront. (Não renderiza materiais complexos via `.mtl` internamente, você cobre a modelagem toda com uma textura única principal chapada Diffuse).

* **`engine3d.loadOBJ(path)`** 
Devolve a Estrutura (Table) na memória mapeada de arrays de Vértice/Indice do Sistema Filesystem (Recomendado botar em pastas do Asset).

* **`engine3d.drawOBJ(model, texture, x, y, z, scale, rx, ry, rz)`** 
Desenha e varre as ordens das matrizes de Render daquele modelo (`model` carregado acima com formato OBJ) aplicando rotação nos três Eixos nativos no frame com o Z-Index apropriado pela Câmera.
