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
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a><span data-ttu-id="09e09-103">Virtuálisgép-bővítmények tartalmazó erőforráscsoportok exportálása</span><span class="sxs-lookup"><span data-stu-id="09e09-103">Exporting Resource Groups that contain VM extensions</span></span>

<span data-ttu-id="09e09-104">Azure erőforráscsoport-sablonok is exportálhatók be egy új Resource Manager-sablon, majd újratelepítése.</span><span class="sxs-lookup"><span data-stu-id="09e09-104">Azure Resource Groups can be exported into a new Resource Manager template that can then be redeployed.</span></span> <span data-ttu-id="09e09-105">hello exportálási folyamat értelmezi a meglévő erőforrásokat, és létrehoz egy Resource Manager-sablon, amely telepítésekor hasonló erőforráscsoport eredményez.</span><span class="sxs-lookup"><span data-stu-id="09e09-105">hello export process interprets existing resources, and creates a Resource Manager template that when deployed results in a similar Resource Group.</span></span> <span data-ttu-id="09e09-106">Ha virtuálisgép-bővítmények tartalmazó erőforráscsoport elleni hello erőforráscsoport exportálási lehetőséget választja, több elemek szükség toobe számít például bővítmény kompatibilitási, és védett beállítások.</span><span class="sxs-lookup"><span data-stu-id="09e09-106">When using hello Resource Group export option against a Resource Group containing Virtual Machine extensions, several items need toobe considered such as extension compatibility and protected settings.</span></span>

<span data-ttu-id="09e09-107">Ez a dokumentum részletek hello erőforráscsoport exportálási folyamat működésével kapcsolatban virtuálisgép-bővítmények, beleértve a listáját támogatott bővítmények, és a védett adatok részleteinek kezelése.</span><span class="sxs-lookup"><span data-stu-id="09e09-107">This document details how hello Resource Group export process works regarding virtual machine extensions, including a list of supported extensions, and details on handling secured data.</span></span>

## <a name="supported-virtual-machine-extensions"></a><span data-ttu-id="09e09-108">Támogatott virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="09e09-108">Supported Virtual Machine Extensions</span></span>

<span data-ttu-id="09e09-109">Több virtuálisgép-bővítmények érhetők el.</span><span class="sxs-lookup"><span data-stu-id="09e09-109">Many Virtual Machine extensions are available.</span></span> <span data-ttu-id="09e09-110">Nem az összes bővítmény exportálhatja azokat a Resource Manager-sablon hello "Automatizálási parancsfájl" funkció használata.</span><span class="sxs-lookup"><span data-stu-id="09e09-110">Not all extensions can be exported into a Resource Manager template using hello “Automation Script” feature.</span></span> <span data-ttu-id="09e09-111">A virtuálisgép-bővítmény nem támogatott, hogy szükséges-e toobe manuálisan újra hello exportált sablon üzembe helyezni.</span><span class="sxs-lookup"><span data-stu-id="09e09-111">If a virtual machine extension is not supported, it needs toobe manually placed back into hello exported template.</span></span>

<span data-ttu-id="09e09-112">hello következő kiterjesztésekkel exportálhatók hello automatizálási parancsfájl szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="09e09-112">hello following extensions can be exported with hello automation script feature.</span></span>

