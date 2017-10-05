---
title: "HDInsight csatlakozni a helyszíni hálózatra - Azure HDInsight |} Microsoft Docs"
description: "Útmutató a HDInsight-fürtök létrehozása egy Azure virtuális hálózatra, és csatlakoztassa a helyszíni hálózat. Megtudhatja, hogyan konfigurálhatja a névfeloldást HDInsight és a helyszíni hálózat között egyéni DNS-kiszolgáló használatával."
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
ms.openlocfilehash: 6fc863010cc59e20e7d86ea9344489e574be75f2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-hdinsight-to-your-on-premise-network"></a><span data-ttu-id="6dca1-104">HDInsight csatlakozni a helyi hálózatra</span><span class="sxs-lookup"><span data-stu-id="6dca1-104">Connect HDInsight to your on-premise network</span></span>

<span data-ttu-id="6dca1-105">Útmutató a HDInsight hálózathoz való kapcsolódáshoz a helyszíni Azure virtuális hálózatok és a VPN-átjáró használatával.</span><span class="sxs-lookup"><span data-stu-id="6dca1-105">Learn how to connect HDInsight to your on-premises network by using Azure Virtual Networks and a VPN gateway.</span></span> <span data-ttu-id="6dca1-106">Ez a dokumentum a tervezési tudnivalókat tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="6dca1-106">This document provides planning information on:</span></span>

* <span data-ttu-id="6dca1-107">Egy Azure virtuális hálózatot, amely a helyszíni hálózat csatlakozik a HDInsight eszközzel.</span><span class="sxs-lookup"><span data-stu-id="6dca1-107">Using HDInsight in an Azure Virtual Network that connects to your on-premises network.</span></span>

* <span data-ttu-id="6dca1-108">Konfigurálja a DNS-név feloldása a virtuális hálózat és a helyszíni hálózat között.</span><span class="sxs-lookup"><span data-stu-id="6dca1-108">Configuring DNS name resolution between the virtual network and your on-premises network.</span></span>

* <span data-ttu-id="6dca1-109">A HDInsight internet-hozzáférés korlátozása a hálózati biztonsági csoportok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6dca1-109">Configuring network security groups to restrict internet access to HDInsight.</span></span>

* <span data-ttu-id="6dca1-110">A virtuális hálózaton a HDInsight által biztosított portok.</span><span class="sxs-lookup"><span data-stu-id="6dca1-110">Ports provided by HDInsight on the virtual network.</span></span>

## <a name="create-the-virtual-network-configuration"></a><span data-ttu-id="6dca1-111">A virtuális hálózati konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dca1-111">Create the Virtual network configuration</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6dca1-112">Ha a részletes útmutatót a csatlakozó HDInsight a helyszíni hálózat az Azure virtuális hálózat használatával, lásd: a [a helyi hálózathoz való csatlakozás HDInsight](connect-on-premises-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="6dca1-112">If you are looking for step by step guidance on connecting HDInsight to your on-premises network using an Azure Virtual Network, see the [Connect HDInsight to your on-premise network](connect-on-premises-network.md) document.</span></span>

<span data-ttu-id="6dca1-113">A következő dokumentumok segítségével megtudhatja, hogyan hozzon létre egy Azure virtuális hálózatot, amely a helyszíni hálózat csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="6dca1-113">Use the following documents to learn how to create an Azure Virtual Network that is connected to your on-premises network:</span></span>
    
