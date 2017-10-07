---
title: "Trend Micro mély biztonsági a virtuális gép aaaInstall |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooinstall és a Trend Micro biztonságának konfigurálása a hello klasszikus telepítési modellt az Azure-ban létrehozott virtuális gép."
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
ms.openlocfilehash: dc5492db07a37a2296df5da673183a14c6d5b1f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Hogyan tooinstall és a Trend Micro mély biztonságának konfigurálása a Windows virtuális gép szolgáltatásként
> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

Ez a cikk bemutatja, hogyan tooinstall és a Trend Micro mély biztonságának konfigurálása egy új vagy meglévő virtuális gépen (VM) Windows Server rendszert futtató szolgáltatásként. Szolgáltatásként mély biztonsági magában foglalja a kártevők elleni védelem, a tűzfal, egy behatolás elleni védelmi rendszerének és integritásának ellenőrzésére.

hello ügyfél biztonsági bővítményként keresztül hello Virtuálisgép-ügynök telepítve van. Új virtuális gépen a hello mély biztonsági ügynök hello Virtuálisgép-ügynök hello Azure-portál által automatikusan létrehozott, telepítenie.

Hello klasszikus portálra, a hello Azure CLI vagy a PowerShell segítségével létrehozni meglévő virtuális lehet, hogy nincs egy Virtuálisgép-ügynök. Egy meglévő virtuális gép nem található a Virtuálisgép-ügynök hello esetén toodownload kell, és azt telepítse először. Ez a cikk mindkét eseteire vonatkozik.

Ha egy helyszíni megoldás a Trend Micro aktuális előfizetést, használhatja toohelp védeni az Azure virtuális gépeken. Ha nem az ügyfél még, regisztrálhat egy próba-előfizetésre. Információ a megoldásról további információkért lásd: hello Trend Micro blogbejegyzésben [Microsoft Azure virtuális gép ügynök bővítmény a mély biztonsági](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-hello-deep-security-agent-on-a-new-vm"></a>Hello mély biztonsági ügynök telepítése egy új virtuális Gépre

Hello [Azure-portálon](http://portal.azure.com) lehetővé teszi, hogy hello Trend Micro biztonsági bővítményének telepítése az hello lemezképének használatakor **piactér** toocreate hello virtuális gépet. Ha egyetlen virtuális gép hoz létre, egy egyszerűen tooadd védelmet a Trend Micro hello portál használatával.

A hello egyik bejegyzésének használatával **piactér** megnyit egy varázslót, amely segít állítson be hello virtuális gépet. Hello használata **beállítások** panelen, a harmadik panel hello hello varázsló tooinstall hello Trend Micro biztonsági bővítmény.  Általános útmutatás: [hello Azure-portálon a Windows operációs rendszert futtató virtuális gép létrehozása](tutorial.md).

Amikor elérhetővé toohello **beállítások** hello varázsló panel hello lépéseket követve:

1. Kattintson a **bővítmények**, majd kattintson a **felvenni a bővítményt** hello következő ablaktáblán.

   ![Hello bővítmény felvételének megkezdése][1]

2. Válassza ki **mély biztonsági ügynök** a hello **új erőforrás** ablaktáblán. Hello mély biztonsági ügynök ablaktábláján kattintson **létrehozása**.

   ![A részletes biztonsági megbízott azonosítása][2]

3. Adja meg a hello **bérlői azonosító** és **bérlői aktiválási jelszó** hello bővítmény. Másik lehetőségként megadhat egy **biztonsági házirend-azonosító**. Kattintson a **OK** tooadd hello ügyfél.

   ![Adja meg a bővítmény részletei][3]

## <a name="install-hello-deep-security-agent-on-an-existing-vm"></a>A meglévő virtuális hello mély biztonsági ügynök telepítése
tooinstall hello ügynök egy meglévő virtuális gépen, a következő elemek hello kell:

* hello Azure PowerShell modul, a 0.8.2 verzió vagy újabb, a helyi számítógépen. Ellenőrizheti a hello Azure PowerShell hello segítségével telepített verziójának **Get-Module azure |} táblázat formázása verzió** parancsot. Útmutatás és egy hivatkozásra toohello legújabb verzió: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). Jelentkezzen be tooyour Azure-előfizetés `Add-AzureAccount`.
* hello hello cél virtuális gépen telepített Virtuálisgép-ügynök.

Először is győződjön meg arról, hogy hello Virtuálisgép-ügynök már telepítve van. Adja meg hello felhőszolgáltatás neve és a virtuális gép nevét, és futtassa a parancsokat egy rendszergazda szintű Azure PowerShell parancssorba a következő hello. Cserélje le mindent, ami hello idézőjelek között, beleértve a hello < és > karakter.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Ha nem tudja hello felhőszolgáltatás és a virtuális gép nevét, futtassa **Get-AzureVM** toodisplay, hogy az összes információt hello virtuális gépek az aktuális előfizetésben.

Ha hello **write-host** parancs beolvasása **igaz**, hello Virtuálisgép-ügynök telepítve van. Ha a visszaadott érték **hamis**, hello utasításokat, és egy hivatkozásra toohello, töltse le a hello Azure blogbejegyzés [ügynök és Virtuálisgép-bővítmények – 2. rész](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Ha hello Virtuálisgép-ügynök telepítve van, futtassa az alábbi parancsokat.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Következő lépések
Hello ügynök toostart fut, a telepítés néhány percet vesz igénybe. Ezt követően kell tooactivate mély biztonsági hello virtuális gépen, a fiók kezelhető egy átfogó biztonsági Manager. Tekintse meg a következő cikkeket a további utasításokat hello:

* Információ a megoldásról a trend a cikk [Instant-On Cloud Security a Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
* A [Windows PowerShell-mintaparancsfájl](http://go.microsoft.com/fwlink/?LinkId=404100) tooconfigure hello virtuális gép
* [Útmutatás](http://go.microsoft.com/fwlink/?LinkId=404099) hello minta

## <a name="additional-resources"></a>További források
[Hogyan toolog Windows Server rendszert futtató tooa virtuális gépen]

[Az Azure Virtuálisgép-bővítmények és szolgáltatások]

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
[Hogyan toolog Windows Server rendszert futtató tooa virtuális gépen]:connect-logon.md
[Az Azure Virtuálisgép-bővítmények és szolgáltatások]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
