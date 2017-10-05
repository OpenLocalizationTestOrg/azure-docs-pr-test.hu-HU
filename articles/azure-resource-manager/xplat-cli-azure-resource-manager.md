---
title: "Kezelheti az erőforrásokat az Azure parancssori felülettel |} Microsoft Docs"
description: "Azure-erőforrások és csoportok kezelése az Azure parancssori felület (CLI) használatával"
editor: 
manager: timlt
documentationcenter: 
author: tfitzmac
services: azure-resource-manager
ms.assetid: bb0af466-4f65-4559-ac3a-43985fa096ff
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: tomfitz
ms.openlocfilehash: 3ad4e68b90979fd7f9d3ddf5278e65e19cb07152
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Azure-erőforrások és csoportok kezelése az Azure parancssori felület használatával
> [!div class="op_single_selector"]
> * [Portal](resource-group-portal.md) 
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST API](resource-manager-rest-api.md)
> 
> 

Az Azure parancssori felület (CLI) használatával telepítheti és kezelheti az erőforrásokat a Resource Manager számos eszköz egyike. Ez a cikk az Azure-erőforrások és csoportok kezelése az Azure parancssori felület használatával erőforrás-kezelő módban gyakori módjai be. Erőforrások telepítése a parancssori felület használatával kapcsolatos információkért lásd: [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](resource-group-template-deploy-cli.md). Azure-erőforrások és erőforrás-kezelő kapcsolatos háttér, látogasson el a [Azure Resource Manager áttekintése](resource-group-overview.md).

> [!NOTE]
> Az Azure parancssori felület az Azure erőforrások kezeléséhez, kell [az Azure parancssori felület telepítése](../cli-install-nodejs.md), és [jelentkezzen be az Azure](../xplat-cli-connect.md) használatával a `azure login` parancsot. Ellenőrizze, hogy a parancssori felület erőforrás-kezelő módban van (Futtatás `azure config mode arm`). Ha ezt ezeket, készen áll nyissa meg.
> 
> 

## <a name="get-resource-groups-and-resources"></a>Erőforráscsoport-sablonok és erőforrások
### <a name="resource-groups"></a>Erőforráscsoportok
Ahhoz, hogy az összes erőforráscsoport az előfizetés és a helyek listáját, futtassa ezt a parancsot.

    azure group list


### <a name="resources"></a>Erőforrások
 Egy csoport például nevű összes erőforrás listáját *testRG*, használja a következő parancsot:

    azure resource list testRG

A csoporton belül egyedi erőforrás megtekintéséhez, például a virtuális gép nevű *MyUbuntuVM*, a következő paranccsal:

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Figyelje meg a **Microsoft.Compute/virtualMachines** paraméter. Ez a paraméter azt jelzi, hogy az adatokat a kért erőforrás típusa.

> [!NOTE]
> Használatakor a **azure-erőforrás** eltérő parancsok a **lista** parancs, meg kell adnia az erőforrás API-verzió a **-o** paraméter. Ha bizonytalan az API-verzió, tekintse meg a sablon fájlt, és a apiVersion mező található az erőforrás. A Resource Manager API-verziók kapcsolatban bővebben lásd: [erőforrás-szolgáltatók és típusok](resource-manager-supported-services.md).
> 
> 

Ha valamely erőforrás adatainak megtekintése, gyakran célszerű használni a `--json` paraméter. Ez a paraméter a kimeneti olvashatóbbá teszi, mivel egyes értékek beágyazott struktúrákat, és a gyűjteményekhez. A következő példa bemutatja, hogy az eredmények visszaadása a **megjelenítése** JSON-dokumentumként parancsot.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> Ha azt szeretné, mentse a JSON-adatok fájlba a &gt; fájlba kimenete karakter. Példa:
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a>Címkék
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Erőforrások kezelése
Például a storage-fiókok erőforrás hozzáadása az erőforráscsoporthoz, hasonló parancs futtatása:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

Az erőforrás API-verzió megadása mellett a **-o** paraméter a **-p** paraméter felelt meg az összes szükséges JSON-formátumú karakterlánc- vagy további tulajdonságokat.

Egy meglévő erőforrás, például virtuálisgép-erőforrás törléséhez használja a következőhöz hasonló parancsot:

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Meglévő erőforrásokat egy másik erőforráscsoportba vagy előfizetésbe történő áthelyezéséhez használja a **az azure erőforrás-áthelyezés** parancsot. A következő példa bemutatja, hogyan egy Redis gyorsítótárhoz áthelyezése egy új erőforráscsoportot. Az a **-i** paraméter, adjon meg egy vesszővel elválasztott lista az erőforrás-azonosító által történő áthelyezéséhez.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Az erőforrások hozzáférés-vezérlése
Az Azure parancssori felület használatával történő hozzáférés szabályozása érdekében Azure-erőforrások szabályzatok létrehozása és kezelése. Házirend-definíciók és a szabályzatok hozzárendelését erőforrások kapcsolatos háttér, lásd: [házirend segítségével kezelheti az erőforrásokat, és hozzáférést](resource-manager-policy.md).