| <span data-ttu-id="09e09-113">Mellék</span><span class="sxs-lookup"><span data-stu-id="09e09-113">Extension</span></span> ||||
|---|---|---|---|
| <span data-ttu-id="09e09-114">Acronis biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="09e09-114">Acronis Backup</span></span> | <span data-ttu-id="09e09-115">Datadog Windows-ügynök</span><span class="sxs-lookup"><span data-stu-id="09e09-115">Datadog Windows Agent</span></span> | <span data-ttu-id="09e09-116">Javítás Linux operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="09e09-116">OS Patching For Linux</span></span> | <span data-ttu-id="09e09-117">Virtuális gép pillanatkép Linux</span><span class="sxs-lookup"><span data-stu-id="09e09-117">VM Snapshot Linux</span></span>
| <span data-ttu-id="09e09-118">Linux Acronis biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="09e09-118">Acronis Backup Linux</span></span> | <span data-ttu-id="09e09-119">Docker-bővítmény</span><span class="sxs-lookup"><span data-stu-id="09e09-119">Docker Extension</span></span> | <span data-ttu-id="09e09-120">Puppet ügynök</span><span class="sxs-lookup"><span data-stu-id="09e09-120">Puppet Agent</span></span> |
| <span data-ttu-id="09e09-121">BG adatai</span><span class="sxs-lookup"><span data-stu-id="09e09-121">Bg Info</span></span> | <span data-ttu-id="09e09-122">DSC-bővítmény</span><span class="sxs-lookup"><span data-stu-id="09e09-122">DSC Extension</span></span> | <span data-ttu-id="09e09-123">Hely 24 x 7 Apm felmérése</span><span class="sxs-lookup"><span data-stu-id="09e09-123">Site 24x7 Apm Insight</span></span> |
| <span data-ttu-id="09e09-124">BMC CTM ügynök Linux</span><span class="sxs-lookup"><span data-stu-id="09e09-124">BMC CTM Agent Linux</span></span> | <span data-ttu-id="09e09-125">Dynatrace Linux</span><span class="sxs-lookup"><span data-stu-id="09e09-125">Dynatrace Linux</span></span> | <span data-ttu-id="09e09-126">24 x 7 Linux helykiszolgáló</span><span class="sxs-lookup"><span data-stu-id="09e09-126">Site 24x7 Linux Server</span></span> |
| <span data-ttu-id="09e09-127">BMC CTM ügynök Windows</span><span class="sxs-lookup"><span data-stu-id="09e09-127">BMC CTM Agent Windows</span></span> | <span data-ttu-id="09e09-128">Dynatrace Windows</span><span class="sxs-lookup"><span data-stu-id="09e09-128">Dynatrace Windows</span></span> | <span data-ttu-id="09e09-129">Hely 24 x 7 a Windows Server</span><span class="sxs-lookup"><span data-stu-id="09e09-129">Site 24x7 Windows Server</span></span> |
| <span data-ttu-id="09e09-130">Chef ügyfél</span><span class="sxs-lookup"><span data-stu-id="09e09-130">Chef Client</span></span> | <span data-ttu-id="09e09-131">HPE biztonsági alkalmazás Defender</span><span class="sxs-lookup"><span data-stu-id="09e09-131">HPE Security Application Defender</span></span> | <span data-ttu-id="09e09-132">Trend Micro DSA</span><span class="sxs-lookup"><span data-stu-id="09e09-132">Trend Micro DSA</span></span> |
| <span data-ttu-id="09e09-133">Egyéni parancsfájl</span><span class="sxs-lookup"><span data-stu-id="09e09-133">Custom Script</span></span> | <span data-ttu-id="09e09-134">Infrastruktúra-szolgáltatási kártevőirtó</span><span class="sxs-lookup"><span data-stu-id="09e09-134">IaaS Antimalware</span></span> | <span data-ttu-id="09e09-135">Trend Micro DSA Linux</span><span class="sxs-lookup"><span data-stu-id="09e09-135">Trend Micro DSA Linux</span></span> |
| <span data-ttu-id="09e09-136">Egyéni szkriptbővítmény</span><span class="sxs-lookup"><span data-stu-id="09e09-136">Custom Script Extension</span></span> | <span data-ttu-id="09e09-137">IaaS-diagnosztika</span><span class="sxs-lookup"><span data-stu-id="09e09-137">IaaS Diagnostics</span></span> | <span data-ttu-id="09e09-138">Linux virtuális gép hozzáférés</span><span class="sxs-lookup"><span data-stu-id="09e09-138">VM Access For Linux</span></span> |
| <span data-ttu-id="09e09-139">Egyéni parancsfájl Linux</span><span class="sxs-lookup"><span data-stu-id="09e09-139">Custom Script for Linux</span></span> | <span data-ttu-id="09e09-140">Linux-Chef ügyfél</span><span class="sxs-lookup"><span data-stu-id="09e09-140">Linux Chef Client</span></span> | <span data-ttu-id="09e09-141">Linux virtuális gép hozzáférés</span><span class="sxs-lookup"><span data-stu-id="09e09-141">VM Access For Linux</span></span> |
| <span data-ttu-id="09e09-142">Datadog Linux-ügynök</span><span class="sxs-lookup"><span data-stu-id="09e09-142">Datadog Linux Agent</span></span> | <span data-ttu-id="09e09-143">Linux-diagnosztika</span><span class="sxs-lookup"><span data-stu-id="09e09-143">Linux Diagnostic</span></span> | <span data-ttu-id="09e09-144">Virtuális gép pillanatkép</span><span class="sxs-lookup"><span data-stu-id="09e09-144">VM Snapshot</span></span> |

