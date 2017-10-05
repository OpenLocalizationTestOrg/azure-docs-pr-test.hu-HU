---
title: "A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, az intellij-t az Azure-eszközkészlet használatával |} Microsoft Docs"
description: "Útmutató a webes alkalmazás a Microsoft Azure is közzé egy Docker-tároló az intellij-t az Azure-eszközkészlet használatával."
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
ms.openlocfilehash: b771238934183c953615ac33c42a275d80657556
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a>A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, az intellij-t az Azure-eszközkészlet használatával

A [rugó keretrendszer] egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat. A beépített több népszerű projektek a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.

[Docker] egy nyílt forráskódú megoldás, amely a fejlesztőket automatizálja a központi telepítési méretezés és a tárolók a futó alkalmazások kezelését.

Ez az oktatóanyag végigvezeti a központi telepítése egy rugó rendszerindító alkalmazásra, amely egy Docker-tároló Microsoft Azure az intellij-t az Azure-eszközkészlet használatával.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repo"></a>A alapértelmezett rugó rendszerindító Docker tárház klónozása

A következő lépések végigvezetik a rugó rendszerindító Docker-tárház klónozása IntelliJ használatával. Ha azt szeretné, a parancssor használatára, lásd: [rugó rendszerindító alkalmazást az Azure Tárolószolgáltatásban Linux központilag][Deploy Spring Boot on Linux in ACS].

1. Nyissa meg az intellij-t.

1. Az üdvözlőképernyőn jelölje ki a **GitHub** beállítást a **tekintse meg a verziókövetés** listája.

   ![GitHub Verziókövetés beállítása][CL01]

1. Adja meg a hitelesítő adatok, ha a rendszer kéri-e jelentkezni.

   * Ha a felhasználónév/jelszó segítségével bejelentkezni a Githubon:

      ![A GitHub-felhasználónevét és a jelszó megadása párbeszédpanel][CL02a]

   * Ha egy jogkivonatot használ próbál bejelentkezni a Githubon:

      ![A GitHub-token megadása párbeszédpanel][CL02b]

1. Adja meg **https://github.com/spring-guides/gs-spring-boot-docker.git** a tárház URL-címet, adja meg a helyi elérési út és a mappa adatait, és kattintson **Klónozás**.

   ![Klónozott tárház párbeszédpanel][CL03]

1. Amikor a rendszer kéri az IntelliJ-projekt létrehozása, válassza ki a **nem**.

   ![Az IntelliJ-projekt létrehozása elutasítása][CL04]

1. Az üdvözlőoldalon kattintson **projekt importálása**.

   ![Importálja a projekt kiválasztása][CL05]

1. Keresse meg az elérési utat, ahol a rugó rendszerindító tárház klónozott, válassza ki a **teljes** mappa a gyökérszintű, majd **OK**.

   ![Válasszon egy mappát az importálás][CL06]

1. Amikor a rendszer kéri, válassza ki a **Create project meglévő forrásokból származó**.

   ![Lehetőség meglévő forrásokból projekt létrehozása][CL07]

1. Írja be a projekt nevét, vagy fogadja el az alapértelmezett, ellenőrizze a helyes elérési útját a **teljes** mappa, és kattintson **következő**.

   ![Adja meg a projekt neve][CL08]

1. Az importálás könyvtárak testreszabása, és kattintson a **következő**.

   ![Válassza ki a könyvtárak][CL09]

1. Tekintse át az importáláshoz, majd kattintson a könyvtárak **következő**.

   ![Tekintse át a projekt függvénytárak][CL10]

1. Tekintse át a modul szerkezete, majd a **következő**.

   ![Tekintse át a modul szerkezete][CL11]

1. Adja meg a JDK, és kattintson **következő**.

   ![Adja meg a JDK][CL12]

1. Kattintson a **Befejezés** gombra.

   ![A Befejezés gombra.][CL13]

IntelliJ importálja a rugó rendszerindító alkalmazás projektként, és a struktúra jeleníti meg, ha az importálás befejeződik.

![Az IntelliJ rugó rendszerindító alkalmazás][CL14]

## <a name="build-your-spring-boot-app"></a>A rugó rendszerindító build alkalmazás

### <a name="build-the-app-by-using-the-maven-pom"></a>Az alkalmazás összeállítása a Maven POM használatával

1. Nyissa meg a Maven eszköz ablakot, ha már nincs megnyitva. Kattintson a **nézet** > **Windows eszköz** > **Maven-projektek**.

   ![Eszköz Windows és a Maven-projektek parancsok][BU01]

1. A Maven eszköz ablakban kattintson a jobb gombbal **csomag** válassza **Maven Build futtatása**. (Ha a Maven project nem jelenik meg automatikusan, kattintson a **újbóli importálása** a Maven eszköztár ikonjára.)

   ![Maven Build parancs futtatása][BU02]

1. Az IntelliJ megjelenjen-e egy **létrehozása sikeres** jelenik meg, ha a rugó rendszerindító alkalmazás sikeresen létrehozva.

   ![BUILD üzenetet][BU03]

### <a name="create-a-deployment-ready-artifact"></a>A telepítés kész eltérés létrehozása

A rugó rendszerindító alkalmazás közzétételéhez létrehozásához szükséges a telepítés kész összetevő. Ehhez a következő lépések szükségesek:

