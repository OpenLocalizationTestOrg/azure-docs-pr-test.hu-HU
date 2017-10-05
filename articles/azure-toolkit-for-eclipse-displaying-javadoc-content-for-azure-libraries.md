---
title: "Javadoc tartalom megjelenítése az eclipse-ben a Javához készült Azure szalagtárak csomag"
description: "Az eclipse-ben az Azure-tárak Javadoc tartalom megjelenítési módját."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: b44deb773b2159cba1d5d957455409f10fc49334
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Javadoc tartalom megjelenítése az eclipse-ben a Javához készült Azure szalagtárak csomag
Az Azure-könyvtárakban Java Javadoc tartalmának tekintheti meg az Eclipse-környezetben a Javadoc tartalmat, hogy az Azure-könyvtárakban Java társításával. A következő lépések bemutatják a Ez a funkció Eclipse belül.

Ez az eljárás azt feltételezi, hogy már hozzáadta a Javához készült Azure könyvtárban a build elérési útját.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>A megjelenítendő Javadoc tartalom az eclipse-ben a Javához készült Azure-tárak
* Belül meg Eclipse Project Explorer az a **hivatkozott szalagtárak** szakasz a projekt, nyissa meg a helyi menü az Azure-könyvtárhoz a Java JAR. Például **microsoft-windowsazure-api-0.1.0.jar** (a verziószám lehet különböző, mely telepített verzió függ).

* Kattintson a **Tulajdonságok** elemre.

* Belül a **tulajdonságok** párbeszédpanelen, a bal oldali ablaktáblán kattintson a **Javadoc hely**. A **Javadoc hely** párbeszédpanel jelenik meg.

* Megadhat egy **Javadoc URL-cím**, vagy egy **az archívumban Javadoc**.

   * Ha úgy dönt, hogy adjon meg egy **Javadoc URL-cím**, használjon, mint az URL-címeket **http://dl.windowsazure.com/javadoc** vagy **http://dl.windowsazure.com/storage/javadoc**.

   * Ha úgy dönt, hogy használjon **Javadoc az archívumban**, megadhat egy külső fájlban, vagy a munkaterületen.

   A választás, és tallózással keresse meg vagy ellenőrzése igény szerint. A következő példa az Azure-könyvtárakban Java társítja a megfelelő Javadoc JAR helyileg letöltött nevű **c:\MyAzureJARs**.

   ![][ic553487]

* *Nem kötelező lépés*: kattintson a **érvényesítése**. Itt a Javadoc JAR potenciális problémákat sikerült megjeleníteni.

* Kattintson az **OK** gombra.

Miután a könyvtárhoz kapcsolódó, Javadoc tartalma megjelenjen-e az Eclipse IDE belül. Például ha `blob` van definiálva típus `CloudBlockBlob` a kód a következő az Javadoc tartalom, amely akkor jelenik meg, amikor beírja `blob.acquireLease` kódban:

![][ic553488]

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse] 

Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->
