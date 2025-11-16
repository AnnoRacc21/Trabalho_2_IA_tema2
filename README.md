# Trabalho_2_IA_tema2

Alunos:
- Airton Filho
- André Filho
- Guilherme Louro
- Nicolas Mady
- Victor Hugo

---

Link para o Arquivo Google Colab: https://colab.research.google.com/drive/1h_GK37sKkpGiZywOdERaMWsVsXNVJsJP?usp=

---

# Projeto 2: Classificação de Lixo para Reciclagem (TrashNet)

Este repositório contém o código e a análise para o segundo trabalho da disciplina FIA (Prof. Edjard Mota), focado na construção de um classificador de imagens para resíduos.

## 1. Descrição do Projeto

O objetivo deste projeto é construir um classificador de imagens multiclasse capaz de categorizar imagens de lixo em seis categorias distintas: `glass` (vidro), `paper` (papel), `cardboard` (papelão), `plastic` (plástico), `metal` e `trash` (lixo orgânico/rejeito).

A automação da triagem de resíduos é um passo crucial para otimizar os processos de reciclagem e este projeto explora o uso de Redes Neurais Convolucionais (CNNs) para resolver esse problema.

### Dataset

O projeto utiliza o dataset **TrashNet**, que consiste em 2.527 imagens divididas nas seis classes mencionadas. O dataset é descrito como "relativamente pequeno e um pouco desbalanceado", o que torna o uso de *data augmentation* uma técnica essencial para evitar overfitting e melhorar a generalização do modelo.

### Metodologia

O modelo é uma Rede Neural Convolucional (CNN) padrão, construída em Keras (TensorFlow). A arquitetura consiste em 3 blocos de convolução (Conv2D -> ReLU -> MaxPooling2D), seguidos por um classificador (Flatten -> Dense -> Dropout -> Dense). A camada de saída final possui 6 neurônios e uma função de ativação `softmax` para a classificação multiclasse. Para o carregamento e pré-processamento dos dados, foi utilizado o `ImageDataGenerator` do Keras, que também foi responsável por aplicar o *data augmentation* em tempo real (como rotação, zoom e inversão).

## 2. Análise dos Resultados

A avaliação do modelo foi dividida em duas partes: a análise da acurácia ao longo do tempo e a análise detalhada dos erros através da matriz de confusão.

### Gráfico de Acurácia

A análise do gráfico de acurácia (treino vs. teste) revela dois pontos principais:

1.  **Overfitting:** Existe uma lacuna visível e significativa entre a acurácia de treino (linha azul) e a acurácia de teste/validação (linha vermelha). A acurácia de treino ultrapassa 65%, enquanto a de teste oscila instavelmente em torno de 45-55%. Isso indica que o modelo "decorou" os dados de treino, mas teve dificuldade em generalizar seu aprendizado para novas imagens.
2.  **Instabilidade:** A acurácia de teste (linha vermelha) é muito volátil, com picos e vales acentuados. Isso sugere que o modelo não é robusto e que pequenas variações nos dados de teste podem levar a grandes mudanças no desempenho.

### Matriz de Confusão

A matriz de confusão nos permite entender exatamente quais classes o modelo está confundindo.

* O modelo é excelente em identificar **`paper` (papel)**. Na linha "paper", ele acertou 76 vezes, com pouquíssimas confusões.
* O modelo teve um desempenho muito ruim com **`plastic` (plástico)**. Na linha "plastic", ele só acertou 7 vezes. A maioria das imagens de plástico foi classificada incorretamente como `paper` (26 vezes) ou `glass` (23 vezes).
* A classe **`trash` (lixo)** também teve um desempenho fraco, acertando apenas 4 vezes e sendo frequentemente confundida com `paper` (7 vezes) e `glass` (6 vezes).

**Principais Confusões Observadas:**
1.  **`plastic` -> `paper` (26 vezes):** O modelo confunde muito plástico com papel.
2.  **`plastic` -> `glass` (23 vezes):** A segunda maior confusão. Isso pode ocorrer porque ambos os materiais podem ser transparentes ou reflexivos.
3.  **`cardboard` -> `paper` (16 vezes):** Uma confusão compreensível, dado que papelão é um derivado de papel.

## 3. Conclusão sobre o Impacto do Data Augmentation

O data augmentation (aumento de dados) era uma técnica obrigatória para este projeto, dado o dataset pequeno e desbalanceado. O objetivo principal era combater o overfitting. Com base nos resultados, podemos concluir que:

* O data augmentation foi necessário, mas não suficiente. Sem ele, o overfitting (a lacuna entre as curvas azul e vermelha no gráfico) provavelmente seria ainda mais rápido e severo. Ele permitiu que o modelo treinasse por mais épocas sem divergir completamente.

* No entanto, o overfitting ainda é o principal problema do modelo. A performance no conjunto de teste (validação) permaneceu baixa e instável. Isso indica que, embora o *data augmentation* tenha ajudado, ele não conseguiu superar as limitações de um dataset pequeno e desbalanceado. Para melhorar o desempenho, seriam necessárias mais imagens reais, uma arquitetura de modelo diferente (talvez *transfer learning*) ou técnicas de aumento de dados mais sofisticadas.

## 4. Como Executar

O link para o Google Colab contém todo o código para carregamento de dados, definição do modelo, treinamento e avaliação. O notebook disponível neste repositório também, mas é necessário carregar localmente o dataset.

Para executar o projeto localmente:
1.  Clone este repositório.
2.  Faça o download do dataset [TrashNet no Kaggle](https://www.kaggle.com/datasets/asdasdasasdas/garbage-classification).
3.  Abra o `notebook.ipynb` em um ambiente com TensorFlow e Keras instalados (como Google Colab ou Jupyter Lab).
4.  Atualize o caminho do dataset na Célula 5 (`data_path = '...'`).
5.  Execute todas as células.

Para executar o projeto no Google Colab, basta utilizar-se do Notebook.
