---
title: "aaaReset jelszó vagy a távoli asztal konfigurálásának a virtuális gép Windows hello |} Microsoft Docs"
description: "Ismerje meg, hogyan tooreset egy fiók jelszavát, vagy egy Windows virtuális gép használata a távoli asztali szolgáltatások hello Azure-portálon vagy az Azure PowerShell."
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
ms.openlocfilehash: 5258df7196621f0adb50debd08dd248922a966de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a><span data-ttu-id="daa07-103">Hogyan tooreset hello a távoli asztal szolgáltatás vagy egy Windows virtuális gépre bejelentkezési jelszava</span><span class="sxs-lookup"><span data-stu-id="daa07-103">How tooreset hello Remote Desktop service or its login password in a Windows VM</span></span>
<span data-ttu-id="daa07-104">Ha nem sikerül tooa Windows rendszerű virtuális gép (VM), hello helyi rendszergazda jelszavát, vagy állítsa hello távoli asztal szolgáltatás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="daa07-104">If you can't connect tooa Windows virtual machine (VM), you can reset hello local administrator password or reset hello Remote Desktop service configuration.</span></span> <span data-ttu-id="daa07-105">Az Azure PowerShell tooreset hello jelszó vagy hello Azure portál vagy hello VM hozzáférési bővítményét is használhatja.</span><span class="sxs-lookup"><span data-stu-id="daa07-105">You can use either hello Azure portal or hello VM Access extension in Azure PowerShell tooreset hello password.</span></span> <span data-ttu-id="daa07-106">Ha a PowerShell használata esetén győződjön meg arról, hogy rendelkezik-e hello [legújabb PowerShell-modul telepítve és konfigurálva](/powershell/azure/overview) és tooyour Azure-előfizetés van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="daa07-106">If you are using PowerShell, make sure that you have hello [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in tooyour Azure subscription.</span></span> <span data-ttu-id="daa07-107">Emellett [hello klasszikus telepítési modellel létrehozott virtuális gépek a következő lépések](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="daa07-107">You can also [perform these steps for VMs created with hello Classic deployment model](reset-rdp.md).</span></span>

## <a name="ways-tooreset-configuration-or-credentials"></a><span data-ttu-id="daa07-108">Többféleképpen tooreset konfigurációját vagy a hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="daa07-108">Ways tooreset configuration or credentials</span></span>
<span data-ttu-id="daa07-109">Alaphelyzetbe állíthatja a távoli asztali szolgáltatások és a hitelesítő adatokat néhány más-más módon igényeitől függően:</span><span class="sxs-lookup"><span data-stu-id="daa07-109">You can reset Remote Desktop services and credentials in a few different ways, depending on your needs:</span></span>

