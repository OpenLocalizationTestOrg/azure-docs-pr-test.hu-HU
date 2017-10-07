---
title: "aaaVisualizing a Service Fabric Explorerrel fürt |} Microsoft Docs"
description: "Service Fabric Explorerben talál egy olyan webalapú eszköz ellenőrzi, és a felhőalapú alkalmazások és a Microsoft Azure Service Fabric-fürt csomópontjának kezelése."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: ryanwi
ms.openlocfilehash: 73adc4fc254cf6b949b4419b02a046cee3f6a83d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>A fürt megjelenítése a Service Fabric Explorerrel
Service Fabric Explorerben talál egy olyan webalapú eszköz ellenőrzi, és az alkalmazások és az Azure Service Fabric-fürt csomópontjának kezelése. Service Fabric Explorer gazdája közvetlenül hello fürt, ezért mindig elérhető, függetlenül attól, amelyen fut a fürtön.

## <a name="video-tutorial"></a>Oktatóvideó

Hogyan tekintse meg a Service Fabric Explorer toouse toolearn hello a Microsoft Virtual Academy videót a következő:

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-tooservice-fabric-explorer"></a>Csatlakozás tooService Fabric Explorerrel
Ha már elvégezte túl hello utasításokat[a fejlesztőkörnyezet előkészítése](service-fabric-get-started.md), elindíthatja a Service Fabric Explorer a helyi fürtön lévő toohttp://localhost:19080 navigálva / Explorer.

## <a name="understand-hello-service-fabric-explorer-layout"></a>Service Fabric Explorer elrendezés hello ismertetése
Service Fabric Explorer bontható hello bal oldali fában hello segítségével. Hello fa hello gyökerében hello fürt irányítópult áttekintése a fürt, beleértve az alkalmazás és a csomópont állapotának összegzését.

![Service Fabric Explorer fürt irányítópult][sfx-cluster-dashboard]

### <a name="view-hello-clusters-layout"></a>Hello fürt Elrendezés megtekintése
A Service Fabric-fürt csomópontjának kerülnek, egy kétdimenziós rácsot a tartalék tartományok között, és a frissítési tartományt. Az elhelyezési biztosítja, hogy az alkalmazások továbbra is elérhető, a hardver meghibásodása és alkalmazásfrissítések hello jelenlétét. Hogyan hello a jelenlegi fürthöz van leírva hello betűcsoport-leképezés használatával tekintheti meg.

