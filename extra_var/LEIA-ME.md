# Extensão: VAR e decomposição da variância (FEVD)

Material complementar, fora da análise principal (notebooks 01 a 07 na raiz).

| Notebook | O que faz |
|---|---|
| `06_var.ipynb` | VAR(1) com as 7 variáveis, identificação de Cholesky (externas primeiro), FEVD do câmbio em 1, 3, 6 e 12 meses, respostas ao impulso e teste de Granger |
| `07_janelas_moveis_var.ipynb` | A mesma FEVD em janelas móveis de 60 e 84 meses |

**Por que ficou de fora da análise principal:** os resultados coincidem com os da regressão limpa
(63% externo no explicado, na amostra completa; mesmas fases nas janelas móveis). O VAR serve
como validação por um segundo método e acrescenta três informações:

- o câmbio absorve os choques **no próprio mês**: a FEVD quase não muda entre 1 e 12 meses;
- o teste de Granger não encontra evidência de que o bloco doméstico antecipe o externo
  (p = 0,18), o que apoia a hipótese de economia pequena e aberta;
- a Selic deixa de ser relevante quando se olha só a **surpresa** (a parte não antecipada).

**Para rodar:** execute antes os notebooks 01 a 03 da raiz (eles geram `dados/tratados/`).
Os resultados destes notebooks são salvos em `extra_var/resultados/`.
