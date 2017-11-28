---
title: "Alaphelyzetbe állítja a jelszót, vagy a távoli asztali konfigurálása a Windows virtuális gép |} Microsoft Docs"
description: "Útmutató új egy fiók jelszavát, vagy a távoli asztali szolgáltatások a Windows virtuális gép az Azure portálon vagy az Azure PowerShell használatával."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 2e002e3f336422b8fa1eceece889cd083e355a68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a><span data-ttu-id="28599-103">A távoli asztal szolgáltatás vagy egy Windows virtuális gépre a bejelentkezés jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="28599-103">How to reset the Remote Desktop service or its login password in a Windows VM</span></span>
<span data-ttu-id="28599-104">Ha nem tud csatlakozni a Windows rendszerű virtuális gép (VM), a helyi rendszergazda jelszavát, vagy állítsa a távoli asztal szolgáltatás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="28599-104">If you can't connect to a Windows virtual machine (VM), you can reset the local administrator password or reset the Remote Desktop service configuration.</span></span> <span data-ttu-id="28599-105">Használhatja az Azure-portálon vagy a virtuális gép hozzáférési bővítményét az Azure PowerShell a jelszó alaphelyzetbe állításához.</span><span class="sxs-lookup"><span data-stu-id="28599-105">You can use either the Azure portal or the VM Access extension in Azure PowerShell to reset the password.</span></span> <span data-ttu-id="28599-106">Ha a PowerShell használata esetén győződjön meg arról, hogy rendelkezik a [legújabb PowerShell-modul telepítve és konfigurálva](/powershell/azure/overview) és az Azure-előfizetéssel jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="28599-106">If you are using PowerShell, make sure that you have the [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in to your Azure subscription.</span></span> <span data-ttu-id="28599-107">Emellett [hajtsa végre ezeket a lépéseket a klasszikus telepítési modellel létrehozott virtuális gépek](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="28599-107">You can also [perform these steps for VMs created with the Classic deployment model](reset-rdp.md).</span></span>

## <a name="ways-to-reset-configuration-or-credentials"></a><span data-ttu-id="28599-108">Konfiguráció vagy a hitelesítő adatok alaphelyzetbe módjai</span><span class="sxs-lookup"><span data-stu-id="28599-108">Ways to reset configuration or credentials</span></span>
<span data-ttu-id="28599-109">Alaphelyzetbe állíthatja a távoli asztali szolgáltatások és a hitelesítő adatokat néhány más-más módon igényeitől függően:</span><span class="sxs-lookup"><span data-stu-id="28599-109">You can reset Remote Desktop services and credentials in a few different ways, depending on your needs:</span></span>

