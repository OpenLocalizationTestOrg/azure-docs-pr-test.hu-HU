---
title: "Symantec Endpoint Protection szolgáltatás a Windows Azure-ban aaaInstall |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és hello Symantec Endpoint Protection biztonsági kiterjesztés konfigurálása egy új vagy meglévő Azure virtuális Géphez létre hello klasszikus telepítési modellt."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 19dcebc7-da6b-4510-907b-d64088e81fa2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: iainfou
ms.openlocfilehash: cb6083366185c26c9f43ff94d835cd90725de5b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Hogyan tooinstall és a Symantec Endpoint Protection konfigurálása a Windows virtuális gép
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

Ez a cikk bemutatja, hogyan tooinstall és egy meglévő virtuális gépen (VM) Windows Server rendszert futtató hello Symantec Endpoint Protection-ügyfél konfigurálása. A teljes ügyfél szolgáltatásokat, például vírus- és kémprogram-védelmi-, tűzfal, és behatolás megelőzési tartalmazza. hello ügyfél biztonsági bővítményként segítségével hello Virtuálisgép-ügynök telepítve van.

Ha egy helyszíni megoldás a Symantec egy meglévő előfizetéshez, használhatja tooprotect az Azure virtuális gépeken. Ha nem az ügyfél még, regisztrálhat egy próba-előfizetésre. Információ a megoldásról további információkért lásd: [Symantec Endpoint Protection a Microsoft Azure platformon][Symantec]. Ennek a lapnak a hivatkozások toolicensing információi és útmutatója hello ügyfél telepítése, ha Ön már egy Symantec-ügyfél is van.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Symantec Endpoint Protection telepítése egy meglévő virtuális gépen
Mielőtt elkezdené, hello következőkre lesz szüksége:

* hello Azure PowerShell modul, a 0.8.2 verzió vagy újabb verzióját, a munkahelyi számítógép. Ellenőrizheti a hello Azure PowerShell hello telepített verziójának **Get-Module azure |} táblázat formázása verzió** parancsot. Útmutatás és egy hivatkozásra toohello legújabb verzió: [hogyan tooInstall és konfigurálása az Azure PowerShell][PS]. Jelentkezzen be tooyour Azure-előfizetés `Add-AzureAccount`.
* hello futó Virtuálisgép-ügynök hello Azure virtuális gépen.

Először győződjön meg arról, hogy hello Virtuálisgép-ügynök már telepítve van a hello virtuális gépen. Adja meg hello felhőszolgáltatás neve és a virtuális gép nevét, és futtassa a parancsokat egy rendszergazda szintű Azure PowerShell parancssorba a következő hello. Cserélje le mindent, ami hello idézőjelek között, beleértve a hello < és > karakter.

> [!TIP]
> Ha nem tudja hello felhőszolgáltatás és a virtuális gép nevét, futtassa **Get-AzureVM** toolist hello nevek az összes virtuális gép az aktuális előfizetésben.

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

Ha hello **write-host** parancs megjeleníti **igaz**, hello Virtuálisgép-ügynök telepítve van. Ha megjeleníti **hamis**, hello utasításokat, és egy hivatkozásra toohello, töltse le a hello Azure blogbejegyzés [ügynök és Virtuálisgép-bővítmények – 2. rész][Agent].

Ha hello Virtuálisgép-ügynök telepítve van, ezek parancsok tooinstall hello Symantec Endpoint Protection-ügynök fut.

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

tooverify, amelyek Symantec biztonsági bővítmény hello telepítve van és naprakész:

1. Jelentkezzen be a virtuális gép toohello. Útmutatásért lásd: [hogyan tooLog a virtuális gép futó Windows Server tooa][Logon].
2. A Windows Server 2008 R2, kattintson a **Start > Symantec Endpoint Protection**. Írja be a Windows Server 2012 vagy Windows Server 2012 R2-ben hello kezdőképernyőről **Symantec**, és kattintson a **Symantec Endpoint Protection**.
3. A hello **állapot** hello lapján **állapot-Symantec Endpoint Protection** ablak, alkalmazza a frissítéseket, vagy szükség esetén indítsa újra.

## <a name="additional-resources"></a>További források
[Hogyan tooLog a virtuális gép futó Windows Server tooa][Logon]

[Az Azure Virtuálisgép-bővítmények és szolgáltatások][Ext]

<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493
