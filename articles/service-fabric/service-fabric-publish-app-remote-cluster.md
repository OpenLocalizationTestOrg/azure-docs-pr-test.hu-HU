---
title: "az alkalmazás tooa távoli fürt a Visual Studio aaaPublish |} Microsoft Docs"
description: "Ismerje meg, hogyan toopublish egy alkalmazás tooa távoli a service fabric fürt Visual Studio használatával."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: faecd892-eb54-4d9c-8023-c67442afb8e8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/29/2016
ms.author: cawa
ms.openlocfilehash: d0f06f120cc7e22f3f8e73ce0970e1da5823e647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-visual-studio"></a>Központi telepítése, és távolítsa el az alkalmazásokat a Visual Studio használatával
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [FabricClient API-k](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

hello Azure Service Fabric-bővítményt a Visual Studio ide egy könnyen kezelhető, megismételhető és parancsfájlok futtatására alkalmas toopublish egy alkalmazás tooa Service Fabric-fürt.

## <a name="hello-artifacts-required-for-publishing"></a>a közzététel szükséges hello összetevők
### <a name="deploy-fabricapplicationps1"></a>FabricApplication.ps1 központi telepítése
Ez azért, amely a közzétételi profil elérési út használ paraméterként alkalmazás-közzététel a Service Fabric PowerShell-parancsfájlt. Mivel ez a parancsfájl az alkalmazás a része,-e üdvözlő toomodify azt az alkalmazást a megfelelő.

### <a name="publish-profiles"></a>Profilok közzététele
A mappa hello Service Fabric-alkalmazás projekt neve a **PublishProfiles** XML-fájlok, például egy alkalmazás-közzététel alapvető információt tároló tartalmazza:

* A Service Fabric-fürt kapcsolódási paraméterek
* Tooan alkalmazás paraméterfájl elérési útja
* Beállítások frissítése

Alapértelmezés szerint az alkalmazás tartalmazza három profilok közzététele: Local.1Node.xml Local.5Node.xml és Cloud.xml. Másolás és beillesztés hello alapértelmezett fájlok további profilok adhat.

### <a name="application-parameter-files"></a>Alkalmazásparaméter-fájlokat
A mappa hello Service Fabric-alkalmazás projekt neve a **ApplicationParameters** XML-fájlok az alkalmazás a felhasználó által megadott jegyzék paramétereket tartalmaz. Application manifest-fájlok is paraméteres, így a központi telepítési beállításokra eltérő értékeket is használhat. toolearn az alkalmazás paraméterezése kapcsolatos további információkért lásd: [kezelése a Service Fabric több környezet](service-fabric-manage-multiple-environment-app-configuration.md).

> [!NOTE]
> A aktorszolgáltatások, hello projekt összeállítsa először tooedit hello fájlt valamelyik szerkesztőben vagy hello megkísérlése előtt közzététele a párbeszédpanel bezárásához. Ennek az az oka hello jegyzékfájlt részét hello az összeállítás közbeni jön létre.

## <a name="toopublish-an-application-using-hello-publish-service-fabric-application-dialog-box"></a>alkalmazás hello Fabric-alkalmazás közzététele párbeszédpanelen toopublish
hello következő lépések bemutatják, hogyan egy alkalmazást, amely toopublish hello **Fabric-alkalmazás közzététele** hello Visual Studio Service Fabric-eszközök által biztosított párbeszédpanel.

1. A Service Fabric-alkalmazás projekt hello hello helyi menüben válassza a **közzététel...** tooview hello **Fabric-alkalmazás közzététele** párbeszédpanel megnyitásához.
   
    ![hello ** közzététele Service Fabric Application ** párbeszédpanel][0]
   
    hello kiválasztott hello fájl **céloz profil** legördülő lista akkor, ha hello-beállítások, kivételével **verziók Manifest**, lesznek mentve. Felhasználhatja egy meglévő profilt, vagy hozzon létre egy újat kiválasztásával **< profilok kezelése... >** a hello **céloz profil** legördülő lista. Ha úgy dönt, hogy a közzétételi profilt, annak tartalmát hello megfelelő mezőkben hello párbeszédpanel jelenik meg. toosave a módosításokat, tetszőleges időpontban, válassza a hello **profil mentése** hivatkozásra.    
2. A hello **csatlakozási végpont** területen adjon meg egy helyi vagy távoli Service Fabric fürt közzétételi végpont. tooadd, vagy módosítsa a hello csatlakozási végpont, kattintson a hello **csatlakozási végpont** legördülő listából. hello listán mutatja hello érhető el a Service Fabric fürt kapcsolati végpontok toowhich közzététele az Azure előfizetés alapján. Megjegyzés: Ha még nem jelentkezett a tooVisual Studio, meg fog felszólító toodo stb.
   
    Hello fürt kiválasztása párbeszédpanel bezárásához toochoose hello készletből az elérhető előfizetések és a fürt használja.
   
    ![hello ** válasszon Service Fabric fürt ** párbeszédpanel][1]
   
   > [!NOTE]
   > Ha szeretné toopublish tooan tetszőleges végpontot (például egy entitás fürt), lásd: hello **tooan tetszőleges a fürt végpontja közzétételi** az alábbi szakasz.
   > 
   > 
   
    Ha úgy dönt, hogy a végpont, a Visual Studio hello kapcsolat toohello kijelölt Service Fabric-fürt érvényesíti. Hello fürt nem biztonságos, ha a Visual Studio tooit közvetlenül is csatlakozhat. Azonban ha hello fürt biztonságos, szüksége lesz tooinstall egy tanúsítványt a helyi számítógépen, a folytatás előtt. Lásd: [hogyan tooconfigure biztonságos kapcsolatok](service-fabric-visualstudio-configure-secure-connections.md) további információt. Amikor elkészült, válassza ki a hello **OK** gombra. hello kijelölt fürt megjelenik hello **Fabric-alkalmazás közzététele** párbeszédpanel megnyitásához.
3. A hello **alkalmazás paraméterfájl** legördülő listából válassza tooan alkalmazás paraméterfájl. Egy alkalmazás paraméterfájl paraméter értékét a felhasználó által megadott hello Alkalmazásjegyzék-fájl tartalmazza. tooadd, vagy módosítsa a paramétert, válassza ki a hello **szerkesztése** gombra. Adja meg vagy módosítsa a hello hello paraméter értékével **paraméterek** rács. Amikor elkészült, válassza ki a hello **mentése** gombra.
   
    ![hello ** szerkesztése paraméterek ** párbeszédpanel][2]
4. Használjon hello **frissítési hello alkalmazás** jelölőnégyzet toospecify hogy ez a művelet közzé-e frissítés. Frissítés közzététele műveletek eltér a normál műveletek közzététele. Lásd: [Service Fabric alkalmazás frissítése](service-fabric-application-upgrade.md) eltérések listáját. frissítési beállítások tooconfigure, válassza ki a hello **beállítások konfigurálása** hivatkozásra. hello frissítési paraméter-szerkesztő jelenik meg. Lásd: [konfigurálása a Service Fabric-alkalmazás frissítését hello](service-fabric-visualstudio-configure-upgrade.md) frissítési paraméterekkel kapcsolatos további toolearn.
5. Válassza ki a hello **Manifest verzióit...** gomb tooview hello **szerkesztése verziók** párbeszédpanel megnyitásához. Egy frissítési tootake hely tooupdate alkalmazás és szolgáltatás verziók kell. Lásd: [Service Fabric-alkalmazás frissítési oktatóanyag](service-fabric-application-upgrade-tutorial.md) toolearn hogyan alkalmazás és a service manifest verziók hatással lehet a frissítési folyamat.
   
    ![hello ** szerkesztése verziók ** párbeszédpanel][3]
   
    Ha hello alkalmazás és szolgáltatás verziók szemantikai versioning 1.0.0 vagy numerikus értékek például 1.0.0.0 hello formátumban, válassza ki a hello **automatikus frissítése az alkalmazás és szolgáltatás verziók** lehetőséget. Ha úgy dönt, ezt a beállítást, a hello szolgáltatás és alkalmazás verziószáma mindig a kódot, a konfiguráció automatikusan frissülnek, vagy adatok Csomagverzió frissül. Ha manuálisan szeretné tooedit hello verziók, törölje a jelet hello jelölőnégyzet toodisable ezt a szolgáltatást.
   
   > [!NOTE]
   > Összes csomag bejegyzések tooappear szereplő projekthez esetén először létrehozni hello projekt toogenerate hello Service Manifest fájlok hello bejegyzéseket.
   > 
   > 
6. Ha elkészült adja meg az összes szükséges beállítást hello, válassza ki a hello **közzététel** toopublish gombra a kiválasztott alkalmazás toohello Service Fabric-fürt. a megadott hello-beállítások alkalmazása toohello közzétételi folyamat.

## <a name="publish-tooan-arbitrary-cluster-endpoint-including-party-clusters"></a>Tooan tetszőleges a fürt végpontja (beleértve az entitás-fürtök) közzététele
Visual Studio közzététel élmény hello közzététele az Azure-előfizetésekkel társított tooremote fürtök van optimalizálva. Azt azonban lehetséges toopublish tooarbitrary végpontok (például a Service Fabric-fürtök fél) által közvetlen szerkesztésével hello közzétételi profil XML. Fent ismertetett három profilok közzététele alapértelmezett--által biztosított**Local.1Node.xml**, **Local.5Node.xml**, és **Cloud.xml**– de üdvözlő toocreate További profilok különböző környezetekben. Például érdemes toocreate profil tooparty fürtök, lehet, hogy nevű közzétételéhez **Party.xml**.

Ha nem biztonságos fürt tooan kapcsolódik, szükséges csak a hello fürt kapcsolat végpontja, például a `partycluster1.eastus.cloudapp.azure.com:19000`. Abban az esetben a hello hello csatlakozási végpont közzététele profil például ehhez hasonló lenne:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Ha védett fürt tooa kapcsolódik, hello ügyféltanúsítványt a hitelesítéshez használt hello helyi tároló toobe tooprovide hello részleteit is szüksége lesz. További részletekért lásd: [konfigurálása biztonságos kapcsolatok tooa Service Fabric-fürt](service-fabric-visualstudio-configure-secure-connections.md).

  Ha a közzétételi profilt készen áll, hivatkozhasson rá az hello közzétételére párbeszédpanelt alább látható módon.

  ![Új közzétételi profil közzé párbeszédpanel][4]

  Vegye figyelembe, hogy ebben az esetben új hello közzétételi profil hello alapértelmezett alkalmazásparaméter-fájlokat a tooone mutat. Ez akkor megfelelő, ha azt szeretné, toopublish hello alkalmazás konfigurációs tooa ugyanannyi környezetekben. Ezzel szemben a toohave különböző konfigurációt, amelyet toopublish a környezetben kívánt esetben ez tenné logika toocreate a megfelelő alkalmazás paraméterfájl.

## <a name="next-steps"></a>Következő lépések
Hogyan tooautomate hello közzétételi folyamat folyamatos integrációt környezetben: toolearn [beállíthat folyamatos integrációt a Service Fabric](service-fabric-set-up-continuous-integration.md).

[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
