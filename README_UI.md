# UI da Operação Área Segura — guia de montagem no GDevelop

Tudo aqui foi pensado para a tela de **1920×1080** e para o **pixel de 2 px** que combinamos para o cenário. Os botões e painéis já estão desenhados com esse pixel: a arte original é metade do tamanho e foi ampliada 2× sem suavizar, então eles ficam do mesmo tamanho de pixel do fundo e dos sprites.

As imagens de `mockups/` mostram como cada tela deve ficar. A fonte que aparece nelas é só um substituto (veja "Fonte" abaixo).

## 1. Configurações do projeto (uma vez só)

- **Resolução do jogo:** 1920×1080 (Propriedades do jogo → Resolução).
- **Escala de pixels:** modo de escala **Nearest** (Propriedades do jogo → "Modo de escala"), para os painéis não ficarem borrados.
- **Arredondamento de pixels** ligado, se a opção existir na sua versão.
- Os objetos de UI ficam em **100%**: o tamanho vem do próprio objeto (PanelSprite), não de redimensionar a imagem.
- Use sempre **larguras e alturas pares** nos painéis, para as bordas continuarem alinhadas com a grade de 2 px.

## 2. Camadas (Layers) da cena

| Camada | O que fica nela |
|---|---|
| Cenário | fundo e sprites do pacote HD (fundo em y=120) |
| HUD | topo (y 0–120) e ficha à direita (x 1440–1920) |
| Escurecer | retângulo preto (Pintor de formas, ou um Sprite preto esticado), escondido até abrir uma pergunta ou o painel final |
| Pergunta | modal de pergunta e painel final |

Ordem de baixo para cima: Cenário → HUD → Escurecer → Pergunta. Deixe HUD, Escurecer e Pergunta com a câmera parada.

- **Com uma pergunta aberta:** escureça só a cena, com um retângulo de 1440×960 em (0, 120) e 60% de opacidade. A ficha da direita continua clara, porque o jogador ainda pode pedir dica enquanto responde.
- **No painel final:** escureça a tela inteira, 1920×1080 em (0, 0), com 75% de opacidade.

## 3. Texturas e margens (PanelSprite)

Crie cada PanelSprite com uma das imagens de `kit_ui/` e preencha as margens abaixo. A imagem é desenhada nas bordas e esticada no meio.

| Imagem | Tamanho | Margens (esq/dir/cima/baixo) | Uso |
|---|---|---|---|
| painel.png | 48×48 | 12 | fundo do HUD e da ficha |
| painel_destaque.png | 48×48 | 12 | modal de pergunta e painel final (borda amarela) |
| chip.png | 48×48 | 12 (use 6 se a altura for menor que 48) | chips do HUD, cartão do objeto, etapas inativas |
| botao_normal / hover / pressionado / desativado.png | 48×48 | 12 | botões principais (4 estados) |
| opcao_normal / hover / correta / errada / desativada.png | 48×48 | 12 | alternativas de resposta (5 estados) |
| linha_normal / resolvida.png | 48×48 | 12 nas linhas de risco; **6** nas células da barra de progresso | ficha |
| slot_vazio / slot_cheio.png | 52×60 | 6 | slots do código final |

Imagens que **não** são PanelSprite (Sprite comum, 100%):

| Imagem | Tamanho | Uso |
|---|---|---|
| caixa_vazia / caixa_marcada.png | 24×24 | caixinha de cada linha da ficha |
| cadeado_fechado / aberto.png | 24×28 | cadeado da saída |
| dica_vazia / dica_cheia.png | 20×24 | lâmpadas de dica |
| icone_fechar.png | 16×16 | extra, não aparece nos mockups (no modal, a tecla Esc fecha) |
| faixa_alerta.png | 32×32 | objeto **Sprite ladrilhado** (Tiled Sprite): largura 1920 e altura 16 no HUD, largura 976 e altura 14 no modal |

Em botões e alternativas, troque a imagem do PanelSprite por estado (normal, hover, pressionado, desativado), do mesmo jeito que se troca a animação de um Sprite. Os eventos de clique ficam para depois.

## 4. Posições, tela por tela

As coordenadas são na cena de 1920×1080 (canto superior esquerdo do objeto). No modal, as posições já estão somadas ao canto do modal.

### Jogo (HUD)

