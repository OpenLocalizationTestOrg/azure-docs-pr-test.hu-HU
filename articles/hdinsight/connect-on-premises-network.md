---
title: "aaaConnect HDInsight tooyour a helyszíni hálózat - Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy HDInsight fürt egy Azure virtuális hálózatban, és csatlakoztassa tooyour a helyi hálózaton. Ismerje meg, hogyan tooconfigure névfeloldás HDInsight és a helyszíni hálózat között egyéni DNS-kiszolgáló használatával."
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 8a3adf0e3df7726d8e6566d723700506baaf89a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-hdinsight-tooyour-on-premise-network"></a><span data-ttu-id="adfbf-104">HDInsight tooyour helyi hálózat</span><span class="sxs-lookup"><span data-stu-id="adfbf-104">Connect HDInsight tooyour on-premise network</span></span>

<span data-ttu-id="adfbf-105">Ismerje meg, hogyan tooconnect HDInsight tooyour a helyszíni hálózati Azure virtuális hálózatok és a VPN-átjáró használatával.</span><span class="sxs-lookup"><span data-stu-id="adfbf-105">Learn how tooconnect HDInsight tooyour on-premises network by using Azure Virtual Networks and a VPN gateway.</span></span> <span data-ttu-id="adfbf-106">Ez a dokumentum a tervezési tudnivalókat tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="adfbf-106">This document provides planning information on:</span></span>

* <span data-ttu-id="adfbf-107">Egy Azure virtuális hálózatot, amely a tooyour a HDInsight eszközzel a helyszíni hálózat.</span><span class="sxs-lookup"><span data-stu-id="adfbf-107">Using HDInsight in an Azure Virtual Network that connects tooyour on-premises network.</span></span>

* <span data-ttu-id="adfbf-108">Konfigurálja a DNS-név feloldása hello virtuális hálózat és a helyszíni hálózat között.</span><span class="sxs-lookup"><span data-stu-id="adfbf-108">Configuring DNS name resolution between hello virtual network and your on-premises network.</span></span>

* <span data-ttu-id="adfbf-109">Hálózati biztonsági csoportok toorestrict internet access tooHDInsight konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="adfbf-109">Configuring network security groups toorestrict internet access tooHDInsight.</span></span>

* <span data-ttu-id="adfbf-110">A portok hello virtuális hálózaton a HDInsight által biztosított.</span><span class="sxs-lookup"><span data-stu-id="adfbf-110">Ports provided by HDInsight on hello virtual network.</span></span>

## <a name="create-hello-virtual-network-configuration"></a><span data-ttu-id="adfbf-111">Hello virtuális hálózati konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="adfbf-111">Create hello Virtual network configuration</span></span>

