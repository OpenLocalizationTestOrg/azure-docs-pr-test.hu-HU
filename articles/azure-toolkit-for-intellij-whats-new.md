---
title: "What's New in IntelliJ Azure eszköztára |} Microsoft Docs"
description: "További tudnivalók az Azure-eszközkészlet a legújabb funkciókat az intellij-t."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 46ed791f-df59-416a-809e-f52345ad973c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm;asirveda;martinsawicki
ms.openlocfilehash: 23714856f0f778be04cf321e0726b53ade430f57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>What's New in IntelliJ Azure eszköztára
## <a name="azure-toolkit-for-intellij-releases"></a>Az IntelliJ kiadásokban Azure eszköztára
Ez a cikk ismerteti a különböző kiadások és az Azure-eszközkészlet legújabb frissítéseinek az intellij-t.

> [!NOTE]
> Az Eclipse IDE egy Azure eszközkészletét is van. További információkért lásd: [Eclipse Azure eszköztára].
> 
> 

### <a name="april-14-2017"></a>2017. április 14.
Az Azure eszközkészlet az IntelliJ - 2017. április kiadásban a következő fejlesztéseket tartalmazza:

* **Továbbfejlesztett Azure bejelentkezési élmény**: az intellij-t az Azure-eszközkészlet mostantól támogatja az Azure-fiókjába való bejelentkezés két módszerrel: *interaktív* és *automatikus*. További információkért lásd: [Azure bejelentkezési az utasítások az intellij-t Azure eszköztára].
* **Közzététel a Docker-tárolók**: most közzéteheti a webalkalmazások Azure eszközkészlet segítségével az IntelliJ Docker tárolóként. További információkért lásd: [webalkalmazás közzététele az Azure-eszközkészlet segítségével az intellij-t egy Docker-tároló,].
* **Tárolási fiókkezelés**: az intellij-t az Azure-eszközkészlet mostantól támogatja a storage-fiókok kezelése az Azure Explorer nézetből. További információkért lásd: [kezelése Storage-fiókok az Azure-kezelővel használ az IntelliJ].
* **Virtuálisgép-kezelő**: az intellij-t az Azure-eszközkészlet mostantól támogatja a virtuális gépek kezelése az Azure Explorer eszköz ablakból. További információkért lásd: [kezelése használó virtuális gépek az Azure-kezelővel az IntelliJ].
* **Távoli hibaelhárítási támogatás eltávolítása**. Java-webalkalmazások Azure App Service távoli hibakeresés el lett távolítva az Azure-eszközkészlet az IntelliJ; Ez volt néhány problémát, amely az ügyfelek volt tapasztalják az eszközkészlet használata esetén feloldásához szükséges.

### <a name="august-26-2016"></a>2016 augusztusától 26
Az IntelliJ - kiadás 2016 augusztusától Azure eszköztára a következő fejlesztéseket tartalmazza:

* **Egyéni JDK Terjesztéseket**. Az IntelliJ Azure eszköztára mostantól támogatja a megadása, és az Azure-webalkalmazás tárolóhoz egy tetszőleges JDK-verzió telepítése:
  * Az Azure által biztosított JDKs, mellett is választhat Zulu OpenJDK verziók által elérhetővé tett Azure Azul rendszerek széles kijelölés.
  * A saját JDK terjesztési is megadható, ha a feltöltés csomagot .zip fájlként a tárfiókhoz.
* **Az Azure terület Intézőbeli nézetében fejlesztései**:
  * Új Azure Resource Manager-modell használatával a virtuális gép felügyelet támogatása: listában, létrehozhat, és törölje a resource manager-alapú virtuális gépek anélkül, hogy az IDE.
  * A Tárfiók a blob felügyelet kiegészíti a meglévő funkciók "klasszikus" storage-fiókok Azure Resource Manager használatával támogatása.
* **Az SQL Server 6.0 Microsoft JDBC-illesztőt**. A frissítés a Microsoft SQL Server (v6.0), amely magában foglalja a legfrissebb JDBC-illesztőt most része, amely egyszerűen hozzáadhatja a Java-projektekhez tárként, ezáltal a mag cseréje a régebbi verziót.

### <a name="june-29-2016"></a>2016. június 29.
Az Azure eszközkészlet az IntelliJ - 2016. június kiadásban a következő fejlesztéseket tartalmazza:

* **Java 8 követelmény**. Az IntelliJ Azure eszköztára megköveteli Java 8, bár ez a követelmény csak az eszközkészlet van – az alkalmazások továbbra is használhatja a Java Azure által támogatott összes verzióját.
* **A legújabb Java JDKs támogatása**. A Java JDKs a legfrissebb verziója most már támogatja az Azure-eszközkészlet az intellij-t.
* **Az Azure SDK v2.9.1 támogatása**. Az Azure SDK legújabb verziójára már az intellij-t Azure eszköztára minimális előzetesen szükséges.
* **Minták integrált**. Az IntelliJ Azure eszköztára most funkciókat több minta alkalmazás segítségével a fejlesztők a kezdéshez.
* **A HDInsight eszközzel integrációs**. Azure HDInsight eszközök most vannak becsomagolva Azure eszközkészlete az intellij-t. További információkért lásd: [IntelliJ HDInsight-eszközei beépülő].
* **Java-webalkalmazások távoli hibakeresés**. Az IntelliJ Azure eszköztára mostantól támogatja a Java-webalkalmazások Azure App Service távoli hibakeresés.

### <a name="april-12-2016"></a>2016. április 12.
Az Azure eszközkészlet az IntelliJ - 2016. április kiadásban a következő fejlesztéseket tartalmazza:

* **Az Azure SDK v2.9.0 támogatása**. Az Azure SDK legújabb verziójára már az intellij-t Azure eszköztára minimális előzetesen szükséges.
* **Vegyes használhatóság, válaszkészsége és teljesítménnyel kapcsolatos fejlesztések az Azure Web Apps támogatás kapcsolódó**. Hogyan kommunikál a az eszközkészlet Azure eredménye a teljesítményoptimalizálás számos olyan gyorsabban felhasználói felületén.
* **Az IntelliJ belül az Azure-ban meglévő webes alkalmazás tároló törlésére**. Az IntelliJ Azure eszköztára mostantól lehetővé teszi egy meglévő Azure webes tároló törlése IntelliJ maradjanak.

## <a name="see-also"></a>Lásd még:
A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:

* [Eclipse Azure eszköztára]
  * [Az Eclipse-hez készült Azure-eszközkészlet újdonságai]
  * [Az Eclipse-hez készült Azure-eszközkészlet telepítése]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
  * [Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * *What's New in IntelliJ (Ez a cikk) Azure eszköztára*
  * [Az IntelliJ-hez készült Azure-eszközkészlet telepítése]
  * [Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure bejelentkezési az utasítások az intellij-t Azure eszköztára]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[webalkalmazás közzététele az Azure-eszközkészlet segítségével az intellij-t egy Docker-tároló,]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[kezelése Storage-fiókok az Azure-kezelővel használ az IntelliJ]: ./azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer.md
[kezelése használó virtuális gépek az Azure-kezelővel az IntelliJ]: ./azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer.md

[Azure Java fejlesztői központ]: http://go.microsoft.com/fwlink/?LinkID=699547

[IntelliJ HDInsight-eszközei beépülő]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
