---
title: "aaaReset hello jelszó vagy egy Windows Azure-ban a távoli asztal konfigurálásának |} Microsoft Docs"
description: "Ismerje meg, hogyan egy fiók jelszavát, vagy a távoli asztali szolgáltatások a Windows virtuális gép létrehozott hello klasszikus telepítési modell segítségével tooreset hello Azure-portálon vagy az Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: 1721a91fc6c89b46df74e76dfcf918b1c4c77a4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-hello-classic-deployment-model"></a><span data-ttu-id="7aeb1-103">Hogyan tooreset hello távoli asztal szolgáltatás vagy egy Windows virtuális gépre bejelentkezési jelszava létrehozott hello klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="7aeb1-103">How tooreset hello Remote Desktop service or its login password in a Windows VM created using hello Classic deployment model</span></span>
> [!IMPORTANT]
> <span data-ttu-id="7aeb1-104">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7aeb1-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="7aeb1-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="7aeb1-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="7aeb1-107">Emellett [hello Resource Manager üzembe helyezési modellben létrehozott virtuális gépek a következő lépések](../reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="7aeb1-107">You can also [perform these steps for VMs created with hello Resource Manager deployment model](../reset-rdp.md).</span></span>

<span data-ttu-id="7aeb1-108">Ha nem sikerül tooa Windows rendszerű virtuális gép (VM), hello helyi rendszergazda jelszavát, vagy állítsa hello távoli asztal szolgáltatás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-108">If you can't connect tooa Windows virtual machine (VM), you can reset hello local administrator password or reset hello Remote Desktop service configuration.</span></span> <span data-ttu-id="7aeb1-109">Az Azure PowerShell tooreset hello jelszó vagy hello Azure portál vagy hello VM hozzáférési bővítményét is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-109">You can use either hello Azure portal or hello VM Access extension in Azure PowerShell tooreset hello password.</span></span>

## <a name="ways-tooreset-configuration-or-credentials"></a><span data-ttu-id="7aeb1-110">Többféleképpen tooreset konfigurációját vagy a hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="7aeb1-110">Ways tooreset configuration or credentials</span></span>
<span data-ttu-id="7aeb1-111">Alaphelyzetbe állíthatja a távoli asztali szolgáltatások és a hitelesítő adatokat néhány más-más módon igényeitől függően:</span><span class="sxs-lookup"><span data-stu-id="7aeb1-111">You can reset Remote Desktop services and credentials in a few different ways, depending on your needs:</span></span>

- [<span data-ttu-id="7aeb1-112">Hello Azure portál segítségével vissza</span><span class="sxs-lookup"><span data-stu-id="7aeb1-112">Reset using hello Azure portal</span></span>](#azure-portal)
- [<span data-ttu-id="7aeb1-113">Alaphelyzetbe állítja az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7aeb1-113">Reset using Azure PowerShell</span></span>](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a><span data-ttu-id="7aeb1-114">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7aeb1-114">Azure portal</span></span>
<span data-ttu-id="7aeb1-115">Használhatja a hello [Azure-portálon](https://portal.azure.com) tooreset hello a távoli asztal szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-115">You can use hello [Azure portal](https://portal.azure.com) tooreset hello Remote Desktop service.</span></span> <span data-ttu-id="7aeb1-116">tooexpand hello portál menüjében kattintson a három hello sávok hello bal felső sarokban található, és kattintson a **virtuális gépek (klasszikus)**:</span><span class="sxs-lookup"><span data-stu-id="7aeb1-116">tooexpand hello portal menu, click hello three bars in hello upper left corner and then click **Virtual machines (classic)**:</span></span>

![Tallózással keresse meg az Azure virtuális gép](./media/reset-rdp/Portal-Select-Classic-VM.png)

<span data-ttu-id="7aeb1-118">Válassza ki a Windows virtuális gépet, és kattintson a **távoli alaphelyzetbe állítása...** . hello alábbi párbeszédpanel megjelenik tooreset hello távoli asztal konfigurálásának:</span><span class="sxs-lookup"><span data-stu-id="7aeb1-118">Select your Windows virtual machine and then click **Reset Remote...**. hello following dialog appears tooreset hello Remote Desktop configuration:</span></span>

![RDP-konfiguráció lapon alaphelyzetbe állítása](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

<span data-ttu-id="7aeb1-120">Hello felhasználónév és jelszó hello helyi rendszergazdai fiók is alaphelyzetbe állíthatja.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-120">You can also reset hello username and password of hello local administrator account.</span></span> <span data-ttu-id="7aeb1-121">A virtuális Gépet, kattintson **támogatási + hibaelhárítás** > **jelszó-átállítási**.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-121">From your VM, click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="7aeb1-122">hello jelszó alaphelyzetbe állítása panelen jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="7aeb1-122">hello password reset blade is displayed:</span></span>

![Jelszó alaphelyzetbe állítása lap](./media/reset-rdp/Portal-PW-Reset-Windows.png)

<span data-ttu-id="7aeb1-124">Miután megadta a hello új felhasználónevet és jelszót, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-124">After you enter hello new user name and password, click **Save**.</span></span>

## <a name="vmaccess-extension-and-powershell"></a><span data-ttu-id="7aeb1-125">VMAccess bővítmény és a PowerShell</span><span class="sxs-lookup"><span data-stu-id="7aeb1-125">VMAccess extension and PowerShell</span></span>
<span data-ttu-id="7aeb1-126">Győződjön meg arról, hogy hello hello virtuális gépen a Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-126">Make sure hello VM Agent is installed on hello virtual machine.</span></span> <span data-ttu-id="7aeb1-127">hello VMAccess bővítmény nem szükséges toobe telepítve előtt is használhatja, mindaddig, amíg hello Virtuálisgép-ügynök érhető el.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-127">hello VMAccess extension doesn't need toobe installed before you can use it, as long as hello VM Agent is available.</span></span> <span data-ttu-id="7aeb1-128">Győződjön meg arról, hogy hello Virtuálisgép-ügynök már telepítve van a következő parancs hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-128">Verify that hello VM Agent is already installed by using hello following command.</span></span> <span data-ttu-id="7aeb1-129">(Helyére "myCloudService" és "myVM" hello nevét a felhőszolgáltatás és a virtuális Gépet, illetve.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-129">(Replace "myCloudService" and "myVM" by hello names of your cloud service and your VM, respectively.</span></span> <span data-ttu-id="7aeb1-130">Ezeket a neveket is megtudhat futtatásával `Get-AzureVM` paraméter nélkül.)</span><span class="sxs-lookup"><span data-stu-id="7aeb1-130">You can learn these names by running `Get-AzureVM` without any parameters.)</span></span>

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

<span data-ttu-id="7aeb1-131">Ha hello **write-host** parancs megjeleníti **igaz**, hello Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-131">If hello **write-host** command displays **True**, hello VM Agent is installed.</span></span> <span data-ttu-id="7aeb1-132">Ha megjeleníti **hamis**, hello utasításokat, és a hivatkozás toohello, töltse le a hello [ügynök és Virtuálisgép-bővítmények – 2. rész](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blogbejegyzést.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-132">If it displays **False**, see hello instructions and a link toohello download in hello [VM Agent and Extensions - Part 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blog post.</span></span>

<span data-ttu-id="7aeb1-133">Ha a virtuális gép hello hello portál használatával hozott létre, ellenőrizze, hogy `$vm.GetInstance().ProvisionGuestAgent` adja vissza **igaz**.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-133">If you created hello virtual machine by using hello portal, check whether `$vm.GetInstance().ProvisionGuestAgent` returns **True**.</span></span> <span data-ttu-id="7aeb1-134">Ha nem, ez a parancs használatával állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="7aeb1-134">If not, you can set it by using this command:</span></span>

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

<span data-ttu-id="7aeb1-135">Ez a parancs megakadályozza, hogy a következő hiba, ha a számítógépén hello hello **Set-AzureVMExtension** hello lépések parancsot: "Rendelkezés Vendégügynök engedélyezni kell hello Virtuálisgép-objektum infrastruktúra-szolgáltatási virtuális gép hozzáférési bővítmény beállítása előtt."</span><span class="sxs-lookup"><span data-stu-id="7aeb1-135">This command prevents hello following error when you're running hello **Set-AzureVMExtension** command in hello next steps: “Provision Guest Agent must be enabled on hello VM object before setting IaaS VM Access Extension.”</span></span>

### <a name="reset-hello-local-administrator-account-password"></a><span data-ttu-id="7aeb1-136">**Hello helyi rendszergazdai fiók jelszavának visszaállítása**</span><span class="sxs-lookup"><span data-stu-id="7aeb1-136">**Reset hello local administrator account password**</span></span>
<span data-ttu-id="7aeb1-137">A bejelentkezési hitelesítő adatok létrehozása a hello aktuális helyi rendszergazda fiók nevét és egy új jelszót, és futtassa a hello `Set-AzureVMAccessExtension` az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-137">Create a sign-in credential with hello current local administrator account name and a new password, and then run hello `Set-AzureVMAccessExtension` as follows.</span></span>

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

<span data-ttu-id="7aeb1-138">Ha neve eltér a hello jelenlegi fiókot, hello VMAccess bővítmény átnevezi a helyi rendszergazdai fiók hello hello toothat fiókot hozzárendeli és kijelentkezési a távoli asztal kiállított. Ha hello helyi rendszergazdai fiók le van tiltva, a VMAccess bővítmény hello lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-138">If you type a different name than hello current account, hello VMAccess extension renames hello local administrator account, assigns hello password toothat account, and issues a Remote Desktop sign-out. If hello local administrator account is disabled, hello VMAccess extension enables it.</span></span>

<span data-ttu-id="7aeb1-139">Ezek a parancsok is alaphelyzetbe állíthatja a hello távoli asztal szolgáltatás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-139">These commands also reset hello Remote Desktop service configuration.</span></span>

### <a name="reset-hello-remote-desktop-service-configuration"></a><span data-ttu-id="7aeb1-140">**Állítsa hello távoli asztal szolgáltatás konfigurációját**</span><span class="sxs-lookup"><span data-stu-id="7aeb1-140">**Reset hello Remote Desktop service configuration**</span></span>
<span data-ttu-id="7aeb1-141">tooreset hello távoli asztal szolgáltatás konfigurációját, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="7aeb1-141">tooreset hello Remote Desktop service configuration, run hello following command:</span></span>

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

<span data-ttu-id="7aeb1-142">hello VMAccess bővítmény hello virtuális gépen két parancsot futtatja:</span><span class="sxs-lookup"><span data-stu-id="7aeb1-142">hello VMAccess extension runs two commands on hello virtual machine:</span></span>

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

<span data-ttu-id="7aeb1-143">Ez a parancs lehetővé teszi, hogy a hello beépített Windows tűzfal csoport, amely lehetővé teszi a távoli asztal bejövő forgalmat, amely a 3389-es TCP-portot használja.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-143">This command enables hello built-in Windows Firewall group that allows incoming Remote Desktop traffic, which uses TCP port 3389.</span></span>

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

<span data-ttu-id="7aeb1-144">Ez a parancs beállítja a hello fDenyTSConnections beállításjegyzékbeli érték too0, amely lehetővé teszi a távoli asztali kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-144">This command sets hello fDenyTSConnections registry value too0, enabling Remote Desktop connections.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7aeb1-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7aeb1-145">Next steps</span></span>
<span data-ttu-id="7aeb1-146">Ha hello Azure virtuális gép hozzáférési bővítmény nem válaszol, és nem tooreset hello jelszót, akkor [hello helyi Windows jelszó-átállítási offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7aeb1-146">If hello Azure VM access extension does not respond and you are unable tooreset hello password, you can [reset hello local Windows password offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="7aeb1-147">Ez a módszer egy speciális folyamat, és szükségessé teszi tooconnect hello virtuális merevlemezt, hello problematikus VM tooanother virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-147">This method is a more advanced process and requires you tooconnect hello virtual hard disk of hello problematic VM tooanother VM.</span></span> <span data-ttu-id="7aeb1-148">Kövesse a jelen cikkben leírt először hello lépéseket, és csak a hello offline jelszó alaphelyzetbe állítása módszer utolsó lehetőségként kísérlet.</span><span class="sxs-lookup"><span data-stu-id="7aeb1-148">Follow hello steps documented in this article first, and only attempt hello offline password reset method as a last resort.</span></span>

[<span data-ttu-id="7aeb1-149">Az Azure Virtuálisgép-bővítmények és szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="7aeb1-149">Azure VM extensions and features</span></span>](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[<span data-ttu-id="7aeb1-150">Csatlakozás tooan Azure virtuális gép RDP- vagy SSH</span><span class="sxs-lookup"><span data-stu-id="7aeb1-150">Connect tooan Azure virtual machine with RDP or SSH</span></span>](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[<span data-ttu-id="7aeb1-151">Távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gép hibakeresése</span><span class="sxs-lookup"><span data-stu-id="7aeb1-151">Troubleshoot Remote Desktop connections tooa Windows-based Azure virtual machine</span></span>](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

