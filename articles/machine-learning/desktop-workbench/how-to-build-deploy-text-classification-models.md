---
title: Compilación e implementación de un modelo de clasificación de texto con Azure Machine Learning Package for Text Analytics
description: Obtenga información sobre cómo compilar, entrenar, probar e implementar un modelo de clasificación de texto con Azure Machine Learning Package for Text Analytics.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 05/07/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 97d988332a2c5234cb260cef29f195f0fecfee45
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994945"
---
# <a name="build-and-deploy-text-classification-models-with-azure-machine-learning"></a>Compilación e implementación de modelos de clasificación de texto con Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


En este artículo, puede encontrar información sobre cómo entrenar e implementar un modelo de clasificación de texto con **Azure Machine Learning Package for Text Analytics** (AMLPTA). El objetivo de la clasificación de texto es asignar parte de un texto a una o varias clases o categorías predefinidas. Por ejemplo, este texto podría ser un documento, un artículo periodístico, una consulta de búsqueda, un correo electrónico, un tweet, incidencias de soporte técnico.

Hay aplicaciones generales de clasificación de texto, como: 
+ Clasificación de artículos periodísticos y contenido de noticias en distintos temas
+ Organización de páginas web en categorías jerárquicas 
+ Filtrado de correo electrónico no deseado
+ análisis de opiniones
+ Predicción de la intención del usuario a partir de las consultas de búsqueda
+ Enrutamiento de las incidencias de soporte técnico
+ Análisis de los comentarios de los clientes 

El flujo de trabajo de compilación e implementación del modelo de clasificación de texto para un modelo con AMLPTA es el siguiente:

1. Carga de los datos
2. Entrenamiento del modelo
3. Aplicación del clasificador 
4. Evaluación del rendimiento del modelo
5. Guardado de la canalización
6. Prueba de la canalización
8. Implementación del modelo como servicio web

