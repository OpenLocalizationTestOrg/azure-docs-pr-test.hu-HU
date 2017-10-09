---
title: "a Service Fabric Eclipse beépülő modul aaaAzure |} Microsoft Docs"
description: "Ismerkedés a Service Fabric beépülő modul hello az eclipse-ben."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/21/2016
ms.author: saysa
ms.openlocfilehash: 4ba5a28a6282387249a2bd4e62314e891ff04162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a>Az Eclipse Service Fabric beépülő moduljának Java alkalmazásfejlesztése
Az eclipse-ben, hello legszélesebb körben használt egyik integrált fejlesztési környezetekben (IDEs) Java-fejlesztők számára. Ez a cikk azt ismerteti hogyan tooset az Eclipse fejlesztői környezetet toowork Azure Service Fabric fel. Ismerje meg, hogyan tooinstall hello Service Fabric beépülő modul, a Service Fabric-alkalmazás létrehozása, és a Service Fabric application tooa helyi vagy távoli Service Fabric-fürt üzembe az Eclipse Neonfény.

## <a name="install-or-update-hello-service-fabric-plug-in-in-eclipse-neon"></a>Telepítse vagy frissítse a hello Service Fabric Eclipse Neonfény beépülő modulja
Telepíthet egy Service Fabric beépülő modult az Eclipse-en. beépülő modul hello megkönnyítheti hello folyamat létrehozása és a Java-szolgáltatások telepítése.

1.  Gondoskodjon arról, hogy Eclipse Neonfény hello legújabb verzióját és hello Buildship legújabb verziójának telepítése (1.0.17 vagy újabb verzió):
    -   toocheck hello verziók telepített összetevőket, az Eclipse Neonfény, nyissa meg túl**súgó** > **telepítésének részletei**.
    -   tooupdate Buildship, lásd: [Eclipse Buildship: Eclipse beépülő modulokat Gradle][buildship-update].
    -   a toocheck és telepítse a frissítéseket az Eclipse Neonfény, nyissa meg túl**súgó** > **frissítések keresése**.

2.  túl nyissa meg a Service Fabric beépülő modulja, Eclipse Neonfény tooinstall hello**súgó** > **új szoftverek telepítése**.
  1.    A hello **együttműködve** adja meg a **http://dl.microsoft.com/eclipse**.
  2.    Kattintson az **Add** (Hozzáadás) parancsra.

         ![Az Eclipse Neon Service Fabric beépülő modulja][sf-eclipse-plugin-install]
  3.    Válassza ki a hello Service Fabric beépülő modult, és kattintson a **következő**.
  4.    Hello telepítési lépéseket, és fogadja el a hello Microsoft szoftverlicenc-szerződést.

Ha már hello Service Fabric beépülő modul telepítve van, győződjön meg arról, hogy rendelkezik-e hello legújabb verziójára. túl nyissa meg az elérhető frissítések toocheck**súgó** > **telepítésének részletei**. A telepített beépülő modulok hello listájában válassza ki a Service Fabric, és kattintson **frissítés**. A rendszer telepíti az elérhető frissítéseket.

