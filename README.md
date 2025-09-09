# Robô Híbrido OB

Robô para previsão do custo do dólar (USD) com abordagem híbrida, combinando sinais técnicos e modelos de aprendizado de máquina. O objetivo é gerar previsões de curto e médio prazo para auxiliar em decisões de hedge, câmbio e negociação algorítmica.

> Aviso legal: Este projeto é educacional e experimental. Não constitui recomendação de investimento. Use por sua conta e risco.

## Principais recursos

- Sinais técnicos: médias móveis, RSI, bandas de Bollinger, volatilidade implícita/realizada (quando disponível)
- Modelos preditivos: regressão, ensembles e/ou redes neurais (a definir conforme evolução)
- Pipeline de dados: ingestão, limpeza, engenharia de atributos e validação
- Backtesting básico: avaliação fora da amostra com métricas padronizadas
- Exportação de previsões e gráficos

## Arquitetura (visão geral)

1. Coleta de dados de USD/BRL (spot ou futuros) a partir de provedores públicos/privados
2. Pré-processamento e engenharia de atributos (features) técnico-estatísticos
3. Treinamento e validação de modelos (time series split)
4. Geração de previsões e cálculo de incerteza (intervalos, quantis ou ensembles)
5. Avaliação contínua e registro de métricas

> Nota: O sufixo "OB" do nome pode referir-se a uma futura integração com Order Book (livro de ofertas). No estágio atual, a arquitetura é genérica e extensível para incluir esse componente quando disponível.

## Instalação

### Requisitos

- Python 3.10+ (recomendado 3.11)
- `git` e `make` (opcionais, mas úteis)
- Docker (opcional, para ambiente isolado)

### Passos rápidos (Python)

```bash
git clone <URL_DO_REPOSITORIO>
cd Robo-Hibrido-OB
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\\Scripts\\activate
pip install --upgrade pip
# Quando o arquivo de dependências for adicionado:
# pip install -r requirements.txt
```

### Passos rápidos (Docker, opcional)

```bash
# Quando o Dockerfile estiver disponível:
docker build -t robo-hibrido-ob .
docker run --rm -it --env-file .env robo-hibrido-ob
```

## Configuração

Utilize variáveis de ambiente via arquivo `.env` (exemplo abaixo). Ajuste conforme o provedor de dados e seu fluxo.

```dotenv
# Símbolo base (ex.: USD/BRL)
SYMBOL=USD/BRL

# Janela de histórico (em dias)
HISTORY_DAYS=365

# Provedor de dados (ex.: yfinance, alphavantage, quandl)
DATA_PROVIDER=yfinance
DATA_API_KEY=

# Parâmetros de treino/validação
TEST_SIZE_DAYS=60
CV_FOLDS=5
SEED=42

# Saídas
OUTPUT_DIR=./outputs
```

> Dica: não commitar `.env` no repositório. Use `.env.example` como modelo público.

## Dados

- Fontes possíveis: Yahoo Finance (`yfinance`), Alpha Vantage, Quandl, CME (futuros), provedores pagos
- Frequência: intradiária ou diária, conforme objetivo e disponibilidade
- Cuidados: timezone, datas de feriado, rolagem de contratos, missing values e outliers

## Uso (exemplos conceituais)

Enquanto os módulos forem implementados, o fluxo típico deverá ser algo como:

```bash
# 1) Baixar e preparar dados
python -m src.data.prepare --symbol "USD/BRL" --provider yfinance

# 2) Treinar modelo
python -m src.models.train --config configs/base.yaml

# 3) Gerar previsões
python -m src.models.predict --horizon 5 --output ./outputs

# 4) Rodar backtest
python -m src.backtest.run --horizon 5 --metric mape rmse
```

> Os comandos acima são um guia inicial. Os módulos/arquivos citados serão adicionados gradualmente.

## Estrutura sugerida

```
Robo-Hibrido-OB/
  ├── src/
  │   ├── data/            # ingestão, limpeza, features
  │   ├── models/          # treino, predição, persistência
  │   ├── backtest/        # avaliação, métricas, relatórios
  │   ├── utils/           # helpers, logs, io
  │   └── main.py          # entrypoint (opcional)
  ├── configs/             # YAML/JSON de parâmetros
  ├── notebooks/           # EDA e protótipos
  ├── outputs/             # previsões, gráficos, checkpoints
  ├── tests/               # testes unitários
  ├── requirements.txt     # dependências (alternativa: Poetry/PDM)
  ├── .env.example         # modelo de configuração
  └── README.md
```

## Métricas e avaliação

- Regressão: RMSE, MAE, MAPE, sMAPE, R²
- Direcional: acurácia direcional, matriz de confusão binária
- Série temporal: avaliação por janelas rolantes, walk-forward, backtest fora da amostra

## Qualidade e boas práticas

- Formatação e lint: `black`, `ruff` ou `flake8`
- Tipagem: `mypy`
- Testes: `pytest` com cobertura mínima
- Reprodutibilidade: seeds fixos, versionamento de dados/modelos quando possível

## Roadmap

- [ ] Definir fonte de dados padrão (ex.: `yfinance`) e criar pipeline mínimo
- [ ] Implementar engenharia de atributos técnicos (MAs, RSI, BBands)
- [ ] Treinar primeiro baseline (ex.: regressão, XGBoost)
- [ ] Criar rotina de backtest básica
- [ ] Persistir e carregar modelos (joblib ou similar)
- [ ] Exportar previsões e gráficos
- [ ] Documentar exemplos reais de uso
- [ ] (Opcional) Integrar Order Book e sinais microestruturais

## Contribuição

Contribuições são bem-vindas! Abra uma issue para discutir ideias, bugs ou melhorias. Para alterações maiores, proponha um PR descrevendo motivação, abordagem e impactos.

## Licença

Defina a licença do projeto (ex.: MIT, Apache-2.0). Se não tiver certeza, mantenha o repositório privado até decidir.

## Aviso legal (importante)

Este projeto não garante resultados e não é aconselhamento financeiro. Mercados são voláteis e imprevisíveis. Faça sua própria análise e gerencie riscos adequadamente.
