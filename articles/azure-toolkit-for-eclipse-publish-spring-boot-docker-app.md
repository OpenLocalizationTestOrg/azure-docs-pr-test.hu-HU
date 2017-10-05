---
title: "A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, az eclipse-ben az Azure-eszközkészlet használatával |} Microsoft Docs"
description: "Útmutató a webes alkalmazás a Microsoft Azure is közzé egy Docker-tároló az eclipse-ben az Azure-eszközkészlet használatával."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/21/2017
ms.author: robmcm
ms.openlocfilehash: fcb60fcfbda26f5f37bfb0edcb01f8737188b6bc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a>A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, az eclipse-ben az Azure-eszközkészlet használatával

A [rugó keretrendszer] egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat. A beépített több népszerű projektek a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.

[Docker] egy nyílt forráskódú megoldás, amely a fejlesztőket automatizálja a központi telepítési méretezés és a tárolók a futó alkalmazások kezelését.

Ez az oktatóanyag végigvezeti a központi telepítése egy rugó rendszerindító alkalmazásra, amely egy Docker-tároló Microsoft Azure az eclipse-ben az Azure-eszközkészlet használatával.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repository"></a>Az alapértelmezett rendszerindító Docker rugó tárház klónozása

### <a name="import-the-public-repository"></a>A nyilvános tárház importálása

A következő lépések végigvezetik a helyi számítógépen a rugó rendszerindító Docker-tárház klónozása IntelliJ használatával. Ha azt szeretné, a parancssor használatára, lásd: [rugó rendszerindító alkalmazást az Azure Tárolószolgáltatásban Linux központilag][Deploy Spring Boot on Linux in ACS].

1. Nyissa meg az eclipse-ben.

1. Kattintson a **fájl** > **importálási**.

   ![Fájl importálása menü][CL01]

1. Ha a **importálási** párbeszédpanel:

   a. Bontsa ki a **Git**.

   b. Válassza ki **Projects from Git**.
   
   c. Kattintson a **Tovább** gombra.

   ![Importálása párbeszédpanel][CL02]

1. Az a **tárház forrásának kijelölése** lap:

   a. Válassza ki **URI klónozása**.
   
   b. Kattintson a **Tovább** gombra.

   ![Válassza ki a tárház forrás lap][CL03]

1. Az a **forrás Git-tárház** lap:

   a. A **URI**, adja meg `https://github.com/spring-guides/gs-spring-boot-docker.git`. Ez a lépés automatikusan kitölti a **állomás** és **tárház elérési** mezők a megfelelő értékekkel.
   
   b. A rugó rendszerindító tárház nyilvános, így nem kell megadnia a Git-felhasználónevét és jelszavát.
   
   c. Kattintson a **Tovább** gombra.

   ![Forrás Git-tárház lap][CL04]

1. Az a **ág kiválasztása** kattintson **következő**.

   ![Ág kiválasztása lap][CL05]

1. Az a **helyi cél** lap:

   a. Adja meg a helyi mappát, ahol a helyi tárházban.
   
   b. Kattintson a **Tovább** gombra.

   ![Helyi cél lap][CL06]

1. Az a **válassza ki a használandó projektek importálása varázsló** lap:

   a. Válassza ki **általános projektként importálási**.
   
   b. Kattintson a **Tovább** gombra.

   !["A használandó projektek importálása varázsló kiválasztása" című oldalt][CL07]

1. Az a **Import Projects** lap:

   a. Írja be a projekt nevét.
   
   b. Kattintson a **Befejezés** gombra.

   ![Lap projektek importálása][CL08]

