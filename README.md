# steam-review-retention

Trabalho da disciplina de Data Science, Bacharelado em Ciência da Computação, PUCPR (2026-2), Prof. Rayson Laroca.

## Pergunta de pesquisa

A partir do conteúdo de um review publicado na Steam e do contexto em que ele foi escrito (perfil do autor, tempo de jogo até aquele momento, forma de aquisição do jogo e estágio de desenvolvimento), é possível prever se o jogador continuará jogando após publicar a avaliação?

Classificação binária. O alvo `continued` vale 1 quando o autor jogou pelo menos 2 horas depois de publicar o review, o mesmo limiar da política de reembolso da Steam.

## Dados

Reviews coletados diretamente da API pública da Steam (`store.steampowered.com/appreviews`) em 14 de setembro de 2026.

Amostragem por jogo: entre os 2.000 jogos com mais donos (ranking do SteamSpy), ficaram os que têm de 10 mil a 60 mil reviews em inglês. Desses, foram sorteados 30 (seed 42) e, de cada um, coletados os 5.000 reviews em inglês mais recentes publicados antes de 1º de janeiro de 2026. Total: 150.000 reviews, 149.319 após limpeza. O corte garante uma janela mínima de oito meses entre o review e a coleta, necessária para observar se o jogador voltou a jogar.

Limitação: a API só devolve reviews do mais novo para o mais antigo, então a amostra de cada jogo não é aleatória e concentra-se em 2024 e 2025.

## Arquivos

| Arquivo | O que é |
|---|---|
| `tde-data-science-parte01.ipynb` | Notebook da entrega: preparação, descrição estatística, análises univariada e multivariada, final plots |
| `steam_reviews_data.csv` | Amostra usada pelo notebook (150.000 linhas, 57 MB) |
| `gerar_amostra_api.ipynb` | Notebook auxiliar que gera o CSV a partir da API. Não faz parte da entrega |
| `jogos_sorteados_2026.csv` | Os 30 jogos aceitos na amostra |
| `jogos_descartados_2026.csv` | Jogos sorteados e descartados, com o motivo |
| `amostra_2026_meta.json` | Data da coleta, corte, seed e limites usados |

A pasta `raw/` (JSON bruto por jogo) não é versionada. Ela é recriada ao rodar `gerar_amostra_api.ipynb`.

## Como rodar

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

O notebook da entrega lê apenas `steam_reviews_data.csv` na mesma pasta. Não acessa a rede.

Para regenerar a amostra do zero, rode `gerar_amostra_api.ipynb`. A coleta leva de 40 a 90 minutos e pode ser interrompida e retomada.
