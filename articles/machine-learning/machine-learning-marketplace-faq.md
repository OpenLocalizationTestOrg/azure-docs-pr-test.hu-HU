---
title: "Gyakori kérdések – aaa(deprecated) közzétételéhez és használatához a Machine Learning-alkalmazások az Azure piactérről |} Microsoft Docs"
description: "(elavult) Gépi tanulás alkalmazások közzétételi hello Azure piactér kapcsolatos gyakori kérdések"
services: machine-learning
documentationcenter: 
author: bharaths
manager: jhubbard
editor: cgronlun
ms.assetid: 26b3a1e7-8b9a-4004-98bc-17456d4c25e8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: b3ae45dfff211fe9baccaf54faaf360a8309c780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-publishing-and-using-machine-learning-apps-in-hello-azure-marketplace-faq"></a>(elavult) Közzététel és a Machine Learning-alkalmazások használata az Azure piactér hello: gyakran ismételt kérdések

> [!NOTE]
> DataMarket és Data Services rendszer hatókörről, és a meglévő előfizetések rendszerből, és elindítása megszakítva 3/31/2017. Ennek köszönhetően ez a cikk is elavult. 
> 
> Másik megoldásként, közzéteheti a Machine Learning kísérleteket toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) hello adatok tudományos Közösség hello juttatásra. További információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).


## <a name="questions-about-consuming-from-marketplace"></a>Piactérről fel kapcsolatos kérdések
**1. Miért kapok hello bemeneti hello webszolgáltatáshoz beírása után a következő hibaüzenet jelenik meg:**

**hello kérés egy háttér-időtúllépés vagy a háttér-hibát eredményezett. hello team hello problémát vizsgál. Hello kellemetlenségért elnézését. (500)**

A bemeneti paraméterek nem felelnek meg az adott webszolgáltatás hello toohello formátum. A bemeneti paraméterek és a webszolgáltatás hello korlátozásai tekintse meg a toohello megfelelő dokumentáció hivatkozás toofind hello formátuma helytelen.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**2. Látható hello "Intéző ehhez az adatkészlethez" lapot, és illessze be egy másik böngészőablakban, milyen hitelesítő adatokat kell I tooaccess hello eredmények használja, és hogyan hello webszolgáltatáshoz hello API-hivatkozás másolása látható őket?**

Piactér-fiókját hello felhasználónév és a hello elsődleges fiókkulcs hello jelszó célszerű használni. hello elsődleges fiókkulcs hello található **ehhez az adatkészlethez felfedezés** lapján hello webszolgáltatás hello leírása (hello kattintson **megjelenítése** gombra). hello eredménye lehet, hogy hello böngészőben megjelenő, vagy elérhető, vagy túl letöltési, attól függően, hogy mely böngészőt használ.

**3. Miért kapok hello hello bemeneti hello webszolgáltatáshoz hello "Intéző ehhez az adatkészlethez" lapon beírása után a következő hibaüzenet jelenik meg:** 

**Váratlan hiba történt a kérés feldolgozása közben. Próbálkozzon újra.**

Egy vagy több bemeneti paraméterek, a webszolgáltatás túllépte hello korlát amikor hello webszolgáltatás hello piactéren fel **ehhez az adatkészlethez felfedezés** lap. hello szolgáltatás már bemeneti adat hosszánál HTTP POST metódusok használatával hívható. Tekintse meg a [R használata a Machine Learning és a közzétett tooMarketplace megoldások minta](machine-learning-r-csharp-web-service-examples.md).

**4. Miért nem látom semmit hello "API EXPLORER" lapon int hello tároló a klasszikus Azure portál hello?** 

Ez az egy ismert probléma az Azure klasszikus portál piactér hello. hello csoport dolgozik tooresolve probléma. 

## <a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a>Azure Machine Learning piactéren történő közzétételre vonatkozó kérdések
**1. Miért van a tranzakciók emblémák vagy képek nem frissíti a webszolgáltatáshoz?** 

Emblémák és lemezképek gyorsítótárba kerüljenek-e hello közzétételi portálon, és új embléma hello vagy kép tooupdate hello portál too10 nap igénybe vehet.

**2. Miért van hello "Részletek" lap piactér hibaüzenetet jelenít meg a webszolgáltatás?**

Egy ismert probléma piactér van tooAzure Machine Learning szolgáltatás részletei a csatlakozáskor. hello csoport dolgozik tooresolve probléma.

**3. Miért hello R mintakód hello Azure Machine Learning web Services esetében nem működik a piactéren fogyasztó hello webszolgáltatások?**

hello hitelesítési rendszereket különböző tooconnecting toothese webszolgáltatások hello piactéren keresztül közvetlenül csatlakozó tooAzure Machine Learning webszolgáltatások képest. a piactéren hello szolgáltatások OData-szolgáltatásaival, és a GET vagy POST metódusok hívható. 

**4. Miért van hello támogatási hivatkozások a webszolgáltatás nem frissül megfelelően a ajánlatok részénél kínál?**

hello támogatási hivatkozások / publisher, nem az ajánlat / globálisak. 

**5. Hogyan tehetők közzé egy webszolgáltatás-bővítmény bemeneti kötegelt módban a piactéren?**

hello kötegelt bemeneti mód jelenleg nem támogatott a piactér webszolgáltatások.

**6. Ki lehet kapcsolatba lépni tooget súgó Ha kérdéseik vannak a közzétevő váljon, vagy ha közzététele során problémába ütközik??**

Lépjen kapcsolatba a hello Azure piactér csapatának < mailto:datamarketbd@microsoft.com > további információt.

