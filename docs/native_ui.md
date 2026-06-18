# System Nativo (Teclado, Sensores, Permissões Android)

Neste módulo nativo (System Ui) está incluso o poder para engajamento e utilidades que só são possíveis com a plataforma nativa do Android cruzando C/Jni de ponte real pro Hardware de forma invisível pra você focar apenas em seu jogo Lua.

---

## Modos de Tela Básicos

### `system.setOrientation(orientation)`
* **O que isso faz?** Quebra a inércia, trancando ou alternando a disposição visual pra forçar os jogares e a janela a rodarem seu jogo de um modo pretendido sem virar quando tombam a tela.
* **Exemplo de uso:** `system.setOrientation("landscape")` (Paisagem horizontal para ação) ou `"portrait"` (Retrato de celular comum em pé).

### `system.setFullscreen(isFullscreen)`
* **O que isso faz?** Força uma quebra imersiva de exclusividade e oculta toda as interações de Android como a Barra Superior de Horários e Barra Puxada Inferior botando total prioridade da Engine usando `true`.

---

## Interação Humana

### `system.vibrate(ms)`
* **O que isso faz?** Interage por impulsos táteis usando o rotor de massas do seu dispositivo Android chacoalhando e notificando o toque por tempo escolhido (milisegundos).
* **Cuidado de uso:** Muito usado consome bateria e fadiga as mãos dos usuários rapidamente, utilize para feedbacks pontuais (ex: Recebeu dano forte, Finalizou desafio longo, Clique chave). E tenha em mente que Aparelho Antigo vai falhar esse comando (ignore pois é nativamente silenciado em bypass sem gerar crash fatal).

### `system.openURL(url)`
* **O que isso faz?** Minimiza seu App na base sem fechar, acoplando e despachando um intent universal do Android forçando a interface nativa abrir as páginas requeridas direto pelo Navegador Local (Google Chrome, Firefox).

---

## Explorador de Arquivos & Multimídia Nativos

As vezes se precisa pegar algo pesado salvo pelo computador para dentro da engine para desenhar mas sem copiar ou carregar isso em pasta:

### `system.pickFile()` / `system.getPickedFile()`
* **O que faz?** Paralisa a thread e levanta as pastas da Galeria Local do telefone para escolher documentos ou Fotos pra carregar ao jogo em formatos string `paths` depois de resolvido para ser carregado pelo seu jogo.
* **Atenção:** Funciona através de assincronicidade interna, deve aguardar ou rodar de forma continua com estado `if` para ver se não retornou Nada ("nil") antes de tentar converter a String.

### `system.pickColor()` / `system.getPickedColor()`
* Usado similarmente porém exibe os Modos Radiais Nativos pra Selecionar paletas exatas visuais `{r,g,b,a}` convertíveis diretamente.

---

## Teclado e Textos Nativos

### `system.showKeyboard()` / `system.hideKeyboard()`
* Sobe e Esconde mecanicamente o teclado Virtual do Android para que o jogador possa digitar!
* Junte com `system.getKeyboardHeight()` para saber se subiu com que tamanho!
