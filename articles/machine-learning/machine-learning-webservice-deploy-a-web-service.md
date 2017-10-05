---
title: "Egy új, az Azure Machine Learning webszolgáltatás üzembe helyezése |} Microsoft Docs"
description: "A munkafolyamat üzembe az ARM-alapú webszolgáltatás"
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: a358b04f-0d08-4d50-820e-eeac971854cf
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: v-donglo
ROBOTS: NOINDEX
redirect_url: machine-learning-publish-a-machine-learning-web-service
redirect_document_id: TRUE
ms.openlocfilehash: 1415709f9da2bb2cce859af9feb0ec15c1fa5801
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-new-web-service"></a>Új webszolgáltatás üzembe helyezése
A Microsoft Azure Machine learning most biztosít alapuló webszolgáltatások [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) lehetővé teszi a új számlázási csomag beállítások és a webszolgáltatás telepítése több területre.

Az általános munkafolyamat használatával a Microsoft Azure Machine Learning webszolgáltatások webes szolgáltatás telepítése a következő:

* A prediktív kísérlet létrehozása
* Telepítés
* Konfigurálja a neve
* számlázási csomag
* Tesztelheti
* ezért használja fel.

Az alábbi ábra mutatja be a munkafolyamat.

![Webes szolgáltatás telepítési munkafolyamat][1]

## <a name="deploy-web-service-from-studio"></a>A Studio webszolgáltatás telepítése
A kísérlet telepítendő új webszolgáltatásként. Jelentkezzen be a Machine Learning Studio eszközben, és hozzon létre egy új prediktív webszolgáltatás-bővítmény. 

**Megjegyzés:**: Ha már telepítette, egy kísérlet egy klasszikus webszolgáltatás nem telepítheti azt egy új webszolgáltatás.

Kattintson a **futtatása** a lap alján a kísérlet vászonra, és kattintson a **webes szolgáltatás telepítése** és **[Új] webes szolgáltatás telepítése**. A Machine Learning webszolgáltatás-kezelő a központi telepítés lap nyílik meg.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Machine Learning Web Service Manager telepítése kísérlet lap
A kísérletben telepítése lapon adja meg a webszolgáltatás nevét.
Válasszon egy tarifacsomagot. Ha egy meglévő tarifacsomagot is kiválaszthatja, ellenkező esetben kell létrehoznia egy új ár tervet a szolgáltatás. 

1. Az a **ár terv** legördülő listán, jelöljön ki egy meglévő terv vagy a **jelölje be új csomagot** lehetőséget.
2. A **neve**, adjon meg egy nevet, amely azonosítja a számlázási csomagot.
3. Válasszon egyet a a **havi megtervezése rétegek**. Vegye figyelembe, hogy a terv rétegek alapértelmezett régiójától és a webes szolgáltatás alapértelmezés szerint a rendszer adott régióban.

Kattintson a **telepítés** és megnyílik a gyors üzembe helyezés lap a webszolgáltatáshoz.

## <a name="quickstart-page"></a>Gyors üzembe helyezés lap
A webes szolgáltatás gyors üzembe helyezés lap lehetővé teszi az access és útmutatást a leggyakoribb feladatokat kell végrehajtania egy új webszolgáltatás-bővítmény létrehozása után. Itt egyszerűen elérhetők mind a **teszt** lap és **felhasználás** lap.

## <a name="testing-your-web-service"></a>A webszolgáltatás tesztelése
A gyors üzembe helyezés lapon kattintson a gyakori feladatok teszt webszolgáltatás.   

A webes szolgáltatás tesztelése, egy kérés-válasz szolgáltatás (RR-EKET):

* Kattintson a **teszt** menüsávjában.
* Kattintson a **kérés-válasz**.
* Adja meg a bemeneti oszlopok kísérletbe megfelelő értékeket.
* Kattintson a vizsgálat **kérés-válasz**.

A lap jobb oldalán a megjeleníteni kívánt eredményeket.

A kötegelt végrehajtási szolgáltatás (BES) webszolgáltatás teszteléséhez egy CSV-fájlt fogja használni:

* Kattintson a **teszt** menüsávjában.
* Kattintson a **kötegelt**.
* A bemeneti kattintson a Tallózás gombra, és navigáljon a minta adatfájl.
* Kattintson a **teszt**.

A vizsgálat állapotának jelenik meg **tesztelése kötegelt feladatok**.

## <a name="consuming-your-web-service"></a>A webszolgáltatás felhasználása
Amikor telepít egy webszolgáltatás, az Azure Machine Learning kísérleteket a REST API számos különféle eszközök és rendszerek által felhasználható adja meg. Ennek az az oka az egyszerű REST API fogad el, és megválaszolja az JSON formátumú üzenetek. Az Azure Machine Learning portal biztosít kódot, amely segítségével az R, C# és Python a webszolgáltatás hívására.

A felhasználása oldalon található:

* Az API-kulcs és az URI-azonosítói webszolgáltatás alkalmazásokban felhasználásához tartozó.
* Az Excel és a webes alkalmazás-sablonok indítsa a fogyasztás folyamat elindításához.
* Példakód C#, python, és R az első lépésekhez.

A webszolgáltatások felhasználása további információkért lásd: [hogyan kell használni az Azure Machine Learning Web service](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Következő lépések
Webszolgáltatások felhasználása további információkért lásd:

[Hogyan kell használni az Azure Machine Learning Web service](machine-learning-consume-web-services.md)

[Az Azure Machine Learning webszolgáltatások: Telepítés és használat](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
