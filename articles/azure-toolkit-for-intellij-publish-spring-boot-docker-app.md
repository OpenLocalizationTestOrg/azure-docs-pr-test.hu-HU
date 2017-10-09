---
title: "a rugó rendszerindító alkalmazást egy Docker-tároló segítségével aaaPublish hello Azure eszközkészlet IntelliJ |} Microsoft Docs"
description: "Megtudhatja, hogyan toopublish egy webes alkalmazás tooMicrosoft Azure-bA egy Docker-tároló segítségével hello Azure eszközkészlet intellij-t."
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
ms.openlocfilehash: 8964cb33fd8f61a39f091633ae9074d9658232fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a>A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, IntelliJ hello Azure eszközkészlet használatával

Hello [rugó keretrendszer] egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat. Hello több népszerű projektekre beépített a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.

[Docker] egy nyílt forráskódú megoldás, amely a fejlesztőket automatizálásához hello telepítési méretezés és a tárolók a futó alkalmazások kezelését.

Ez az oktatóanyag bemutatja, hogyan hello lépéseket toodeploy egy rugó rendszerindító alkalmazásra, amely egy Docker-tároló tooMicrosoft Azure IntelliJ hello Azure eszközkészlet használatával.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repo"></a>Hello alapértelmezett rugó rendszerindító Docker tárház klónozása

hello következő lépések végigvezetik hello rugó rendszerindító Docker tárház klónozása IntelliJ használatával. Ha azt szeretné, hogy a parancssor toouse, [rugó rendszerindító alkalmazást az Azure Tárolószolgáltatásban Linux központilag][Deploy Spring Boot on Linux in ACS].

1. Nyissa meg az intellij-t.

1. Hello üdvözlőképernyőn jelölje ki a hello **GitHub** hello beállítása **tekintse meg a verziókövetés** listája.

   ![GitHub Verziókövetés beállítása][CL01]

1. Adja meg a hitelesítő adatokat, ha a kért toolog.

   * Ha a felhasználónév/jelszó toolog tooGitHub használ:

      ![A GitHub-felhasználónevét és a jelszó megadása párbeszédpanel][CL02a]

   * Ha egy token toolog tooGitHub használ:

      ![A GitHub-token megadása párbeszédpanel][CL02b]

1. Adja meg **https://github.com/spring-guides/gs-spring-boot-docker.git** hello tárház URL-címhez, adja meg a helyi elérési út és a mappa adatait, és kattintson **Klónozás**.

   ![Klónozott tárház párbeszédpanel][CL03]

1. Amikor felszólítja az IntelliJ projektre, válassza toocreate **nem**.

   ![Az IntelliJ-projektek toocreate elutasítása][CL04]

1. A hello kezdőlapján kattintson **projekt importálása**.

   ![Importálja a projekt kiválasztása][CL05]

1. Keresse meg a hello elérési utat, ahol hello rugó rendszerindító tárház klónozott, válassza ki a hello **teljes** mappa hello gyökérszintű, majd **OK**.

   ![Válasszon egy mappát az importálás][CL06]

1. Amikor a rendszer kéri, válassza ki a **Create project meglévő forrásokból származó**.

   ![A beállítás toocreate meglévő forrásokból projekt][CL07]

1. Írja be a projekt nevét vagy hello alapértelmezés elfogadásához, ellenőrizze a helyes elérési utat toohello hello **teljes** mappa, és kattintson **következő**.

   ![Adja meg a hello projekt neve][CL08]

1. Az importálás könyvtárak testreszabása, és kattintson a **következő**.

   ![Válassza ki a könyvtárak][CL09]

1. Tekintse át a hello szalagtárak tooimport, majd **következő**.

   ![Tekintse át a projekt függvénytárak][CL10]

1. Tekintse át a hello modul szerkezete, majd **következő**.

   ![Tekintse át a modul szerkezete][CL11]