* [<span data-ttu-id="6dca1-114">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="6dca1-114">Using the Azure portal</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [<span data-ttu-id="6dca1-115">Az Azure PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="6dca1-115">Using Azure PowerShell</span></span>](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [<span data-ttu-id="6dca1-116">Az Azure parancssori felület használata</span><span class="sxs-lookup"><span data-stu-id="6dca1-116">Using Azure CLI</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a><span data-ttu-id="6dca1-117">Névfeloldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6dca1-117">Configure name resolution</span></span>

<span data-ttu-id="6dca1-118">A csatlakoztatott hálózati neve történő kommunikációra HDInsight és erőforrások engedélyezéséhez el kell végeznie a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="6dca1-118">To allow HDInsight and resources in the joined network to communicate by name, you must perform the following actions:</span></span>

* <span data-ttu-id="6dca1-119">Hozzon létre egy egyéni DNS-kiszolgáló Azure virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="6dca1-119">Create a custom DNS server in the Azure Virtual Network.</span></span>

* <span data-ttu-id="6dca1-120">A virtuális hálózatot az egyéni DNS-kiszolgáló használata helyett az alapértelmezett Azure rekurzív feloldó konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="6dca1-120">Configure the virtual network to use the custom DNS server instead of the default Azure Recursive Resolver.</span></span>

* <span data-ttu-id="6dca1-121">Konfigurálja az egyéni DNS-kiszolgáló és a helyi DNS-kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="6dca1-121">Configure forwarding between the custom DNS server and your on-premises DNS server.</span></span>

<span data-ttu-id="6dca1-122">Ez a konfiguráció lehetővé teszi, hogy az alábbiakra:</span><span class="sxs-lookup"><span data-stu-id="6dca1-122">This configuration enables the following behavior:</span></span>

* <span data-ttu-id="6dca1-123">A teljes tartományneveit, amelyek rendelkeznek a DNS-utótag kérelmek __a virtuális hálózat__ jutnak el az egyéni DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6dca1-123">Requests for fully qualified domain names that have the DNS suffix __for the virtual network__ are forwarded to the custom DNS server.</span></span> <span data-ttu-id="6dca1-124">Az egyéni DNS-kiszolgáló majd továbbítja ezeket a kérelmeket az Azure rekurzív feloldó, amely az IP-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="6dca1-124">The custom DNS server then forwards these requests to the Azure Recursive Resolver, which returns the IP address.</span></span>

* <span data-ttu-id="6dca1-125">Minden más kérelemhez továbbítja a helyi DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6dca1-125">All other requests are forwarded to the on-premises DNS server.</span></span> <span data-ttu-id="6dca1-126">A nyilvános internet erőforrások, például a microsoft.com még akkor is, kérelmeket a rendszer a helyi DNS-kiszolgálót a névfeloldáshoz továbbítja.</span><span class="sxs-lookup"><span data-stu-id="6dca1-126">Even requests for public internet resources such as microsoft.com are forwarded to the on-premises DNS server for name resolution.</span></span>

<span data-ttu-id="6dca1-127">Az alábbi ábra a zöld sorai, hogy a virtuális hálózat DNS-utótag erőforrásokra vonatkozó kéréseket.</span><span class="sxs-lookup"><span data-stu-id="6dca1-127">In the following diagram, green lines are requests for resources that end in the DNS suffix of the virtual network.</span></span> <span data-ttu-id="6dca1-128">Kék sorai a helyszíni hálózat vagy a nyilvános interneten erőforrásokra vonatkozó kéréseket.</span><span class="sxs-lookup"><span data-stu-id="6dca1-128">Blue lines are requests for resources in the on-premises network or on the public internet.</span></span>

![A konfigurációban a dokumentumban használt DNS-kérelmek feloldása ábrája](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a><span data-ttu-id="6dca1-130">Hozzon létre egy egyéni DNS-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="6dca1-130">Create a custom DNS server</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6dca1-131">Hozzon létre és a DNS-kiszolgáló a HDInsight a virtuális hálózaton történő telepítése előtt konfigurálnia kell.</span><span class="sxs-lookup"><span data-stu-id="6dca1-131">You must create and configure the DNS server before installing HDInsight into the virtual network.</span></span>

<span data-ttu-id="6dca1-132">Linux virtuális gép által használt létrehozásához a [kötési](https://www.isc.org/downloads/bind/) DNS szoftver, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6dca1-132">To create a Linux VM that uses the [Bind](https://www.isc.org/downloads/bind/) DNS software, use the following steps:</span></span>

> [!NOTE]
> <span data-ttu-id="6dca1-133">Az alábbi lépéseket használja a [Azure-portálon](https://portal.azure.com) egy Azure virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6dca1-133">The following steps use the [Azure portal](https://portal.azure.com) to create an Azure Virtual Machine.</span></span> <span data-ttu-id="6dca1-134">Egyéb módon hozhat létre a virtuális gép, tekintse meg a [hozzon létre Virtuálisgép - Azure CLI](../virtual-machines/linux/quick-create-cli.md) és [hozzon létre Virtuálisgép - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="6dca1-134">For other ways to create a virtual machine, see the [Create VM - Azure CLI](../virtual-machines/linux/quick-create-cli.md) and [Create VM - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) documents.</span></span>

1. <span data-ttu-id="6dca1-135">Az a [Azure-portálon](https://portal.azure.com), jelölje be  __+__ , __számítási__, és __Ubuntu Server 16.04 LTS__.</span><span class="sxs-lookup"><span data-stu-id="6dca1-135">From the [Azure portal](https://portal.azure.com), select __+__, __Compute__, and __Ubuntu Server 16.04 LTS__.</span></span>

    ![Egy Ubuntu virtuális gép létrehozása](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. <span data-ttu-id="6dca1-137">Az a __alapjai__ területen adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="6dca1-137">From the __Basics__ section, enter the following information:</span></span>

    * <span data-ttu-id="6dca1-138">__Név__: egy rövid nevet, amely azonosítja a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="6dca1-138">__Name__: A friendly name that identifies this virtual machine.</span></span> <span data-ttu-id="6dca1-139">Például __DNSProxy__.</span><span class="sxs-lookup"><span data-stu-id="6dca1-139">For example, __DNSProxy__.</span></span>
    * <span data-ttu-id="6dca1-140">__Felhasználónév__: SSH-fiókjának nevét.</span><span class="sxs-lookup"><span data-stu-id="6dca1-140">__User name__: The name of the SSH account.</span></span>
    * <span data-ttu-id="6dca1-141">__Nyilvános SSH-kulcs__ vagy __jelszó__: az SSH-fiókhoz tartozó hitelesítési módszer.</span><span class="sxs-lookup"><span data-stu-id="6dca1-141">__SSH public key__ or __Password__: The authentication method for the SSH account.</span></span> <span data-ttu-id="6dca1-142">Azt javasoljuk, nyilvános kulcsok, mert azok biztonságosabb.</span><span class="sxs-lookup"><span data-stu-id="6dca1-142">We recommend using public keys, as they are more secure.</span></span> <span data-ttu-id="6dca1-143">További információkért lásd: a [Linux virtuális gépek létrehozása és használata SSH-kulcsok](../virtual-machines/linux/mac-create-ssh-keys.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="6dca1-143">For more information, see the [Create and use SSH keys for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md) document.</span></span>
    * <span data-ttu-id="6dca1-144">__Erőforráscsoport__: válasszon __meglévő__, majd válassza ki a korábban létrehozott virtuális hálózat tartalmazó erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="6dca1-144">__Resource group__: Select __Use existing__, and then select the resource group that contains the virtual network created earlier.</span></span>
    * <span data-ttu-id="6dca1-145">__Hely__: Jelölje ki a virtuális hálózat ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="6dca1-145">__Location__: Select the same location as the virtual network.</span></span>

    ![Alapszintű virtuálisgép-konfiguráció](./media/connect-on-premises-network/vm-basics.png)

    <span data-ttu-id="6dca1-147">Más bejegyzések hagyja meg az alapértelmezett értékeket, és válassza ki __OK__.</span><span class="sxs-lookup"><span data-stu-id="6dca1-147">Leave other entries at the default values and then select __OK__.</span></span>

3. <span data-ttu-id="6dca1-148">Az a __méret kiválasztása__ területen válassza ki a Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="6dca1-148">From the __Choose a size__ section, select the VM size.</span></span> <span data-ttu-id="6dca1-149">A jelen oktatóanyag esetében válassza legkisebb és a legalacsonyabb költséget.</span><span class="sxs-lookup"><span data-stu-id="6dca1-149">For this tutorial, select the smallest and lowest cost option.</span></span> <span data-ttu-id="6dca1-150">A folytatáshoz használja a __válasszon__ gombra.</span><span class="sxs-lookup"><span data-stu-id="6dca1-150">To continue, use the __Select__ button.</span></span>

4. <span data-ttu-id="6dca1-151">Az a __beállítások__ területen adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="6dca1-151">From the __Settings__ section, enter the following information:</span></span>

    * <span data-ttu-id="6dca1-152">__Virtuális hálózati__: válassza ki a korábban létrehozott virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="6dca1-152">__Virtual network__: Select the virtual network that you created earlier.</span></span>

    * <span data-ttu-id="6dca1-153">__Alhálózati__: Jelölje ki az alapértelmezett alhálózatot a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="6dca1-153">__Subnet__: Select the default subnet for the virtual network.</span></span> <span data-ttu-id="6dca1-154">Tegye __nem__ jelölje ki az alhálózatot, a VPN-átjáró által használt.</span><span class="sxs-lookup"><span data-stu-id="6dca1-154">Do __not__ select the subnet used by the VPN gateway.</span></span>

    * <span data-ttu-id="6dca1-155">__Diagnosztikai tárfiók__: Válasszon egy meglévő tárfiókot használ, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="6dca1-155">__Diagnostics storage account__: Either select an existing storage account or create a new one.</span></span>

    ![Virtuális hálózati beállítások](./media/connect-on-premises-network/virtual-network-settings.png)

    <span data-ttu-id="6dca1-157">A többi bejegyzés hagyja az alapértelmezett értéket, majd válasszon __OK__ folytatja.</span><span class="sxs-lookup"><span data-stu-id="6dca1-157">Leave the other entries at the default value, then select __OK__ to continue.</span></span>

5. <span data-ttu-id="6dca1-158">Az a __beszerzési__ szakaszban jelölje be a __beszerzési__ a virtuális gép létrehozása gombra.</span><span class="sxs-lookup"><span data-stu-id="6dca1-158">From the __Purchase__ section, select the __Purchase__ button to create the virtual machine.</span></span>

6. <span data-ttu-id="6dca1-159">A virtuális gép létrehozása után annak __áttekintése__ szakasz jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6dca1-159">Once the virtual machine has been created, its __Overview__ section is displayed.</span></span> <span data-ttu-id="6dca1-160">Válassza ki a listáról a bal oldali __tulajdonságok__.</span><span class="sxs-lookup"><span data-stu-id="6dca1-160">From the list on the left, select __Properties__.</span></span> <span data-ttu-id="6dca1-161">Mentse a __nyilvános IP-cím__ és __magánhálózati IP-cím__ értékeket.</span><span class="sxs-lookup"><span data-stu-id="6dca1-161">Save the __Public IP address__ and __Private IP address__ values.</span></span> <span data-ttu-id="6dca1-162">A következő szakaszban fogja használni.</span><span class="sxs-lookup"><span data-stu-id="6dca1-162">It will be used in the next section.</span></span>

    ![Nyilvános és magánhálózati IP-címek](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a><span data-ttu-id="6dca1-164">Telepítse és konfigurálja a Bind (DNS szoftver)</span><span class="sxs-lookup"><span data-stu-id="6dca1-164">Install and configure Bind (DNS software)</span></span>

1. <span data-ttu-id="6dca1-165">SSH használatával csatlakozhat a __nyilvános IP-cím__ a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6dca1-165">Use SSH to connect to the __public IP address__ of the virtual machine.</span></span> <span data-ttu-id="6dca1-166">Az alábbi példában egy virtuális gépet, 40.68.254.142 csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="6dca1-166">The following example connects to a virtual machine at 40.68.254.142:</span></span>

    ```bash
    ssh sshuser@40.68.254.142
    ```

    <span data-ttu-id="6dca1-167">Cserélje le `sshuser` a fürt létrehozásakor megadott SSH a felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="6dca1-167">Replace `sshuser` with the SSH user account you specified when creating the cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6dca1-168">Nincsenek az sokféleképpen beszerzése a `ssh` segédprogram.</span><span class="sxs-lookup"><span data-stu-id="6dca1-168">There are a variety of ways to obtain the `ssh` utility.</span></span> <span data-ttu-id="6dca1-169">A Linux, Unix és macOS Ez biztosítja az operációs rendszer részeként.</span><span class="sxs-lookup"><span data-stu-id="6dca1-169">On Linux, Unix, and macOS, it is provided as part of the operating system.</span></span> <span data-ttu-id="6dca1-170">Ha Windows használja, fontolja meg az alábbi lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="6dca1-170">If you are using Windows, consider one of the following options:</span></span>
    >
    > * [<span data-ttu-id="6dca1-171">Azure-felhőbe rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="6dca1-171">Azure Cloud Shell</span></span>](../cloud-shell/quickstart.md)
    > * [<span data-ttu-id="6dca1-172">A Windows 10 Ubuntu bash</span><span class="sxs-lookup"><span data-stu-id="6dca1-172">Bash on Ubuntu on Windows 10</span></span>](https://msdn.microsoft.com/commandline/wsl/about)
    > * [<span data-ttu-id="6dca1-173">Git (https://git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="6dca1-173">Git (https://git-scm.com/)</span></span>](https://git-scm.com/)
    > * [<span data-ttu-id="6dca1-174">OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)</span><span class="sxs-lookup"><span data-stu-id="6dca1-174">OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)</span></span>](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. <span data-ttu-id="6dca1-175">A kötés telepítéséhez használja az SSH-munkamenetet a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="6dca1-175">To install Bind, use the following commands from the SSH session:</span></span>

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. <span data-ttu-id="6dca1-176">Bind továbbítani a névfeloldási kérelmeket a helyszíni DNS-kiszolgáló konfigurálásához használja a következő szöveget a tartalmát a `/etc/bind/named.conf.options` fájlt:</span><span class="sxs-lookup"><span data-stu-id="6dca1-176">To configure Bind to forward name resolution requests to your on-prem DNS server, use the following text as the contents of the `/etc/bind/named.conf.options` file:</span></span>

        acl goodclients {
            10.0.0.0/16; # Replace with the IP address range of the virtual network
            10.1.0.0/16; # Replace with the IP address range of the on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with the IP address of the on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform to RFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > <span data-ttu-id="6dca1-177">Cserélje le az értékeket a `goodclients` a virtuális hálózat és a helyi hálózaton az IP-címtartománnyal rendelkező szakasz.</span><span class="sxs-lookup"><span data-stu-id="6dca1-177">Replace the values in the `goodclients` section with the IP address range of the virtual network and on-premises network.</span></span> <span data-ttu-id="6dca1-178">Ebben a szakaszban határozza meg a címeket, DNS-kiszolgáló elfogadja az érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="6dca1-178">This section defines the addresses that this DNS server accepts requests from.</span></span>
    >
    > <span data-ttu-id="6dca1-179">Cserélje le a `192.168.0.1` bejegyzést a `forwarders` a helyi DNS-kiszolgáló IP-című szakasz.</span><span class="sxs-lookup"><span data-stu-id="6dca1-179">Replace the `192.168.0.1` entry in the `forwarders` section with the IP address of your on-premises DNS server.</span></span> <span data-ttu-id="6dca1-180">Ez a bejegyzés a helyi DNS-kiszolgálót továbbítja a DNS-kérésekre.</span><span class="sxs-lookup"><span data-stu-id="6dca1-180">This entry routes DNS requests to your on-premises DNS server for resolution.</span></span>

    <span data-ttu-id="6dca1-181">Ez a fájl szerkesztéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="6dca1-181">To edit this file, use the following command:</span></span>

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    <span data-ttu-id="6dca1-182">Mentse a fájlt, használja a __Ctrl + X__, __Y__, majd __Enter__.</span><span class="sxs-lookup"><span data-stu-id="6dca1-182">To save the file, use __Ctrl+X__, __Y__, and then __Enter__.</span></span>

4. <span data-ttu-id="6dca1-183">Az SSH-munkamenetből használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="6dca1-183">From the SSH session, use the following command:</span></span>

    ```bash
    hostname -f
    ```

    <span data-ttu-id="6dca1-184">Ez a parancs értéket ad vissza az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="6dca1-184">This command returns a value similar to the following text:</span></span>

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    <span data-ttu-id="6dca1-185">A `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` szöveg a __DNS-utótag__ ehhez a virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="6dca1-185">The `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` text is the __DNS suffix__ for this virtual network.</span></span> <span data-ttu-id="6dca1-186">Mentse ezt az értéket, a rendszer később.</span><span class="sxs-lookup"><span data-stu-id="6dca1-186">Save this value, as it is used later.</span></span>

5. <span data-ttu-id="6dca1-187">Konfigurálja a DNS-nevek a virtuális hálózaton lévő erőforrások köti, használja a következő szöveg tartalmát a `/etc/bind/named.conf.local` fájlt:</span><span class="sxs-lookup"><span data-stu-id="6dca1-187">To configure Bind to resolve DNS names for resources within the virtual network, use the following text as the contents of the `/etc/bind/named.conf.local` file:</span></span>

        // Replace the following with the DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # The Azure recursive resolver
        };

    > [!IMPORTANT]
    > <span data-ttu-id="6dca1-188">Le kell cserélnie a `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` a korábban kapott DNS-utótagját.</span><span class="sxs-lookup"><span data-stu-id="6dca1-188">You must replace the `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` with the DNS suffix you retrieved earlier.</span></span>

    <span data-ttu-id="6dca1-189">Ez a fájl szerkesztéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="6dca1-189">To edit this file, use the following command:</span></span>

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    <span data-ttu-id="6dca1-190">Mentse a fájlt, használja a __Ctrl + X__, __Y__, majd __Enter__.</span><span class="sxs-lookup"><span data-stu-id="6dca1-190">To save the file, use __Ctrl+X__, __Y__, and then __Enter__.</span></span>

6. <span data-ttu-id="6dca1-191">Indítsa el a kötési, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="6dca1-191">To start Bind, use the following command:</span></span>

    ```bash
    sudo service bind9 restart
    ```

7. <span data-ttu-id="6dca1-192">Győződjön meg arról, hogy a kötés képes névfeloldásra a helyszíni hálózati erőforrások, az alábbi parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="6dca1-192">To verify that bind can resolve the names of resources in your on-premises network, use the following commands:</span></span>

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="6dca1-193">Cserélje le `dns.mynetwork.net` a helyszíni hálózat erőforrás teljes tartománynevét (FQDN).</span><span class="sxs-lookup"><span data-stu-id="6dca1-193">Replace `dns.mynetwork.net` with the fully qualified domain name (FQDN) of a resource in your on-premises network.</span></span>
    >
    > <span data-ttu-id="6dca1-194">Cserélje le `10.0.0.4` rendelkező a __belső IP-cím__ a egyéni DNS-kiszolgáló a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="6dca1-194">Replace `10.0.0.4` with the __internal IP address__ of your custom DNS server in the virtual network.</span></span>

    <span data-ttu-id="6dca1-195">A válasz megjelenik az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="6dca1-195">The response appears similar to the following text:</span></span>

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a><span data-ttu-id="6dca1-196">Az egyéni DNS-kiszolgáló használatára a virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6dca1-196">Configure the virtual network to use the custom DNS server</span></span>

<span data-ttu-id="6dca1-197">A virtuális hálózat az egyéni DNS-kiszolgáló használata helyett az Azure rekurzív feloldó konfigurálásához használja az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6dca1-197">To configure the virtual network to use the custom DNS server instead of the Azure recursive resolver, use the following steps:</span></span>

1. <span data-ttu-id="6dca1-198">Az a [Azure-portálon](https://portal.azure.com), a virtuális hálózat, majd válassza ki és __DNS-kiszolgálók__.</span><span class="sxs-lookup"><span data-stu-id="6dca1-198">In the [Azure portal](https://portal.azure.com), select the virtual network, and then select __DNS Servers__.</span></span>

2. <span data-ttu-id="6dca1-199">Válassza ki __egyéni__, és írja be a __belső IP-cím__ az egyéni DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6dca1-199">Select __Custom__, and enter the __internal IP address__ of the custom DNS server.</span></span> <span data-ttu-id="6dca1-200">Végül válassza ki __mentése__.</span><span class="sxs-lookup"><span data-stu-id="6dca1-200">Finally, select __Save__.</span></span>

    ![Az egyéni DNS-kiszolgáló a hálózat beállítása](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-the-on-premises-dns-server"></a><span data-ttu-id="6dca1-202">A helyi DNS-kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6dca1-202">Configure the on-premises DNS server</span></span>

<span data-ttu-id="6dca1-203">Az előző szakaszban konfigurálta az egyéni DNS-kiszolgálót a helyi DNS-kiszolgáló kérelmeket továbbítja.</span><span class="sxs-lookup"><span data-stu-id="6dca1-203">In the previous section, you configured the custom DNS server to forward requests to the on-premises DNS server.</span></span> <span data-ttu-id="6dca1-204">Ezután konfigurálnia kell a helyi DNS-kiszolgálót az egyéni DNS-kiszolgáló kérelmeket továbbítja.</span><span class="sxs-lookup"><span data-stu-id="6dca1-204">Next, you must configure the on-premises DNS server to forward requests to the custom DNS server.</span></span>

<span data-ttu-id="6dca1-205">A DNS-kiszolgáló konfigurálásának részletes lépéseit tekintse meg a DNS-kiszolgáló szoftver dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6dca1-205">For specific steps on how to configure your DNS server, consult the documentation for your DNS server software.</span></span> <span data-ttu-id="6dca1-206">Keresse meg a lépéseket, hogyan kell beállítani egy __feltételes továbbítók__.</span><span class="sxs-lookup"><span data-stu-id="6dca1-206">Look for the steps on how to configure a __conditional forwarder__.</span></span>

<span data-ttu-id="6dca1-207">Feltételes továbbítás csak egy kapcsolatspecifikus DNS-utótag kérelmek továbbítja.</span><span class="sxs-lookup"><span data-stu-id="6dca1-207">A conditional forward only forwards requests for a specific DNS suffix.</span></span> <span data-ttu-id="6dca1-208">Ebben az esetben konfigurálnia kell a virtuális hálózat DNS-utótag továbbító.</span><span class="sxs-lookup"><span data-stu-id="6dca1-208">In this case, you must configure a forwarder for the DNS suffix of the virtual network.</span></span> <span data-ttu-id="6dca1-209">Ez utótag kérelmek továbbítani kell az egyéni DNS-kiszolgáló IP-címét.</span><span class="sxs-lookup"><span data-stu-id="6dca1-209">Requests for this suffix should be forwarded to the IP address of the custom DNS server.</span></span> 

<span data-ttu-id="6dca1-210">A következő szöveg látható egy példa feltételes továbbító konfigurációját a **kötési** DNS szoftver:</span><span class="sxs-lookup"><span data-stu-id="6dca1-210">The following text is an example of a conditional forwarder configuration for the **Bind** DNS software:</span></span>

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The custom DNS server's internal IP address
    };

<span data-ttu-id="6dca1-211">A DNS-sel kapcsolatos **Windows Server 2016**, tekintse meg a [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) dokumentáció...</span><span class="sxs-lookup"><span data-stu-id="6dca1-211">For information on using DNS on **Windows Server 2016**, see the [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) documentation...</span></span>

<span data-ttu-id="6dca1-212">Miután konfigurálta a helyi DNS-kiszolgáló, használhat `nslookup` ellenőrzése, hogy a virtuális hálózati nevekkel oldhatja a helyi hálózatról.</span><span class="sxs-lookup"><span data-stu-id="6dca1-212">Once you have configured the on-premises DNS server, you can use `nslookup` from the on-premises network to verify that you can resolve names in the virtual network.</span></span> <span data-ttu-id="6dca1-213">Az alábbi példa</span><span class="sxs-lookup"><span data-stu-id="6dca1-213">The following example</span></span> 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

<span data-ttu-id="6dca1-214">Ebben a példában a helyi DNS-kiszolgáló 196.168.0.4 feloldani a nevet az egyéni DNS-kiszolgáló használja.</span><span class="sxs-lookup"><span data-stu-id="6dca1-214">This example uses the on-premises DNS server at 196.168.0.4 to resolve the name of the custom DNS server.</span></span> <span data-ttu-id="6dca1-215">Cserélje le a helyi DNS-kiszolgáló egy IP-címét.</span><span class="sxs-lookup"><span data-stu-id="6dca1-215">Replace the IP address with the one for the on-premises DNS server.</span></span> <span data-ttu-id="6dca1-216">Cserélje le a `dnsproxy` címmel az egyéni DNS-kiszolgáló teljesen minősített tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="6dca1-216">Replace the `dnsproxy` address with the fully qualified domain name of the custom DNS server.</span></span>

## <a name="optional-control-network-traffic"></a><span data-ttu-id="6dca1-217">Választható lehetőség: Vezérlő hálózati forgalom</span><span class="sxs-lookup"><span data-stu-id="6dca1-217">Optional: Control network traffic</span></span>

<span data-ttu-id="6dca1-218">Hálózati biztonsági csoportokkal (NSG) vagy a felhasználó által definiált útvonalak (UDR) segítségével szabályozhatja a hálózati forgalom.</span><span class="sxs-lookup"><span data-stu-id="6dca1-218">You can use network security groups (NSG) or user-defined routes (UDR) to control network traffic.</span></span> <span data-ttu-id="6dca1-219">Az NSG-k lehetővé teszik a bejövő és kimenő forgalmat, és adatforgalom engedélyezéséhez vagy letiltásához a.</span><span class="sxs-lookup"><span data-stu-id="6dca1-219">NSGs allow you to filter inbound and outbound traffic, and allow or deny the traffic.</span></span> <span data-ttu-id="6dca1-220">Udr-EK engedélyezi, hogy hogyan a forgalom a virtuális hálózat, az internethez és a helyszíni hálózati erőforrások között szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="6dca1-220">UDRs allow you to control how traffic flows between resources in the virtual network, the internet, and the on-premises network.</span></span>

> [!WARNING]
> <span data-ttu-id="6dca1-221">HDInsight Azure felhőben adott IP-címekről érkező bejövő hozzáférést, és korlátlan kimenő hozzáférést igényel.</span><span class="sxs-lookup"><span data-stu-id="6dca1-221">HDInsight requires inbound access from specific IP addresses in the Azure cloud, and unrestricted outbound access.</span></span> <span data-ttu-id="6dca1-222">Ha az NSG-k vagy udr-EK segítségével szabályozza a forgalmat, a következő lépésekkel kell meg:</span><span class="sxs-lookup"><span data-stu-id="6dca1-222">When using NSGs or UDRs to control traffic, you must perform the following steps:</span></span>
>
> 1. <span data-ttu-id="6dca1-223">Az IP-címek keresése, amely tartalmazza a virtuális hálózat helyét.</span><span class="sxs-lookup"><span data-stu-id="6dca1-223">Find the IP addresses for the location that contains your virtual network.</span></span> <span data-ttu-id="6dca1-224">Hely szerint szükséges IP-címek listáját lásd: [szükséges IP-címek](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).</span><span class="sxs-lookup"><span data-stu-id="6dca1-224">For a list of required IPs by location, see [Required IP addresses](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).</span></span>
>
> 2. <span data-ttu-id="6dca1-225">Az IP-címekről érkező bejövő forgalom engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6dca1-225">Allow inbound traffic from the IP addresses.</span></span>
>
>    * <span data-ttu-id="6dca1-226">__NSG__: engedélyezése __bejövő__ porton forgalom __443-as__ a a __Internet__.</span><span class="sxs-lookup"><span data-stu-id="6dca1-226">__NSG__: Allow __inbound__ traffic on port __443__ from the __Internet__.</span></span>
>    * <span data-ttu-id="6dca1-227">__UDR__: állítsa be a __következő ugrásaként__ az útvonal típusú __Internet__.</span><span class="sxs-lookup"><span data-stu-id="6dca1-227">__UDR__: Set the __Next Hop__ type of the route to __Internet__.</span></span>

<span data-ttu-id="6dca1-228">Az NSG-k létrehozásához Azure PowerShell vagy az Azure parancssori felület használatának példája, tekintse meg a [kiterjesztése HDInsight az Azure virtuális hálózatokkal](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="6dca1-228">For an example of using Azure PowerShell or the Azure CLI to create NSGs, see the [Extend HDInsight with Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) document.</span></span>

## <a name="create-the-hdinsight-cluster"></a><span data-ttu-id="6dca1-229">A HDInsight-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dca1-229">Create the HDInsight cluster</span></span>

> [!WARNING]
> <span data-ttu-id="6dca1-230">A virtuális hálózat HDInsight telepítése előtt konfigurálnia kell az egyéni DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6dca1-230">You must configure the custom DNS server before installing HDInsight in the virtual network.</span></span>

<span data-ttu-id="6dca1-231">Az alábbi témakörben található lépésekkel a [az Azure portál használatával HDInsight-fürtök létrehozása](./hdinsight-hadoop-create-linux-clusters-portal.md) dokumentum hozhat létre HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="6dca1-231">Use the steps in the [Create an HDInsight cluster using the Azure portal](./hdinsight-hadoop-create-linux-clusters-portal.md) document to create an HDInsight cluster.</span></span>

> [!WARNING]
> * <span data-ttu-id="6dca1-232">Fürt létrehozása során ki kell választania, amely tartalmazza a virtuális hálózat helyét.</span><span class="sxs-lookup"><span data-stu-id="6dca1-232">During cluster creation, you must choose the location that contains your virtual network.</span></span>
>
> * <span data-ttu-id="6dca1-233">Az a __speciális beállítások__ rész-konfiguráció, ki kell választania a virtuális hálózat és a korábban létrehozott alhálózati.</span><span class="sxs-lookup"><span data-stu-id="6dca1-233">In the __Advanced settings__ part of configuration, you must select the virtual network and subnet that you created earlier.</span></span>

## <a name="connecting-to-hdinsight"></a><span data-ttu-id="6dca1-234">HDInsight csatlakozik</span><span class="sxs-lookup"><span data-stu-id="6dca1-234">Connecting to HDInsight</span></span>

<span data-ttu-id="6dca1-235">A legtöbb dokumentáció a HDInsight feltételezi, hogy rendelkezik-e a fürt eléréséhez az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="6dca1-235">Most documentation on HDInsight assumes that you have access to the cluster over the internet.</span></span> <span data-ttu-id="6dca1-236">Például, hogy a fürthöz https://CLUSTERNAME.azurehdinsight.net kapcsolódhat.</span><span class="sxs-lookup"><span data-stu-id="6dca1-236">For example, that you can connect to the cluster at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="6dca1-237">A cím használja a nyilvános átjáró, amely nem érhető el, ha már használta az NSG-k vagy udr-EK hozzáférés korlátozása az internetről.</span><span class="sxs-lookup"><span data-stu-id="6dca1-237">This address uses the public gateway, which is not available if you have used NSGs or UDRs to restrict access from the internet.</span></span>

<span data-ttu-id="6dca1-238">Közvetlenül csatlakozhat a HDInsight a virtuális hálózaton keresztül, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6dca1-238">To directly connect to HDInsight through the virtual network, use the following steps:</span></span>

1. <span data-ttu-id="6dca1-239">Annak megállapításához, a belső teljes tartománynevek a HDInsight-fürtcsomóponton, használja a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="6dca1-239">To discover the internal fully qualified domain names of the HDInsight cluster nodes, use one of the following methods:</span></span>

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

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

2. <span data-ttu-id="6dca1-240">Annak megállapításához, a portot, amelyet a szolgáltatás érhető el, tekintse meg a [a HDInsight Hadoop-szolgáltatás által használt portok](./hdinsight-hadoop-port-settings-for-services.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="6dca1-240">To determine the port that a service is available on, see the [Ports used by Hadoop services on HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6dca1-241">Az átjárócsomópontokkal tárolt egyes szolgáltatások aktívak csak az egyik csomópont egyszerre.</span><span class="sxs-lookup"><span data-stu-id="6dca1-241">Some services hosted on the head nodes are only active on one node at a time.</span></span> <span data-ttu-id="6dca1-242">Ha megpróbál egy központi csomóponton szolgáltatások elérésére, és sikertelen lesz, a más átjárócsomópont vált.</span><span class="sxs-lookup"><span data-stu-id="6dca1-242">If you try accessing a service on one head node and it fails, switch to the other head node.</span></span>
    >
    > <span data-ttu-id="6dca1-243">Például Ambari csak egy központi csomóponton aktív egyszerre lehet.</span><span class="sxs-lookup"><span data-stu-id="6dca1-243">For example, Ambari is only active on one head node at a time.</span></span> <span data-ttu-id="6dca1-244">Ha egy központi csomóponton Ambari elérése 404 hibaüzenetet ad vissza, majd fut. a más központi csomóponton.</span><span class="sxs-lookup"><span data-stu-id="6dca1-244">If you try accessing Ambari on one head node and it returns a 404 error, then it is running on the other head node.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6dca1-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6dca1-245">Next steps</span></span>

* <span data-ttu-id="6dca1-246">A HDInsight eszközzel virtuális hálózatban további információkért lásd: [kiterjesztése HDInsight Azure virtuális hálózatok használatával](./hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="6dca1-246">For more information on using HDInsight in a virtual network, see [Extend HDInsight by using Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span></span>

* <span data-ttu-id="6dca1-247">Azure virtuális hálózataihoz további információkért tekintse meg a [Azure Virtual Network áttekintése](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6dca1-247">For more information on Azure virtual networks, see the [Azure Virtual Network overview](../virtual-network/virtual-networks-overview.md).</span></span>

* <span data-ttu-id="6dca1-248">Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="6dca1-248">For more information on network security groups, see [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="6dca1-249">A felhasználó által definiált útvonalak további információkért lásd: [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6dca1-249">For more information on user-defined routes, see [USer-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>
