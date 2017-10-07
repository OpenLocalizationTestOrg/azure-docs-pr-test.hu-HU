---
title: "aaaData modell kompatibilitási szint az Azure Analysis Services |} Microsoft Docs"
description: "Táblázatos modell kompatibilitási szintnek ismertetése."
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
ms.date: 08/16/2017
ms.author: owend
ms.openlocfilehash: bfaf0c60666729d1e6e0baf082c046ea9faa4e86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Az Analysis Services rendszerbeli táblázatos modellek kompatibilitási szintjének

*Kompatibilitási szint* toorelease-specifikus viselkedések az Analysis Services-motor hello hivatkozik. Módosítások toohello kompatibilitási szint általában egybe az SQL Server kiadások. Ezeket a módosításokat az Azure Analysis Services toomaintain paritása mindkét platform is valósíthatók meg. Kompatibilitási szint módosítások is érintik a táblázatos modellek szolgáltatásához. Például DirectQuery és az objektum táblázatos metaadatok rendelkezik különböző megvalósítások attól függően, hogy hello kompatibilitási szintet. 

Az Azure Analysis Services hello 1200-as és 1 400 kompatibilitási szintű táblázatos modellek támogatja.

hello legfrissebb kompatibilitási szintet a 1 400. Ez a szint az SQL Server 2017 Analysis Services megegyezik. Fő szolgáltatásainak hello 1 400 kompatibilitási szintje a következők:

*  Új infrastruktúra adatkapcsolat és a táblázatos modellek történő TOM API-k és TMSL parancsfájlkezelés támogatása. Ezen új szolgáltatás lehetővé teszi, hogy a további adatforrások, például az Azure Blob-tároló támogatása.
*  Adatok átalakítása és adatok adategyesítési képességeket adatok beolvasása és M kifejezések használatával.
*  Mértékek egy DAX-kifejezés az adott sorok tulajdonsághoz támogatja. Ez a tulajdonság lehetővé teszi, hogy az ügyfél eszközök, például a Microsoft Excel toodrill egy összesített jelentés toodetailed-adatok. Például ha a felhasználó egy régiót, és a hónap összes értékesítés, akkor megtekintheti a kapcsolódó hello rendelés részleteit. 
*  A tábla és oszlop objektumszintű biztonsági nevet, továbbá toohello adatok rajtuk.
*  Továbbfejlesztett támogatása az egyenetlen szélű hierarchiákat.
*  Teljesítmény- és továbbfejlesztések.
  
## <a name="set-compatibility-level"></a>Kompatibilitási szint beállítása 
 Amikor létrehoz egy új táblázatos modell projektet SSDT, megadhatja hello kompatibilitási szint hello **táblázatos modellek tervezőjében** párbeszédpanel. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Ha hello **ne jelenjen meg többé ez az üzenet** lehetőség, minden ezt követő projektek használata hello alapértelmezettként megadott hello kompatibilitási szintet. Hello alapértelmezett kompatibilitási szint az SSDT-ben **eszközök** > **beállítások**.  
  
 az SSDT, set hello meglévő táblázatos modell projekt tooupgrade **kompatibilitási szint** hello modellben tulajdonság **tulajdonságok** ablak. A szem előtt tartani, hello kompatibilitási szint frissítése nem vonható vissza.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>Az SQL Server Management Studio táblázatos modell adatbázis kompatibilitási szintjének ellenőrzése 
 Az SSMS, kattintson a jobb gombbal hello adatbázis neve > **tulajdonságok** > **kompatibilitási szint**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>Támogatott kompatibilitási szint szolgáltatáshoz az ssms kiszolgáló  
 Az SSMS, kattintson a jobb gombbal hello kiszolgáló neve > **tulajdonságok** > **támogatott kompatibilitási szint**.  
  
 Ez a tulajdonság meghatározza a legmagasabb kompatibilitási szintje hello egy adatbázist, amely (kivéve az előzetes verzió) hello kiszolgálón fog futni. hello támogatott kompatibilitási szintje nem módosítható.  

## <a name="next-steps"></a>Következő lépések
  [A modell létrehozása Azure-portálon](analysis-services-create-model-portal.md)   
  [Analysis Services kezelése](analysis-services-manage.md)  
