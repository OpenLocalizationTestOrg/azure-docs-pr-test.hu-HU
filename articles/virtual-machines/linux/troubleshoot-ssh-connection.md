---
title: "Egy Azure virtuális gép SSH-kapcsolati problémák elhárítása |} Microsoft Docs"
description: "Hibák elhárításához, például \"Az SSH-kapcsolat sikertelen\" vagy \"Az SSH-kapcsolat elutasítva\" egy Azure virtuális gép Linux operációs rendszert futtató módjáról."
keywords: "ssh kapcsolat visszautasította, ssh hiba és az azure-ssh, SSH-kapcsolat sikertelen"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: dcb82e19-29b2-47bb-99f2-900d4cfb5bbb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: 3a282c8b2c2ba2749de6a2d3688bd57d75703b22
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a><span data-ttu-id="9c596-104">Az Azure Linux virtuális gép, amely nem sikerül, hibák, vagy elutasítják SSH-kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="9c596-104">Troubleshoot SSH connections to an Azure Linux VM that fails, errors out, or is refused</span></span>
<span data-ttu-id="9c596-105">Oka lehet különböző, hogy Secure Shell (SSH) hibák, az SSH-kapcsolódási hibák, vagy az SSH a rendszer elutasította a rendszer, amikor egy Linux virtuális gép (VM) csatlakozni próbál.</span><span class="sxs-lookup"><span data-stu-id="9c596-105">There are various reasons that you encounter Secure Shell (SSH) errors, SSH connection failures, or SSH is refused when you try to connect to a Linux virtual machine (VM).</span></span> <span data-ttu-id="9c596-106">Ez a cikk segít keresse meg és javítsa ki a problémákat.</span><span class="sxs-lookup"><span data-stu-id="9c596-106">This article helps you find and correct the problems.</span></span> <span data-ttu-id="9c596-107">Az Azure-portálon az Azure parancssori felület vagy a Linux virtuális gép hozzáférési bővítményével hibakeresésre és problémák megoldásához használható.</span><span class="sxs-lookup"><span data-stu-id="9c596-107">You can use the Azure portal, Azure CLI, or VM Access Extension for Linux to troubleshoot and resolve connection problems.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="9c596-108">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők a [az MSDN Azure és a Stack Overflow fórumok](http://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="9c596-108">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and Stack Overflow forums](http://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="9c596-109">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="9c596-109">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="9c596-110">Lépjen a [az Azure támogatási webhelyén](http://azure.microsoft.com/support/options/) válassza **segítségre van szüksége**.</span><span class="sxs-lookup"><span data-stu-id="9c596-110">Go to the [Azure support site](http://azure.microsoft.com/support/options/) and select **Get support**.</span></span> <span data-ttu-id="9c596-111">Támogatja az Azure használatával kapcsolatos információkért olvassa el a [Microsoft Azure-támogatás – gyakori kérdések](http://azure.microsoft.com/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="9c596-111">For information about using Azure Support, read the [Microsoft Azure support FAQ](http://azure.microsoft.com/support/faq/).</span></span>

## <a name="quick-troubleshooting-steps"></a><span data-ttu-id="9c596-112">Első hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="9c596-112">Quick troubleshooting steps</span></span>
<span data-ttu-id="9c596-113">Hibaelhárítási lépések, után próbáljon újra csatlakozni a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="9c596-113">After each troubleshooting step, try reconnecting to the VM.</span></span>

1. <span data-ttu-id="9c596-114">Visszaállítja az SSH-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="9c596-114">Reset the SSH configuration.</span></span>
2. <span data-ttu-id="9c596-115">A felhasználó hitelesítő adatainak visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="9c596-115">Reset the credentials for the user.</span></span>
3. <span data-ttu-id="9c596-116">Ellenőrizze a [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) szabályok lehetővé teszik az SSH-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="9c596-116">Verify the [Network Security Group](../../virtual-network/virtual-networks-nsg.md) rules permit SSH traffic.</span></span>
   * <span data-ttu-id="9c596-117">Győződjön meg arról, hogy létezik-e a hálózati biztonsági csoport szabály az SSH-forgalom engedélyezése (ami alapértelmezés szerint a 22-es TCP-port).</span><span class="sxs-lookup"><span data-stu-id="9c596-117">Ensure that a Network Security Group rule exists to permit SSH traffic (by default, TCP port 22).</span></span>
   * <span data-ttu-id="9c596-118">Nem használhat port átirányítási / leképezése egy Azure terheléselosztó használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="9c596-118">You cannot use port redirection / mapping without using an Azure load balancer.</span></span>
4. <span data-ttu-id="9c596-119">Ellenőrizze a [VM erőforrás állapota](../../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9c596-119">Check the [VM resource health](../../resource-health/resource-health-overview.md).</span></span> 
   * <span data-ttu-id="9c596-120">Győződjön meg arról, hogy a virtuális gép jelenti, hogy kifogástalan-e.</span><span class="sxs-lookup"><span data-stu-id="9c596-120">Ensure that the VM reports as being healthy.</span></span>
   * <span data-ttu-id="9c596-121">Ha engedélyezve van a rendszerindítási diagnosztika, győződjön meg arról, a virtuális gép nem jelent a naplókban hibák.</span><span class="sxs-lookup"><span data-stu-id="9c596-121">If you have boot diagnostics enabled, verify the VM is not reporting boot errors in the logs.</span></span>
5. <span data-ttu-id="9c596-122">Indítsa újra a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="9c596-122">Restart the VM.</span></span>
6. <span data-ttu-id="9c596-123">Telepítse újra a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="9c596-123">Redeploy the VM.</span></span>

<span data-ttu-id="9c596-124">Részletesebb hibaelhárítási lépéseket és magyarázatokat olvasási továbbra is.</span><span class="sxs-lookup"><span data-stu-id="9c596-124">Continue reading for more detailed troubleshooting steps and explanations.</span></span>

## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a><span data-ttu-id="9c596-125">Rendelkezésre álló metódusok SSH-kapcsolati problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="9c596-125">Available methods to troubleshoot SSH connection issues</span></span>
<span data-ttu-id="9c596-126">Új hitelesítő adatokat, vagy SSH-konfigurációt az alábbi módszerek egyikének használatával:</span><span class="sxs-lookup"><span data-stu-id="9c596-126">You can reset credentials or SSH configuration using one of the following methods:</span></span>

* <span data-ttu-id="9c596-127">[Azure-portálon](#use-the-azure-portal) - nagyszerű, ha az SSH-konfigurációt vagy SSH-kulcs gyorsan alaphelyzetbe kell, és nem kell az Azure-eszközök telepítve.</span><span class="sxs-lookup"><span data-stu-id="9c596-127">[Azure portal](#use-the-azure-portal) - great if you need to quickly reset the SSH configuration or SSH key and you don't have the Azure tools installed.</span></span>
* <span data-ttu-id="9c596-128">[Az Azure CLI 2.0](#use-the-azure-cli-20) – Ha már a parancssorban, gyorsan alaphelyzetbe az SSH-konfigurációt vagy a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="9c596-128">[Azure CLI 2.0](#use-the-azure-cli-20) - if you are already on the command line, quickly reset the SSH configuration or credentials.</span></span> <span data-ttu-id="9c596-129">Használhatja a [Azure CLI 1.0](#use-the-azure-cli-10)</span><span class="sxs-lookup"><span data-stu-id="9c596-129">You can also use the [Azure CLI 1.0](#use-the-azure-cli-10)</span></span>
* <span data-ttu-id="9c596-130">[Az Azure VMAccessForLinux bővítmény](#use-the-vmaccess-extension) – hozzon létre, és újra felhasználhatja a json-definíciós fájlokat, és az SSH konfigurációs vagy felhasználói hitelesítő adatok alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="9c596-130">[Azure VMAccessForLinux extension](#use-the-vmaccess-extension) - create and reuse json definition files to reset the SSH configuration or user credentials.</span></span>

<span data-ttu-id="9c596-131">Minden hibaelhárítási lépés után ismét kapcsolódni a virtuális Gépre próbálja.</span><span class="sxs-lookup"><span data-stu-id="9c596-131">After each troubleshooting step, try connecting to your VM again.</span></span> <span data-ttu-id="9c596-132">Ha továbbra sem sikerül kapcsolódni, próbálja meg a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="9c596-132">If you still cannot connect, try the next step.</span></span>

## <a name="use-the-azure-portal"></a><span data-ttu-id="9c596-133">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="9c596-133">Use the Azure portal</span></span>
<span data-ttu-id="9c596-134">Az Azure-portál a helyi számítógépen bármely eszközök telepítése nélkül a SSH vagy felhasználói hitelesítő adatok alaphelyzetbe állítása gyors lehetőséget kínál.</span><span class="sxs-lookup"><span data-stu-id="9c596-134">The Azure portal provides a quick way to reset the SSH configuration or user credentials without installing any tools on your local computer.</span></span>

<span data-ttu-id="9c596-135">Válassza ki a virtuális Gépet az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="9c596-135">Select your VM in the Azure portal.</span></span> <span data-ttu-id="9c596-136">Görgessen le a **támogatási + hibaelhárítás** válassza ki azt **jelszó-átállítási** az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="9c596-136">Scroll down to the **Support + Troubleshooting** section and select **Reset password** as in the following example:</span></span>

![Új SSH-konfigurációt vagy a hitelesítő adatokat az Azure-portálon](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a><span data-ttu-id="9c596-138">Az SSH-konfigurációjának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="9c596-138">Reset the SSH configuration</span></span>
<span data-ttu-id="9c596-139">Első lépésként válassza ki a `Reset configuration only` a a **mód** legördülő menü, az előző képernyőképet, majd kattintson a **alaphelyzetbe** gombra.</span><span class="sxs-lookup"><span data-stu-id="9c596-139">As a first step, select `Reset configuration only` from the **Mode** drop-down menu as in the preceding screenshot, then click the **Reset** button.</span></span> <span data-ttu-id="9c596-140">Ez a művelet befejezése után próbálja meg ismét elérni a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="9c596-140">Once this action has completed, try to access your VM again.</span></span>

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="9c596-141">Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="9c596-141">Reset SSH credentials for a user</span></span>
<span data-ttu-id="9c596-142">Válassza a hitelesítő adatokat egy meglévő felhasználó alaphelyzetbe állításához `Reset SSH public key` vagy `Reset password` a a **mód** legördülő menü, ahogy az előző képernyőképet.</span><span class="sxs-lookup"><span data-stu-id="9c596-142">To reset the credentials of an existing user, select either `Reset SSH public key` or `Reset password` from the **Mode** drop-down menu as in the preceding screenshot.</span></span> <span data-ttu-id="9c596-143">Adja meg a felhasználónevet és egy SSH-kulcs vagy új jelszót, majd kattintson a **alaphelyzetbe** gombra.</span><span class="sxs-lookup"><span data-stu-id="9c596-143">Specify the username and an SSH key or new password, then click the **Reset** button.</span></span>

<span data-ttu-id="9c596-144">A felhasználó sudo jogosultsági szintű ebben a menüben a virtuális gép is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="9c596-144">You can also create a user with sudo privileges on the VM from this menu.</span></span> <span data-ttu-id="9c596-145">Adjon meg egy új felhasználónevet és a kapcsolódó jelszó vagy SSH-kulcs, és kattintson a **alaphelyzetbe** gombra.</span><span class="sxs-lookup"><span data-stu-id="9c596-145">Enter a new username and associated password or SSH key, and then click the **Reset** button.</span></span>

## <a name="use-the-azure-cli-20"></a><span data-ttu-id="9c596-146">Az Azure parancssori felület használatával 2.0</span><span class="sxs-lookup"><span data-stu-id="9c596-146">Use the Azure CLI 2.0</span></span>
<span data-ttu-id="9c596-147">Ha még nem tette meg, telepítse a legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) és való bejelentkezéshez az Azure fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="9c596-147">If you haven't already, install the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="9c596-148">Ha létrehozott és Linux egyéni lemezképet feltöltve, ellenőrizze, hogy a [Microsoft Azure Linux ügynök](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 2.0.5 verzió vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="9c596-148">If you created and uploaded a custom Linux disk image, make sure the [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 or later is installed.</span></span> <span data-ttu-id="9c596-149">A gyűjtemény lemezképei használatával létrehozott virtuális gép esetében a hozzáférés bővítmény már telepített és konfigurálást.</span><span class="sxs-lookup"><span data-stu-id="9c596-149">For VMs created using Gallery images, this access extension is already installed and configured for you.</span></span>

### <a name="reset-ssh-configuration"></a><span data-ttu-id="9c596-150">SSH-konfigurációjának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="9c596-150">Reset SSH configuration</span></span>
<span data-ttu-id="9c596-151">Először is próbálja meg az SSH-konfigurációjának visszaállítása az alapértelmezett értékekre, és az SSH-kiszolgáló a virtuális gép újraindul.</span><span class="sxs-lookup"><span data-stu-id="9c596-151">You can initially try resetting the SSH configuration to default values and rebooting the SSH server on the VM.</span></span> <span data-ttu-id="9c596-152">Vegye figyelembe, hogy ez nem módosítja a felhasználói fiók nevét, jelszó vagy SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="9c596-152">Note that this does not change the user account name, password, or SSH keys.</span></span>
<span data-ttu-id="9c596-153">Az alábbi példában [az vm felhasználói alaphelyzetbe állítása-ssh](/cli/azure/vm/user#reset-ssh) nevű virtuális gép SSH-konfigurációjának visszaállítása `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-153">The following example uses [az vm user reset-ssh](/cli/azure/vm/user#reset-ssh) to reset the SSH configuration on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="9c596-154">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c596-154">Use your own values as follows:</span></span>

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="9c596-155">Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="9c596-155">Reset SSH credentials for a user</span></span>
<span data-ttu-id="9c596-156">Az alábbi példában [az vm felhasználói frissítése](/cli/azure/vm/user#update) alaphelyzetbe állítani a hitelesítő adatokat a `myUsername` a megadott értékre `myPassword`, a nevű virtuális Gépre `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-156">The following example uses [az vm user update](/cli/azure/vm/user#update) to reset the credentials for `myUsername` to the value specified in `myPassword`, on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="9c596-157">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c596-157">Use your own values as follows:</span></span>

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

<span data-ttu-id="9c596-158">Ha SSH-kulcs hitelesítést használ, visszaállíthatja az SSH-kulcs az adott felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="9c596-158">If using SSH key authentication, you can reset the SSH key for a given user.</span></span> <span data-ttu-id="9c596-159">Az alábbi példában **az vm hozzáférés set-linux-felhasználói** frissíteni az SSH-kulcs tárolható `~/.ssh/id_rsa.pub` nevű felhasználó `myUsername`, a nevű virtuális Gépre `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-159">The following example uses **az vm access set-linux-user** to update the SSH key stored in `~/.ssh/id_rsa.pub` for the user named `myUsername`, on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="9c596-160">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c596-160">Use your own values as follows:</span></span>

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-the-vmaccess-extension"></a><span data-ttu-id="9c596-161">Használja a VMAccess bővítmény</span><span class="sxs-lookup"><span data-stu-id="9c596-161">Use the VMAccess extension</span></span>
<span data-ttu-id="9c596-162">A Linux virtuális gép hozzáférési bővítményével beolvassa a json-fájl, amely meghatározza a műveletek elvégzéséhez. Ilyen műveletek közé tartoznak a SSHD alaphelyzetbe állítása, egy SSH-kulcsának visszaállítása vagy egy felhasználót.</span><span class="sxs-lookup"><span data-stu-id="9c596-162">The VM Access Extension for Linux reads in a json file that defines actions to carry out. These actions include resetting SSHD, resetting an SSH key, or adding a user.</span></span> <span data-ttu-id="9c596-163">Továbbra is használhatja az Azure parancssori felület a VMAccess bővítmény hívására, de is felhasználhatja a json-fájlok több virtuális gépek között, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="9c596-163">You still use the Azure CLI to call the VMAccess extension, but you can reuse the json files across multiple VMs if desired.</span></span> <span data-ttu-id="9c596-164">Ez a megközelítés lehetővé teszi majd hívható forgatókönyvek a megadott json-fájlokat egy gyűjteményének létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9c596-164">This approach allows you to create a repository of json files that can then be called for given scenarios.</span></span>

### <a name="reset-sshd"></a><span data-ttu-id="9c596-165">SSHD alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="9c596-165">Reset SSHD</span></span>
<span data-ttu-id="9c596-166">Hozzon létre egy fájlt `settings.json` a következő tartalommal:</span><span class="sxs-lookup"><span data-stu-id="9c596-166">Create a file named `settings.json` with the following content:</span></span>

```json
{  
    "reset_ssh":"True"
}
```

<span data-ttu-id="9c596-167">Az Azure parancssori felület használatával, majd meghívja a `VMAccessForLinux` bővítmény a SSHD kapcsolat alaphelyzetbe állítására a json-fájl megadásával.</span><span class="sxs-lookup"><span data-stu-id="9c596-167">Using the Azure CLI, you then call the `VMAccessForLinux` extension to reset your SSHD connection by specifying your json file.</span></span> <span data-ttu-id="9c596-168">Az alábbi példában [az virtuálisgép-bővítmény készlet](/cli/azure/vm/extension#set) alaphelyzetbe állítja a nevű virtuális Gépre SSHD `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-168">The following example uses [az vm extension set](/cli/azure/vm/extension#set) to reset SSHD on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="9c596-169">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c596-169">Use your own values as follows:</span></span>

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="9c596-170">Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="9c596-170">Reset SSH credentials for a user</span></span>
<span data-ttu-id="9c596-171">SSHD úgy tűnik, hogy megfelelően működik, ha alaphelyzetbe állíthatja a előállító felhasználó hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="9c596-171">If SSHD appears to function correctly, you can reset the credentials for a giver user.</span></span> <span data-ttu-id="9c596-172">A felhasználó jelszavának alaphelyzetbe állításához hozzon létre egy fájlt `settings.json`.</span><span class="sxs-lookup"><span data-stu-id="9c596-172">To reset the password for a user, create a file named `settings.json`.</span></span> <span data-ttu-id="9c596-173">Az alábbi példában a hitelesítő adatok alaphelyzetbe állítása `myUsername` a megadott értékre `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="9c596-173">The following example resets the credentials for `myUsername` to the value specified in `myPassword`.</span></span> <span data-ttu-id="9c596-174">Adja meg a következő sorokat a `settings.json` fájl, a saját értékekkel:</span><span class="sxs-lookup"><span data-stu-id="9c596-174">Enter the following lines into your `settings.json` file, using your own values:</span></span>

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

<span data-ttu-id="9c596-175">Az SSH-kulcs egy felhasználó alaphelyzetbe állításához először hozzon létre egy fájlt vagy `settings.json`.</span><span class="sxs-lookup"><span data-stu-id="9c596-175">Or to reset the SSH key for a user, first create a file named `settings.json`.</span></span> <span data-ttu-id="9c596-176">Az alábbi példában a hitelesítő adatok alaphelyzetbe állítása `myUsername` a megadott értékre `myPassword`, a virtuális Gépre nevű `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-176">The following example resets the credentials for `myUsername` to the value specified in `myPassword`, on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="9c596-177">Adja meg a következő sorokat a `settings.json` fájl, a saját értékekkel:</span><span class="sxs-lookup"><span data-stu-id="9c596-177">Enter the following lines into your `settings.json` file, using your own values:</span></span>

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

<span data-ttu-id="9c596-178">A json-fájl létrehozása után használja az Azure parancssori felület hívásához a `VMAccessForLinux` állítsa alaphelyzetbe az SSH hitelesítő adatait adja meg a json-fájl kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="9c596-178">After creating your json file, use the Azure CLI to call the `VMAccessForLinux` extension to reset your SSH user credentials by specifying your json file.</span></span> <span data-ttu-id="9c596-179">Az alábbi példa alaphelyzetbe állítja a virtuális gép nevű hitelesítő adatok `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-179">The following example resets credentials on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="9c596-180">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c596-180">Use your own values as follows:</span></span>

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-the-azure-cli-10"></a><span data-ttu-id="9c596-181">Az Azure parancssori felület használatával 1.0</span><span class="sxs-lookup"><span data-stu-id="9c596-181">Use the Azure CLI 1.0</span></span>
<span data-ttu-id="9c596-182">Ha még nem tette, [telepítse az Azure CLI 1.0, és csatlakozzon az Azure-előfizetéshez](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="9c596-182">If you haven't already, [install the Azure CLI 1.0 and connect to your Azure subscription](../../cli-install-nodejs.md).</span></span> <span data-ttu-id="9c596-183">Győződjön meg arról, hogy a Resource Manager módot az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9c596-183">Make sure that you are using Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="9c596-184">Ha létrehozott és Linux egyéni lemezképet feltöltve, ellenőrizze, hogy a [Microsoft Azure Linux ügynök](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 2.0.5 verzió vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="9c596-184">If you created and uploaded a custom Linux disk image, make sure the [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 or later is installed.</span></span> <span data-ttu-id="9c596-185">A gyűjtemény lemezképei használatával létrehozott virtuális gép esetében a hozzáférés bővítmény már telepített és konfigurálást.</span><span class="sxs-lookup"><span data-stu-id="9c596-185">For VMs created using Gallery images, this access extension is already installed and configured for you.</span></span>

### <a name="reset-ssh-configuration"></a><span data-ttu-id="9c596-186">SSH-konfigurációjának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="9c596-186">Reset SSH configuration</span></span>
<span data-ttu-id="9c596-187">Nincs megfelelően konfigurálva a SSHD konfigurációs magát, vagy a szolgáltatás hibát észlelt.</span><span class="sxs-lookup"><span data-stu-id="9c596-187">The SSHD configuration itself may be misconfigured or the service encountered an error.</span></span> <span data-ttu-id="9c596-188">Ellenőrizze, hogy az SSH-konfigurációt maga érvényes SSHD alaphelyzetbe állíthatja.</span><span class="sxs-lookup"><span data-stu-id="9c596-188">You can reset SSHD to make sure the SSH configuration itself is valid.</span></span> <span data-ttu-id="9c596-189">SSHD alaphelyzetbe kell lennie az első hibaelhárítási lépés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="9c596-189">Resetting SSHD should be the first troubleshooting step you take.</span></span>

<span data-ttu-id="9c596-190">Az alábbi példa alaphelyzetbe állítja a virtuális gép neve SSHD `myVM` az erőforráscsoport neve `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-190">The following example resets SSHD on a VM named `myVM` in the resource group named `myResourceGroup`.</span></span> <span data-ttu-id="9c596-191">A saját virtuális gép és erőforrás-neveket használja az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9c596-191">Use your own VM and resource group names as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="9c596-192">Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="9c596-192">Reset SSH credentials for a user</span></span>
<span data-ttu-id="9c596-193">SSHD úgy tűnik, hogy megfelelően működik, ha az előállító felhasználó jelszavának alaphelyzetbe állíthatja.</span><span class="sxs-lookup"><span data-stu-id="9c596-193">If SSHD appears to function correctly, you can reset the password for a giver user.</span></span> <span data-ttu-id="9c596-194">Az alábbi példában a hitelesítő adatok alaphelyzetbe állítása `myUsername` a megadott értékre `myPassword`, a virtuális Gépre nevű `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-194">The following example resets the credentials for `myUsername` to the value specified in `myPassword`, on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="9c596-195">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c596-195">Use your own values as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

<span data-ttu-id="9c596-196">Ha SSH-kulcs hitelesítést használ, visszaállíthatja az SSH-kulcs az adott felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="9c596-196">If using SSH key authentication, you can reset the SSH key for a given user.</span></span> <span data-ttu-id="9c596-197">Az alábbi példa frissíti az SSH-kulcs tárolható `~/.ssh/id_rsa.pub` nevű felhasználó `myUsername`, a nevű virtuális Gépre `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-197">The following example updates the SSH key stored in `~/.ssh/id_rsa.pub` for the user named `myUsername`, on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="9c596-198">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c596-198">Use your own values as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a><span data-ttu-id="9c596-199">Virtuális gép újraindítása</span><span class="sxs-lookup"><span data-stu-id="9c596-199">Restart a VM</span></span>
<span data-ttu-id="9c596-200">Ha az SSH konfigurációs és a felhasználói hitelesítő adatok alaphelyzetbe állítása, vagy ennek során hibába ütközött, próbálkozhat újraindítással a virtuális gép az alapul szolgáló számítási problémák címre.</span><span class="sxs-lookup"><span data-stu-id="9c596-200">If you have reset the SSH configuration and user credentials, or encountered an error in doing so, you can try restarting the VM to address underlying compute issues.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="9c596-201">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9c596-201">Azure portal</span></span>
<span data-ttu-id="9c596-202">Indítsa újra a virtuális gépek az Azure portál használatával, jelölje ki a virtuális Gépet, majd kattintson a **indítsa újra a** gombra az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="9c596-202">To restart a VM using the Azure portal, select your VM and click the **Restart** button as in the following example:</span></span>

![Indítsa újra a virtuális gépek Azure-portálon](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli-10"></a><span data-ttu-id="9c596-204">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9c596-204">Azure CLI 1.0</span></span>
<span data-ttu-id="9c596-205">Az alábbi példa nevű virtuális gép újraindul `myVM` az erőforráscsoport neve `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-205">The following example restarts the VM named `myVM` in the resource group named `myResourceGroup`.</span></span> <span data-ttu-id="9c596-206">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c596-206">Use your own values as follows:</span></span>

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a><span data-ttu-id="9c596-207">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9c596-207">Azure CLI 2.0</span></span>
<span data-ttu-id="9c596-208">Az alábbi példában [az virtuális gép újraindítása](/cli/azure/vm#restart) nevű virtuális gép újraindítására `myVM` az erőforráscsoport neve `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-208">The following example uses [az vm restart](/cli/azure/vm#restart) to restart the VM named `myVM` in the resource group named `myResourceGroup`.</span></span> <span data-ttu-id="9c596-209">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c596-209">Use your own values as follows:</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a><span data-ttu-id="9c596-210">Virtuális gép ismételt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="9c596-210">Redeploy a VM</span></span>
<span data-ttu-id="9c596-211">Egy virtuális Gépet az Azure, amely előfordulhat, hogy javítsa ki a mögöttes hálózati probléma merül fel egy másik csomópontra telepítheti.</span><span class="sxs-lookup"><span data-stu-id="9c596-211">You can redeploy a VM to another node within Azure, which may correct any underlying networking issues.</span></span> <span data-ttu-id="9c596-212">További információ a virtuális gép újbóli: [újratelepíteni a virtuális gépet az új Azure csomópont](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9c596-212">For information about redeploying a VM, see [Redeploy virtual machine to new Azure node](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="9c596-213">Ez a művelet befejezése után rövid élettartamú lemezen tárolt adatok ekkor elvesznek, és dinamikus IP-címek, amelyek a virtuális géphez kapcsolódó frissülni fog.</span><span class="sxs-lookup"><span data-stu-id="9c596-213">After this operation finishes, ephemeral disk data will be lost and dynamic IP addresses that are associated with the virtual machine will be updated.</span></span>
> 
> 

### <a name="azure-portal"></a><span data-ttu-id="9c596-214">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9c596-214">Azure portal</span></span>
<span data-ttu-id="9c596-215">Újratelepíteni a virtuális gépek az Azure portál használatával, jelölje ki a virtuális Gépet, és görgessen le a **támogatási + hibaelhárítás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9c596-215">To redeploy a VM using the Azure portal, select your VM and scroll down to the **Support + Troubleshooting** section.</span></span> <span data-ttu-id="9c596-216">Kattintson a **újratelepíteni** gombra az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="9c596-216">Click the **Redeploy** button as in the following example:</span></span>

![Telepítse újra a virtuális gépek Azure-portálon](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli-10"></a><span data-ttu-id="9c596-218">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9c596-218">Azure CLI 1.0</span></span>
<span data-ttu-id="9c596-219">Az alábbi példa redeploys nevű virtuális gép `myVM` az erőforráscsoport neve `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-219">The following example redeploys the VM named `myVM` in the resource group named `myResourceGroup`.</span></span> <span data-ttu-id="9c596-220">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c596-220">Use your own values as follows:</span></span>

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a><span data-ttu-id="9c596-221">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9c596-221">Azure CLI 2.0</span></span>
<span data-ttu-id="9c596-222">A következő használja például [az vm helyezze üzembe újra](/cli/azure/vm#redeploy) újratelepíteni a virtuális gép nevű `myVM` az erőforráscsoport neve `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="9c596-222">The following example use [az vm redeploy](/cli/azure/vm#redeploy) to redeploy the VM named `myVM` in the resource group named `myResourceGroup`.</span></span> <span data-ttu-id="9c596-223">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c596-223">Use your own values as follows:</span></span>

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a><span data-ttu-id="9c596-224">A klasszikus telepítési modell használatával létrehozott virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="9c596-224">VMs created by using the Classic deployment model</span></span>
<span data-ttu-id="9c596-225">Próbálja meg ezeket a lépéseket a klasszikus üzembe helyezési modell használatával létrehozott virtuális gépek a leggyakrabban használt SSH-kapcsolódási hibák megoldására.</span><span class="sxs-lookup"><span data-stu-id="9c596-225">Try these steps to resolve the most common SSH connection failures for VMs that were created by using the classic deployment model.</span></span> <span data-ttu-id="9c596-226">Minden lépés után próbáljon újra csatlakozni a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="9c596-226">After each step, try reconnecting to the VM.</span></span>

* <span data-ttu-id="9c596-227">A távelérés alaphelyzetbe állítása a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9c596-227">Reset remote access from the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9c596-228">Az Azure portálon, válassza ki a virtuális Gépet, majd kattintson a **távoli alaphelyzetbe állítása...**  gombra.</span><span class="sxs-lookup"><span data-stu-id="9c596-228">On the Azure portal, select your VM and click the **Reset Remote...** button.</span></span>
* <span data-ttu-id="9c596-229">Indítsa újra a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="9c596-229">Restart the VM.</span></span> <span data-ttu-id="9c596-230">Az a [Azure-portálon](https://portal.azure.com), jelölje ki a virtuális Gépet, és kattintson a **indítsa újra a** gombra.</span><span class="sxs-lookup"><span data-stu-id="9c596-230">On the [Azure portal](https://portal.azure.com), select your VM and click the **Restart** button.</span></span>
    
* <span data-ttu-id="9c596-231">Telepítse újra a virtuális Gépet egy új Azure csomópontra.</span><span class="sxs-lookup"><span data-stu-id="9c596-231">Redeploy the VM to a new Azure node.</span></span> <span data-ttu-id="9c596-232">Telepítse újra a virtuális gépek kapcsolatos információkért lásd: [újratelepíteni a virtuális gépet az új Azure csomópont](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9c596-232">For information about how to redeploy a VM, see [Redeploy virtual machine to new Azure node](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
  
    <span data-ttu-id="9c596-233">Ez a művelet befejezése után rövid élettartamú lemezen tárolt adatok ekkor elvesznek, és dinamikus IP-címek, amelyek a virtuális géphez kapcsolódó frissülni fog.</span><span class="sxs-lookup"><span data-stu-id="9c596-233">After this operation finishes, ephemeral disk data will be lost and dynamic IP addresses that are associated with the virtual machine will be updated.</span></span>
* <span data-ttu-id="9c596-234">Kövesse az utasításokat a [alaphelyzetbe állításával a jelszó vagy SSH a Linux-alapú virtuális gépek](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) számára:</span><span class="sxs-lookup"><span data-stu-id="9c596-234">Follow the instructions in [How to reset a password or SSH for Linux-based virtual machines](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) to:</span></span>
  
  * <span data-ttu-id="9c596-235">A jelszó vagy SSH-kulcs visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="9c596-235">Reset the password or SSH key.</span></span>
  * <span data-ttu-id="9c596-236">Hozzon létre egy *sudo* felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="9c596-236">Create a *sudo* user account.</span></span>
  * <span data-ttu-id="9c596-237">Visszaállítja az SSH-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="9c596-237">Reset the SSH configuration.</span></span>
* <span data-ttu-id="9c596-238">Ellenőrizze a VM erőforrás állapota a platform probléma merül fel.</span><span class="sxs-lookup"><span data-stu-id="9c596-238">Check the VM's resource health for any platform issues.</span></span><br>
     <span data-ttu-id="9c596-239">Válassza ki a virtuális Gépet, és görgessen lefelé **beállítások** > **állapotának ellenőrzése**.</span><span class="sxs-lookup"><span data-stu-id="9c596-239">Select your VM and scroll down **Settings** > **Check Health**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c596-240">További források</span><span class="sxs-lookup"><span data-stu-id="9c596-240">Additional resources</span></span>
* <span data-ttu-id="9c596-241">Ha SSH továbbra sem tudja a virtuális géphez után lépések végrehajtása után, lásd: [részletes hibaelhárítási lépéseket](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) való tekintse át a probléma megoldásához szükséges további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9c596-241">If you are still unable to SSH to your VM after following the after steps, see [more detailed troubleshooting steps](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to review additional steps to resolve your issue.</span></span>
* <span data-ttu-id="9c596-242">Alkalmazás-hozzáférés hibaelhárítással kapcsolatos további információkért lásd: [egy Azure virtuális gépen futó alkalmazáshoz való hozzáférés hibáinak elhárítása](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="9c596-242">For more information about troubleshooting application access, see [Troubleshoot access to an application running on an Azure virtual machine](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="9c596-243">A klasszikus üzembe helyezési modell használatával létrehozott virtuális gépek hibaelhárítással kapcsolatos további információkért lásd: [alaphelyzetbe állításával a jelszó vagy SSH a Linux-alapú virtuális gépek](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9c596-243">For more information about troubleshooting virtual machines that were created by using the classic deployment model, see [How to reset a password or SSH for Linux-based virtual machines](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

