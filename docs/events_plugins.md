# Eventos & Plugins (Comunicação de Subsistemas)

O ecossistema oficial Cat Engine propõe o desacoplamento de lógicas em arquivos Lua distintos de "Módulos de Sistema" em vez de apenas scripts pesados e maciços. Para isso a plataforma envia os módulos arquiteturais `events` e `plugin` integrados.

---

## Módulo: Events

Base de Comunicação em Estágio de Reatividade (Listen and Execute). Uma interface global para intermédio entre código em vez de criar dependências diretas de requerimentos cruzados que sobrecarregam a RAM do aparelho.

### Modo de Uso
* **`events.on(event_name, id, func)`** 
Acopla um ouvinte de identificador único a um evento global de String.
  ```lua
  -- Exemplo: Uma bomba no jogo ouve se a fase foi encerrada.
  events.on("phase_ended", "bomb_listener_01", function(msg)
      system.log("Bomba desarmada porque: " .. msg)
  end)
  ```

* **`events.off(event_name, id)`** 
Remove da memória o ouvinte, sendo crucial para destruir dependências que não existem mais e evitar disparos fantasmas e estourar a Heap Limit.

* **`events.emit(event_name, ...)`** 
O "Gatilho". Dispara na ordem todos os ouvintes associados aquele evento global.
  ```lua
  -- Lógica isolada de UI
  events.emit("phase_ended", "O jogador tocou o fim da fase!")
  ```

---

## Módulo: Plugin

O injetor de Namespaces da Engine. Muitas vezes em suas bibliotecas separadas dentro de `catlibs/`, não basta retornar tabelas normais via diretiva pura `require`, e você quer de forma segura que ele injete e registre o seu ecossistema sem repetições pesadas globais do `_G`. 

### Modo de Uso

* **`plugin.register(namespace, plugin_module)`**
O Sistema acarreta o Módulo diretamente na Tabela protetora Oficial interna da Engine como ferramenta. O nome do namespace em string virará a assinatura dele.

* **`plugin.get(namespace)`**
Resgata em Singleton e devolve o Módulo se ele já existe e foi carregado na inicialização global, útil para scripts de entidades resgatarem lógicas mestras do usuário com facilidade sem reconstruírem bibliotecas iterativas do disco local (`catlibs/`).

* **Onde fica guardado?**
A Cat Engine armazena os módulos nos canos abertos locais e no espaço em raiz via:
  * `cat.events`
  * `cat.plugin`
Abaixo dos panos, o módulo em Lua só direciona os retornos para os apontadores raízes de `cat`.
