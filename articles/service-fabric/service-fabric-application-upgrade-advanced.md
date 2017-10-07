---
title: "Alkalmazás frissítése témakörök aaaAdvanced |} Microsoft Docs"
description: "Ez a cikk a Service Fabric-alkalmazás tooupgrading vonatkozó speciális témaköröket ismerteti."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar;chackdan
ms.openlocfilehash: bdaf3db6209c574d39f57e0bf9951fad5ad1cbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>A Service Fabric az alkalmazásfrissítés: speciális kapcsolatos témakörök
## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Hozzáadásával vagy eltávolításával a szolgáltatások egy alkalmazás frissítése során
Ha egy új szolgáltatás hozzáadása tooan alkalmazás már telepítve, és közzétett frissítéseként hello új szolgáltatás hozzáadott toohello telepített alkalmazás.  Ilyen frissítés hello szolgáltatások hello alkalmazás már nincs hatással. Azonban felvett hello szolgáltatás egy példánya el kell indítani a hello új szolgáltatás toobe aktív (hello segítségével `New-ServiceFabricService` parancsmag).

Szolgáltatások frissítés részeként-alkalmazásból is eltávolítható lesz. Azonban hello-to-be törölt szolgáltatás minden aktuális példányának hello frissítés folytatása előtt le kell állítani (hello segítségével `Remove-ServiceFabricService` parancsmag).

## <a name="manual-upgrade-mode"></a>Manuális frissítési módra
> [!NOTE]
> hello nem figyelt manuális mód csak a sikertelen vagy felfüggesztett frissítés figyelembe kell venni. hello figyelt módja hello ajánlott frissítési mód a Service Fabric-alkalmazások.
>
>

Az Azure Service Fabric biztosít több frissítési módok toosupport fejlesztési és éles fürt. A kiválasztott központi telepítési beállítások különböző környezetek eltérőek lehetnek.

figyelt hello működés közbeni alkalmazásfrissítés hello legjellemzőbb frissítési toouse hello éles környezetben. Hello frissítésekor házirend van megadva, a Service Fabric biztosítja, hogy hello alkalmazás kifogástalan előtt hello frissítés előrehalad.

 hello alkalmazás-rendszergazda használhat működés közbeni alkalmazás frissítési mód toohave teljes szabályozhatják hello frissítési folyamat állapota hello keresztül különböző frissítési tartományok hello manuális. Ebben a módban akkor hasznos, ha testreszabott vagy összetett értékelési állapotházirend szükség, vagy a nem hagyományos frissítés történik (például hello alkalmazás már szerepel az adatveszteség).

Végezetül hello alkalmazás automatikus működés közbeni frissítés esetén hasznos fejlesztési vagy tesztelési környezetben tooprovide gyors iterációs ciklus szolgáltatás fejlesztése során.

## <a name="change-toomanual-upgrade-mode"></a>Toomanual frissítési módjának módosítása
**Manuális**--Stop hello alkalmazás frissítést, a hello aktuális UD és módosítási hello mód tooUnmonitored manuális frissítése. hello rendszergazdának kell toomanually hívás **MoveNextApplicationUpgradeDomainAsync** tooproceed hello a frissítése vagy indul el, a visszaállítás elindítása az új frissítés által. Miután hello frissítés hello manuális módba lép, marad hello manuális üzemmódban addig, amíg az új frissítést lehet kezdeményezni. Hello **GetApplicationUpgradeProgressAsync** parancs visszaadja a HÁLÓ\_alkalmazás\_frissítése\_állapot\_működés közbeni\_előre\_FÜGGŐBEN.

## <a name="upgrade-with-a-diff-package"></a>A diff csomag frissítése
A Service Fabric-alkalmazás tudja frissíteni a teljes, önállóan alkalmazási csomaggal rendelkező kiépítés. Egy alkalmazás, amely tartalmazza a frissített hello alkalmazásfájlok csak különbözeti csomag használatával is frissítése, hello alkalmazásjegyzék és hello service manifest-fájlok frissítése.

A teljes alkalmazáscsomag szükséges toostart összes hello fájlokat tartalmazza, és futtassa a Service Fabric-alkalmazás. Diff csomag csak a módosított hello utolsó kiépítés és hello aktuális frissítés között hello fájlokat tartalmazza, valamint hello teljes alkalmazás jegyzékfájlja és hello service manifest fájlt. Összes hivatkozás hello alkalmazásjegyzék vagy szolgáltatás jegyzékfájl, amely nem található a hello build elrendezés hello lemezképtárolóhoz kell keresni.

Teljes alkalmazáscsomagok olyan alkalmazások toohello fürt hello első telepítéshez szükséges. Soron következő frissítések a teljes alkalmazáscsomagok vagy különbözeti csomag is lehet.

Ha különbözeti csomagot lenne a legjobb megoldás alkalommal:

* Különbözeti csomag használata ajánlott, ha a nagy alkalmazáscsomagok, amelyek több service manifest fájl és/vagy több kód csomagok, konfigurációs csomagokat vagy adatok csomagok hivatkozik.
* Diff csomag részesíti előnyben, amikor a központi telepítési rendszer közvetlenül a az alkalmazás létrehozási folyamata hello build elrendezés generáló. Ebben az esetben annak ellenére, hogy hello kódja nem változott, újonnan beépített szerelvények egy másik ellenőrzőösszeg beolvasása. A teljes alkalmazás csomag használata, tooupdate hello verzió összes kódot csomag kell rendelkeznie. Diff csomagot használ, csak megadja, hogy módosultak a hello fájlok és hello jegyzékfájlt Ha hello verziója megváltozott.

Ha egy alkalmazás frissítve van, a Visual Studio használatával, hello különbözeti csomag automatikusan van közzétéve. diff csomag toocreate manuálisan, hello alkalmazásjegyzék, és hello szolgáltatás jegyzéknek meg kell adni, de csak a módosított hello csomagok végső alkalmazáscsomag hello fel kell venni.

Például kezdjük hello (verziószámok ismertetése könnyű előírt) alkalmazás a következő:

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Most tegyük fel a keresett tooupdate csak hello kódcsomag service1 a PowerShell használatával különbözeti csomagot. A frissített alkalmazás már hello mappastruktúra a következő:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Ebben az esetben frissítenie hello application manifest too2.0.0 és hello szolgáltatás jegyzékfájljának service1 tooreflect hello csomag frissítése. az alkalmazáscsomag hello mappa hello struktúra a következő lenne:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Következő lépések
[Az alkalmazás használata a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti Önt az alkalmazásfrissítés Visual Studio használatával.

[Az alkalmazás használatával Powershell frissítése](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti Önt az alkalmazásfrissítés PowerShell használatával.

Szabályozhatja, hogy az alkalmazás használatával frissíti [frissítése paraméterek](service-fabric-application-upgrade-parameters.md).

Az alkalmazásfrissítéseket Learning kompatibilissé hogyan toouse [Adatszerializálás](service-fabric-application-upgrade-data-serialization.md).

Javítsa ki a gyakori problémákat alkalmazásfrissítések toohello lépéseit hivatkozással [Alkalmazásfrissítések hibaelhárítási](service-fabric-application-upgrade-troubleshooting.md).
