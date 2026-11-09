# Câmbio brasileiro: fatores externos ou domésticos?

## A pergunta

O que mexe mais com o real: o que acontece **lá fora** (juros americanos, aversão global ao risco,
força do dólar no mundo, preço das commodities) ou o que acontece **aqui dentro** (Selic, risco-país,
expectativas fiscais)?

A pergunta surgiu numa discussão na faculdade. Cada lado tinha um episódio para defender, a crise
de 2015, a eleição de 2018, a Covid, e ninguém conseguia fechar a questão. Chegando em casa,
resolvi testar com dados.

**Resposta curta: o mundo pesa mais, mas quanto depende do período.** Sob a hipótese de pequena
economia aberta, e atribuindo ao bloco externo a parcela de explicação compartilhada,
aproximadamente **72% da variação cambial explicada pelo modelo está associada a fatores externos**
(2009 a 2026). Essa predominância, entretanto, não é constante: em episódios de deterioração
doméstica, a contribuição relativa dos fatores internos aumenta de forma expressiva, chegando a
quase empatar com a dos externos.

![Peso dos fatores externos ao longo do tempo](resultados/figuras/6_post_peso_exterior.png)

## Principais conclusões

1. **Na média, o mundo pesa mais.** Dos ~61% da variação mensal do câmbio que o modelo explica,
   cerca de 70% vêm de fora e 30% de dentro.
2. **Os canais externos são o dólar global e as commodities, não os juros americanos.** Quando o
   dólar se fortalece contra as moedas desenvolvidas, o real cai junto; quando as commodities sobem,
   o real se valoriza. São as duas variáveis externas de maior efeito no modelo.
3. **O juro americano de 10 anos não tem efeito significativo.** A intuição do *carry trade* (juro
   americano sobe, capital sai do Brasil) não aparece porque o movimento é ambíguo: em 6 dos 8
   piores meses do real, os juros americanos *caíram*. Nas crises, investidores compram Treasuries
   (fuga para a qualidade) ao mesmo tempo em que vendem emergentes.
4. **O canal doméstico é o risco-país.** Selic e expectativa fiscal não têm efeito significativo na
   variação mensal, provavelmente porque o mercado já as antecipa.
5. **O risco-país não é só "nosso".** Pelo menos 38% da variação mensal do CDS brasileiro é contágio
   global, e cerca de 40% do efeito do dólar global e das commodities chega ao real **através** do
   risco-país.
6. **A divisão muda no tempo** (janela móvel de 5 anos, datada pelo fim):

   | Janelas terminando em | Peso externo no explicado | O que estava dentro da janela |
   |---|---|---|
   | dez/2013 – dez/2015 | 86% | Pós-crise de 2008, liquidez global abundante |
   | jan/2016 – jul/2018 | 76% | Crise fiscal e política de 2015–16 entrando |
   | ago/2018 – ago/2023 | 54% | Recessão, eleição de 2018, furo do teto de gastos em 2021 |
   | set/2023 – out/2026 | 78% | As crises domésticas saem da janela |

   O ponto mais baixo é **45% (janela terminando em nov/2021)**, que cobre a eleição de 2018, a
   resposta fiscal à Covid e o furo do teto de gastos. Com intervalos de confiança de 95%
   (bootstrap), o mundo pesa mais **com folga** nas janelas de 2013–2016 e de 2024–2026; nas janelas
   de 2018 a 2023, o intervalo inclui os 50%, ou seja, os dados não distinguem os dois blocos. Em
   nenhuma janela o Brasil pesa mais de forma estatisticamente clara.

## O que olhei (e por quê)

Usei dados **mensais**, de jan/2009 a out/2026 (214 meses), sempre o valor do último dia útil do mês.

| Bloco | Variável | Fonte | Por que esta |
|---|---|---|---|
| Câmbio | PTAX de venda | BCB (SGS 1) | Taxa oficial de referência, usada em contratos e derivativos |
| Externa | Treasury 10 anos | FRED (DGS10) | Juros americanos de referência |
| Externa | VIX | FRED (VIXCLS) | Aversão global ao risco ("índice do medo") |
| Externa | Dólar vs. economias avançadas | FRED (DTWEXAFEGS) | Força global do dólar **sem** incluir o real na cesta |
| Externa | Bloomberg Commodity Index | Investing.com | Preço das commodities. O real é uma "moeda de commodities": o Brasil exporta soja, minério e petróleo |
| Doméstica | Meta Selic | BCB (SGS 432) | Política monetária |
| Doméstica | CDS 5 anos do Brasil | Investing.com | Risco-país, cotado diariamente até hoje |
| Doméstica | Resultado primário esperado | Focus/BCB | Expectativa fiscal do mercado |