| Elemento | Objeto no GDevelop | x | y | largura | altura | Observação |
|---|---|---|---|---|---|---|
| Chip FASE | PanelSprite "chip" + 2 Texto | 640 | 14 | 170 | 80 | rótulo 16 px, valor 36 px |
| Chip TEMPO | PanelSprite "chip" + 2 Texto | 826 | 14 | 170 | 80 | rótulo 16 px, valor 36 px |
| Chip RISCOS | PanelSprite "chip" + 2 Texto | 1012 | 14 | 170 | 80 | rótulo 16 px, valor 36 px |
| Chip ERROS | PanelSprite "chip" + 2 Texto | 1198 | 14 | 170 | 80 | rótulo 16 px, valor 36 px |
| Chip EQUIPE | PanelSprite "chip" + 2 PanelSprite "botao_normal" com Texto J1/J2 | 1384 | 14 | 170 | 80 |  |
| Botão Como jogar | PanelSprite "botao_*" (4 estados) + Texto | 1650 | 22 | 240 | 64 | texto 24 px, cor #09111a |
| Fundo do HUD | PanelSprite "painel" + faixa "faixa_alerta" (repetida) | 0 | 0 | 1920 | 120 | faixa: y=104, altura 16 |
| Logo | Sprite (capacete) | 24 | 16 | 72 | 72 |  |
| Título | Texto | 112 | 22 | — | — | 34 px, amarelo #ffd45c |
| Subtítulo | Texto | 112 | 66 | — | — | 20 px, cinza #8ea0b2 |

### Jogo (ficha)

| Elemento | Objeto no GDevelop | x | y | largura | altura | Observação |
|---|---|---|---|---|---|---|
| Painel da ficha | PanelSprite "painel" | 1440 | 120 | 480 | 960 | padding interno 24 |
| Título / subtítulo / missão | 3 Texto (30, 20, 20 px) | 1464 | 142 | 432 | — | missão com quebra de linha, largura 432 |
| Barra de progresso | 7 PanelSprite "linha_normal/resolvida" (margem 6) | 1464 | 372 | 432 | 16 | cada célula 54x16, passo 63 (ou use a barra Tiny Square existente) |
| 7 linhas de risco | PanelSprite "linha_*" + Sprite "caixa_*" + 2 Texto | 1464 | 410 | 432 | 48 | passo vertical 54; texto 20 px; tag NR 18 px |
| Dicas | 3 Sprite "dica_cheia/vazia" + botão | 1464 | 798 | 432 | 64 | lâmpadas 20x24, passo 34 |
| Saída / cadeado | PanelSprite "chip" + Sprite "cadeado_*" + 2 Texto | 1464 | 884 | 432 | 176 |  |
| 7 slots do código | PanelSprite "slot_vazio/cheio" + Texto | 1474 | 988 | 52 | 60 | passo 60; dígito 32 px |

### Jogo (cena)

| Elemento | Objeto no GDevelop | x | y | largura | altura | Observação |
|---|---|---|---|---|---|---|
| Cenário | Sprites (pacote HD) | 0 | 120 | 1440 | 960 | sem moldura; área clicável = riscos |

### Pergunta

| Elemento | Objeto no GDevelop | x | y | largura | altura | Observação |
|---|---|---|---|---|---|---|
| Fundo do modal | PanelSprite "painel_destaque" + faixa | 220 | 190 | 1000 | 780 | abre sobre a cena (escurecer a cena com um retângulo preto 60%) |
| Etapas 1/2/3 | 3 PanelSprite ("botao_normal" ativa, "chip" inativa) + Texto | 244 | 230 | 192 | 46 | passo 204; texto 20 px |
| Cartão do objeto | PanelSprite "chip" + Sprite do risco (miniatura) | 244 | 296 | 200 | 126 |  |
| Enunciado | Texto 32 px, quebra de linha em 700 px (até 2 linhas) | 476 | 300 | 700 | — | o texto de apoio (20 px) vem logo abaixo |
| 4 alternativas | PanelSprite "opcao_*" (5 estados) + letra + Texto 24 px | 244 | 440 | 952 | 72 | passo 80; texto com quebra em 828 px (até 2 linhas) |
| Retorno + próxima | PanelSprite "opcao_correta/errada" + Texto + botão | 244 | 764 | 952 | 182 | aparece embaixo das alternativas, no lugar de "Confirmar" |

**Botões do modal que mudam conforme o estado:**

| Elemento | Objeto | x | y | largura | altura | Observação |
|---|---|---|---|---|---|---|
| Confirmar resposta | PanelSprite "botao_desativado" → "botao_normal" | 896 | 894 | 300 | 56 | texto 20 px; fica desativado até escolher uma alternativa |
| Próxima pergunta | PanelSprite "botao_normal" | 882 | 774 | 300 | 52 | fica dentro da caixa de retorno, na linha do título "ISSO MESMO!" / "NÃO É ESSA" |
| Dica de uso | Texto 20 px | 244 | 910 | — | — | "Esc: fechar · Dica: no painel da direita" |
| Contador no topo | Texto 20 px, alinhado à direita em x=1196 | — | 242 | — | — | "RISCO 3 DE 7 · ERROS 1" |

