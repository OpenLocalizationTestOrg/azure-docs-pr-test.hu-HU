---
title: "aaaAdd figyelési & diagnosztikai tooan Azure virtuális gép |} Microsoft Docs"
description: "Egy Azure resource manager sablon toocreate egy új Windows rendszerű virtuális gép használja az Azure diagnostics-bővítménnyel."
services: virtual-machines-windows
documentationcenter: 
author: sbtron
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8cde8fe7-977b-43d2-be74-ad46dc946058
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: saurabh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d8a831421a0f9d38c09d51cf8c2e6dff913c77ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-monitoring-and-diagnostics-with-a-windows-vm-and-azure-resource-manager-templates"></a>Figyelés és diagnosztika használata egy Windows virtuális gép és az Azure Resource Manager sablonok
hello Azure Diagnostics bővítmény hello figyelést biztosít, és diagnosztikai képességek a Windows Azure virtuális gép alapján. Hello hello azure resource manager-sablon része belefoglalja engedélyezheti ezeket a képességeket hello virtuális gépen. Lásd: [Azure Resource Manager sablonok készítése a Virtuálisgép-bővítmények](template-description.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#extensions) további információ a virtuálisgép-sablon részeként bármely kiterjesztéssel együtt. Ez a cikk azt ismerteti, hogyan adhat meg hello Azure Diagnostics bővítmény tooa windows virtuális gépre vonatkozó sablont.  

## <a name="add-hello-azure-diagnostics-extension-toohello-vm-resource-definition"></a>Hello Azure Diagnostics bővítmény toohello VM erőforrás-definíció hozzáadása
tooenable hello diagnosztika kiterjesztése a Windows virtuális gépként kell tooadd hello bővítmény VM erőforrásként a Resource manager-sablon hello.

Az egyszerű erőforrás-kezelő alapú virtuális gép hozzáadása hello bővítmény konfigurációs toohello *erőforrások* tömbhöz a virtuális gép hello: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Egy másik közös egyezmény van hozzáadása hello bővítménykonfiguráció erőforrások gyökércsomópont hello hello sablon helyett definiáló hello virtuális gép erőforrások csomópont alatt. Ezt a módszert használja az informatikai részleg a hierarchikus kapcsolat hello bővítmény és hello virtuális gép között meg hello tooexplicitly *neve* és *típus* értékeket. Példa: 

    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

hello bővítmény mindig hello virtuális géphez kapcsolódó, lehetősége van közvetlenül közvetlenül meghatározása hello virtuális gép erőforrás csomópont alatt vagy adjon meg egy hello talál szinten, majd hello hierarchikus elnevezési egyezmény tooassociate használja a virtuális hello gép.

Virtuálisgép-méretezési csoportok hello bővítmények konfiguráció van megadva a hello *extensionProfile* hello tulajdonságának *VirtualMachineProfile*.

Hello *publisher* hello értékű tulajdonság **Microsoft.Azure.Diagnostics** és hello *típus* hello értékű tulajdonság **IaaSDiagnostics** hello Azure Diagnostics bővítmény egyedi módon azonosítja.

hello értékének hello *neve* tulajdonság lehet használt toorefer toohello bővítmény hello erőforráscsoportban. Beállítás kifejezetten túl**Microsoft.Insights.VMDiagnosticsSettings** engedélyezi azt toobe hello Azure portálon győződjön meg arról, amely figyelési diagramok belül megjelennek a megfelelő hello Azure-portálon hello alapján.

Hello *typeHandlerVersion* megadja hello verzióját hello bővítmény toouse szeretné. Beállítás *autoUpgradeMinorVersion* alverziót túl**igaz** biztosítja, hogy elérhetővé válik a hello legújabb alverzió hello bővítmény érhető el. Javasoljuk, hogy mindig állítsa *autoUpgradeMinorVersion* tooalways kell **igaz** úgy, hogy be mindig toouse hello legújabb érhető el diagnosztika bővítmény összes hello új szolgáltatásokkal és hiba javítások. 

Hello *beállítások* elem hello bővítményt, amely beállítása és olvasni hello bővítmény (néha hivatkozott tooas nyilvános konfigurációs) konfigurációk tulajdonságait tartalmazza. Hello *xmlcfg* tulajdonsága tartalmazza XML-alapú konfigurációs hello diagnosztika naplókhoz, teljesítményszámlálók hello diagnosztikai ügynök által gyűjtendő stb. Lásd: [diagnosztika konfigurációs séma](https://msdn.microsoft.com/library/azure/dn782207.aspx) hello XML-séma maga további információt. A gyakori eljárás megegyezik toostore hello tényleges XML-konfiguráció egy változó hello Azure Resource Manager-sablon és majd ÖSSZEFŰZ és base64 kódolása őket tooset hello értéke *xmlcfg*. Hello című rész a [diagnosztika konfigurációs változók](#diagnostics-configuration-variables) toounderstand hogyan toostore hello változók xml többet. Hello *storageAccount* tulajdonság határozza meg a hello tárolási fiók toowhich diagnosztikai adatok hello neve lesz áthelyezve. 

a Tulajdonságok hello *protectedSettings* (néha hivatkozott tooas privát konfigurációjának) állítható be, de nem lehet olvasni a beállítása után vissza. csak írható jellege hello *protectedSettings* teszi például hello tárfiók kulcsa titkos kulcsok tárolásának ahová hello diagnosztikai adatok kerülnek.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Diagnosztikai tárfiók paraméter megadása
hello diagnosztika bővítmény json részlet fenti azt feltételezi, hogy a két paraméter *existingdiagnosticsStorageAccountName* és *existingdiagnosticsStorageResourceGroup* toospecify hello diagnosztika diagnosztikai adatok tárolására szolgáló tárfiókot. Adja meg a hello diagnosztikai tárfiók, egy paraméter segítségével könnyen toochange hello diagnosztikai tárfiók különböző környezetek között például érdemes lehet egy másik diagnosztikai tárfiók toouse a teszteléshez és egy másikat pedig az a éles környezet.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "hello name of an existing storage account toowhich diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "hello resource group for hello storage account specified in existingdiagnosticsStorageAccountName"
              }
        }

