# A Lenda do Dragao Bug (RPG de texto em Python)

Somente biblioteca padrao (Python 3.8+; testado em 3.12).

## Como jogar e testar

    python rpg.py                  # jogar
    python rpg.py --treinar 20000  # treinar a IA (Q-Learning) por simulacao, sem percorrer a campanha
    python rpg.py --rapido         # sem pausas
    python rpg.py --semente 42     # semente fixa (mesma semente + mesmas entradas = mesmo jogo)
    python test_rpg.py             # 103 testes automatizados

## Arquivos

| arquivo | conteudo |
|---|---|
| `rpg.py` | jogo: telas, combate, IA, eventos, dialogos, lojas, viagens, salvamento |
| `rpg_mundo.py` | logica pura do mundo (sem entrada/saida): tempo, NPCs, memoria, afinidade, rumores, faccoes, missoes, estoques, eventos sociais, serializacao |
| `rpg_dados.py` | dados estaticos: itens, inimigos, locais, rotas, NPCs, relacoes, lojas, missoes, rumores, mudancas, eventos sociais |
| `test_rpg.py` | testes (unittest) |
| `backup/rpg_antes_da_expansao.py` | versao anterior, preservada |

## Principais sistemas

- **Mundo**: 10 locais ligados por 10 rotas (grafo em `ROTAS`). Rotas podem exigir nivel/item, estar fechadas
  ate um fato (ponte, portao) ou ocultas ate uma descoberta. Nao ha teletransporte.
- **Tempo**: manha/tarde/noite. Explorar, viajar (1 periodo por trecho), dormir e investigar gastam tempo.
- **NPCs** (43): rotina por periodo, humor, memorias estruturadas (max. 8 por NPC), afinidade, relacoes entre NPCs.
- **Dialogos**: 7 tons + rumores + "sobre outra pessoa" + "acontecimentos recentes". A reacao depende da
  personalidade (18 arquetipos), humor, memoria, horario e estado do mundo.
- **Rumores**: propagam por rotas abertas (com mutacao), podem ser investigados, espalhados e desmentidos.
- **Faccoes** (5) e reputacao por cidade, com efeito em precos, dialogos e missoes.
- **Missoes** (17): objetivos tipados, recompensas, alternativas com custo/consequencia, missoes ocultas.
- **Economia**: 17 mercadores com estoque limitado que repoe; precos reagem a escassez, afinidade, faccao, feira.
- **Itens**: 76 itens, 5 raridades, 7 slots, efeitos temporarios, aprimoramento (+3).
- **Eventos**: 40 eventos com raridade e condicoes, escolhidos pelo Diretor de Eventos; 9 eventos sociais.
- **Companheiros** (Bia, Ze, Tobias): um por vez, julgam suas escolhas, ajudam em combate e podem sair.
- **Mudancas permanentes**: pontes, portoes, estoques, NPCs que se mudam e novos moradores (persistem no save).
- **Combate/IA**: classes, inimigos, Dragao Bug (agora com 3 fases), fraquezas por tipo de dano, Q-Learning.

## Salvamento

`rpg_save.json` (schema 2). Saves antigos validos carregam com valores padrao nos campos novos.
Saves corrompidos geram mensagem clara e o arquivo e preservado como `.corrompido`.
Um save existente nunca e sobrescrito em silencio (vira `.anterior`).
