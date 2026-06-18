# JSON (Processamento de Dados)

Módulo padrão da Cat Engine nativo de processamento C++ que facilita a leitura e a escrita do formato JSON. Ideal para salvar progresso do jogo de forma limpa, realizar comunicação com servidores web ou configurações de personagens.

---

## Modos de Uso

### `json.encode(value)`
* Converte uma tabela Lua (ou valor compatível) em uma string no formato JSON com identação mínima.
* **Retorno:** `String`
* **Exemplo:**
  ```lua
  local dados_jogador = {
      nome = "Herói",
      vida = 100,
      inventario = {"Poção", "Espada"}
  }
  
  local json_string = json.encode(dados_jogador)
  system.log(json_string) 
  -- Resultado aproximado: {"nome":"Herói","vida":100,"inventario":["Poção","Espada"]}
  ```

### `json.decode(string)`
* Pega uma string de formato JSON e converte de volta em uma Tabela mapeada Lua para poder iterar.
* **Retorno:** `Table`
* **Exemplo:**
  ```lua
  local texto_json = '{"level":5, "dificuldade":"Dificil"}'
  
  local dados = json.decode(texto_json)
  system.log("Nível Atual: " .. dados.level)
  ```

---

## Usando junto ao FileSystem

A melhor forma de gerenciar progresso com JSON:

```lua
-- Salvando...
local save_data = { coins = 500, checkpoint = 3 }
fs.write(fs.savePath .. "/meusave.json", json.encode(save_data))

-- Lendo e convertendo...
if fs.exists(fs.savePath .. "/meusave.json") then
    local texto = fs.read(fs.savePath .. "/meusave.json")
    local progresso = json.decode(texto)
    system.log("Moedas do banco: " .. progresso.coins)
end
```
