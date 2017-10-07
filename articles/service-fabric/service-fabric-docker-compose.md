---
title: Service Fabric Docker Compose Preview aaaAzure
description: "Az Azure Service Fabric elfogadja a Docker Compose formátum toomake azt könnyebb tooorchestrate meglévő tárolók Service Fabric használatával. Ez a támogatás jelenleg előzetes verzió."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: a60d1321fd6ef07b241a98c5ab2b8dfe5d441b53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="docker-compose-application-support-in-azure-service-fabric-preview"></a>Docker Compose alkalmazások támogatása az Azure Service Fabric (előzetes verzió)

Docker használ hello [docker-compose.yml](https://docs.docker.com/compose) fájl több tároló alkalmazások meghatározásához. toomake az ügyfelek könnyen térhetnek ismeri a Docker tooorchestrate meglévő tároló alkalmazások az Azure Service Fabric, vannak megadva Docker Compose preview támogatása natív módon hello platform. A Service Fabric fogadhat 3 és az újabb `docker-compose.yml` fájlokat. 

Ez a támogatás jelenleg előzetes verzióban érhető, mert a Compose irányelvek csak egy részhalmazát esetén támogatott. Például alkalmazás frissítések nem támogatottak. Azonban mindig távolítsa el és telepíthet központilag alkalmazásokat helyett őket.

toouse ez megtekintéséhez a fürt létrehozása verziójával 5.7-es vagy nagyobb a Service Fabric-futtatókörnyezet hello hello hello megfelelő SDK és Azure-portálon keresztül. 

> [!NOTE]
> Ez a funkció jelenleg előzetes verzióban érhető és éles környezetben nem támogatott.

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a>A Service Fabric a Docker Compose fájl központi telepítése

hello következő parancsokat a Service Fabric-alkalmazás létrehozása (nevű `fabric:/TestContainerApp` a fenti példa hello), amelyet figyelése és kezelése, mint bármely más Service Fabric-alkalmazás. Használhatja a megadott alkalmazásnév hello állapotlekérdezések száma.

### <a name="use-powershell"></a>A PowerShell használata

Egy docker-compose.yml fájlt a Service Fabric Compose-alkalmazás létrehozása a következő parancsot a PowerShell hello futtatásával:

```powershell
New-ServiceFabricComposeApplication -ApplicationName fabric:/TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

`RegistryUserName`és `RegistryPassword` tekintse meg a toohello tároló beállításjegyzék felhasználónevet és jelszót. Hello alkalmazás befejezése után ellenőrizheti annak állapotát hello a következő parancs használatával:

```powershell
Get-ServiceFabricComposeApplicationStatus -ApplicationName fabric:/TestContainerApp -GetAllPages
```

toodelete hello Compose alkalmazást PowerShell, a következő parancs használata hello keresztül:

```powershell
Remove-ServiceFabricComposeApplication  -ApplicationName fabric:/TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a>Azure Service Fabric CLI (sfctl) használata

Service Fabric CLI parancs a következő hello is használhatja:

```azurecli
sfctl compose create --application-id fabric:/TestContainerApp --compose-file docker-compose.yml [ [ --repo-user --repo-pass --encrypted ] | [ --repo-user ] ] [ --timeout ]
```

Hello alkalmazás létrehozása után hello a következő parancs használatával ellenőrizheti az állapotát:

```azurecli
sfctl compose status --application-id TestContainerApp [ --timeout ]
```

toodelete hello új alkalmazást, a következő parancs hello használata:

```azurecli
sfctl compose remove  --application-id TestContainerApp [ --timeout ]
```

## <a name="supported-compose-directives"></a>Támogatott Compose irányelvek

Ez az előzetes kiadás egy részhalmazát hello konfigurációs beállítások formátumból hello Compose 3-as verzió, beleértve a következő primitívek hello támogatja:

* Szolgáltatások > telepítése > replikák
* Szolgáltatások > telepítése > elhelyezési > megkötések
* Szolgáltatások > telepítése > erőforrások > korlátok
    * -cpu-megosztások
    * -memória
    * -memória-csere
* Szolgáltatások > parancsok
* Szolgáltatások > környezet
* Szolgáltatások > portok
* Szolgáltatások > kép
* Szolgáltatások > elszigetelése (csak Windows)
* Szolgáltatások > Naplózás > illesztőprogram
* Szolgáltatások > Naplózás > illesztőprogram > Beállítások
* Kötet & telepítése > kötet

Hello fürt erőforrás-korlátok, felelősek beállítása, a [Service Fabric erőforrás irányítás](service-fabric-resource-governance.md). Minden más Docker Compose irányelvek nem támogatottak az előzetes verzió.

## <a name="servicednsname-computation"></a>ServiceDnsName számítási

Ha hello szolgáltatás neve meg egy új fájlt a teljes tartománynév (Ez azt jelenti, hogy pontot tartalmaz [.]), a Service Fabric által regisztrált hello DNS-név `<ServiceName>` (beleértve a hello pont). Ha nem, akkor minden elérésiút-szegmens az hello alkalmazásnév hello szolgáltatás DNS-név, hello első elérésiút-szegmens hello felső szintű tartomány címke váljon a tartomány címke válik.

Például ha hello megadott alkalmazásnév nem `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` hello regisztrált DNS-neve lesz.

## <a name="differences-between-compose-instance-definition-and-service-fabric-application-model-type-definition"></a>Új (példány-definíció) és a Service Fabric-alkalmazás modell (típusdefinícióban) közötti különbségek

Egy docker-compose.yml fájlt a tárolók, beleértve azok tulajdonságait és konfigurációk telepíthető együttesét írja le.
Például hello fájl tartalmazhat környezeti változókat és a portok. Telepítési paraméterek, például egy elhelyezési korlátozás, erőforrás-korlátozások és DNS-nevek, a hello docker-compose.yml fájlt is megadható.

Hello [Service Fabric-alkalmazás modell](service-fabric-application-model.md) használ szolgáltatás és alkalmazás típusainak, sok alkalmazás példány esetében hello ugyanarra a típusra. Lehet például egy ügyfél egy alkalmazáspéldányt. Ez a típus-alapú modell hello több verzióit támogatja ugyanazt az alkalmazástípus hello futásidejű regisztrált.

Például az ügyfél A rendelkezhet példányként létrehozott AppTypeA 1.0 típusú kérelmet, és B ügyfél rendelkezhet a hello példányként létrehozott egy másik alkalmazás azonos típusú és verziót. Hello alkalmazástípusok hello alkalmazásjegyzékeknek határozhat meg, és hello alkalmazás nevét és a telepítési paraméterek megadva hello alkalmazás létrehozásakor.

Bár ez a modell rugalmasságot nyújt, azt is tervezi, ahol típusok tartoznak, amely implicit hello jegyzékfájl egyszerűbb, a példány-alapú telepítési modell toosupport. Ebben a modellben minden alkalmazás saját független jegyzékfájl lekérdezi. Ebből a törekvésből támogatásának hozzáadásával a docker-compose.yml, ez az egy példány-alapú üzembe helyezési formátum jelenleg előzetes.

## <a name="next-steps"></a>Következő lépések

* Olvassa a hello [Service Fabric alkalmazásmodellt.](service-fabric-application-model.md)
* [A Service Fabric parancssori felület használatának első lépései](service-fabric-cli.md)