Bevált gyakorlat toospecify egy diagnosztikai tárfiók egy másik erőforráscsoportban található, mint hello erőforráscsoport hello virtuális gép. Erőforráscsoport tekinthető toobe egy telepítési egység saját élettartamú, a virtuális gép telepítése és újratelepítése az új konfigurációt frissítések azt válnak, tooit, de azt szeretné, hogy hello diagnosztikai adatok tárolását hello toocontinue ugyanazt a tárfiókot különböző ezen virtuális gépek telepítéséhez. Egy másik erőforráscsoportban lehetővé teszi, hogy hello tárolási tooaccept származó adatokat különböző virtuális gépek telepítéséhez, így könnyen tootroubleshoot hello tárfiók rendelkező problémák között hello különböző verzióit.

> [!NOTE]
> Ha egy windows virtuális gépre vonatkozó sablont hoz létre a Visual Studio eszközből hello alapértelmezett tárfiókot is be lehet állítani a toouse hello ugyanazt a tárfiókot, ahol hello virtuális gép virtuális Merevlemeze feltöltődött. Ez a virtuális gép hello toosimplify kezdeti telepítése. Újra kell figyelembe hello sablon toouse során eltérő tárfiók is lehet az átadott paraméterként. 
> 
> 

## <a name="diagnostics-configuration-variables"></a>Diagnosztikai konfiguráció változók
hello diagnosztika bővítmény json részlet fenti definiál egy *accountid* változó toosimplify első hello tárfiók kulcsa hello diagnosztika tárolására:   

    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


Hello *xmlcfg* hello diagnosztika bővítmény tulajdonság van definiálva, amelyek együtt halmaz zónanevének több változók használata. hello ezek a változók értékei az XML-ben, escape-karaktersorozatot megfelelően hello json változók beállításakor toobe van szükségük.

