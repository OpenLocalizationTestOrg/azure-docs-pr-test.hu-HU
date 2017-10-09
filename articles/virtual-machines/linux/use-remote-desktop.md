---
title: "aaaUse távoli asztal tooa Linux virtuális gép az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és a távoli asztal (xrdp) tooconnect tooa Linux virtuális gép konfigurálása az Azure-grafikus eszközök használatával"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: iainfou
ms.openlocfilehash: 64d30be101ceeb49fc05bb10293ad63db358efe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-remote-desktop-tooconnect-tooa-linux-vm-in-azure"></a><span data-ttu-id="fc6a6-103">Telepítse és konfigurálja a távoli asztal tooconnect tooa Linux virtuális gép az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="fc6a6-103">Install and configure Remote Desktop tooconnect tooa Linux VM in Azure</span></span>
<span data-ttu-id="fc6a6-104">Secure shell (SSH-) kapcsolaton keresztül hello parancssorból általában felügyelt Linux virtuális gépek (VM) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-104">Linux virtual machines (VMs) in Azure are usually managed from hello command line using a secure shell (SSH) connection.</span></span> <span data-ttu-id="fc6a6-105">Amikor új tooLinux vagy gyors hibaelhárítási forgatókönyvek esetén hello használata a távoli asztal könnyebb lehet.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-105">When new tooLinux, or for quick troubleshooting scenarios, hello use of remote desktop may be easier.</span></span> <span data-ttu-id="fc6a6-106">Ez a következő cikket: részletek hogyan tooinstall és egy asztali környezet konfigurálása ([xfce](https://www.xfce.org)) és a távoli asztal ([xrdp](http://www.xrdp.org)) a Linux virtuális gép hello Resource Manager üzembe helyezési modellben a.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-106">This article details how tooinstall and configure a desktop environment ([xfce](https://www.xfce.org)) and remote desktop ([xrdp](http://www.xrdp.org)) for your Linux VM using hello Resource Manager deployment model.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="fc6a6-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fc6a6-107">Prerequisites</span></span>
<span data-ttu-id="fc6a6-108">Ez a cikk az Azure-ban meglévő Linux virtuális gép szükséges.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-108">This article requires an existing Linux VM in Azure.</span></span> <span data-ttu-id="fc6a6-109">Ha egy virtuális gép toocreate van szüksége, használja a következő módszerek hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-109">If you need toocreate a VM, use one of hello following methods:</span></span>

- <span data-ttu-id="fc6a6-110">Hello [Azure CLI 2.0](quick-create-cli.md)</span><span class="sxs-lookup"><span data-stu-id="fc6a6-110">hello [Azure CLI 2.0](quick-create-cli.md)</span></span>
- <span data-ttu-id="fc6a6-111">Hello [Azure-portálon](quick-create-portal.md)</span><span class="sxs-lookup"><span data-stu-id="fc6a6-111">hello [Azure portal](quick-create-portal.md)</span></span>


## <a name="install-a-desktop-environment-on-your-linux-vm"></a><span data-ttu-id="fc6a6-112">Egy asztali környezet telepítése a Linux virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="fc6a6-112">Install a desktop environment on your Linux VM</span></span>
<span data-ttu-id="fc6a6-113">A legtöbb Linux virtuális gépet az Azure-ban nem rendelkeznek egy asztali környezetben alapértelmezés szerint telepítve.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-113">Most Linux VMs in Azure do not have a desktop environment installed by default.</span></span> <span data-ttu-id="fc6a6-114">Linux virtuális gépek általában felügyelt asztali környezet helyett SSH-kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-114">Linux VMs are commonly managed using SSH connections rather than a desktop environment.</span></span> <span data-ttu-id="fc6a6-115">Nincsenek a Linux, amelyek segítségével különböző asztali környezetekhez.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-115">There are various desktop environments in Linux that you can choose.</span></span> <span data-ttu-id="fc6a6-116">Attól függően, hogy a munkakörnyezet választott akkor előfordulhat, hogy használnak egy too2 GB szabad lemezterület, és 5 too10 perc tooinstall érvénybe és konfigurálja az összes szükséges hello csomagok.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-116">Depending on your choice of desktop environment, it may consume one too2 GB of disk space, and take 5 too10 minutes tooinstall and configure all hello required packages.</span></span>

<span data-ttu-id="fc6a6-117">hello következő példa telepíti a hello lightweight [xfce4](https://www.xfce.org/) Ubuntu virtuális gép az asztali környezetet.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-117">hello following example installs hello lightweight [xfce4](https://www.xfce.org/) desktop environment on an Ubuntu VM.</span></span> <span data-ttu-id="fc6a6-118">Az egyéb terjesztéseket némileg eltérőek a parancsokat (használja `yum` tooinstall Red Hat Enterprise Linux és megfelelő konfigurálása `selinux` szabályokat, vagy használja `zypper` például a SUSE, tooinstall).</span><span class="sxs-lookup"><span data-stu-id="fc6a6-118">Commands for other distributions vary slightly (use `yum` tooinstall on Red Hat Enterprise Linux and configure appropriate `selinux` rules, or use `zypper` tooinstall on SUSE, for example).</span></span>

<span data-ttu-id="fc6a6-119">Első, SSH tooyour virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-119">First, SSH tooyour VM.</span></span> <span data-ttu-id="fc6a6-120">hello alábbi példa csatlakozik toohello nevű virtuális gép *myvm.westus.cloudapp.azure.com* hello a felhasználónevet használva a *azureuser*:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-120">hello following example connects toohello VM named *myvm.westus.cloudapp.azure.com* with hello username of *azureuser*:</span></span>

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

<span data-ttu-id="fc6a6-121">Ha Windows használ, és az SSH használatával további információra van szüksége, tekintse meg [hogyan toouse SSH-kulcsok Windows](ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="fc6a6-121">If you are using Windows and need more information on using SSH, see [How toouse SSH keys with Windows](ssh-from-windows.md).</span></span>

<span data-ttu-id="fc6a6-122">Következő lépésként telepítse a xfce használatával `apt` az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-122">Next, install xfce using `apt` as follows:</span></span>

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a><span data-ttu-id="fc6a6-123">A távoli asztali kiszolgáló telepítése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fc6a6-123">Install and configure a remote desktop server</span></span>
<span data-ttu-id="fc6a6-124">Most, hogy egy asztali környezetben telepített, konfigurálhatja a bejövő kapcsolatok a távoli asztali szolgáltatás toolisten.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-124">Now that you have a desktop environment installed, configure a remote desktop service toolisten for incoming connections.</span></span> <span data-ttu-id="fc6a6-125">[xrdp](http://xrdp.org) egy nyílt forráskódú Remote Desktop Protocol (RDP) kiszolgáló, amely elérhető a legtöbb Linux terjesztéseket, és jól xfce működik.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-125">[xrdp](http://xrdp.org) is an open source Remote Desktop Protocol (RDP) server that is available on most Linux distributions, and works well with xfce.</span></span> <span data-ttu-id="fc6a6-126">Telepíthető xrdp az Ubuntu virtuális gép az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-126">Install xrdp on your Ubuntu VM as follows:</span></span>

```bash
sudo apt-get install xrdp
```

<span data-ttu-id="fc6a6-127">Mondja el xrdp milyen asztali környezetben toouse a munkamenet indításakor.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-127">Tell xrdp what desktop environment toouse when you start your session.</span></span> <span data-ttu-id="fc6a6-128">Beállítása xrdp toouse xfce az asztali környezetet az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-128">Configure xrdp toouse xfce as your desktop environment as follows:</span></span>

```bash
echo xfce4-session >~/.xsession
```

<span data-ttu-id="fc6a6-129">Indítsa újra a hello xrdp szolgáltatást hello módosítások tootake hatása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-129">Restart hello xrdp service for hello changes tootake effect as follows:</span></span>

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a><span data-ttu-id="fc6a6-130">Egy helyi felhasználói fiók jelszavának beállítása</span><span class="sxs-lookup"><span data-stu-id="fc6a6-130">Set a local user account password</span></span>
<span data-ttu-id="fc6a6-131">Ha létrehozott egy jelszót a felhasználói fiók létrehozása után a virtuális gép, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-131">If you created a password for your user account when you created your VM, skip this step.</span></span> <span data-ttu-id="fc6a6-132">Ha csak SSH-kulcs hitelesítést használ, és nem rendelkezik a helyi fiók jelszavának megadásához adjon meg egy jelszót, a virtuális gép tooyour xrdp toolog használata előtt.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-132">If you only use SSH key authentication and do not have a local account password set, specify a password before you use xrdp toolog in tooyour VM.</span></span> <span data-ttu-id="fc6a6-133">xrdp nem fogadja el az SSH-kulcsok a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-133">xrdp cannot accept SSH keys for authentication.</span></span> <span data-ttu-id="fc6a6-134">hello alábbi példa meghatározza, hogy egy hello felhasználói fiók jelszavát *azureuser*:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-134">hello following example specifies a password for hello user account *azureuser*:</span></span>

```bash
sudo passwd azureuser
```

> [!NOTE]
> <span data-ttu-id="fc6a6-135">A jelszó megadása nem frissíti a a SSHD konfigurációs toopermit jelszavas bejelentkezéseket, ha jelenleg nem létezik.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-135">Specifying a password does not update your SSHD configuration toopermit password logins if it currently does not.</span></span> <span data-ttu-id="fc6a6-136">Biztonsági szempontból előfordulhat, hogy tooconnect tooyour VM kívánja az SSH-alagút-alapú hitelesítéssel rendelkező, és csatlakoztassa a tooxrdp.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-136">From a security perspective, you may wish tooconnect tooyour VM with an SSH tunnel using key-based authentication and then connect tooxrdp.</span></span> <span data-ttu-id="fc6a6-137">Ha igen, ugorjon a következő lépés a hálózati biztonsági csoport szabály tooallow távoli asztali forgalom létrehozásával hello.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-137">If so, skip hello following step on creating a network security group rule tooallow remote desktop traffic.</span></span>


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a><span data-ttu-id="fc6a6-138">A távoli asztal forgalmat hálózati biztonsági csoport szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="fc6a6-138">Create a Network Security Group rule for Remote Desktop traffic</span></span>
<span data-ttu-id="fc6a6-139">tooallow távoli asztal forgalom tooreach a Linux virtuális gép, egy hálózati biztonsági szabály kell toobe létrehozása, amely lehetővé teszi a TCP port 3389 tooreach a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-139">tooallow Remote Desktop traffic tooreach your Linux VM, a network security group rule needs toobe created that allows TCP on port 3389 tooreach your VM.</span></span> <span data-ttu-id="fc6a6-140">További információ a hálózati biztonsági csoportszabályok: [Mi az a hálózati biztonsági csoport?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="fc6a6-140">For more information about network security group rules, see [What is a Network Security Group?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span> <span data-ttu-id="fc6a6-141">Emellett [használata hello Azure portál toocreate egy hálózati biztonsági szabály](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fc6a6-141">You can also [use hello Azure portal toocreate a network security group rule](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="fc6a6-142">hello alábbi példák hozzon létre egy hálózati biztonsági csoport szabály [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) nevű *myNetworkSecurityGroupRule* túl*engedélyezése* forgalom*tcp* port *3389-es*.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-142">hello following examples create a network security group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) named *myNetworkSecurityGroupRule* too*allow* traffic on *tcp* port *3389*.</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1010 \
    --destination-port-range 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a><span data-ttu-id="fc6a6-143">A Linux virtuális gép csatlakozzon a távoli asztali ügyfél</span><span class="sxs-lookup"><span data-stu-id="fc6a6-143">Connect your Linux VM with a Remote Desktop client</span></span>
<span data-ttu-id="fc6a6-144">Nyissa meg a helyi távoli asztali ügyfél, és csatlakozzon a toohello IP-cím vagy a Linux virtuális Gépet DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-144">Open your local remote desktop client and connect toohello IP address or DNS name of your Linux VM.</span></span> <span data-ttu-id="fc6a6-145">Írja be hello felhasználónév és jelszó hello felhasználói fiók a virtuális Gépet a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-145">Enter hello username and password for hello user account on your VM as follows:</span></span>

![Csatlakozzon a távoli asztali ügyfél használatával tooxrdp](./media/use-remote-desktop/remote-desktop-client.png)

<span data-ttu-id="fc6a6-147">Hitelesítés után hello xfce asztali környezetben betölteni, és keresse meg a következő példa hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-147">After authenticating, hello xfce desktop environment will load and look similar toohello following example:</span></span>

![asztali környezetben xfce xrdp keresztül](./media/use-remote-desktop/xfce-desktop-environment.png)


## <a name="troubleshoot"></a><span data-ttu-id="fc6a6-149">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="fc6a6-149">Troubleshoot</span></span>
<span data-ttu-id="fc6a6-150">Ha tooyour Linux virtuális gépet egy távoli asztali ügyfél nem tud csatlakozni, használja `netstat` a a Linux virtuális gép tooverify, amely a virtuális Gépet a figyeli az RDP-kapcsolatok az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-150">If you cannot connect tooyour Linux VM using a Remote Desktop client, use `netstat` on your Linux VM tooverify that your VM is listening for RDP connections  as follows:</span></span>

```bash
sudo netstat -plnt | grep rdp
```

<span data-ttu-id="fc6a6-151">hello a következő példa azt mutatja meg a várt módon 3389-es TCP-porton VM hello:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-151">hello following example shows hello VM listening on TCP port 3389 as expected:</span></span>

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

<span data-ttu-id="fc6a6-152">Ha hello xrdp szolgáltatás nem figyel, az Ubuntu virtuális gép indítsa újra hello szolgáltatást az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-152">If hello xrdp service is not listening, on an Ubuntu VM restart hello service as follows:</span></span>

```bash
sudo service xrdp restart
```

<span data-ttu-id="fc6a6-153">Tekintse át naplók */var/log*Thug a Ubuntu virtuális gépén az jelzések toowhy hello szolgáltatás nem válaszol.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-153">Review logs in */var/log*Thug  on your Ubuntu VM for indications as toowhy hello service may not be responding.</span></span> <span data-ttu-id="fc6a6-154">Ugyanígy figyelheti hello syslog során egy távoli asztali kapcsolat kísérlet tooview hibák:</span><span class="sxs-lookup"><span data-stu-id="fc6a6-154">You can also monitor hello syslog during a remote desktop connection attempt tooview any errors:</span></span>

```bash
tail -f /var/log/syslog
```

<span data-ttu-id="fc6a6-155">Más például Red Hat Enterprise Linux és SUSE Linux terjesztésekről különböző módokon toorestart szolgáltatások és a másodlagos napló fájl helyek tooreview állhat.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-155">Other Linux distributions such as Red Hat Enterprise Linux and SUSE may have different ways toorestart services and alternate log file locations tooreview.</span></span>

<span data-ttu-id="fc6a6-156">Ha a távoli asztali ügyfél nem kapott választ, és nem látható eseményeket hello rendszernaplóba, ez a viselkedés azt jelzi, hogy a távoli asztali forgalom nem érhető el a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-156">If you do not receive any response in your remote desktop client and do not see any events in hello system log, this behavior indicates that remote desktop traffic cannot reach hello VM.</span></span> <span data-ttu-id="fc6a6-157">Tekintse át a hálózati biztonsági csoport szabályok tooensure, hogy rendelkezik-e a szabály toopermit TCP 3389-es porton.</span><span class="sxs-lookup"><span data-stu-id="fc6a6-157">Review your network security group rules tooensure that you have a rule toopermit TCP on port 3389.</span></span> <span data-ttu-id="fc6a6-158">További információkért lásd: [alkalmazás csatlakozási problémák](../windows/troubleshoot-app-connection.md).</span><span class="sxs-lookup"><span data-stu-id="fc6a6-158">For more information, see [Troubleshoot application connectivity issues](../windows/troubleshoot-app-connection.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="fc6a6-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fc6a6-159">Next steps</span></span>
<span data-ttu-id="fc6a6-160">Létrehozásával és az SSH-kulcsok használata a Linux virtuális gépek kapcsolatos további információkért lásd: [hozzon létre SSH-kulcsok Linux virtuális gépek Azure-ban](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="fc6a6-160">For more information about creating and using SSH keys with Linux VMs, see [Create SSH keys for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

<span data-ttu-id="fc6a6-161">Az SSH használata a Windows további információkért lásd: [hogyan toouse SSH-kulcsok Windows](ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="fc6a6-161">For information on using SSH from Windows, see [How toouse SSH keys with Windows](ssh-from-windows.md).</span></span>

