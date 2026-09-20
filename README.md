# Análise Exploratória do Wine Quality (Vinho Tinto e Vinho Branco)

Homework 1 da disciplina **TI0175 - Inteligência Computacional Aplicada**, Universidade Federal do Ceará (UFC).


## Sobre o projeto

Este repositório reúne o código, os dados de entrada e o artigo produzidos para o Homework 1, cujo objetivo é obter uma boa compreensão de um conjunto de dados por meio de estatísticas descritivas e visualizações. Entre as alternativas oferecidas no enunciado, foi escolhida a **Alternativa 3, Wine Quality**, que contém amostras de vinho tinto e de vinho branco da variedade *Vinho Verde*, produzidas em Portugal. Os dados foram obtidos no [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/186/wine+quality).

Cada amostra é descrita por onze preditores físico-químicos (acidez fixa, acidez volátil, ácido cítrico, açúcar residual, cloretos, dióxido de enxofre livre, dióxido de enxofre total, densidade, pH, sulfatos e teor alcoólico) e por um rótulo de classe, a nota sensorial `quality`. O conjunto de vinho tinto tem N = 1599 observações e o de vinho branco tem N = 4899, ambos com D = 11 preditores. Nos dados observados, o vinho tinto apresenta L = 6 classes distintas de qualidade e o vinho branco apresenta L = 7, ou seja, menos do que as dez notas possíveis da escala teórica de 0 a 10. A distribuição de observações por classe é bastante desbalanceada, com a maior parte das amostras concentrada nas notas intermediárias.

## O que a análise cobre

O notebook segue os cinco itens do enunciado, e cada item é tratado separadamente para o vinho tinto e para o vinho branco, exceto o último.

No **Item 1**, o conjunto de dados é descrito: são obtidos a lista de preditores, o número de observações N, o número de preditores D, o número de classes L e a distribuição de observações por classe. No **Item 2**, é feita a análise monovariada incondicional, com histogramas, box-plots e uma tabela com média, desvio padrão e assimetria (*skewness*) de cada preditor, calculados sobre todas as observações. No **Item 3**, a mesma análise é repetida de forma condicional, ou seja, para cada preditor e para cada classe de qualidade, o que gera histogramas e box-plots por classe e uma tabela de métricas indexada pelo par (preditor, classe). Essa análise é a base para discutir o poder discriminativo individual de cada preditor.

O **Item 4** trata da análise bivariada incondicional: uma matriz de gráficos de dispersão entre todos os pares de preditores, com pontos coloridos pela classe de qualidade, e a matriz de correlação de Pearson, apresentada como tabela e como mapa de calor. Por fim, o **Item 5** implementa a análise de componentes principais (PCA) **do zero**, sem bibliotecas ou funções prontas de PCA. Os dados de vinho tinto e branco são combinados em um único conjunto de 6497 observações, padronizados (escore z), e a matriz de covariância é construída manualmente. Os autovalores e autovetores são obtidos com `numpy.linalg.eigh`, usada apenas como rotina de álgebra linear, e as observações são projetadas nas duas primeiras componentes. O notebook calcula ainda a variância explicada, os *loadings* de PC1 e PC2 e a média das coordenadas de cada classe no plano projetado. Nesse item, o rótulo usado para colorir a projeção é o **tipo de vinho** (tinto ou branco), e não a nota de qualidade, e ele não participa do cálculo da PCA.

## Decisões metodológicas

Nos Itens 2 e 3, o desvio padrão é calculado com a fórmula populacional (`ddof=0`, divisão por N), pois as observações disponíveis são tratadas como a população de interesse e não como uma amostra de um universo maior. No Item 5, a padronização usa o desvio padrão amostral (`ddof=1`) e a matriz de covariância é dividida por N − 1, que é a convenção usual em PCA. A assimetria é calculada com `scipy.stats.skew` em sua configuração padrão. Nos gráficos de dispersão, as classes mais frequentes são desenhadas primeiro para que as classes raras não fiquem escondidas atrás delas, e cada classe mantém a mesma cor em todos os painéis.

## Estrutura do repositório

```
.
├── Homework1_ICA.ipynb        # Notebook com todo o código (Itens 1 a 5)
├── data/
│   ├── winequality-red.csv    # Vinho tinto (UCI), separador ";"
│   └── winequality-white.csv  # Vinho branco (UCI), separador ";"
├── paper/                     # Artigo no formato IEEE (até 6 páginas)
├── requirements.txt           # Dependências
└── README.md
```

## Dependências

O código usa Python 3 e as bibliotecas `pandas`, `numpy`, `scipy` e `matplotlib`. O arquivo `requirements.txt` pode conter simplesmente:

```
pandas
numpy
scipy
matplotlib
jupyter
```

## Como executar

O notebook foi desenvolvido no **Google Colab**, e os caminhos de arquivo no código apontam para `/content/`. Há duas formas de executá-lo.

**No Google Colab.** Abra o notebook, envie os arquivos `winequality-red.csv` e `winequality-white.csv` para o ambiente (pelo painel de arquivos ou com `files.upload()`) e execute as células em ordem, do início ao fim.

**Localmente.** Clone o repositório, instale as dependências e abra o notebook:

```bash
git clone https://github.com/davidbmonte/<nome-do-repositorio>.git
cd <nome-do-repositorio>
pip install -r requirements.txt
jupyter notebook Homework1_ICA.ipynb
```

Antes de executar, ajuste os caminhos de leitura dos arquivos CSV, que aparecem nas células dos Itens 1 e 5, trocando `/content/winequality-red.csv` por `data/winequality-red.csv` (e o equivalente para o vinho branco). As células devem ser executadas em ordem, porque as funções dos Itens 3 e 4 dependem de variáveis definidas antes, como a lista `preditores` criada no Item 1.

## Artigo

O relatório do trabalho está escrito no formato de artigo de conferência do IEEE, com no máximo seis páginas, e contém título, resumo, introdução, métodos, resultados e referências. As figuras e tabelas discutidas no texto são geradas pelo notebook.