## O que deixei de fora (e por quê)

| Ficou de fora | Motivo |
|---|---|
| **Crise de 2008** | O CDS disponível tem preços parados (sem negociação) durante quase todo 2008, por isso comecei a amostra em 2009 |
| **EMBI+** como risco-país | Descontinuado em jul/2024. Usei só para validar o CDS: as variações mensais dos dois têm correlação de 0,84 |
| **DXY** e índice amplo do dólar (Broad) | O DXY tem pesos de 1973 (o euro pesa ~58%); o Broad **inclui o real**, o que criaria correlação mecânica com a variável explicada |
| **Índice de commodities do FMI** | É a média do mês, e as outras variáveis são do último dia. Esse descompasso enfraquecia o sinal (correlação com o câmbio de −0,29, contra −0,57 do BCOM diário) |
| **Dívida líquida** como variável fiscal | O Brasil é credor líquido em dólar: quando o real cai, a dívida líquida cai sozinha. Ela seria efeito do câmbio, não causa |
| **Dívida bruta** | Só existe no Focus desde 2018, curto demais para janelas de 5 anos |
| **China, fluxo de capital, intervenções do BC** | Ficaram para uma próxima versão. São os candidatos naturais para reduzir os ~39% não explicados |
| **Dados diários ou semanais** | Mais ruído e problemas de horário (a PTAX fecha às 13h de Brasília; o VIX no fechamento de Nova York). Selic e Focus têm ritmo mensal |

## Metodologia

**1. Tratamento** ([03_tratamento](03_tratamento.ipynb)). Transformei preços e índices (câmbio, dólar,
commodities, VIX, CDS) em variação percentual (100·Δln) e taxas e % do PIB (Treasury, Selic,
primário) em diferença simples. Converti a expectativa fiscal para um **horizonte fixo de 12 meses**:
sem isso, a série "salta" toda virada de ano, quando o Focus troca o ano de referência. Os testes ADF
e KPSS confirmam que todas as variações são estacionárias.

**2. Regressão** ([04_regressao](04_regressao.ipynb)). Estimei por MQO, com variáveis padronizadas
(cada coeficiente é o efeito de um choque de 1 desvio-padrão) e erros-padrão robustos HAC
(Newey-West), três versões: só externas, só domésticas e todas juntas.

**3. O problema da sobreposição.** Somando o que cada bloco explica sozinho, passa-se do total: **35%
da variação é explicada pelos dois blocos ao mesmo tempo**, porque o CDS anda junto com o dólar
global, as commodities e o VIX. Sem resolver isso, não dá para dizer quanto é externo.

| Método | Externo | Sobreposição | Doméstico | Não explicado | Externo / explicado |
|---|---|---|---|---|---|
| Regressão ingênua | 9% | 35% | 17% | 39% | indefinido |
| Regressão com risco-país "limpo" | 44% | 0% | 17% | 39% | **72%** |

*Cada linha divide 100% da variância mensal do câmbio. A última coluna ignora o não explicado.*

![Comparação dos métodos](resultados/figuras/1_comparacao_metodos.png)

**4. Limpeza do risco-país** ([05_limpeza_risco](05_limpeza_risco.ipynb)). Regredi cada variável
doméstica nas quatro externas e usei o **resíduo** como componente doméstico: a parte que as
externas incluídas não explicam. Isso atribui a sobreposição ao bloco externo e reflete a hipótese
de **economia pequena e aberta**: no curto prazo, o mundo afeta o Brasil, e não o contrário.
Se eu fizesse a limpeza no sentido inverso, o peso externo cairia de 72% para 15%. Ou seja, **a
conclusão depende dessa hipótese**, que é teórica, mas tem apoio empírico (ver Validação).

![Efeito de cada variável](resultados/figuras/2_efeito_por_variavel.png)

