# Filesystem (Sistema de Arquivos)

O módulo `Filesystem` manipula todo o carregamento e salvamento de dados do seu projeto. Seja ler níveis criados externamente por texto/JSON ou guardar o progresso do usuário durante as partidas. Ele restringe as ações para ambientes seguros para prevenir problemas com o Android local.

---

## Caminhos GLOBAIS do Motor (Strings Rápidas)

* **`fs.basePath`**: O diretório raiz fixo do "Jogo", onde ficam arquivos imutáveis instalados (como suas imagens, seus scripts e assets principais distribuídos no app).
* **`fs.savePath`**: Diretório especial oculto que o próprio Android dedica pro APENAS seu App ler e escrever. Tudo usando `write()` normalmente aponta de forma transparente e segura pra dentro dessa raiz garantida pra não apagar arquivos do OS.

---

## Funções Disponíveis

### `fs.read(path)`

* **O que isso faz?** Lê inteiramente o conteúdo do arquivo presente no caminho enviado em formato de String pura. Ideal para interpretar arquivos de texto nativo.
* **Parâmetros:**
  * `path` (string): Caminho do recurso (ex: `"data/level1.json"`).
* **Retorno:**
  * `String` (ou nulo se ocorrer falha ou o arquivo não contiver formato suportado).
* **Exemplo:**
  ```lua
  local save_data = fs.read("save1.txt")
  if save_data then
      print("Dados recuperados: " .. save_data)
  end
  ```
* **O que acontece se eu usar errado?** Passar caminhos inválidos ou procurar arquivos que não existem resulta em um retorno vazio (`nil`).

---

### `fs.append(path, data)`

* **O que isso faz?** Anexa informações (em String) no final de um arquivo já existente, ao invés de sobrescrever o conteúdo inteiro como o `fs.write()`. Muito útil para realizar longos "Logs" de erro e de status das partidas!
* **Exemplo:** `fs.append("log.txt", "\nNovo Player Conectado.")`

---

### `fs.list(path)`

* **O que isso faz?** Retorna uma Tabela em Lua contendo todos os nomes de arquivos e pastas enraizados e filhos diretos daquele diretório providenciado. Excelente para carregar de forma dinâmica listas de fases e inimigos em menu!
* **Retorno:** Array Tabela de `Strings`.
* **Exemplo:**
  ```lua
  local pastas = fs.list("saves/")
  for _, nome_save in ipairs(pastas) do
      system.log("Slot encontrado: " .. nome_save)
  end
  ```

---

### `fs.mkdir(path)` / `fs.isDirectory(path)`

* **O que isso faz?** Gerencia pastas nativas! `mkdir` cria um novo diretório, enquanto `isDirectory` verifica e atesta como booleano se realmente aquele nome em path corresponde à uma pasta. 

---

### `fs.remove(path)`

* **O que isso faz?** Exclui e destrói para sempre o diretório ou o Arquivo mencionado na Sandbox do aparelho.
* **Atenção Cuidado:** Operação C/C++ Nativa. Sem lixeira para recuperação, utilize com responsabilidade na hora de excluir de vez as gravações dos save states e perfis!

---

### `fs.write(path, data)`

* **O que isso faz?** Registra e sobrescreve um documento ou recurso utilizando strings. Perfeito pra salvar dados de pontuação. Atenção: Sobrescreve inteiramente o conteúdo passado.
* **Parâmetros:**
  * `path` (string): Nome longo a ser editado. Ex: `"save1.txt"`.
  * `data` (string): Conteúdo bruto para inserção.
* **Retorno:** `Booleano` constatando sucesso de execução ou falha.
* **Exemplo:**
  ```lua
  local dados_jogador = "score: 100\nlevel: 2"
  fs.write("save_slot_1.txt", dados_jogador)
  ```

---

### `fs.exists(path)`

* **O que isso faz?** Utilitário simples e prático para conferir rapidamente se um diretório ou arquivo foi alocado sem causar nenhum crash abrindo arquivos inexistentes.
* **Parâmetros:**
  * `path` (string): Arquivo para varredura.
* **Retorno:** `Booleano`. `true` em caso positivo, o arquivo existe. `false` pra contrário.
* **Exemplo:**
  ```lua
  if fs.exists("my_texture.png") then
      graphics.loadTexture("my_texture.png")
  end
  ```

### `fs.load(path)`
Carrega um arquivo Lua como um chunk executável e de código na memória, permitindo execução via `load()`.

### `fs.isFile(path)`
Verifica se o caminho fornecido é efetivamente um arquivo padrão em vez de um diretório ou se é inexistente. Retorna um booleano.

### `fs.getSize(path)`
Retorna o tamanho total (em Bytes) de um arquivo no armazenamento local.

### `fs.getExtension(path)`
Retorna apenas a extensão pertencente ao arquivo (ex: "txt", "png", "json"). Retorna nulo se não houver.

### `fs.getFileName(path)`
Retorna de forma limpa apenas o nome do arquivo ignorando todo o prefixo estrutural das pastas (ex: `data/saves/file.txt` retorna `file.txt`).

### `fs.getDirectory(path)`
Retorna apenas o percurso (pastas/diretórios) excluindo o arquivo alvo focado contido nele.

### `fs.getModifiedTime(path)`
Traz em formato numérico Timestamp, indicando quando o arquivo/caminho sofreu qualquer modificação e salvamento.


## Outras Funções do Módulo

### `fs.copy(src, dst)`

* **O que isso faz?** Copia um arquivo de um diretório de origem para um diretório de destino.
* **Parâmetros:**
  * `src` (string): O caminho do arquivo original que você deseja copiar.
  * `dst` (string): O caminho de destino para onde o arquivo será copiado, incluindo o novo nome do arquivo.
* **Exemplo:**
  ```lua
  fs.copy("saves/save1.txt", "saves/save1_backup.txt")
  ```

### `fs.move(src, dst)`

* **O que isso faz?** Move um arquivo de um diretório para outro, excluindo o arquivo original no processo. Pode ser usado para organizar arquivos salvos em diferentes pastas.
* **Parâmetros:**
  * `src` (string): O caminho do arquivo original que você deseja mover.
  * `dst` (string): O diretório e nome de destino para onde o arquivo irá.
* **Exemplo:**
  ```lua
  fs.move("temp_data.json", "data/persistent_data.json")
  ```

### `fs.rename(src, dst)`

* **O que isso faz?** Renomeia um arquivo ou pasta existente. É semelhante ao `fs.move`, mas seu propósito principal é alterar o nome do arquivo no mesmo diretório.
* **Parâmetros:**
  * `src` (string): O caminho atual do arquivo existente.
  * `dst` (string): O novo nome e caminho do arquivo alterado.
* **Exemplo:**
  ```lua
  fs.rename("saves/player_old.txt", "saves/player_new.txt")
  ```

