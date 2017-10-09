---
title: "a rugó rendszerindító alkalmazást egy Docker-tároló segítségével aaaPublish hello Azure eszköztára Eclipse |} Microsoft Docs"
description: "Megtudhatja, hogyan toopublish egy webes alkalmazás tooMicrosoft Azure-bA egy Docker-tároló segítségével hello Azure eszköztára eclipse-ben."
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
ms.openlocfilehash: 29390c49c339a1ebb87cb3951b21cea01c0da15f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a>A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, Eclipse hello Azure eszközkészlet használatával

Hello [rugó keretrendszer] egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat. Hello több népszerű projektekre beépített a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.

[Docker] egy nyílt forráskódú megoldás, amely a fejlesztőket automatizálásához hello telepítési méretezés és a tárolók a futó alkalmazások kezelését.

Ez az oktatóanyag bemutatja, hogyan hello lépéseket toodeploy egy rugó rendszerindító alkalmazásra, amely egy Docker-tároló tooMicrosoft Azure Eclipse hello Azure eszközkészlet használatával.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repository"></a>Hello alapértelmezett rugó rendszerindító Docker tárház klónozása

### <a name="import-hello-public-repository"></a>Importálás hello nyilvános tárház

hello következő lépések végigvezetik hello rugó rendszerindító Docker tárház tooyour helyi számítógép klónozása IntelliJ használatával. Ha azt szeretné, hogy a parancssor toouse, [rugó rendszerindító alkalmazást az Azure Tárolószolgáltatásban Linux központilag][Deploy Spring Boot on Linux in ACS].

1. Nyissa meg az eclipse-ben.

1. Kattintson a **fájl** > **importálási**.

   ![Fájl importálása menü][CL01]

1. Ha hello **importálási** párbeszédpanel:

   a. Bontsa ki a **Git**.

   b. Válassza ki **Projects from Git**.
   
   c. Kattintson a **Tovább** gombra.

   ![Importálása párbeszédpanel][CL02]

1. A hello **tárház forrásának kijelölése** lap:

   a. Válassza ki **URI klónozása**.
   
   b. Kattintson a **Tovább** gombra.

   ![Válassza ki a tárház forrás lap][CL03]

1. A hello **forrás Git-tárház** lap:

   a. A **URI**, adja meg `https://github.com/spring-guides/gs-spring-boot-docker.git`. Ez a lépés automatikusan kitölti hello **állomás** és **tárház elérési** mezőket a hello javítsa az értékeket.
   
   b. hello rugó rendszerindító tárház nyilvános, így nem kell tooenter a Git-felhasználónevét és jelszavát.
   
   c. Kattintson a **Tovább** gombra.

   ![Forrás Git-tárház lap][CL04]

1. A hello **ág kiválasztása** kattintson **következő**.

   ![Ág kiválasztása lap][CL05]

1. A hello **helyi cél** lap:

   a. Adja meg a hello helyi mappát, ahol a helyi tárházban.
   
   b. Kattintson a **Tovább** gombra.

   ![Helyi cél lap][CL06]

1. A hello **válassza ki a projektek importálásához varázsló toouse** oldalon:

   a. Válassza ki **általános projektként importálási**.
   
   b. Kattintson a **Tovább** gombra.

   !["A projektek importálásához varázsló toouse kiválasztása" című oldalt][CL07]

1. A hello **Import Projects** lap:

   a. Írja be a projekt nevét.
   
   b. Kattintson a **Befejezés** gombra.

   ![Lap projektek importálása][CL08]

