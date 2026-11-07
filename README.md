# Câmbio brasileiro: fatores externos ou domésticos?

O que pesa mais sobre o real: o cenário internacional (juros americanos, aversão global ao risco,
força do dólar) ou o Brasil (Selic, risco-país, expectativas fiscais)?

**Resposta curta: depende do período.** Na média de 2009–2026, cerca de **60% da variação explicada**
do câmbio vem de choques externos — mas, nas janelas que incluem a crise fiscal e política de
2015–2018, os choques domésticos passaram a dominar.

![Peso dos fatores externos ao longo do tempo](resultados/figuras/3_peso_externo_no_tempo.png)

## Principais resultados

| Método | Externo | Doméstico | Não explicado | Externo / explicado |
|---|---|---|---|---|
| Regressão ingênua (sobreposição de 30% sem dono) | 7% | 22% | 41% | — |
| Regressão com risco-país "limpo" | 37% | 22% | 41% | 63% |
| VAR, FEVD h = 12 meses | 38% | 22% | 39% | 63% |

*Frações da variância mensal do câmbio (jan/2009–out/2026, 214 meses).*

![Comparação dos métodos](resultados/figuras/1_comparacao_metodos.png)

- **O canal externo é o dólar global, não os juros americanos.** O dólar responde por 28% da
  variância do câmbio; o Treasury de 10 anos, por menos de 1%.
- **O canal doméstico é o risco-país (CDS).** Selic e expectativa fiscal quase não aparecem na
  variação mensal — o mercado já as antecipa.
- **Quase metade do efeito do dólar global chega ao real via risco-país**: o CDS brasileiro sobe
  quando o dólar se fortalece no mundo.
- **O peso externo varia no tempo** (janelas de 5 anos, datadas pelo fim):

  | Janelas terminando em | Peso externo no explicado |
  |---|---|
  | dez/2013 – dez/2015 | 79% |
  | jan/2016 – jul/2018 | 64% |
  | ago/2018 – ago/2023 | 47% |
  | set/2023 – out/2026 | 60% |

## Método

Modelo de partida:

$$\Delta\ln(\text{USD/BRL})_t = \alpha + \beta_1 X_t + \beta_2 B_t + \varepsilon_t$$

com $X_t$ = variáveis externas e $B_t$ = domésticas.

| Notebook | Etapa |
|---|---|
| [01_ambiente](01_ambiente.ipynb) | Teste de conexão com BCB e FRED |
| [02_coleta](02_coleta.ipynb) | Coleta: SGS/BCB, Focus, FRED, IPEADATA, CDS |
| [03_tratamento](03_tratamento.ipynb) | Frequência mensal, log-diferenças, Focus em horizonte fixo, testes ADF/KPSS |
| [04_regressao](04_regressao.ipynb) | MQO padronizado com erros HAC; decomposição do R² |
| [05_limpeza_risco](05_limpeza_risco.ipynb) | Separação do risco-país em contágio global e componente doméstico |
| [06_var](06_var.ipynb) | VAR(1), identificação de Cholesky, FEVD em 1, 3, 6 e 12 meses |
| [07_janelas_moveis](07_janelas_moveis.ipynb) | FEVD em janelas móveis de 60 e 84 meses |
| [08_resultados](08_resultados.ipynb) | Gráficos finais e tabela-resumo ([resumo.xlsx](resultados/resumo.xlsx)) |

**Variáveis** (mensais, último dia útil):

| Bloco | Variável | Fonte | Transformação |
|---|---|---|---|
| Dependente | PTAX de venda | SGS/BCB, série 1 | 100·Δln |
| Externa | Treasury 10 anos | FRED, DGS10 | Δ |
| Externa | VIX | FRED, VIXCLS | 100·Δln |
| Externa | Dólar vs. economias avançadas | FRED, DTWEXAFEGS | 100·Δln |
| Doméstica | Meta Selic | SGS/BCB, série 432 | Δ |
| Doméstica | CDS 5 anos Brasil | Investing.com | 100·Δln |
| Doméstica | Resultado primário esperado (12 meses à frente) | Focus/BCB | Δ |

**Identificação.** Ordenação de Cholesky: Treasury → VIX → dólar → Selic → primário → CDS → câmbio.
Externas primeiro reflete a hipótese de economia pequena e aberta (o mundo afeta o Brasil no
mesmo mês, não o contrário). O teste de Granger não encontra evidência de que o bloco doméstico
antecipe o externo (p = 0,18). Invertendo os blocos, o peso externo cairia de 63% para 12% — **a conclusão
depende dessa hipótese**.

## Limitações

- A amostra começa em 2009: o CDS disponível tem preços parados durante a maior parte de 2008.
- Commodities, fluxos de capital e intervenções do BC estão fora do modelo (~40% não explicado).
- As janelas móveis não têm intervalos de confiança; leia tendências, não oscilações de um mês.

## Como reproduzir

1. Criar o ambiente:
   ```bash
   conda env create -f environment.yml
   conda activate cambio
   ```
2. Criar uma chave gratuita da API do FRED em <https://fred.stlouisfed.org> (*My Account → API Keys*)
   e guardá-la na variável de ambiente `FRED_API_KEY`.
3. Baixar manualmente o histórico diário do **CDS 5 anos do Brasil** no
   [Investing.com](https://www.investing.com/rates-bonds/brazil-cds-5-years-usd-historical-data)
   e salvar como `dados/brutos/cds_investing.csv`.
4. Rodar os notebooks em ordem, de `01` a `08`.

Os dados brutos não estão no repositório: as séries públicas são baixadas pelo `02_coleta`, e o CSV
do CDS e o VIX têm restrições de redistribuição. Como as fontes revisam dados, uma nova coleta pode
gerar números ligeiramente diferentes dos apresentados aqui (coleta em 07/11/2026).
