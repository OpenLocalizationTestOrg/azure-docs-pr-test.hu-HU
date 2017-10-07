---
title: "aaaRedeploy Windows virtuális gépek Azure-ban |} Microsoft Docs"
description: "Hogyan tooredeploy Windows virtuális gépek Azure toomitigate RDP-kapcsolaton ad ki."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: genlin
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 903d9d5bf241075931ee4b746690c553d808a58f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="redeploy-windows-virtual-machine-toonew-azure-node"></a>Telepítse újra a Windows virtuális gép toonew Azure csomópont
Ha Ön rendelkezik lett nehézségekkel hibaelhárítás a távoli asztal (RDP) kapcsolat vagy az alkalmazás eléréséhez tooWindows-alapú Azure virtuális gép (VM) újratelepíteni a virtuális gép segíthet hello. A virtuális gép újbóli telepítésének, hello VM tooa új csomópont belül hello Azure-infrastruktúra helyezi át, és majd bekapcsolja azt vissza, a konfigurációs beállításokat és a kapcsolódó erőforrások megőrzése. Ez a cikk bemutatja, hogyan tooredeploy a virtuális gépet az Azure PowerShell vagy hello Azure-portálon.

> [!NOTE]
> Miután a virtuális gép újbóli telepítésének, hello mennyiségű ideiglenes lemezes elvesztését, és dinamikus IP-címek társított virtuális hálózati illesztő frissítése. 


## <a name="using-azure-powershell"></a>Az Azure PowerShell használata
Győződjön meg arról, hogy hello rendelkezik a legújabb Azure PowerShell 1.x van telepítve a számítógépre. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

hello következő példa telepíti hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Következő lépések
Csatlakozás a virtuális gép tooyour problémát tapasztal, ha található segítséget [RDP-kapcsolatok hibáinak elhárítása](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [részletes hibaelhárítási RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ha nem fér hozzá a virtuális gép futó alkalmazást, is olvasható [problémák elhárítása alkalmazás](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

