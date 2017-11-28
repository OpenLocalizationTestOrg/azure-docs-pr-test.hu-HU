---
title: "egy Azure virtuális hálózat (klasszikus), több alhálózattal aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy virtuális hálózat (klasszikus), több alhálózattal az Azure-ban."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: cc7b9ad08d5c26dba09584762bedf614d2847032
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a><span data-ttu-id="be23a-103">Hozzon létre egy virtuális hálózat (klasszikus), több alhálózattal</span><span class="sxs-lookup"><span data-stu-id="be23a-103">Create a virtual network (classic) with multiple subnets</span></span>

> [!IMPORTANT]
> <span data-ttu-id="be23a-104">Két Azure tartozik [különböző üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) az erőforrások létrehozására és kezelésére vonatkozó: Resource Manager és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="be23a-104">Azure has two [different deployment models](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) for creating and working with resources: Resource Manager and classic.</span></span> <span data-ttu-id="be23a-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="be23a-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="be23a-106">A Microsoft azt javasolja, keresztül hello legtöbb új virtuális hálózat létrehozása [erőforrás-kezelő](virtual-networks-create-vnet-arm-pportal.md) üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="be23a-106">Microsoft recommends creating most new virtual networks through hello [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) deployment model.</span></span>

<span data-ttu-id="be23a-107">Ebből az oktatóanyagból megtudhatja, hogyan toocreate egy alapszintű Azure virtuális hálózat (klasszikus), amely rendelkezik külön nyilvános és titkos alhálózat.</span><span class="sxs-lookup"><span data-stu-id="be23a-107">In this tutorial, learn how toocreate a basic Azure virtual network (classic) that has separate public and private subnets.</span></span> <span data-ttu-id="be23a-108">Azure-erőforrások, például a virtuális gépek és felhőszolgáltatások az alhálózat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="be23a-108">You can create Azure resources, like Virtual machines and Cloud services in a subnet.</span></span> <span data-ttu-id="be23a-109">Létrehozott virtuális hálózatok (klasszikus) erőforrásokhoz kommunikálhatnak egymással, illetve más hálózatokhoz csatlakozó tooa virtuális hálózatán lévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="be23a-109">Resources created in virtual networks (classic) can communicate with each other, and with resources in other networks connected tooa virtual network.</span></span>

<span data-ttu-id="be23a-110">További információk [virtuális hálózati](virtual-network-manage-network.md) és [alhálózati](virtual-network-manage-subnet.md) beállításait.</span><span class="sxs-lookup"><span data-stu-id="be23a-110">Learn more about all [virtual network](virtual-network-manage-network.md) and [subnet](virtual-network-manage-subnet.md) settings.</span></span>

> [!WARNING]
> <span data-ttu-id="be23a-111">Virtuális hálózatok (klasszikus) Azure azonnal törli amikor egy [előfizetés le van tiltva](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit).</span><span class="sxs-lookup"><span data-stu-id="be23a-111">Virtual networks (classic) are immediately deleted by Azure when a [subscription is disabled](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit).</span></span> <span data-ttu-id="be23a-112">Virtuális hálózatok (klasszikus) törlődnek, függetlenül attól, hogy erőforrások hello virtuális hálózat létezik.</span><span class="sxs-lookup"><span data-stu-id="be23a-112">Virtual networks (classic) are deleted regardless of whether resources exist in hello virtual network.</span></span> <span data-ttu-id="be23a-113">Ha később újra engedélyezi hello előfizetés, szereplő hello virtuális hálózati erőforrások kell újból létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="be23a-113">If you later re-enable hello subscription, resources that existed in hello virtual network must be recreated.</span></span>