Például adja meg a következő házirendet megtagadja a hozzáférést minden kérelmek, ahol helyre nincs USA nyugati régiója vagy északi középső Régiójában, és mentse a házirend-definíció fájl policy.json:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Ezután futtassa a **házirend-definíció létrehozása** parancs:

    azure policy definition create MyPolicy -p c:\temp\policy.json

Ez a parancs kimenetét mutatja a következőhöz hasonló:

    + Házirend-definíció MyPolicy adatok létrehozása: házirendnév: MyPolicy adatok: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    adatok: PolicyType: egyéni adatok: DisplayName: adatok meg nem határozott: Leírás: adatok nincs definiálva: PolicyRule: mező = hely, a = [westus, northcentralus], hatás = megtagadása

 Rendelje hozzá a házirend a hatókörben, ha azt szeretné, használja a **PolicyDefinitionId** az előző parancs által visszaadott. A következő példában az ebben a hatókörben az előfizetés, de erőforráscsoport-sablonok vagy egyéni erőforrások hatókörét megadhatja:

    az Azure házirend-hozzárendelés létrehozása MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

Get, módosíthatja, vagy távolítsa el a házirend-definíciók használatával a **házirend-definíció megjelenítése**, **házirend-definíció set**, és **házirend-definíció törlése** parancsok.

Hasonlóan beolvasása, módosítsa vagy távolítsa el a házirend-hozzárendelések használatával a **házirend-hozzárendelés megjelenítése**, **házirend-hozzárendelés set**, és **házirend-hozzárendelés törlése** parancsok.

## <a name="export-a-resource-group-as-a-template"></a>Erőforráscsoport exportálása sablonként
Egy meglévő erőforráscsoportot megtekintheti a Resource Manager-sablon ahhoz az erőforráscsoporthoz. A sablon exportálása két előnyökkel jár:

1. Könnyen automatizálható a későbbi központi telepítés alkalmával a megoldás, mert az infrastrukturális van definiálva a sablonban.
2. Válhat ismeri a sablon szintaxisát a JSON a megoldás jelölő megtekintésével.

Az Azure parancssori felület használatával, exportálja a sablont, amely az erőforráscsoport aktuális állapotát jeleníti meg, vagy töltse le az adott példány használt tanúsítványsablont.

* **Erőforráscsoport sablonjának exportálása** – Ez akkor hasznos, ha módosítja egy erőforráscsoport, és szeretné beolvasni a jelenlegi állapotában a JSON-ábrázolását. A létrehozott sablon azonban csak minimális mennyiségű paraméterek és változók nem tartalmaz. Kódolt legtöbbje a sablonban lévő érték. A létrehozott sablon telepítése előtt lehet, hogy a konvertálandó értékek-paraméterek sorozatán testreszabásához, különböző környezetek központi telepítését.
  
    Erőforráscsoport sablonjának exportálása egy helyi könyvtárba, futtassa a `azure group export` parancsot a következő példában látható módon. (A helyi könyvtárat, az operációs rendszer környezetének megfelelő helyettesítő.)
  
        azure group export testRG ~/azure/templates/
* **Töltse le a sablont az adott példány** – Ez akkor hasznos, ha meg kell a tényleges erőforrások telepítéséhez használt tanúsítványsablont. A sablon tartalmazza az összes paraméterek és az eredeti telepítési definiált változókat. Azonban ha valaki van a szervezet módosította az erőforráscsoport a sablon a definíció kívül, ez a sablon nem határoz meg az erőforráscsoport aktuális állapotát.
  
    Az adott példány egy helyi könyvtárba használt sablon letöltéséhez futtassa a `azure group deployment template download` parancsot. Példa:
  
        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/

> [!NOTE]
> Sablon exportálása jelenleg előzetes verzióban érhető, és nem az összes erőforrástípus jelenleg támogatja a sablon exportálása. Sablon exportálása megkísérelte arról, hogy bizonyos erőforrások nem lettek-e exportálva hiba jelenhet meg. Szükség esetén manuálisan ezeket az erőforrásokat a sablonban megadott azt letöltése után.
> 
> 

## <a name="next-steps"></a>Következő lépések
* Részletek a telepítési műveletek és az Azure parancssori felülettel telepítési hibáinak elhárítása: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).
* Ha azt szeretné, állítson be egy alkalmazás vagy parancsprogram hozzáférését az erőforrásokhoz, tekintse meg a parancssori felület segítségével [Azure CLI használata az erőforrásokhoz való hozzáféréshez szolgáltatásnevet létrehozni](resource-group-authenticate-service-principal-cli.md).
* Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.

