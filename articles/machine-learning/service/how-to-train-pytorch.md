---
title: Entrenamiento de modelos de PyTorch con Azure Machine Learning
description: Aprenda a ejecutar el entrenamiento distribuido de modelos de PyTorch y de nodo único con el estimador de PyTorch
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 27d4ad03e4a7f911fe3c9981618337a2fff51317
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114624"
---
# <a name="how-to-train-pytorch-models"></a>Entrenamiento de modelos de PyTorch

Para el entrenamiento de red neuronal profunda (DNN) con PyTorch, Azure Machine Learning proporciona una clase personalizada de `PyTorch` de `Estimator`. El estimador de `PyTorch` del SDK de Azure le permite enviar fácilmente los trabajos de entrenamiento de PyTorch para las ejecuciones de nodo único y distribuidas en Azure Compute.

## <a name="single-node-training"></a>Entrenamiento de nodo único
El entrenamiento con el estimador de `PyTorch` es similar a usar el [base`Estimator`](how-to-train-ml-models.md), así que primero debe leer el artículo de instrucciones y comprender los conceptos que presenta.
  
Para ejecutar un trabajo de PyTorch, cree una instancia de un objeto `PyTorch`. Debería haber creado su objeto de [destino de proceso](how-to-set-up-training-targets.md#batch) `compute_target` y su objeto de [almacén de datos](how-to-access-data.md) `ds`.

```Python
from azureml.train.dnn import PyTorch

script_params = {
    '--data_dir': ds
}

pt_est = PyTorch(source_directory='./my-pytorch-proj',
                 script_params=script_params,
                 compute_target=compute_target,
                 entry_script='train.py',
                 use_gpu=True)
```

En este caso, se especifican los siguientes parámetros al constructor de PyTorch:
Parámetro | DESCRIPCIÓN
--|--
`source_directory` |  Directorio local que contiene todo el código necesario para el trabajo de aprendizaje. Esta carpeta se copia desde el equipo local al proceso remoto
`script_params` |  Diccionario que especifica los argumentos de línea de comandos para el script de aprendizaje `entry_script`, en forma de pares <argumento de línea de comandos, valor>
`compute_target` |  Proceso remoto que ejecutará el script de aprendizaje, en este caso un clúster [Batch AI](how-to-set-up-training-targets.md#batch)
`entry_script` |  Ruta del archivo (relativa al `source_directory`) del script de aprendizaje que se va a ejecutar en el proceso remoto. Este archivo y los archivos adicionales de los que depende deben encontrarse en esta carpeta
`conda_packages` |  Lista de paquetes de Python para instalarse mediante Conda que necesita el script de aprendizaje. El constructor tiene otro parámetro llamado `pip_packages` que puede usar para todos los paquetes de PIP necesitados
`use_gpu` |  Establezca esta marca en `True` para aprovechar la GPU para el entrenamiento. De manera predeterminada, su valor es `False`.

Puesto que utiliza el estimador de `PyTorch`, el contenedor utilizado para el entrenamiento incluirá el paquete de PyTorch y las dependencias relacionadas necesarias para el entrenamiento en las CPU y los GPU.

A continuación, envíe el trabajo de PyTorch:
```Python
run = exp.submit(pt_est)
```

## <a name="distributed-training"></a>Entrenamiento distribuido
El estimador de `PyTorch` también permite entrenar los modelos a escala mediante clústeres de GPU y CPU de las máquinas virtuales de Azure. Puede ejecutar fácilmente el entrenamiento distribuido de PyTorch con unas pocas llamadas de API, mientras que Azure Machine Learning administrará en segundo plano toda la infraestructura y orquestación necesaria para llevar a cabo estas cargas de trabajo.

Actualmente, Azure Machine Learning admite el entrenamiento distribuido de PyTorch con MPI mediante la plataforma Horovod.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) es una plataforma de ring-allreduce de código abierto para entrenamiento distribuido desarrollado por Uber.

Para ejecutar PyTorch distribuido mediante la plataforma Horovod, cree el objeto de PyTorch como sigue:

```Python
from azureml.train.dnn import PyTorch

pt_est = PyTorch(source_directory='./my-pytorch-project',
                 script_params={},
                 compute_target=compute_target,
                 entry_script='train.py',
                 node_count=2,
                 process_count_per_node=1,
                 distributed_backend='mpi',
                 use_gpu=True)
```

Este código expone los siguientes parámetros nuevos para el constructor de PyTorch:
Parámetro | DESCRIPCIÓN | Valor predeterminado
--|--|--
`node_count` |  Número de nodos que se usará para el trabajo de aprendizaje. | `1`
`process_count_per_node` |  Número de procesos (o "trabajos") que se ejecutarán en cada nodo. | `1`
`distributed_backend` |  Back-end para iniciar el entrenamiento distribuido, que ofrece el estimador mediante MPI.  Para llevar a cabo un aprendizaje paralelo o distribuido (por ejemplo, `node_count` > 1, o `process_count_per_node` > 1, o bien ambos) con MPI (y Horovod), establezca `distributed_backend='mpi'`. La implementación de MPI utilizada por Azure Machine Learning es [Open MPI](https://www.open-mpi.org/). | `None`

El ejemplo anterior ejecutará un entrenamiento distribuido con dos trabajos, un trabajo por nodo.

Horovod y sus dependencias se instalarán automáticamente, para que simplemente pueda importarlo en el script de entrenamiento `train.py` como sigue:
```Python
import torch
import horovod
```

Por último, envíe el trabajo de PyTorch distribuido:
```Python
run = exp.submit(pt_est)
```

## <a name="examples"></a>Ejemplos
Para ver un tutorial sobre el aprendizaje de PyTorch de nodo único, consulte:
* [training/01.train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/tree/master/training/01.train-hyperparameter-tune-deploy-with-pytorch)

Para ver un tutorial sobre PyTorch distribuido con Horovod, consulte:
* [training/02.distributed-pytorch-with-horovod](https://github.com/Azure/MachineLearningNotebooks/blob/master/training/02.distributed-pytorch-with-horovod)

Obtenga estos cuadernos:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes
* [Seguir métricas de ejecución durante el entrenamiento](how-to-track-experiments.md)
* [Ajustar los hiperparámetros](how-to-tune-hyperparameters.md)
* [Implementar un modelo entrenado](how-to-deploy-and-where.md)