<span data-ttu-id="be23a-114">Hello segítségével létrehozhat egy virtuális hálózat (klasszikus) [Azure-portálon](#portal), hello [Azure parancssori felület (CLI) 1.0](#azure-cli), vagy [PowerShell](#powershell).</span><span class="sxs-lookup"><span data-stu-id="be23a-114">You can create a virtual network (classic) by using hello [Azure portal](#portal), hello [Azure command-line interface (CLI) 1.0](#azure-cli), or [PowerShell](#powershell).</span></span>

## <a name="portal"></a><span data-ttu-id="be23a-115">Portál</span><span class="sxs-lookup"><span data-stu-id="be23a-115">Portal</span></span>

1. <span data-ttu-id="be23a-116">Egy webböngészőben nyissa meg toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="be23a-116">In an Internet browser, go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="be23a-117">Jelentkezzen be a [Azure-fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="be23a-117">Log in using your [Azure account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="be23a-118">Ha az Azure-fiók nem rendelkezik, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="be23a-118">If you don't have an Azure account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="be23a-119">Kattintson a **+ új** hello portálon.</span><span class="sxs-lookup"><span data-stu-id="be23a-119">Click **+New** in hello portal.</span></span>
3. <span data-ttu-id="be23a-120">Adja meg *virtuális hálózati* a hello **keresési hello piactér** mezőt hello hello tetején **új** panel, amely akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="be23a-120">Enter *Virtual network* in hello **Search hello Marketplace** box at hello top of hello **New** blade that appears.</span></span>  <span data-ttu-id="be23a-121">Kattintson a **virtuális hálózati** amikor megjelenik a hello keresési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="be23a-121">Click **Virtual network** when it appears in hello search results.</span></span>
4. <span data-ttu-id="be23a-122">Válassza ki **klasszikus** a hello **telepítési modell kiválasztása** hello párbeszédpanel **virtuális hálózati** panel, amely akkor jelenik meg, majd kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="be23a-122">Select **Classic** in hello **Select a deployment model** box in hello **Virtual Network** blade that appears, then click **Create**.</span></span> 
5. <span data-ttu-id="be23a-123">Adja meg a következő értékeket a hello hello **virtuális hálózat létrehozása (klasszikus)** panel megnyitásához, és kattintson **létrehozása**:</span><span class="sxs-lookup"><span data-stu-id="be23a-123">Enter hello following values on hello **Create virtual network (classic)** blade and then click **Create**:</span></span>

    |<span data-ttu-id="be23a-124">Beállítás</span><span class="sxs-lookup"><span data-stu-id="be23a-124">Setting</span></span>|<span data-ttu-id="be23a-125">Érték</span><span class="sxs-lookup"><span data-stu-id="be23a-125">Value</span></span>|
    |---|---|
    |<span data-ttu-id="be23a-126">Név</span><span class="sxs-lookup"><span data-stu-id="be23a-126">Name</span></span>|<span data-ttu-id="be23a-127">myVNet</span><span class="sxs-lookup"><span data-stu-id="be23a-127">myVnet</span></span>|
    |<span data-ttu-id="be23a-128">Címtér</span><span class="sxs-lookup"><span data-stu-id="be23a-128">Address space</span></span>|<span data-ttu-id="be23a-129">10.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="be23a-129">10.0.0.0/16</span></span>|
    |<span data-ttu-id="be23a-130">Alhálózat neve</span><span class="sxs-lookup"><span data-stu-id="be23a-130">Subnet name</span></span>|<span data-ttu-id="be23a-131">Nyilvános</span><span class="sxs-lookup"><span data-stu-id="be23a-131">Public</span></span>|
    |<span data-ttu-id="be23a-132">Alhálózati címtartomány</span><span class="sxs-lookup"><span data-stu-id="be23a-132">Subnet address range</span></span>|<span data-ttu-id="be23a-133">10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="be23a-133">10.0.0.0/24</span></span>|
    |<span data-ttu-id="be23a-134">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="be23a-134">Resource group</span></span>|<span data-ttu-id="be23a-135">Hagyja **hozzon létre új** kiválasztva, és írja be **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="be23a-135">Leave **Create new** selected, and then enter **myResourceGroup**.</span></span>|
    |<span data-ttu-id="be23a-136">Egyes előfizetésekhez és helyekhez</span><span class="sxs-lookup"><span data-stu-id="be23a-136">Subscription and location</span></span>|<span data-ttu-id="be23a-137">Válassza ki az egyes előfizetésekhez és helyekhez.</span><span class="sxs-lookup"><span data-stu-id="be23a-137">Select your subscription and location.</span></span>

    <span data-ttu-id="be23a-138">Ha új tooAzure, további információ [erőforráscsoportok](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [előfizetések](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), és [helyek](https://azure.microsoft.com/regions) (más néven tooas *régiók*).</span><span class="sxs-lookup"><span data-stu-id="be23a-138">If you're new tooAzure, learn more about [resource groups](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [subscriptions](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), and [locations](https://azure.microsoft.com/regions) (also referred tooas *regions*).</span></span>
4. <span data-ttu-id="be23a-139">Hello portálon csak egy alhálózat virtuális hálózat létrehozásakor is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="be23a-139">In hello portal, you can create only one subnet when you create a virtual network.</span></span> <span data-ttu-id="be23a-140">Ebben az oktatóanyagban létrehoz egy második alhálózat hello virtuális hálózat létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="be23a-140">In this tutorial, you create a second subnet after you create hello virtual network.</span></span> <span data-ttu-id="be23a-141">Később létrehozhat az internetről elérhető erőforrások a hello **nyilvános** alhálózat.</span><span class="sxs-lookup"><span data-stu-id="be23a-141">You might later create Internet-accessible resources in hello **Public** subnet.</span></span> <span data-ttu-id="be23a-142">Is létrehozhat, amelyek nem érhetők el az Internet hello hello az erőforrások **titkos** alhálózat.</span><span class="sxs-lookup"><span data-stu-id="be23a-142">You also might create resources that aren't accessible from hello Internet in hello **Private** subnet.</span></span> <span data-ttu-id="be23a-143">toocreate hello második alhálózat, adja meg **myVnet** a hello **keresési erőforrások** mezőt hello lap hello tetején.</span><span class="sxs-lookup"><span data-stu-id="be23a-143">toocreate hello second subnet, enter **myVnet** in hello **Search resources** box at hello top of hello page.</span></span> <span data-ttu-id="be23a-144">Kattintson a **myVnet** amikor megjelenik a hello keresési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="be23a-144">Click **myVnet** when it appears in hello search results.</span></span>
5. <span data-ttu-id="be23a-145">Kattintson a **alhálózatok** (a hello **beállítások** szakasz) a hello **virtuális hálózat létrehozása (klasszikus)** megjelenő panelen.</span><span class="sxs-lookup"><span data-stu-id="be23a-145">Click **Subnets** (in hello **SETTINGS** section) on hello **Create virtual network (classic)** blade that appears.</span></span>
6. <span data-ttu-id="be23a-146">Kattintson a **+ Hozzáadás** a hello **myVnet - alhálózatok** panel, amely akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="be23a-146">Click **+Add** on hello **myVnet - Subnets** blade that appears.</span></span>
7. <span data-ttu-id="be23a-147">Adja meg **titkos** a **neve** a hello **alhálózat hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="be23a-147">Enter **Private** for **Name** on hello **Add subnet** blade.</span></span> <span data-ttu-id="be23a-148">Adja meg **10.0.1.0/24** a **-címtartományt**.</span><span class="sxs-lookup"><span data-stu-id="be23a-148">Enter **10.0.1.0/24** for **Address range**.</span></span>  <span data-ttu-id="be23a-149">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="be23a-149">Click **OK**.</span></span>
8. <span data-ttu-id="be23a-150">A hello **myVnet - alhálózatok** panelen láthatja hello **nyilvános** és **titkos** létrehozott alhálózatok.</span><span class="sxs-lookup"><span data-stu-id="be23a-150">On hello **myVnet - Subnets** blade, you can see hello **Public** and **Private** subnets that you created.</span></span>
9. <span data-ttu-id="be23a-151">**Nem kötelező**: Ez az oktatóanyag befejezése után érdemes lehet toodelete hello erőforrások létrehozott, így a használati költségek:</span><span class="sxs-lookup"><span data-stu-id="be23a-151">**Optional**: When you finish this tutorial, you might want toodelete hello resources that you created, so that you don't incur usage charges:</span></span>
    - <span data-ttu-id="be23a-152">Kattintson a **áttekintése** a hello **myVnet** panelen.</span><span class="sxs-lookup"><span data-stu-id="be23a-152">Click **Overview** on hello **myVnet** blade.</span></span>
    - <span data-ttu-id="be23a-153">Kattintson a hello **törlése** hello ikonra **myVnet** panelen.</span><span class="sxs-lookup"><span data-stu-id="be23a-153">Click hello **Delete** icon on hello **myVnet** blade.</span></span>
    - <span data-ttu-id="be23a-154">tooconfirm hello törlését, kattintson a **Igen** a hello **virtuális hálózati Delete** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="be23a-154">tooconfirm hello deletion, click **Yes** in hello **Delete virtual network** box.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="be23a-155">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="be23a-155">Azure CLI</span></span>

1. <span data-ttu-id="be23a-156">Megadhat [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), vagy a CLI hello hello Azure Cloud rendszerhéj belül használja.</span><span class="sxs-lookup"><span data-stu-id="be23a-156">You can either [install and configure hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), or use hello CLI within hello Azure Cloud Shell.</span></span> <span data-ttu-id="be23a-157">hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül.</span><span class="sxs-lookup"><span data-stu-id="be23a-157">hello Azure Cloud Shell is a free Bash shell that you can run directly within hello Azure portal.</span></span> <span data-ttu-id="be23a-158">Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van.</span><span class="sxs-lookup"><span data-stu-id="be23a-158">It has hello Azure CLI preinstalled and configured toouse with your account.</span></span> <span data-ttu-id="be23a-159">Írja be a parancssori felület parancsait tooget súgóját `azure <command> --help`.</span><span class="sxs-lookup"><span data-stu-id="be23a-159">tooget help for CLI commands, type `azure <command> --help`.</span></span> 
2. <span data-ttu-id="be23a-160">CLI munkamenetben a következő paranccsal hello tooAzure jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="be23a-160">In a CLI session, log in tooAzure with hello command that follows.</span></span> <span data-ttu-id="be23a-161">Ha **kipróbálás** alábbi hello mezőbe, a felhő rendszerhéj megnyitása.</span><span class="sxs-lookup"><span data-stu-id="be23a-161">If you click **Try it** in hello box below, a Cloud Shell opens.</span></span> <span data-ttu-id="be23a-162">Azure-előfizetésre, tooyour bejelentkezhet a következő parancs hello megadása nélkül:</span><span class="sxs-lookup"><span data-stu-id="be23a-162">You can log in tooyour Azure subscription, without entering hello following command:</span></span>

    ```azurecli-interactive
    azure login
    ```

3. <span data-ttu-id="be23a-163">tooensure hello CLI szolgáltatásfelügyelet módban van, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="be23a-163">tooensure hello CLI is in Service Management mode, enter hello following command:</span></span>

    ```azurecli-interactive
    azure config mode asm
    ```

4. <span data-ttu-id="be23a-164">Virtuális hálózat létrehozása a saját alhálózattal:</span><span class="sxs-lookup"><span data-stu-id="be23a-164">Create a virtual network with a private subnet:</span></span>

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. <span data-ttu-id="be23a-165">Hozzon létre egy nyilvános alhálózatot hello virtuális hálózaton belül:</span><span class="sxs-lookup"><span data-stu-id="be23a-165">Create a public subnet within hello virtual network:</span></span>

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. <span data-ttu-id="be23a-166">Tekintse át a virtuális hálózati hello és -alhálózatok:</span><span class="sxs-lookup"><span data-stu-id="be23a-166">Review hello virtual network and subnets:</span></span>

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. <span data-ttu-id="be23a-167">**Nem kötelező**: érdemes létrehozott Ez az oktatóanyag befejezésekor, hogy a használati költségek toodelete hello erőforrások:</span><span class="sxs-lookup"><span data-stu-id="be23a-167">**Optional**: You might want toodelete hello resources that you created when you finish this tutorial, so that you don't incur usage charges:</span></span>

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> <span data-ttu-id="be23a-168">Abban az esetben, ha nem adja meg egy erőforrás csoport toocreate egy (klasszikus) virtuális hálózati hello parancssori felület használatával, ha Azure hello virtuális hálózatot hoz létre a nevű erőforráscsoportban *alapértelmezett-hálózat*.</span><span class="sxs-lookup"><span data-stu-id="be23a-168">Though you can't specify a resource group toocreate a virtual network (classic) in using hello CLI, Azure creates hello virtual network in a resource group named *Default-Networking*.</span></span>

## <a name="powershell"></a><span data-ttu-id="be23a-169">PowerShell</span><span class="sxs-lookup"><span data-stu-id="be23a-169">PowerShell</span></span>

1. <span data-ttu-id="be23a-170">Hello hello PowerShell legújabb verziójának telepítéséhez [Azure](https://www.powershellgallery.com/packages/Azure) modul.</span><span class="sxs-lookup"><span data-stu-id="be23a-170">Install hello latest version of hello PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) module.</span></span> <span data-ttu-id="be23a-171">Ha új tooAzure PowerShell, lásd: [Azure PowerShell áttekintése](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="be23a-171">If you're new tooAzure PowerShell, see [Azure PowerShell overview](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="be23a-172">Indítson el egy PowerShell-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="be23a-172">Start a PowerShell session.</span></span>
3. <span data-ttu-id="be23a-173">A PowerShellben, jelentkezzen be tooAzure hello megadásával `Add-AzureAccount` parancsot.</span><span class="sxs-lookup"><span data-stu-id="be23a-173">In PowerShell, log in tooAzure by entering hello `Add-AzureAccount` command.</span></span>
4. <span data-ttu-id="be23a-174">Módosítsa a következő hello elérési út és fájlnév, mint a megfelelő, majd exportálja a meglévő hálózati konfigurációs fájlban:</span><span class="sxs-lookup"><span data-stu-id="be23a-174">Change hello following path and filename, as appropriate, then export your existing network configuration file:</span></span>

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. <span data-ttu-id="be23a-175">toocreate nyilvános és titkos alhálózatokat, virtuális hálózat használata bármilyen szöveg szerkesztő tooadd hello **VirtualNetworkSite** elem toohello hálózati konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="be23a-175">toocreate a virtual network with public and private subnets, use any text editor tooadd hello **VirtualNetworkSite** element that follows toohello network configuration file.</span></span>

    ```xml
    <VirtualNetworkSite name="myVnet" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Private">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Public">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    ```

    <span data-ttu-id="be23a-176">Felülvizsgálati hello teljes [hálózati konfigurációs fájl séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="be23a-176">Review hello full [network configuration file schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>

6. <span data-ttu-id="be23a-177">Hello hálózati konfigurációs fájl importálása:</span><span class="sxs-lookup"><span data-stu-id="be23a-177">Import hello network configuration file:</span></span>

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > <span data-ttu-id="be23a-178">Megváltozott hálózati konfigurációs fájlok importálása okozhat módosítások tooexisting virtuális hálózatok (klasszikus), az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="be23a-178">Importing a changed network configuration file can cause changes tooexisting virtual networks (classic) in your subscription.</span></span> <span data-ttu-id="be23a-179">Győződjön meg arról, csak akkor hozzá hello korábbi virtuális hálózaton, és módosítsa vagy távolítsa el a meglévő virtuális hálózatok az előfizetésből nem.</span><span class="sxs-lookup"><span data-stu-id="be23a-179">Ensure you only add hello previous virtual network and that you don't change or remove any existing virtual networks from your subscription.</span></span> 

7. <span data-ttu-id="be23a-180">Tekintse át a virtuális hálózati hello és -alhálózatok:</span><span class="sxs-lookup"><span data-stu-id="be23a-180">Review hello virtual network and subnets:</span></span>

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. <span data-ttu-id="be23a-181">**Nem kötelező**: érdemes toodelete hello erőforrások létrehozott Ez az oktatóanyag befejezésekor, hogy a használati költségek.</span><span class="sxs-lookup"><span data-stu-id="be23a-181">**Optional**: You might want toodelete hello resources that you created when you finish this tutorial, so that you don't incur usage charges.</span></span> <span data-ttu-id="be23a-182">toodelete hello virtuális hálózat, teljes lépések 4 – 6 újra, az idő eltávolításakor hello **VirtualNetworkSite** 5 lépésben hozzáadott elemet.</span><span class="sxs-lookup"><span data-stu-id="be23a-182">toodelete hello virtual network, complete steps 4-6 again, this time removing hello **VirtualNetworkSite** element you added in step 5.</span></span>
 
> [!NOTE]
> <span data-ttu-id="be23a-183">Abban az esetben, ha egy erőforrás csoport toocreate egy virtuális hálózat (klasszikus) nem adható meg a PowerShell használatával, ha Azure hello virtuális hálózatot hoz létre a nevű erőforráscsoportban *alapértelmezett-hálózat*.</span><span class="sxs-lookup"><span data-stu-id="be23a-183">Though you can't specify a resource group toocreate a virtual network (classic) in using PowerShell, Azure creates hello virtual network in a resource group named *Default-Networking*.</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="be23a-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="be23a-184">Next steps</span></span>

- <span data-ttu-id="be23a-185">minden virtuális hálózati és alhálózati beállítások toolearn lásd: [virtuális hálózatok kezeléséhez](virtual-network-manage-network.md) és [kezelheti a virtuális hálózati alhálózat](virtual-network-manage-subnet.md).</span><span class="sxs-lookup"><span data-stu-id="be23a-185">toolearn about all virtual network and subnet settings, see [Manage virtual networks](virtual-network-manage-network.md) and [Manage virtual network subnets](virtual-network-manage-subnet.md).</span></span> <span data-ttu-id="be23a-186">Lehetősége van különböző virtuális hálózatok és alhálózatok használatát mutatja be egy éles környezetben toomeet eltérő követelmények vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="be23a-186">You have various options for using virtual networks and subnets in a production environment toomeet different requirements.</span></span>
- <span data-ttu-id="be23a-187">toofilter bejövő és kimenő forgalmat, létrehozása és alkalmazása [hálózati biztonsági csoportok](virtual-networks-nsg.md) toosubnets.</span><span class="sxs-lookup"><span data-stu-id="be23a-187">toofilter inbound and outbound subnet traffic, create and apply [network security groups](virtual-networks-nsg.md) toosubnets.</span></span>
- <span data-ttu-id="be23a-188">Hozzon létre egy [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy egy [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gépet, és csatlakoztassa tooan már meglévő virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="be23a-188">Create a [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) or a [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtual machine, and then connect it tooan existing virtual network.</span></span>
- <span data-ttu-id="be23a-189">tooconnect két virtuális hálózatok a hello Azure ugyanazon a helyen, és hozzon létre egy [virtuális hálózati társviszony-létesítés](create-peering-different-deployment-models.md) hello virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="be23a-189">tooconnect two virtual networks in hello same Azure location, create a  [virtual network peering](create-peering-different-deployment-models.md) between hello virtual networks.</span></span> <span data-ttu-id="be23a-190">Is egyenrangú egy virtuális hálózat (erőforrás-kezelő) tooa virtuális hálózat (klasszikus), de nem hozható létre társviszony-létesítés virtuális hálózatok (klasszikus) között.</span><span class="sxs-lookup"><span data-stu-id="be23a-190">You can peer a virtual network (Resource Manager) tooa virtual network (classic), but you cannot create a peering between two virtual networks (classic).</span></span>
- <span data-ttu-id="be23a-191">Hello virtuális hálózat tooan a helyszíni hálózati használatával kapcsolódnak a [VPN-átjáró](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) körön.</span><span class="sxs-lookup"><span data-stu-id="be23a-191">Connect hello virtual network tooan on-premises network by using a [VPN Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) or [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) circuit.</span></span>
