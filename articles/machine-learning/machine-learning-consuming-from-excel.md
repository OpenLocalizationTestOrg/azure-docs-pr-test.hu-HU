---
title: "a Machine Learning webszolgáltatásba az Excelből aaaConsume |} Microsoft Docs"
description: "Az Azure Machine Learning webszolgáltatásba az Excelből felhasználása"
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
ms.assetid: 3f3cdd2f-1816-487e-ab78-530e01e9788f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/13/2017
ms.author: tedway
ms.openlocfilehash: e2e8bbf7ba75b6618a0285539555ce175ec03c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a>Azure Machine Learning webszolgáltatások használata az Excel programból
 Az Azure Machine Learning Studio segítségével könnyen toocall webszolgáltatások közvetlenül az Excelből hello kell toowrite kódok nélkül.

Ha az Excel 2013 (vagy újabb verzió) vagy az Excel Online, akkor azt javasoljuk, hogy használja-e hello Excel [Excel-bővítmény](machine-learning-excel-add-in-for-web-services.md).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a>Lépések
A webszolgáltatás közzététele. [Ezen a lapon](machine-learning-walkthrough-5-publish-web-service.md) ismerteti, hogyan toodo azt. Hello Excel-munkafüzet funkció jelenleg csak egy egyetlen kimeneti (Ez azt jelenti, hogy egy pontozási egycímkés) rendelkező kérelem/válasz szolgáltatásokat támogatott. 

Miután egy webszolgáltatás, kattintson a hello **WEBSZOLGÁLTATÁSOK** hello bal oldali hello Studio szakaszt, és válassza ki az Excelből hello web service tooconsume.

**Klasszikus webszolgáltatás**

1. A hello **IRÁNYÍTÓPULT** hello webszolgáltatás egy sort hello lapján **kérelem/válasz** szolgáltatás. Ha ez a szolgáltatás egyetlen kimeneti rendelkezett, megjelennie hello **töltse le az Excel-munkafüzet** sorhoz hivatkozásra.
   
    ![][1]
2. Kattintson a **töltse le az Excel-munkafüzet**.

**Új webszolgáltatás**

1. Hello Azure Machine Learning webszolgáltatás portálon, válassza ki a **felhasználás**.
2. A hello felhasználás lap hello **webes szolgáltatás felhasználásához beállításai** területen kattintson a hello Excel ikonra.

**Hello munkafüzet használatát**

1. Nyissa meg hello munkafüzet.
2. Biztonsági figyelmeztetés jelenik meg; Kattintson a hello **szerkesztésének engedélyezése** gombra.
   
    ![][2]
3. Biztonsági figyelmeztetés jelenik meg. Kattintson a hello **tartalom engedélyezése** toorun makrók a munkalapon gombra.
   
    ![][3]
4. Ha makrók engedélyezve vannak, egy tábla jön létre. Oszlopok kék is szükséges hello bevitt RR-EKET webes szolgáltatás, vagy **paraméterek**. Vegye figyelembe a hello RR-EKET szolgáltatás hello kimenete **ELŐREJELZETT ÉRTÉKEKET** zöld. Minden oszlop egy adott sor kitöltődnek, hello munkafüzet automatikusan meghívja a pontozási API hello és hello eredmények program pontozza a mennyiségeket jeleníti meg.
   
    ![][4]
5. tooscore több mint egy olyan sor, kitöltés hello második sor adatok és hello előre meghatározott értékek előállítása. Több sort is beillesztheti egyszerre.

(Diagramokat, energiagazdálkodási térkép, feltételes formázási stb) hello Excel szolgáltatások bármelyikét használhatja hello előre meghatározott értékek toohelp hello adatainak megjelenítése.    

## <a name="sharing-your-workbook"></a>A munkafüzet megosztása
A hello makrók toowork az API-kulcs hello számolótábla részének kell lennie. Ez azt jelenti, hogy ossza meg hello munkafüzetbe csak a megbízható entitások/személyeknek.

## <a name="automatic-updates"></a>Automatikus frissítések
Az RRS kezdeményezték a két ezekben a helyzetekben:

1. először egy sor tartalma az összes hello a **paraméterek**
2. Amikor bármelyik hello **paraméterek** kellett az összes sor változásairól a **paraméterek** megadott.

[1]: ./media/machine-learning-consuming-from-excel/excellink.png
[2]: ./media/machine-learning-consuming-from-excel/enableeditting.png
[3]: ./media/machine-learning-consuming-from-excel/enablecontent.png
[4]: ./media/machine-learning-consuming-from-excel/sampletable.png
