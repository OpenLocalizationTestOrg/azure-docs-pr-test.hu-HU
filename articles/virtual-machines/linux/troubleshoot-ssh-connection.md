---
title: "SSH-kapcsolat aaaTroubleshoot problémák tooan Azure virtuális gép |} Microsoft Docs"
description: "Hogyan tootroubleshoot állít ki például az \"SSH-kapcsolat sikertelen\" vagy \"Az SSH-kapcsolat elutasítva\" egy Azure virtuális gép Linux operációs rendszert futtató."
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
ms.openlocfilehash: dfb4e75e571c8306edf5f300c4e0f07a5fe7750a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-ssh-connections-tooan-azure-linux-vm-that-fails-errors-out-or-is-refused"></a><span data-ttu-id="8053d-104">SSH-kapcsolatok tooan Azure Linux virtuális Gépet, amely nem sikerül, hibák, vagy elutasítják hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="8053d-104">Troubleshoot SSH connections tooan Azure Linux VM that fails, errors out, or is refused</span></span>
<span data-ttu-id="8053d-105">Oka lehet különböző, hogy Secure Shell (SSH) hibák, az SSH-kapcsolódási hibák, vagy az SSH a rendszer elutasította a rendszer, amikor megpróbál tooconnect tooa Linux virtuális gép (VM).</span><span class="sxs-lookup"><span data-stu-id="8053d-105">There are various reasons that you encounter Secure Shell (SSH) errors, SSH connection failures, or SSH is refused when you try tooconnect tooa Linux virtual machine (VM).</span></span> <span data-ttu-id="8053d-106">Ez a cikk segít keresés és a megfelelő hello problémákat.</span><span class="sxs-lookup"><span data-stu-id="8053d-106">This article helps you find and correct hello problems.</span></span> <span data-ttu-id="8053d-107">Linux tootroubleshoot hello Azure-portálon az Azure parancssori felület vagy a Virtuálisgép-hozzáférés bővítmény használatát, és a csatlakozási problémák megoldását.</span><span class="sxs-lookup"><span data-stu-id="8053d-107">You can use hello Azure portal, Azure CLI, or VM Access Extension for Linux tootroubleshoot and resolve connection problems.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="8053d-108">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](http://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="8053d-108">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](http://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="8053d-109">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="8053d-109">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="8053d-110">Nyissa meg toohello [az Azure támogatási webhelyén](http://azure.microsoft.com/support/options/) válassza **segítségre van szüksége**.</span><span class="sxs-lookup"><span data-stu-id="8053d-110">Go toohello [Azure support site](http://azure.microsoft.com/support/options/) and select **Get support**.</span></span> <span data-ttu-id="8053d-111">Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](http://azure.microsoft.com/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="8053d-111">For information about using Azure Support, read hello [Microsoft Azure support FAQ](http://azure.microsoft.com/support/faq/).</span></span>

## <a name="quick-troubleshooting-steps"></a><span data-ttu-id="8053d-112">Első hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="8053d-112">Quick troubleshooting steps</span></span>
<span data-ttu-id="8053d-113">Hibaelhárítási lépések, után toohello VM újracsatlakozás próbálja.</span><span class="sxs-lookup"><span data-stu-id="8053d-113">After each troubleshooting step, try reconnecting toohello VM.</span></span>

1. <span data-ttu-id="8053d-114">Visszaállítja a hello SSH-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="8053d-114">Reset hello SSH configuration.</span></span>
2. <span data-ttu-id="8053d-115">Alaphelyzetbe állítja a hello hello felhasználó hitelesítő adatai.</span><span class="sxs-lookup"><span data-stu-id="8053d-115">Reset hello credentials for hello user.</span></span>
3. <span data-ttu-id="8053d-116">Ellenőrizze a hello [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) szabályok lehetővé teszik az SSH-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="8053d-116">Verify hello [Network Security Group](../../virtual-network/virtual-networks-nsg.md) rules permit SSH traffic.</span></span>
   * <span data-ttu-id="8053d-117">Győződjön meg arról, hogy létezik-e a hálózati biztonsági csoport szabály toopermit SSH-forgalom (ami alapértelmezés szerint a 22-es TCP-port).</span><span class="sxs-lookup"><span data-stu-id="8053d-117">Ensure that a Network Security Group rule exists toopermit SSH traffic (by default, TCP port 22).</span></span>
   * <span data-ttu-id="8053d-118">Nem használhat port átirányítási / leképezése egy Azure terheléselosztó használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="8053d-118">You cannot use port redirection / mapping without using an Azure load balancer.</span></span>
4. <span data-ttu-id="8053d-119">Ellenőrizze a hello [VM erőforrás állapota](../../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8053d-119">Check hello [VM resource health](../../resource-health/resource-health-overview.md).</span></span> 
   * <span data-ttu-id="8053d-120">Győződjön meg arról, hogy hello VM jelentéseket, hogy kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="8053d-120">Ensure that hello VM reports as being healthy.</span></span>
   * <span data-ttu-id="8053d-121">Ha engedélyezve van a rendszerindítási diagnosztika, győződjön meg arról, hello virtuális gép nem jelent rendszerindítási hibák hello rögzít.</span><span class="sxs-lookup"><span data-stu-id="8053d-121">If you have boot diagnostics enabled, verify hello VM is not reporting boot errors in hello logs.</span></span>
5. <span data-ttu-id="8053d-122">Indítsa újra a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="8053d-122">Restart hello VM.</span></span>
6. <span data-ttu-id="8053d-123">Telepítse újra a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="8053d-123">Redeploy hello VM.</span></span>

<span data-ttu-id="8053d-124">Részletesebb hibaelhárítási lépéseket és magyarázatokat olvasási továbbra is.</span><span class="sxs-lookup"><span data-stu-id="8053d-124">Continue reading for more detailed troubleshooting steps and explanations.</span></span>

## <a name="available-methods-tootroubleshoot-ssh-connection-issues"></a><span data-ttu-id="8053d-125">Rendelkezésre álló metódusok tootroubleshoot SSH-kapcsolódási problémák</span><span class="sxs-lookup"><span data-stu-id="8053d-125">Available methods tootroubleshoot SSH connection issues</span></span>
<span data-ttu-id="8053d-126">Új hitelesítő adatokat, vagy SSH-konfigurációt hello a következő módszerek egyikével:</span><span class="sxs-lookup"><span data-stu-id="8053d-126">You can reset credentials or SSH configuration using one of hello following methods:</span></span>

* <span data-ttu-id="8053d-127">[Azure-portálon](#use-the-azure-portal) – nagy Ha tooquickly kell hello SSH-konfigurációt vagy SSH-kulcs visszaállítása, és nem az Azure-eszközök telepítve hello.</span><span class="sxs-lookup"><span data-stu-id="8053d-127">[Azure portal](#use-the-azure-portal) - great if you need tooquickly reset hello SSH configuration or SSH key and you don't have hello Azure tools installed.</span></span>
* <span data-ttu-id="8053d-128">[Az Azure CLI 2.0](#use-the-azure-cli-20) – Ha már hello parancssorban, gyorsan alaphelyzetbe állítása hello SSH-konfigurációt vagy a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="8053d-128">[Azure CLI 2.0](#use-the-azure-cli-20) - if you are already on hello command line, quickly reset hello SSH configuration or credentials.</span></span> <span data-ttu-id="8053d-129">Is használhatja a hello [Azure CLI 1.0](#use-the-azure-cli-10)</span><span class="sxs-lookup"><span data-stu-id="8053d-129">You can also use hello [Azure CLI 1.0](#use-the-azure-cli-10)</span></span>
* <span data-ttu-id="8053d-130">[Az Azure VMAccessForLinux bővítmény](#use-the-vmaccess-extension) - létrehozása, és újra felhasználhatja json definíciós fájlok tooreset hello SSH konfigurációs vagy felhasználói hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="8053d-130">[Azure VMAccessForLinux extension](#use-the-vmaccess-extension) - create and reuse json definition files tooreset hello SSH configuration or user credentials.</span></span>

<span data-ttu-id="8053d-131">Hibaelhárítási lépések, után próbáljon tooyour VM újra.</span><span class="sxs-lookup"><span data-stu-id="8053d-131">After each troubleshooting step, try connecting tooyour VM again.</span></span> <span data-ttu-id="8053d-132">Ha továbbra sem sikerül kapcsolódni, próbálja hello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="8053d-132">If you still cannot connect, try hello next step.</span></span>

## <a name="use-hello-azure-portal"></a><span data-ttu-id="8053d-133">Hello Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="8053d-133">Use hello Azure portal</span></span>
<span data-ttu-id="8053d-134">hello Azure portál egy gyorsan tooreset hello SSH konfigurációs vagy felhasználói hitelesítő adatok nélkül biztosít bármely eszközök telepítése a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8053d-134">hello Azure portal provides a quick way tooreset hello SSH configuration or user credentials without installing any tools on your local computer.</span></span>

<span data-ttu-id="8053d-135">Válassza ki a virtuális gép hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="8053d-135">Select your VM in hello Azure portal.</span></span> <span data-ttu-id="8053d-136">Görgessen lefelé toohello **támogatási + hibaelhárítás** válassza ki azt **jelszó-átállítási** beállítani, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="8053d-136">Scroll down toohello **Support + Troubleshooting** section and select **Reset password** as in hello following example:</span></span>

![SSH-konfigurációt vagy hello Azure-portálon a hitelesítő adatok alaphelyzetbe állítása](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-hello-ssh-configuration"></a><span data-ttu-id="8053d-138">Hello SSH-konfigurációjának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="8053d-138">Reset hello SSH configuration</span></span>
<span data-ttu-id="8053d-139">Első lépésként válassza ki a `Reset configuration only` a hello **mód** legördülő menü képernyőképe, megelőző hello a kattintson a hello **alaphelyzetbe** gombra.</span><span class="sxs-lookup"><span data-stu-id="8053d-139">As a first step, select `Reset configuration only` from hello **Mode** drop-down menu as in hello preceding screenshot, then click hello **Reset** button.</span></span> <span data-ttu-id="8053d-140">Ez a művelet befejezése után próbálja tooaccess a virtuális Gépet újra.</span><span class="sxs-lookup"><span data-stu-id="8053d-140">Once this action has completed, try tooaccess your VM again.</span></span>

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="8053d-141">Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="8053d-141">Reset SSH credentials for a user</span></span>
<span data-ttu-id="8053d-142">Válassza egy meglévő felhasználó tooreset hello hitelesítő adatok `Reset SSH public key` vagy `Reset password` a hello **mód** legördülő menü képernyőképe megelőző hello hasonlóan.</span><span class="sxs-lookup"><span data-stu-id="8053d-142">tooreset hello credentials of an existing user, select either `Reset SSH public key` or `Reset password` from hello **Mode** drop-down menu as in hello preceding screenshot.</span></span> <span data-ttu-id="8053d-143">Hello felhasználónév és egy SSH-kulcs vagy új jelszót adjon meg, majd kattintson a hello **alaphelyzetbe** gombra.</span><span class="sxs-lookup"><span data-stu-id="8053d-143">Specify hello username and an SSH key or new password, then click hello **Reset** button.</span></span>

<span data-ttu-id="8053d-144">A felhasználó sudo jogosultsági szintű hello VM ebből a menüből is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="8053d-144">You can also create a user with sudo privileges on hello VM from this menu.</span></span> <span data-ttu-id="8053d-145">Adjon meg egy új felhasználónevet és a kapcsolódó jelszó vagy SSH-kulcs, és kattintson a hello **alaphelyzetbe** gombra.</span><span class="sxs-lookup"><span data-stu-id="8053d-145">Enter a new username and associated password or SSH key, and then click hello **Reset** button.</span></span>

## <a name="use-hello-azure-cli-20"></a><span data-ttu-id="8053d-146">Azure CLI 2.0 hello használata</span><span class="sxs-lookup"><span data-stu-id="8053d-146">Use hello Azure CLI 2.0</span></span>
<span data-ttu-id="8053d-147">Ha még nem tette meg, telepítse hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="8053d-147">If you haven't already, install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="8053d-148">Ha elkészült, és a feltöltött Linux egyéni lemezképet, győződjön meg arról, hogy hello [Microsoft Azure Linux ügynök](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 2.0.5 verzió vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="8053d-148">If you created and uploaded a custom Linux disk image, make sure hello [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 or later is installed.</span></span> <span data-ttu-id="8053d-149">A gyűjtemény lemezképei használatával létrehozott virtuális gép esetében a hozzáférés bővítmény már telepített és konfigurálást.</span><span class="sxs-lookup"><span data-stu-id="8053d-149">For VMs created using Gallery images, this access extension is already installed and configured for you.</span></span>

### <a name="reset-ssh-configuration"></a><span data-ttu-id="8053d-150">SSH-konfigurációjának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="8053d-150">Reset SSH configuration</span></span>
<span data-ttu-id="8053d-151">Először próbálja meg visszaállítása folyamatban hello SSH konfigurációs toodefault értékek, illetve lehet újraindítás hello SSH-kiszolgálót a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="8053d-151">You can initially try resetting hello SSH configuration toodefault values and rebooting hello SSH server on hello VM.</span></span> <span data-ttu-id="8053d-152">Vegye figyelembe, hogy ez nem módosítja hello felhasználói fiók nevét, a jelszó vagy SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="8053d-152">Note that this does not change hello user account name, password, or SSH keys.</span></span>
<span data-ttu-id="8053d-153">hello alábbi példában [az vm felhasználói alaphelyzetbe állítása-ssh](/cli/azure/vm/user#reset-ssh) tooreset hello SSH-konfigurációt a hello nevű virtuális gép `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-153">hello following example uses [az vm user reset-ssh](/cli/azure/vm/user#reset-ssh) tooreset hello SSH configuration on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="8053d-154">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8053d-154">Use your own values as follows:</span></span>

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="8053d-155">Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="8053d-155">Reset SSH credentials for a user</span></span>
<span data-ttu-id="8053d-156">hello alábbi példában [az vm felhasználói frissítése](/cli/azure/vm/user#update) tooreset hello hitelesítő adatait, `myUsername` toohello érték van megadva a `myPassword`, a hello nevű virtuális gép `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-156">hello following example uses [az vm user update](/cli/azure/vm/user#update) tooreset hello credentials for `myUsername` toohello value specified in `myPassword`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="8053d-157">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8053d-157">Use your own values as follows:</span></span>

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

<span data-ttu-id="8053d-158">Ha SSH-kulcs hitelesítést használ, alaphelyzetbe állíthatja a hello SSH-kulcs az adott felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="8053d-158">If using SSH key authentication, you can reset hello SSH key for a given user.</span></span> <span data-ttu-id="8053d-159">hello alábbi példában **az vm hozzáférés set-linux-felhasználói** tooupdate hello SSH-kulcs tárolható `~/.ssh/id_rsa.pub` nevű hello felhasználó `myUsername`, a hello nevű virtuális gép `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-159">hello following example uses **az vm access set-linux-user** tooupdate hello SSH key stored in `~/.ssh/id_rsa.pub` for hello user named `myUsername`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="8053d-160">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8053d-160">Use your own values as follows:</span></span>

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-hello-vmaccess-extension"></a><span data-ttu-id="8053d-161">Hello VMAccess bővítmény használatára</span><span class="sxs-lookup"><span data-stu-id="8053d-161">Use hello VMAccess extension</span></span>
<span data-ttu-id="8053d-162">a json-fájl, amely meghatározza a kimenő műveletek toocarry hello Linux virtuális gép hozzáférési bővítményével olvassa be. Ilyen műveletek közé tartoznak a SSHD alaphelyzetbe állítása, egy SSH-kulcsának visszaállítása vagy egy felhasználót.</span><span class="sxs-lookup"><span data-stu-id="8053d-162">hello VM Access Extension for Linux reads in a json file that defines actions toocarry out. These actions include resetting SSHD, resetting an SSH key, or adding a user.</span></span> <span data-ttu-id="8053d-163">Hello Azure CLI toocall hello VMAccess bővítmény továbbra is használhatja, de is felhasználhatja hello json-fájlok több virtuális gépek között, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="8053d-163">You still use hello Azure CLI toocall hello VMAccess extension, but you can reuse hello json files across multiple VMs if desired.</span></span> <span data-ttu-id="8053d-164">Ez a megközelítés lehetővé teszi majd hívható forgatókönyvek a megadott json-fájlok tára toocreate.</span><span class="sxs-lookup"><span data-stu-id="8053d-164">This approach allows you toocreate a repository of json files that can then be called for given scenarios.</span></span>

### <a name="reset-sshd"></a><span data-ttu-id="8053d-165">SSHD alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="8053d-165">Reset SSHD</span></span>
<span data-ttu-id="8053d-166">Hozzon létre egy fájlt `settings.json` a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="8053d-166">Create a file named `settings.json` with hello following content:</span></span>

```json
{  
    "reset_ssh":"True"
}
```

<span data-ttu-id="8053d-167">Hello Azure parancssori felület használatával, majd meghívja a hello `VMAccessForLinux` bővítmény tooreset a SSHD kapcsolat megadásával a json-fájl.</span><span class="sxs-lookup"><span data-stu-id="8053d-167">Using hello Azure CLI, you then call hello `VMAccessForLinux` extension tooreset your SSHD connection by specifying your json file.</span></span> <span data-ttu-id="8053d-168">hello alábbi példában [az virtuálisgép-bővítmény készlet](/cli/azure/vm/extension#set) tooreset SSHD hello nevű virtuális gép a `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-168">hello following example uses [az vm extension set](/cli/azure/vm/extension#set) tooreset SSHD on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="8053d-169">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8053d-169">Use your own values as follows:</span></span>

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="8053d-170">Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="8053d-170">Reset SSH credentials for a user</span></span>
<span data-ttu-id="8053d-171">Ha SSHD toofunction megfelelően jelenik meg, alaphelyzetbe állíthatja a hello előállító felhasználó hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="8053d-171">If SSHD appears toofunction correctly, you can reset hello credentials for a giver user.</span></span> <span data-ttu-id="8053d-172">egy felhasználó tooreset hello jelszavát, hozzon létre egy fájlt `settings.json`.</span><span class="sxs-lookup"><span data-stu-id="8053d-172">tooreset hello password for a user, create a file named `settings.json`.</span></span> <span data-ttu-id="8053d-173">hello alábbi példa visszaállítja hello hitelesítő adatainak `myUsername` toohello érték van megadva a `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="8053d-173">hello following example resets hello credentials for `myUsername` toohello value specified in `myPassword`.</span></span> <span data-ttu-id="8053d-174">Adja meg a következő sorokat hello a `settings.json` fájl, a saját értékekkel:</span><span class="sxs-lookup"><span data-stu-id="8053d-174">Enter hello following lines into your `settings.json` file, using your own values:</span></span>

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

<span data-ttu-id="8053d-175">Vagy a felhasználó az SSH-kulcs hello, először hozzon létre egy fájlt tooreset `settings.json`.</span><span class="sxs-lookup"><span data-stu-id="8053d-175">Or tooreset hello SSH key for a user, first create a file named `settings.json`.</span></span> <span data-ttu-id="8053d-176">hello alábbi példa visszaállítja hello hitelesítő adatainak `myUsername` toohello érték van megadva a `myPassword`, a hello nevű virtuális gép `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-176">hello following example resets hello credentials for `myUsername` toohello value specified in `myPassword`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="8053d-177">Adja meg a következő sorokat hello a `settings.json` fájl, a saját értékekkel:</span><span class="sxs-lookup"><span data-stu-id="8053d-177">Enter hello following lines into your `settings.json` file, using your own values:</span></span>

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

<span data-ttu-id="8053d-178">A json-fájl létrehozása után használja a hello Azure CLI toocall hello `VMAccessForLinux` bővítmény tooreset az SSH-felhasználó hitelesítő adatait a json-fájl megadásával.</span><span class="sxs-lookup"><span data-stu-id="8053d-178">After creating your json file, use hello Azure CLI toocall hello `VMAccessForLinux` extension tooreset your SSH user credentials by specifying your json file.</span></span> <span data-ttu-id="8053d-179">hello alábbi példa visszaállítja hello nevű VM-felhasználó hitelesítő adatai `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-179">hello following example resets credentials on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="8053d-180">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8053d-180">Use your own values as follows:</span></span>

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-hello-azure-cli-10"></a><span data-ttu-id="8053d-181">Hello Azure CLI 1.0 használata</span><span class="sxs-lookup"><span data-stu-id="8053d-181">Use hello Azure CLI 1.0</span></span>
<span data-ttu-id="8053d-182">Ha még nem tette, [hello Azure CLI 1.0 telepítése, és csatlakozzon az Azure-előfizetés tooyour](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="8053d-182">If you haven't already, [install hello Azure CLI 1.0 and connect tooyour Azure subscription](../../cli-install-nodejs.md).</span></span> <span data-ttu-id="8053d-183">Győződjön meg arról, hogy a Resource Manager módot az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="8053d-183">Make sure that you are using Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="8053d-184">Ha elkészült, és a feltöltött Linux egyéni lemezképet, győződjön meg arról, hogy hello [Microsoft Azure Linux ügynök](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 2.0.5 verzió vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="8053d-184">If you created and uploaded a custom Linux disk image, make sure hello [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 or later is installed.</span></span> <span data-ttu-id="8053d-185">A gyűjtemény lemezképei használatával létrehozott virtuális gép esetében a hozzáférés bővítmény már telepített és konfigurálást.</span><span class="sxs-lookup"><span data-stu-id="8053d-185">For VMs created using Gallery images, this access extension is already installed and configured for you.</span></span>

### <a name="reset-ssh-configuration"></a><span data-ttu-id="8053d-186">SSH-konfigurációjának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="8053d-186">Reset SSH configuration</span></span>
<span data-ttu-id="8053d-187">hello SSHD konfigurációs maga nincs megfelelően konfigurálva, vagy hello szolgáltatás hibát észlelt.</span><span class="sxs-lookup"><span data-stu-id="8053d-187">hello SSHD configuration itself may be misconfigured or hello service encountered an error.</span></span> <span data-ttu-id="8053d-188">Alaphelyzetbe állíthatja a SSHD toomake, hogy érvényes-e a saját magát hello SSH-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="8053d-188">You can reset SSHD toomake sure hello SSH configuration itself is valid.</span></span> <span data-ttu-id="8053d-189">SSHD alaphelyzetbe kell hello első hibaelhárítási lépés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="8053d-189">Resetting SSHD should be hello first troubleshooting step you take.</span></span>

<span data-ttu-id="8053d-190">hello alábbi példa visszaállítja a virtuális gép neve SSHD `myVM` nevű hello erőforráscsoportban `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-190">hello following example resets SSHD on a VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="8053d-191">A saját virtuális gép és erőforrás-neveket használja az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="8053d-191">Use your own VM and resource group names as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="8053d-192">Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="8053d-192">Reset SSH credentials for a user</span></span>
<span data-ttu-id="8053d-193">Ha SSHD toofunction megfelelően jelenik meg, hello előállító felhasználó jelszavának alaphelyzetbe állíthatja.</span><span class="sxs-lookup"><span data-stu-id="8053d-193">If SSHD appears toofunction correctly, you can reset hello password for a giver user.</span></span> <span data-ttu-id="8053d-194">hello alábbi példa visszaállítja hello hitelesítő adatainak `myUsername` toohello érték van megadva a `myPassword`, a hello nevű virtuális gép `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-194">hello following example resets hello credentials for `myUsername` toohello value specified in `myPassword`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="8053d-195">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8053d-195">Use your own values as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

<span data-ttu-id="8053d-196">Ha SSH-kulcs hitelesítést használ, alaphelyzetbe állíthatja a hello SSH-kulcs az adott felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="8053d-196">If using SSH key authentication, you can reset hello SSH key for a given user.</span></span> <span data-ttu-id="8053d-197">a következő példa frissítések hello hello SSH-kulcs tárolható `~/.ssh/id_rsa.pub` nevű hello felhasználó `myUsername`, a hello nevű virtuális gép `myVM` a `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-197">hello following example updates hello SSH key stored in `~/.ssh/id_rsa.pub` for hello user named `myUsername`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="8053d-198">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8053d-198">Use your own values as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a><span data-ttu-id="8053d-199">Virtuális gép újraindítása</span><span class="sxs-lookup"><span data-stu-id="8053d-199">Restart a VM</span></span>
<span data-ttu-id="8053d-200">Ha hello SSH konfigurációs és a felhasználói hitelesítő adatok alaphelyzetbe állítása, vagy ennek során hibába ütközött, próbálkozhat újraindítással hello VM tooaddress az alapul szolgáló számítási problémákat.</span><span class="sxs-lookup"><span data-stu-id="8053d-200">If you have reset hello SSH configuration and user credentials, or encountered an error in doing so, you can try restarting hello VM tooaddress underlying compute issues.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="8053d-201">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8053d-201">Azure portal</span></span>
<span data-ttu-id="8053d-202">a virtuális gép használatával toorestart hello Azure portál, válassza ki a virtuális Gépet, és kattintson hello **indítsa újra a** gomb, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="8053d-202">toorestart a VM using hello Azure portal, select your VM and click hello **Restart** button as in hello following example:</span></span>

![Indítsa újra a virtuális gép hello Azure-portálon](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli-10"></a><span data-ttu-id="8053d-204">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8053d-204">Azure CLI 1.0</span></span>
<span data-ttu-id="8053d-205">a következő példa újraindítást hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-205">hello following example restarts hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="8053d-206">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8053d-206">Use your own values as follows:</span></span>

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a><span data-ttu-id="8053d-207">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8053d-207">Azure CLI 2.0</span></span>
<span data-ttu-id="8053d-208">hello alábbi példában [az virtuális gép újraindítása](/cli/azure/vm#restart) toorestart hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-208">hello following example uses [az vm restart](/cli/azure/vm#restart) toorestart hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="8053d-209">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8053d-209">Use your own values as follows:</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a><span data-ttu-id="8053d-210">Virtuális gép ismételt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="8053d-210">Redeploy a VM</span></span>
<span data-ttu-id="8053d-211">Az Azure, amely előfordulhat, hogy javítsa ki a mögöttes hálózati probléma merül fel a virtuális gép tooanother csomópont központilag telepítheti.</span><span class="sxs-lookup"><span data-stu-id="8053d-211">You can redeploy a VM tooanother node within Azure, which may correct any underlying networking issues.</span></span> <span data-ttu-id="8053d-212">További információ a virtuális gép újbóli: [telepítse újra a virtuális gép toonew Azure csomópont](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8053d-212">For information about redeploying a VM, see [Redeploy virtual machine toonew Azure node](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="8053d-213">Ez a művelet befejezése után rövid élettartamú lemezen tárolt adatok ekkor elvesznek, és dinamikus IP-címek hello virtuális géphez társított frissülni fog.</span><span class="sxs-lookup"><span data-stu-id="8053d-213">After this operation finishes, ephemeral disk data will be lost and dynamic IP addresses that are associated with hello virtual machine will be updated.</span></span>
> 
> 

### <a name="azure-portal"></a><span data-ttu-id="8053d-214">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8053d-214">Azure portal</span></span>
<span data-ttu-id="8053d-215">a virtuális gép használatával tooredeploy hello Azure portál, válassza ki a virtuális Gépet, és görgessen lefelé toohello **támogatási + hibaelhárítás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8053d-215">tooredeploy a VM using hello Azure portal, select your VM and scroll down toohello **Support + Troubleshooting** section.</span></span> <span data-ttu-id="8053d-216">Kattintson a hello **újratelepíteni** gomb, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="8053d-216">Click hello **Redeploy** button as in hello following example:</span></span>

![Telepítse újra a virtuális gépek a hello Azure-portálon](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli-10"></a><span data-ttu-id="8053d-218">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8053d-218">Azure CLI 1.0</span></span>
<span data-ttu-id="8053d-219">a következő példa redeploys hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-219">hello following example redeploys hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="8053d-220">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8053d-220">Use your own values as follows:</span></span>

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a><span data-ttu-id="8053d-221">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8053d-221">Azure CLI 2.0</span></span>
<span data-ttu-id="8053d-222">használja például következő hello [az vm helyezze üzembe újra](/cli/azure/vm#redeploy) tooredeploy hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="8053d-222">hello following example use [az vm redeploy](/cli/azure/vm#redeploy) tooredeploy hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="8053d-223">A saját értékeit a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8053d-223">Use your own values as follows:</span></span>

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-hello-classic-deployment-model"></a><span data-ttu-id="8053d-224">Hello klasszikus telepítési modell használatával létrehozott virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="8053d-224">VMs created by using hello Classic deployment model</span></span>
<span data-ttu-id="8053d-225">Ezen lépések tooresolve hello leggyakoribb SSH kapcsolódási hibák próbálja hello klasszikus telepítési modell használatával létrehozott virtuális gépek esetén.</span><span class="sxs-lookup"><span data-stu-id="8053d-225">Try these steps tooresolve hello most common SSH connection failures for VMs that were created by using hello classic deployment model.</span></span> <span data-ttu-id="8053d-226">Minden lépés után toohello VM újracsatlakozás próbálja.</span><span class="sxs-lookup"><span data-stu-id="8053d-226">After each step, try reconnecting toohello VM.</span></span>

* <span data-ttu-id="8053d-227">Hello a távelérés alaphelyzetbe állítása [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8053d-227">Reset remote access from hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8053d-228">A hello Azure-portálon, válassza ki a virtuális Gépet, és kattintson hello **távoli alaphelyzetbe állítása...**  gombra.</span><span class="sxs-lookup"><span data-stu-id="8053d-228">On hello Azure portal, select your VM and click hello **Reset Remote...** button.</span></span>
* <span data-ttu-id="8053d-229">Indítsa újra a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="8053d-229">Restart hello VM.</span></span> <span data-ttu-id="8053d-230">A hello [Azure-portálon](https://portal.azure.com), válassza ki a virtuális Gépet, majd kattintson a hello **indítsa újra a** gombra.</span><span class="sxs-lookup"><span data-stu-id="8053d-230">On hello [Azure portal](https://portal.azure.com), select your VM and click hello **Restart** button.</span></span>
    
* <span data-ttu-id="8053d-231">Telepítse újra a hello VM tooa új Azure csomópont.</span><span class="sxs-lookup"><span data-stu-id="8053d-231">Redeploy hello VM tooa new Azure node.</span></span> <span data-ttu-id="8053d-232">További információ a virtuális gépek tooredeploy lásd: [telepítse újra a virtuális gép toonew Azure csomópont](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8053d-232">For information about how tooredeploy a VM, see [Redeploy virtual machine toonew Azure node](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
  
    <span data-ttu-id="8053d-233">Ez a művelet befejezése után rövid élettartamú lemezen tárolt adatok ekkor elvesznek, és dinamikus IP-címek hello virtuális géphez társított frissülni fog.</span><span class="sxs-lookup"><span data-stu-id="8053d-233">After this operation finishes, ephemeral disk data will be lost and dynamic IP addresses that are associated with hello virtual machine will be updated.</span></span>
* <span data-ttu-id="8053d-234">Hello utasításait követve [hogyan tooreset jelszó vagy SSH a Linux-alapú virtuális gépek](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) számára:</span><span class="sxs-lookup"><span data-stu-id="8053d-234">Follow hello instructions in [How tooreset a password or SSH for Linux-based virtual machines](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) to:</span></span>
  
  * <span data-ttu-id="8053d-235">Jelszó-átállítási hello vagy SSH-kulcs.</span><span class="sxs-lookup"><span data-stu-id="8053d-235">Reset hello password or SSH key.</span></span>
  * <span data-ttu-id="8053d-236">Hozzon létre egy *sudo* felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="8053d-236">Create a *sudo* user account.</span></span>
  * <span data-ttu-id="8053d-237">Visszaállítja a hello SSH-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="8053d-237">Reset hello SSH configuration.</span></span>
* <span data-ttu-id="8053d-238">A platform probléma merül fel hello VM erőforrás állapotának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="8053d-238">Check hello VM's resource health for any platform issues.</span></span><br>
     <span data-ttu-id="8053d-239">Válassza ki a virtuális Gépet, és görgessen lefelé **beállítások** > **állapotának ellenőrzése**.</span><span class="sxs-lookup"><span data-stu-id="8053d-239">Select your VM and scroll down **Settings** > **Check Health**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8053d-240">További források</span><span class="sxs-lookup"><span data-stu-id="8053d-240">Additional resources</span></span>
* <span data-ttu-id="8053d-241">Ha egy virtuális gép továbbra sem tudja tooSSH tooyour következő hello után lépéseket, tekintse meg a [részletes hibaelhárítási lépéseket](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooreview további lépések tooresolve a problémát.</span><span class="sxs-lookup"><span data-stu-id="8053d-241">If you are still unable tooSSH tooyour VM after following hello after steps, see [more detailed troubleshooting steps](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooreview additional steps tooresolve your issue.</span></span>
* <span data-ttu-id="8053d-242">Alkalmazás-hozzáférés hibaelhárítással kapcsolatos további információkért lásd: [kapcsolatos problémák elhárítása access tooan alkalmazást egy Azure virtuális gépen futó](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="8053d-242">For more information about troubleshooting application access, see [Troubleshoot access tooan application running on an Azure virtual machine](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="8053d-243">Hello klasszikus telepítési modell használatával létrehozott virtuális gépek hibaelhárítással kapcsolatos további információkért lásd: [hogyan tooreset jelszó vagy SSH a Linux-alapú virtuális gépek](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8053d-243">For more information about troubleshooting virtual machines that were created by using hello classic deployment model, see [How tooreset a password or SSH for Linux-based virtual machines](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

