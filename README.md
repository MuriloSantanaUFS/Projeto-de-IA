# Previsão de Risco de Burnout Acadêmico em Contextos de Ferramentas de IA

## Objetivo

Desenvolver e comparar modelos de classificação supervisionada para prever o nível de risco de burnout (**Baixo**, **Médio** ou **Alto**) de estudantes universitários, utilizando como preditores padrões de uso de ferramentas de IA generativa, hábitos de estudo tradicionais, indicadores de desempenho acadêmico, fatores psicossociais (ansiedade, dependência percebida) e contexto institucional.

## Integrantes

| Nome | GitHub |
|---|---|
| Julyana Azevedo Lima | [@julyana-ai](https://github.com/julyana-ai) |
| Murilo Santos de Santana | [@MuriloSantanaUFS](https://github.com/MuriloSantanaUFS) |

## Fonte dos dados

Dataset **Impact of AI on Students**, disponível no Kaggle: [laveshjadon/ai-impact-on-students](https://www.kaggle.com/datasets/laveshjadon/ai-impact-on-students).

O conjunto possui 50.000 registros e 16 atributos. A ausência total de valores nulos e duplicatas sugere que o dataset foi gerado sinteticamente para fins didáticos — essa hipótese é discutida na seção de compreensão dos dados do notebook.

## Tipo da tarefa

**Classificação multiclasse** (3 classes). O atributo-alvo, `Burnout_Risk_Level`, representa categorias discretas e mutuamente exclusivas (Low, Medium, High), e não um valor numérico contínuo — por isso o problema foi definido como classificação, e não regressão.

## Organização dos arquivos

```
├── README.md                          # este arquivo
├── projeto_burnout_risk_level.ipynb   #notebook principal 
└── dataset/                           #apenas para deixar o dataset registrado, mas o acesso a esse será feito diretamente da biblioteca do Kaggle
```

## Instruções para abrir o notebook no Colab

1. Acesse o notebook diretamente pelo link do Google Colab: *[Acesso ao Notebook](https://colab.research.google.com/drive/1tgLMQJmxbh5Lml7wi3mrjsLlBtNSzX2B)*
2. Ou, a partir do GitHub, abra o arquivo `.ipynb` e clique em "Open in Colab".
3. Execute as células em ordem, do início ao fim (Runtime → Run all). O notebook carrega o dataset automaticamente via `kagglehub`, sem necessidade de upload manual de arquivos.

## Modelos utilizados

- **Baseline**: `DummyClassifier` (estratégia estratificada), usado como referência mínima de comparação.
- **SGDClassifier**: classificador linear treinado por gradiente descendente estocástico (um dos modelos mínimos exigidos para a tarefa).
- **RandomForestClassifier**: modelo de ensemble baseado em múltiplas árvores de decisão (segundo modelo mínimo exigido).

Ambos os modelos foram comparados via validação cruzada estratificada (5 folds) sobre o conjunto de treinamento, com o `RandomForestClassifier` selecionado como modelo final.

## Principais resultados

**Comparação de modelos (F1-macro médio, validação cruzada):**

| Modelo | F1-macro médio | Desvio-padrão |
|---|---|---|
| Baseline | 0,3319 | 0,0022 |
| SGDClassifier | 0,4696 | 0,0313 |
| **RandomForestClassifier** | **0,5239** | **0,0048** |

A `RandomForestClassifier` foi selecionada como modelo final por apresentar o melhor desempenho médio e a maior estabilidade entre os folds.

**Avaliação no conjunto de teste:** o modelo final apresenta maior confusão entre classes adjacentes (Low↔Medium e Medium↔High) do que entre os extremos (Low↔High), o que é um sinal positivo — o erro mais grave na prática (confundir risco baixo com risco alto) é o menos frequente. Ainda assim, cerca de 10% dos estudantes de risco **High** foram classificados como **Low**, o que representa a principal limitação prática do modelo.

Detalhes completos (matriz de confusão, acurácia, precisão, revocação e F1-score por classe) estão disponíveis no notebook, seção 7.


## 🎥 Link do vídeo

[Assistir à apresentação](https://drive.google.com/file/d/1pmfe3VUh2ENQ9rtAjDPBdQOuEQEGjNAp/view?usp=drivesdk)

## Declaração de uso de ferramentas de IA

Foi utilizada a ferramenta Claude (Anthropic) como apoio ao longo do desenvolvimento do projeto, com as seguintes finalidades:
- Estruturação e revisão do notebook conforme os critérios do enunciado da disciplina.
- Auxílio nas dúvidas pontuais sobre tratamento de dados e sobre os modelos.

A verificação do código e das interpretações foi feita executando cada célula do notebook e conferindo se os resultados numéricos gerados correspondiam ao que era esperado, antes de aceitar qualquer trecho sugerido. Todo o código e as interpretações foram compreendidos e revisados pelos membros do grupo, que se responsabilizam pela explicação e justificativa do trabalho apresentado no vídeo.
