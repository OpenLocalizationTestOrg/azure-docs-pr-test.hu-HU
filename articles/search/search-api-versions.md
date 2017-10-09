---
title: "Azure Search aaaAPI verzióinak |} Microsoft Docs"
description: "Az Azure Search REST API-k és hello ügyféloldali kódtára a .NET SDK hello házirendje."
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 0458053a-164e-4682-a802-00097ecde981
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/11/2017
ms.author: brjohnst
ms.openlocfilehash: 4fa722fad5577c6b254be7fa673eb240fff316a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="api-versions-in-azure-search"></a>Az Azure Search API-verziók
Az Azure Search rendszeresen bevezeti szolgáltatás-frissítéseket. Néha, de nem mindig nekünk a ezeket a frissítéseket toopublish az API-toopreserve előző verziókkal való kompatibilitás újabb verziója szükséges. Közzététele egy új verzió lehetővé teszi toocontrol mikor és hogyan integrálja a keresési szolgáltatás frissítéseiről a kódban.

Szabály azt próbálja toopublish új verzió csak akkor, ha szükséges, mivel az egyes elérhető tooupgrade magába foglaló a kód toouse egy olyan új API-verzió. Új verzió csak azt tesznek közzé, ha igazolnia kell toochange hello API oly módon, hogy megszakítja a visszamenőleges kompatibilitás érdekében bizonyos elemeit. Ez akkor fordulhat elő, javítások tooexisting szolgáltatásokat, vagy módosítsa a meglévő API felületet biztosít új funkciókat.

Ugyanaz a szabály SDK frissítések hello kövesse azt. hello Azure Search SDK a következő hello [szemantikai versioning](http://semver.org/) szabályok, ami azt jelenti, hogy ugyanolyan verziójú három részből áll: a fő alverziószám és buildszáma (például 1.1.0-ás). A Microsoft hello SDK törés az előző verziókkal való kompatibilitás változások esetén csak az új főverzió ad ki. Nem törhető szolgáltatás-frissítéseket azt hello alverzió növeli, és hibajavítások a csak megnöveli a Microsoft hello buildszámát.

> [!NOTE]
> Az Azure Search szolgáltatás példányát támogatja több REST API-t, többek között a legújabb hello. Egy verzió toouse tovább már nem hello legújabb, de azt javasoljuk, hogy áttelepítette-e a kód toouse hello legújabb verziója. Hello REST API használata esetén minden kérelemben hello api-version paraméter segítségével adjon meg a hello API-verzió. Hello .NET SDK használata esetén a hello használata SDK hello verziója hello megfelelő hello REST API verziója határozza meg. Ha egy régebbi SDK használ, folytathatja toorun ezt a kódot, módosítások nélküli akkor is, ha hello szolgáltatás frissített toosupport egy újabb API-verzió.

## <a name="snapshot-of-current-versions"></a>Aktuális verzió pillanatképe
Az alábbiakban van hello pillanatképet minden programozási felületek tooAzure keresési aktuális verziója.

| Felületek | Legutóbbi főverzió | status |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |3.0 |Általában akkor érhető el, November 2016 kiadott |
| [.NET SDK minta](https://aka.ms/search-sdk-preview) |2.0 előzetes verzió |2016 augusztusától, amely a minta |
| [Szolgáltatás REST API-ja](https://docs.microsoft.com/rest/api/searchservice/) |2016-09-01 |Általánosan elérhető |
| [Szolgáltatás REST API minta](search-api-2015-02-28-preview.md) |2015-02-28 – előzetes verzió |Előzetes verzió |
| [.NET-kezelési SDK](https://aka.ms/search-mgmt-sdk) |2015-08-19 |Általánosan elérhető |
| [Kezelési REST API](https://docs.microsoft.com/rest/api/searchmanagement/) |2015-08-19 |Általánosan elérhető |

A REST API-k, beleértve a hello hello `api-version` minden egyes hívásakor szükség. Így könnyen tootarget egy bizonyos verzióra, például egy előnézeti API. hello alábbi példa bemutatja, hogyan hello `api-version` paraméter meg van adva:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2016-09-01

> [!NOTE]
> Bár egyes kérelem egy `api-version`, azt javasoljuk, hogy használja-e hello ugyanolyan verziójú API-t minden olyan kérelem esetében. Ez akkor különösen igaz olyan esetben, ha új API-verziók bevezetni attribútumot, vagy nem ismeri fel a korábbi verziók műveletek. API-verziók keverése rendelkezhet nem várt következményekkel és el kell kerülni.
>
> hello szolgáltatás REST API és a felügyeleti REST API-t is rendszerverzióval ellátott egymástól függetlenül. A verziószámokra bármilyen hasonlóság előfordulhatnak.

Általánosan elérhető (vagy GA) API-k éles környezetben használható, és tulajdonos tooAzure garantált szolgáltatási szintek. Az előzetes verziókban kísérleti funkciók, amelyek nem mindig áttelepített tooa GA verziójával rendelkezik. **Kifejezetten ajánlott termelési alkalmazások API-k megtekintés ellen.**

## <a name="about-preview-and-generally-available-versions"></a>Előzetes és általánosan elérhető verziójának kapcsolatos
Az Azure Search mindig előzetes kiadását hello REST API-n keresztül kísérleti funkciók először, majd kiadás előtti verzióját hello .NET SDK használatával.

Előzetes verziójú funkciók nem garantált toobe tooa GA kiadás át. Mivel a szolgáltatások GA verziójával minősülnek stabil és szokatlan toochange kis visszamenőlegesen kompatibilis javítások és továbbfejlesztett hello kivételével, előzetes verziójú funkciók érhetők el a teszteléshez és kísérletezés hello célja a visszajelzéseket a szolgáltatás megtervezését és megvalósítását.

Azonban mivel előzetes verziójú funkciók tulajdonos toochange, ajánlott ellen, amely egy függőséget az előzetes verziókban a termelési kód írása. Ha egy régebbi előzetes verzióját használja, ajánlott az áttelepítés toohello általánosan elérhető (GA) verziója.

A .NET SDK hello: található kód áttelepítési útmutató a [frissítési hello .NET SDK](search-dotnet-sdk-migration.md).

Általános rendelkezésre állási azt jelenti, hogy Azure Search most hello szolgáltatásiszint-szerződéssel (SLA) alapján. hello SLA helyen találhatók [Azure keresési szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