- [<span data-ttu-id="28599-110">Az Azure portál segítségével vissza</span><span class="sxs-lookup"><span data-stu-id="28599-110">Reset using the Azure portal</span></span>](#azure-portal)
- [<span data-ttu-id="28599-111">Alaphelyzetbe állítja az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="28599-111">Reset using Azure PowerShell</span></span>](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a><span data-ttu-id="28599-112">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="28599-112">Azure portal</span></span>
<span data-ttu-id="28599-113">Bontsa ki a portál menüjében, kattintson a három sávok bal felső sarokban, és kattintson a **virtuális gépek**:</span><span class="sxs-lookup"><span data-stu-id="28599-113">To expand the portal menu, click the three bars in the upper left corner and then click **Virtual machines**:</span></span>

![Tallózással keresse meg az Azure virtuális gép](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a><span data-ttu-id="28599-115">**A helyi rendszergazdai fiók jelszavának visszaállítása**</span><span class="sxs-lookup"><span data-stu-id="28599-115">**Reset the local administrator account password**</span></span>

<span data-ttu-id="28599-116">Válassza ki a Windows virtuális gépet, majd kattintson az **támogatási + hibaelhárítás** > **jelszó-átállítási**.</span><span class="sxs-lookup"><span data-stu-id="28599-116">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="28599-117">A jelszó alaphelyzetbe állítása panelen jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="28599-117">The password reset blade is displayed:</span></span>

![Jelszó alaphelyzetbe állítása lap](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

<span data-ttu-id="28599-119">Adja meg a felhasználónevet és egy új jelszót, majd kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="28599-119">Enter the username and a new password, then click **Update**.</span></span> <span data-ttu-id="28599-120">Próbáljon újra kapcsolódni a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="28599-120">Try connecting to your VM again.</span></span>

### <a name="reset-the-remote-desktop-service-configuration"></a><span data-ttu-id="28599-121">**Állítsa a távoli asztal szolgáltatás konfigurációját.**</span><span class="sxs-lookup"><span data-stu-id="28599-121">**Reset the Remote Desktop service configuration**</span></span>

<span data-ttu-id="28599-122">Válassza ki a Windows virtuális gépet, majd kattintson az **támogatási + hibaelhárítás** > **jelszó-átállítási**.</span><span class="sxs-lookup"><span data-stu-id="28599-122">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="28599-123">A jelszó alaphelyzetbe állítása panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="28599-123">The password reset blade is displayed.</span></span> 

![RDP-konfigurációjának visszaállítása](./media/reset-rdp/Portal-RM-RDP-Reset.png)

<span data-ttu-id="28599-125">Válassza ki **alaphelyzetbe állítása konfigurációs csak** a legördülő menüből, majd kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="28599-125">Select **Reset configuration only** from the drop-down menu, then click **Update**.</span></span> <span data-ttu-id="28599-126">Próbáljon újra kapcsolódni a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="28599-126">Try connecting to your VM again.</span></span>


## <a name="vmaccess-extension-and-powershell"></a><span data-ttu-id="28599-127">VMAccess bővítmény és a PowerShell</span><span class="sxs-lookup"><span data-stu-id="28599-127">VMAccess extension and PowerShell</span></span>
<span data-ttu-id="28599-128">Győződjön meg arról, hogy rendelkezik a [legújabb PowerShell-modul telepítve és konfigurálva](/powershell/azure/overview) van bejelentkezve az Azure-előfizetése és a `Login-AzureRmAccount` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="28599-128">Make sure that you have the [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in to your Azure subscription with the `Login-AzureRmAccount` cmdlet.</span></span>

### <a name="reset-the-local-administrator-account-password"></a><span data-ttu-id="28599-129">**A helyi rendszergazdai fiók jelszavának visszaállítása**</span><span class="sxs-lookup"><span data-stu-id="28599-129">**Reset the local administrator account password**</span></span>
<span data-ttu-id="28599-130">Alaphelyzetbe állítja a rendszergazdai jelszót vagy a felhasználó nevét a [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="28599-130">Reset the administrator password or user name with the [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="28599-131">Hozzon létre a fiók hitelesítő adatait az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="28599-131">Create your account credentials as follows:</span></span>

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> <span data-ttu-id="28599-132">Ha a virtuális Gépet a neve eltér az aktuális helyi rendszergazdai fiók, a VMAccess bővítmény átnevezi a helyi rendszergazdai fiók, rendeli hozzá a megadott jelszó ezekhez a fiókokhoz, és kibocsát egy távoli asztali kijelentkezési esemény.</span><span class="sxs-lookup"><span data-stu-id="28599-132">If you type a different name than the current local administrator account on your VM, the VMAccess extension renames the local administrator account, assigns your specified password to that account, and issues a Remote Desktop logoff event.</span></span> <span data-ttu-id="28599-133">Ha a helyi rendszergazdai fiókhoz az a virtuális gép le van tiltva, a VMAccess bővítmény lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="28599-133">If the local administrator account on your VM is disabled, the VMAccess extension enables it.</span></span>

<span data-ttu-id="28599-134">Az alábbi példa frissíti a virtuális gép nevű `myVM` az erőforráscsoport neve `myResourceGroup` megadott hitelesítő adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="28599-134">The following example updates the VM named `myVM` in the resource group named `myResourceGroup` to the credentials specified.</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-the-remote-desktop-service-configuration"></a><span data-ttu-id="28599-135">**Állítsa a távoli asztal szolgáltatás konfigurációját.**</span><span class="sxs-lookup"><span data-stu-id="28599-135">**Reset the Remote Desktop service configuration**</span></span>
<span data-ttu-id="28599-136">A virtuális Gépet, a távelérés alaphelyzetbe állításával a [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="28599-136">Reset remote access to your VM with the [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="28599-137">Az alábbi példa alaphelyzetbe állítja a hozzáférés bővítmény nevű `myVMAccess` nevű virtuális gépen `myVM` a a `myResourceGroup` erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="28599-137">The following example resets the access extension named `myVMAccess` on the VM named `myVM` in the `myResourceGroup` resource group:</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> <span data-ttu-id="28599-138">Bármikor a virtuális gép is van csak egyetlen virtuális gép hozzáférés ügynök.</span><span class="sxs-lookup"><span data-stu-id="28599-138">At any point, a VM can have only a single VM access agent.</span></span> <span data-ttu-id="28599-139">A virtuális gép hozzáférés ügynök tulajdonságainak beállítása sikeres legyen, a `-ForceRerun` beállítás használható.</span><span class="sxs-lookup"><span data-stu-id="28599-139">To set the VM access agent properties successfully, the `-ForceRerun` option can be used.</span></span> <span data-ttu-id="28599-140">Használata esetén `-ForceRerun`, ügyeljen arra, hogy a Virtuálisgép-hozzáférés ügynök az előző parancs alkalmazott ugyanazt a nevet használja.</span><span class="sxs-lookup"><span data-stu-id="28599-140">When using `-ForceRerun`, make sure to use the same name for the VM access agent as used in any previous commands.</span></span>

<span data-ttu-id="28599-141">Ha még nem lehet csatlakozni távolról a virtuális gép, tekintse meg a további lépéseket [távoli asztali kapcsolatainak hibaelhárítása a Windows-alapú Azure virtuális gépekhez](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="28599-141">If you still can't connect remotely to your virtual machine, see more steps to try at [Troubleshoot Remote Desktop connections to a Windows-based Azure virtual machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


## <a name="next-steps"></a><span data-ttu-id="28599-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="28599-142">Next steps</span></span>
<span data-ttu-id="28599-143">Ha az Azure virtuális gép hozzáférési bővítmény nem válaszol, és nem tudja alaphelyzetbe állítani a jelszót, akkor [a helyi Windows-jelszó alaphelyzetbe állítása offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="28599-143">If the Azure VM access extension does not respond and you are unable to reset the password, you can [reset the local Windows password offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="28599-144">Ez a módszer egy speciális folyamat, és megköveteli, hogy a virtuális merevlemez a problematikus VM kapcsolódjon egy másik virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="28599-144">This method is a more advanced process and requires you to connect the virtual hard disk of the problematic VM to another VM.</span></span> <span data-ttu-id="28599-145">Kövesse a jelen cikkben leírt első lépéseket, és csak kísérlet a kapcsolat nélküli jelszó alaphelyzetbe állítása módszer utolsó lehetőségként.</span><span class="sxs-lookup"><span data-stu-id="28599-145">Follow the steps documented in this article first, and only attempt the offline password reset method as a last resort.</span></span>

[<span data-ttu-id="28599-146">Az Azure Virtuálisgép-bővítmények és szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="28599-146">Azure VM extensions and features</span></span>](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[<span data-ttu-id="28599-147">Egy Azure virtuális gép RDP- vagy SSH kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="28599-147">Connect to an Azure virtual machine with RDP or SSH</span></span>](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[<span data-ttu-id="28599-148">A Windows-alapú Azure virtuális géphez a távoli asztali kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="28599-148">Troubleshoot Remote Desktop connections to a Windows-based Azure virtual machine</span></span>](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

