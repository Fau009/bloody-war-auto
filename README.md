# Bloody War Auto — v0.1

## Como instalar
1. Abra `chrome://extensions`
2. Ative o "Modo do desenvolvedor" (canto superior direito)
3. Clique em "Carregar sem compactação"
4. Selecione esta pasta (a que tem o `manifest.json`)
5. Abra `https://www.thebloodywar.com/` em uma aba e faça login
6. Clique no ícone da extensão pra abrir o popup, ligue o toggle mestre,
   ative PDB e/ou PDL, e clique em "Editar alvo/filtro" pra configurar

## O que já funciona
- Leitura em tempo real dos slots PDB/PDL (contagem e contador de recarga)
- Loop de checagem a cada 15s, respeitando modo Contínuo ou Agendado
  (com janela de horário independente por sistema)
- **Desligamento automático por tempo**: ao ligar a automação, marque
  "Desativar automaticamente?" e defina os minutos — a extensão desliga
  sozinha mesmo com o popup fechado (usa `chrome.alarms`, não um timer
  da página). O contador regressivo aparece no popup.
- Fluxo de ataque a Criaturas: navega até Mapa Mundo → acha a região pelo
  nome → entra → acha a criatura pelo nome → clica Atacar → volta ao mapa
- Fluxo de ataque a Batalhas: navega até Batalhas → aplica o filtro
  (campo + valor de busca) → clica Filtrar → ataca o primeiro resultado
- Log das últimas 30 ações, visível no popup

## Pontos a testar/confirmar antes de usar de verdade

1. **Valor exato do filtro "Tipo de Oponente"** — não capturamos o HTML
   com esse filtro preenchido. Abra o jogo, selecione "Tipo de Oponente"
   no dropdown, digite algo no campo de busca e veja o que retorna
   resultados (`Bot`, `Bots`, `Real`, `Player`, etc.) — ajuste em
   Configurações → Batalhas → Valor buscado.

2. **Navegação em páginas de Mapa/Batalha fora da sidebar padrão** —
   quando o layout usa o botão "Menu" (em vez da sidebar sempre visível),
   o código tenta clicar nesse botão e depois no item do menu, mas não
   temos o HTML do menu já aberto pra confirmar o seletor certo. Se a
   navegação falhar, abra o console (F12) na aba do jogo e veja os logs
   `[BloodyWarAuto]` pra saber onde travou.

3. **Slots máximos e tempo de recarga** — os valores padrão em
   `shared.js` (`DEFAULT_CONFIG`) são baseados no que vimos nos seus
   prints. Ajuste em Configurações se sua conta tiver valores diferentes.

4. **Seletores dependem da versão atual do jogo** (classes geradas por
   uma lib chamada HeroUI/NextUI, com hashes que podem mudar em updates
   do jogo). Se algo parar de funcionar, o primeiro passo é comparar o
   HTML atual da página com os seletores em `content.js`.

5. **Alarme de desligamento automático e reinício do navegador** — o
   `chrome.alarms` normalmente sobrevive a fechar/abrir o Chrome, mas
   isso não é 100% garantido em todo sistema operacional. Se o PC for
   reiniciado com o timer ativo, vale conferir o popup ao voltar.

## Estrutura dos arquivos
- `manifest.json` — configuração da extensão
- `shared.js` — config padrão + helpers de storage (usado por popup,
  options, background e content)
- `content.js` — roda dentro do jogo: lê slots, decide quando atacar,
  executa os cliques
- `background.js` — só cria a config padrão na instalação
- `popup.html/js/css` — painel rápido (liga/desliga, status, log)
- `options.html/js/css` — tela de configuração dos alvos/filtros
