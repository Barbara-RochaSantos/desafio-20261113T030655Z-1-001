# Entrega — Consolidador de Reservas Onfly

## Defeitos Corrigidos e Causa Raiz

1. **Conversão de câmbio (`src/onfly_report/services/currency_converter.py`)**
   - **Causa raiz:** O cálculo dividia o valor pela taxa usando em vez de multiplicar usando Decimal, além disso usava castings desnecessários que causavam imprecisão no cálculo.

2. **Política de status faturável (`src/onfly_report/services/status_policy.py`)**
   - **Causa raiz:** A regra status != "CANCELLED" incluía reservas NO_SHOW nos totais financeiros e de noites, quando apenas CONFIRMED deveria ser faturado.
   
3. **Ordenação do ranking de hotéis (`src/onfly_report/services/report_builder.py`)**
   - **Causa raiz:** A ordenação padrão do sorted() classificava top_hotels pelo faturamento em ordem crescente em vez de decrescente

4. **Cálculo de noites (`src/onfly_report/services/stay_calculator.py`)**
   - **Causa raiz:** A subtração usava apenas o dia do mês (checkout.day - checkin.day), gerando valores negativos na virada de mês/ano em vez de usar (checkout - checkin).days.

## Abordagem e Investigação

1. Execução da suíte `pytest -q` para mapear as falhas iniciais.
2. Inspeção isolada de cada classe (`currency_converter`, `status_policy`, `report_builder` e `stay_calculator`) juntamente com execução de debbugs para me ajudar a investigar os arquivos onde estavam a regra de negócio com erro.

## Resultado Pytest
.....                                                                    [100%]
5 passed in 4.79s

## Como Validar e Resultado dos Testes

```bash
PYTHONPATH=src python -m onfly_report --input data/reservas_hoteis.csv --rates data/taxas_cambio.csv --output output/report.json
pytest -v