hello következő szakasz ismerteti a hello diagnosztika konfigurációs XML-t, a standard szint rendszerteljesítmény-számlálók együtt egyes windows-Eseménynapló és a diagnosztika infrastruktúra naplókat gyűjt. Lett escape-karakterrel megjelölve, és így hello konfigurációs közvetlenül beilleszthető hello változók szakaszban a sablon megfelelő formátumú. Lásd: hello [diagnosztika konfigurációs séma](https://msdn.microsoft.com/library/azure/dn782207.aspx) hello konfigurációs XML-t több emberi olvasható példát.

        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

hello metrikák definition XML-csomópont a fenti konfigurációs hello fontos konfigurációs elem, mint azt határozza meg, hogyan hello teljesítményszámlálókat meghatározott hello XML-t a korábbi *PerformanceCounter* csomópont összegzik és tárolja. 

> [!IMPORTANT]
> A metrikák meghajtó hello figyelési diagramok és a riasztásokat a hello Azure-portálon.  Hello **metrikák** hello csomópont *resourceID* és **MetricAggregation** hello diagnosztika a virtuális gép konfigurációja a kell szerepelnie, ha azt szeretné, hogy a virtuális gép toosee hello figyelési adatok hello Azure-portálon. 
> 
> 

hello következő metrikák definíciók hello xml példája: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Hello *resourceID* attribútum egyedileg azonosítja a hello virtuális gépet az előfizetésben. Győződjön meg arról, hogy toouse hello subscription() és resourceGroup() működik, hogy hello sablon automatikusan frissíti hello előfizetés és az erőforrás-csoport alapján telepíti ezeket az értékeket.

Ha ismétlődő több virtuális gépet hoz létre, akkor meg kell toopopulate hello *resourceID* értéket egy copyIndex() függvény toocorrectly különböztetni az egyes virtuális gépek. Hello *xmlCfg* érték lehet frissített toosupport a következőképpen:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

hello MetricAggregation értékének *PT1H* és *PT1M* összesítést egy perc alatt, és akár egy óráig összesítést jelölésére.

## <a name="wadmetrics-tables-in-storage"></a>A tárolási WADMetrics táblák
a fenti hello metrikák konfigurációs az alábbi elnevezési konvenciókat hello eredményez a diagnosztikai tárfiók táblák:

* **WADMetrics** : szabványos előtag WADMetrics táblák
* **PT1H** vagy **PT1M** : azt jelzi, hogy hello táblát tartalmazza az összesített adatok több mint 1 óra vagy 1 perc
* **P10D** : azt jelzi, hogy hello tábla fog adatokat tartalmazni bekapcsolásakor a adatgyűjtés hello tábla számított 10 napig
* **V2S** : karakterlánc-konstansra
* **ÉÉÉÉHHNN** : hello dátum mely hello tábla kezdésének adatok gyűjtése

Példa: *WADMetricsPT1HP10DV2S20151108* metrikai adatok indítása a 11-november-2015-10 napja egy óra alatt összesített értéket fogja tartalmazni.    

Minden egyes WADMetrics tábla a következő oszlopok hello fogja tartalmazni:

* **PartitionKey**: hello partitionkey összeállított hello alapján *resourceID* érték toouniquely hello VM erőforrás azonosítására. a pl.: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
* **RowKey** : hello formátumot követi <Descending time tick>:<Performance Counter Name>. idő osztásjelek számítása csökkenő hello maximális idő ticks mínusz hello hello összesítési időszak kezdő időpontja hello. Például Ha a 10-november-2015 elindult hello mintavételi időszak 00:00Hrs UTC majd hello számítási lenne: DateTime.MaxValue.Ticks - (új DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Ticks). A hello memória rendelkezésre álló bájtok teljesítmény számláló hello sorkulcsa jelenik meg, például: 2519551871999999999__:005CMemory:005CAvailable:0020 bájt
* **CounterName** : hello teljesítményszámláló hello neve. Ez megegyezik hello *counterSpecifier* hello XML-konfiguráció sémaellenőrzése definiálva.
* **Maximális** : hello teljesítményszámláló maximális értékének hello hello összesítési időszakban.
* **Minimális** : hello teljesítményszámláló minimális értékének hello hello összesítési időszakban.
* **Teljes** : hello teljesítményszámláló az összes értékek összegét hello jelentett hello összesítési időszakban.
* **Count** : hello teljesítményszámláló jelentett értékek hello teljes száma.
* **Átlagos** : hello teljesítményszámláló átlagos (összesen és száma) értékének hello hello összesítési időszakban.

## <a name="next-steps"></a>Következő lépések
* További diagnosztikai kiterjesztésű Windows virtuális gép teljes mintasablon: [201-vm-figyelési-diagnosztika-bővítmény](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
* Szabályzatsablonokat hello resource manager sablon [Azure PowerShell](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [Azure parancssori felülettel](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* További információ [Azure Resource Manager sablonok készítése](../../resource-group-authoring-templates.md)

