# Prevendo-a-ocorrencia-de-diabetes-Linguagem-R
### Descrição
Este repositório possui uma construção desenvolvida no curso Análise de Dados em Linguagem R da Enap. Sendo uma construção de um modelo de machine learning utilizando a linguagem R. O objetivo dessa análise é demonstrar a construção do modelo preditivo, usado para identificar padrões e mostrar o que pode acontecer de acordo com os dados analisados.

### 📃Relatório
O repositório possui o respectivo arquivo PDF do relatório para a avaliação do modelo, além das descrições das etapas dentro do script. Entretanto, apresentarei as etapas e seus resultados de forma breve por aqui:

**Etapa 1**: Obtenção dos Dados

**Etapa 2**: Preparação dos dados

  Foi realizada a devida verificação dos dados, em busca de inconsistências, verificando o tipo dos dados das colunas do dataset, a existência de valores não preenchidos e a proporção dos valores de cada categoria. Posteriormente, tais inconsistências foram corrigidas.

**Etapa 3**: Análise exploratória

  Foram elaborados diversos gráficos para a análise exploratória dos dados, como os histogramas a seguir, que mostram as frequências relacionadas ao causador:
  
<img width="740" height="326" alt="image" src="https://github.com/user-attachments/assets/6c9f5328-02bd-4504-a34d-3bcc0f99233f" />
<img width="761" height="327" alt="image" src="https://github.com/user-attachments/assets/b7d24553-5c83-4bd8-ae56-00ba10c5ce75" />
<img width="771" height="308" alt="image" src="https://github.com/user-attachments/assets/3b57f97d-6330-4775-87e5-2f3691d6ef1d" />

**Etapa 4**: Construção do Modelo

  Foi realizada a divisão dos dados em treino e teste — 70% dos dados foram destinados ao treino e 30% ao teste. A imagem abaixo apresenta a distribuição das linhas totais entre os conjuntos de treino e teste:
  
<img width="202" height="153" alt="image" src="https://github.com/user-attachments/assets/e1936043-8329-4b18-8d54-6ecb81773de6" />

  Em seguida, foi realizado o primeiro treinamento do modelo (KNN). A imagem abaixo mostra seu desempenho, com acurácia variando entre 69% e 72%:
  
  <img width="566" height="126" alt="image" src="https://github.com/user-attachments/assets/3ed7e349-1f74-4646-9616-27f3e0624a7c" />
  
  Após o primeiro treinamento, foi desenvolvida uma segunda versão do modelo, testando diferentes valores de k. O gráfico de linhas a seguir demonstra o desempenho obtido:

<img width="779" height="341" alt="image" src="https://github.com/user-attachments/assets/93495a86-5176-49da-8402-841c6f9caeca" />

  Em seguida, foi criada a terceira versão do modelo, utilizando o algoritmo Naive Bayes, que apresentou desempenho entre 75% e 76% de acurácia:

<img width="840" height="73" alt="image" src="https://github.com/user-attachments/assets/31b459d2-0f39-425d-ac6f-6c69a52b7b51" />

Na quarta versão, foi aplicado o algoritmo Random Forest, obtendo desempenho entre 71% e 73% de acurácia:

<img width="385" height="100" alt="image" src="https://github.com/user-attachments/assets/349dbfa6-ef4d-4b46-a399-072f50e36ddc" />

 Por fim, foi desenvolvida a quinta e última versão do modelo, utilizando o pacote KernLab, que alcançou acurácia entre 73% e 76%.

**Etapa 5**: Realizando predições

**Etapa 6**: Visualização do Resultado

  O arquivo com os resultados está disponível no repositório.

## 🛠️ Construído com

* [Linguagem R](https://www-r--project-org.translate.goog/?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc) - A linguagem de programação utilizada.
* [RStudio](https://posit.co/download/rstudio-desktop) - O software utilizada.
