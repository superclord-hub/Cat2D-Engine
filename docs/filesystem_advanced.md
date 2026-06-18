# Filesystem Avançado (Leitura, I/O e Zip)

O módulo avançado de Arquivos tem como premissa total cobrir grandes demandas que arquivos longínquos trazem à performance, além de suporte de modding utilizando empacotamentos sem onerar os caminhos relativos.

---

## Leitura Segmentada Otimizada

Carregar um livro inteiro ou nível extenso via `read()` trava em uma super String ocupando toda a memória RAM em blocos ineficientes de re-alocação lenta na hora da interpretação.

### `fs.readLines(path)` / `fs.writeLines(path, lines)`
* **O que isso faz?** Lê (Iterando em Arrays e Tabelas limpas segmentadas puras) cada trecho por linha (\n). Resultando numa Tabela (uma Tabela de linhas). Onde para manipular, não é preciso quebrar textos.
* **Exemplo de Iteração Fácil de Levels Tiled:**
  ```lua
  local levelLineData = fs.readLines("matrix_do_mapa.txt")
  for key, line in ipairs(levelLineData) do
      system.log("Na linha " .. tostring(key) .. " esta carregado: " .. line)
  end
  ```

### `fs.readBytes(path)` / `fs.writeBytes(path, bytes)`
* Muito utilizado ao interpretar Modelos Puros de Criptografia Baseados em Base64 ou dados Raw não legíveis convencionalmente por interpretação textual pura.

---

## Ações de Modificador Interno e Zip File

### `fs.copy(src, dst)` / `fs.move(src, dst)` e `fs.rename(...)`
Transita locais internados na Sandbox com segurança. Move transfere direto de lugar (Excelente para atualizações da Cloud ou Internet e Download de Patchs de DLC nativos na CatEngine onde eles salvam na temporária local pra serem mandados ao definitivo Save Principal posteriormente).

### `fs.zip(folder, zipfile)` / `fs.unzip(zipfile, destfolder)`
O Grande trunfo nativo inter-conectado C-Lua interno da Engine.
* **O que isso faz?** Extrai e Empacota a Custo Zero em Hardware Multithread de forma assustadoramente leve! Suportar conteúdo da Comunidade via Mods empacotados, pacotes de missões personalizadas ou custom Skins baixadas pra dentro do Game jamais foi tão fácil.
* **Exemplo Simples Local:**
  ```lua
  -- Extraí skins baixadas novas
  fs.unzip("patch_skins_natal.zip", "assets/skins/")
  local nova_skin = graphics.loadTexture("assets/skins/santa.png")
  ```

---

## Dicas e Otimizações Críticas das Sandbox (Files)
* A Cat Engine NUNCA permitirá que você escreva ou corrompa dados por fora da Engine App! Jamais tente enviar endereços ou Strings Absolutas com `////` na tentativa nativa Root do aparelho de corromper HD interno pra Hack ou acesso Root nativo proibido da Android Security! O Filesystem sempre injetará internamente SandBox Appending Limits travando todo ponteiro à `Android/Data/com.app.jogo/files/` impedindo as leituras. Você não é obrigado nem permitido criar o "Início absoluto", apenas caminhos relativos ao jogo, exemplo:`"images/sprites/hero.png"`.

### `fs.createFile(path)`
Força de forma bruta a criação do arquivo no diretório específico, ignorando ou zerando qualquer arquivo antigo que estivesse no mesmo nome. Diferente de `write()` que grava texto nativo.

### `fs.clearFile(path)`
Zera limpo todo o conteúdo texto e binário injetado no arquivo alvo existente, poupando custo de deletar o arquivo pra criar um novo. 

### `fs.getFreeSpace(path)`
Puxa o tamanho em bytes exatos de área livre pro App na memória do storage local para garantir limites de gravações seguras do arquivo alvo.

### `fs.getTotalSpace(path)`
Semelhante a FreeSpace, traz a capacidade de limite total. 

### `fs.isSymlink(path)` e `fs.makeSymlink(target, linkPath)`
Recursos extremamente profundos limitados ou específicos de acesso e organização interna e C++, `isSymlink()` varre pra verificar Links e `makeSymlink()` força a criação de falsos diretos de caminhos do Storage.

### `fs.isReadable(path)` e `fs.isWritable(path)`
Booleans eficientes que atestam perfeitamente se você tem bloqueios do nativo da plataforma e Security Limits impedindo tanto leitura `isReadable` como corrompendo a escrita `isWritable`.