## <a name="export-hello-resource-group"></a><span data-ttu-id="09e09-145">Hello erőforráscsoport exportálása</span><span class="sxs-lookup"><span data-stu-id="09e09-145">Export hello Resource Group</span></span>

<span data-ttu-id="09e09-146">tooexport egy erőforráscsoport, újrafelhasználható sablonba teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="09e09-146">tooexport a Resource Group into a reusable template, complete hello following steps:</span></span>

1. <span data-ttu-id="09e09-147">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="09e09-147">Sign in toohello Azure portal</span></span>
2. <span data-ttu-id="09e09-148">A központ menüben hello kattintson az erőforráscsoportok</span><span class="sxs-lookup"><span data-stu-id="09e09-148">On hello Hub Menu, click Resource Groups</span></span>
3. <span data-ttu-id="09e09-149">Válassza ki a célként megadott erőforráscsoportja hello hello listából</span><span class="sxs-lookup"><span data-stu-id="09e09-149">Select hello target resource group from hello list</span></span>
4. <span data-ttu-id="09e09-150">Hello erőforráscsoport panelen kattintson az automatizálási parancsfájl</span><span class="sxs-lookup"><span data-stu-id="09e09-150">In hello Resource Group blade, click Automation Script</span></span>

![Sablon exportálása](./media/extensions-export-templates/template-export.png)

<span data-ttu-id="09e09-152">hello Azure Resource Manager automatizálása parancsfájlt hoz létre a Resource Manager-sablon, egy paraméterfájl és több központi telepítési mintaparancsfájlok például a PowerShell és az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="09e09-152">hello Azure Resource Manager automations script produces a Resource Manager template, a parameters file, and several sample deployment scripts such as PowerShell and Azure CLI.</span></span> <span data-ttu-id="09e09-153">Ezen a ponton hello exportált sablon letölthető a hello Letöltés gombra kattintva, fel van véve egy új sablont toohello Sablonkönyvtár vagy újratelepíteni a hello segítségével telepítése gombra.</span><span class="sxs-lookup"><span data-stu-id="09e09-153">At this point, hello exported template can be downloaded using hello download button, added as a new template toohello template library, or redeployed using hello deploy button.</span></span>

## <a name="configure-protected-settings"></a><span data-ttu-id="09e09-154">Védett beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="09e09-154">Configure protected settings</span></span>

<span data-ttu-id="09e09-155">Sok Azure virtuálisgép-bővítmények közé tartozik egy védett beállításokat, titkosítja a bizalmas adatokat, például a hitelesítő adatokat és a konfigurációs karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="09e09-155">Many Azure virtual machine extensions include a protected settings configuration, that encrypts sensitive data such as credentials and configuration strings.</span></span> <span data-ttu-id="09e09-156">Védett beállításainak exportálása nem rendelkező hello automatizálási parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="09e09-156">Protected settings are not exported with hello automation script.</span></span> <span data-ttu-id="09e09-157">Ha szükséges, a védett beállításokat kell ismételten beszúrni a hello toobe exportált sablon.</span><span class="sxs-lookup"><span data-stu-id="09e09-157">If necessary, protected settings need toobe reinserted into hello exported templated.</span></span>

### <a name="step-1---remove-template-parameter"></a><span data-ttu-id="09e09-158">1. lépés – a sablonparaméter eltávolítása</span><span class="sxs-lookup"><span data-stu-id="09e09-158">Step 1 - Remove template parameter</span></span>

<span data-ttu-id="09e09-159">Erőforráscsoport exportálása, hello egyetlen sablonparaméter tooprovide létrehozásakor egy érték toohello exportált védett beállításait.</span><span class="sxs-lookup"><span data-stu-id="09e09-159">When hello Resource Group is exported, a single template parameter is created tooprovide a value toohello exported protected settings.</span></span> <span data-ttu-id="09e09-160">Ezzel a paraméterrel eltávolítható.</span><span class="sxs-lookup"><span data-stu-id="09e09-160">This parameter can be removed.</span></span> <span data-ttu-id="09e09-161">tooremove hello paraméter, nézze át a hello paraméterlista, és törli, a következőhöz hasonló toothis JSON példa hello paramétert.</span><span class="sxs-lookup"><span data-stu-id="09e09-161">tooremove hello parameter, look through hello parameter list and delete hello parameter that looks similar toothis JSON example.</span></span>

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a><span data-ttu-id="09e09-162">2. lépés - Get védett beállításainak tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="09e09-162">Step 2 - Get protected settings properties</span></span>

