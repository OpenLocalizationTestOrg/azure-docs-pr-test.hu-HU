---
title: "Azure Diagnostics használatával aaaCollect naplók |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan naplózza az Azure Diagnostics toocollect be tooset az Azure-ban futó Service Fabric-fürt."
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
ms.openlocfilehash: afbcfbe972b1847ef33bf0539b4398794b1bd56b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a>Naplók gyűjtése az Azure Diagnostics használatával
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
> * [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

Az Azure Service Fabric-fürtöt használ, esetén a jó ötlet toocollect hello jelentkezik minden hello csomópontról egy központi helyen. Hello naplók rendelkezik egy központi helyen segítségével elemezheti és a fürt, vagy az adott fürtben található futó hello alkalmazások és szolgáltatások a problémák elhárításához.

Egyirányú tooupload és a gyűjtés naplók toouse hello Azure Diagnostics bővítményt, mely feltöltések tooAzure tároló-, Azure Application Insights, és az Azure Event Hubs naplózza. hello naplók hasznosak nem, amely közvetlenül a tároló, vagy az Event Hubs. Azonban egy külső folyamat tooread hello események tárolóból, és helyezze el őket a termék például [Naplóelemzési](../log-analytics/log-analytics-service-fabric.md) vagy egy másik napló elemzési megoldás. [Az Azure Application Insights](https://azure.microsoft.com/services/application-insights/) egy átfogó naplózási keresési és analytics szolgáltatás beépített tartalmaz.

## <a name="prerequisites"></a>Előfeltételek
Ezen eszközök tooperform használja a jelen dokumentum hello műveleteket:

* [Az Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (tooAzure Felhőszolgáltatások azonban rendelkezik jó útmutatást és példákat kapcsolatos)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Az Azure Resource Manager-ügyfél](https://github.com/projectkudu/ARMClient)
* [Azure Resource Manager-sablon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-toocollect"></a>Napló adatforrások, hogy érdemes-e toocollect
* **A Service Fabric naplók**: hello platform toostandard esemény Windows (nyomkövetés) és az EventSource csatornák által kibocsátott. Naplók több típusok egyike lehet:
  * A működési események: hello platform hajtja végre a Service Fabric-műveletek naplókat. Például alkalmazások és a szolgáltatások, a csomópont állapotváltozások és a frissítési információkat létrehozását.
  * [Reliable Actors programozási modell események](service-fabric-reliable-actors-diagnostics.md)
  * [Megbízható Services-programozási modell események](service-fabric-reliable-services-diagnostics.md)
* **Alkalmazásesemények**: a szolgáltatás kódból kibocsátott, és a hello EventSource segítőosztály megadott hello Visual Studio sablonok használatával írt események. Hogyan toowrite naplózza az alkalmazásról további információkért lásd: [figyelő és diagnosztizálhatja a helyi számítógép fejlesztési telepítő szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-hello-diagnostics-extension"></a>Hello diagnosztika bővítmény telepítése
hello első lépése a naplók gyűjtésére toodeploy hello diagnosztika bővítmény minden egyes hello Service Fabric-fürt hello virtuális gépen. hello diagnosztika bővítmény naplókat gyűjt mindegyik virtuális gépre, és feltölti megadott toohello tárfiók. hogy használ-e hello Azure-portálon vagy az Azure Resource Manager kissé függően változhat a hello lépéseket. hello lépéseket is e hello telepítés része a fürt létrehozása, vagy egy már létező fürt függően változhat. Nézzük hello lépéseket az egyes forgatókönyvek esetében.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-hello-portal"></a>Hello portálon keresztül fürt létrehozásának részeként hello diagnosztika bővítmény telepítése
toodeploy hello diagnosztika bővítmény toohello virtuális gépek hello fürt fürt létrehozásának részeként, hello diagnosztikai beállítások panelen látható a következő kép hello használja. tooenable Reliable Actors vagy Reliable Services eseménygyűjtés, győződjön meg arról, hogy túl van-e állítva a diagnosztika**a** (hello az alapértelmezett beállítás). Hello fürt létrehozása után ezeket a beállításokat nem módosíthatja hello portál használatával.

![A fürt létrehozásához hello portálon az Azure diagnosztikai beállításai](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

az Azure támogatási csapata hello *szükséges* támogatási naplózza toohelp resolve bármely Ön által létrehozott támogatási kérelmek. Ezek a naplók valós időben gyűjti, és vannak tárolva egy hello storage-fiókok létrehozása hello erőforráscsoportban. hello diagnosztikai beállítások alkalmazásszintű események beállítása. Ezek az események tartalmazzák [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) események, [Reliable Services](service-fabric-reliable-services-diagnostics.md) eseményeket, és néhány rendszerszintű Service Fabric események toobe Azure Storage szolgáltatásban tárolja.

Például termékek [Elasticsearch](https://www.elastic.co/guide/index.html) vagy a saját folyamatának hello események lekérheti hello tárfiók. Jelenleg nincs módja toofilter vagy karcsúsítása hello küldött eseményeket toohello tábla. Ha nem valósítja meg a folyamat tooremove események hello táblából, hello tábla toogrow továbbra is.

A fürt létrehozásakor hello portál használatával erősen ajánlott, hogy töltse le a hello sablon **OK gombra kattintás előtt** toocreate hello fürt. Részletekért lásd a túl[Azure Resource Manager-sablonok használatával a Service Fabric-fürt beállított](service-fabric-cluster-creation-via-arm.md). Szüksége lesz hello sablon toomake módosítások később, mert néhány módosítást hello portál használatával nem hajtható végre.

Exportálhatja sablonok hello portálról hello lépések segítségével. Ezeket a sablonokat lehet azonban nehezebb toouse, mert előfordulhat, hogy hiányoznak a szükséges információk null értékeket.

1. Nyissa meg az erőforráscsoportot.
2. Válassza ki **beállítások** toodisplay hello-beállítások panelen.
3. Válassza ki **központi telepítések** toodisplay hello telepítési előzmények panelen.
4. Válassza ki a hello központi telepítés központi telepítési toodisplay hello részleteit.
5. Válassza ki **sablon exportálása** toodisplay hello sablon panel.
6. Válassza ki **toofile mentése** tooexport hello sablon, a paraméter és a PowerShell-fájlokat tartalmazó .zip fájl.

Hello fájlok exportálását követően kell toomake módosítását. Hello parameters.json fájl szerkesztése és eltávolítása hello **adminPassword** elemet. Ennek hatására a hello jelszó kérése, hello telepítési parancsfájl futtatásakor. Amikor hello telepítési parancsfájlt futtatja, lehetséges, hogy toofix null paraméterértékeket.

toouse hello letöltött sablon tooupdate egy konfigurációt:

1. Bontsa ki a hello tartalma tooa mappát a helyi számítógépen.
2. Módosítsa a hello tartalma tooreflect hello új konfigurációt.
3. Indítsa el a Powershellt, és módosítsa a toohello mappát, amelyikbe kibontotta hello tartalmat.
4. Futtatás **deploy.ps1** , és töltse ki hello előfizetés-azonosító, hello erőforráscsoport-név (használata hello azonos tooupdate hello konfigurációja), és egyedi telepítési nevét.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Fürt létrehozásának részeként hello diagnosztika bővítmény telepítése Azure Resource Manager használatával
egy fürt erőforrás-kezelő használatával toocreate, kell tooadd hello diagnosztika konfigurációs JSON toohello teljes fürt Resource Manager sablon hello fürt létrehozása előtt. Egy minta öt-Virtuálisgép-fürt Resource Manager sablon nyújtunk hozzá diagnosztikai konfiguráció tooit a Resource Manager sablon minták részeként. Ezen a helyen hello Azure-minták katalógusban látható: [öt csomópontból álló fürt diagnosztika Resource Manager sablon példával](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).

toosee hello diagnosztika beállítás hello Resource Manager-sablon, nyitott hello azuredeploy.json fájlt, és keresse meg a **IaaSDiagnostics**. a fürt ezt a sablont, jelölje be hello segítségével toocreate **tooAzure telepítése** gomb hello előző hivatkozás érhető el.

Azt is megteheti, hello erőforrás-kezelő minta letöltése, ellenőrizze a módosítások tooit és hello segítségével hozzon létre egy fürtöt hello módosított sablon `New-AzureRmResourceGroupDeployment` parancsot egy Azure PowerShell-ablakban. Tekintse meg a következő kód hello paraméter jelzi, hogy átadta toohello parancsban hello. Hogyan toodeploy erőforrás szerint kell csoportosítani a PowerShell használatával további információkért lásd: hello cikk [hello Azure Resource Manager sablonnal erőforráscsoport telepítése](../azure-resource-manager/resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

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

## <a name="update-diagnostics-toocollect-health-and-load-events"></a>Diagnosztika toocollect állapotát és a betöltési események frissítése

Hello 5.4-es verziójában a Service Fabric-től kezdődően állapotát és a terheléselosztási metrika események állnak rendelkezésre a gyűjtemény. Ezek az események tükrözze hello állapotfigyelő segítségével hello rendszer vagy a kód által előállított eseményeket vagy nem tölthető be, mint jelentéskészítési API-k [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) vagy [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Ez lehetővé teszi, hogy összesítésére és idővel állapotának megtekintése és a riasztás állapotát vagy a betöltési események alapján. Ezek az események a Visual Studio diagnosztikai eseménynapló hozzáadása tooview "Microsoft-ServiceFabric:4:0x4000000000000008" toohello ETW-hitelesítésszolgáltatók listáját.

toocollect hello események, hello resource manager sablon tooinclude módosítása

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

## <a name="update-diagnostics-toocollect-and-upload-logs-from-new-eventsource-channels"></a>Diagnosztika toocollect frissítése és a naplók feltöltése az új EventSource csatornák
tooupdate diagnosztika toocollect naplókat az új EventSource csatornák, amelyek megfelelnek egy új alkalmazást, amely körülbelül toodeploy, hajtsa végre a ugyanaz, mint hello lépések hello [előző szakaszban](#deploywadarm) ad egy meglévő diagnosztikai telepítéshez fürt.

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

## <a name="next-steps"></a>Következő lépések
részletesebben toounderstand milyen eseményeket kell keresnie az közben hibaelhárításával, lásd: a kibocsátott diagnosztikai eseményei hello [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) és [Reliable Services](service-fabric-reliable-services-diagnostics.md).

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Ismerje meg, hogyan toocollect teljesítményszámlálókkal és a naplók segítségével hello diagnosztika bővítmény](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [A Naplóelemzési Service Fabric-megoldás](../log-analytics/log-analytics-service-fabric.md)

