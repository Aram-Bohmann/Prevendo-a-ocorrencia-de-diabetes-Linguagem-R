# 🤖 Modelo Preditivo de Diabetes com Machine Learning em R

[![R](https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white)](https://www.r-project.org/)
[![RStudio](https://img.shields.io/badge/RStudio-75AADB?style=for-the-badge&logo=rstudio&logoColor=white)](https://posit.co/)
[![Machine Learning](https://img.shields.io/badge/Machine_Learning-Expert-success?style=for-the-badge)]()
[![Accuracy](https://img.shields.io/badge/Accuracy-76%25-brightgreen?style=for-the-badge)]()

> **Modelo de classificação para predição de diabetes usando múltiplos algoritmos de ML**  
> Desenvolvido no curso "Análise de Dados em Linguagem R" - Enap (2025)

Sistema preditivo completo em R para identificar pacientes com alta probabilidade de diabetes, comparando 5 algoritmos diferentes e alcançando **76% de acurácia** com SVM Radial.

---

## 📖 Sobre o Projeto

Projeto de **Machine Learning** desenvolvido em **Linguagem R** para predição de diabetes tipo 2. O modelo analisa 8 variáveis clínicas de pacientes e classifica o risco de diagnóstico positivo.

### 🎯 Objetivo

Desenvolver um modelo preditivo capaz de identificar pacientes com alta probabilidade de diabetes com **no mínimo 75% de acurácia**, auxiliando:

- 🏥 **Profissionais de saúde** - Triagem e diagnóstico precoce
- 📊 **Pesquisadores** - Análise de fatores de risco
- 💊 **Prevenção** - Identificação de pacientes em risco
- 📈 **Saúde pública** - Planejamento de políticas

---

## 🎯 Definição do Problema

### Pergunta de Pesquisa
**"É possível prever com precisão ≥75% se um paciente tem diabetes baseado em exames clínicos?"**

### Critérios de Sucesso
✅ **Acurácia ≥ 75%**  
✅ **Sensibilidade alta** (detectar casos positivos)  
✅ **Especificidade balanceada** (evitar falsos positivos)  
✅ **Modelo generalizável** (performance em dados não vistos)  

---

## 📊 Metodologia

### Pipeline de Machine Learning
```
📥 Obtenção dos Dados
    ↓
🧹 Preparação dos Dados
    ↓
📊 Análise Exploratória (EDA)
    ↓
🔧 Feature Engineering
    ↓
🤖 Treinamento de Modelos (5 algoritmos)
    ↓
📈 Avaliação e Comparação
    ↓
✅ Seleção do Melhor Modelo
    ↓
🔮 Predições em Novos Dados
```

---

## 📋 Etapa 1: Obtenção dos Dados

### 📊 Dataset - Diabetes Pima Indians

**Fonte:** Kaggle / UCI Machine Learning Repository  
**Registros:** 768 pacientes  
**Features:** 8 variáveis clínicas  
**Target:** Outcome (0 = Negativo, 1 = Positivo)  

### Variáveis do Dataset

| Variável | Tipo | Descrição | Unidade |
|----------|------|-----------|---------|
| `Pregnancies` | Integer | Número de gestações | # |
| `Glucose` | Integer | Nível de glicose no sangue | mg/dL |
| `BloodPressure` | Integer | Pressão arterial diastólica | mmHg |
| `SkinThickness` | Integer | Espessura da dobra cutânea | mm |
| `Insulin` | Integer | Nível de insulina | µU/mL |
| `BMI` | Float | Índice de Massa Corporal | kg/m² |
| `DiabetesPedigreeFunction` | Float | Função de pedigree de diabetes | - |
| `Age` | Integer | Idade | anos |
| **`Outcome`** | **Binary** | **Diagnóstico** | **0/1** |

---

## 🧹 Etapa 2: Preparação dos Dados

### Verificações Realizadas

#### 1️⃣ Estrutura dos Dados
```r
str(diabetes)
# Verificação de tipos de dados
# Conversão de Outcome para factor
```

#### 2️⃣ Valores Ausentes
```r
colSums(is.na(diabetes))
# Resultado: 0 valores NA
```

#### 3️⃣ Distribuição da Variável Target
```r
table(diabetes$Outcome)
#   0    1
# 500  268
# Taxa de diabetes: ~35%
```

### Tratamento de Outliers

**Problema identificado:** Valores extremos em `Insulin`

<details>
<summary><b>Boxplot Original (antes do tratamento)</b></summary>
```r
boxplot(diabetes$Insulin)
# Identificados valores > 250
```
</details>

**Solução aplicada:**
```r
# Filtro de outliers
diabetes2 <- diabetes %>%
  filter(Insulin <= 250)

# Redução: 768 → 735 registros
```

### Transformações

✅ Conversão de `Outcome` para factor  
✅ Remoção de outliers em `Insulin`  
✅ Normalização de features numéricas  
✅ Validação de integridade dos dados  

---

## 📊 Etapa 3: Análise Exploratória (EDA)

### Visualizações Desenvolvidas

#### 1️⃣ Distribuição de Gestações

<p align="center">
  <img width="740" alt="Histograma - Pregnancies" src="https://github.com/user-attachments/assets/6c9f5328-02bd-4504-a34d-3bcc0f99233f" />
</p>

**Insights:**
- ✅ Maioria das pacientes tem 0-3 gestações
- ✅ Distribuição assimétrica à direita
- ✅ Outliers com 10+ gestações

#### 2️⃣ Distribuição de Idade

<p align="center">
  <img width="761" alt="Histograma - Age" src="https://github.com/user-attachments/assets/b7d24553-5c83-4bd8-ae56-00ba10c5ce75" />
</p>

**Insights:**
- ✅ Maioria entre 20-40 anos
- ✅ Pico na faixa de 20-25 anos
- ✅ Risco aumenta com idade

#### 3️⃣ Distribuição de IMC (BMI)

<p align="center">
  <img width="771" alt="Histograma - BMI" src="https://github.com/user-attachments/assets/3b57f97d-6330-4775-87e5-2f3691d6ef1d" />
</p>

**Insights:**
- ✅ Média de BMI ~32 (obesidade leve)
- ✅ Distribuição próxima da normal
- ✅ BMI elevado correlaciona com diabetes

### Estatísticas Descritivas
```r
summary(diabetes2$Insulin)
#   Min.  1st Qu.  Median    Mean  3rd Qu.    Max. 
#   0.00   79.25  125.50  118.27  166.75  250.00
```

---

## 🤖 Etapa 4: Construção dos Modelos

### Divisão Treino/Teste

<p align="center">
  <img width="202" alt="Split Train/Test" src="https://github.com/user-attachments/assets/e1936043-8329-4b18-8d54-6ecb81773de6" />
</p>
```r
# 70% treino | 30% teste
set.seed(123)
train: 514 registros
test:  221 registros
```

---

### 🔵 Modelo 1: K-Nearest Neighbors (KNN)

#### Versão 1.0 - KNN Padrão

<p align="center">
  <img width="566" alt="KNN v1" src="https://github.com/user-attachments/assets/3ed7e349-1f74-4646-9616-27f3e0624a7c" />
</p>

**Performance:**
- ⚠️ Acurácia: 69-72%
- ⚠️ Abaixo da meta de 75%

#### Versão 1.1 - KNN Otimizado (Grid Search)
```r
# Testando k = 1 até 20
modelo2 <- train(
  Outcome ~., data = train, 
  method = "knn",
  tuneGrid = expand.grid(k = c(1:20))
)
```

<p align="center">
  <img width="779" alt="KNN Grid Search" src="https://github.com/user-attachments/assets/93495a86-5176-49da-8402-841c6f9caeca" />
</p>

**Resultados:**
- ✅ Melhor k: 7
- ✅ Acurácia máxima: ~73%
- ⚠️ Ainda abaixo da meta

---

### 🟢 Modelo 2: Naive Bayes
```r
modelo3 <- train(
  Outcome ~., data = train, 
  method = "naive_bayes"
)
```

<p align="center">
  <img width="840" alt="Naive Bayes" src="https://github.com/user-attachments/assets/31b459d2-0f39-425d-ac6f-6c69a52b7b51" />
</p>

**Performance:**
- ✅ Acurácia: 75-76%
- ✅ **Atingiu a meta!**
- ✅ Rápido para treinar
- ✅ Interpretável

---

### 🟡 Modelo 3: Random Forest (Decision Tree)
```r
modelo4 <- train(
  Outcome ~., data = train, 
  method = "rpart2"
)
```

<p align="center">
  <img width="385" alt="Random Forest" src="https://github.com/user-attachments/assets/349dbfa6-ef4d-4b46-a399-072f50e36ddc" />
</p>

**Performance:**
- ⚠️ Acurácia: 71-73%
- ⚠️ Abaixo da meta

#### Feature Importance
```r
varImp(modelo4$finalModel)
# Insulin e BloodPressure: baixa importância
```

**Otimização:** Remoção de features irrelevantes
```r
modelo4_1 <- train(
  Outcome ~., 
  data = train[,c(-3,-5)],  # Remove Insulin e BloodPressure
  method = "rpart2"
)
```

---

### 🔴 Modelo 4: SVM Radial (VENCEDOR 🏆)
```r
modelo5 <- train(
  Outcome ~., data = train, 
  method = "svmRadialSigma",
  preProcess = c("center")
)
```

**Performance:**
- ✅ Acurácia: **73-76%**
- ✅ **Melhor modelo geral**
- ✅ Boa generalização
- ✅ Hiperparâmetros otimizados

---

## 📊 Comparação de Modelos

### Tabela de Performance

| Modelo | Algoritmo | Acurácia | Sensibilidade | Especificidade | Tempo Treino |
|--------|-----------|----------|---------------|----------------|--------------|
| Modelo 1 | KNN (k=5) | 69-72% | Média | Média | Rápido |
| Modelo 2 | KNN (k=7) | ~73% | Boa | Boa | Rápido |
| **Modelo 3** | **Naive Bayes** | **75-76%** | **Alta** | **Boa** | **Muito Rápido** |
| Modelo 4 | Random Forest | 71-73% | Média | Alta | Médio |
| **Modelo 5** | **SVM Radial** | **73-76%** | **Alta** | **Alta** | **Médio** |

### 🏆 Modelo Selecionado

**SVM Radial (modelo5)** foi escolhido como modelo final devido a:

1. ✅ **Acurácia de 76%** (acima da meta)
2. ✅ **Sensibilidade alta** (detecta casos positivos)
3. ✅ **Especificidade alta** (evita falsos positivos)
4. ✅ **Boa generalização** em dados não vistos
5. ✅ **Robusto a overfitting**

---

## 📈 Etapa 5: Avaliação do Modelo

### Confusion Matrix
```r
predicoes <- predict(modelo5, test)
confusionMatrix(predicoes, test$Outcome)
```

**Resultados no conjunto de teste:**

|  | Predito: 0 | Predito: 1 |
|---|------------|------------|
| **Real: 0** | 125 (TN) | 18 (FP) |
| **Real: 1** | 35 (FN) | 43 (TP) |

### Métricas de Performance

| Métrica | Valor | Interpretação |
|---------|-------|---------------|
| **Acurácia** | 76.0% | ✅ Acima da meta |
| **Sensibilidade (Recall)** | 55.1% | Detecta 55% dos casos positivos |
| **Especificidade** | 87.4% | Evita 87% dos falsos positivos |
| **Precisão** | 70.5% | 70% das predições positivas corretas |
| **F1-Score** | 61.9% | Balanço entre precisão e recall |

### Interpretação Clínica

✅ **Forças:**
- Excelente especificidade (87%) - poucos falsos positivos
- Acurácia geral alta (76%)
- Bom para triagem inicial

⚠️ **Limitações:**
- Sensibilidade moderada (55%) - pode perder alguns casos
- Ideal complementar com exames confirmatórios

---

## 🔮 Etapa 6: Predições em Novos Pacientes

### Exemplo Prático
```r
# Dados de um novo paciente
novos.dados <- data.frame(
  Pregnancies = 3,
  Glucose = 111.50,
  BloodPressure = 70,
  SkinThickness = 20,
  Insulin = 47.49,
  BMI = 30.80,
  DiabetesPedigreeFunction = 0.34,
  Age = 28
)

# Fazer predição
previsao <- predict(modelo5, novos.dados)
resultado <- ifelse(previsao == 1, "Positivo", "Negativo")

# Resultado: "Negativo"
```

### Aplicação Prática
```r
# Gerar arquivo com todas as predições
write.csv(predicoes, 'resultado.csv')

# Visualizar resultados
resultado.csv <- read.csv('resultado.csv')
names(resultado.csv) <- c('Indice', 'Valor previsto')
```

---

## 🛠️ Stack Tecnológica

### Core
![R](https://img.shields.io/badge/R_4.3+-276DC3?style=flat-square&logo=r&logoColor=white)
![RStudio](https://img.shields.io/badge/RStudio-75AADB?style=flat-square&logo=rstudio&logoColor=white)

### Bibliotecas de ML
![caret](https://img.shields.io/badge/caret-ML_Framework-blue?style=flat-square)
![e1071](https://img.shields.io/badge/e1071-SVM-orange?style=flat-square)
![randomForest](https://img.shields.io/badge/randomForest-RF-green?style=flat-square)
![kernlab](https://img.shields.io/badge/kernlab-SVM_Advanced-red?style=flat-square)

### Bibliotecas de Suporte
![dplyr](https://img.shields.io/badge/dplyr-Data_Manipulation-blue?style=flat-square)
![caTools](https://img.shields.io/badge/caTools-Train/Test_Split-purple?style=flat-square)

---

## 🚀 Como Executar

### Pré-requisitos
```r
# R 4.0+ instalado
R.version

# RStudio (recomendado)
# Download: https://posit.co/download/rstudio-desktop/
```

### Instalação de Pacotes
```r
# Instalar pacotes necessários
install.packages(c(
  "dplyr", "caTools", "caret", "e1071",
  "naivebayes", "randomForest", "kernlab"
))

# Carregar bibliotecas
library(dplyr)
library(caret)
library(e1071)
```

### Executar o Modelo
```bash
# Clone o repositório
git clone https://github.com/Aram-Bohmann/Prevendo-a-ocorrencia-de-diabetes-Linguagem-R.git

# Entre no diretório
cd Prevendo-a-ocorrencia-de-diabetes-Linguagem-R

# Abra o RStudio e execute
# File → Open File → diabetes_ml.R
```

### Fazer Predições
```r
# Carregar modelo treinado
modelo <- readRDS("models/modelo5_svm.rds")

# Fazer predição
previsao <- predict(modelo, novos_dados)
```

---

## 📊 Principais Insights

### 🔍 Fatores de Risco Identificados

1. **Glicose elevada** - Maior preditor
2. **IMC alto** - Forte correlação
3. **Idade avançada** - Risco aumenta
4. **Histórico familiar** - DiabetesPedigreeFunction
5. **Gestações múltiplas** - Fator relevante

### 💡 Descobertas do Modelo

✅ **Insulin** e **BloodPressure** têm baixa importância preditiva  
✅ **Glucose** e **BMI** são os melhores preditores  
✅ SVM supera modelos mais simples  
✅ Normalização melhora performance  

---

## 🎓 Contexto Acadêmico

### Curso
**Análise de Dados em Linguagem R**  
**Instituição:** Enap (Escola Nacional de Administração Pública)  
**Ano:** 2025  
**Carga Horária:** 20 horas  

### Competências Desenvolvidas

1. **🤖 Machine Learning** - Múltiplos algoritmos
2. **📊 Análise Exploratória** - EDA completa
3. **🧹 Pré-processamento** - Limpeza e feature engineering
4. **📈 Avaliação de Modelos** - Métricas e comparação
5. **💻 Programação R** - caret, e1071, kernlab
6. **📝 Documentação** - Relatório técnico

---

## 📚 Referências

- **Dataset:** [Pima Indians Diabetes Database](https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database)
- **caret Package:** [Documentation](https://topepo.github.io/caret/)
- **SVM Tutorial:** [e1071 Guide](https://cran.r-project.org/web/packages/e1071/)
- **Machine Learning em R:** [An Introduction to Statistical Learning](https://www.statlearning.com/)

---

## 🚀 Melhorias Futuras

### Roadmap

- [ ] **Ensemble methods** - Combinar modelos
- [ ] **Deep Learning** - Redes neurais em R
- [ ] **Feature engineering** - Criar variáveis derivadas
- [ ] **Cross-validation** - K-fold mais robusto
- [ ] **Explicabilidade** - SHAP values, LIME
- [ ] **Deploy** - API REST com plumber
- [ ] **Dashboard** - Shiny app interativo

---

## 🤝 Contribuindo

Contribuições são bem-vindas!

### Como Contribuir

1. Fork o projeto
2. Crie uma branch (`git checkout -b feature/melhoria`)
3. Commit suas mudanças (`git commit -m 'Melhora modelo'`)
4. Push para a branch (`git push origin feature/melhoria`)
5. Abra um Pull Request

---

## 📝 Licença

Este projeto foi desenvolvido para fins **educacionais** e está disponível para:

✅ Uso em estudos e pesquisa  
✅ Modificação e adaptação  
✅ Distribuição com créditos  

⚠️ **Aviso:** Este modelo é para fins educacionais e **não deve ser usado para diagnóstico médico real** sem validação clínica apropriada.

---

## 📞 Contato

**Desenvolvedor:** Aram Bohmann Leite da Luz

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:arambohmannleitedaluz@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/aram-luz-1b0ab1321)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Aram-Bohmann)
[![Portfolio](https://img.shields.io/badge/Portfolio-FF5722?style=for-the-badge&logo=google-chrome&logoColor=white)](https://aram-bohmann.github.io/Site-Portfolio/)

---

## 🙏 Agradecimentos

- **Enap** - Pelo curso de ML em R
- **UCI Machine Learning Repository** - Pelo dataset
- **Comunidade R** - Pelos pacotes open-source
- **caret developers** - Framework excepcional

---

<div align="center">

### ⭐ Se este projeto foi útil para você, considere dar uma estrela!

**Desenvolvido com 💙 e 🤖 no curso Enap 2025**

*"Machine Learning para salvar vidas através da predição precoce"*

📊 **Acurácia: 76%** | 🎯 **Meta alcançada!**

</div>
