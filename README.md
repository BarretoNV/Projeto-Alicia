# Chef's Quest

Um joguinho romântico e interativo feito em HTML5 Canvas, da minha amiga para o namorado dela, feito em arquivo único, para contar uma história em fases. O jogo tem tela inicial, introdução, mapa de memórias entre fases, 6 mini-games e uma cena final.

## Como Rodar

Abra o arquivo `index.html` em um navegador moderno.

O projeto não precisa de servidor, build ou dependências externas. A imagem `pngharley.png` deve ficar na mesma pasta do `index.html`, porque ela é usada na tela inicial. Se a imagem não carregar, o jogo ainda tem um desenho de moto em canvas como fallback.

## Controles

- `Espaco` ou `Enter`: avancar dialogos, confirmar escolhas e interagir.
- `Setas`: mover, navegar ou escolher opcoes, dependendo da fase.
- `WASD`: atalhos equivalentes em algumas fases.
- `M`: ligar/desligar som.

O audio usa Web Audio API e so comeca depois da primeira interacao do jogador, como tecla, clique, `Espaco` ou `Enter`.

## Debug

Para testar fases sem jogar tudo desde o comeco, use parametros na URL:

- `index.html?scene=title`: abre direto na tela inicial.
- `index.html?fase=1`: abre direto na Fla Fit.
- `index.html?fase=2`: abre direto no Morro do Rato.
- `index.html?fase=3`: abre direto no Cinema.
- `index.html?fase=4`: abre direto na Praia de Santa Clara.
- `index.html?fase=5`: abre direto no Calebito.
- `index.html?fase=6`: abre direto em O Dia a Dia.
- `index.html?final=1`: abre direto na cena final.

Tambem existem atalhos globais:

- `Ctrl+R`: reinicia o jogo recarregando a página.
- `Ctrl+1` ate `Ctrl+6`: pula direto para a fase correspondente.
- `Ctrl+F`: pula direto para o final.

Os atalhos limpam estados temporarios como dialogo ativo, fade pendente e coracoes flutuantes para evitar mistura de cenas.

## Fases

1. **Fla Fit Academia**  
   Mini-game estilo Simon Says com botoes de direcao.

2. **Morro do Rato**  
   Plataforma simples: pule, colete goiabas e chegue na bandeira.

3. **Cinema**  
   Quiz de memoria afetiva com escolhas por setas e confirmacao.

4. **Praia de Santa Clara**  
   Desvie das piranhas e colete bagres.

5. **Sorvete Calebito**  
   Escolha o sabor certo antes dos sabores embaralharem.

6. **O Dia a Dia**  
   Puzzle dos livros e boss da Bella protegendo o pao.

Depois das fases, o jogo segue para a cena final.

## Arquitetura Geral

O jogo fica praticamente todo dentro do `<script>` do `index.html`.

- `createTitle()`: tela inicial.
- `createIntro()`: introducao narrativa.
- `createLocCard(idx)`: mapa de memorias antes de cada fase.
- `createGame(idx, skipIntro)`: roteador das 6 fases.
- `createFinal()`: cena final.
- `showDialog()`, `updateDialog()` e `drawDialog()`: sistema de dialogo.
- `SFX`: efeitos sonoros e temas musicais por fase usando Web Audio API.
- `drawHUD()` e `drawBottomHint()`: helpers para HUDs padronizados.

Cada cena normalmente retorna um objeto com:

- `update()`: atualiza logica, timers e input.
- `draw()`: desenha a cena no canvas.
- `onKey(code)`: trata teclas especificas da cena quando necessario.

Os sprites principais sao desenhados via canvas/pixel art. A Harley da tela inicial usa `pngharley.png`, com fallback em `drawHarley()`.

## Dicas De Manutencao

- Para testar uma fase especifica, use `?fase=N`.
- Para validar sintaxe depois de editar o HTML, extraia o conteudo do `<script>` e rode `node --check`.
- Prefira mudancas localizadas por fase, porque o jogo e um arquivo unico grande.
- Ao mexer em dialogos, teste `Espaco` e `Enter`.
- Ao mexer em audio, lembre que o navegador so libera som depois da primeira interacao.
- Ao mexer em musicas, use `SFX.setMusicTheme(nome)` e mantenha apenas um timer de musica ativo.
- Ao mexer na tela inicial, preserve `pngharley.png` ou o fallback `drawHarley()`.

## Arquivos

- `index.html`: jogo completo.
- `pngharley.png`: imagem da Harley usada na tela inicial.
