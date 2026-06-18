# Thread (Corrotinas e Assincronia)

O módulo de `thread` facilita a criação de rotinas agendadas (Corrotinas em Lua) combinadas com Assincronismo (`async`). Ele é excelente para criar fluxos de jogo contínuos lógicos: como NPCs com inteligências complexas cheias de pauses ou temporizaçoes de tutoriais de interface sem infernar o loop principal do `update`.

---

## Modos de Uso

### `thread.start(func)`
* Inicia a execução isolada de uma Função anônima em paralelo ao Loop.
* **Retorno:** Corrotina gerada.
* **Exemplo:**
  ```lua
  thread.start(function()
      system.log("Passo 1!")
      thread.sleep(2) -- Trava esta thread por 2 segundos mas o jogo continua!
      system.log("Passo 2!")
  end)
  ```

### `thread.stop(co)`
* Mata e encerra a execução de uma corrotina gerada pelo `.start()`.

---

## Modos de Congelamento Lógico

Essas funções servem EXCLUSIVAMENTE para serem declaradas dentro de uma `thread` válida e irão interromper e "adormecer" a linha de controle até que o prazo delas se conclua, sendo muito parecido com async/await em outras linguagens, liberando o jogo para rodar macio.

* **`thread.sleep(seconds)`** 
  Interrompe o script por X segundos literais antes de voltar na próxima linha.
  
* **`thread.wait_until(condition_func)`**
  Gela o script até que uma Função condicional retorne verdadeiro (`true`).
  *Ex: `thread.wait_until(function() return player_life_is_zero end)`*

* **`thread.tween(obj, duration, target_x, target_y)`**
  Inicia automaticamente uma animação (Tween de Interpolação Linear) dos atributos `x` e `y` de uma Tabela/Objeto Lua e adormece esta thread até o tempo exato final que a animação for declaradamente pronta. (Muito útil para animadores de cenas de visual novel).

---

## Exemplo Real: Inimigo Burro
```lua
function iniciar_inimigo(inimigo)
    thread.start(function()
        while true do
            -- Anda devagar
            thread.tween(inimigo, 3.0, inimigo.x + 100, inimigo.y)
            -- Fica Parado olhando o ambiente
            thread.sleep(1.0)
            -- Retorna devagar
            thread.tween(inimigo, 3.0, inimigo.x - 100, inimigo.y)
        end
    end)
end
```


## Outras Funções do Módulo

### `thread._resume()` (ou `:_resume()`)
Dá o estopim inicial para ativar o mecanismo (seja o gerador de partículas, som, stream e etc).