1. Nyissa meg a webalkalmazás-projektet az intellij-t.

1. Kattintson a **fájl**, és kattintson a **szerkezetének**.

   ![Projekt struktúra parancs][ART01]

1. Kattintson a zöld plusz (**+**) az összetevő hozzáadásához kattintson a szimbólum **JAR**, és kattintson a **üres**.

   ![Összetevő felvétele][ART02]

1. Annak biztosítása, hogy nem adja hozzá a ".jar" kiterjesztést közben az összetevő neve, és adja meg a célmappában Maven kimenetének.

   ![Adja meg az összetevő tulajdonságai][ART03]

1. Az összetevő a jegyzékfájl létrehozása (nem kötelező):

   a. Kattintson a **jegyzékfájl létrehozása**.

      ![A jegyzékfájl létrehozása gombra][ART04a]

   b. Válassza ki az alapértelmezett elérési összetevő, és kattintson a **OK**.

      ![Adja meg az összetevő elérési útja][ART04b]

   c. Kattintson a három pont (**...** ) a fő osztályban található.

      ![Keresse meg a fő osztály][ART04c]

   d. Válassza ki a fő osztályban, és kattintson a **OK**.

      ![Adja meg a fő osztály][ART04d]

1. Kattintson az **OK** gombra.

   ![A projekt struktúra párbeszédpanel bezárásához.][ART05]

> [!NOTE]
> Az IntelliJ összetevők létrehozásával kapcsolatos további információkért lásd: [konfigurálása összetevők] a JetBrains webhelyen.
>

### <a name="build-the-artifact-for-deployment"></a>A központi telepítés összetevő létrehozása

1. Kattintson a **Build**, és kattintson a **összetevők**.

   ![Az összetevők parancs létrehozása][BU04]

1. Ha a **Build összetevő** helyi menü megjelenik, kattintson a **Build**.

   ![Build összetevő a helyi menü][BU05]

A rugó rendszerindító alkalmazás befejezett összetevő IntelliJ megjelenjen-e a projekt eszköz ablakban.

   ![Létrehozott összetevő][BU06]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>A webalkalmazás közzététele az Azure-bA egy Docker-tároló használatával

1. Ha nem bejelentkezett az Azure-fiókjába, kövesse a lépéseket [bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ][Azure Sign In for IntelliJ].

1. A Project Explorer eszköz ablakban kattintson a jobb gombbal a projektre, majd válassza ki **Azure** > **Docker-tároló közzététel**.

   ![Docker-tároló parancsként közzététele][PU01]

1. Ha a **Docker-tároló központi telepítése az Azure-on** párbeszédpanel jelenik meg, a meglévő Docker-gazdagépekből jelennek meg. Ha egy meglévő gazdagépre telepíti, a 4. lépés kihagyhatja. Ellenkező esetben a következő lépések segítségével gazdagép létrehozása:

   a. Kattintson a zöld plusz (**+**) szimbólum.

      ![Új Docker-gazdagép hozzáadása][PU02]

   b. Ha a **Docker állomás létrehozása** párbeszédpanel jelenik meg, dönthet úgy, hogy fogadja el az alapértelmezett vagy az egyéni beállításait az új Docker-állomáshoz is megadhat. (Különböző beállításának részletes leírását lásd: [egy Docker-tároló egy webalkalmazást az intellij-t az Azure-eszközkészlet használatával közzététel][Publish Container with Azure Toolkit].) Kattintson a **következő** amikor megadott mely beállításokat kívánják használni.

      ![Adja meg a Docker állomás beállításai][PU03a]

   c. Választhat egy az Azure key vault a meglévő bejelentkezési hitelesítő adatok használatát, vagy dönthet úgy, új Docker bejelentkezési hitelesítő adatok megadásához. Kattintson a **Befejezés** Ha megadta a beállításokat.

      ![Docker állomás hitelesítő adatok megadása][PU03b]

1. Válassza ki a Docker-állomás, és kattintson a **következő**.

   ![Válassza ki a Docker állomás használata][PU04]

1. Az utolsó oldalán a **Docker-tároló központi telepítése az Azure-on** párbeszédpanelen adja meg a következő beállításokat:

   a. Adjon meg egy egyéni nevet, a tároló, amely üzemelteti a Docker-tároló választhat, vagy elfogadhatja az alapértelmezett.

   b. Adja meg a TCP-portok a docker-állomás a következő szintaxissal: *[külső port]*:*[belső port]*. Például **80:8080** megadja egy külső port a 80-as és az alapértelmezett belső rugó rendszerindító port a 8080-as.
   
      Ha testreszabta a belső port (például a a application.yml fájl szerkesztésével), meg kell adnia a portszámot a megfelelő irányításához megtörténik az Azure-ban.

   c. Ezek a beállítások konfigurálása után kattintson **Befejezés**.

   ![Egy Docker-tároló az Azure telepítéséhez][PU05]

1. Ha az Azure-eszközkészlet közzététele befejeződött, az Azure tevékenységnapló megjeleníti **közzétett** az állapot.

   ![Docker-gazdagép telepítése sikeres volt][PU06]

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

A rugó rendszerindító alkalmazások létrehozása az IntelliJ segítségével további módszerekkel kapcsolatos további tudnivalókért lásd: [rugó rendszerindító projektek létrehozása](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) a JetBrains webhelyen.

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
