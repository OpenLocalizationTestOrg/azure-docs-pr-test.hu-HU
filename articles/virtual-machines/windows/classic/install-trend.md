---
title: "A virtuális gép telepítése a Trend Micro részletes biztonsági |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan kell telepíteni, és a Trend Micro biztonságának konfigurálása a klasszikus telepítési modellt az Azure-ban létrehozott egy virtuális gépen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: iainfou
ms.openlocfilehash: 911b8f12472dcbda3e6bfeb8c97bf1d04a63e1dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>A Trend Micro Deep Security szolgáltatásként való telepítése és konfigurálása windowsos virtuális gépen
> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk a klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.

Ez a cikk bemutatja, hogyan telepítse és konfigurálja a Trend Micro mély biztonsági egy új vagy meglévő virtuális gépen (VM) Windows Server rendszert futtató szolgáltatásként. Szolgáltatásként mély biztonsági magában foglalja a kártevők elleni védelem, a tűzfal, egy behatolás elleni védelmi rendszerének és integritásának ellenőrzésére.

Az ügyfél biztonsági bővítményként keresztül a Virtuálisgép-ügynök telepítve van. Új virtuális gépen, az mély biztonsági ügynök telepítése, mert a Virtuálisgép-ügynök automatikusan hozza létre az Azure-portálon.

A klasszikus portál, az Azure parancssori felület vagy a PowerShell használatával létrehozni meglévő virtuális lehet, hogy nincs egy Virtuálisgép-ügynök. Egy meglévő virtuális gépen, amelyen a Virtuálisgép-ügynök nem rendelkezik akkor kell letöltheti és telepítheti azt. Ez a cikk mindkét eseteire vonatkozik.

Ha egy helyszíni megoldás a Trend Micro aktuális előfizetést, használhatja az Azure virtuális gépek védelme érdekében. Ha nem az ügyfél még, regisztrálhat egy próba-előfizetésre. Információ a megoldásról további információkért lásd a következő Trend Micro blogbejegyzésben [Microsoft Azure virtuális gép ügynök bővítmény a mély biztonsági](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Az átfogó biztonsági ügynök telepítése egy új virtuális gép

A [Azure-portálon](http://portal.azure.com) lehetővé teszi, hogy a Trend Micro biztonsági bővítményének telepítése, a lemezkép használatakor az **piactér** a virtuális gép létrehozásához. Ha egyetlen virtuális gép hoz létre, a portál használatával egyszerű módja védelem hozzáadása a Trend Micro.

Az egyik bejegyzésének használatával a **piactér** megnyit egy varázslót, amely segítséget nyújt a virtuális gép beállítása. Használja a **beállítások** panelen, a harmadik panelen, a varázsló a Trend Micro biztonsági bővítmény telepítésére.  Általános útmutatás: [hozzon létre egy olyan virtuális géphez a Windows Azure-portálon](tutorial.md).

Amikor elér a **beállítások** varázsló panelen tegye a következőket:

1. Kattintson a **bővítmények**, majd kattintson a **felvenni a bővítményt** a következő oldalon.

   ![A bővítmény felvételének megkezdése][1]

2. Válassza ki **mély biztonsági ügynök** a a **új erőforrás** ablaktáblán. A részletes biztonsági ügynök ablaktábláján kattintson **létrehozása**.

   ![A részletes biztonsági megbízott azonosítása][2]

3. Adja meg a **bérlői azonosító** és **bérlői aktiválási jelszó** a bővítmény. Másik lehetőségként megadhat egy **biztonsági házirend-azonosító**. Kattintson a **OK** ügyfél.

   ![Adja meg a bővítmény részletei][3]

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>A részletes biztonsági ügynök telepíthető egy meglévő virtuális Gépen
Az ügynök telepítése egy meglévő virtuális Gépre, az alábbi elemek szükségesek:

* Az Azure PowerShell modul, a 0.8.2 verzió vagy újabb, a helyi számítógépen. Ellenőrizheti, hogy az Azure PowerShell használatával telepített verzióját az **Get-Module azure |} táblázat formázása verzió** parancsot. Útmutatás és egy hivatkozást a legújabb verzióra: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview). Jelentkezzen be az Azure-előfizetés használata `Add-AzureAccount`.
* A Virtuálisgép-ügynök telepítve legyen a cél virtuális gépen.

Először is győződjön meg arról, hogy a Virtuálisgép-ügynök telepítve van. Adja meg a felhőszolgáltatás neve és a virtuális gép nevét, és futtassa a következő parancsokat egy rendszergazda szintű Azure PowerShell parancssorban. Cserélje le a mindent, ami az ajánlatokat, beleértve a < és > karakter.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Ha nem ismeri a felhőszolgáltatás és a virtuális gép nevét, futtassa **Get-AzureVM** megjelenítheti, hogy az összes virtuális gép az aktuális előfizetésben.

Ha a **write-host** parancs beolvasása **igaz**, a Virtuálisgép-ügynök telepítve van. Ha a visszaadott érték **hamis**, tekintse meg az utasításokat és az Azure blogbejegyzésben letöltési mutató hivatkozást [ügynök és Virtuálisgép-bővítmények – 2. rész](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Ha a Virtuálisgép-ügynök telepítve van, futtassa az alábbi parancsokat.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Következő lépések
Az ügynök arra, hogy elindítsa a telepítés néhány percet vesz igénybe. Ezt követően részletes biztonsági aktiválása a virtuális gépen, így kezelhető egy átfogó biztonsági Manager kell. Tekintse meg a további utasításokat a következő cikkeket:

* Információ a megoldásról a trend a cikk [Instant-On Cloud Security a Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
* A [Windows PowerShell-mintaparancsfájl](http://go.microsoft.com/fwlink/?LinkId=404100) a virtuális gép konfigurálásához
* [Útmutatás](http://go.microsoft.com/fwlink/?LinkId=404099) a minta

## <a name="additional-resources"></a>További források
[Bejelentkezés a Windows Server rendszerű virtuális géphez]

[Az Azure Virtuálisgép-bővítmények és szolgáltatások]

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
[Bejelentkezés a Windows Server rendszerű virtuális géphez]:connect-logon.md
[Az Azure Virtuálisgép-bővítmények és szolgáltatások]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
