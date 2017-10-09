---
title: a Power BI Analysis Services aaaConnect tooAzure |} Microsoft Docs
description: "Ismerje meg, hogyan tooconnect tooan Azure Analysis Services-kiszolgáló a Power BI használatával."
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
ms.openlocfilehash: f6c4cdec6edb92900ad2e552e23a4d9172ba9b84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-power-bi"></a>Csatlakozás a Power BI

Miután elkészítette a kiszolgáló Azure-ban, és egy táblázatos modell tooit telepített, a szervezet felhasználói készen tooconnect és adatok. 

> [!TIP]
> Lehet, hogy toouse hello legújabb verziójának [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Csatlakozás a Power BI Desktop

1. A Power BI Desktopban, kattintson a **adatok beolvasása** > **Azure** > **Azure Analysis Services-adatbázis**.

2. A **Server**, adjon meg hello kiszolgálónevet. 
    
    Lehet, hogy tooinclude hello teljes URL-címet. Például asazure://westcentralus.asazure.windows.net/advworks.

3. A **adatbázis**, ha tudja hello hello táblázatos modellű adatbázisnál vagy azt szeretné, hogy a, illessze be ide tooconnect perspektíva nevét. Ellenkező esetben hagyja üresen a mezőt, és jelöljön ki egy adatbázist vagy perspektíva később.

4. Hagyja hello alapértelmezett **élő csatlakozás** lehetőséget, majd nyomja le az **Connect**. 

5. Ha a rendszer kéri, adja meg a bejelentkezési hitelesítő adatait. 

6. A **Navigator**, hello kiszolgálót, majd válassza ki a hello modell vagy perspektíva tooconnect számára, majd kattintson a kívánt **Connect**. Kattintson a modell vagy perspektíva tooshow, ha a nézet összes hello objektum.

    a jelentések nézetének üres jelentést a Power BI Desktop hello modell megnyitása. hello mezők listája nem rejtett modell objektumait. A kapcsolat állapota hello jobb alsó sarkában jelenik meg.

## <a name="connect-in-power-bi-service"></a>Csatlakozás a Power bi-ban (szolgáltatás)

1. Hozzon létre egy Power BI Desktop fájlt, amely rendelkezik egy élő kapcsolattal tooyour modell a kiszolgálón.
2. A [Power BI](https://powerbi.microsoft.com), kattintson a **adatok beolvasása** > **fájlok**. Keresse meg és jelölje ki a fájlt.



## <a name="see-also"></a>Lásd még:
[Csatlakozás tooAzure Analysis Services](analysis-services-connect.md)   
[Ügyfél-függvénytárak](analysis-services-data-providers.md)

