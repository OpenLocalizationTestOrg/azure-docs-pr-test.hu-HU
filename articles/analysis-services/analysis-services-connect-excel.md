---
title: Analysis Services Excel aaaConnect tooAzure |} Microsoft Docs
description: "Ismerje meg, hogyan tooconnect tooan Azure Analysis Services Excel használatával."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 175b83e7d936716a626aa4b3bf22b5598bb983d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-excel"></a>Csatlakozás az Excellel

Miután elkészítette a kiszolgáló Azure-ban, és telepített egy táblázatos modell tooit, Ön készen tooconnect és adatok megkezdéséhez.


## <a name="connect-in-excel"></a>Csatlakozás az Excel programban

Kapcsolódás Excel tooa kiszolgáló támogatja az Excel 2016 adatok beolvasása paranccsal. Nem támogatott a Kapcsolódás a Power Pivot hello importálása varázsló használatával. 

**az Excel 2016 tooconnect**

1. Az Excel 2016, az hello **adatok** menüszalag, kattintson a **külső adatok beolvasása** > **egyéb forrásokból származó** > **Analysis Services** .

2. A hello Adatkapcsolat varázsló, a **kiszolgálónév**, többek között a protokollt és URI hello kiszolgálónevet adja meg. Ezt követően a **bejelentkezési adatok**, jelölje be **a következő felhasználónevet és jelszót használja hello**, majd írja be például a hello szervezeti felhasználónevet, nancy@adventureworks.com, és a jelszót.

    ![Az Excel bejelentkezéstől csatlakozás](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. A **adatbázis és tábla kijelölése**, hello adatbázis és a modell vagy perspektíva, és kattintson a **Befejezés**.
   
    ![Kapcsolódás Excel válassza modellből](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Lásd még:
[Ügyfél-függvénytárak](analysis-services-data-providers.md)   
[A kiszolgáló kezelése](analysis-services-manage.md)     


