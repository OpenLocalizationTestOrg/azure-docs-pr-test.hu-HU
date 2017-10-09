---
title: "aaaMulti-földrajzi súgót |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate munkaterület és egy webszolgáltatás-bővítmény közzététele az Azure-régióban hello Dél központi az Amerikai Egyesült Államok (SCUS) eltér az Azure-régió."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/6/2017
ms.author: tedway; neerajkh
ms.openlocfilehash: 77b055950ebfe329131b40e5f0a2f6be1e33c51e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="multi-geo-help-documentation"></a>Több régióra kiterjedő súgódokumentáció
Ez a cikk ismerteti, hogyan hozhat létre munkaterületet, és egy webszolgáltatás-bővítmény közzététele a különböző Azure-régiók.  Hello [Azure termékek régió lap](https://azure.microsoft.com/en-us/regions/services/) sorolja fel a régiókban, ahol az Azure Machine Learning áll rendelkezésre.

## <a name="create-a-workspace"></a>Munkaterületek létrehozása
1. Jelentkezzen be a klasszikus Azure portál toohello.
2. Kattintson a **+ új** > **ADATSZOLGÁLTATÁSOK** > **gépi tanulás** > **gyors létrehozás**.  A **hely** válassza ki, mint egy másik régióban **Délkelet-Ázsia**.
   ![1 kép multi-földrajzi Súgó][1]
3. Válasszon hello munkaterületet, és kattintson **bejelentkezési tooML Studio**.
   ![2 kép multi-földrajzi Súgó][2]
4. Most már rendelkezik egy munkaterület egy másik régióban, csakúgy, mint bármely más munkaterület használhat. a munkaterületek között tooswitch a hely toohello jobb felső sarkában a képernyőn. Kattintson a hello legördülő, jelölje be hello terület és jelölje ki hello munkaterület. Helyi toohello munkaterület régióban.  Például a létrehozott egy olyan munkaterületről, webszolgáltatások mindegyikét lesz hello ugyanabban a régióban hello munkaterületen található.
   ![3 kép multi-földrajzi Súgó][3]

## <a name="open-an-experiment-from-gallery"></a>Egy kísérlet megnyitásához a gyűjteményből
Ha egy kísérlet megnyitásához a gyűjteményből, is kiválaszthatja, melyik régiót szeretné toocopy hello végeztük el.

![4 kép multi-földrajzi Súgó][4a]

## <a name="web-service-management"></a>Rendszerfelügyeleti webszolgáltatás
tooprogrammatically webes szolgáltatások, például átképezési, használjon hello régióspecifikus cím kezelése:

* https://asiasoutheast.Management.azureml.NET
* https://europewest.Management.azureml.NET

### <a name="things-toonote"></a>Dolgot toonote
1. Csak olyan kísérletek toohello tartozó munkaterületek között másolhat ugyanabban a régióban ily módon. Ha szüksége toocopy kísérlet között különböző régiókban munkaterületek, használhatja a hello [PowerShell](http://aka.ms/amlps) parancsmag [ *másolási-AmlExperiment* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) tooaccomplish, amely. Egy másik megoldás toopublish hello kísérlet gyűjteménye a fel nem sorolt módban, majd nyissa meg a hello munkaterület a hello más régióban.
2. hello régió választó csak egy régió munkaterületek jelennek egyszerre.  
3. Egy szabad munkaterület vagy Vendég Access (névtelen) munkaterületen létrejön és Dél központi USA tárolva  
4. Egy olyan munkaterületről, Délkelet-Ázsiában található telepített webszolgáltatások is lehet üzemeltetni Délkelet-Ázsia.  

## <a name="more-information"></a>További információ
Tegyen fel kérdést a hello [Azure Machine Learning fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
