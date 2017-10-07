---
title: "Kapcsolódás az tooAzure Analysis Services szükséges aaaClient könyvtárak |} Microsoft Docs"
description: "Ismerteti a szükséges ügyfél alkalmazások és eszközök tooconnect Azure Analysis Services klienskódtárak segítségével"
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
ms.openlocfilehash: 74ba5c05ba76c6587c5aed38f200a1ba469aa4f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="client-libraries-for-connecting-tooazure-analysis-services"></a>Kapcsolódás az tooAzure Analysis Services klienskódtárak segítségével

Ügyféloldali kódtáraknál ügyfél alkalmazások és eszközök tooconnect tooAnalysis szolgáltatások kiszolgálók szükségesek. 

Analysis Services használata három klienskódtárak segítségével. ADOMD.NET és az Analysis Services Management Objects (AMO), olyan felügyelt klienskódtárak segítségével. hello Analysis Services OLE DB-szolgáltató (MSOLAP DLL) egy natív ügyfél könyvtárban. Általában három hello telepített ugyanannyi időt vesz igénybe. Az Azure Analysis Services hello legújabb verziója szükséges. 

Microsoft ügyfélalkalmazásokat, például a Power BI Desktop és Excel tárakat három ügyfél telepítéséhez. Előfordulhat azonban, attól függően, hogy hello verziója vagy a frissítési gyakoriságának klienskódtárak nem hello Azure Analysis Services által igényelt legújabb verziói. hello Ugyanez vonatkozik toocustom alkalmazások vagy egyéb felületek, például AsCmd, TOM, ADOMD.NET. Ezek az alkalmazások szükséges hello szalagtárak manuális telepítése. hello ügyfél szalagtárak manuális telepítésre terjeszthető csomag SQL Server szolgáltatáscsomagok szerepelnek. Azonban a ügyfél tárak kapcsolt toohello SQL Server verziója, és nem lehet hello legújabb.  

Ügyféloldali kódtáraknál ügyfélkapcsolatok adatok szolgáltatók szükséges tooconnect egy Azure Analysis Services-kiszolgáló tooa adatforrásból eltérnek. További információ az adatforrás-kapcsolatok esetén toolearn lásd [adatforrás-kapcsolatok](analysis-services-datasource.md).

## <a name="download-hello-latest-client-libraries"></a>Töltse le a legfrissebb klienskódtárak hello  
Ha éles környezetben, és teljesen elérhető és támogatott verziója szükséges a következő ügyfél függvénytárainak hello használata.

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Következő lépések
[Csatlakozás az Excel használatával](analysis-services-connect-excel.md)    
[Kapcsolódás PowerBI-jal](analysis-services-connect-pbi.md)