> [!IMPORTANT]
> <span data-ttu-id="adfbf-112">Ha a részletes útmutatót a csatlakozó HDInsight tooyour a helyszíni hálózat az Azure virtuális hálózat használatával című hello [csatlakozás HDInsight tooyour helyi hálózati](connect-on-premises-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="adfbf-112">If you are looking for step by step guidance on connecting HDInsight tooyour on-premises network using an Azure Virtual Network, see hello [Connect HDInsight tooyour on-premise network](connect-on-premises-network.md) document.</span></span>

<span data-ttu-id="adfbf-113">Használjon hello következő dokumentumok toolearn hogyan toocreate egy Azure virtuális hálózathoz csatlakoztatott tooyour a helyszíni hálózat:</span><span class="sxs-lookup"><span data-stu-id="adfbf-113">Use hello following documents toolearn how toocreate an Azure Virtual Network that is connected tooyour on-premises network:</span></span>
    
* [<span data-ttu-id="adfbf-114">Hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="adfbf-114">Using hello Azure portal</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [<span data-ttu-id="adfbf-115">Az Azure PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="adfbf-115">Using Azure PowerShell</span></span>](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [<span data-ttu-id="adfbf-116">Az Azure parancssori felület használata</span><span class="sxs-lookup"><span data-stu-id="adfbf-116">Using Azure CLI</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a><span data-ttu-id="adfbf-117">Névfeloldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="adfbf-117">Configure name resolution</span></span>

<span data-ttu-id="adfbf-118">tooallow HDInsight és hello csatlakoztatott hálózati toocommunicate nevű erőforrások, végezze el a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="adfbf-118">tooallow HDInsight and resources in hello joined network toocommunicate by name, you must perform hello following actions:</span></span>

* <span data-ttu-id="adfbf-119">Hozzon létre egy egyéni DNS-kiszolgáló hello Azure-beli virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="adfbf-119">Create a custom DNS server in hello Azure Virtual Network.</span></span>

* <span data-ttu-id="adfbf-120">Hello virtuális hálózati toouse hello egyéni DNS-kiszolgáló konfigurálása hello alapértelmezett Azure rekurzív feloldó helyett.</span><span class="sxs-lookup"><span data-stu-id="adfbf-120">Configure hello virtual network toouse hello custom DNS server instead of hello default Azure Recursive Resolver.</span></span>

* <span data-ttu-id="adfbf-121">Konfigurálja a továbbítási hello egyéni DNS-kiszolgáló és a helyi DNS-kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="adfbf-121">Configure forwarding between hello custom DNS server and your on-premises DNS server.</span></span>

<span data-ttu-id="adfbf-122">Ez a konfiguráció lehetővé teszi, hogy a következő viselkedés hello:</span><span class="sxs-lookup"><span data-stu-id="adfbf-122">This configuration enables hello following behavior:</span></span>

* <span data-ttu-id="adfbf-123">A teljes tartománynevek DNS-utótag hello rendelkező kérelmek __hello virtuális hálózat__ toohello egyéni DNS-kiszolgáló a rendszer továbbítja.</span><span class="sxs-lookup"><span data-stu-id="adfbf-123">Requests for fully qualified domain names that have hello DNS suffix __for hello virtual network__ are forwarded toohello custom DNS server.</span></span> <span data-ttu-id="adfbf-124">hello egyéni DNS-kiszolgáló ezután továbbítja a kérelmek toohello Azure rekurzív feloldó, amely hello IP-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="adfbf-124">hello custom DNS server then forwards these requests toohello Azure Recursive Resolver, which returns hello IP address.</span></span>

* <span data-ttu-id="adfbf-125">Minden más kérelemhez toohello a helyi DNS-kiszolgáló továbbítja.</span><span class="sxs-lookup"><span data-stu-id="adfbf-125">All other requests are forwarded toohello on-premises DNS server.</span></span> <span data-ttu-id="adfbf-126">A nyilvános internet erőforrások, például a microsoft.com még akkor is, kérelmeket a rendszer továbbítja toohello a helyi DNS-kiszolgáló a névfeloldáshoz.</span><span class="sxs-lookup"><span data-stu-id="adfbf-126">Even requests for public internet resources such as microsoft.com are forwarded toohello on-premises DNS server for name resolution.</span></span>

<span data-ttu-id="adfbf-127">A következő diagram hello a zöld sorai hello virtuális hálózat DNS-utótagja hello végződő erőforrásokra vonatkozó kéréseket.</span><span class="sxs-lookup"><span data-stu-id="adfbf-127">In hello following diagram, green lines are requests for resources that end in hello DNS suffix of hello virtual network.</span></span> <span data-ttu-id="adfbf-128">Kék sorai hello a helyi hálózaton vagy az erőforrásokra vonatkozó kéréseket hello nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="adfbf-128">Blue lines are requests for resources in hello on-premises network or on hello public internet.</span></span>

![A dokumentumban használt hello konfigurációs feloldása DNS-kérelmek ábrája](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a><span data-ttu-id="adfbf-130">Hozzon létre egy egyéni DNS-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="adfbf-130">Create a custom DNS server</span></span>

> [!IMPORTANT]
> <span data-ttu-id="adfbf-131">Hozzon létre és hello DNS-kiszolgáló HDInsight hello virtuális hálózaton történő telepítése előtt konfigurálnia kell.</span><span class="sxs-lookup"><span data-stu-id="adfbf-131">You must create and configure hello DNS server before installing HDInsight into hello virtual network.</span></span>

<span data-ttu-id="adfbf-132">Linux virtuális gép által használt hello toocreate [kötési](https://www.isc.org/downloads/bind/) DNS szoftverek használata hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="adfbf-132">toocreate a Linux VM that uses hello [Bind](https://www.isc.org/downloads/bind/) DNS software, use hello following steps:</span></span>

> [!NOTE]
> <span data-ttu-id="adfbf-133">hello következő lépések használják hello [Azure-portálon](https://portal.azure.com) toocreate egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="adfbf-133">hello following steps use hello [Azure portal](https://portal.azure.com) toocreate an Azure Virtual Machine.</span></span> <span data-ttu-id="adfbf-134">Más módokon toocreate egy virtuális gépet, lásd: hello [hozzon létre Virtuálisgép - Azure CLI](../virtual-machines/linux/quick-create-cli.md) és [hozzon létre Virtuálisgép - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="adfbf-134">For other ways toocreate a virtual machine, see hello [Create VM - Azure CLI](../virtual-machines/linux/quick-create-cli.md) and [Create VM - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) documents.</span></span>

1. <span data-ttu-id="adfbf-135">A hello [Azure-portálon](https://portal.azure.com), jelölje be  __+__ , __számítási__, és __Ubuntu Server 16.04 LTS__.</span><span class="sxs-lookup"><span data-stu-id="adfbf-135">From hello [Azure portal](https://portal.azure.com), select __+__, __Compute__, and __Ubuntu Server 16.04 LTS__.</span></span>

    ![Egy Ubuntu virtuális gép létrehozása](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. <span data-ttu-id="adfbf-137">A hello __alapjai__ területen adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="adfbf-137">From hello __Basics__ section, enter hello following information:</span></span>

    * <span data-ttu-id="adfbf-138">__Név__: egy rövid nevet, amely azonosítja a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="adfbf-138">__Name__: A friendly name that identifies this virtual machine.</span></span> <span data-ttu-id="adfbf-139">Például __DNSProxy__.</span><span class="sxs-lookup"><span data-stu-id="adfbf-139">For example, __DNSProxy__.</span></span>
    * <span data-ttu-id="adfbf-140">__Felhasználónév__: hello hello SSH-fiókjának nevét.</span><span class="sxs-lookup"><span data-stu-id="adfbf-140">__User name__: hello name of hello SSH account.</span></span>
    * <span data-ttu-id="adfbf-141">__Nyilvános SSH-kulcs__ vagy __jelszó__: hello hello SSH-fiókhoz tartozó hitelesítési módszer.</span><span class="sxs-lookup"><span data-stu-id="adfbf-141">__SSH public key__ or __Password__: hello authentication method for hello SSH account.</span></span> <span data-ttu-id="adfbf-142">Azt javasoljuk, nyilvános kulcsok, mert azok biztonságosabb.</span><span class="sxs-lookup"><span data-stu-id="adfbf-142">We recommend using public keys, as they are more secure.</span></span> <span data-ttu-id="adfbf-143">További információkért lásd: hello [Linux virtuális gépek létrehozása és használata SSH-kulcsok](../virtual-machines/linux/mac-create-ssh-keys.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="adfbf-143">For more information, see hello [Create and use SSH keys for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md) document.</span></span>
    * <span data-ttu-id="adfbf-144">__Erőforráscsoport__: válasszon __meglévő__, majd válassza ki a korábban létrehozott virtuális hálózat hello tartalmazó hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="adfbf-144">__Resource group__: Select __Use existing__, and then select hello resource group that contains hello virtual network created earlier.</span></span>
    * <span data-ttu-id="adfbf-145">__Hely__: Válasszon hello hello virtuális hálózat ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="adfbf-145">__Location__: Select hello same location as hello virtual network.</span></span>

    ![Alapszintű virtuálisgép-konfiguráció](./media/connect-on-premises-network/vm-basics.png)

    <span data-ttu-id="adfbf-147">Hagyja más bejegyzések: hello alapértelmezett értékeket, és válassza ki __OK__.</span><span class="sxs-lookup"><span data-stu-id="adfbf-147">Leave other entries at hello default values and then select __OK__.</span></span>

3. <span data-ttu-id="adfbf-148">A hello __méret kiválasztása__ szakaszban, jelölje be hello Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="adfbf-148">From hello __Choose a size__ section, select hello VM size.</span></span> <span data-ttu-id="adfbf-149">Ebben az oktatóanyagban válassza ki a legkisebb hello és legalacsonyabb költséget lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="adfbf-149">For this tutorial, select hello smallest and lowest cost option.</span></span> <span data-ttu-id="adfbf-150">toocontinue, használjon hello __válasszon__ gombra.</span><span class="sxs-lookup"><span data-stu-id="adfbf-150">toocontinue, use hello __Select__ button.</span></span>

4. <span data-ttu-id="adfbf-151">A hello __beállítások__ területen adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="adfbf-151">From hello __Settings__ section, enter hello following information:</span></span>

    * <span data-ttu-id="adfbf-152">__Virtuális hálózati__: válassza ki a korábban létrehozott virtuális hálózati hello.</span><span class="sxs-lookup"><span data-stu-id="adfbf-152">__Virtual network__: Select hello virtual network that you created earlier.</span></span>

    * <span data-ttu-id="adfbf-153">__Alhálózati__: hello alapértelmezett alhálózati hello virtuális hálózat kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="adfbf-153">__Subnet__: Select hello default subnet for hello virtual network.</span></span> <span data-ttu-id="adfbf-154">Tegye __nem__ válasszon hello hello VPN-átjáró által használt.</span><span class="sxs-lookup"><span data-stu-id="adfbf-154">Do __not__ select hello subnet used by hello VPN gateway.</span></span>

    * <span data-ttu-id="adfbf-155">__Diagnosztikai tárfiók__: Válasszon egy meglévő tárfiókot használ, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="adfbf-155">__Diagnostics storage account__: Either select an existing storage account or create a new one.</span></span>

    ![Virtuális hálózati beállítások](./media/connect-on-premises-network/virtual-network-settings.png)

    <span data-ttu-id="adfbf-157">Hagyja hello más bejegyzések hello alapértelmezett értéket, majd válasszon __OK__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="adfbf-157">Leave hello other entries at hello default value, then select __OK__ toocontinue.</span></span>

5. <span data-ttu-id="adfbf-158">A hello __beszerzési__ szakaszban, jelölje be hello __beszerzési__ gomb toocreate hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="adfbf-158">From hello __Purchase__ section, select hello __Purchase__ button toocreate hello virtual machine.</span></span>

6. <span data-ttu-id="adfbf-159">Hello virtuális gép létrehozása után annak __áttekintése__ szakasz jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="adfbf-159">Once hello virtual machine has been created, its __Overview__ section is displayed.</span></span> <span data-ttu-id="adfbf-160">Hello hello bal oldali listában jelölje ki __tulajdonságok__.</span><span class="sxs-lookup"><span data-stu-id="adfbf-160">From hello list on hello left, select __Properties__.</span></span> <span data-ttu-id="adfbf-161">Mentse a hello __nyilvános IP-cím__ és __magánhálózati IP-cím__ értékeket.</span><span class="sxs-lookup"><span data-stu-id="adfbf-161">Save hello __Public IP address__ and __Private IP address__ values.</span></span> <span data-ttu-id="adfbf-162">A következő szakaszban hello lesz.</span><span class="sxs-lookup"><span data-stu-id="adfbf-162">It will be used in hello next section.</span></span>

    ![Nyilvános és magánhálózati IP-címek](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a><span data-ttu-id="adfbf-164">Telepítse és konfigurálja a Bind (DNS szoftver)</span><span class="sxs-lookup"><span data-stu-id="adfbf-164">Install and configure Bind (DNS software)</span></span>

1. <span data-ttu-id="adfbf-165">Használja az SSH tooconnect toohello __nyilvános IP-cím__ hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="adfbf-165">Use SSH tooconnect toohello __public IP address__ of hello virtual machine.</span></span> <span data-ttu-id="adfbf-166">a következő példa hello tooa virtuális gépen: 40.68.254.142 csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="adfbf-166">hello following example connects tooa virtual machine at 40.68.254.142:</span></span>

    ```bash
    ssh sshuser@40.68.254.142
    ```

    <span data-ttu-id="adfbf-167">Cserélje le `sshuser` hello SSH-felhasználói fiókot a megadott hello fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="adfbf-167">Replace `sshuser` with hello SSH user account you specified when creating hello cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="adfbf-168">Számos módon tooobtain hello `ssh` segédprogram.</span><span class="sxs-lookup"><span data-stu-id="adfbf-168">There are a variety of ways tooobtain hello `ssh` utility.</span></span> <span data-ttu-id="adfbf-169">A Linux, Unix és macOS akkor biztosított hello operációs rendszer részeként.</span><span class="sxs-lookup"><span data-stu-id="adfbf-169">On Linux, Unix, and macOS, it is provided as part of hello operating system.</span></span> <span data-ttu-id="adfbf-170">Ha Windows használja, fontolja meg hello alábbi beállítások egyikét:</span><span class="sxs-lookup"><span data-stu-id="adfbf-170">If you are using Windows, consider one of hello following options:</span></span>
    >
    > * [<span data-ttu-id="adfbf-171">Azure-felhőbe rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="adfbf-171">Azure Cloud Shell</span></span>](../cloud-shell/quickstart.md)
    > * [<span data-ttu-id="adfbf-172">A Windows 10 Ubuntu bash</span><span class="sxs-lookup"><span data-stu-id="adfbf-172">Bash on Ubuntu on Windows 10</span></span>](https://msdn.microsoft.com/commandline/wsl/about)
    > * [<span data-ttu-id="adfbf-173">Git (https://git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="adfbf-173">Git (https://git-scm.com/)</span></span>](https://git-scm.com/)
    > * [<span data-ttu-id="adfbf-174">OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)</span><span class="sxs-lookup"><span data-stu-id="adfbf-174">OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)</span></span>](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. <span data-ttu-id="adfbf-175">tooinstall Bind, a következő parancsok hello SSH-munkamenetből hello használata:</span><span class="sxs-lookup"><span data-stu-id="adfbf-175">tooinstall Bind, use hello following commands from hello SSH session:</span></span>

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. <span data-ttu-id="adfbf-176">tooconfigure Bind tooforward neve feloldási kérések tooyour helyszíni DNS-kiszolgáló, használja a szöveg hello hello tartalmát, a következő hello `/etc/bind/named.conf.options` fájlt:</span><span class="sxs-lookup"><span data-stu-id="adfbf-176">tooconfigure Bind tooforward name resolution requests tooyour on-prem DNS server, use hello following text as hello contents of hello `/etc/bind/named.conf.options` file:</span></span>

        acl goodclients {
            10.0.0.0/16; # Replace with hello IP address range of hello virtual network
            10.1.0.0/16; # Replace with hello IP address range of hello on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with hello IP address of hello on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform tooRFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > <span data-ttu-id="adfbf-177">Cserélje le a hello hello értékek `goodclients` hello IP-címtartománnyal rendelkező szakasza hello virtuális hálózat és a helyszíni hálózat.</span><span class="sxs-lookup"><span data-stu-id="adfbf-177">Replace hello values in hello `goodclients` section with hello IP address range of hello virtual network and on-premises network.</span></span> <span data-ttu-id="adfbf-178">Ez a rész hello címek, amelyek a DNS-kiszolgáló elfogadja az érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="adfbf-178">This section defines hello addresses that this DNS server accepts requests from.</span></span>
    >
    > <span data-ttu-id="adfbf-179">Cserélje le a hello `192.168.0.1` hello bejegyzést `forwarders` hello IP-cím szakasz a helyi DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="adfbf-179">Replace hello `192.168.0.1` entry in hello `forwarders` section with hello IP address of your on-premises DNS server.</span></span> <span data-ttu-id="adfbf-180">A bejegyzés útvonalak DNS kérelmek tooyour a helyi DNS-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="adfbf-180">This entry routes DNS requests tooyour on-premises DNS server for resolution.</span></span>

    <span data-ttu-id="adfbf-181">tooedit ezt a fájlt, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="adfbf-181">tooedit this file, use hello following command:</span></span>

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    <span data-ttu-id="adfbf-182">toosave hello fájl használata __Ctrl + X__, __Y__, majd __Enter__.</span><span class="sxs-lookup"><span data-stu-id="adfbf-182">toosave hello file, use __Ctrl+X__, __Y__, and then __Enter__.</span></span>

4. <span data-ttu-id="adfbf-183">Hello SSH-munkamenetet használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="adfbf-183">From hello SSH session, use hello following command:</span></span>

    ```bash
    hostname -f
    ```

    <span data-ttu-id="adfbf-184">Ez a parancs visszaadja a szöveg a következő érték hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="adfbf-184">This command returns a value similar toohello following text:</span></span>

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    <span data-ttu-id="adfbf-185">Hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` szövege hello __DNS-utótag__ ehhez a virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="adfbf-185">hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` text is hello __DNS suffix__ for this virtual network.</span></span> <span data-ttu-id="adfbf-186">Mentse ezt az értéket, a rendszer később.</span><span class="sxs-lookup"><span data-stu-id="adfbf-186">Save this value, as it is used later.</span></span>

5. <span data-ttu-id="adfbf-187">tooconfigure Bind tooresolve DNS-nevek erőforrások hello virtuális hálózatról, használja a szöveg hello hello tartalmát, a következő hello `/etc/bind/named.conf.local` fájlt:</span><span class="sxs-lookup"><span data-stu-id="adfbf-187">tooconfigure Bind tooresolve DNS names for resources within hello virtual network, use hello following text as hello contents of hello `/etc/bind/named.conf.local` file:</span></span>

        // Replace hello following with hello DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # hello Azure recursive resolver
        };

    > [!IMPORTANT]
    > <span data-ttu-id="adfbf-188">Le kell cserélnie hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` a korábban kapott hello DNS-utótagot.</span><span class="sxs-lookup"><span data-stu-id="adfbf-188">You must replace hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` with hello DNS suffix you retrieved earlier.</span></span>

    <span data-ttu-id="adfbf-189">tooedit ezt a fájlt, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="adfbf-189">tooedit this file, use hello following command:</span></span>

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    <span data-ttu-id="adfbf-190">toosave hello fájl használata __Ctrl + X__, __Y__, majd __Enter__.</span><span class="sxs-lookup"><span data-stu-id="adfbf-190">toosave hello file, use __Ctrl+X__, __Y__, and then __Enter__.</span></span>

6. <span data-ttu-id="adfbf-191">toostart Bind, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="adfbf-191">toostart Bind, use hello following command:</span></span>

    ```bash
    sudo service bind9 restart
    ```

7. <span data-ttu-id="adfbf-192">tooverify, amely a kötés képes névfeloldásra hello a helyszíni hálózat, a következő parancsok használata hello erőforrások:</span><span class="sxs-lookup"><span data-stu-id="adfbf-192">tooverify that bind can resolve hello names of resources in your on-premises network, use hello following commands:</span></span>

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="adfbf-193">Cserélje le `dns.mynetwork.net` a hello teljesen minősített tartománynevét (FQDN) a helyszíni hálózati erőforrások.</span><span class="sxs-lookup"><span data-stu-id="adfbf-193">Replace `dns.mynetwork.net` with hello fully qualified domain name (FQDN) of a resource in your on-premises network.</span></span>
    >
    > <span data-ttu-id="adfbf-194">Cserélje le `10.0.0.4` a hello __belső IP-cím__ a egyéni DNS-kiszolgáló hello virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="adfbf-194">Replace `10.0.0.4` with hello __internal IP address__ of your custom DNS server in hello virtual network.</span></span>

    <span data-ttu-id="adfbf-195">hello válasz jelenik meg a következő szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="adfbf-195">hello response appears similar toohello following text:</span></span>

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-hello-virtual-network-toouse-hello-custom-dns-server"></a><span data-ttu-id="adfbf-196">Hello virtuális hálózati toouse hello egyéni DNS-kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="adfbf-196">Configure hello virtual network toouse hello custom DNS server</span></span>

<span data-ttu-id="adfbf-197">tooconfigure hello virtuális hálózati toouse hello egyéni DNS-kiszolgáló helyett hello Azure rekurzív feloldó lépések hello használata:</span><span class="sxs-lookup"><span data-stu-id="adfbf-197">tooconfigure hello virtual network toouse hello custom DNS server instead of hello Azure recursive resolver, use hello following steps:</span></span>

1. <span data-ttu-id="adfbf-198">A hello [Azure-portálon](https://portal.azure.com), hello virtuális hálózatot, majd válassza ki és __DNS-kiszolgálók__.</span><span class="sxs-lookup"><span data-stu-id="adfbf-198">In hello [Azure portal](https://portal.azure.com), select hello virtual network, and then select __DNS Servers__.</span></span>

2. <span data-ttu-id="adfbf-199">Válassza ki __egyéni__, és írja be a hello __belső IP-cím__ hello egyéni DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="adfbf-199">Select __Custom__, and enter hello __internal IP address__ of hello custom DNS server.</span></span> <span data-ttu-id="adfbf-200">Végül válassza ki __mentése__.</span><span class="sxs-lookup"><span data-stu-id="adfbf-200">Finally, select __Save__.</span></span>

    ![Hello hálózati hello egyéni DNS-kiszolgáló beállítása](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-hello-on-premises-dns-server"></a><span data-ttu-id="adfbf-202">Hello a helyi DNS-kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="adfbf-202">Configure hello on-premises DNS server</span></span>

<span data-ttu-id="adfbf-203">Hello előző szakaszban hello egyéni DNS server tooforward kérelmek toohello a helyi DNS-kiszolgáló konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="adfbf-203">In hello previous section, you configured hello custom DNS server tooforward requests toohello on-premises DNS server.</span></span> <span data-ttu-id="adfbf-204">Ezután konfigurálnia kell hello helyszíni DNS server tooforward kérelmek toohello egyéni DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="adfbf-204">Next, you must configure hello on-premises DNS server tooforward requests toohello custom DNS server.</span></span>

<span data-ttu-id="adfbf-205">Adott útmutatást a tooconfigure a DNS-kiszolgáló, a DNS-kiszolgáló szoftver hello dokumentációban.</span><span class="sxs-lookup"><span data-stu-id="adfbf-205">For specific steps on how tooconfigure your DNS server, consult hello documentation for your DNS server software.</span></span> <span data-ttu-id="adfbf-206">Hello bemutatjuk, hogyan keresse meg tooconfigure egy __feltételes továbbítók__.</span><span class="sxs-lookup"><span data-stu-id="adfbf-206">Look for hello steps on how tooconfigure a __conditional forwarder__.</span></span>

<span data-ttu-id="adfbf-207">Feltételes továbbítás csak egy kapcsolatspecifikus DNS-utótag kérelmek továbbítja.</span><span class="sxs-lookup"><span data-stu-id="adfbf-207">A conditional forward only forwards requests for a specific DNS suffix.</span></span> <span data-ttu-id="adfbf-208">Ebben az esetben konfigurálnia kell egy továbbítót hello DNS-utótag hello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="adfbf-208">In this case, you must configure a forwarder for hello DNS suffix of hello virtual network.</span></span> <span data-ttu-id="adfbf-209">Ez utótag kérelmek továbbítani toohello hello egyéni DNS-kiszolgáló IP-címét.</span><span class="sxs-lookup"><span data-stu-id="adfbf-209">Requests for this suffix should be forwarded toohello IP address of hello custom DNS server.</span></span> 

<span data-ttu-id="adfbf-210">hello következő szöveg látható egy példa egy feltételes továbbítók konfigurációja hello **kötési** DNS szoftver:</span><span class="sxs-lookup"><span data-stu-id="adfbf-210">hello following text is an example of a conditional forwarder configuration for hello **Bind** DNS software:</span></span>

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello custom DNS server's internal IP address
    };

<span data-ttu-id="adfbf-211">A DNS-sel kapcsolatos **Windows Server 2016**, lásd: hello [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) dokumentáció...</span><span class="sxs-lookup"><span data-stu-id="adfbf-211">For information on using DNS on **Windows Server 2016**, see hello [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) documentation...</span></span>

<span data-ttu-id="adfbf-212">Miután konfigurálta a hello a helyi DNS-kiszolgáló, használhatja `nslookup` hello virtuális hálózat nevét, hogy hogyan oldható hello a helyszíni hálózati tooverify a.</span><span class="sxs-lookup"><span data-stu-id="adfbf-212">Once you have configured hello on-premises DNS server, you can use `nslookup` from hello on-premises network tooverify that you can resolve names in hello virtual network.</span></span> <span data-ttu-id="adfbf-213">a következő példa hello</span><span class="sxs-lookup"><span data-stu-id="adfbf-213">hello following example</span></span> 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

<span data-ttu-id="adfbf-214">Ebben a példában, használja a helyi DNS-kiszolgálójának 196.168.0.4 hello tooresolve hello hello egyéni DNS-kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="adfbf-214">This example uses hello on-premises DNS server at 196.168.0.4 tooresolve hello name of hello custom DNS server.</span></span> <span data-ttu-id="adfbf-215">Cserélje le hello IP-cím hello hello a helyi DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="adfbf-215">Replace hello IP address with hello one for hello on-premises DNS server.</span></span> <span data-ttu-id="adfbf-216">Cserélje le a hello `dnsproxy` hello teljesen minősített tartománynevét hello egyéni DNS-kiszolgáló címet.</span><span class="sxs-lookup"><span data-stu-id="adfbf-216">Replace hello `dnsproxy` address with hello fully qualified domain name of hello custom DNS server.</span></span>

## <a name="optional-control-network-traffic"></a><span data-ttu-id="adfbf-217">Választható lehetőség: Vezérlő hálózati forgalom</span><span class="sxs-lookup"><span data-stu-id="adfbf-217">Optional: Control network traffic</span></span>

<span data-ttu-id="adfbf-218">Hálózati biztonsági csoportokkal (NSG) vagy a felhasználó által definiált útvonalak (UDR) toocontrol hálózati forgalmat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="adfbf-218">You can use network security groups (NSG) or user-defined routes (UDR) toocontrol network traffic.</span></span> <span data-ttu-id="adfbf-219">Az NSG-k lehetővé teszik toofilter bejövő és kimenő forgalmat, és adatforgalom engedélyezéséhez vagy letiltásához hello.</span><span class="sxs-lookup"><span data-stu-id="adfbf-219">NSGs allow you toofilter inbound and outbound traffic, and allow or deny hello traffic.</span></span> <span data-ttu-id="adfbf-220">Udr-EK hogyan a forgalom között hello virtuális hálózatán lévő erőforrásokat toocontrol lehetővé teszik, hello internet és a helyszíni hálózat hello.</span><span class="sxs-lookup"><span data-stu-id="adfbf-220">UDRs allow you toocontrol how traffic flows between resources in hello virtual network, hello internet, and hello on-premises network.</span></span>

> [!WARNING]
> <span data-ttu-id="adfbf-221">HDInsight hello Azure felhőben az adott IP-címekről érkező bejövő hozzáférést, és korlátlan kimenő hozzáférést igényel.</span><span class="sxs-lookup"><span data-stu-id="adfbf-221">HDInsight requires inbound access from specific IP addresses in hello Azure cloud, and unrestricted outbound access.</span></span> <span data-ttu-id="adfbf-222">Az NSG-k vagy udr-EK toocontrol forgalom használatakor hello a következő lépéseket kell végrehajtania:</span><span class="sxs-lookup"><span data-stu-id="adfbf-222">When using NSGs or UDRs toocontrol traffic, you must perform hello following steps:</span></span>
>
> 1. <span data-ttu-id="adfbf-223">Hello IP-címek található hello helyre, amely tartalmazza a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="adfbf-223">Find hello IP addresses for hello location that contains your virtual network.</span></span> <span data-ttu-id="adfbf-224">Hely szerint szükséges IP-címek listáját lásd: [szükséges IP-címek](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).</span><span class="sxs-lookup"><span data-stu-id="adfbf-224">For a list of required IPs by location, see [Required IP addresses](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).</span></span>
>
> 2. <span data-ttu-id="adfbf-225">Hello IP-címekről érkező bejövő forgalom engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="adfbf-225">Allow inbound traffic from hello IP addresses.</span></span>
>
>    * <span data-ttu-id="adfbf-226">__NSG__: engedélyezése __bejövő__ porton forgalom __443-as__ a hello __Internet__.</span><span class="sxs-lookup"><span data-stu-id="adfbf-226">__NSG__: Allow __inbound__ traffic on port __443__ from hello __Internet__.</span></span>
>    * <span data-ttu-id="adfbf-227">__UDR__: Set hello __következő ugrásaként__ hello útvonal too__Internet__ típusú.</span><span class="sxs-lookup"><span data-stu-id="adfbf-227">__UDR__: Set hello __Next Hop__ type of hello route too__Internet__.</span></span>

<span data-ttu-id="adfbf-228">Például egy Azure PowerShell vagy a hello Azure CLI toocreate NSG-k segítségével, tekintse meg a hello [kiterjesztése HDInsight az Azure virtuális hálózatokkal](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="adfbf-228">For an example of using Azure PowerShell or hello Azure CLI toocreate NSGs, see hello [Extend HDInsight with Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) document.</span></span>

## <a name="create-hello-hdinsight-cluster"></a><span data-ttu-id="adfbf-229">Hello HDInsight-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="adfbf-229">Create hello HDInsight cluster</span></span>

> [!WARNING]
> <span data-ttu-id="adfbf-230">A virtuális hálózati hello HDInsight telepítése előtt konfigurálnia kell hello egyéni DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="adfbf-230">You must configure hello custom DNS server before installing HDInsight in hello virtual network.</span></span>

<span data-ttu-id="adfbf-231">Hello használata hello szükséges lépések [hello Azure-portál használatával HDInsight-fürtök létrehozása](./hdinsight-hadoop-create-linux-clusters-portal.md) dokumentum toocreate HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="adfbf-231">Use hello steps in hello [Create an HDInsight cluster using hello Azure portal](./hdinsight-hadoop-create-linux-clusters-portal.md) document toocreate an HDInsight cluster.</span></span>

> [!WARNING]
> * <span data-ttu-id="adfbf-232">Fürt létrehozása során ki kell választania, amely tartalmazza a virtuális hálózat hello helyét.</span><span class="sxs-lookup"><span data-stu-id="adfbf-232">During cluster creation, you must choose hello location that contains your virtual network.</span></span>
>
> * <span data-ttu-id="adfbf-233">A hello __speciális beállítások__ rész-konfiguráció, ki kell választania hello virtuális hálózat és a korábban létrehozott alhálózati.</span><span class="sxs-lookup"><span data-stu-id="adfbf-233">In hello __Advanced settings__ part of configuration, you must select hello virtual network and subnet that you created earlier.</span></span>

## <a name="connecting-toohdinsight"></a><span data-ttu-id="adfbf-234">Csatlakozás tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="adfbf-234">Connecting tooHDInsight</span></span>

<span data-ttu-id="adfbf-235">A legtöbb dokumentáció a HDInsight feltételezi, hogy rendelkezik-e hozzáférési toohello fürt keresztül hello internet.</span><span class="sxs-lookup"><span data-stu-id="adfbf-235">Most documentation on HDInsight assumes that you have access toohello cluster over hello internet.</span></span> <span data-ttu-id="adfbf-236">Például, hogy csatlakozhasson https://CLUSTERNAME.azurehdinsight.net toohello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="adfbf-236">For example, that you can connect toohello cluster at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="adfbf-237">A cím hello nyilvános átjárón, amely esetén nem érhető el, használja az NSG-k vagy udr-EK toorestrict a hozzáférést a hello internet használja.</span><span class="sxs-lookup"><span data-stu-id="adfbf-237">This address uses hello public gateway, which is not available if you have used NSGs or UDRs toorestrict access from hello internet.</span></span>

<span data-ttu-id="adfbf-238">toodirectly tooHDInsight hello virtuális hálózaton keresztül csatlakozni, használja a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="adfbf-238">toodirectly connect tooHDInsight through hello virtual network, use hello following steps:</span></span>

1. <span data-ttu-id="adfbf-239">toodiscover hello belső teljes tartománynevek hello HDInsight-csomópont, a hello a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="adfbf-239">toodiscover hello internal fully qualified domain names of hello HDInsight cluster nodes, use one of hello following methods:</span></span>

    ```powershell
    $resourceGroupName = "hello resource group that contains hello virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

2. <span data-ttu-id="adfbf-240">toodetermine hello port, amelyet a szolgáltatás érhető el, lásd: hello [a HDInsight Hadoop-szolgáltatás által használt portok](./hdinsight-hadoop-port-settings-for-services.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="adfbf-240">toodetermine hello port that a service is available on, see hello [Ports used by Hadoop services on HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="adfbf-241">Egyes hello átjárócsomópontokkal üzemeltetett szolgáltatások aktívak csak az egyik csomópont egyszerre.</span><span class="sxs-lookup"><span data-stu-id="adfbf-241">Some services hosted on hello head nodes are only active on one node at a time.</span></span> <span data-ttu-id="adfbf-242">Ha megpróbál egy központi csomóponton szolgáltatások elérésére, és sikertelen lesz, váltás más átjárócsomópont toohello.</span><span class="sxs-lookup"><span data-stu-id="adfbf-242">If you try accessing a service on one head node and it fails, switch toohello other head node.</span></span>
    >
    > <span data-ttu-id="adfbf-243">Például Ambari csak egy központi csomóponton aktív egyszerre lehet.</span><span class="sxs-lookup"><span data-stu-id="adfbf-243">For example, Ambari is only active on one head node at a time.</span></span> <span data-ttu-id="adfbf-244">Ha egy központi csomóponton Ambari eléréséhez adja vissza, 404 hibaüzenetet, majd a futtató hello más átjárócsomópont.</span><span class="sxs-lookup"><span data-stu-id="adfbf-244">If you try accessing Ambari on one head node and it returns a 404 error, then it is running on hello other head node.</span></span>

## <a name="next-steps"></a><span data-ttu-id="adfbf-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="adfbf-245">Next steps</span></span>

* <span data-ttu-id="adfbf-246">A HDInsight eszközzel virtuális hálózatban további információkért lásd: [kiterjesztése HDInsight Azure virtuális hálózatok használatával](./hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="adfbf-246">For more information on using HDInsight in a virtual network, see [Extend HDInsight by using Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span></span>

* <span data-ttu-id="adfbf-247">Egy Azure virtuális hálózatot további információkért lásd: hello [Azure Virtual Network áttekintése](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="adfbf-247">For more information on Azure virtual networks, see hello [Azure Virtual Network overview](../virtual-network/virtual-networks-overview.md).</span></span>

* <span data-ttu-id="adfbf-248">Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="adfbf-248">For more information on network security groups, see [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="adfbf-249">A felhasználó által definiált útvonalak további információkért lásd: [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="adfbf-249">For more information on user-defined routes, see [USer-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>