**5. Janelas móveis** ([06_janelas_moveis](06_janelas_moveis.ipynb)). Refiz a regressão limpa em
cada janela de 60 meses (e 84 meses, como robustez), deslizando um mês por vez. Cada ponto é datado
no **fim** da janela. Consequência importante para a leitura: um episódio afeta o gráfico por cinco
anos e **sai** de uma vez. O salto de out/2023, por exemplo, não vem de nada que aconteceu em 2023,
e sim da eleição de 2018 deixando a janela.

Para o intervalo de confiança da janela de 60 meses, usei **bootstrap em blocos**: em cada janela,
reamostrei os meses em blocos de 6 meses seguidos, refiz a análise inteira e repeti 500 vezes. A
faixa azul do gráfico vai do percentil 2,5 ao 97,5. Os intervalos são largos (cerca de 43 pontos
percentuais em média), o esperado para uma razão estimada com apenas 60 meses.

![Peso externo no tempo, versão técnica](resultados/figuras/3_peso_externo_no_tempo.png)

**Validação.** Na pasta [extra_var/](extra_var/), coloquei uma outra abordagem recomendada e implementada com auxílio teórico e prático da IA. Um VAR com decomposição da variância (FEVD),
um método diferente que chega a ~71% de peso externo no explicado e às mesmas fases. O teste de
Granger feito lá não encontra evidência de que o bloco doméstico antecipe o externo (p = 0,33), o
que apoia a hipótese de economia pequena e aberta.

## Limitações

- Cerca de **39% da variação mensal do câmbio não é explicada** pelo modelo.
- A divisão entre externo e doméstico **depende da hipótese** de que o mundo afeta o Brasil e não o
  contrário. Essa hipótese tende a favorecer o bloco externo.
- Variáveis externas que ficaram de fora (China, fluxo de capital, contágio entre emergentes)
  acabam contadas como domésticas. Por esse lado, o peso externo provavelmente está **subestimado**.
  Um exemplo concreto: incluir as commodities elevou o peso externo de 63% para 72%.
- Os efeitos medidos são **contemporâneos** (mesmo mês) e de associação, não causalidade estrita.
- Os intervalos de confiança das janelas móveis são largos: leia tendências de vários anos, não
  oscilações de um ou dois meses.

## Estrutura do repositório

| Arquivo | Conteúdo |
|---|---|
| [02_coleta](02_coleta.ipynb) | Coleta: SGS/BCB, Focus, FRED, IPEADATA, CDS e commodities |
| [03_tratamento](03_tratamento.ipynb) | Frequência mensal, transformações, horizonte fixo do Focus, testes de estacionariedade |
| [04_regressao](04_regressao.ipynb) | Regressões padronizadas com erros HAC e decomposição do R² |
| [05_limpeza_risco](05_limpeza_risco.ipynb) | Limpeza das domésticas e decomposição do risco-país |
| [06_janelas_moveis](06_janelas_moveis.ipynb) | Regressão limpa em janelas móveis |
| [07_resultados](07_resultados.ipynb) | Gráficos finais e tabela-resumo ([resumo.xlsx](resultados/resumo.xlsx)) |
| [extra_var/](extra_var/) | VAR e FEVD (validação por um segundo método) |

## Como reproduzir

1. Criar o ambiente:
   ```bash
   conda env create -f environment.yml
   conda activate cambio
   ```
2. Criar uma chave gratuita da API do FRED em <https://fred.stlouisfed.org> (*My Account → API Keys*)
   e guardá-la na variável de ambiente `FRED_API_KEY`.
3. Baixar manualmente no Investing.com, com dados diários, e salvar em `dados/brutos/`:
   - o histórico do **CDS 5 anos do Brasil**
     ([link](https://www.investing.com/rates-bonds/brazil-cds-5-years-usd-historical-data)),
     como `cds_investing.csv`;
   - o histórico do **Bloomberg Commodity Index**, como `bcom_investing.csv`. Comece em 2008: o site
     limita cada download a ~5.000 linhas.
4. Rodar os notebooks em ordem, de `02` a `07`.

Os dados brutos não estão no repositório: as séries públicas são baixadas pelo `02_coleta`, e os CSVs
do Investing.com e o VIX têm restrições de redistribuição. Como as fontes revisam dados, uma nova
coleta pode gerar números ligeiramente diferentes dos apresentados aqui (coleta em 08/11/2026).