1. Adja meg a JDK, és kattintson **következő**.

   ![Adja meg a JDK][CL12]

1. Kattintson a **Befejezés** gombra.

   ![A Befejezés gombra.][CL13]

Az IntelliJ hello rugó rendszerindító alkalmazás projektként importálja, és hello struktúra megjelenít, amikor hello befejeződött.

![Az IntelliJ rugó rendszerindító alkalmazás][CL14]

## <a name="build-your-spring-boot-app"></a>A rugó rendszerindító build alkalmazás

### <a name="build-hello-app-by-using-hello-maven-pom"></a>Hozza létre a hello alkalmazását hello Maven POM használatával

1. Hello Maven eszköz ablak nyílik meg, ha már nincs megnyitva. Kattintson a **nézet** > **Windows eszköz** > **Maven-projektek**.

   ![Eszköz Windows és a Maven-projektek parancsok][BU01]

1. Hello Maven-eszköz ablakában kattintson a jobb gombbal **csomag** válassza **Maven Build futtatása**. (Ha a Maven project nem jelenik meg automatikusan, kattintson a hello **újbóli importálása** hello Maven eszköztár ikonjára.)

   ![Maven Build parancs futtatása][BU02]

1. Az IntelliJ megjelenjen-e egy **létrehozása sikeres** jelenik meg, ha a rugó rendszerindító alkalmazás sikeresen létrehozva.

   ![BUILD üzenetet][BU03]

### <a name="create-a-deployment-ready-artifact"></a>A telepítés kész eltérés létrehozása

toopublish a rugó rendszerindító alkalmazást, a telepítés kész összetevő toocreate van szüksége. A lépéseket követve hello használata:

1. Nyissa meg a webalkalmazás-projektet az intellij-t.

1. Kattintson a **fájl**, és kattintson a **szerkezetének**.

   ![Projekt struktúra parancs][ART01]

1. Kattintson a zöld plusz hello (**+**) tooadd összetevő szimbólumának, kattintson a **JAR**, és kattintson a **üres**.

   ![Összetevő felvétele][ART02]

1. Az összetevő neve eközben nem tooadd kiterjesztés ".jar" hello, és adja meg az Maven kimeneti hello hello célmappa.

   ![Adja meg az összetevő tulajdonságai][ART03]

1. Az összetevő a jegyzékfájl létrehozása (nem kötelező):

   a. Kattintson a **jegyzékfájl létrehozása**.

      ![Hello jegyzékfájl létrehozása gombra][ART04a]

   b. Válassza ki a hello alapértelmezett elérési hello összetevő, és kattintson a **OK**.

      ![Adja meg az összetevő elérési útja][ART04b]

   c. Hello három pont gombra (**...** ) toolocate hello fő osztályban.

      ![Keresse meg a fő osztály][ART04c]

   d. Válassza ki a fő osztályban, és kattintson a **OK**.

      ![Adja meg a fő osztály][ART04d]

1. Kattintson az **OK** gombra.

   ![Zárja be a hello szerkezetének párbeszédpanel][ART05]

> [!NOTE]
> Az IntelliJ összetevők létrehozásával kapcsolatos további információkért lásd: [konfigurálása összetevők] hello JetBrains webhelyen.
>

### <a name="build-hello-artifact-for-deployment"></a>Központi telepítés hello összetevő létrehozása

1. Kattintson a **Build**, és kattintson a **összetevők**.

   ![Az összetevők parancs létrehozása][BU04]

1. Ha hello **Build összetevő** helyi menü megjelenik, kattintson a **Build**.

   ![Build összetevő a helyi menü][BU05]

A rugó rendszerindító alkalmazás befejeződött hello összetevő IntelliJ megjelenjen-e hello projekt eszköz ablakban.

   ![Létrehozott összetevő][BU06]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>A webes alkalmazás tooAzure közzététele egy Docker-tároló használatával

