---
title: "Service Fabric esemény összesítési a Windows Azure diagnosztikai aaaAzure |} Microsoft Docs"
description: "További tudnivalók összesítésére és események gyűjtése ÜVEGVATTA figyelési és diagnosztika Azure Service Fabric-fürt segítségével."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 4827ce66620e61c5b4a8682db55952333113188a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Esemény összesítésére és az adatgyűjtést, a Windows Azure diagnosztikai
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Az Azure Service Fabric-fürtöt használ, esetén a jó ötlet toocollect hello jelentkezik minden hello csomópontról egy központi helyen. Hello naplók rendelkezik egy központi helyen segítségével elemezheti és a fürt, vagy az adott fürtben található futó hello alkalmazások és szolgáltatások a problémák elhárításához.

Egyirányú tooupload és a gyűjtés naplók toouse hello Windows Azure diagnosztikai (ÜVEGVATTA) bővítmény a naplók tooAzure tárolási feltölti, és hello beállítás toosend naplók tooAzure Application Insights vagy az Event Hubs is rendelkezik. Is használja egy külső folyamat tooread hello eseményeket tárolóból, és helyezze el őket egy analysis platform termék, például a [OMS Naplóelemzési](../log-analytics/log-analytics-service-fabric.md) vagy egy másik napló elemzési megoldás.

## <a name="prerequisites"></a>Előfeltételek
Ezek az eszközök a használt tooperform hello műveleteket ebben a dokumentumban:

