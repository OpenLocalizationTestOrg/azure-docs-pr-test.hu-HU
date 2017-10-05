---
title: "(elavult) Gyakori kérdések – közzétételéhez és használatához a Machine Learning-alkalmazások az Azure piactérről |} Microsoft Docs"
description: "(elavult) Közzétételi Machine Learning-alkalmazások az Azure piactéren kapcsolatos gyakori kérdések"
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
redirect_document_id: TRUE
ms.openlocfilehash: a4631dfeb2f817b3a3c612a8f6af48398e4f2ab9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-publishing-and-using-machine-learning-apps-in-the-azure-marketplace-faq"></a>(elavult) Kiadása és használata a Machine Learning-alkalmazások az Azure piactéren: gyakran ismételt kérdések

> [!NOTE]
> DataMarket és Data Services rendszer hatókörről, és a meglévő előfizetések rendszerből, és elindítása megszakítva 3/31/2017. Ennek köszönhetően ez a cikk is elavult. 
> 
> Alternatív megoldásként közzéteheti a Machine Learning kísérleteket, hogy a [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) az adatok tudományos közösségi érdekében. További információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery erőforrások](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).


## <a name="questions-about-consuming-from-marketplace"></a>Piactérről fel kapcsolatos kérdések
**1. Miért kapok a következő hibaüzenet jelenik meg a webszolgáltatás bemeneti beírása után:**

**A kérés egy háttér-időtúllépés vagy a háttér-hibát eredményezett. A csapata vizsgálja a problémát. Elnézését kérjük a kellemetlenségért. (500)**

A bemeneti paraméterek nem felelnek meg az adott webszolgáltatás megkívánt formátumban. Tekintse meg a megfelelő dokumentáció-hivatkozás található a bemeneti paraméterekben megfelelő formátum és a webszolgáltatás vonatkozó korlátozások.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**2. Ha a webszolgáltatás, amelyet a "Intéző ehhez az adatkészlethez", és illessze be azt egy másik böngészőben ablakba, milyen hitelesítő adatokat kell látható API hivatkozásra másolása az eredmények eléréséhez használni, és hogyan tekinthető meg őket?**

A jelszót a piactér fiók célszerű használni a felhasználónév és az elsődleges kulcsa. Az elsődleges kulcsa megtalálható a **ehhez az adatkészlethez felfedezés** a webszolgáltatás leírását a lap (kattintson a **megjelenítése** gombra). Az eredmény jeleníthet meg a böngészőben vagy elképzelhető, hogy töltse le, a böngésző használatával.

**3. Miért jelenik meg hibaüzenet, a bemeneti a webszolgáltatás az "Ez az adatkészlet böngészés" oldalon beírása után:** 

**Váratlan hiba történt a kérés feldolgozása közben. Próbálkozzon újra.**

Egy vagy több bemeneti paraméterek, a webszolgáltatás túllépte a maximális hossz amikor fel a webszolgáltatás a piactéren **ehhez az adatkészlethez felfedezés** lap. A szolgáltatások egy már bemeneti adat hosszánál HTTP POST metódusok használatával hívható. Tekintse meg a [R használata a Machine Learning megoldások mintát, és teszi közzé az piactér](machine-learning-r-csharp-web-service-examples.md).

**4. Miért nem látom sem a "API EXPLORER" lapon int a tároló, a klasszikus Azure-portálon?** 

Ez az egy ismert probléma az Azure klasszikus portál Piactéri. A csapat dolgozik a probléma megoldásához. 

## <a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a>Azure Machine Learning piactéren történő közzétételre vonatkozó kérdések
**1. Miért van a tranzakciók emblémák vagy képek nem frissíti a webszolgáltatáshoz?** 

Emblémák és lemezképek gyorsítótárba kerüljenek-e a közzétételi portálon, és új embléma vagy a portál frissítése kép legfeljebb 10 napig is eltarthat.

**2. Miért van a "Részletek" lap piactér hibaüzenetet jelenít meg a webszolgáltatás?**

Egy ismert piactér probléma van a szolgáltatás részletes adatai az Azure Machine Learning való csatlakozáshoz. A csapat dolgozik a probléma megoldásához.

**3. Miért az Azure Machine Learning webszolgáltatások R példakód nem működik a piactér webszolgáltatások felhasználása?**

A hitelesítési rendszerekkel eltérőek, ezek a webszolgáltatások a piactéren keresztül csatlakozik közvetlenül képest Azure Machine Learning webszolgáltatások történő csatlakozás során. A szolgáltatásoknak a piactér OData-szolgáltatásaival, és a GET vagy POST metódusok hívható. 

**4. Miért van a támogatási hivatkozások a webszolgáltatás nem frissül megfelelően a ajánlatok részénél kínál?**

A támogatási hivatkozások / publisher, nem az ajánlat / globálisak. 

**5. Hogyan tehetők közzé egy webszolgáltatás-bővítmény bemeneti kötegelt módban a piactéren?**

A bemeneti kötegelt módban jelenleg nem támogatott a piactér webszolgáltatások.

**6. Ki kell kapcsolatba Ha segítséget szeretne kérni, ha kérdéseik vannak a közzétevő váljon, vagy ha közzététele során problémába ütközik??**

Lépjen kapcsolatba az Azure piactér csapatának < mailto:datamarketbd@microsoft.com > további információt.