> [!NOTE]
> Ha telepítésekor vagy frissítésekor a Service Fabric beépülő modul hello lassú, tooan Eclipse beállítás miatt lehet. Eclipse metaadatok gyűjti az Eclipse-példány regisztrált összes módosítások tooupdate helyeken. hello folyamat keresése és telepítése a Service Fabric beépülő modul frissítés toospeed lépjen túl**elérhető szoftverek helyek**. Törölje az hello jelölőnégyzetet egy mutat, toohello Service Fabric beépülő modul helye (http://dl.microsoft.com/eclipse/azure/servicefabric) hello kivételével minden webhelyére vonatkozóan.

## <a name="create-a-service-fabric-application-in-eclipse"></a>Service Fabric-alkalmazás létrehozása az Eclipse-ben

1.  Az Eclipse Neonfény, nyissa meg túl**fájl** > **új** > **más**. Válassza a **Service Fabric Project** (Service Fabric-projekt) lehetőséget, majd kattintson a **Next** (Tovább) gombra.

    ![Service Fabric – Új projekt, 1. oldal][create-application/p1]

2.  Adja meg a projekt nevét, majd kattintson a **Next** (Tovább) gombra.

    ![Service Fabric – Új projekt, 2. oldal][create-application/p2]

3.  Válassza a sablonok hello lista **Szolgáltatássablon**. Válassza ki a szolgáltatássablon típusát (Actor, Stateless, Container, Guest Binary – aktor, állapot nélküli, tároló, vendég bináris), majd kattintson a **Next** (Tovább) gombra.

    ![Service Fabric – Új projekt, 3. oldal][create-application/p3]

4.  Adja meg a hello szolgáltatás nevét és részletes ismertetése, és kattintson **Befejezés**.

    ![Service Fabric – Új projekt, 4. oldal][create-application/p4]

5. Amikor az első Service Fabric-projektet hoz létre a hello **nyissa meg a társított perspektíva** párbeszédpanel, kattintson a **Igen**.

    ![Service Fabric – Új projekt, 5. oldal][create-application/p5]

6.  Így néz ki az új projekt:

    ![Service Fabric – Új projekt, 6. oldal][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a>Service Fabric-alkalmazás felépítése és üzembe helyezése az Eclipse-ben

1.  Kattintson a jobb gombbal az új Service Fabric-alkalmazásra, majd válassza a **Service Fabric** lehetőséget.

    ![Service Fabric – helyi menü][publish/RightClick]

2. Hello almenü válassza ki a kívánt hello beállítást:
    -   toobuild hello alkalmazás nélkül a tisztítás, kattintson a **Build alkalmazás**.
    -   hello alkalmazás össze toodo kattintson **újraépítése alkalmazás**.
    -   a beépített összetevőihez tooclean hello alkalmazást kattintson **tiszta alkalmazás**.

3.  Ebből a menüből üzembe helyezheti vagy közzéteheti az alkalmazását, illetve visszavonhatja annak üzembe helyezését:
    -   toodeploy tooyour helyi fürthöz, és kattintson a **alkalmazás központi telepítése**.
    -   A hello **alkalmazás közzététele** párbeszédpanelen jelölje ki a közzétételi profilt:
        -  **Local.json**
        -  **Cloud.json**

     A JavaScript Object Notation (JSON) fájlok (például kapcsolati végpontok és biztonsági információk) helyi szükséges tooconnect tooyour vagy Azure-felhőbe információk tárolására.

  ![A Service Fabric közzétételi menüje][publish/Publish]

Egy másik módja toodeploy a Service Fabric-alkalmazás Eclipse használatával fut konfigurációkat.

  1.    Nyissa meg túl**futtatása** > **konfigurációk futtatása**.
  2.    A **Gradle projekt**, jelölje be hello **ServiceFabricDeployer** futtassa konfigurációt.
  3.    Hello jobb oldali ablaktáblában lévő hello **argumentumok** lapon, a **publishProfile**, jelölje be **helyi** vagy **felhő**.  hello alapértelmezett érték a **helyi**. távoli toodeploy tooa vagy felhő fürthöz, jelölje be **felhő**.
  4.    profilok közzététele, a Szerkesztés, hogy a nem üres-e a megfelelő információkat hello hello tooensure **Local.json** vagy **Cloud.json** igény szerint. Hozzáadhat vagy frissíthet végpontrészleteket és biztonsági hitelesítő adatokat.
  5.    Győződjön meg arról, hogy **munkakönyvtár** toodeploy kívánt toohello alkalmazás mutat. toochange hello alkalmazás, kattintson a hello **munkaterület** gombra, és válassza ki a kívánt hello alkalmazás.
  6.    Kattintson az **Apply** (Alkalmaz), majd a **Run** (Futtatás) gombra.

Néhány másodpercen belül megtörténik az alkalmazás felépítése és üzembe helyezése. A Service Fabric Explorerben hello központi telepítési állapot figyelése  

## <a name="add-a-service-fabric-service-tooyour-service-fabric-application"></a>A Service Fabric-szolgáltatás tooyour Service Fabric-alkalmazás hozzáadása

a Service Fabric szolgáltatás tooan meglévő Service Fabric-alkalmazás, tooadd hello lépéseket követve:

1.  Kattintson a jobb gombbal hello projekt tooadd szeretné, hogy a szolgáltatás számára, és kattintson a **Service Fabric**.

    ![Service Fabric – Szolgáltatás hozzáadása, 1. oldal][add-service/p1]

2.  Kattintson a **Fabric-szolgáltatás hozzáadása**, és a lépéseket tooadd készletét teljes hello toohello szolgáltatásprojektet.
3.  Jelölje be hello Szolgáltatássablon tooadd tooyour projektet, és kattintson a **következő**.

    ![Service Fabric – Szolgáltatás hozzáadása, 2. oldal][add-service/p2]

4.  Írjon be hello szolgáltatás neve (és más részleteket, igény szerint), és kattintson a hello **hozzáadása szolgáltatás** gombra.  

    ![Service Fabric – Szolgáltatás hozzáadása, 3. oldal][add-service/p3]

5.  Hello szolgáltatás hozzáadása után a projekt általános struktúra a következő projekt hasonló toohello néz ki:

    ![Service Fabric – Szolgáltatás hozzáadása, 4. oldal][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a>A Service Fabric Java-alkalmazás jegyzékverzióinak szerkesztése

tooedit jegyzék verziók, kattintson jobb gombbal a hello projektet, nyissa meg túl**Service Fabric** válassza **Manifest verziók szerkesztése...**  hello menü legördülő menüből. Hello varázslóban, frissítheti az hello jegyzék a jegyzék, szolgáltatás alkalmazásjegyzék és verziók hello a **kód**, **Config** és **adatok** csomagok.

Ha hello beállítást **automatikus frissítése az alkalmazás és szolgáltatás verziók** és majd frissítés, majd hello jegyzék verziók automatikusan frissül. például toogive, kiválaszthatja hello jelölőnégyzetet, majd a frissítés hello verziójának **kód** 0.0.0 verziójában too0.0.1, majd kattintson a **Befejezés**, majd szolgáltatás a fürtjegyzék verziója és az alkalmazás jegyzékében verzió too0.0.1 automatikusan frissítve lesz.

## <a name="upgrade-your-service-fabric-java-application"></a>A Service Fabric Java-alkalmazásának frissítése

A frissítési esetben mondja ki a létrehozott hello **App1** projektben hello Service Fabric az Eclipse beépülő modul segítségével. Telepítette azt beépülő modul toocreate hello segítségével nevű alkalmazást **fabric: / App1Application**. hello alkalmazástípus **App1AppicationType**, és a hello alkalmazás verziója 1.0. Most szeretné tooupgrade az alkalmazás rendelkezésre állásának megszakítása nélkül.

Először a beállítások módosításait tooyour alkalmazás, és majd újraépítése a módosított hello szolgáltatást. Frissítés hello szolgáltatás jegyzékfájl (ServiceManifest.xml) módosított frissítése hello verzióival hello szolgáltatás (és kód, konfigurációs vagy vonatkozó adatokat). Is módosítsa a hello alkalmazás jegyzékfájlja (ApplicationManifest.xml) frissítése hello verziószámú hello alkalmazáshoz, és hello módosított szolgáltatás.  

tooupgrade az alkalmazás Eclipse Neonfény használatával, hozhat létre egy duplikált futtatási konfigurációs profilt. Ezt követően tooupgrade használni az alkalmazást igény szerint.

1.  Nyissa meg túl**futtatása** > **konfigurációk futtatása**. Hello bal oldali ablaktáblában kattintson a hello kis nyílra toohello bal oldalán **Gradle projekt**.
2.  Kattintson a jobb gombbal a **ServiceFabricDeployer** elemre, majd válassza a **Duplicate** (Megkettőzés) parancsot. Adjon egy új nevet a konfigurációnak, például **ServiceFabricUpgrader**.
3.  Hello jobb oldali panelen, a hello **argumentumok** lapján módosítsa **- Pconfig = "telepítése"** túl**- Pconfig = a "frissítés"**, és kattintson a **alkalmaz**.

Ez a folyamat hoz létre, és futtassa a konfigurációs profil mentése használhat bármilyen idő tooupgrade az alkalmazást. Azt is lekérése hello legújabb frissített alkalmazástípus verziója hello Alkalmazásjegyzék-fájl.

hello alkalmazás frissítése néhány percet vesz igénybe. A Service Fabric Explorerben hello az alkalmazásfrissítés figyelheti.

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a>Service Fabric Java-alkalmazások régi toobe Maven használt áttelepítése
A Service Fabric Java SDK tooMaven tárházat nemrég került át Service Fabric Java szalagtárak. Amíg hello használata az eclipse-ben létrehozhat új alkalmazások legújabb frissített projektek (Ez az a Maven képes toowork) hoz létre, frissítheti a meglévő Service Fabric állapot nélküli vagy szereplő Java-alkalmazások, amelyek hello Service Fabric Java SDK használata korábbi, toouse hello Service Fabric Java függőségek a Mavenben. Kövesse az említett hello lépéseket [Itt](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure Maven együttműködve biztosítja a régebbi alkalmazásokat.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-eclipse/service-fabric-eclipse-plugin.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