* [Az Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (tooAzure Felhőszolgáltatások azonban rendelkezik jó útmutatást és példákat kapcsolatos)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Az Azure Resource Manager-ügyfél](https://github.com/projectkudu/ARMClient)
* [Azure Resource Manager-sablon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a>Naplófájl és esemény források

### <a name="service-fabric-platform-events"></a>A Service Fabric platform események
A bemutatott [Ez a cikk](service-fabric-diagnostics-event-generation-infra.md), a Service Fabric beállítja, néhány out-of-az-box naplózási csatornák, amely hello a következő csatornák könnyen konfigurált ÜVEGVATTA toosend figyelése és diagnosztikai adatok tooa tárolási tábla vagy máshol:
  * A működési események: hello platform hajtja végre a Service Fabric-magasabb szintű műveletek. Például alkalmazások és a szolgáltatások, a csomópont állapotváltozások és a frissítési információkat létrehozását. Ezek kibocsátott mint esemény Windows (nyomkövetés) naplók
  * [Reliable Actors programozási modell események](service-fabric-reliable-actors-diagnostics.md)
  * [Megbízható Services-programozási modell események](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a>Alkalmazás-események
 Hello Visual Studio sablonok találhatók az alkalmazások és szolgáltatások kódból kibocsátott, és a hello EventSource segítőosztály használatával írt események. Hogyan naplózza az toowrite EventSource az alkalmazásról további információkért lásd: [figyelő és diagnosztizálhatja a helyi számítógép fejlesztési telepítő szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-hello-diagnostics-extension"></a>Hello diagnosztika bővítmény telepítése
hello első lépése a naplók gyűjtésére toodeploy hello diagnosztika bővítmény minden egyes hello Service Fabric-fürt hello virtuális gépen. hello diagnosztika bővítmény naplókat gyűjt mindegyik virtuális gépre, és feltölti megadott toohello tárfiók. hogy használ-e hello Azure-portálon vagy az Azure Resource Manager kissé függően változhat a hello lépéseket. hello lépéseket is e hello telepítés része a fürt létrehozása, vagy egy már létező fürt függően változhat. Nézzük hello lépéseket az egyes forgatókönyvek esetében.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>A fürt létrehozása az Azure portálon keresztül részeként hello diagnosztika bővítmény telepítése
toodeploy hello diagnosztika bővítmény toohello virtuális gépek hello fürt fürt létrehozásának részeként, használhat hello diagnosztikai beállítások panel hello következő látható kép – győződjön meg arról, hogy túl van-e állítva a diagnosztika**a** (hello az alapértelmezett beállítás) . Hello fürt létrehozása után ezeket a beállításokat nem módosíthatja hello portál használatával.

![A fürt létrehozásához hello portálon az Azure diagnosztikai beállításai](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

A fürt létrehozásakor hello portál használatával erősen ajánlott, hogy töltse le a hello sablon **OK gombra kattintás előtt** toocreate hello fürt. Részletekért lásd a túl[Azure Resource Manager-sablonok használatával a Service Fabric-fürt beállított](service-fabric-cluster-creation-via-arm.md). Szüksége lesz hello sablon toomake módosítások később, mert néhány módosítást hello portál használatával nem hajtható végre.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Fürt létrehozásának részeként hello diagnosztika bővítmény telepítése Azure Resource Manager használatával
egy fürt erőforrás-kezelő használatával toocreate, kell tooadd hello diagnosztika konfigurációs JSON toohello teljes fürt Resource Manager sablon hello fürt létrehozása előtt. Egy minta öt-Virtuálisgép-fürt Resource Manager sablon nyújtunk hozzá diagnosztikai konfiguráció tooit a Resource Manager sablon minták részeként. Ezen a helyen hello Azure-minták katalógusban látható: [öt csomópontból álló fürt diagnosztika Resource Manager sablon példával](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

toosee hello diagnosztika beállítás hello Resource Manager-sablon, nyitott hello azuredeploy.json fájlt, és keresse meg a **IaaSDiagnostics**. a fürt ezt a sablont, jelölje be hello segítségével toocreate **tooAzure telepítése** gomb hello előző hivatkozás érhető el.

Azt is megteheti, hello erőforrás-kezelő minta letöltése, ellenőrizze a módosítások tooit és hello segítségével hozzon létre egy fürtöt hello módosított sablon `New-AzureRmResourceGroupDeployment` parancsot egy Azure PowerShell-ablakban. Tekintse meg a következő kód hello paraméter jelzi, hogy átadta toohello parancsban hello. Hogyan toodeploy erőforrás szerint kell csoportosítani a PowerShell használatával további információkért lásd: hello cikk [hello Azure Resource Manager sablonnal erőforráscsoport telepítése](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="deploy-hello-diagnostics-extension-tooan-existing-cluster"></a>Hello diagnosztika bővítmény tooan meglévő fürt központi telepítése
Ha egy meglévő fürtöt, amely nem rendelkezik telepített diagnosztika, vagy ha azt szeretné, hogy egy meglévő konfigurációt toomodify, adja hozzá, vagy frissítheti azt. Hello Resource Manager-sablon által használt toocreate hello meglévő fürt módosítása vagy hello sablon letöltése hello portálról korábban leírt módon. Módosítsa a hello template.json fájlt hello a következő feladatok végrehajtásával.

Vegyen fel egy új tároló-erőforrás toohello sablont toohello források szakaszában hozzáadásával.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Ezután adja hozzá a toohello paraméterek között szakasz után hello tárolási fiók definíciók `supportLogStorageAccountName` és `vmNodeType0Name`. Cserélje le a helyőrzőket hello *ide kerül a tárfiók neve* hello nevű hello tárfiók.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for hello application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for hello storage account that contains application diagnostics data from hello cluster"
      }
    },
```
Frissítse a hello `VirtualMachineProfile` hello template.json fájl a következő kód hello bővítmények tömbön belüli hello hozzáadásával szakaszában. Lehet, hogy tooadd hello elején vagy hello végén, attól függően, hogy hol csatlakoztatva van egy vesszővel.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Hello template.json fájl leírtak módosítása után közzé hello Resource Manager-sablon. Hello sablon exportált, ha a fájl futtatása hello deploy.ps1 addig hello sablont. Miután telepít, győződjön meg arról, hogy **ProvisioningState** van **sikeres**.

## <a name="collect-health-and-load-events"></a>Állapot gyűjtése és események betöltése

Hello 5.4-es verziójában a Service Fabric-től kezdődően állapotát és a terheléselosztási metrika események állnak rendelkezésre a gyűjtemény. Ezek az események tükrözze hello állapotfigyelő segítségével hello rendszer vagy a kód által előállított eseményeket vagy nem tölthető be, mint jelentéskészítési API-k [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) vagy [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Ez lehetővé teszi, hogy összesítésére és idővel állapotának megtekintése és a riasztás állapotát vagy a betöltési események alapján. Ezek az események a Visual Studio diagnosztikai eseménynapló hozzáadása tooview "Microsoft-ServiceFabric:4:0x4000000000000008" toohello ETW-hitelesítésszolgáltatók listáját.

toocollect hello események, hello Resource Manager sablon tooinclude módosítása

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387912",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

## <a name="collect-reverse-proxy-events"></a>Fordított proxy eseményeinek gyűjtése

A Service Fabric hello 5.7 kiadástól kezdve [fordított proxy](service-fabric-reverseproxy.md) események állnak rendelkezésre a következő gyűjtemény számára.
Fordított proxy az események két csatornákon, egy tartalmazó hiba bocsát ki tükröző események kérelmek feldolgozási hibák, és egy másikra hello fordított proxy feldolgozás összes hello kérelmek kapcsolatos részletes eseményeket tartalmazó hello. 

1. Hiba eseményeinek gyűjtése: ezek az események a Visual Studio diagnosztikai eseménynapló hozzáadása tooview "Microsoft-ServiceFabric:4:0x4000000000000010" toohello ETW-hitelesítésszolgáltatók listáját.
toocollect hello események Azure fürtök hello Resource Manager sablon tooinclude módosítása

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387920",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

2. Összegyűjteni az összes kérelem események feldolgozását: A Visual Studio diagnosztikai eseménynapló, frissítés a Microsoft-ServiceFabric hello bejegyzés túl hello ETW-szolgáltató listában "Microsoft-ServiceFabric:4:0x4000000000000020".
Az Azure Service Fabric-fürtök esetén hello resource manager sablon tooinclude módosítása

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387936",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```
> Ez hello fordított proxyn keresztül történő összes forgalom gyűjt, és gyorsan felhasználhat a tárolási kapacitás ajánlott toojudiciously engedélyezése események gyűjtését a csatornán.

Az Azure Service Fabric-fürtök esetén minden hello csomópontról hello események gyűjtése és hello SystemEventTable összesíteni.
Részletes hello fordított proxy események, tekintse meg a hello [fordított proxy diagnosztika útmutató](service-fabric-reverse-proxy-diagnostics.md).

## <a name="collect-from-new-eventsource-channels"></a>Új EventSource csatornák gyűjtése

tooupdate diagnosztika toocollect naplókat az új EventSource csatornák, amelyek megfelelnek egy új alkalmazást, amely körülbelül toodeploy, végezze el a hello korábban leírt lépéseket egy meglévő fürt diagnosztika hello beállítása.

Hello frissítése `EtwEventSourceProviderConfiguration` hello új EventSource csatornák hello konfiguráció alkalmazása előtt módosítsa hello segítségével hello template.json tooadd bejegyzések szakasz `New-AzureRmResourceGroupDeployment` PowerShell-parancsot. hello eseményforrás hello nevét a kód hello ServiceEventSource.cs Visual Studio által létrehozott fájl részeként van definiálva.

Ha a forrás saját Eventsource neve, például saját Eventsource kód tooplace hello események követően egy táblába MyDestinationTableName nevű hello hozzáadása.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

toocollect teljesítményszámlálók vagy eseménynaplók, hello Resource Manager sablon módosításához megadott hello példák felhasználásával [Windows virtuális gép létrehozása a figyelési és diagnosztikaAzureResourceManager-sablonhasználatával](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Újbóli hello Resource Manager-sablon.

## <a name="collect-performance-counters"></a>A teljesítményszámlálók adatainak összegyűjtése

a fürtről toocollect teljesítménymutatók hello teljesítmény számlálók tooyour "WadCfg > DiagnosticMonitorConfiguration" hello Resource Manager-sablon a fürt adja hozzá. Lásd: [Service Fabric teljesítményszámlálók](service-fabric-diagnostics-event-generation-perf.md) teljesítményszámlálóinak, javasoljuk a összegyűjtése.

Például itt hivatott egy teljesítményszámláló mintát 15 másodpercenként (ezt módosíthatja, és a következőképpen hello formátuma "PT\<idő >\<egység >", például a PT3M három perces időközönként mintát lenne), és toohello át tárolási tábla egy percenként.

  ```json
  "PerformanceCounters": {
      "scheduledTransferPeriod": "PT1M",
      "PerformanceCounterConfiguration": [
          {
              "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
              "sampleRate": "PT15S",
              "unit": "Percent",
              "annotation": [
              ],
              "sinks": ""
          }
      ]
  }
  ```
  
Ha Ön az Application Insights fogadó hello szakaszban leírtak szerint használja, és ezek metrikák tooshow fel az Application Insightsban, végezze el, hogy tooadd hello fogadó neve hello "mosdók" szakaszban, ahogy fent látható. Emellett érdemes lehet létrehozni egy külön táblázatban toosend a teljesítményszámlálókat, így azok utasok nagyobb nem csoportjának kimenő hello adatforrásból származó adatok hello más engedélyezte a naplózási csatornák.


## <a name="send-logs-tooapplication-insights"></a>Küldési tooApplication Insights naplói

Figyelés és diagnosztikai adatok tooApplication Insights (AI) küldése hello ÜVEGVATTA konfiguráció részeként végezhető. Ha úgy dönt, hogy az esemény elemzése és a képi megjelenítés AI toouse, olvassa el a [esemény elemzése és az Application insights szolgáltatással a képi megjelenítés](service-fabric-diagnostics-event-analysis-appinsights.md) tooset fel egy AI fogadó a "WadCfg" részeként.

## <a name="next-steps"></a>Következő lépések

Miután konfigurálta az Azure diagnostics megfelelően, adatait jeleníti meg a tárolási táblákba hello ETW és EventSource naplókat. Ha toouse OMS, Kibana, vagy bármely más elemzés és a képi megjelenítés adatplatform, amely közvetlenül nincs beállítva a Resource Manager-sablon hello, győződjön meg arról, hogy tooset be a választott tooread hello platformja a következő tárolási táblákból hello adatok. Az OMS nagyon viszonylag egyszerű, és az ismertetése [esemény és a naplófájlok elemzése OMS keresztül](service-fabric-diagnostics-event-analysis-oms.md). Az Application Insights egy bit egy különleges esetben abban az értelemben, hello diagnosztika bővítmény konfigurációjának részeként konfigurálhatók, mivel így ügyet toohello [megfelelő cikk](service-fabric-diagnostics-event-analysis-appinsights.md) Ha úgy dönt, hogy toouse AI.

>[!NOTE]
>Jelenleg nincs módja toofilter vagy karcsúsítása hello küldött eseményeket toohello tábla. Ha nem valósítja meg a folyamat tooremove események hello táblából, hello tábla toogrow továbbra is. Jelenleg egy futó hello karcsúsítási szolgáltatás például [figyelő minta](https://github.com/Azure-Samples/service-fabric-watchdog-service), javasoljuk, hogy lehet írni egy a saját kezűleg is, kivéve ha egy jó 30 vagy 90 nap időkereteket toostore naplókat, és.

* [Ismerje meg, hogyan toocollect teljesítményszámlálókkal és a naplók segítségével hello diagnosztika bővítmény](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Esemény elemzése és a képi megjelenítés az Application insights szolgáltatással](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Esemény elemzése és az OMS képi megjelenítés](service-fabric-diagnostics-event-analysis-oms.md)