1. Ha a tárház sikeresen klónozták, megjelenik az eclipse-ben szereplő összes fájlt.

   ![Helyi tárház][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a>A helyi tárházból Maven-projekt létrehozása

A rugó rendszerindító Docker-tárház egy befejezett Maven project, mert ezzel fogja a jelen oktatóanyag tartalmazza. 

1. Kattintson a **fájl** > **importálási**.

   ![Importálási parancs, a Fájl menü][CL01]

1. Ha a **importálási** párbeszédpanel:

   a. Bontsa ki a **Maven**.
   
   b. Válassza ki **meglévő Maven-projektek**.
   
   c. Kattintson a **Tovább** gombra.

   ![Importálása párbeszédpanel][MV01]

1. Az a **Maven-projektek** lap:

   a. A **gyökérkönyvtár**, adja meg a **teljes** mappa a helyi tárházba.
   
   b. Bontsa ki a **speciális** szakaszt, és adjon meg egy egyéni nevet **sablon**.
   
   c. Jelölje be a **pom.xml** fájlt a projektben.
   
   d. Kattintson a **Befejezés** gombra.

   ![Maven-projektek lap][MV02]

1. Amikor a Maven project sikeresen meg van nyitva, a második projekt eclipse-ben szereplő látható.

   ![Helyi Maven project][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a>A rugó rendszerindító alkalmazás elkészítésére Maven használatával

1. Válassza ki a Maven projekt Eclipse Project Explorer.

1. Kattintson a **futtatása** > **futtató** > **Maven build**.

   ![Parancsokat futtató Maven build][BU01]

1. Ha az alkalmazás sikeresen épül, a konzolablakban állapotát jeleníti meg.

   ![Sikeres Maven build][BU02]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>A webalkalmazás közzététele az Azure-bA egy Docker-tároló használatával

1. Válassza ki a Maven projekt Eclipse Project Explorer.

1. Kattintson az Azure **közzététel** menüben, majd kattintson **Docker-tároló közzététel**.

   ![Docker-tároló parancsként közzététele][PU01]

1. Ha a **Docker-tároló üzembe helyezése az Azure-on** párbeszédpanel jelenik meg:

   a. Adja meg a Docker egyéni rendszerkép neve.
   
   b. A **összetevő telepítéséhez**, adja meg az elérési útját a **gs-rugó-rendszerindítás – docker-0.1.0.jar** csak beépített fájlt.

   ![Adja meg a Docker-beállítások][PU02]

   Bármely létező Docker-gazdagépekből jelennek meg. 

1. Ha egy meglévő gazdagépre telepíti, kihagyhatja az 5. Ellenkező esetben a következő lépések segítségével gazdagép létrehozása:

   a. Kattintson az **Add** (Hozzáadás) parancsra.

      ![Új Docker-gazdagép hozzáadása][PU03]

   b. Ha a **Docker állomás létrehozása** párbeszédpanel jelenik meg, dönthet úgy, hogy fogadja el az alapértelmezett vagy az egyéni beállításait az új Docker-állomáshoz is megadhat. (Különböző beállításának részletes leírását lásd: [egy Docker-tároló egy webalkalmazást az intellij-t az Azure-eszközkészlet használatával közzététel][Publish Container with Azure Toolkit].) Kattintson a **következő** amikor megadott mely beállításokat kívánják használni.

      ![Adja meg a Docker állomás beállításai][PU04]

   c. Választhat egy az Azure key vault a meglévő bejelentkezési hitelesítő adatok használatát, vagy dönthet úgy, új Docker bejelentkezési hitelesítő adatok megadásához. Kattintson a **Befejezés** Ha megadta a beállításokat.

      ![Docker állomás hitelesítő adatok megadása][PU05]

1. Válassza ki a Docker-állomás, és kattintson a **következő**.

   ![Válassza ki a Docker állomás használata][PU06]

1. Az utolsó oldalán a **Docker-tároló üzembe helyezése az Azure-on** párbeszédpanelen adja meg a következő beállításokat:

   a. Adjon meg egy egyéni nevet, a tároló, amely üzemelteti a Docker-tároló választhat, vagy elfogadhatja az alapértelmezett.

   b. Adja meg a TCP-portok a docker-állomás a következő szintaxissal: *[külső port]*:*[belső port]*. Például **80:8080** megadja egy külső port a 80-as és az alapértelmezett belső rugó rendszerindító port a 8080-as.
   
      Ha testreszabta a belső port (például a a application.yml fájl szerkesztésével), meg kell adnia a portszámot a megfelelő irányításához megtörténik az Azure-ban.

   c. Ezek a beállítások megadása után kattintson az **Befejezés**.

   ![Egy Docker-tároló az Azure telepítéséhez][PU07]

1. Ha az Azure-eszközkészlet közzététele befejeződött, az Azure tevékenységnapló megjeleníti **közzétett** az állapot.

   ![Docker-gazdagép telepítése sikeres volt][PU08]

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó keretrendszer]: https://spring.io/

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL01.png
[CL02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL02.png
[CL03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL09.png

[MV01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV01.png
[MV02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV02.png
[MV03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV03.png

[BU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU02.png

[PU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU02.png
[PU03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU03.png
[PU04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU06.png
[PU07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU07.png
[PU08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU08.png
