---
title: "Virtuálisgép-bővítmények tartalmazó Azure erőforráscsoportok aaaExporting |} Microsoft Docs"
description: "Virtuálisgép-bővítmények tartalmazó Resource Manager-sablonok exportálása."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7f4e2ca6-f1c7-4f59-a2cc-8f63132de279
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: nepeters
ms.openlocfilehash: cdbc2030988a19fe68429e8733dc60536c264abf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a>Virtuálisgép-bővítmények tartalmazó erőforráscsoportok exportálása

Azure erőforráscsoport-sablonok is exportálhatók be egy új Resource Manager-sablon, majd újratelepítése. hello exportálási folyamat értelmezi a meglévő erőforrásokat, és létrehoz egy Resource Manager-sablon, amely telepítésekor hasonló erőforráscsoport eredményez. Ha virtuálisgép-bővítmények tartalmazó erőforráscsoport elleni hello erőforráscsoport exportálási lehetőséget választja, több elemek szükség toobe számít például bővítmény kompatibilitási, és védett beállítások.

Ez a dokumentum részletek hello erőforráscsoport exportálási folyamat működésével kapcsolatban virtuálisgép-bővítmények, beleértve a listáját támogatott bővítmények, és a védett adatok részleteinek kezelése.

## <a name="supported-virtual-machine-extensions"></a>Támogatott virtuálisgép-bővítmények

Több virtuálisgép-bővítmények érhetők el. Nem az összes bővítmény exportálhatja azokat a Resource Manager-sablon hello "Automatizálási parancsfájl" funkció használata. A virtuálisgép-bővítmény nem támogatott, hogy szükséges-e toobe manuálisan újra hello exportált sablon üzembe helyezni.

hello következő kiterjesztésekkel exportálhatók hello automatizálási parancsfájl szolgáltatással.

| Mellék ||||
|---|---|---|---|
| Acronis biztonsági mentése | Datadog Windows-ügynök | Javítás Linux operációs rendszer | Virtuális gép pillanatkép Linux
| Linux Acronis biztonsági mentése | Docker-bővítmény | Puppet ügynök |
| BG adatai | DSC-bővítmény | Hely 24 x 7 Apm felmérése |
| BMC CTM ügynök Linux | Dynatrace Linux | 24 x 7 Linux helykiszolgáló |
| BMC CTM ügynök Windows | Dynatrace Windows | Hely 24 x 7 a Windows Server |
| Chef ügyfél | HPE biztonsági alkalmazás Defender | Trend Micro DSA |
| Egyéni parancsfájl | Infrastruktúra-szolgáltatási kártevőirtó | Trend Micro DSA Linux |
| Egyéni szkriptbővítmény | IaaS-diagnosztika | Linux virtuális gép hozzáférés |
| Egyéni parancsfájl Linux | Linux-Chef ügyfél | Linux virtuális gép hozzáférés |
| Datadog Linux-ügynök | Linux-diagnosztika | Virtuális gép pillanatkép |

## <a name="export-hello-resource-group"></a>Hello erőforráscsoport exportálása

tooexport egy erőforráscsoport, újrafelhasználható sablonba teljes hello a következő lépéseket:

1. Jelentkezzen be toohello Azure-portálon
2. A központ menüben hello kattintson az erőforráscsoportok
3. Válassza ki a célként megadott erőforráscsoportja hello hello listából
4. Hello erőforráscsoport panelen kattintson az automatizálási parancsfájl

![Sablon exportálása](./media/extensions-export-templates/template-export.png)

hello Azure Resource Manager automatizálása parancsfájlt hoz létre a Resource Manager-sablon, egy paraméterfájl és több központi telepítési mintaparancsfájlok például a PowerShell és az Azure parancssori felület. Ezen a ponton hello exportált sablon letölthető a hello Letöltés gombra kattintva, fel van véve egy új sablont toohello Sablonkönyvtár vagy újratelepíteni a hello segítségével telepítése gombra.

## <a name="configure-protected-settings"></a>Védett beállításainak konfigurálása

Sok Azure virtuálisgép-bővítmények közé tartozik egy védett beállításokat, titkosítja a bizalmas adatokat, például a hitelesítő adatokat és a konfigurációs karakterlánc. Védett beállításainak exportálása nem rendelkező hello automatizálási parancsfájl. Ha szükséges, a védett beállításokat kell ismételten beszúrni a hello toobe exportált sablon.

### <a name="step-1---remove-template-parameter"></a>1. lépés – a sablonparaméter eltávolítása

Erőforráscsoport exportálása, hello egyetlen sablonparaméter tooprovide létrehozásakor egy érték toohello exportált védett beállításait. Ezzel a paraméterrel eltávolítható. tooremove hello paraméter, nézze át a hello paraméterlista, és törli, a következőhöz hasonló toothis JSON példa hello paramétert.

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a>2. lépés - Get védett beállításainak tulajdonságai

Mivel minden egyes védett beállítás szükséges tulajdonságait, ezek a tulajdonságok listájának kell összegyűjteni toobe. Minden egyes hello védett beállítások konfigurációs paramétere megtalálható hello [Azure Resource Manager séma a Githubon](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json). A séma csak hello paraméterkészletei ebben a dokumentumban hello áttekintés részben felsorolt hello Extensions tartalmazza. 

A belül hello séma-tárházban, keressen rá szükség hello bővítményt, ehhez a példához `IaaSDiagnostics`. Egyszer hello bővítmények `protectedSettings` objektum található, jegyezze fel az egyes paramétereket. Hello hello példájában `IaasDiagnostic` bővítmény hello szükséges paraméterek `storageAccountName`, `storageAccountKey`, és `storageAccountEndPoint`.

```json
"protectedSettings": {
    "type": "object",
    "properties": {
        "storageAccountName": {
            "type": "string"
        },
        "storageAccountKey": {
            "type": "string"
        },
        "storageAccountEndPoint": {
            "type": "string"
        }
    },
    "required": [
        "storageAccountName",
        "storageAccountKey",
        "storageAccountEndPoint"
    ]
}
```

### <a name="step-3---re-create-hello-protected-configuration"></a>3. lépés – védett hello konfiguráció újbóli létrehozása

Az exportált sablon hello, keressen `protectedSettings` és hello exportált védett beállításobjektumot cserélje le egy új, amely tartalmazza a szükséges hello kiterjesztési paraméterek és minden egyes értéket.

Hello hello példájában `IaasDiagnostic` bővítmény hello új védett beállítások konfigurálása jelenne meg a következő példa hello:

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

hello végső kiterjesztés erőforrás a következőhöz hasonló toohello JSON például a következő:

```json
{
    "name": "Microsoft.Insights.VMDiagnosticsSettings",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "[variables('apiVersion')]",
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
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]",
            "storageAccountEndPoint": "https://core.windows.net"
        }
    }
}
```

Sablon paraméterértéket tooprovide tulajdonság használata esetén ezek létrehozott toobe kell. Sablonparaméterek létrehozásához, a beállítás értéke a védett, győződjön meg arról, hogy toouse hello `SecureString` paraméter írja be, hogy az érzékeny értékek biztonságosak. Paraméterek használatával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](../../resource-group-authoring-templates.md).

Hello hello példájában `IaasDiagnostic` kiterjesztés, a következő paraméterek hello jönnek létre hello paraméterek szakaszban hello Resource Manager-sablon.

```json
"storageAccountName": {
    "defaultValue": null,
    "type": "SecureString"
},
"storageAccountKey": {
    "defaultValue": null,
    "type": "SecureString"
}
```

Ezen a ponton hello sablont is telepíthető a sablon az üzembe helyezési módszer használatával.
