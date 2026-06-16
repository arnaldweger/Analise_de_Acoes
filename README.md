# 📊 Seleção Quantitativa de Ações com Valor da Informação (IV)

Modelo quantitativo de triagem de ações da B3 utilizando a técnica de **Valor da Informação (IV)**, aplicada a 589 empresas listadas com dados fundamentalistas disponíveis.

---

## 🎯 Problema

Gestoras de investimento que operam análise fundamentalista manual conseguem cobrir, em média, apenas 5–10% do universo de ações da B3. O objetivo deste projeto é construir um **funil quantitativo** que reduza esse universo para uma lista de 50–150 ações de alta qualidade, permitindo que analistas concentrem esforço humano onde há maior potencial de retorno.

---

## 🔧 Metodologia

O modelo foi construído em cinco etapas:

**1. Limpeza e Tratamento dos Dados**
Diagnóstico e remoção de anomalias na base bruta: ações com cotação zero, P/L negativo (prejuízo) e ROE negativo (destruição de valor). A base passou de 997 para 589 ações válidas.

**2. Classificação Binária — "Ação Boa?"**
Cada ação recebe uma pontuação de 0 a 4 com base em quatro critérios simultâneos:

| Critério | Regra |
|---|---|
| ROE | > 10% |
| Margem Líquida | > 5% |
| Dívida Bruta / Patrimônio | < 1 |
| P/L | Entre 0 e 15 |

Ações que atendem aos 4 critérios são classificadas como **"Sim"** (94 ações — 16% da base tratada).

**3. Segmentação em Faixas (Binning)**
Os 19 indicadores fundamentalistas foram segmentados em 4 faixas usando os percentis P25, P50 e P75 como pontos de corte via `pd.cut()`.

**4. Cálculo do Valor da Informação (IV)**
Para cada faixa de cada indicador, calculado o poder discriminante entre ações "boas" e "ruins" através da diferença entre distribuições. Indicadores com maior IV recebem maior peso no score final.

**5. Score Final e Recomendações**
```
Score Final = Σ (peso_da_faixa × IV_total_do_indicador)
```
A lista de recomendações é gerada em dois níveis:
- **Lista Ampla:** Score ≥ mediana (~50% das ações)
- **Lista Restrita:** Score ≥ P75 (25% melhores — alta convicção)

---

## 📁 Estrutura do Repositório

```
├── Analise_AticaCapital.ipynb   # Notebook principal com toda a análise
└── Base Financial Analysis.xlsx # Base com 997 ações e 21 indicadores
```

---

## 🛠️ Tecnologias

- Python 3.x
- pandas
- matplotlib
- seaborn

---

## ▶️ Como executar

```bash
# 1. Clone o repositório
git clone https://github.com/seu-usuario/analise-acoes-atica-capital.git

# 2. Instale as dependências
pip install pandas matplotlib seaborn openpyxl

# 3. Abra o notebook
jupyter notebook Analise_AticaCapital.ipynb
```

---

## 📌 Limitações e próximos passos

- Modelo estático: critérios fixos sem ajuste por ciclo econômico
- Sem segmentação setorial (margens e endividamento variam muito por setor)
- Próximos passos: integração com API para atualização automática da base e incorporação de análise de momentum de preço

---

*Projeto desenvolvido como exercício de análise quantitativa aplicada ao mercado de capitais brasileiro.*
