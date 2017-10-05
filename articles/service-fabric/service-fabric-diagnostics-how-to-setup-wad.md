---
title: "Azure Diagnostics használatával naplógyűjtéshez |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan állíthat be Azure Diagnostics egy Azure-ban futó Service Fabric-fürt naplóinak gyűjtése."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 9f7e1fa5-6543-4efd-b53f-39510f18df56
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 190a8a393f2e7d74a792db4efa81f94a18b02221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a>Naplók gyűjtése az Azure Diagnostics használatával
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
> * [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

Amikor egy Azure Service Fabric-fürtöt használ, célszerű gyűjteni a egy központi helyen található minden csomópontot. A naplók rendelkezik egy központi helyen segítségével elemezheti és a fürt, vagy a futó alkalmazások és szolgáltatások az adott fürtben található a problémák elhárításához.

Egy feltöltése és naplógyűjtéshez módja az Azure Diagnostics bővítménnyel Azure Storage, Azure Application Insights vagy az Azure Event Hubs naplók feltöltését. A napló az értékek nem, amely közvetlenül a tároló, vagy az Event Hubs. De használhat külső folyamat kiolvassa az eseményeket a tárolóból, és helyezze el őket a termék például [Naplóelemzési](../log-analytics/log-analytics-service-fabric.md) vagy egy másik napló elemzési megoldás. [Az Azure Application Insights](https://azure.microsoft.com/services/application-insights/) egy átfogó naplózási keresési és analytics szolgáltatás beépített tartalmaz.

## <a name="prerequisites"></a>Előfeltételek
Az eszközök segítségével hajtsa végre műveleteket ebben a dokumentumban:

* [Az Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (Azure Felhőszolgáltatások azonban az helyes útmutatást és példákat tartalmaz)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Az Azure Resource Manager-ügyfél](https://github.com/projectkudu/ARMClient)
* [Azure Resource Manager-sablon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-to-collect"></a>Előfordulhat, hogy szeretne gyűjteni napló források
* **A Service Fabric naplók**: kibocsátott platformnak szabványos esemény-nyomkövetés Windows (ETW) és az EventSource csatorna. Naplók több típusok egyike lehet:
  * A működési események: műveletek a Service Fabric-platformról végző naplókat. Például alkalmazások és a szolgáltatások, a csomópont állapotváltozások és a frissítési információkat létrehozását.
  * [Reliable Actors programozási modell események](service-fabric-reliable-actors-diagnostics.md)
  * [Megbízható Services-programozási modell események](service-fabric-reliable-services-diagnostics.md)
* **Alkalmazásesemények**: a szolgáltatás kódból kibocsátott, és a Visual Studio sablonok találhatók az EventSource segítőosztály írhatók eseményeket. A naplók írásával az alkalmazásról további információkért lásd: [figyelő és diagnosztizálhatja a helyi számítógép fejlesztési telepítő szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-the-diagnostics-extension"></a>A diagnosztika bővítmény telepítése
Az első lépés a naplók gyűjtésére központi telepítése a diagnosztika bővítmény minden egyes a Service Fabric-fürt a virtuális gépen. A diagnosztika bővítmény naplókat gyűjt mindegyik virtuális gépre, és feltölti a megadott tárfiók. A lépések kissé függően hogy használ-e az Azure portálon vagy az Azure Resource Manager változhat. A lépéseket is függően változnak, hogy a központi telepítés része a fürt létrehozása, vagy egy már létező fürt. Vizsgáljuk meg az egyes forgatókönyv lépéseit.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>A fürt létrehozása a portálon keresztül részeként diagnosztika bővítmény telepítése
A diagnosztika bővítményt telepíteni a virtuális gépeket a fürt fürt létrehozásának részeként, használhatja a diagnosztikai beállítások panelen a következő ábrán látható. Engedélyezi a Reliable Actors vagy Reliable Services eseménygyűjtés, győződjön meg arról, hogy diagnosztikai beállításai **a** (az alapértelmezett beállítás). A fürt létrehozása után ezeket a beállításokat nem módosíthatja a portál használatával.

![A fürt létrehozása a portálon az Azure diagnosztikai beállításai](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Az Azure támogatási csapata *szükséges* naplók segíthetnek megoldani az összes Ön által létrehozott támogatási kérelmek támogatásához. Ezek a naplók gyűjtött valós időben, és a storage-fiókok létrehozása az erőforráscsoportban valamelyikében. A diagnosztikai beállítások alkalmazásszintű események beállítása. Ezek az események tartalmazzák [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) események, [Reliable Services](service-fabric-reliable-services-diagnostics.md) eseményeket, és egyes rendszerszintű Service Fabric események Azure Storage tárolja.

Például termékek [Elasticsearch](https://www.elastic.co/guide/index.html) vagy a saját folyamatának az események lekérheti a tárfiók. Jelenleg nincs mód való vagy készítsen fel a figyelmet, hogy a tábla küldött. Egy folyamat leállását események eltávolítja a tábla nem valósítja meg, ha a tábla egyre nő.

A fürt létrehozásakor a portál használatával erősen ajánlott, hogy töltse le a sablon **OK gombra kattintás előtt** a fürt létrehozásához. További információkért tekintse meg [Azure Resource Manager-sablonok használatával a Service Fabric-fürt beállított](service-fabric-cluster-creation-via-arm.md). A módosítások később, a sablon lesz szüksége, mert nem módosítja a portál használatával.

Sablonok a következő lépések segítségével exportálhatja a portálról. Azonban ezeket a sablonokat is több használata is nehézkes lehet miatt előfordulhat, hogy hiányoznak a szükséges információk null értékeket.

1. Nyissa meg az erőforráscsoportot.
2. Válassza ki **beállítások** a beállítások panel megjelenítéséhez.
3. Válassza ki **központi telepítések** a központi telepítés előzményei panel megjelenítéséhez.
4. Jelölje ki a telepítést, a központi telepítés részleteinek megjelenítéséhez.
5. Válassza ki **sablon exportálása** a sablon panel megjelenítéséhez.
6. Válassza ki **fájlba Mentés** a sablonban, a paraméter és a PowerShell-fájlokat tartalmazó .zip fájl exportálása.

Miután exportálta a fájlokat, a módosítások kell. A parameters.json fájl szerkesztése és eltávolítása a **adminPassword** elemet. Ennek következtében a a jelszó kérése a telepítési parancsfájl futtatásakor. Jelenik meg a telepítési parancsfájlt, akkor előfordulhat, hogy javítsa ki a null paraméterértékeket.

A letöltött sablonok használata a konfiguráció frissítése:

1. Bontsa ki a tartalmát a helyi számítógép egyik mappájába.
2. A tartalom tükrözzék az új konfiguráció módosítása.
3. Indítsa el a Powershellt, és módosítsa a mappát, amelyikbe kibontotta a tartalmat.
4. Futtatás **deploy.ps1** , és töltse ki az előfizetés-azonosító, az erőforráscsoport neve (használja ugyanazt a konfiguráció frissítése) és egyedi telepítési nevét.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>A fürt létrehozásának részeként diagnosztika bővítmény telepítése Azure Resource Manager használatával
Fürt létrehozásához erőforrás-kezelő használatával kell hozzáadnia a diagnosztika konfigurációs JSON a teljes fürt Resource Manager-sablon, a fürt létrehozása előtt. Egy minta öt-Virtuálisgép-fürt Resource Manager sablon nyújtunk a Resource Manager sablon minták részeként hozzáadott diagnosztika konfigurációjával. Ezen a helyen az Azure-minták oldalon láthatja: [öt csomópontból álló fürt diagnosztika Resource Manager sablon példával](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).

A diagnosztika beállítást a Resource Manager-sablon megtekintéséhez nyissa meg az azuredeploy.json fájlt, és keresse meg **IaaSDiagnostics**. A fürt létrehozásához a sablon használatával, jelölje ki a **az Azure telepítéséhez** gomb érhető el az előző kapcsolat.

Azt is megteheti, töltse le az erőforrás-kezelő minta, módosítani és hozzon létre egy fürtöt a módosított sablon használatával a `New-AzureRmResourceGroupDeployment` parancsot egy Azure PowerShell-ablakban. Tekintse meg a következő kódot a paraméterek, amelyeket átad a a parancsot. Az erőforráscsoport PowerShell használatával történő központi telepítéséről részletes információkért lásd: a cikk [központi telepítése egy erőforráscsoportot az Azure Resource Manager sablon](../azure-resource-manager/resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>A diagnosztika bővítmény telepítése egy meglévő fürthöz
Ha egy meglévő fürtöt, amely nem rendelkezik telepített diagnosztika, vagy ha azt szeretné, hogy egy meglévő konfigurációjának módosítása, adja hozzá, vagy frissítheti azt. Módosítsa a Resource Manager-sablon, amellyel a meglévő fürt létrehozása, vagy a sablon letöltéséről a portálon, a fentebb leírt módon. A template.json fájl módosítása a következő feladatok végrehajtásával.

A sablon a források szakaszában ad hozzá egy új tárolási erőforrás hozzáadása.

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

 A következő után vesz fel a Paraméterek szakaszban csak a tárolási fiók definíciók között `supportLogStorageAccountName` és `vmNodeType0Name`. Cserélje le a helyőrzőket *ide kerül a tárfiók neve* a tárfiók nevével.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Ezt követően frissítse a `VirtualMachineProfile` a template.json fájl a következő kódrészletet a bővítmények tömbön belüli szakaszában. Ne feledje hozzáadni vesszővel elején vagy végén, attól függően, hogy hol csatlakoztatva van-e.

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

Lásd a template.json fájl módosítása, után közzé a Resource Manager-sablon. Ha a sablon exportált, a sablon a deploy.ps1 fájlt futtatja addig. Miután telepít, győződjön meg arról, hogy **ProvisioningState** van **sikeres**.

## <a name="update-diagnostics-to-collect-health-and-load-events"></a>Frissítse a diagnosztikai állapot gyűjtése és események betöltése

A Service Fabric 5.4 kiadástól kezdve, állapotát és a terheléselosztási metrika események állnak rendelkezésre a gyűjteményhez. Ezek az események tükrözze az állapotfigyelő segítségével a rendszer vagy a kód által előállított eseményeket vagy nem tölthető be, mint jelentéskészítési API-k [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) vagy [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Ez lehetővé teszi, hogy összesítésére és idővel állapotának megtekintése és a riasztás állapotát vagy a betöltési események alapján. Ezeket az eseményeket a Visual Studio diagnosztikai eseménynapló adja hozzá megtekintése "Microsoft-ServiceFabric:4:0x4000000000000008" ETW-szolgáltatók listáját.

Az események összegyűjtésére, tartalmazza a resource manager-sablon módosítása

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

## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Diagnosztika összegyűjtésére és a naplók feltöltése az új EventSource csatornák frissítése
Frissíteni a diagnosztikai naplók összegyűjtésére új EventSource csatornák, amelyek megfelelnek egy új alkalmazást, amely a kívánt központi telepítése, hajtsa végre a lépéseket, mint a a [előző szakaszban](#deploywadarm) diagnosztika egy meglévő fürt beállításához.

Frissítse a `EtwEventSourceProviderConfiguration` az template.json fájl bejegyzés hozzáadása előtt, a konfiguráció új EventSource csatornák használatával módosítsa a `New-AzureRmResourceGroupDeployment` PowerShell-parancsot. A forrás nevét a kódot, a Visual Studio által létrehozott ServiceEventSource.cs fájl részeként van definiálva.

Ha a forrás saját Eventsource neve, adja hozzá például a következő kódot a saját Eventsource eseményei helyezzen MyDestinationTableName nevű tábla.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Gyűjti az teljesítményszámlálók és az eseménynaplók, módosítsa a Resource Manager-sablon a megadott példák felhasználásával [figyelése és diagnosztika Windows virtuális gép létrehozása Azure Resource Manager-sablon használatával](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). A Resource Manager-sablon, majd közzé.

## <a name="next-steps"></a>Következő lépések
Milyen eseményeket kell keresnie az problémák elhárítása során részletes ismertetése: az a kibocsátott diagnosztikai eseményei [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) és [Reliable Services](service-fabric-reliable-services-diagnostics.md).

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Megtudhatja, hogyan gyűjti a teljesítményszámlálók és a naplókat a diagnosztika bővítmény segítségével](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [A Naplóelemzési Service Fabric-megoldás](../log-analytics/log-analytics-service-fabric.md)

