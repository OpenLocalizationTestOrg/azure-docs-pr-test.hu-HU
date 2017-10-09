---
title: "aaaHow tooquery graph adatokat az Adatbázisba az Azure Cosmos? | Microsoft Docs"
description: "További tudnivalók az Azure Cosmos Adatbázisba tooquery Diagramadatok"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
tags: 
ms.assetid: 8bde5c80-581c-4f70-acb4-9578873c92fa
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: denlee
ms.openlocfilehash: fdde881edd6c488e2fea51e5c9665e1d736009fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-with-hello-graph-api-preview"></a>A Cosmos DB Azure: Hogyan tooquery a hello Graph API-val (előzetes verzió)?

hello Azure Cosmos DB [Graph API](graph-introduction.md) (előzetes verzió) támogatja a [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/) lekérdezések. Ez a cikk minta dokumentumok biztosít, és lekérdezi a tooget-t elindította. Részletes Gremlin hivatkozás megtalálható hello [Gremlin támogatási](gremlin-support.md) cikk.

Ez a cikk ismerteti a következő feladatok hello: 

> [!div class="checklist"]
> * Gremlin az adatok lekérdezése

## <a name="prerequisites"></a>Előfeltételek

Az alábbi lekérdezéseket toowork van Azure Cosmos DB-fiókja és Diagramadatok hello tárolóban van. Nem rendelkezik egyetlen, az? Teljes hello [5 perces gyors üzembe helyezés](create-graph-dotnet.md) vagy hello [fejlesztői útmutató](tutorial-query-graph.md) toocreate egy fiókot, és töltse fel az adatbázist. Futtathatja a következő lekérdezést hello hello [Azure Cosmos DB .NET graph könyvtár](graph-sdk-dotnet.md), [Gremlin konzol](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), vagy a kedvenc Gremlin illesztőprogramot.

## <a name="count-vertices-in-hello-graph"></a>Count csúcsban hello Graph

a következő kódrészletet hello bemutatja, hogyan toocount hello hello Graph csúcsban száma:

```
g.V().count()
```

## <a name="filters"></a>Szűrők

Szűrők Gremlin tartozó használatával végezheti el `has` és `hasLabel` lépéseit, és egyesíthet használatával `and`, `or`, és `not` toobuild összetettebb szűrők. Azure Cosmos DB biztosít a csúcsban és gyors lekérdezések fok belül az összes tulajdonság séma-független indexelése:

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a>Leképezése

Kivetítheti az egyes tulajdonságok hello lekérdezés eredményében hello segítségével `values` . lépés:

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a>Kapcsolódó szélek és csúcsban keresése

Eddig is csak láttuk lekérdezési operátorok, amelyek működnek a bármely adatbázis. Diagramok esetén gyors és hatékony átjárás műveletekhez toonavigate toorelated szélek és csúcsban van szükség. Keressük Thomas összes barátok. Jelenleg ezt úgy teheti meg Gremlin `outE` toofind összes hello kimenő éleinek Thomas a lépést, majd toohello a-csúcsban áthaladó Gremlin tartozó használatával szegélyek a `inV` . lépés:

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

hello a következő lekérdezés hajt végre két ugrások toofind összes Thomas' "ismerősök olyan ismerőseinek", meghívásával `outE` és `inV` kétszer. 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

Összetettebb lekérdezések létrehozhatja és hatékony graph átjárás logika használatával Gremlin, többek között a következőket keverési szűrő kifejezések használatával ismétlési végrehajtása hello megvalósítása `loop` lépést, és végrehajtási feltételes navigációs hello segítségével `choose` lépés. További információ a teendők [Gremlin támogatási](gremlin-support.md)!

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Megtudta, hogyan tooquery Graph használatával 

Most már folytathatja toohello következő útmutató toolearn hogyan toodistribute az adatok globálisan.

> [!div class="nextstepaction"]
> [Az adatok globálisan terjesztése](tutorial-global-distribution-documentdb.md)