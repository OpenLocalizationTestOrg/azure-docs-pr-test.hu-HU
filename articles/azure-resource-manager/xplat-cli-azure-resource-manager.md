---
title: "hello Azure CLI aaaManage erőforrások |} Microsoft Docs"
description: "Használja a hello Azure parancssori felület (CLI) toomanage Azure erőforrások és csoportok"
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
ms.openlocfilehash: 3df70e123d14d3bbf2648c71970bac1db4afc025
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cli-toomanage-azure-resources-and-resource-groups"></a>Használja a hello Azure CLI toomanage Azure erőforráscsoport-sablonok és erőforrások
> [!div class="op_single_selector"]
> * [Portál](resource-group-portal.md) 
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST API](resource-manager-rest-api.md)
> 
> 

hello Azure parancssori felület (CLI) az egyik több eszközt használhat toodeploy és a Resource Manager-erőforrások kezelése. Ez a cikk be közös módon toomanage Azure erőforráscsoport-sablonok használatával és az erőforrások hello Azure CLI Resource Manager módra. Hello CLI toodeploy erőforrások használatával kapcsolatos információkért lásd: [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](resource-group-template-deploy-cli.md). Azure-erőforrások és erőforrás-kezelő kapcsolatos háttér, a Microsoft hello [Azure Resource Manager áttekintése](resource-group-overview.md).

> [!NOTE]
> toomanage Azure hello Azure parancssori Felülettel rendelkező erőforrások, kell túl[hello Azure parancssori felület telepítése](../cli-install-nodejs.md), és [tooAzure-e jelentkezni](../xplat-cli-connect.md) hello segítségével `azure login` parancsot. Győződjön meg arról, hogy hello CLI erőforrás-kezelő módban van (Futtatás `azure config mode arm`). Régebben már kötöttek ezeket, ha készen áll a toogo most.
> 
> 

## <a name="get-resource-groups-and-resources"></a>Erőforráscsoport-sablonok és erőforrások
### <a name="resource-groups"></a>Erőforráscsoportok
az előfizetés és a helyek, az összes erőforráscsoport listájának tooget futtassa ezt a parancsot.

    azure group list


### <a name="resources"></a>Erőforrások
 toolist csoportosítva, például az összes erőforrás neve *testRG*, a következő parancs hello használata:

    azure resource list testRG

hello csoportban, például egy nevű virtuális gép egyedi erőforrás tooview *MyUbuntuVM*, hello következő paranccsal:

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Értesítés hello **Microsoft.Compute/virtualMachines** paraméter. Ezzel a paraméterrel adhatja meg a kért információ hello erőforrás hello típusát.

> [!NOTE]
> Hello használatakor **azure-erőforrás** hello eltérő parancsok **lista** parancsban, meg kell adnia hello erőforrás hello API verziója hello **-o** paraméter. Ha bizonytalan hello API-verziót, tekintse át a hello sablonfájl, és hello apiVersion mező hello erőforrás található. A Resource Manager API-verziók kapcsolatban bővebben lásd: [erőforrás-szolgáltatók és típusok](resource-manager-supported-services.md).
> 
> 

Az erőforrás adatainak megtekintése, esetén gyakran hasznos toouse hello `--json` paraméter. Ez a paraméter hello kimeneti olvashatóbbá teszi, mivel egyes értékek beágyazott struktúrákat, és a gyűjteményekhez. hello következő példa bemutatja, hogyan hello eredményeit adatszolgáltató hello **megjelenítése** JSON-dokumentumként parancsot.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> Ha azt szeretné, mentés hello JSON adatok toofile hello segítségével &gt; karakter tooa toodirect hello kimenetfájlba. Példa:
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a>Címkék
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Erőforrások kezelése
például a tárolási fiók tooa erőforráscsoport erőforrás tooadd hasonló parancs futtatása:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

Ezenkívül toospecifying hello-hello erőforrás API-verzió hello **-o** paraméter, használjon hello **-p** paraméter toopass JSON-formátumú karakterlánc, és a szükséges, vagy további tulajdonságokat.

Virtuálisgép-erőforrás, például a meglévő erőforrás toodelete hello következő paranccsal:

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

toomove meglévő erőforrások tooanother erőforráscsoportba vagy előfizetésbe, használja a hello **az azure erőforrás-áthelyezés** parancsot. a következő példa azt mutatja meg hogyan hello toomove Redis Cache tooa új erőforráscsoportot. A hello **-i** paraméter, hello erőforrás-azonosítók toomove vesszővel tagolt listáját tartalmazzák.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-tooresources"></a>Vezérlő hozzáférés tooresources
Hello Azure CLI toocreate használja, és házirendek toocontrol tooAzure erőforrások kezelése. Házirend-definíciók és hozzárendelése házirendek tooresources kapcsolatos háttér, lásd: [házirend toomanage erőforrásainak használatához, és hozzáférést](resource-manager-policy.md).

