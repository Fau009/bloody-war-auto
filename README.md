# ⚔ Bloody War Auto — Chrome

Extensão para **Google Chrome** que automatiza o jogo [The Bloody War](https://www.thebloodywar.com/) — um RPG de combate por navegador.

> Versão Firefox disponível em: [bloody-war-auto-firefox](https://github.com/Fau009/bloody-war-auto-firefox)

---

## Objetivo

Automatizar as tarefas repetitivas do jogo para que o personagem continue evoluindo mesmo sem o jogador presente:

- Atacar criaturas e outros jogadores enquanto houver slots disponíveis
- Trabalhar nos horários programados para acumular gold
- Treinar atributos automaticamente com o gold disponível
- Manter a vida alta comprando e usando poções quando necessário
- Registrar tudo em um painel de relatórios com gráficos e histórico de sessões

---

## Instalação no Chrome

1. Baixe ou clone este repositório
2. Abra o Chrome e acesse `chrome://extensions`
3. Ative o **"Modo do desenvolvedor"** (canto superior direito)
4. Clique em **"Carregar sem compactação"**
5. Selecione a pasta deste projeto (a que contém o `manifest.json`)
6. O ícone ⚔ aparecerá na barra de extensões

> Para fixar o ícone na barra, clique no 🧩 e depois no 📌 ao lado da extensão.

---

## Como usar

### 1. Abrir o painel
Clique no ícone ⚔ da extensão. O popup mostra o status do personagem, slots e ações recentes.

### 2. Ligar o sistema
Clique no interruptor no topo do popup. Quando estiver verde, o bot está ativo e rodando a cada 15 segundos.

### 3. Configurar os alvos
Cada seção do popup tem um link **"Editar →"** que abre a página de configurações diretamente na aba certa:

| Seção | O que configura |
|---|---|
| **Criaturas** | Região do mapa e nome exato da criatura alvo |
| **Batalhas** | Filtros de busca (tipo de oponente ou nível) em cascata |
| **Trabalho** | Duração do turno e horários de agendamento |
| **Treinamento** | Atributos a treinar e reserva mínima de gold |
| **Saúde** | Poção a comprar, limite de HP e reserva de gold |

### 4. Atalho de configuração
O ícone **⚙** no canto superior direito do popup abre as configurações completas.

### 5. Desligamento automático
Marque **"Desativar automaticamente?"** e defina os minutos — a extensão se desliga sozinha mesmo com o popup fechado.

---

## Painel de Relatórios

Acesse pela aba **📊 Relatórios** nas configurações. Disponível após acumular histórico de uso.

### KPIs
Total de ações, taxa de vitória, batalhas vs criaturas, gold ganho, gold gasto e gold líquido.

### Gráficos
- Vitórias vs Derrotas (rosca)
- PDL vs PDB (rosca)
- Distribuição por tipo de atividade (rosca)
- Ações por hora do dia (barras)
- Evolução do gold ao longo do tempo (linha)
- Gold líquido por hora do dia (barras)
- Ações por dia da semana (barras)
- Taxa de vitória PDL vs PDB (barras)
- Gold médio por combate (barras)

### Histórico de Sessões
Cada sessão registra o período de ativação até desativação do sistema, com:
- Horário de início e fim
- Duração total
- Breakdown de atividades: batalhas, criaturas, treinos, poções, trabalhos
- Vitórias, derrotas e gold líquido da sessão

---

## Estrutura dos arquivos

| Arquivo | Função |
|---|---|
| `manifest.json` | Configuração da extensão (Manifest V3, Chrome) |
| `shared.js` | Config padrão e helpers de storage compartilhados |
| `content.js` | Roda dentro do jogo — lê slots e executa os cliques |
| `background.js` | Service Worker — gerencia alarmes de auto-disable |
| `popup.html/js/css` | Painel rápido (liga/desliga, status, log recente) |
| `options.html/js/css` | Configurações completas e painel de relatórios |
| `reports.js` | Motor de gráficos canvas e análise de dados |

---

## Observações

- O histórico fica salvo localmente no navegador (não é enviado a nenhum servidor)
- Se desinstalar a extensão ou limpar os dados do navegador, o histórico é perdido
- Os seletores do `content.js` dependem da versão atual do jogo — se algo parar de funcionar, compare o HTML do jogo com os seletores e abra uma issue