1. Ha hello tárház sikeresen klónozták, megjelenik az eclipse-ben szereplő összes hello fájlokat.

   ![Helyi tárház][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a>A helyi tárházból Maven-projekt létrehozása

hello rugó rendszerindító Docker-tárház egy befejezett Maven project, mert ezzel fogja a jelen oktatóanyag tartalmazza. 

1. Kattintson a **fájl** > **importálási**.

   ![Az import parancs hello fájl menü][CL01]

1. Ha hello **importálási** párbeszédpanel:

   a. Bontsa ki a **Maven**.
   
   b. Válassza ki **meglévő Maven-projektek**.
   
   c. Kattintson a **Tovább** gombra.

   ![Importálása párbeszédpanel][MV01]

1. A hello **Maven-projektek** lap:

   a. A **gyökérkönyvtár**, adja meg a hello **teljes** mappa a helyi tárházba.
   
   b. Bontsa ki a hello **speciális** szakaszt, és adjon meg egy egyéni nevet **sablon**.
   
   c. Jelölje be hello be a hello **pom.xml** fájl hello projektben.
   
   d. Kattintson a **Befejezés** gombra.

   ![Maven-projektek lap][MV02]

1. Amikor a hello Maven project sikeresen meg van nyitva, a második projekt eclipse-ben szereplő látható.

   ![Helyi Maven project][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a>A rugó rendszerindító alkalmazás elkészítésére Maven használatával

1. Az Eclipse Project Explorer hello válassza ki a hello Maven project.

1. Kattintson a **futtatása** > **futtató** > **Maven build**.

   ![Parancsok toorun Maven build szerint][BU01]

1. Ha az alkalmazás sikeresen épül, a konzolablakban hello hello állapotát jeleníti meg.

   ![Sikeres Maven build][BU02]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>A webes alkalmazás tooAzure közzététele egy Docker-tároló használatával

1. Az Eclipse Project Explorer hello válassza ki a hello Maven project.

1. Kattintson a hello Azure **közzététel** menüben, majd kattintson **Docker-tároló közzététel**.

   ![Docker-tároló parancsként közzététele][PU01]

1. Ha hello **Docker-tároló üzembe helyezése az Azure-on** párbeszédpanel jelenik meg:

   a. Adja meg a Docker egyéni rendszerkép neve.
   
   b. A **összetevő toodeploy**, adja meg a hello elérési toohello **gs-rugó-rendszerindítás – docker-0.1.0.jar** csak beépített fájlt.

   ![Adja meg a Docker-beállítások][PU02]

   Bármely létező Docker-gazdagépekből jelennek meg. 

1. Ha úgy dönt, hogy toodeploy tooan meglévő állomás, kihagyhatja toostep 5. Ellenkező esetben használja a következő lépéseket toocreate állomás hello:

   a. Kattintson az **Add** (Hozzáadás) parancsra.

      ![Új Docker-gazdagép hozzáadása][PU03]

   b. Ha hello **Docker állomás létrehozása** párbeszédpanel jelenik meg, dönthet, hogy tooaccept hello alapértelmezett értékeket, vagy megadhatja az egyéni beállításait az új Docker-állomáshoz. (A részletes leírását hello különböző beállításairól [a webes alkalmazás is egy Docker-tároló közzé a IntelliJ hello Azure eszközkészlet használatával][Publish Container with Azure Toolkit].) Kattintson a **következő** esetén mely beállítások toouse megadott.

      ![Adja meg a Docker állomás beállításai][PU04]

   c. Az az Azure key vault közül választhat a meglévő toouse bejelentkezési hitelesítő adatok, vagy dönthet úgy tooenter új Docker bejelentkezési hitelesítő adatokat. Kattintson a **Befejezés** Ha megadta a beállításokat.

      ![Docker állomás hitelesítő adatok megadása][PU05]

1. Válassza ki a Docker-állomás, és kattintson a **következő**.

   ![Válassza ki a Docker állomás toouse][PU06]

1. Hello utolsó oldalán hello **Docker-tároló üzembe helyezése az Azure-on** párbeszédpanelen adja meg az alábbi beállítások hello:

   a. Dönthet toospecify hello tárolót a Docker-tároló futtató egyéni nevet, vagy elfogadhatja az alapértelmezett hello.

   b. Adja meg a TCP-portok hello a docker állomás hello a következő szintaxis használatával: *[külső port]*:*[belső port]*. Például **80:8080** egy külső port a 80-as és hello alapértelmezett belső rugó rendszerindító port a 8080-as határozza meg.
   
      Ha a belső port testreszabott (például a hello application.yml fájl szerkesztésével), szüksége toospecify hello portszám hello megfelelő útválasztási toooccur az Azure-ban.

   c. Ezek a beállítások megadása után kattintson az **Befejezés**.

   ![Egy Docker-tároló az Azure telepítéséhez][PU07]

1. Amikor hello Azure eszközkészlet közzététele befejeződött, hello Azure tevékenységnapló megjeleníti **közzétett** hello állapot.

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