1. Ha nincs bejelentkezve tooyour Azure-fiókra, kövesse hello [bejelentkezési utasításokat hello Azure eszközkészlet az IntelliJ][Azure Sign In for IntelliJ].

1. Hello Project Explorer eszköz ablakában kattintson a jobb gombbal a hello projektet, és válassza **Azure** > **Docker-tároló közzététel**.

   ![Docker-tároló parancsként közzététele][PU01]

1. Ha hello **Docker-tároló központi telepítése az Azure-on** párbeszédpanel jelenik meg, a meglévő Docker-gazdagépekből jelennek meg. Ha úgy dönt, hogy toodeploy tooan meglévő állomás, kihagyhatja toostep 4. Ellenkező esetben használja a következő lépéseket toocreate állomás hello:

   a. Kattintson a zöld plusz hello (**+**) szimbólum.

      ![Új Docker-gazdagép hozzáadása][PU02]

   b. Ha hello **Docker állomás létrehozása** párbeszédpanel jelenik meg, dönthet, hogy tooaccept hello alapértelmezett értékeket, vagy megadhatja az egyéni beállításait az új Docker-állomáshoz. (A részletes leírását hello különböző beállításairól [a webes alkalmazás is egy Docker-tároló közzé a IntelliJ hello Azure eszközkészlet használatával][Publish Container with Azure Toolkit].) Kattintson a **következő** esetén mely beállítások toouse megadott.

      ![Adja meg a Docker állomás beállításai][PU03a]

   c. Az az Azure key vault közül választhat a meglévő toouse bejelentkezési hitelesítő adatok, vagy dönthet úgy tooenter új Docker bejelentkezési hitelesítő adatokat. Kattintson a **Befejezés** Ha megadta a beállításokat.

      ![Docker állomás hitelesítő adatok megadása][PU03b]

1. Válassza ki a Docker-állomás, és kattintson a **következő**.

   ![Válassza ki a hello Docker állomás toouse][PU04]

1. Hello utolsó oldalán hello **Docker-tároló központi telepítése az Azure-on** párbeszédpanelen adja meg az alábbi beállítások hello:

   a. Dönthet toospecify hello tárolót a Docker-tároló futtató egyéni nevet, vagy elfogadhatja az alapértelmezett hello.

   b. Adja meg a TCP-portok hello a docker állomás hello a következő szintaxis használatával: *[külső port]*:*[belső port]*. Például **80:8080** egy külső port a 80-as és hello alapértelmezett belső rugó rendszerindító port a 8080-as határozza meg.
   
      Ha a belső port testreszabott (például a hello application.yml fájl szerkesztésével), szüksége toospecify hello portszám hello megfelelő útválasztási toooccur az Azure-ban.

   c. Ezek a beállítások konfigurálása után kattintson **Befejezés**.

   ![Egy Docker-tároló az Azure telepítéséhez][PU05]

1. Amikor hello Azure eszközkészlet közzététele befejeződött, hello Azure tevékenységnapló megjeleníti **közzétett** hello állapot.

   ![Docker-gazdagép telepítése sikeres volt][PU06]

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

toolearn további módszereket biztosít a rugó rendszerindító alkalmazások IntelliJ, használatával kapcsolatban lásd: [rugó rendszerindító projektek létrehozása](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) hello JetBrains webhelyen.

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[konfigurálása összetevők]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó keretrendszer]: https://spring.io/

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL01.png
[CL02a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02a.png
[CL02b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02b.png
[CL03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL09.png
[CL10]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL10.png
[CL11]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL11.png
[CL12]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL12.png
[CL13]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL13.png
[CL14]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL14.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART03.png
[ART04a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04a.png
[ART04b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04b.png
[ART04c]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04c.png
[ART04d]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04d.png
[ART05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART05.png

[BU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU02.png
[BU03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU03.png
[BU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU04.png
[BU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU05.png
[BU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU06.png

[PU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU02.png
[PU03a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03a.png
[PU03b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03b.png
[PU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU06.png
