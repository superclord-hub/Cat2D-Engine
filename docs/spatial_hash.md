# Spatial Hash (Buscas e Otimização A.I.)

Sistemas Gigantes com centenas de NPC Inteligentes tendem a travar o Game no simples fato de ficarem calculando `math.distance(npc, outro_npc)` a todo segundo testando 20.000 opções em Arrays pra tentar achar um adversário perto! 

Para resolver, nós utilizamos o particionamento em Grade (Spatial Hash)! Base de quadtree e subdivisões rápidas.

---

## Como Funciona?

### Injetando Posições Mapeadas Brutas
Use o global isolado de altissima velocidade de C++: `spatial` para adicionar coisas.

* `spatial.insert(id_unico, x, y)`: Salva o ID (int) na grade C++.
* `spatial.update(id_unico, x_novo, y_novo)`: Move ele nas grades com a matemática super veloz em background.
* `spatial.remove(id_unico)`: Apaga ele!
* `spatial.clear()`: Reseta tudo do mapa.

---

## Buscando Vizinhos Imediatos do Seu NPC

Quando seu inimigo tentar achar presas próximas, ignore a Lua Iteration Table (Tabela Lua) e puxe:

* `local presas_proximas = spatial.getInRadius(npc_x, npc_y, raio_visao)`

Isso retornará (em Microssegundos Reais) apenas os **IDs** próximos de quem realmente estava naquele raio e não todos os inimigos da sala para você fazer contas brutas. Uma economia extrema no sistema nativo.
