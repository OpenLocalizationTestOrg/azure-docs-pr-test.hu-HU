---
title: "A Linux virtuális gép távoli asztal használata az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítse és konfigurálja az Azure-ban a grafikus eszközök Linux virtuális gép kapcsolódni a távoli asztal (xrdp)"
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
ms.openlocfilehash: d8d6130a270285c84c1dd057a3512cdeb39287f6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="install-and-configure-remote-desktop-to-connect-to-a-linux-vm-in-azure"></a><span data-ttu-id="a0370-103">Telepítse és konfigurálja a távoli asztal egy Linux virtuális Gépet az Azure-ban való kapcsolódáshoz</span><span class="sxs-lookup"><span data-stu-id="a0370-103">Install and configure Remote Desktop to connect to a Linux VM in Azure</span></span>
<span data-ttu-id="a0370-104">A parancssorból, a secure shell (SSH-) kapcsolaton keresztül általában felügyelt Linux virtuális gépek (VM) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="a0370-104">Linux virtual machines (VMs) in Azure are usually managed from the command line using a secure shell (SSH) connection.</span></span> <span data-ttu-id="a0370-105">Új Linux vagy gyors hibaelhárítási helyzetekben, amikor a távoli asztal használata könnyebben lehet.</span><span class="sxs-lookup"><span data-stu-id="a0370-105">When new to Linux, or for quick troubleshooting scenarios, the use of remote desktop may be easier.</span></span> <span data-ttu-id="a0370-106">Ez a cikk részletesen telepítése és konfigurálása az asztali környezetet ([xfce](https://www.xfce.org)) és a távoli asztal ([xrdp](http://www.xrdp.org)) a Linux virtuális gép használata a Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="a0370-106">This article details how to install and configure a desktop environment ([xfce](https://www.xfce.org)) and remote desktop ([xrdp](http://www.xrdp.org)) for your Linux VM using the Resource Manager deployment model.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="a0370-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a0370-107">Prerequisites</span></span>
<span data-ttu-id="a0370-108">Ez a cikk az Azure-ban meglévő Linux virtuális gép szükséges.</span><span class="sxs-lookup"><span data-stu-id="a0370-108">This article requires an existing Linux VM in Azure.</span></span> <span data-ttu-id="a0370-109">Ha a virtuális gép létrehozása az kell, használja a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="a0370-109">If you need to create a VM, use one of the following methods:</span></span>

- <span data-ttu-id="a0370-110">A [Azure CLI 2.0](quick-create-cli.md)</span><span class="sxs-lookup"><span data-stu-id="a0370-110">The [Azure CLI 2.0](quick-create-cli.md)</span></span>
- <span data-ttu-id="a0370-111">A [Azure-portálon](quick-create-portal.md)</span><span class="sxs-lookup"><span data-stu-id="a0370-111">The [Azure portal](quick-create-portal.md)</span></span>


## <a name="install-a-desktop-environment-on-your-linux-vm"></a><span data-ttu-id="a0370-112">Egy asztali környezet telepítése a Linux virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="a0370-112">Install a desktop environment on your Linux VM</span></span>
<span data-ttu-id="a0370-113">A legtöbb Linux virtuális gépet az Azure-ban nem rendelkeznek egy asztali környezetben alapértelmezés szerint telepítve.</span><span class="sxs-lookup"><span data-stu-id="a0370-113">Most Linux VMs in Azure do not have a desktop environment installed by default.</span></span> <span data-ttu-id="a0370-114">Linux virtuális gépek általában felügyelt asztali környezet helyett SSH-kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="a0370-114">Linux VMs are commonly managed using SSH connections rather than a desktop environment.</span></span> <span data-ttu-id="a0370-115">Nincsenek a Linux, amelyek segítségével különböző asztali környezetekhez.</span><span class="sxs-lookup"><span data-stu-id="a0370-115">There are various desktop environments in Linux that you can choose.</span></span> <span data-ttu-id="a0370-116">Attól függően, hogy a munkakörnyezet választott akkor előfordulhat, hogy használnak egy – 2 GB szabad lemezterület, és telepítse és konfigurálja a szükséges csomagokat 5 – 10 percig tarthat.</span><span class="sxs-lookup"><span data-stu-id="a0370-116">Depending on your choice of desktop environment, it may consume one to 2 GB of disk space, and take 5 to 10 minutes to install and configure all the required packages.</span></span>

<span data-ttu-id="a0370-117">A következő példa telepíti az egyszerűsített [xfce4](https://www.xfce.org/) Ubuntu virtuális gép az asztali környezetet.</span><span class="sxs-lookup"><span data-stu-id="a0370-117">The following example installs the lightweight [xfce4](https://www.xfce.org/) desktop environment on an Ubuntu VM.</span></span> <span data-ttu-id="a0370-118">Parancsok a némileg eltérő más terjesztési (használata `yum` Red Hat Enterprise Linux telepítését és konfigurálását a megfelelő `selinux` szabályokat, vagy használja `zypper` SUSE, például telepítése).</span><span class="sxs-lookup"><span data-stu-id="a0370-118">Commands for other distributions vary slightly (use `yum` to install on Red Hat Enterprise Linux and configure appropriate `selinux` rules, or use `zypper` to install on SUSE, for example).</span></span>

<span data-ttu-id="a0370-119">Első, az SSH-kapcsolatot a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="a0370-119">First, SSH to your VM.</span></span> <span data-ttu-id="a0370-120">Az alábbi példa nevű virtuális gép csatlakozik *myvm.westus.cloudapp.azure.com* a felhasználónevet használva a *azureuser*:</span><span class="sxs-lookup"><span data-stu-id="a0370-120">The following example connects to the VM named *myvm.westus.cloudapp.azure.com* with the username of *azureuser*:</span></span>

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

<span data-ttu-id="a0370-121">Ha Windows használ, és az SSH használatával további információra van szüksége, tekintse meg [SSH használata a Windows kulcsok](ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="a0370-121">If you are using Windows and need more information on using SSH, see [How to use SSH keys with Windows](ssh-from-windows.md).</span></span>

<span data-ttu-id="a0370-122">Következő lépésként telepítse a xfce használatával `apt` az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a0370-122">Next, install xfce using `apt` as follows:</span></span>

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a><span data-ttu-id="a0370-123">A távoli asztali kiszolgáló telepítése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a0370-123">Install and configure a remote desktop server</span></span>
<span data-ttu-id="a0370-124">Most, hogy egy asztali környezetben telepített, konfigurálja a távoli asztali szolgáltatás a bejövő kapcsolatok figyelésére.</span><span class="sxs-lookup"><span data-stu-id="a0370-124">Now that you have a desktop environment installed, configure a remote desktop service to listen for incoming connections.</span></span> <span data-ttu-id="a0370-125">[xrdp](http://xrdp.org) egy nyílt forráskódú Remote Desktop Protocol (RDP) kiszolgáló, amely elérhető a legtöbb Linux terjesztéseket, és jól xfce működik.</span><span class="sxs-lookup"><span data-stu-id="a0370-125">[xrdp](http://xrdp.org) is an open source Remote Desktop Protocol (RDP) server that is available on most Linux distributions, and works well with xfce.</span></span> <span data-ttu-id="a0370-126">Telepíthető xrdp az Ubuntu virtuális gép az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a0370-126">Install xrdp on your Ubuntu VM as follows:</span></span>

```bash
sudo apt-get install xrdp
```

<span data-ttu-id="a0370-127">Mondja el xrdp milyen asztali környezet a munkamenet indításakor.</span><span class="sxs-lookup"><span data-stu-id="a0370-127">Tell xrdp what desktop environment to use when you start your session.</span></span> <span data-ttu-id="a0370-128">Adja meg xrdp használandó xfce az asztali környezetet az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a0370-128">Configure xrdp to use xfce as your desktop environment as follows:</span></span>

```bash
echo xfce4-session >~/.xsession
```

<span data-ttu-id="a0370-129">Indítsa újra a xrdp szolgáltatást, a módosítások érvénybe lépéséhez az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a0370-129">Restart the xrdp service for the changes to take effect as follows:</span></span>

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a><span data-ttu-id="a0370-130">Egy helyi felhasználói fiók jelszavának beállítása</span><span class="sxs-lookup"><span data-stu-id="a0370-130">Set a local user account password</span></span>
<span data-ttu-id="a0370-131">Ha létrehozott egy jelszót a felhasználói fiók létrehozása után a virtuális gép, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="a0370-131">If you created a password for your user account when you created your VM, skip this step.</span></span> <span data-ttu-id="a0370-132">Ha csak SSH kulcs hitelesítést használ, és nem rendelkezik a helyi fiók jelszavának megadásához adjon meg egy jelszót, mielőtt xrdp jelentkezzen be a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="a0370-132">If you only use SSH key authentication and do not have a local account password set, specify a password before you use xrdp to log in to your VM.</span></span> <span data-ttu-id="a0370-133">xrdp nem fogadja el az SSH-kulcsok a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="a0370-133">xrdp cannot accept SSH keys for authentication.</span></span> <span data-ttu-id="a0370-134">Az alábbi példában a felhasználói fiókhoz tartozó jelszót ad meg *azureuser*:</span><span class="sxs-lookup"><span data-stu-id="a0370-134">The following example specifies a password for the user account *azureuser*:</span></span>

```bash
sudo passwd azureuser
```

> [!NOTE]
> <span data-ttu-id="a0370-135">Adja meg a jelszót nem frissíti a SSHD konfigurációját, és lehetővé teszik a jelszavas bejelentkezéseket, ha jelenleg nem létezik.</span><span class="sxs-lookup"><span data-stu-id="a0370-135">Specifying a password does not update your SSHD configuration to permit password logins if it currently does not.</span></span> <span data-ttu-id="a0370-136">Biztonsági szempontból előfordulhat, hogy szeretne csatlakozni a virtuális gép SSH-alagút-alapú hitelesítést használ, és xrdp csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="a0370-136">From a security perspective, you may wish to connect to your VM with an SSH tunnel using key-based authentication and then connect to xrdp.</span></span> <span data-ttu-id="a0370-137">Ha igen, a távoli asztali forgalom engedélyezésére hálózati biztonsági csoport szabályt hoz létre a következő lépés kihagyásához.</span><span class="sxs-lookup"><span data-stu-id="a0370-137">If so, skip the following step on creating a network security group rule to allow remote desktop traffic.</span></span>


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a><span data-ttu-id="a0370-138">A távoli asztal forgalmat hálózati biztonsági csoport szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="a0370-138">Create a Network Security Group rule for Remote Desktop traffic</span></span>
<span data-ttu-id="a0370-139">A távoli asztal forgalom való elérni a Linux virtuális Gépet, a hálózati biztonsági csoport szabályt kell létrehozni, amely lehetővé teszi a TCP-port 3389-es elérni a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="a0370-139">To allow Remote Desktop traffic to reach your Linux VM, a network security group rule needs to be created that allows TCP on port 3389 to reach your VM.</span></span> <span data-ttu-id="a0370-140">További információ a hálózati biztonsági csoportszabályok: [Mi az a hálózati biztonsági csoport?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="a0370-140">For more information about network security group rules, see [What is a Network Security Group?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span> <span data-ttu-id="a0370-141">Emellett [egy hálózati biztonsági szabály létrehozása az Azure portál segítségével](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a0370-141">You can also [use the Azure portal to create a network security group rule](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="a0370-142">Az alábbi példák a hálózati biztonsági csoport szabály létrehozása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) nevű *myNetworkSecurityGroupRule* való *engedélyezése* forgalom*tcp* port *3389-es*.</span><span class="sxs-lookup"><span data-stu-id="a0370-142">The following examples create a network security group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) named *myNetworkSecurityGroupRule* to *allow* traffic on *tcp* port *3389*.</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1010 \
    --destination-port-range 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a><span data-ttu-id="a0370-143">A Linux virtuális gép csatlakozzon a távoli asztali ügyfél</span><span class="sxs-lookup"><span data-stu-id="a0370-143">Connect your Linux VM with a Remote Desktop client</span></span>
<span data-ttu-id="a0370-144">Nyissa meg a helyi távoli asztali ügyfél, és kapcsolódjon az IP-cím vagy a Linux virtuális Gépet DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="a0370-144">Open your local remote desktop client and connect to the IP address or DNS name of your Linux VM.</span></span> <span data-ttu-id="a0370-145">Írja be a felhasználónevet és jelszót a felhasználói fiók a virtuális gépen az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a0370-145">Enter the username and password for the user account on your VM as follows:</span></span>

![A távoli asztali ügyfél használatával xrdp kapcsolódni](./media/use-remote-desktop/remote-desktop-client.png)

<span data-ttu-id="a0370-147">Hitelesítés után a xfce asztali környezet betölteni, és keresse meg a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="a0370-147">After authenticating, the xfce desktop environment will load and look similar to the following example:</span></span>

![asztali környezetben xfce xrdp keresztül](./media/use-remote-desktop/xfce-desktop-environment.png)


## <a name="troubleshoot"></a><span data-ttu-id="a0370-149">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="a0370-149">Troubleshoot</span></span>
<span data-ttu-id="a0370-150">Ha a Linux virtuális gép egy távoli asztali ügyfél nem tud csatlakozni, használja `netstat` a Linux virtuális gépén ellenőrzése, hogy a virtuális Gépet a figyeli az RDP-kapcsolatok az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a0370-150">If you cannot connect to your Linux VM using a Remote Desktop client, use `netstat` on your Linux VM to verify that your VM is listening for RDP connections  as follows:</span></span>

```bash
sudo netstat -plnt | grep rdp
```

<span data-ttu-id="a0370-151">A következő példa bemutatja a várt módon 3389-es TCP-porton VM:</span><span class="sxs-lookup"><span data-stu-id="a0370-151">The following example shows the VM listening on TCP port 3389 as expected:</span></span>

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

<span data-ttu-id="a0370-152">Ha a xrdp szolgáltatás nem figyel, az Ubuntu virtuális gép indítsa újra a szolgáltatást az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a0370-152">If the xrdp service is not listening, on an Ubuntu VM restart the service as follows:</span></span>

```bash
sudo service xrdp restart
```

<span data-ttu-id="a0370-153">Tekintse át naplók */var/log*Thug a helyet, miért a szolgáltatás nem válaszol a Ubuntu virtuális gépén.</span><span class="sxs-lookup"><span data-stu-id="a0370-153">Review logs in */var/log*Thug  on your Ubuntu VM for indications as to why the service may not be responding.</span></span> <span data-ttu-id="a0370-154">A syslog is végezhet figyelést, a hibák megtekintése a távoli asztali kapcsolat tett kísérlet során:</span><span class="sxs-lookup"><span data-stu-id="a0370-154">You can also monitor the syslog during a remote desktop connection attempt to view any errors:</span></span>

```bash
tail -f /var/log/syslog
```

<span data-ttu-id="a0370-155">Előfordulhat, hogy más például Red Hat Enterprise Linux és SUSE Linux terjesztésekről különböző módokon újraindítja a szolgáltatásokat és alternatív naplófájl helye áttekintése.</span><span class="sxs-lookup"><span data-stu-id="a0370-155">Other Linux distributions such as Red Hat Enterprise Linux and SUSE may have different ways to restart services and alternate log file locations to review.</span></span>

<span data-ttu-id="a0370-156">Ha a távoli asztali ügyfél nem kapott választ, és nem látja a rendszernaplóba eseményeket, ez arra utal, hogy a távoli asztali forgalom nem tudja elérni a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="a0370-156">If you do not receive any response in your remote desktop client and do not see any events in the system log, this behavior indicates that remote desktop traffic cannot reach the VM.</span></span> <span data-ttu-id="a0370-157">Tekintse át a hálózati biztonsági csoportszabályok annak érdekében, hogy rendelkezik-e egy szabályt, amely engedélyezi a TCP-port 3389-es.</span><span class="sxs-lookup"><span data-stu-id="a0370-157">Review your network security group rules to ensure that you have a rule to permit TCP on port 3389.</span></span> <span data-ttu-id="a0370-158">További információkért lásd: [alkalmazás csatlakozási problémák](../windows/troubleshoot-app-connection.md).</span><span class="sxs-lookup"><span data-stu-id="a0370-158">For more information, see [Troubleshoot application connectivity issues](../windows/troubleshoot-app-connection.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="a0370-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0370-159">Next steps</span></span>
<span data-ttu-id="a0370-160">Létrehozásával és az SSH-kulcsok használata a Linux virtuális gépek kapcsolatos további információkért lásd: [hozzon létre SSH-kulcsok Linux virtuális gépek Azure-ban](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="a0370-160">For more information about creating and using SSH keys with Linux VMs, see [Create SSH keys for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

<span data-ttu-id="a0370-161">Az SSH használata a Windows további információkért lásd: [SSH használata a Windows kulcsok](ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="a0370-161">For information on using SSH from Windows, see [How to use SSH keys with Windows](ssh-from-windows.md).</span></span>