**Painel final (vitória ou tempo esgotado):**

| Elemento | Objeto | x | y | largura | altura | Observação |
|---|---|---|---|---|---|---|
| Fundo | PanelSprite "painel_destaque" + faixa 976×14 em (372, 162) | 360 | 150 | 1200 | 780 | |
| Título | Texto 56 px, centralizado em x=960 | — | 210 | — | — | "SAÍDA LIBERADA!" (verde) ou "TEMPO ESGOTADO" (vermelho) |
| Mensagem | Texto 24 px, centralizado em x=960 | — | 290 | — | — | |
| 7 slots do código | PanelSprite "slot_cheio/vazio" + Texto 32 px | 754 | 340 | 52 | 60 | passo 60; slot vazio mostra "•" |
| Cadeado | Sprite "cadeado_aberto/fechado" | 948 | 416 | 24 | 28 | |
| Quadro de resultados | PanelSprite "chip" | 480 | 480 | 960 | 300 | 4 linhas de 26 px com passo 46 a partir de y=502, textos em x=520 e valores alinhados à direita em x=1400 |
| Pontuação | 2 Texto (28 px e 40 px) | 520 / 1400 | 716 / 710 | — | — | só aparece na vitória |
| Botões (vitória) | "botao_normal" JOGAR FASE 2 + "opcao_normal" VER RESUMO | 620 e 980 | 820 | 320 | 64 | |
| Botão (tempo esgotado) | "botao_normal" TENTAR NOVAMENTE | 790 | 820 | 340 | 64 | |

Veja `mockup_5_final_vitoria.png` e `mockup_6_final_tempo.png`.

## 5. Telas de exemplo

| Arquivo | Estado |
|---|---|
| mockup_1_jogo.png | jogo em andamento: HUD, cenário e ficha |
| mockup_2_pergunta.png | pergunta aberta, nenhuma alternativa escolhida |
| mockup_3_retorno_erro.png | alternativa errada: retorno em vermelho |
| mockup_4_retorno_certo.png | alternativa certa: retorno em verde e botão "Próxima pergunta" |
| mockup_5_final_vitoria.png | fim de jogo com o código completo |
| mockup_6_final_tempo.png | fim de jogo por tempo esgotado |

## 6. Fonte

Os mockups usam uma fonte monoespaçada comum só para simular. No jogo, use uma **fonte em pixel que tenha acentos** (as perguntas têm ç, ã, é, ô…). Boas opções gratuitas do Google Fonts: **Silkscreen**, **Press Start 2P** e **VT323**. Eu não consegui baixá-las daqui para testar, então antes de adotar uma, escreva "AÇÃO PREVENÇÃO ÉPOCA" num objeto de texto e veja se os acentos aparecem. Se algum sair quebrado, troque de fonte.

Dica: importe o arquivo .ttf como recurso de fonte do GDevelop (não use a fonte de bitmap .fnt, que costuma vir sem acentos), e use tamanhos de texto pares (16, 20, 24, 32…).

## 7. Perguntas

- `perguntas_fase1.json` tem os 7 riscos, cada um com 3 dicas e 3 perguntas (risco → NR → medida) de 4 alternativas, mais o retorno de cada alternativa. É o formato mais fácil de carregar no jogo depois.
- `PERGUNTAS_fase1.md` é a mesma coisa em texto legível, com a lista "Conferir com o grupo" no começo.
- O texto mais longo de cada tela cabe nas caixas: alternativas até 116 caracteres, retorno até 214, dicas até 97.
- A alternativa certa muda de posição entre A, B, C e D, para não virar padrão.

## 8. Decisões que tomei (mude se o grupo preferir)

- **Botão "Confirmar resposta":** o jogador escolhe a alternativa, depois confirma, porque a ideia é a dupla conversar antes de responder. Quando confirma, as alternativas travam e a caixa de retorno aparece no lugar do botão. Se preferirem resposta imediata ao clicar, é só tirar o botão.
- **Resposta errada mostra a certa:** depois de confirmar, a alternativa correta fica verde e a escolhida errada fica vermelha, junto com o retorno. Assim o grupo aprende na hora e a pergunta não precisa se repetir. Se quiserem tentar de novo até acertar, é só não revelar a verde.
- **Pontuação provisória:** 1000 − 50 por erro − 100 por dica usada. É um valor de exemplo para o painel final; o grupo decide a regra.
- **Pixel de 2 px e 1920×1080:** se mudarem de ideia, o kit precisa ser redesenhado, porque os painéis foram desenhados na grade de 2 px.