- [<span data-ttu-id="daa07-110">Hello Azure portál segítségével vissza</span><span class="sxs-lookup"><span data-stu-id="daa07-110">Reset using hello Azure portal</span></span>](#azure-portal)
- [<span data-ttu-id="daa07-111">Alaphelyzetbe állítja az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="daa07-111">Reset using Azure PowerShell</span></span>](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a><span data-ttu-id="daa07-112">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="daa07-112">Azure portal</span></span>
<span data-ttu-id="daa07-113">tooexpand hello portál menüjében kattintson a három hello sávok hello bal felső sarokban található, és kattintson a **virtuális gépek**:</span><span class="sxs-lookup"><span data-stu-id="daa07-113">tooexpand hello portal menu, click hello three bars in hello upper left corner and then click **Virtual machines**:</span></span>

![Tallózással keresse meg az Azure virtuális gép](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-hello-local-administrator-account-password"></a><span data-ttu-id="daa07-115">**Hello helyi rendszergazdai fiók jelszavának visszaállítása**</span><span class="sxs-lookup"><span data-stu-id="daa07-115">**Reset hello local administrator account password**</span></span>

<span data-ttu-id="daa07-116">Válassza ki a Windows virtuális gépet, majd kattintson az **támogatási + hibaelhárítás** > **jelszó-átállítási**.</span><span class="sxs-lookup"><span data-stu-id="daa07-116">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="daa07-117">hello jelszó alaphelyzetbe állítása panelen jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="daa07-117">hello password reset blade is displayed:</span></span>

![Jelszó alaphelyzetbe állítása lap](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

<span data-ttu-id="daa07-119">Adja meg a hello felhasználónevet és egy új jelszót, majd kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="daa07-119">Enter hello username and a new password, then click **Update**.</span></span> <span data-ttu-id="daa07-120">Próbáljon újra kapcsolódni a virtuális gép tooyour.</span><span class="sxs-lookup"><span data-stu-id="daa07-120">Try connecting tooyour VM again.</span></span>

### <a name="reset-hello-remote-desktop-service-configuration"></a><span data-ttu-id="daa07-121">**Állítsa hello távoli asztal szolgáltatás konfigurációját**</span><span class="sxs-lookup"><span data-stu-id="daa07-121">**Reset hello Remote Desktop service configuration**</span></span>

<span data-ttu-id="daa07-122">Válassza ki a Windows virtuális gépet, majd kattintson az **támogatási + hibaelhárítás** > **jelszó-átállítási**.</span><span class="sxs-lookup"><span data-stu-id="daa07-122">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="daa07-123">hello jelszó alaphelyzetbe állítása panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="daa07-123">hello password reset blade is displayed.</span></span> 

![RDP-konfigurációjának visszaállítása](./media/reset-rdp/Portal-RM-RDP-Reset.png)

<span data-ttu-id="daa07-125">Válassza ki **alaphelyzetbe állítása konfigurációs csak** hello legördülő menüből, majd kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="daa07-125">Select **Reset configuration only** from hello drop-down menu, then click **Update**.</span></span> <span data-ttu-id="daa07-126">Próbáljon újra kapcsolódni a virtuális gép tooyour.</span><span class="sxs-lookup"><span data-stu-id="daa07-126">Try connecting tooyour VM again.</span></span>


## <a name="vmaccess-extension-and-powershell"></a><span data-ttu-id="daa07-127">VMAccess bővítmény és a PowerShell</span><span class="sxs-lookup"><span data-stu-id="daa07-127">VMAccess extension and PowerShell</span></span>
<span data-ttu-id="daa07-128">Győződjön meg arról, hogy rendelkezik-e hello [legújabb PowerShell-modul telepítve és konfigurálva](/powershell/azure/overview) és jelentkezik be Azure előfizetés hello tooyour `Login-AzureRmAccount` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="daa07-128">Make sure that you have hello [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in tooyour Azure subscription with hello `Login-AzureRmAccount` cmdlet.</span></span>

### <a name="reset-hello-local-administrator-account-password"></a><span data-ttu-id="daa07-129">**Hello helyi rendszergazdai fiók jelszavának visszaállítása**</span><span class="sxs-lookup"><span data-stu-id="daa07-129">**Reset hello local administrator account password**</span></span>
<span data-ttu-id="daa07-130">Alaphelyzetbe állítása hello rendszergazdai jelszót vagy a felhasználó nevét hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="daa07-130">Reset hello administrator password or user name with hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="daa07-131">Hozzon létre a fiók hitelesítő adatait az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="daa07-131">Create your account credentials as follows:</span></span>

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> <span data-ttu-id="daa07-132">Ha neve hello aktuális helyi rendszergazdai fiók eltér a virtuális gépen, a VMAccess bővítmény hello átnevezi hello helyi rendszergazdai fiók, rendeli hozzá a megadott jelszó toothat fiók és kibocsát egy távoli asztali kijelentkezési esemény.</span><span class="sxs-lookup"><span data-stu-id="daa07-132">If you type a different name than hello current local administrator account on your VM, hello VMAccess extension renames hello local administrator account, assigns your specified password toothat account, and issues a Remote Desktop logoff event.</span></span> <span data-ttu-id="daa07-133">A virtuális gépen hello helyi rendszergazdai fiók le van tiltva, ha a VMAccess bővítmény hello lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="daa07-133">If hello local administrator account on your VM is disabled, hello VMAccess extension enables it.</span></span>

<span data-ttu-id="daa07-134">a következő példa frissítések hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup` toohello megadott hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="daa07-134">hello following example updates hello VM named `myVM` in hello resource group named `myResourceGroup` toohello credentials specified.</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-hello-remote-desktop-service-configuration"></a><span data-ttu-id="daa07-135">**Állítsa hello távoli asztal szolgáltatás konfigurációját**</span><span class="sxs-lookup"><span data-stu-id="daa07-135">**Reset hello Remote Desktop service configuration**</span></span>
<span data-ttu-id="daa07-136">Alaphelyzetbe állítja a távelérés tooyour VM a hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="daa07-136">Reset remote access tooyour VM with hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="daa07-137">hello alábbi példa visszaállítja hello hozzáférési bővítmény nevű `myVMAccess` hello nevű virtuális gép a `myVM` a hello `myResourceGroup` erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="daa07-137">hello following example resets hello access extension named `myVMAccess` on hello VM named `myVM` in hello `myResourceGroup` resource group:</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> <span data-ttu-id="daa07-138">Bármikor a virtuális gép is van csak egyetlen virtuális gép hozzáférés ügynök.</span><span class="sxs-lookup"><span data-stu-id="daa07-138">At any point, a VM can have only a single VM access agent.</span></span> <span data-ttu-id="daa07-139">sikeresen megtörtént, hello tooset hello VM ügynök tulajdonságait `-ForceRerun` beállítás használható.</span><span class="sxs-lookup"><span data-stu-id="daa07-139">tooset hello VM access agent properties successfully, hello `-ForceRerun` option can be used.</span></span> <span data-ttu-id="daa07-140">Használata esetén `-ForceRerun`, ellenőrizze, hogy toouse hello hello VM ügynök az előző parancs alkalmazott ugyanazt a nevet.</span><span class="sxs-lookup"><span data-stu-id="daa07-140">When using `-ForceRerun`, make sure toouse hello same name for hello VM access agent as used in any previous commands.</span></span>

<span data-ttu-id="daa07-141">Ha még nem sikerül távolról tooyour virtuális gépet, tekintse meg a következő további lépéseket tootry [hibaelhárítása távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gép](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="daa07-141">If you still can't connect remotely tooyour virtual machine, see more steps tootry at [Troubleshoot Remote Desktop connections tooa Windows-based Azure virtual machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


## <a name="next-steps"></a><span data-ttu-id="daa07-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="daa07-142">Next steps</span></span>
<span data-ttu-id="daa07-143">Ha hello Azure virtuális gép hozzáférési bővítmény nem válaszol, és nem tooreset hello jelszót, akkor [hello helyi Windows jelszó-átállítási offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="daa07-143">If hello Azure VM access extension does not respond and you are unable tooreset hello password, you can [reset hello local Windows password offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="daa07-144">Ez a módszer egy speciális folyamat, és szükségessé teszi tooconnect hello virtuális merevlemezt, hello problematikus VM tooanother virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="daa07-144">This method is a more advanced process and requires you tooconnect hello virtual hard disk of hello problematic VM tooanother VM.</span></span> <span data-ttu-id="daa07-145">Kövesse a jelen cikkben leírt először hello lépéseket, és csak a hello offline jelszó alaphelyzetbe állítása módszer utolsó lehetőségként kísérlet.</span><span class="sxs-lookup"><span data-stu-id="daa07-145">Follow hello steps documented in this article first, and only attempt hello offline password reset method as a last resort.</span></span>

[<span data-ttu-id="daa07-146">Az Azure Virtuálisgép-bővítmények és szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="daa07-146">Azure VM extensions and features</span></span>](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[<span data-ttu-id="daa07-147">Csatlakozás tooan Azure virtuális gép RDP- vagy SSH</span><span class="sxs-lookup"><span data-stu-id="daa07-147">Connect tooan Azure virtual machine with RDP or SSH</span></span>](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[<span data-ttu-id="daa07-148">Távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gép hibakeresése</span><span class="sxs-lookup"><span data-stu-id="daa07-148">Troubleshoot Remote Desktop connections tooa Windows-based Azure virtual machine</span></span>](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

