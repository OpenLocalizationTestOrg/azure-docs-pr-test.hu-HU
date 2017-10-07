---
title: "a 3-csomópont Deis aaaDeploy fürt |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate 3-csomópont Deis fürt Azure-ban Azure Resource Manager-sablonok"
services: virtual-machines-linux
documentationcenter: 
author: HaishiBai
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5eb67eb7-95d4-461d-8eac-44925224ba5f
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/24/2015
ms.author: hbai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a4c0fb8cbb849264e64b433540157c9afecd184e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-configure-a-3-node-deis-cluster-in-azure"></a><span data-ttu-id="8e02c-103">Telepítse és konfigurálja a 3-csomópont Deis fürt az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="8e02c-103">Deploy and configure a 3-node Deis cluster in Azure</span></span>
<span data-ttu-id="8e02c-104">Ez a cikk végigvezeti a kiépítés egy [Deis](http://deis.io/) fürt az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="8e02c-104">This article walks you through provisioning a [Deis](http://deis.io/) cluster on Azure.</span></span> <span data-ttu-id="8e02c-105">Magában foglalja a hello szükséges tanúsítványok toodeploying létrehozásán és skálázásán minta lépéseket hello **nyissa meg** alkalmazás hello újonnan kiosztott fürtön.</span><span class="sxs-lookup"><span data-stu-id="8e02c-105">It covers all hello steps from creating hello necessary certificates toodeploying and scaling a sample **Go** application on hello newly provisioned cluster.</span></span>

<span data-ttu-id="8e02c-106">hello alábbi ábrán látható hello telepített hello rendszer architektúrájával.</span><span class="sxs-lookup"><span data-stu-id="8e02c-106">hello following diagram shows hello architecture of hello deployed system.</span></span> <span data-ttu-id="8e02c-107">A rendszergazda kezeli a hello fürt használt eszközök, mint Deis **deis** és **deisctl**.</span><span class="sxs-lookup"><span data-stu-id="8e02c-107">A system administrator manages hello cluster using Deis tools such as **deis** and **deisctl**.</span></span> <span data-ttu-id="8e02c-108">Egy Azure terheléselosztó, amely továbbítja az hello kapcsolatok tooone hello tag hello fürtön található csomópontok keresztül létesít kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="8e02c-108">Connections are established through an Azure load balancer, which forwards hello connections tooone of hello member nodes on hello cluster.</span></span> <span data-ttu-id="8e02c-109">hello ügyfelek hozzáférési keresztül, valamint a hello terheléselosztó alkalmazások telepítését.</span><span class="sxs-lookup"><span data-stu-id="8e02c-109">hello clients access deployed applications through hello load balancer as well.</span></span> <span data-ttu-id="8e02c-110">Ebben az esetben hello terheléselosztó továbbítja hello forgalom tooa Deis útválasztó háló, amely további routs forgalom toocorresponding Docker tárolók hello fürtön található.</span><span class="sxs-lookup"><span data-stu-id="8e02c-110">In this case, hello load balancer forwards hello traffic tooa Deis router mesh, which further routs traffic toocorresponding Docker containers hosted on hello cluster.</span></span>

  ![Telepített Desis fürt architektúra ábrája](./media/deis-cluster/architecture-overview.png)

<span data-ttu-id="8e02c-112">A sorrend toorun lépések hello keresztül szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="8e02c-112">In order toorun through hello following steps, you'll need:</span></span>

* <span data-ttu-id="8e02c-113">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8e02c-113">An active Azure subscription.</span></span> <span data-ttu-id="8e02c-114">Ha még nincs fiókja, a szabad eljáráshoz beszerezheti [Azure.com webhelyre](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="8e02c-114">If you don't have one, you can get a free trail on [azure.com](https://azure.microsoft.com/).</span></span>
* <span data-ttu-id="8e02c-115">Munkahelyi vagy iskolai azonosító toouse Azure erőforráscsoport-sablonok.</span><span class="sxs-lookup"><span data-stu-id="8e02c-115">A work or school id toouse Azure resource groups.</span></span> <span data-ttu-id="8e02c-116">Ha egy személyes fiók és jelentkezzen be Microsoft-azonosítóval rendelkezik, akkor túl[hozzon létre egy azonosító különbözik a személyes](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8e02c-116">If you have a personal account and log in with a Microsoft id, you need too[create a work id from your personal one](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="8e02c-117">Vagy – attól függően, hogy az ügyfél operációs rendszer – hello [Azure PowerShell](/powershell/azureps-cmdlets-docs) vagy hello [Azure parancssori felület Mac, Linux és Windows](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="8e02c-117">Either -- depending on your client operating system -- hello [Azure PowerShell](/powershell/azureps-cmdlets-docs) or hello [Azure CLI for Mac, Linux, and Windows](../../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="8e02c-118">[OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="8e02c-118">[OpenSSL](https://www.openssl.org/).</span></span> <span data-ttu-id="8e02c-119">OpenSSL használt toogenerate hello szükséges tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="8e02c-119">OpenSSL is used toogenerate hello necessary certificates.</span></span>
* <span data-ttu-id="8e02c-120">Például egy Git ügyfél [Git bash eszközt](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="8e02c-120">A Git client such as [Git Bash](https://git-scm.com/).</span></span>
* <span data-ttu-id="8e02c-121">tootest hello mintaalkalmazást, biztosítani kell a DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="8e02c-121">tootest hello sample application, you'll also need a DNS server.</span></span> <span data-ttu-id="8e02c-122">A DNS-kiszolgálók vagy helyettesítő A rekordok támogató szolgáltatások is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8e02c-122">You can use any DNS servers or services that support wildcard A records.</span></span>
* <span data-ttu-id="8e02c-123">Egy számítógép toorun Deis ügyféleszközök elől.</span><span class="sxs-lookup"><span data-stu-id="8e02c-123">A computer toorun Deis client tools.</span></span> <span data-ttu-id="8e02c-124">A helyi számítógép vagy virtuális gép is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8e02c-124">You can use either a local machine or a virtual machine.</span></span> <span data-ttu-id="8e02c-125">Ezekkel az eszközökkel futtathatja szinte bármilyen a Linux-disztribúció, de hello következő utasítások használják Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="8e02c-125">You can run these tools on almost any Linux distribution, but hello following instructions use Ubuntu.</span></span>

## <a name="provision-hello-cluster"></a><span data-ttu-id="8e02c-126">Kiépítés hello fürt</span><span class="sxs-lookup"><span data-stu-id="8e02c-126">Provision hello cluster</span></span>
<span data-ttu-id="8e02c-127">Ebben a részben azt ismertetjük egy [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) sablon adattárból hello nyílt forráskódú [azure-gyors üzembe helyezés-sablonok](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="8e02c-127">In this section, you'll use an [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) template from hello open source repository [azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="8e02c-128">Először a hello sablon lesz lemásolásához.</span><span class="sxs-lookup"><span data-stu-id="8e02c-128">First, you'll copy down hello template.</span></span> <span data-ttu-id="8e02c-129">Ezután létre fog hozni egy új SSH-kulcspárral hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="8e02c-129">Then, you'll create a new SSH key pair for authentication.</span></span> <span data-ttu-id="8e02c-130">És ezt követően konfigurálja egy új, fürt azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8e02c-130">And then, you'll configure a new identifier for you cluster.</span></span> <span data-ttu-id="8e02c-131">És végezetül fogja használni a hello héjparancsfájlt vagy hello PowerShell parancsfájl tooprovision hello fürt.</span><span class="sxs-lookup"><span data-stu-id="8e02c-131">And finally, you'll use either hello Shell script or hello PowerShell script tooprovision hello cluster.</span></span>

1. <span data-ttu-id="8e02c-132">Klónozás hello tárház: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="8e02c-132">Clone hello repository: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span>
   
        git clone https://github.com/Azure/azure-quickstart-templates
2. <span data-ttu-id="8e02c-133">Nyissa meg toohello sablon mappája:</span><span class="sxs-lookup"><span data-stu-id="8e02c-133">Go toohello template folder:</span></span>
   
        cd azure-quickstart-templates\deis-cluster-coreos
3. <span data-ttu-id="8e02c-134">Hozzon létre egy új SSH-kulcspárral ssh-keygen használata:</span><span class="sxs-lookup"><span data-stu-id="8e02c-134">Create a new SSH key pair using ssh-keygen:</span></span>
   
        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"
4. <span data-ttu-id="8e02c-135">Létrehoz egy tanúsítványt, titkos kulcs fent hello használata:</span><span class="sxs-lookup"><span data-stu-id="8e02c-135">Generate a certificate using hello above private key:</span></span>
   
        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file toobe generated]
5. <span data-ttu-id="8e02c-136">Nyissa meg túl[https://discovery.etcd.io/new](https://discovery.etcd.io/new) toogenerate új fürt tokent, az alábbihoz hasonló következőhöz:</span><span class="sxs-lookup"><span data-stu-id="8e02c-136">Go too[https://discovery.etcd.io/new](https://discovery.etcd.io/new) toogenerate a new cluster token, which looks something like:</span></span>
   
        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
   <br />
   <span data-ttu-id="8e02c-137">Minden egyes CoreOS fürt toohave egyedi jogkivonatot adott szabad szolgáltatásból kell.</span><span class="sxs-lookup"><span data-stu-id="8e02c-137">Each CoreOS cluster needs toohave a unique token from this free service.</span></span> <span data-ttu-id="8e02c-138">Ellenőrizze a [CoreOS dokumentáció](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="8e02c-138">Please see [CoreOS documentation](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) for more details.</span></span>
6. <span data-ttu-id="8e02c-139">Hello módosítása **felhő-config.yaml** tooreplace hello meglévő fájl **felderítési** token az új jogkivonatot hello:</span><span class="sxs-lookup"><span data-stu-id="8e02c-139">Modify hello **cloud-config.yaml** file tooreplace hello existing  **discovery** token with hello new token:</span></span>
   
        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment hello following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...
7. <span data-ttu-id="8e02c-140">Módosítsa a hello **azuredeploy-parameters.json** fájl: létrehozott a 4. lépésben egy szövegszerkesztőben megnyitott hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="8e02c-140">Modify hello **azuredeploy-parameters.json** file: Open hello certificate you created in step 4 in a text editor.</span></span> <span data-ttu-id="8e02c-141">Másolja az összes szöveg közötti `----BEGIN CERTIFICATE-----` és `-----END CERTIFICATE-----` történő hello **sshKeyData** paraméter (lesz szüksége tooremove összes soremelés karakter).</span><span class="sxs-lookup"><span data-stu-id="8e02c-141">Copy all text between `----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` into hello **sshKeyData** parameter (you'll need tooremove all newline characters).</span></span>
8. <span data-ttu-id="8e02c-142">Módosítsa a hello **newStorageAccountName** paraméter.</span><span class="sxs-lookup"><span data-stu-id="8e02c-142">Modify hello **newStorageAccountName** parameter.</span></span> <span data-ttu-id="8e02c-143">Ez az hello tárfiók a virtuális gép operációs rendszere lemezek.</span><span class="sxs-lookup"><span data-stu-id="8e02c-143">This is hello storage account for VM OS disks.</span></span> <span data-ttu-id="8e02c-144">A fióknév globálisan egyedi toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8e02c-144">This account name has toobe globally unique.</span></span>
9. <span data-ttu-id="8e02c-145">Módosítsa a hello **publicDomainName** paraméter.</span><span class="sxs-lookup"><span data-stu-id="8e02c-145">Modify hello **publicDomainName** parameter.</span></span> <span data-ttu-id="8e02c-146">Ez lesz hello load balancer nyilvános IP-cím társított hello DNS-név részét.</span><span class="sxs-lookup"><span data-stu-id="8e02c-146">This will become part of hello DNS name associated with hello load balancer public IP.</span></span> <span data-ttu-id="8e02c-147">hello végső FQDN lesz hello formátuma *[Ez a paraméter értékének]*. *[régió]* . cloudapp.azure.com. Például ha deishbai32 hello nevet ad meg, és hello erőforráscsoport üzembe helyezett toohello USA nyugati régiója régiót, majd a végső hello tooyour terheléselosztó FQDN lesz deishbai32.westus.cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="8e02c-147">hello final FQDN will have hello format of *[value of this parameter]*.*[region]*.cloudapp.azure.com. For example, if you specify hello name as deishbai32, and hello resource group is deployed toohello West US region, then hello final FQDN tooyour load balancer will be deishbai32.westus.cloudapp.azure.com.</span></span>
10. <span data-ttu-id="8e02c-148">Hello paraméter fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="8e02c-148">Save hello parameter file.</span></span> <span data-ttu-id="8e02c-149">És az Azure PowerShell hello fürt azt követően:</span><span class="sxs-lookup"><span data-stu-id="8e02c-149">And then you can provision hello cluster using Azure PowerShell:</span></span>
    
        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml
    
    <span data-ttu-id="8e02c-150">vagy az Azure parancssori felület:</span><span class="sxs-lookup"><span data-stu-id="8e02c-150">or Azure CLI:</span></span>
    
        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  
11. <span data-ttu-id="8e02c-151">Hello erőforráscsoport üzembe helyezése után megtekintheti a hello csoportban lévő összes hello erőforrást a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="8e02c-151">Once hello resource group is provisioned, you can see all hello resources in hello group on Azure classic portal.</span></span> <span data-ttu-id="8e02c-152">Ahogy az alábbi képernyőfelvételen látható hello hello erőforráscsoport három virtuális gépek virtuális hálózat, amely tartományhoz csatlakoztatott toohello azonos rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="8e02c-152">As shown in hello following screenshot, hello resource group contains a virtual network with three VMs, which are joined toohello same availability set.</span></span> <span data-ttu-id="8e02c-153">hello csoport is tartalmaz egy terheléselosztó, amely van egy társított nyilvános IP-címe.</span><span class="sxs-lookup"><span data-stu-id="8e02c-153">hello group also contains a load balancer, which has an associated public IP.</span></span>
    
    ![hello kiosztott erőforráscsoport a klasszikus Azure portálon](./media/deis-cluster/resource-group.png)

## <a name="install-hello-client"></a><span data-ttu-id="8e02c-155">Hello ügyfél telepítése</span><span class="sxs-lookup"><span data-stu-id="8e02c-155">Install hello client</span></span>
<span data-ttu-id="8e02c-156">Szüksége **deisctl** toocontrol a fürt Deis.</span><span class="sxs-lookup"><span data-stu-id="8e02c-156">You need **deisctl** toocontrol your Deis cluster.</span></span> <span data-ttu-id="8e02c-157">Bár deisctl automatikusan megtörténik a hello fürt összes csomópontjának, továbbra is egy célszerű toouse deisctl egy külön felügyeleti számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8e02c-157">Although deisctl is automatically installed in all hello cluster nodes, it's a good practice toouse deisctl on a separate administrative machine.</span></span> <span data-ttu-id="8e02c-158">Továbbá mert minden csomópont csak privát IP-címek vannak beállítva, szüksége lesz toouse SSH tunneling hello terheléselosztó, amely van egy nyilvános IP-címe, tooconnect toohello csomópont gépek keresztül.</span><span class="sxs-lookup"><span data-stu-id="8e02c-158">Furthermore, because all nodes are configured with only private IP addresses, you'll need toouse SSH tunneling through hello load balancer, which has a public IP, tooconnect toohello node machines.</span></span> <span data-ttu-id="8e02c-159">hello az alábbiakban hello külön Ubuntu fizikai vagy virtuális gépen deisctl beállításának lépésein.</span><span class="sxs-lookup"><span data-stu-id="8e02c-159">hello following are hello steps of setting up deisctl on a separate Ubuntu physical or virtual machine.</span></span>

1. <span data-ttu-id="8e02c-160">Telepítés deisctl:mkdir deis</span><span class="sxs-lookup"><span data-stu-id="8e02c-160">Install deisctl:mkdir deis</span></span>
   
        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl
2. <span data-ttu-id="8e02c-161">Adja hozzá a titkos kulcs toossh ügynök:</span><span class="sxs-lookup"><span data-stu-id="8e02c-161">Add your private key toossh agent:</span></span>
   
        eval `ssh-agent -s`
        ssh-add [path toohello private key file, see step 1 in hello previous section]
3. <span data-ttu-id="8e02c-162">Deisctl konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="8e02c-162">Configure deisctl:</span></span>
   
        export DEISCTL_TUNNEL=[public ip of hello load balancer]:2223

<span data-ttu-id="8e02c-163">hello sablon meghatározza a bejövő NAT-szabályok, amelyek kapcsolódnak 2223 tooinstance 1, 2224 tooinstance 2 és 2225 tooinstance 3.</span><span class="sxs-lookup"><span data-stu-id="8e02c-163">hello template defines inbound NAT rules that map 2223 tooinstance 1, 2224 tooinstance 2, and 2225 tooinstance 3.</span></span> <span data-ttu-id="8e02c-164">Hello deisctl eszközzel redundanciát biztosít.</span><span class="sxs-lookup"><span data-stu-id="8e02c-164">This provides redundancy for using hello deisctl tool.</span></span> <span data-ttu-id="8e02c-165">Ezek a szabályok a klasszikus Azure portálon ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="8e02c-165">You can examine these rules on Azure classic portal:</span></span>

![A hello terheléselosztó NAT-szabályok](./media/deis-cluster/nat-rules.png)

> [!NOTE]
> <span data-ttu-id="8e02c-167">Hello sablon jelenleg csak 3-csomópontot tartalmazó fürt támogatja.</span><span class="sxs-lookup"><span data-stu-id="8e02c-167">Currently hello template only supports 3-node clusters.</span></span> <span data-ttu-id="8e02c-168">Ez az Azure Resource Manager sablon NAT-szabály definíciót, amely nem támogatja a hurok szintaxis korlátozása miatt.</span><span class="sxs-lookup"><span data-stu-id="8e02c-168">This is because of a limitation in Azure Resource Manager template NAT rule definition, which doesn’t support loop syntax.</span></span>
> 
> 

## <a name="install-and-start-hello-deis-platform"></a><span data-ttu-id="8e02c-169">Telepítse és indítsa el a hello Deis platform</span><span class="sxs-lookup"><span data-stu-id="8e02c-169">Install and start hello Deis platform</span></span>
<span data-ttu-id="8e02c-170">Most deisctl tooinstall használja, és indítsa el a hello Deis platform:</span><span class="sxs-lookup"><span data-stu-id="8e02c-170">Now you can use deisctl tooinstall and start hello Deis platform:</span></span>

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path toohello private key file]
    deisctl install platform
    deisctl start platform

> [!NOTE]
> <span data-ttu-id="8e02c-171">Kezdő hello platform eltart egy ideig (akár 10 percet).</span><span class="sxs-lookup"><span data-stu-id="8e02c-171">Starting hello platform takes a while (as much as 10 minutes).</span></span> <span data-ttu-id="8e02c-172">Különösen hello jelentéskészítő szolgáltatás indítása a hosszú időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="8e02c-172">Especially, starting hello builder service can take a long time.</span></span> <span data-ttu-id="8e02c-173">És egyes esetekben néhány záma toosucceed tart: Ha hello művelet toohang úgy tűnik, próbáljon meg `ctrl+c` toobreak végrehajtása hello parancsot, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="8e02c-173">And sometimes it takes a few tries toosucceed: If hello operation seems toohang, try typing `ctrl+c` toobreak execution of hello command and retry.</span></span>
> 
> 

<span data-ttu-id="8e02c-174">Használhat `deisctl list` tooverify, ha az összes szolgáltatás fut:</span><span class="sxs-lookup"><span data-stu-id="8e02c-174">You can use `deisctl list` tooverify if all services are running:</span></span>

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

<span data-ttu-id="8e02c-175">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="8e02c-175">Congratulations!</span></span> <span data-ttu-id="8e02c-176">Most már futó Deis clsuter Azure!</span><span class="sxs-lookup"><span data-stu-id="8e02c-176">Now you've got a running Deis clsuter on Azure!</span></span> <span data-ttu-id="8e02c-177">A következő helyezzünk üzembe egy minta Ugrás alkalmazás toosee hello fürt működés közben.</span><span class="sxs-lookup"><span data-stu-id="8e02c-177">Next, let's deploy a sample Go application toosee hello cluster in action.</span></span>

## <a name="deploy-and-scale-a-hello-world-application"></a><span data-ttu-id="8e02c-178">Üzembe helyezését és méretezését egy Hello World alkalmazásról</span><span class="sxs-lookup"><span data-stu-id="8e02c-178">Deploy and scale a Hello World application</span></span>
<span data-ttu-id="8e02c-179">hello következő lépések bemutatják, hogyan toodeploy a "Hello World" Ugrás alkalmazás toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="8e02c-179">hello following steps show how toodeploy a "Hello World" Go application toohello cluster.</span></span> <span data-ttu-id="8e02c-180">a lépések hello [dokumentáció Deis](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).</span><span class="sxs-lookup"><span data-stu-id="8e02c-180">hello steps are based on [Deis documentation](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).</span></span>

1. <span data-ttu-id="8e02c-181">A hello útválasztási háló toowork megfelelően, szüksége lesz toohave helyettesítő A rekord a tartomány toohello nyilvános IP-cím hello terheléselosztó mutat.</span><span class="sxs-lookup"><span data-stu-id="8e02c-181">For hello routing mesh toowork properly, you’ll need toohave a wildcard A record for your domain pointing toohello public IP of hello load balancer.</span></span> <span data-ttu-id="8e02c-182">hello alábbi képernyőfelvételen látható egy minta tartományregisztrációs hello A rekord GoDaddy:</span><span class="sxs-lookup"><span data-stu-id="8e02c-182">hello following screenshot shows hello A record for a sample domain registration on GoDaddy:</span></span>
   
    ![Godaddy A rekord](./media/deis-cluster/go-daddy.png)
   
   <p />
2. <span data-ttu-id="8e02c-184">Telepítés deis:</span><span class="sxs-lookup"><span data-stu-id="8e02c-184">Install deis:</span></span>
   
        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
3. <span data-ttu-id="8e02c-185">Hozzon létre egy új SSH-kulcsot, és adja hozzá a nyilvános kulcs tooGitHub hello (természetesen is felhasználhatja a meglévő kulcsok).</span><span class="sxs-lookup"><span data-stu-id="8e02c-185">Create a new SSH key, and then add hello public key tooGitHub (of course, you can also reuse your existing keys).</span></span> <span data-ttu-id="8e02c-186">új SSH-kulcspárral, toocreate használja:</span><span class="sxs-lookup"><span data-stu-id="8e02c-186">toocreate a new SSH key pair, use:</span></span>
   
        cd ~/.ssh
        ssh-keygen (press [Enter]s toouse default file names and empty passcode)
4. <span data-ttu-id="8e02c-187">Adja hozzá a id_rsa.pub, vagy az Ön által választott, tooGitHub hello a nyilvános kulcsot.</span><span class="sxs-lookup"><span data-stu-id="8e02c-187">Add id_rsa.pub, or hello public key of your choice, tooGitHub.</span></span> <span data-ttu-id="8e02c-188">Ehhez a hello hozzáadása SSH kulcs gombra kattintva az SSH-kulcsok konfigurációs képernyőjén:</span><span class="sxs-lookup"><span data-stu-id="8e02c-188">You can do this by using hello Add SSH key button in your SSH keys configuration screen:</span></span>
   
   ![GitHub-kulcs](./media/deis-cluster/github-key.png)
   
   <p />
5. <span data-ttu-id="8e02c-190">Új felhasználó regisztrálása:</span><span class="sxs-lookup"><span data-stu-id="8e02c-190">Register a new user:</span></span>
   
        deis register http://deis.[your domain]
   <p />
6. <span data-ttu-id="8e02c-191">Hello SSH-kulcs hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="8e02c-191">Add hello SSH key:</span></span>
   
        deis keys:add [path tooyour SSH public key]
   <p />      
7. <span data-ttu-id="8e02c-192">Hozzon létre egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8e02c-192">Create an application.</span></span>
   
        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
   <p /><span data-ttu-id="8e02c-193">
8.hello git push Docker képek toobe létrehozott és telepített, ami eltarthat néhány percig, akkor indul el.</span><span class="sxs-lookup"><span data-stu-id="8e02c-193">
8. hello git push will trigger Docker images toobe built and deployed, which will take a few minutes.</span></span> <span data-ttu-id="8e02c-194">A felhasználói élmény, a alkalmanként lépés 10 (Pushing tooprivate lemezképtárba) lefagyhat.</span><span class="sxs-lookup"><span data-stu-id="8e02c-194">From my experience, occasionally, Step 10 (Pushing image tooprivate repository) may hang.</span></span> <span data-ttu-id="8e02c-195">Ha ez történik, le is hello folyamat eltávolítása hello használó "deis alkalmazások: semmisítse meg – a <application name> ` tooremove hello application and try again. You can use `deis apps:list" toofind hello nevét az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8e02c-195">When this happens, you can stop hello process, remove hello application using `deis apps:destroy –a <application name>` tooremove hello application and try again. You can use `deis apps:list` toofind out hello name of your application.</span></span> <span data-ttu-id="8e02c-196">Ha minden megfelelően működik, akkor kell kinéznie: hello hasonló parancs végrehajtásának hello végén</span><span class="sxs-lookup"><span data-stu-id="8e02c-196">If everything works out, you should see something like hello following at hello end of command outputs:</span></span>
   
        -----> Launching...
               done, lambda-underdog:v2 deployed tooDeis
               http://lambda-underdog.artitrack.com
               toolearn more, use `deis help` or visit http://deis.io
        toossh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
   <p />
9. <span data-ttu-id="8e02c-197">Győződjön meg arról, hogy működik-e az alkalmazás hello:</span><span class="sxs-lookup"><span data-stu-id="8e02c-197">Verify if hello application is working:</span></span>
   
        curl -S http://[your application name].[your domain]
   <span data-ttu-id="8e02c-198">A következőnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="8e02c-198">You should see:</span></span>
   
        Welcome tooDeis!
        See hello documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list tooget hello name of your application).
   <p />
10. <span data-ttu-id="8e02c-199">Hello too3 alkalmazáspéldányok skálázni:</span><span class="sxs-lookup"><span data-stu-id="8e02c-199">Scale hello application too3 instances:</span></span>
    
        deis scale cmd=3
    <p />
11. <span data-ttu-id="8e02c-200">Másik lehetőségként használhatja az alkalmazás adatok tooexamine részletek deis.</span><span class="sxs-lookup"><span data-stu-id="8e02c-200">Optionally, you can use deis info tooexamine details of your application.</span></span> <span data-ttu-id="8e02c-201">hello következő kimenetek tartoznak, amely az alkalmazás központi telepítése:</span><span class="sxs-lookup"><span data-stu-id="8e02c-201">hello following outputs are from my application deployment:</span></span>
    
        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }
    
        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)
    
        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a><span data-ttu-id="8e02c-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8e02c-202">Next Steps</span></span>
<span data-ttu-id="8e02c-203">Ez a cikk telefonon keresztül minden hello lépéseket tooprovision egy új Deis fürt Azure-ban Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="8e02c-203">This article walked you through all hello steps tooprovision a new Deis cluster on Azure using an Azure Resource Manager template.</span></span> <span data-ttu-id="8e02c-204">hello sablon tooling eszköz kapcsolatok, valamint a telepített alkalmazások terheléselosztását redundanciát támogatja.</span><span class="sxs-lookup"><span data-stu-id="8e02c-204">hello template supports redundancy in tooling connections as well as load balancing for deployed applications.</span></span> <span data-ttu-id="8e02c-205">hello sablon megakadályozza, hogy tag csomópontján, amely értékes nyilvános IP-erőforrások menti, és lehetővé teszi a biztonságosabb környezet toohost nyilvános IP-címek használatával.</span><span class="sxs-lookup"><span data-stu-id="8e02c-205">hello template also avoids using public IPs on member nodes, which saves precious public IP resources and provides a more secured environment toohost applications.</span></span> <span data-ttu-id="8e02c-206">toolearn több, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="8e02c-206">toolearn more, see hello following articles:</span></span>

<span data-ttu-id="8e02c-207">[Azure Resource Manager áttekintése][resource-group-overview]</span><span class="sxs-lookup"><span data-stu-id="8e02c-207">[Azure Resource Manager Overview][resource-group-overview]</span></span>  
<span data-ttu-id="8e02c-208">[Hogyan toouse hello Azure parancssori felület][azure-command-line-tools]</span><span class="sxs-lookup"><span data-stu-id="8e02c-208">[How toouse hello Azure CLI][azure-command-line-tools]</span></span>  
<span data-ttu-id="8e02c-209">[Using Azure PowerShell használata az Azure Resource Manager eszközzel][powershell-azure-resource-manager]</span><span class="sxs-lookup"><span data-stu-id="8e02c-209">[Using Azure PowerShell with Azure Resource Manager][powershell-azure-resource-manager]</span></span>  

[azure-command-line-tools]: ../../cli-install-nodejs.md
[resource-group-overview]: ../../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
