---
title: Query Performance Insight en Azure Database for PostgreSQL
description: En este artículo se describe la característica Query Performance Insight en Azure Database for PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: cc041104169ca8c4344b9d3de597283d122e63db
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394767"
---
# <a name="query-performance-insight"></a>Información de rendimiento de consultas 

**Se aplica a:** Azure Database for PostgreSQL 9.6 y 10

> [!IMPORTANT]
> La característica Query Performance Insight está en versión preliminar pública en un número limitado de regiones. 

Query Performance Insight le ayuda a identificar rápidamente cuáles son las consultas que más tardan en ejecutarse, cómo cambian con el tiempo y qué esperas están afectándoles.

## <a name="permissions"></a>Permisos
Se necesitan los permisos **Propietario** o **Colaborador** para ver el texto de las consultas en Query Performance Insight. **Lector** pueden ver las tablas y los gráficos, pero no el texto de consulta.

## <a name="prerequisites"></a>Requisitos previos
Para que Query Performance Insight funcione, deben existir datos en el [Almacén de consultas](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Ver información de rendimiento
La vista [Query Performance Insight](concepts-query-performance-insight.md) en Azure Portal detectará visualizaciones en la información de clave del Almacén de consultas. 

En la página del portal de su servidor de Azure Database for PostgreSQL, seleccione **Query Performance Insight** en la sección **Soporte técnico y solución de problemas** de la barra de menús.

![Consultas de larga ejecución de Query Performance Insight](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

La pestaña **Consultas de larga ejecución** muestra las 5 principales consultas por promedio de duración por ejecución, que se agregan en intervalos de 15 minutos. Puede ver más consultas seleccionando en el lista desplegable **Número de consultas**. Al hacerlo, los colores del gráfico pueden cambiar a un identificador de consulta específico.

Puede hacer clic y arrastrar en el gráfico para restringir a un período de tiempo específico. Como alternativa, puede usar los iconos de acercar y alejar para ver un período de tiempo mayor o menor, respectivamente.

La tabla debajo del gráfico proporciona más información sobre las consultas de larga ejecución en ese período.

Seleccione la pestaña **Estadísticas de espera** para ver las visualizaciones correspondientes a esperas en el servidor.

![Estadísticas de espera de Query Performance Insight](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre la [supervisión y ajuste](concepts-monitoring.md) en Azure Database for PostgreSQL.