![Service Fabric Explorer betűcsoport-leképezés][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Nézet alkalmazások és szolgáltatások
hello fürt két részfák tartalmaz: egyet az alkalmazások és más csomópontok.

Hello alkalmazás nézet toonavigate Service Fabric logikai hierarchia keresztül is használhatja: alkalmazások, szolgáltatások, partíciók és replikáit.

Hello az alábbi példában a hello alkalmazás **MyApp** két szolgáltatásból áll **MyStatefulService** és **WebService**. Mivel a **MyStatefulService** állapotalapú, egy elsődleges és két másodlagos replika partíciót tartalmaz. Ezzel szemben WebSvcService állapot nélküli, és egyetlen példányt tartalmaz.

![Service Fabric Explorer alkalmazás megtekintése][sfx-application-tree]

Hello fa egyes szintjein a fő ablaktáblán hello hello elem vonatkozó információkat jeleníti meg. Például láthatja hello állapotát és egy adott szolgáltatáshoz verziója.

![Service Fabric Explorer essentials ablaktábla][sfx-service-essentials]

### <a name="view-hello-clusters-nodes"></a>Hello fürt csomópontjai megtekintése
hello csomópont nézetben látható hello hello fürt fizikai elrendezését. Az egyes csomópontoknál megtekintheti, hogy melyik alkalmazások kódja üzemel az adott csomóponton. Pontosabban láthatja, melyik replikák futó van.

## <a name="actions"></a>Műveletek
Service Fabric Explorer biztosít egy gyorsan tooinvoke műveleteket az egyes csomópontok, alkalmazások és szolgáltatások a fürtön belül.

Például toodelete egy alkalmazáspéldány hello fa hello bal oldali lehetőségek közül választhat hello alkalmazást, és válassza **műveletek** > **alkalmazás törlése**.

![A Service Fabric Explorerben alkalmazás törlése][sfx-delete-application]

> [!TIP]
> Végezhet ugyanazokat a műveleteket hello tooeach elem következő hello három pont gombra kattintva.
>
>

hello alábbi táblázat minden egyes entitásnál elérhető hello műveletek:

| **Entitás** | **Művelet** | **Leírás** |
| --- | --- | --- |
| Alkalmazás típusa |Típus leépítése |Hello alkalmazáscsomag eltávolítása hello fürt lemezképtárolóhoz. Adott típusú toobe előbb eltávolítja az összes alkalmazás szükséges. |
| Alkalmazás |Alkalmazás törlése |Törli a hello alkalmazást, beleértve a hozzá tartozó szolgáltatások és azok állapota (ha van ilyen). |
| Szolgáltatás |Szolgáltatás törlése |(Ha van ilyen), törölje a hello szolgáltatást és annak állapotát. |
| Csomópont |Aktiválás |Hello csomópont aktiválása. |
| Csomópont | (Szünet) inaktiválása | A jelenlegi állapotában hello csomópont felfüggesztése. Szolgáltatások továbbra is toorun, de a Service Fabric proaktív helyezi át a semmit, vagy ki, kivéve, ha ez szükséges tooprevent kimaradás vagy adatinkonzisztenciát. Ez a művelet akkor általánosan használt tooenable szolgáltatásokat egy adott csomópont tooensure, hogy azok helyezi át az ellenőrzés során a hibakeresést. | |
| Csomópont | Inaktiválás (újraindítás) | Biztonságosan haladhat minden memórián belüli szolgáltatás ki olyan csomópont, és zárja be az állandó szolgáltatásokban. Jellemzően hello gazdafolyamat vagy a gép kell toobe újraindításakor. | |
| Csomópont | (Az adatok eltávolítása) inaktiválása | Biztonságosan zárja be a megfelelő tartalék replikák létrehozása után hello csomóponton futó összes szolgáltatás. Jellemzően akkor csomópont (vagy legalább a tárhely) kívül Bizottság véglegesen foglalja. | |
| Csomópont | Távolítsa el a csomópont állapota | Távolítsa el a Tudásbázis egy csomópont replikák hello fürtből. Általában akkor használható, ha egy már leállt csomópont helyreállíthatatlan tekintendő. | |
| Csomópont | Újraindítás | Egy Csomóponthiba szimulálása hello csomópont újraindításával. További információ [Itt](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps) | |

Mivel számos műveletet károsak, akkor lehetnek kéri, a célt tooconfirm hello művelet befejezése előtt.

> [!TIP]
> Minden művelet, amely a Service Fabric Explorer segítségével végezheti el is PowerShell vagy a REST API-t tooenable automation végezhető el.
>
>

Service Fabric Explorer toocreate alkalmazáspéldányok használ egy adott alkalmazás típusától és verziójától. Hello alkalmazástípus válasszon hello faszerkezetes nézetben, majd kattintson a hello **app-példány létrehozása** hivatkozás toohello verzióra szeretné hello jobb oldali ablaktáblán.

![Az alkalmazáspéldány létrehozása a Service Fabric Explorerben][sfx-create-app-instance]

> [!NOTE]
> Jelenleg nem paraméterezett Service Fabric Explorer használatával létrehozott alkalmazás-példányokon. Létrehozásuk alapértelmezett paraméterértékek használata.
>
>

## <a name="connect-tooa-remote-service-fabric-cluster"></a>Csatlakozás távoli tooa Service Fabric-fürt
Ha tudja hello a fürt végpontja, és rendelkezik a szükséges engedélyekkel a Service Fabric Explorer minden böngészőből végezheti el. Ez azért, mert Service Fabric Explorer egy másik szolgáltatás, amely az hello fürtben.

### <a name="discover-hello-service-fabric-explorer-endpoint-for-a-remote-cluster"></a>Távoli fürt hello Service Fabric Explorer-végpont felderítése
tooreach Service Fabric Explorer egy adott fürtben, irányítsa a böngészőt, hogy:

http://&lt;a fürt végpontja&gt;: 19080/Explorer

Az Azure-fürtök esetén hello teljes URL-címet is rendelkezésre áll a hello fürt essentials ablaktábláján hello Azure-portálon.

### <a name="connect-tooa-secure-cluster"></a>Csatlakoztassa tooa biztonságos fürtöt
Tanúsítványok, valamint az Azure Active Directory (AAD) használó ügyfél hozzáférési tooyour Service Fabric fürt szabályozhatja.

Tooconnect tooService Fabric Explorer egy biztonságos fürtön kísérel meg, majd hello fürt konfigurációjától függően lesz szükséges toopresent ügyféltanúsítvánnyal kell, illetve az AAD használja-e jelentkezni.

## <a name="next-steps"></a>Következő lépések
* [Tesztelhetőségi áttekintése](service-fabric-testability-overview.md)
* [A Visual Studio a Service Fabric-alkalmazások kezelése](service-fabric-manage-application-in-visual-studio.md)
* [A Service Fabric-alkalmazás központi telepítésének PowerShell használatával](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