Például adja meg a következő házirend toodeny hello összes kérelmet, ahol helye nem USA nyugati régiója vagy északi középső Régiójában, és mentse toohello házirend definíciós fájl policy.json:

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

Futtassa a hello **házirend-definíció létrehozása** parancs:

    azure policy definition create MyPolicy -p c:\temp\policy.json

Ez a parancs megjeleníti a kimeneti hasonló toohello következő:

    + Házirend-definíció MyPolicy adatok létrehozása: házirendnév: MyPolicy adatok: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    adatok: PolicyType: egyéni adatok: DisplayName: adatok meg nem határozott: Leírás: adatok nincs definiálva: PolicyRule: mező = hely, a = [westus, northcentralus], hatás = megtagadása

 egy házirend hello hatókörből szeretne, használja hello tooassign **PolicyDefinitionId** hello előző parancs által visszaadott. A következő példa hello, az ebben a hatókörben hello előfizetés, de hatókörét megadhatja a tooresource csoportok vagy az egyes erőforrások:

    az Azure házirend-hozzárendelés létrehozása MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

Get, módosítása, vagy távolítsa el a házirend-definíciók hello segítségével **házirend-definíció megjelenítése**, **házirend-definíció set**, és **házirend-definíció törlése** parancsok.

Hasonlóképpen, beolvasása, módosítása vagy eltávolítása a házirend-hozzárendelések hello segítségével **házirend-hozzárendelés megjelenítése**, **házirend-hozzárendelés set**, és **házirend-hozzárendelés törlése** parancsok .

## <a name="export-a-resource-group-as-a-template"></a>Erőforráscsoport exportálása sablonként
Egy meglévő erőforráscsoportot, a Resource Manager-sablon hello hello erőforráscsoport tekintheti meg. Exportáló hello sablon két előnyökkel jár:

1. Könnyen automatizálható a későbbi központi telepítés alkalmával hello megoldás, mert minden hello infrastruktúra hello sablon definiálva van.
2. Válhat ismeri a sablon szintaxisát a megoldás jelölő JSON hello megtekintésével.

Hello Azure parancssori felület használatával, exportálja a sablont, amely hello az erőforráscsoport aktuális állapotát jeleníti meg, vagy töltse le az adott példány használt hello tanúsítványsablont.

* **Erőforráscsoport hello sablon exportálása** – Ez akkor hasznos, ha végrehajtott módosítások tooa erőforráscsoport, és tooretrieve hello kell a jelenlegi állapotában JSON-ábrázolását. Hello létrehozott sablon azonban csak minimális mennyiségű paraméterek és változók nem tartalmaz. Hello értékek hello sablonban legtöbbje-sablonja. Hello létrehozott sablon telepítés megkezdése előtt érdemes tooconvert hello több-paraméterek sorozatán különböző környezetek hello telepítés testreszabásához.
  
    az erőforrás csoport tooa helyi címtárhoz, futtassa a hello tooexport hello sablon `azure group export` látható módon a következő példa hello parancsot. (A helyi könyvtárat, az operációs rendszer környezetének megfelelő helyettesítő.)
  
        azure group export testRG ~/azure/templates/
* **Az adott példány hello sablon letöltése** – Ez akkor hasznos, ha tooview hello tényleges sablon, de a használt toodeploy erőforrások van szüksége. hello sablon minden paraméterek és változók hello eredeti központi telepítés meghatározott tartalmaz. Azonban ha valaki van a szervezet végzett módosítások toohello erőforráscsoport hello definíció kívül hello sablon, ez a sablon nem határoz meg hello hello erőforráscsoport aktuális állapotát.
  
    egy adott telepítési tooa helyi könyvtárat, hello futtatásához használt toodownload hello sablon `azure group deployment template download` parancsot. Példa:
  
        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/

> [!NOTE]
> Sablon exportálása jelenleg előzetes verzióban érhető, és nem az összes erőforrástípus jelenleg támogatja a sablon exportálása. A sablon tooexport megkísérelte arról, hogy bizonyos erőforrások nem lettek-e exportálva hiba jelenhet meg. Szükség esetén manuálisan ezeket az erőforrásokat a sablonban megadott azt letöltése után.
> 
> 

## <a name="next-steps"></a>Következő lépések
* üzembe helyezési műveletek tooget részleteit és hello Azure parancssori felület telepítési hibáinak elhárításával kapcsolatos tudnivalókat lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).
* Ha azt szeretné, hogy toouse hello CLI tooset egy alkalmazás vagy parancsfájl tooaccess erőforrások, lásd: [egyszerű szolgáltatás használata az Azure parancssori felület toocreate tooaccess erőforrások](resource-group-authenticate-service-principal-cli.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).