Consulte la [documentación de referencia del paquete](https://aka.ms/aml-packages/text) para ver la referencia detallada de cada módulo y clase.

El código de ejemplo que aparece en este artículo usa una canalización de scikit-learn.

## <a name="prerequisites"></a>Requisitos previos 

1. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

1. Deben estar configuradas e instaladas las siguientes cuentas y la siguiente aplicación:
   - Una cuenta de Experimentación de Azure Machine Learning. 
   - Una cuenta de Administración de modelos de Azure Machine Learning.
   - Azure Machine Learning Workbench instalado.

   Si todavía no se crearon o instalaron las tres, siga las instrucciones que aparecen en el artículo sobre la [instalación de Workbench y el inicio rápido de Azure Machine Learning](../desktop-workbench/quickstart-installation.md). 

1. Azure Machine Learning Package for Text Analytics debe estar instalado. Aprenda a [instalar este paquete aquí](https://aka.ms/aml-packages/text).


## <a name="sample-data-and-jupyter-notebook"></a>Jupyter Notebook y datos de ejemplo

### <a name="get-the-jupyter-notebook"></a>Obtención de Jupyter Notebook

Pruébelo usted mismo. Descargue el Notebook y ejecútelo.

> [!div class="nextstepaction"]
> [Obtención de Jupyter Notebook](https://aka.ms/aml-packages/text/notebooks/text_classification_sentiment_data)

### <a name="download-and-explore-the-sample-data"></a>Descarga y exploración de los datos de ejemplo
En el ejemplo siguiente se usa el [conjunto de datos de 20 grupos de noticias](http://qwone.com/~jason/20Newsgroups/) que está disponible a través de la biblioteca scikit-learn para mostrar cómo crear un clasificador de texto con Azure Machine Learning Package for Text Analytics. 

El conjunto de datos de 20 grupos de noticias tiene alrededor de 18 000 publicaciones en grupos de noticias en 20 temas distintos divididos en dos subconjuntos: uno para entrenamiento y el otro para evaluar el rendimiento. La división en entrenamiento y prueba se basa en la fecha de publicación de cada mensaje, ya sea antes o después de una fecha específica.

```python
# Import Packages 
# Use Azure Machine Learning history magic to control history collection
# History is off by default, options are "on", "off", or "show"
#%azureml history on
%matplotlib inline
# Use the Azure Machine Learning data collector to log various metrics
from azureml.logging import get_azureml_logger
import os

logger = get_azureml_logger()

# Log cell runs into run history
logger.log('Cell','Set up run')
# from tatk.utils import load_newsgroups_data, data_dir, dictionaries_dir, models_dir
import pip
pip.main(["show", "azureml-tatk"])
```

### <a name="set-the-location-of-the-data"></a>Establecimiento de la ubicación de los datos
Establezca la ubicación donde descargó los datos en el parámetro dir de los datos. También puede usar sus propios datos; el conjunto de datos de entrada debe tener un formato de archivo *.tsv.

```python
import os
import pandas as pd

#set the working directory where to save the training data files
resources_dir = os.path.join(os.path.expanduser("~"), "tatk", "resources")
data_dir = os.path.join(os.path.expanduser("~"), "tatk", "data")

from sklearn.datasets import fetch_20newsgroups
twenty_train = fetch_20newsgroups(data_home=data_dir, subset='train')
X_train, y_train = twenty_train.data, twenty_train.target
df_train = pd.DataFrame({"text":X_train, "label":y_train})

twenty_test = fetch_20newsgroups(data_home=data_dir, subset='test')
X_test, y_test = twenty_test.data, twenty_test.target   
df_test = pd.DataFrame({"text":X_test, "label":y_test})
    
# Training Dataset Location
#training_file_path = <specify-your-own-training-data-file-path-here>
# df_train = pd.read_csv(training_file_path,
#                        sep = '\t',                        
#                        header = 0, names= <specify-your-column-name-list-here>)
df_train.head()
print("df_train.shape= {}".format(df_train.shape))

# Test Dataset Location
#test_file_path = <specify-your-own-test-data-file-path-here>
# df_test = pd.read_csv(test_file_path,
#                       sep = '\t',                        
#                       header = 0, names= <specify-your-column-name-list-here>)

print("df_test.shape= {}".format(df_test.shape))
df_test.head()
```
    df_train.shape= (11314, 2)
    df_test.shape= (7532, 2)
 
 Los datos constan de label y text.
    
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>label</th>
      <th>text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7</td>
      <td>From: v064mb9k@ubvmsd.cc.buffalo.edu (NEIL B. ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>From: Rick Miller &lt;rick@ee.uwm.edu&gt;\nSubject: ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>From: mathew &lt;mathew@mantis.co.uk&gt;\nSubject: R...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>From: bakken@cs.arizona.edu (Dave Bakken)\nSub...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>19</td>
      <td>From: livesey@solntze.wpd.sgi.com (Jon Livesey...</td>
    </tr>
  </tbody>
</table>
</div>

Obtenga la correspondencia entre las categorías y su nombre.

```python
int_to_categories = pd.DataFrame({'category':range(20), 'category_name': list(twenty_train.target_names)})
int_to_categories 
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>categoría</th>
      <th>category_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>alt.atheism</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>comp.graphics</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>comp.os.ms-windows.misc</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>comp.sys.ibm.pc.hardware</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>comp.sys.mac.hardware</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>comp.windows.x</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>misc.forsale</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>rec.autos</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>rec.motorcycles</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>rec.sport.baseball</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>rec.sport.hockey</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>sci.crypt</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>sci.electronics</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>sci.med</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>sci.space</td>
    </tr>
    <tr>
      <th>15</th>
      <td>15</td>
      <td>soc.religion.christian</td>
    </tr>
    <tr>
      <th>16</th>
      <td>16</td>
      <td>talk.politics.guns</td>
    </tr>
    <tr>
      <th>17</th>
      <td>17</td>
      <td>talk.politics.mideast</td>
    </tr>
    <tr>
      <th>18</th>
      <td>18</td>
      <td>talk.politics.misc</td>
    </tr>
    <tr>
      <th>19</th>
      <td>19</td>
      <td>talk.religion.misc</td>
    </tr>
  </tbody>
</table>
</div>

Ahora puede crear un histograma de trazado de exploración preliminar de la frecuencia de clase en los conjuntos de datos de entrenamiento y prueba. 

```python
import numpy as np
import math
from matplotlib import pyplot as plt

data = df_train["label"].values
labels = set(data)
print(labels)
bins = range(len(labels)+1) 

#plt.xlim([min(data)-5, max(data)+5])

plt.hist(data, bins=bins, alpha=0.8)
plt.title('training data distribution over the class labels)')
plt.xlabel('class label')
plt.ylabel('frequency')
plt.grid(True)
plt.show()

data = df_test["label"].values
labels = set(data)
print(labels)
bins = range(len(labels)+1) 

#plt.xlim([min(data)-5, max(data)+5])

plt.hist(data, bins=bins, alpha=0.8)
plt.title('test data distribution over the class labels)')
plt.xlabel('class label')
plt.ylabel('frequency')
plt.grid(True)
plt.show()
```

Al ejecutar Jupyter Notebook, se muestran los trazados después de ejecutar el bloque de código anterior.


## <a name="train-the-model"></a>Entrenamiento del modelo

### <a name="specify-scikit-learn-algorithm-and-define-the-text-classifier"></a>Especificación del algoritmo scikit-learn y definición del clasificador de texto

Este paso implica entrenar un modelo de clasificación de texto de scikit-learn con el algoritmo de aprendizaje de regresión logística Uno versus el resto.

Para ver la lista completa de aprendizajes, consulte la documentación para [aprendices de Scikit](http://scikit-learn.org/stable/supervised_learning).

```python
from sklearn.linear_model import LogisticRegression
import tatk
from tatk.pipelines.text_classification.text_classifier import TextClassifier

log_reg_learner =  LogisticRegression(penalty='l2', dual=False, tol=0.0001, 
                            C=1.0, fit_intercept=True, intercept_scaling=1, 
                            class_weight=None, random_state=None, 
                            solver='lbfgs', max_iter=100, multi_class='ovr',
                            verbose=1, warm_start=False, n_jobs=3) 

#train the model a text column "tweets"
text_classifier = TextClassifier(estimator=log_reg_learner, 
                                text_cols = ["text"], 
                                label_cols = ["label"], 
#                                 numeric_cols = None,
#                                 cat_cols = None, 
                                extract_word_ngrams=True, extract_char_ngrams=True)

```

    TextClassifier::create_pipeline ==> start
    :: number of jobs for the pipeline : 12
    0   text_nltk_preprocessor
    1   text_word_ngrams
    2   text_char_ngrams
    3   assembler
    4   learner
    TextClassifier::create_pipeline ==> end
    
### <a name="fit-the-model"></a>Ajuste del modelo

Use los parámetros predeterminados del paquete. De manera predeterminada, el clasificador de texto extrae:
+ Unigramas y bigramas de palabras.
+ Cuatrigramas de caracteres.

```python
text_classifier.fit(df_train)        
```
   
    TextClassifier::fit ==> start
    schema: col=label:I4:0 col=text:TX:1 header+
    NltkPreprocessor::tatk_fit_transform ==> start
    NltkPreprocessor::tatk_fit_transform ==> end     Time taken: 0.08 mins
    NGramsVectorizer::tatk_fit_transform ==> startNGramsVectorizer::tatk_fit_transform ==> start
    
                vocabulary size=216393
    NGramsVectorizer::tatk_fit_transform ==> end     Time taken: 0.41 mins
                vocabulary size=67230
    NGramsVectorizer::tatk_fit_transform ==> end     Time taken: 0.49 mins
    VectorAssembler::transform ==> start, num of input records=11314
    (11314, 216393)
    (11314, 67230)
    all_features::
    (11314, 283623)
    Time taken: 0.06 mins
    VectorAssembler::transform ==> end
    LogisticRegression::tatk_fit ==> start
    
    [Parallel(n_jobs=3)]: Done  20 out of  20 | elapsed:  2.4min finished
    
    LogisticRegression::tatk_fit ==> end     Time taken: 2.4 mins
    Time taken: 3.04 mins
    TextClassifier::fit ==> end

    TextClassifier(add_index_col=False, callable_proprocessors_list=None,
            cat_cols=None, char_hashing_original=False, col_prefix='tmp_00_',
            decompose_n_grams=False, detect_phrases=False,
            dictionary_categories=None, dictionary_file_path=None,
            embedding_file_path=None, embedding_file_path_fastText=None,
            estimator=None, estimator_vectorizers_list=None,
            extract_char_ngrams=True, extract_word_ngrams=True,
            label_cols=['label'], numeric_cols=None,
            pos_tagger_vectorizer=False,
            preprocessor_dictionary_file_path=None, regex_replcaement='',
            replace_regex_pattern=None, scale_numeric_cols=False,
            text_callable_list=None, text_cols=['text'], text_regex_list=None,
            weight_col=None)


Durante el entrenamiento, debe tener columnas de texto y etiqueta. Mientras tanto, solo la columna de texto es necesaria para las predicciones. 

### <a name="examine-and-set-the-parameters-of-the-different-pipeline-steps"></a>Análisis y establecimiento de los parámetros de los distintos pasos de la canalización
    
Por lo general, los parámetros se establecen antes de ajustar un modelo. 

***Ejemplo con text_word_ngrams*** 

En los ejemplos de código siguientes se muestra cómo entrenar el modelo con los parámetros predeterminados de canalización y modelo. 

Para ver los parámetros que se incluyen para "text_word_ngrams", use [get_step_param_names_by_name](https://docs.microsoft.com/python/api/tatk.core.base_text_model.basetextmodel). Esta función devuelve los parámetros como lowercase, input_col, output_col, etc. 

```python
text_classifier.get_step_param_names_by_name("text_word_ngrams")
```

    ['min_df',
     'strip_accents',
     'max_df',
     'decode_error',
     'max_features',
     'binary',
     'input',
     'vocabulary',
     'analyzer',
     'token_pattern',
     'encoding',
     'use_idf',
     'save_overwrite',
     'output_col',
     'stop_words',
     'sublinear_tf',
     'input_col',
     'lowercase',
     'ngram_range',
     'preprocessor',
     'tokenizer',
     'hashing',
     'dtype',
     'norm',
     'smooth_idf',
     'n_hashing_features']

Luego, compruebe los valores de parámetro de "text_char_ngrams":

```python
text_classifier.get_step_params_by_name("text_char_ngrams")        
```
    {'analyzer': 'char_wb',
     'binary': False,
     'decode_error': 'strict',
     'dtype': numpy.float32,
     'encoding': 'utf-8',
     'hashing': False,
     'input': 'content',
     'input_col': 'NltkPreprocessor5283a730506549cc880f074e750607b0',
     'lowercase': True,
     'max_df': 1.0,
     'max_features': None,
     'min_df': 3,
     'n_hashing_features': None,
     'ngram_range': (4, 4),
     'norm': 'l2',
     'output_col': 'NGramsVectorizer8eb11031f6b64eaaad9ff0fd3b0f5b80',
     'preprocessor': None,
     'save_overwrite': True,
     'smooth_idf': True,
     'stop_words': None,
     'strip_accents': None,
     'sublinear_tf': False,
     'token_pattern': '(?u)\\b\\w\\w+\\b',
     'tokenizer': None,
     'use_idf': True,
     'vocabulary': None}

Si es necesario, puede cambiar los parámetros predeterminados.  Con el código siguiente, puede cambiar el rango de n-gramas de caracteres extraídos de (4,4) a (3,4) para extraer tanto las trigramas como las cuatrigramas de caracteres:

```python
text_classifier.set_step_params_by_name("text_char_ngrams", ngram_range =(3,4)) 
text_classifier.get_step_params_by_name("text_char_ngrams")
```
    {'analyzer': 'char_wb',
     'binary': False,
     'decode_error': 'strict',
     'dtype': numpy.float32,
     'encoding': 'utf-8',
     'hashing': False,
     'input': 'content',
     'input_col': 'NltkPreprocessor5283a730506549cc880f074e750607b0',
     'lowercase': True,
     'max_df': 1.0,
     'max_features': None,
     'min_df': 3,
     'n_hashing_features': None,
     'ngram_range': (3, 4),
     'norm': 'l2',
     'output_col': 'NGramsVectorizer8eb11031f6b64eaaad9ff0fd3b0f5b80',
     'preprocessor': None,
     'save_overwrite': True,
     'smooth_idf': True,
     'stop_words': None,
     'strip_accents': None,
     'sublinear_tf': False,
     'token_pattern': '(?u)\\b\\w\\w+\\b',
     'tokenizer': None,
     'use_idf': True,
     'vocabulary': None}

### <a name="export-the-parameters-to-a-file"></a>Exportación de los parámetros a un archivo
Si es necesario, puede optimizar el rendimiento del modelo si vuelve a ejecutar el paso de ajuste del modelo con los parámetros revisados:

```python
import os
params_file_path = os.path.join(data_dir, "params.tsv")
text_classifier.export_params(params_file_path)
```

## <a name="apply-the-classifier"></a>Aplicación del clasificador

Aplique el clasificador de texto entrenado en el conjunto de datos de prueba para generar predicciones de clase:

```python
 df_test = text_classifier.predict(df_test)
```

    TextClassifier ::predict ==> start
    NltkPreprocessor::tatk_transform ==> start
    NltkPreprocessor::tatk_transform ==> end     Time taken: 0.05 mins
    NGramsVectorizer::tatk_transform ==> startNGramsVectorizer::tatk_transform ==> start
    
    NGramsVectorizer::tatk_transform ==> end     Time taken: 0.15 mins
    NGramsVectorizer::tatk_transform ==> end     Time taken: 0.37 mins
    VectorAssembler::transform ==> start, num of input records=7532
    (7532, 216393)
    (7532, 67230)
    all_features::
    (7532, 283623)
    Time taken: 0.03 mins
    VectorAssembler::transform ==> end
    LogisticRegression::tatk_predict_proba ==> start
    LogisticRegression::tatk_predict_proba ==> end   Time taken: 0.01 mins
    LogisticRegression::tatk_predict ==> start
    LogisticRegression::tatk_predict ==> end     Time taken: 0.01 mins
    Time taken: 0.46 mins
    TextClassifier ::predict ==> end
    Order of Labels in predicted probabilities saved to attribute label_order of the class object
    
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>label</th>
      <th>text</th>
      <th>probabilities</th>
      <th>prediction</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7</td>
      <td>From: v064mb9k@ubvmsd.cc.buffalo.edu (NEIL B. ...</td>
      <td>[0.0165036341329, 0.0548664746458, 0.020549685...</td>
      <td>12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>From: Rick Miller &lt;rick@ee.uwm.edu&gt;\nSubject: ...</td>
      <td>[0.025145498995, 0.125877400021, 0.03947047877...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>From: mathew &lt;mathew@mantis.co.uk&gt;\nSubject: R...</td>
      <td>[0.67566338235, 0.0150749738583, 0.00992439163...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>From: bakken@cs.arizona.edu (Dave Bakken)\nSub...</td>
      <td>[0.146063943868, 0.00232465192179, 0.002442807...</td>
      <td>18</td>
    </tr>
    <tr>
      <th>4</th>
      <td>19</td>
      <td>From: livesey@solntze.wpd.sgi.com (Jon Livesey...</td>
      <td>[0.670712265297, 0.017332269703, 0.01062429663...</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>

## <a name="evaluate-model-performance"></a>Evaluación del rendimiento del modelo
El [módulo de evaluación](https://docs.microsoft.com/python/api/tatk.evaluation) evalúa la precisión del clasificador de texto entrenado en el conjunto de datos de prueba. La función de evaluación genera una matriz de confusión y brinda una puntuación macro-F1.

```python
 text_classifier.evaluate(df_test)          
```

    TextClassifier ::evaluate ==> start
    Time taken: 0.0 mins
    TextClassifier ::evaluate ==> end
    

Trace la matriz de confusión sin normalización para visualización.


```python
evaluator.plot_confusion_matrix(normalize=False,
                                title='Confusion matrix, without normalization', 
                                print_confusion_matrix=False,
                                figsize=(8,8),
                                colors=None)
```

Matriz de confusión sin normalización
    
Cuando ejecute el Notebook, aparecerá la matriz de confusión.



Trace la matriz de confusión normalizada para visualización.


```python
evaluator.plot_confusion_matrix(normalize=True,
                                title='Normalized Confusion matrix', 
                                print_confusion_matrix=False,
                                figsize=(8,8),
                                colors=None)
```

    Normalized confusion matrix
   
Cuando ejecute el Notebook, aparecerá la matriz de confusión.

## <a name="save-the-pipeline"></a>Guardado de la canalización
Guarde la canalización de clasificación en un archivo ZIP. Además, guarde las ngramas de palabras y las ngramas de caracteres como archivos de texto.

```python
import os
working_dir = os.path.join(data_dir, 'outputs')  
if not os.path.exists(working_dir):
    os.makedirs(working_dir)

# you can save the trained model as a folder or a zip file
model_file = os.path.join(working_dir, 'sk_model.zip')    
text_classifier.save(model_file)
# %azureml upload outputs/models/sk_model.zip

```
    BaseTextModel::save ==> start
    TatkPipeline::save ==> start
    Time taken: 0.28 mins
    TatkPipeline::save ==> end
    Time taken: 0.38 mins
    BaseTextModel::save ==> end
    
```python
# for debugging, you can save the word n-grams vocabulary to a text file
word_vocab_file_path = os.path.join(working_dir, 'word_ngrams_vocabulary.tsv')
text_classifier.get_step_by_name("text_word_ngrams").save_vocabulary(word_vocab_file_path) 
# %azureml upload outputs/dictionaries/word_ngrams_vocabulary.pkl

# for debugging, you can save the character n-grams vocabulary to a text file
char_vocab_file_path = os.path.join(working_dir, 'char_ngrams_vocabulary.tsv')
text_classifier.get_step_by_name("text_char_ngrams").save_vocabulary(char_vocab_file_path) 
# %azureml upload outputs/dictionaries/char_ngrams_vocabulary.pkl
```

    save_vocabulary ==> start
    saving 216393 n-grams ...
    Time taken: 0.01 mins
    save_vocabulary ==> end
    save_vocabulary ==> start
    saving 67230 n-grams ...
    Time taken: 0.0 mins
    save_vocabulary ==> end
 
## <a name="load-the-pipeline"></a>Carga de la canalización
Cargue la canalización de clasificación y las ngramas de palabras y las ngramas de caracteres para inferencia:

```python
# in order to deploy the trained model, you have to load the zip file of the classifier pipeline
loaded_text_classifier = TextClassifier.load(model_file)

from tatk.feature_extraction import NGramsVectorizer
word_ngram_vocab = NGramsVectorizer.load_vocabulary(word_vocab_file_path)
char_ngram_vocab = NGramsVectorizer.load_vocabulary(char_vocab_file_path)
```
    BaseTextModel::load ==> start
    TatkPipeline::load ==> start
    Time taken: 0.14 mins
    TatkPipeline::load ==> end
    Time taken: 0.15 mins
    BaseTextModel::load ==> end
    loading 216393 n-grams ...
    loading 67230 n-grams ...
    

## <a name="test-the-pipeline"></a>Prueba de la canalización

Para evaluar un conjunto de datos de prueba, aplique la canalización de clasificación de texto cargada:

```python
predictions = loaded_text_classifier.predict(df_test)
loaded_evaluator = loaded_text_classifier.evaluate(predictions)
loaded_evaluator.get_metrics('macro_f1')
```
    TextClassifier ::predict ==> start
    NltkPreprocessor::tatk_transform ==> start
    NltkPreprocessor::tatk_transform ==> end     Time taken: 0.05 mins
    NGramsVectorizer::tatk_transform ==> startNGramsVectorizer::tatk_transform ==> start
    
    NGramsVectorizer::tatk_transform ==> end     Time taken: 0.14 mins
    NGramsVectorizer::tatk_transform ==> end     Time taken: 0.36 mins
    VectorAssembler::transform ==> start, num of input records=7532
    (7532, 216393)
    (7532, 67230)
    all_features::
    (7532, 283623)
    Time taken: 0.03 mins
    VectorAssembler::transform ==> end
    LogisticRegression::tatk_predict_proba ==> start
    LogisticRegression::tatk_predict_proba ==> end   Time taken: 0.01 mins
    LogisticRegression::tatk_predict ==> start
    LogisticRegression::tatk_predict ==> end     Time taken: 0.01 mins
    Time taken: 0.45 mins
    TextClassifier ::predict ==> end
    Order of Labels in predicted probabilities saved to attribute label_order of the class object
    TextClassifier ::evaluate ==> start
    Time taken: 0.0 mins
    TextClassifier ::evaluate ==> en
    
    0.82727029243808903

## <a name="operationalization-deploy-and-consume"></a>Operacionalización: implementación y consumo

En esta sección, se implementa la canalización de clasificación de texto como un servicio web de Azure Machine Learning con la [CLI de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/cli-for-azure-machine-learning). Luego se consume el servicio web para entrenamiento y puntuación.

**Inicio de sesión en la suscripción de Azure con la CLI de Azure**

Con una cuenta [Azure](https://azure.microsoft.com/) con una suscripción válida, inicie sesión con el comando de CLI siguiente:
<br>`az login`

+ Para cambiar a otra suscripción de Azure, use el comando:
<br>`az account set --subscription [your subscription name]`

+ Para ver la cuenta de administración del modelo actual, use el comando:
  <br>`az ml account modelmanagement show`

**Creación y establecimiento del entorno de implementación**

Solo tiene que establecer el entorno de implementación una vez. Si todavía no tiene uno, configure ahora mismo el entorno de implementación con [estas instrucciones](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration#environment-setup). 

1. Asegúrese de que el entorno de Azure Machine Learning, la cuenta de administración del modelo y el grupo de recursos se encuentren en la misma región.

1. Descargue el archivo de configuración de la implementación de Blob Storage y guárdelo en el entorno local:

   ```python
   # Download the deployment config file from Blob storage `url` and save it locally under `file_name`:
   deployment_config_file_url = 'https://aztatksa.blob.core.windows.net/dailyrelease/tatk_deploy_config.yaml'
   deployment_config_file_path=os.path.join(resources_dir, 'tatk_deploy_config.yaml')
   import urllib.request
   urllib.request.urlretrieve(deployment_config_file_url, deployment_config_file_path)
   ```

1. Actualice el archivo de configuración de implementación que descargó para reflejar los recursos:

   ```python
   web_service_name = 'please type your web service name'
   working_directory= os.path.join(resources_dir, 'deployment') 

   web_service = text_classifier.deploy(web_service_name= web_service_name, 
                          config_file_path=deployment_config_file_path,
                          working_directory= working_directory)  
   ```

1. Dado que el modelo de aprendizaje se implementó correctamente, invoque el servicio web de puntuación en el conjunto de datos nuevo:

   ```python
   print("Service URL: {}".format(web_service._service_url))
   print("Service Key: {}".format(web_service._api_key))
   ```

1. Cargue el servicio web en cualquier momento con su nombre:

   ```python
   from tatk.operationalization.csi.csi_web_service import CsiWebService
   url = "<please type the service URL here>"
   key = "<please type the service Key here>"
   web_service = CsiWebService(url, key)
   ```

1. Pruebe el servicio web con el cuerpo de dos correos electrónicos tomados del conjunto de datos de 20 grupos de noticias:

   ```python
   # Example input data for scoring
   import json
   dict1 ={}
   dict1["recordId"] = "a1" 
   dict1["data"]= {}
   dict1["data"]["text"] = """
   I'd be interested in a copy of this code if you run across it.
   (Mail to the author bounced)
    > / hpldsla:comp.graphics / email-address-removed / 12:53 am  May 13,
    1993 /
    > I fooled around with this problem a few years ago, and implemented a
    > simple method that ran on a PC.
    > was very simple - about 40 or 50 lines of code.
    . . .
    > Somewhere I still have it
    > and could dig it out if there was interest.
   """
   
   dict2 ={}
   dict2["recordId"] = "b2"
   dict2["data"] ={}
   dict2["data"]["text"] = """
   >>Could the people discussing recreational drugs such as mj, lsd, mdma, etc.,
   >>take their discussions to alt.drugs? Their discussions will receive greatest
   >>contribution and readership there. The people interested in strictly
   >>"smart drugs" (i.e. Nootropics) should post to this group. The two groups
   >>(alt.drugs & alt.psychoactives) have been used interchangably lately.
   >>I do think that alt.psychoactives is a deceiving name. alt.psychoactives
   >>is supposedly the "smart drug" newsgroup according to newsgroup lists on
   >>the Usenet. Should we establish an alt.nootropics or alt.sdn (smart drugs &
   >>nutrients)? I have noticed some posts in sci.med.nutrition regarding
   >>"smart nutrients." We may lower that groups burden as well.
   >   

   I was wondering if a group called 'sci.pharmacology' would be relevent.
   This would be used for a more formal discussion about pharmacological
   issues (pharmacodynamics, neuropharmacology, etc.)      

   Just an informal proposal (I don't know anything about the net.politics
   for adding a newsgroup, etc.)

   """

   dict_list =[dict1, dict2]
   data ={}
   data["values"] = dict_list
   input_data_json_str = json.dumps(data)
   print (input_data_json_str)
   prediction = web_service.score(input_data_json_str)
   prediction
   ```

   ```
   {"values": [{"recordId": "a1", "data": {"text": "\nI'd be interested in a copy of this code if you run across it.\n(Mail to the author bounced)\n > / hpldsla:comp.graphics / email-address-removed / 12:53 am  May 13,\n 1993 /\n > I fooled around with this problem a few years ago, and implemented a\n > simple method that ran on a PC.\n > was very simple - about 40 or 50 lines of code.\n . . .\n > Somewhere I still have it\n > and could dig it out if there was interest.\n"}}, {"recordId": "b2", "data": {"text": "\n>>Could the people discussing recreational drugs such as mj, lsd, mdma, etc.,\n>>take their discussions to alt.drugs? Their discussions will receive greatest\n>>contribution and readership there. The people interested in strictly\n>>\"smart drugs\" (i.e. Nootropics) should post to this group. The two groups\n>>(alt.drugs & alt.psychoactives) have been used interchangably lately.\n>>I do think that alt.psychoactives is a deceiving name. alt.psychoactives\n>>is supposedly the \"smart drug\" newsgroup according to newsgroup lists on\n>>the Usenet. Should we establish an alt.nootropics or alt.sdn (smart drugs &\n>>nutrients)? I have noticed some posts in sci.med.nutrition regarding\n>>\"smart nutrients.\" We may lower that groups burden as well.\n>\n\nI was wondering if a group called 'sci.pharmacology' would be relevent.\nThis would be used for a more formal discussion about pharmacological\nissues (pharmacodynamics, neuropharmacology, etc.)\n\nJust an informal proposal (I don't know anything about the net.politics\nfor adding a newsgroup, etc.)\n\n"}}]}
   F1 2018-05-02 00:10:58,272 INFO Web service scored. 
    
   '{"values": [{"recordId": "b2", "data": {"text": "\\n>>Could the people discussing recreational drugs such as mj, lsd, mdma, etc.,\\n>>take their discussions to alt.drugs? Their discussions will receive greatest\\n>>contribution and readership there. The people interested in strictly\\n>>\\"smart drugs\\" (i.e. Nootropics) should post to this group. The two groups\\n>>(alt.drugs & alt.psychoactives) have been used interchangably lately.\\n>>I do think that alt.psychoactives is a deceiving name. alt.psychoactives\\n>>is supposedly the \\"smart drug\\" newsgroup according to newsgroup lists on\\n>>the Usenet. Should we establish an alt.nootropics or alt.sdn (smart drugs &\\n>>nutrients)? I have noticed some posts in sci.med.nutrition regarding\\n>>\\"smart nutrients.\\" We may lower that groups burden as well.\\n>\\n\\nI was wondering if a group called \'sci.pharmacology\' would be relevent.\\nThis would be used for a more formal discussion about pharmacological\\nissues (pharmacodynamics, neuropharmacology, etc.)\\n\\nJust an informal proposal (I don\'t know anything about the net.politics\\nfor adding a newsgroup, etc.)\\n\\n", "class": 13}}, {"recordId": "a1", "data": {"text": "\\nI\'d be interested in a copy of this code if you run across it.\\n(Mail to the author bounced)\\n > / hpldsla:comp.graphics / email-address-removed / 12:53 am  May 13,\\n 1993 /\\n > I fooled around with this problem a few years ago, and implemented a\\n > simple method that ran on a PC.\\n > was very simple - about 40 or 50 lines of code.\\n . . .\\n > Somewhere I still have it\\n > and could dig it out if there was interest.\\n", "class": 1}}]}'
   ```


## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre Azure Machine Learning Package for Text Analytics en estos artículos:

+ Lea la [información general del paquete](https://aka.ms/aml-packages/text).

+ Explore la [documentación de referencia](https://aka.ms/aml-packages/text) de este paquete.

+ Obtenga información sobre [otros paquetes Python para Azure Machine Learning](reference-python-package-overview.md).