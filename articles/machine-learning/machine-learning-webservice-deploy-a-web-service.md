---
title: "egy új, az Azure Machine Learning webszolgáltatás-bővítmény aaaDeploying |} Microsoft Docs"
description: "hello munkafolyamat üzembe az ARM-alapú webszolgáltatás"
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
redirect_document_id: True
ms.openlocfilehash: 2cbfda44b97a6b992fbdfdfb0c761e6c9e169035
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-new-web-service"></a>Új webszolgáltatás üzembe helyezése
A Microsoft Azure Machine learning most biztosít alapuló webszolgáltatások [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) lehetővé teszi a új számlázási csomag beállítások és a webes szolgáltatás toomultiple régiók telepítése.

hello általános munkafolyamat toodeploy egy webszolgáltatás-bővítmény a Microsoft Azure Machine Learning webszolgáltatások használatával van:

* A prediktív kísérlet létrehozása
* Telepítés
* Konfigurálja a neve
* számlázási csomag
* Tesztelheti
* ezért használja fel.

a következő ábra hello hello munkafolyamat mutatja be.

![Webes szolgáltatás telepítési munkafolyamat][1]

## <a name="deploy-web-service-from-studio"></a>A Studio webszolgáltatás telepítése
kísérlet egy új webszolgáltatás toodeploy. Jelentkezzen be a Machine Learning Studio hello, és hozzon létre egy új prediktív webszolgáltatás-bővítmény. 

**Megjegyzés:**: Ha már telepítette, egy kísérlet egy klasszikus webszolgáltatás nem telepítheti azt egy új webszolgáltatás.

Kattintson a **futtatása** hello hello alján kísérletezhet a vászonra, és kattintson a **webes szolgáltatás telepítése** és **[Új] webes szolgáltatás telepítése**. hello Machine Learning webszolgáltatás Manager hello telepítési lap nyílik meg.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Machine Learning Web Service Manager telepítése kísérlet lap
Hello telepítése kísérlet oldalon adjon meg egy nevet, hello webszolgáltatáshoz.
Válasszon egy tarifacsomagot. Rendelkezik egy meglévő tarifacsomagot is kiválaszthatja, ellenkező esetben kell létrehoznia egy új ár terv hello szolgáltatást. 

1. A hello **ár terv** legördülő listán, jelöljön ki egy meglévő terv vagy hello **jelölje be új csomagot** lehetőséget.
2. A **neve**, adjon meg egy nevet, amely azonosítja a számlán hello terv.
3. Válasszon ki egy hello **havi megtervezése rétegek**. Ne feledje, hogy hello terv rétegek alapértelmezett toohello tervek az alapértelmezett régiójában a webszolgáltatás üzembe helyezett toothat régióban.

Kattintson a **telepítés** és a webszolgáltatás hello gyors üzembe helyezés lap nyílik meg.

## <a name="quickstart-page"></a>Gyors üzembe helyezés lap
hello webes szolgáltatás gyors üzembe helyezés lap lehetővé teszi az access és útmutatást hello leggyakoribb feladatokat kell végrehajtania egy új webszolgáltatás-bővítmény létrehozása után a. Itt egyszerűen elérhetők mindkét hello **teszt** lap és **felhasználás** lap.

## <a name="testing-your-web-service"></a>A webszolgáltatás tesztelése
Hello gyors üzembe helyezés lapon kattintson a gyakori feladatok teszt webes szolgáltatás.   

tootest hello webszolgáltatás, egy kérés-válasz szolgáltatás (RR-EKET):

* Kattintson a **teszt** hello menüsávjában.
* Kattintson a **kérés-válasz**.
* Adja meg a megfelelő értékeket hello bemeneti oszlopok kísérletbe.
* Kattintson a vizsgálat **kérés-válasz**.

Hello jobb oldalán található hello megjeleníteni kívánt eredményeket.

a kötegelt végrehajtási szolgáltatás (BES) webszolgáltatás tootest, fog használni egy CSV-fájlt:

* Kattintson a **teszt** hello menüsávjában.
* Kattintson a **kötegelt**.
* A bemeneti alatt kattintson a Tallózás gombra, és keresse meg a tooyour mintaadatfájlokat.
* Kattintson a **teszt**.

a teszt hello állapota akkor jelenik meg a **tesztelése kötegelt feladatok**.

## <a name="consuming-your-web-service"></a>A webszolgáltatás felhasználása
Amikor telepít egy webszolgáltatás, az Azure Machine Learning kísérleteket a REST API számos különféle eszközök és rendszerek által felhasználható adja meg. Ennek az az oka hello egyszerű REST API fogad el, és válaszol, JSON formátumú üzenetek. hello Azure Machine Learning portal biztosít, amely használható kód toocall hello webszolgáltatás R, C# és Python.

Hello felhasználása lapon találja meg:

* hello API-kulcs és URI-azonosítói webszolgáltatás alkalmazásokban felhasználásához tartozó.
* Az Excel és a webes alkalmazás sablonok tookick a fogyasztás folyamat elindításához.
* A C#, python, és R tooget mintakód elindult.

A webszolgáltatások felhasználása további információkért lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Következő lépések
Webszolgáltatások felhasználása további információkért lásd:

[Hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md)

[Az Azure Machine Learning webszolgáltatások: Telepítés és használat](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