<span data-ttu-id="09e09-163">Mivel minden egyes védett beállítás szükséges tulajdonságait, ezek a tulajdonságok listájának kell összegyűjteni toobe.</span><span class="sxs-lookup"><span data-stu-id="09e09-163">Because each protected setting has a set of required properties, a list of these properties need toobe gathered.</span></span> <span data-ttu-id="09e09-164">Minden egyes hello védett beállítások konfigurációs paramétere megtalálható hello [Azure Resource Manager séma a Githubon](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json).</span><span class="sxs-lookup"><span data-stu-id="09e09-164">Each parameter of hello protected settings configuration can be found in hello [Azure Resource Manager schema on GitHub](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json).</span></span> <span data-ttu-id="09e09-165">A séma csak hello paraméterkészletei ebben a dokumentumban hello áttekintés részben felsorolt hello Extensions tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="09e09-165">This schema only includes hello parameter sets for hello extensions listed in hello overview section of this document.</span></span> 

<span data-ttu-id="09e09-166">A belül hello séma-tárházban, keressen rá szükség hello bővítményt, ehhez a példához `IaaSDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="09e09-166">From within hello schema repository, search for hello desired extension, for this example `IaaSDiagnostics`.</span></span> <span data-ttu-id="09e09-167">Egyszer hello bővítmények `protectedSettings` objektum található, jegyezze fel az egyes paramétereket.</span><span class="sxs-lookup"><span data-stu-id="09e09-167">Once hello extensions `protectedSettings` object has been located, take note of each parameter.</span></span> <span data-ttu-id="09e09-168">Hello hello példájában `IaasDiagnostic` bővítmény hello szükséges paraméterek `storageAccountName`, `storageAccountKey`, és `storageAccountEndPoint`.</span><span class="sxs-lookup"><span data-stu-id="09e09-168">In hello example of hello `IaasDiagnostic` extension, hello require parameters are `storageAccountName`, `storageAccountKey`, and `storageAccountEndPoint`.</span></span>

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

### <a name="step-3---re-create-hello-protected-configuration"></a><span data-ttu-id="09e09-169">3. lépés – védett hello konfiguráció újbóli létrehozása</span><span class="sxs-lookup"><span data-stu-id="09e09-169">Step 3 - Re-create hello protected configuration</span></span>

<span data-ttu-id="09e09-170">Az exportált sablon hello, keressen `protectedSettings` és hello exportált védett beállításobjektumot cserélje le egy új, amely tartalmazza a szükséges hello kiterjesztési paraméterek és minden egyes értéket.</span><span class="sxs-lookup"><span data-stu-id="09e09-170">On hello exported template, search for `protectedSettings` and replace hello exported protected setting object with a new one that includes hello required extension parameters and a value for each one.</span></span>

<span data-ttu-id="09e09-171">Hello hello példájában `IaasDiagnostic` bővítmény hello új védett beállítások konfigurálása jelenne meg a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="09e09-171">In hello example of hello `IaasDiagnostic` extension, hello new protected setting configuration would look like hello following example:</span></span>

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

<span data-ttu-id="09e09-172">hello végső kiterjesztés erőforrás a következőhöz hasonló toohello JSON például a következő:</span><span class="sxs-lookup"><span data-stu-id="09e09-172">hello final extension resource looks similar toohello following JSON example:</span></span>

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

<span data-ttu-id="09e09-173">Sablon paraméterértéket tooprovide tulajdonság használata esetén ezek létrehozott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="09e09-173">If using template parameters tooprovide property values, these need toobe created.</span></span> <span data-ttu-id="09e09-174">Sablonparaméterek létrehozásához, a beállítás értéke a védett, győződjön meg arról, hogy toouse hello `SecureString` paraméter írja be, hogy az érzékeny értékek biztonságosak.</span><span class="sxs-lookup"><span data-stu-id="09e09-174">When creating template parameters for protected setting values, make sure toouse hello `SecureString` parameter type so that sensitive values are secured.</span></span> <span data-ttu-id="09e09-175">Paraméterek használatával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="09e09-175">For more information on using parameters, see [Authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="09e09-176">Hello hello példájában `IaasDiagnostic` kiterjesztés, a következő paraméterek hello jönnek létre hello paraméterek szakaszban hello Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="09e09-176">In hello example of hello `IaasDiagnostic` extension, hello following parameters would be created in hello parameters section of hello Resource Manager template.</span></span>

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

<span data-ttu-id="09e09-177">Ezen a ponton hello sablont is telepíthető a sablon az üzembe helyezési módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="09e09-177">At this point, hello template can be deployed using any template deployment method.</span></span>
