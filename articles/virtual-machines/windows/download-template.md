---
title: "egy Azure virtuális gép sablonját aaaDownload hello |} Microsoft Docs"
description: "Töltse le a virtuális gép toohelp hello Resource Manager üzembe helyezési modellel központi telepítések végrehajthassa hello templatefor"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 51ef4f51-0942-4249-afea-4a3f87ce1ff8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 86fd05f67409019b5e5c9023881745047860eee1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="download-hello-template-for-a-vm"></a>A virtuális gépek hello sablon letöltése
Ha a virtuális gép létrehozása az Azure-ban hello portál vagy PowerShell-lel, erőforrás-kezelő sablon automatikusan létrejön. A sablon tooquickly duplikált a központi telepítés is használhatja. hello sablon összes hello erőforrás egy erőforráscsoportban található információt tartalmaz. A virtuális gép esetén ez azt jelenti, hello sablon tartalmaz mindent, az erőforráscsoport, beleértve a hálózati erőforrások hello VM hello elősegítésére jön létre.

## <a name="download-hello-template-using-hello-portal"></a>Hello portálon hello sablon letöltése
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Egy hello menüben válassza **virtuális gépek**.
3. Válassza ki a virtuális gép hello hello listából.
4. Válassza ki **automatizálási parancsfájl**.
5. Válassza ki **letöltése** , és mentse a hello .zip fájl tooyour helyi számítógépen.
6. Nyissa meg hello .zip fájlt, és bontsa ki a hello fájlok tooa mappa. hello .zip-fájlt fogja tartalmazni:
   
   * Deploy.ps1
   * Deploy.SH 
   * deployer.RB
   * DeploymentHelper.cs
   * Parameters.JSON
   * Template.JSON

hello template.json fájl hello sablont.

## <a name="download-hello-template-using-powershell"></a>PowerShell-lel hello sablon letöltése
Hello .JSON kiterjesztésű sablonfájl hello segítségével is letöltheti [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) parancsmag. Használhatja a hello `-path` paraméter tooprovide hello fájlnév és hello .JSON kiterjesztésű fájl elérési útját. Ez a példa bemutatja, hogyan toodownload hello hello erőforráscsoport sablonjának nevű **myResourceGroup** toohello **C:\users\public\downloads** mappát a helyi számítógépen.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Következő lépések
toolearn sablonokkal, erőforrásokat üzembe helyezi bővebben lásd: [Resource Manager sablonokhoz](../../azure-resource-manager/resource-manager-template-walkthrough.md).

