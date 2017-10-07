---
title: "a Service Fabric-alkalmazás aaaConfigure hello frissítése |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure hello Microsoft Visual Studio használatával a Service Fabric-alkalmazás frissítésére vonatkozó beállításokat."
services: service-fabric
documentationcenter: na
author: mikkelhegn
manager: mfussell
editor: tglee
ms.assetid: 1757ba85-0b7b-4f16-8a23-2ddaa61c86c6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/29/2017
ms.author: mikkelhegn
ms.openlocfilehash: 8ca50aa9d911f3c98f017490c8fe29011e8d80cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-upgrade-of-a-service-fabric-application-in-visual-studio"></a>A Service Fabric-alkalmazás hello frissítését a Visual Studio konfigurálása
A Visual Studio eszközök Azure Service Fabric frissítési támogatást nyújt a toolocal vagy távoli fürtök. Nincsenek használni kívánt tooupgrade az alkalmazás tooa újabb verziójával hello alkalmazás tesztelés és hibakeresés során felülírása helyett három forgatókönyv:

* Alkalmazásadatok nem fognak veszni hello frissítés során.
* Rendelkezésre állási marad magas bármely szolgáltatáskiesést hello a frissítés során nem lesz, ha nincsenek a frissítési tartományok elosztva elég szolgáltatáspéldány.
* Tesztek futtatható az alkalmazáshoz, amíg a frissítés alatt áll.

## <a name="parameters-needed-tooupgrade"></a>Paraméterek szükséges tooupgrade
Kétféle típusú központi telepítés közül választhat: rendszeres vagy frissítés. A szokásos telepítési töröl bármely korábbi központi telepítési információk és adatok hello fürtön során egy frissítés telepítése megőrzi azt. A Service Fabric-alkalmazás, a Visual Studio frissít, meg kell tooprovide alkalmazás frissítési paraméterek és az állapotházirendeket ellenőrzése. Az alkalmazásfrissítés paraméterek segítségével hello frissítését, szabályozhatja, amíg állapotházirendeket jelölőnégyzet határozza meg, hogy hello frissítése sikeres volt-e. Lásd: [Service Fabric az alkalmazásfrissítés: frissítési paraméterek](service-fabric-application-upgrade-parameters.md) további részleteket.

Három frissítési módot: *figyelt*, *UnmonitoredAuto*, és *UnmonitoredManual*.

* A figyelt frissítés hello frissítés és alkalmazáshoz automatizálja az állapot-ellenőrzéssel.
* UnmonitoredAuto frissítés automatizálja hello frissítését, de átugrása hello alkalmazás állapot-ellenőrzése.
* Ekkor-UnmonitoredManual frissítés toomanually kell frissíteni mindegyik frissítési tartományon.

Minden egyes frissítési módhoz paraméterek más-más részhalmazához. Lásd: [alkalmazás frissítési paraméterei](service-fabric-application-upgrade-parameters.md) hello érhető el frissítési beállításokkal kapcsolatos további toolearn.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>A Service Fabric-alkalmazás, a Visual Studio frissítése
Hello Visual Studio Service Fabric eszközök tooupgrade a Service Fabric-alkalmazás használata, megadhatja a közzétételi folyamat toobe frissítés helyett a szokásos telepítési hello ellenőrzésével **hello alkalmazás frissítése** ellenőrzése mezőbe.

### <a name="tooconfigure-hello-upgrade-parameters"></a>tooconfigure hello frissítési paraméterek
1. Kattintson a hello **beállítások** gomb következő toohello jelölőnégyzetet. Hello **frissítése paraméterek szerkesztése** párbeszédpanel jelenik meg. Hello **frissítése paraméterek szerkesztése** párbeszédpanel hello figyelt UnmonitoredAuto és UnmonitoredManual frissítési módot támogat.
2. Válassza ki a frissítési mód hello toouse szeretne, és ezután adja meg a hello paraméter rács.

    Minden paraméter alapértelmezett értéke van. nem kötelező paraméter hello *DefaultServiceTypeHealthPolicy* egy kivonatoló tábla bemenetből fogad adatokat. Példa hello kivonatoló tábla bemeneti formátumának *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *Servicetypehealthpolicymap paraméterek hiányzó értékei* van egy másik nem kötelező paraméter, amely egy kivonatoló tábla bemenetből fogad adatokat hello a következő formátumban:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Íme egy valós példa:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. Ha UnmonitoredManual frissítési módot választja, akkor manuálisan indítsa el a PowerShell-konzol toocontinue és hello frissítési folyamat befejezéséhez. Tekintse meg a túl[Service Fabric az alkalmazásfrissítés: témakörök speciális](service-fabric-application-upgrade-advanced.md) toolearn hogyan kézi frissítés működik.

## <a name="upgrade-an-application-by-using-powershell"></a>Alkalmazások frissítése a PowerShell használatával
Használhatja a PowerShell-parancsmagok tooupgrade a Service Fabric-alkalmazás. Lásd: [Service Fabric-alkalmazás frissítési oktatóanyag](service-fabric-application-upgrade-tutorial.md) és [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) részletes információkat.

## <a name="specify-a-health-check-policy-in-hello-application-manifest-file"></a>Adja meg a jelölőnégyzet állapotházirend hello Alkalmazásjegyzék-fájl
Minden szolgáltatás a Service Fabric-alkalmazás rendelkezhet saját hello az alapértelmezett érték felülírására állapotfigyelő házirend paramétereket. A paraméterértékek hello alkalmazás-jegyzékfájl biztosíthat.

hello következő példa bemutatja, hogyan tooapply egyedi állapotát ellenőrzi az egyes szolgáltatásokhoz hello alkalmazásjegyzékben házirend.

```xml
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Következő lépések
Egy alkalmazás központi telepítésével kapcsolatos további információkért lásd: [telepítsen egy meglévő alkalmazást az Azure Service Fabric](service-fabric-deploy-existing-app.md).