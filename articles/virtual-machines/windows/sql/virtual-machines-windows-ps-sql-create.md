---
title: "SQL Server virtuális gépen az Azure PowerShell (erőforrás-kezelő) aaaCreate |} Microsoft Docs"
description: "Lépéseket és a PowerShell-parancsfájlok biztosít az Azure virtuális gép létrehozása az SQL Server virtuális gép a gyűjtemény lemezképei."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/17/2017
ms.author: jroth
ms.openlocfilehash: 2b8cb8f69ff9894a95eab617816a60c8674eeefa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a><span data-ttu-id="aaa52-103">Egy SQL Server rendszerű virtuális gép az Azure PowerShell (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="aaa52-103">Provision a SQL Server virtual machine using Azure PowerShell (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="aaa52-104">Portál</span><span class="sxs-lookup"><span data-stu-id="aaa52-104">Portal</span></span>](virtual-machines-windows-portal-sql-server-provision.md)
> * [<span data-ttu-id="aaa52-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="aaa52-105">PowerShell</span></span>](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a><span data-ttu-id="aaa52-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="aaa52-106">Overview</span></span>
<span data-ttu-id="aaa52-107">Az oktatóanyag bemutatja, hogyan using Azure virtuális gép egyetlen toocreate hello **Azure Resource Manager** telepítési modell Azure PowerShell-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="aaa52-107">This tutorial shows you how toocreate a single Azure virtual machine using hello **Azure Resource Manager** deployment model using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="aaa52-108">Ebben az oktatóanyagban létre fogunk hozni egy egyetlen meghajtó lemezkép használatát hello SQL gyűjtemény egyetlen virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="aaa52-108">In this tutorial, we will create a single virtual machine using a single disk drive from an image in hello SQL Gallery.</span></span> <span data-ttu-id="aaa52-109">Hello tárolási, hálózati és számítási erőforrásokat hello virtuális gép által használt új szolgáltatók létre fogunk hozni.</span><span class="sxs-lookup"><span data-stu-id="aaa52-109">We will create new providers for hello storage, network, and compute resources that will be used by hello virtual machine.</span></span> <span data-ttu-id="aaa52-110">Ha meglévő szolgáltatók bármely ezeket az erőforrásokat, helyette használhatja azokat a szolgáltatókat.</span><span class="sxs-lookup"><span data-stu-id="aaa52-110">If you have existing providers for any of these resources, you can use those providers instead.</span></span>

<span data-ttu-id="aaa52-111">Ha ez a témakör a klasszikus verzióra kell hello, lásd: [egy SQL Server rendszerű virtuális gép használata a klasszikus Azure-PowerShell](../classic/ps-sql-create.md).</span><span class="sxs-lookup"><span data-stu-id="aaa52-111">If you need hello classic version of this topic, see [Provision a SQL Server virtual machine using Azure PowerShell Classic](../classic/ps-sql-create.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aaa52-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="aaa52-112">Prerequisites</span></span>
<span data-ttu-id="aaa52-113">Ebben az oktatóanyagban lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="aaa52-113">For this tutorial you'll need:</span></span>

* <span data-ttu-id="aaa52-114">Egy Azure-fiókot és az előfizetés megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="aaa52-114">An Azure account and subscription before you start.</span></span> <span data-ttu-id="aaa52-115">Ha még nincs fiókja, regisztráljon egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="aaa52-115">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="aaa52-116">[Az Azure PowerShell)](/powershell/azure/overview), minimális verziója 1.4.0 vagy újabb (Ez az oktatóanyag verziójával 1.5.0 írása).</span><span class="sxs-lookup"><span data-stu-id="aaa52-116">[Azure PowerShell)](/powershell/azure/overview), minimum version of 1.4.0 or later (this tutorial written using version 1.5.0).</span></span>
  * <span data-ttu-id="aaa52-117">tooretrieve a verziója, a típus **Azure Get-Module - ListAvailable**.</span><span class="sxs-lookup"><span data-stu-id="aaa52-117">tooretrieve your version, type **Get-Module Azure -ListAvailable**.</span></span>

## <a name="configure-your-subscription"></a><span data-ttu-id="aaa52-118">Az előfizetés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aaa52-118">Configure your subscription</span></span>
<span data-ttu-id="aaa52-119">Nyissa meg a Windows Powershellt, és hozzáférést tooyour Azure-fiók létrehozásához hello a következő parancsmag futtatásával.</span><span class="sxs-lookup"><span data-stu-id="aaa52-119">Open Windows PowerShell and establish access tooyour Azure account by running hello following cmdlet.</span></span> <span data-ttu-id="aaa52-120">Választhat egy bejelentkezési képernyő tooenter a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="aaa52-120">You will be presented with a sign in screen tooenter your credentials.</span></span> <span data-ttu-id="aaa52-121">Használja az azonos hello e-mailek és a jelszó toosign használhatja a toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="aaa52-121">Use hello same email and password that you use toosign in toohello Azure portal.</span></span>

    Add-AzureRmAccount

<span data-ttu-id="aaa52-122">Sikeres bejelentkezés után néhány információt a képernyőn hello; előfizetés-azonosító, amelyhez a bejelentkezett megjelenik.</span><span class="sxs-lookup"><span data-stu-id="aaa52-122">After successfully signing in you will see some information on screen that includes hello SubscriptionId with which you signed in.</span></span> <span data-ttu-id="aaa52-123">Ez a hello előfizetés, amely ebben az oktatóanyagban hello erőforrások létrehozza kivéve tooa különböző előfizetés módosítása.</span><span class="sxs-lookup"><span data-stu-id="aaa52-123">This is hello subscription in which hello resources for this tutorial will be created unless you change tooa different subscription.</span></span> <span data-ttu-id="aaa52-124">Ha több SubscriptionIds, futtassa a következő parancsmag tooreturn hello összes a SubscriptionIds listáját:</span><span class="sxs-lookup"><span data-stu-id="aaa52-124">If you have multiple SubscriptionIds, run hello following cmdlet tooreturn a list of all of your SubscriptionIds:</span></span>

    Get-AzureRmSubscription

<span data-ttu-id="aaa52-125">Futtassa a következő parancsmagot a kívánt előfizetés-azonosítójú hello toochange tooanother, előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="aaa52-125">toochange tooanother SubscriptionID, run hello following cmdlet with your desired SubscriptionId.</span></span>

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a><span data-ttu-id="aaa52-126">Adja meg a lemezkép változókat</span><span class="sxs-lookup"><span data-stu-id="aaa52-126">Define image variables</span></span>
<span data-ttu-id="aaa52-127">toosimplify használhatósága és ismeretekkel rendelkezik az oktatóanyag befejezése hello parancsfájlt, először lesz számos változó meghatározásával.</span><span class="sxs-lookup"><span data-stu-id="aaa52-127">toosimplify usability and understanding of hello completed script from this tutorial, we will start by defining a number of variables.</span></span> <span data-ttu-id="aaa52-128">Hello paraméterértékek igényei, de elnevezési korlátozások kapcsolódó tooname hosszak és speciális karakterek ügyeljen arra, ha módosítja a megadott hello értékek módosítása</span><span class="sxs-lookup"><span data-stu-id="aaa52-128">Change hello parameter values as you see fit, but beware of naming restrictions related tooname lengths and special characters when modifying hello values provided.</span></span>

### <a name="location-and-resource-group"></a><span data-ttu-id="aaa52-129">Hely és az erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="aaa52-129">Location and Resource Group</span></span>
<span data-ttu-id="aaa52-130">Két változók toodefine hello adatok régió és hello erőforráscsoport amelybe létrehozhat hello egyéb erőforrások hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="aaa52-130">Use two variables toodefine hello data region and hello resource group into which you will create hello other resources for hello virtual machine.</span></span>

<span data-ttu-id="aaa52-131">Módosítsa a kívánt módon működjenek, és majd hajtható végre a következő parancsmagok tooinitialize hello változókhoz.</span><span class="sxs-lookup"><span data-stu-id="aaa52-131">Modify as desired and then execute hello following cmdlets tooinitialize these variables.</span></span>

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a><span data-ttu-id="aaa52-132">Tárolási tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="aaa52-132">Storage properties</span></span>
<span data-ttu-id="aaa52-133">A következő változók toodefine hello tárolási fiók és hello típusú hello virtuális gép által használt tárolási toobe hello használata.</span><span class="sxs-lookup"><span data-stu-id="aaa52-133">Use hello following variables toodefine hello storage account and hello type of storage toobe used by hello virtual machine.</span></span>

<span data-ttu-id="aaa52-134">Módosítsa a kívánt módon működjenek, és majd hajtható végre a következő parancsmag tooinitialize hello változókhoz.</span><span class="sxs-lookup"><span data-stu-id="aaa52-134">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span> <span data-ttu-id="aaa52-135">Vegye figyelembe, hogy a jelen példában használjuk [prémium szintű Storage](../../../storage/common/storage-premium-storage.md), amely a termelési számítási feladatokhoz ajánlott.</span><span class="sxs-lookup"><span data-stu-id="aaa52-135">Note that in this example, we are using [Premium Storage](../../../storage/common/storage-premium-storage.md), which is recommended for production workloads.</span></span> <span data-ttu-id="aaa52-136">Ez az útmutató és más javaslatokról a részletekért lásd: [teljesítmény ajánlott eljárások az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-performance.md).</span><span class="sxs-lookup"><span data-stu-id="aaa52-136">For details on this guidance and other recommendations, see [Performance best practices for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).</span></span>

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a><span data-ttu-id="aaa52-137">Hálózat tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="aaa52-137">Network properties</span></span>
<span data-ttu-id="aaa52-138">A következő változók toodefine hello hálózati illesztő, hello TCP/IP-kiosztási módszerrel, hello virtuálishálózat-név, hello virtuális alhálózat neve, az IP-címek hello hello virtuális hálózat, hello az IP-címek hello alhálózati és hello hello használata nyilvános tartomány neve címke toobe hello hálózat hello virtuális gép által használt.</span><span class="sxs-lookup"><span data-stu-id="aaa52-138">Use hello following variables toodefine hello network interface, hello TCP/IP allocation method, hello virtual network name, hello virtual subnet name, hello range of IP addresses for hello virtual network, hello range of IP addresses for hello subnet, and hello public domain name label toobe used by hello network in hello virtual machine.</span></span>

<span data-ttu-id="aaa52-139">Módosítsa a kívánt módon működjenek, és majd hajtható végre a következő parancsmag tooinitialize hello változókhoz.</span><span class="sxs-lookup"><span data-stu-id="aaa52-139">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span>

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"

### <a name="virtual-machine-properties"></a><span data-ttu-id="aaa52-140">Virtuális gép tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="aaa52-140">Virtual machine properties</span></span>
<span data-ttu-id="aaa52-141">A következő változók toodefine hello virtuálisgép-nevet, hello számítógépnév, hello virtuálisgép-méret és operációs rendszer Lemeznév hello hello virtuális gép hello használata.</span><span class="sxs-lookup"><span data-stu-id="aaa52-141">Use hello following variables toodefine hello virtual machine name, hello computer name, hello virtual machine size, and hello operating system disk name for hello virtual machine.</span></span>

<span data-ttu-id="aaa52-142">Módosítsa a kívánt módon működjenek, és majd hajtható végre a következő parancsmag tooinitialize hello változókhoz.</span><span class="sxs-lookup"><span data-stu-id="aaa52-142">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span>

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a><span data-ttu-id="aaa52-143">Lemezkép tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="aaa52-143">Image properties</span></span>
<span data-ttu-id="aaa52-144">A következő változók toodefine hello kép toouse hello virtuális gép hello használata.</span><span class="sxs-lookup"><span data-stu-id="aaa52-144">Use hello following variables toodefine hello image toouse for hello virtual machine.</span></span> <span data-ttu-id="aaa52-145">Ebben a példában hello SQL Server 2016 Enterprise lemezképet használja.</span><span class="sxs-lookup"><span data-stu-id="aaa52-145">In this example, hello SQL Server 2016 Enterprise image is used.</span></span>

<span data-ttu-id="aaa52-146">Módosítsa a kívánt módon működjenek, és majd hajtható végre a következő parancsmag tooinitialize hello változókhoz.</span><span class="sxs-lookup"><span data-stu-id="aaa52-146">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span>

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

<span data-ttu-id="aaa52-147">Vegye figyelembe, hogy kaphat a teljes listáját az SQL Server kép ajánlatokat a Get-AzureRmVMImageOffer hello paranccsal:</span><span class="sxs-lookup"><span data-stu-id="aaa52-147">Note that you can get a full list of SQL Server image offerings with hello Get-AzureRmVMImageOffer command:</span></span>

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

<span data-ttu-id="aaa52-148">És egy ajánlatot a Get-AzureRmVMImageSku hello paranccsal elérhető termékváltozatok hello látható.</span><span class="sxs-lookup"><span data-stu-id="aaa52-148">And you can see hello Skus available for an offering with hello Get-AzureRmVMImageSku command.</span></span> <span data-ttu-id="aaa52-149">hello alábbi a parancs megjeleníti a termékváltozatokat hello elérhető **SQL2014SP1-WS2012R2** kínálnak.</span><span class="sxs-lookup"><span data-stu-id="aaa52-149">hello following command shows all Skus available for hello **SQL2014SP1-WS2012R2** offer.</span></span>

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a><span data-ttu-id="aaa52-150">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="aaa52-150">Create a resource group</span></span>
<span data-ttu-id="aaa52-151">Hello Resource Manager üzembe helyezési modelljével hello első-objektumnak hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="aaa52-151">With hello Resource Manager deployment model, hello first object that you create is hello resource group.</span></span> <span data-ttu-id="aaa52-152">Hello használjuk [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsmag toocreate egy Azure-erőforráscsoportot és az erőforrások hello erőforrás csoport nevét és helyét, amelyek korábban inicializálták hello változók határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="aaa52-152">We will use hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet toocreate an Azure resource group and its resources with hello resource group name and location defined by hello variables that you previously initialized.</span></span>

<span data-ttu-id="aaa52-153">Az új erőforráscsoport hajtható végre a következő parancsmag toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="aaa52-153">Execute hello following cmdlet toocreate your new resource group.</span></span>

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a><span data-ttu-id="aaa52-154">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="aaa52-154">Create a storage account</span></span>
<span data-ttu-id="aaa52-155">hello virtuális gép igényel a tárolási erőforrások hello operációsrendszer-lemez és az SQL Server-adatok hello és naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="aaa52-155">hello virtual machine requires storage resources for hello operating system disk and for hello SQL Server data and log files.</span></span> <span data-ttu-id="aaa52-156">Az egyszerűség kedvéért létre fogunk hozni egy lemezt is.</span><span class="sxs-lookup"><span data-stu-id="aaa52-156">For simplicity, we will create a single disk for both.</span></span> <span data-ttu-id="aaa52-157">Később a hello segítségével további lemezek csatolhat [Add-Azure lemez](/powershell/module/azure/add-azuredisk) parancsmag tooplace rendezés az SQL Server-adatok és naplófájlok dedikált lemezeken.</span><span class="sxs-lookup"><span data-stu-id="aaa52-157">You can attach additional disks later using hello [Add-Azure Disk](/powershell/module/azure/add-azuredisk) cmdlet in order tooplace your SQL Server data and log files on dedicated disks.</span></span> <span data-ttu-id="aaa52-158">Hello használjuk [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmag toocreate egy standard tárolási fiókra az új erőforráscsoport és hello tárfiók neve, a tárolási Sku név és a hely definiált hello változók használata korábban inicializálták.</span><span class="sxs-lookup"><span data-stu-id="aaa52-158">We will use hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet toocreate a standard storage account in your new resource group and with hello storage account name, storage Sku name, and location defined using hello variables that you previously initialized.</span></span>

<span data-ttu-id="aaa52-159">Hajtsa végre a következő parancsmag toocreate hello új tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="aaa52-159">Execute hello following cmdlet toocreate your new storage account.</span></span>

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a><span data-ttu-id="aaa52-160">Hálózati erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaa52-160">Create network resources</span></span>
<span data-ttu-id="aaa52-161">hello virtuális gép hálózati erőforrások több hálózati kapcsolatot igényel.</span><span class="sxs-lookup"><span data-stu-id="aaa52-161">hello virtual machine requires a number of network resources for network connectivity.</span></span>

* <span data-ttu-id="aaa52-162">Minden virtuális gép virtuális hálózat szükséges.</span><span class="sxs-lookup"><span data-stu-id="aaa52-162">Each virtual machine requires a virtual network.</span></span>
* <span data-ttu-id="aaa52-163">A virtuális hálózati rendelkeznie kell legalább egy meghatározott alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="aaa52-163">A virtual network must have at least one subnet defined.</span></span>
* <span data-ttu-id="aaa52-164">Egy adott hálózati csatoló nyilvános vagy magán IP-címet kell megadni.</span><span class="sxs-lookup"><span data-stu-id="aaa52-164">A network interface must be defined with either a public or a private IP address.</span></span>

### <a name="create-a-virtual-network-subnet-configuration"></a><span data-ttu-id="aaa52-165">Hozzon létre egy virtuális hálózati alhálózat beállítása</span><span class="sxs-lookup"><span data-stu-id="aaa52-165">Create a virtual network subnet configuration</span></span>
<span data-ttu-id="aaa52-166">Először hozzon létre a virtuális hálózati alhálózat konfigurációját fogja.</span><span class="sxs-lookup"><span data-stu-id="aaa52-166">We will start by creating a subnet configuration for our virtual network.</span></span> <span data-ttu-id="aaa52-167">A jelen oktatóanyagban létrehozunk egy alapértelmezett alhálózatot hello [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aaa52-167">For our tutorial, we will create a default subnet using hello [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet.</span></span> <span data-ttu-id="aaa52-168">Létre fogunk hozni a virtuális hálózati alhálózat beállítása hello alhálózat nevét és címét előtaggal rendelkező megadott hello változókat, amelyek korábban inicializálták.</span><span class="sxs-lookup"><span data-stu-id="aaa52-168">We will create our virtual network subnet configuration with hello subnet name and address prefix defined using hello variables that you previously initialized.</span></span>

> [!NOTE]
> <span data-ttu-id="aaa52-169">Hello virtuális hálózati alhálózat konfiguráció Ez a parancsmag segítségével további tulajdonságok adhatók, de ez az oktatóanyag hello terjed.</span><span class="sxs-lookup"><span data-stu-id="aaa52-169">You can define additional properties of hello virtual network subnet configuration using this cmdlet, but that is beyond hello scope of this tutorial.</span></span>
>
>

<span data-ttu-id="aaa52-170">A következő parancsmag toocreate hello hajtható végre a virtuális alhálózati konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="aaa52-170">Execute hello following cmdlet toocreate your virtual subnet configuration.</span></span>

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a><span data-ttu-id="aaa52-171">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaa52-171">Create a virtual network</span></span>
<span data-ttu-id="aaa52-172">Ezután létrehozunk hello segítségével a virtuális hálózati [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aaa52-172">Next, we will create our virtual network using hello [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet.</span></span> <span data-ttu-id="aaa52-173">Létre fogunk hozni a virtuális hálózat az új erőforráscsoportban, hello nevét, helyét és címelőtag definiálva, amely korábban inicializálták hello változók használata, és hello előző lépésben megadott hello alhálózati konfigurációt használ.</span><span class="sxs-lookup"><span data-stu-id="aaa52-173">We will create our virtual network in your new resource group, with hello name, location, and address prefix defined using hello variables that you previously initialized, and using hello subnet configuration that you defined in hello previous step.</span></span>

<span data-ttu-id="aaa52-174">A következő parancsmag toocreate hello hajtható végre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="aaa52-174">Execute hello following cmdlet toocreate your virtual network.</span></span>

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-hello-public-ip-address"></a><span data-ttu-id="aaa52-175">Hello nyilvános IP-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaa52-175">Create hello public IP address</span></span>
<span data-ttu-id="aaa52-176">Most, hogy a megadott virtuális hálózat, tooconfigure IP-címet kell kapcsolatot toohello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="aaa52-176">Now that we have our virtual network defined, we need tooconfigure an IP address for connectivity toohello virtual machine.</span></span> <span data-ttu-id="aaa52-177">Ebben az oktatóanyagban létre fogunk hozni egy nyilvános IP-címet, dinamikus IP-címzés toosupport internetkapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="aaa52-177">For this tutorial, we will create a public IP address using dynamic IP addressing toosupport Internet connectivity.</span></span> <span data-ttu-id="aaa52-178">Hello használjuk [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) parancsmag toocreate hello nyilvános IP-cím hello erőforrás létrehozott csoport prevously és hello nevét, helyét, elosztási módszert és DNS tartománynév-címke definiálva hello használata az változókat, amelyek korábban inicializálták.</span><span class="sxs-lookup"><span data-stu-id="aaa52-178">We will use hello [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) cmdlet toocreate hello public IP address in hello resource group created prevously and with hello name, location, allocation method, and DNS domain name label defined using hello variables that you previously initialized.</span></span>

> [!NOTE]
> <span data-ttu-id="aaa52-179">Hello azt az nyilvános IP-cím, ez a parancsmag segítségével további tulajdonságok adhatók, de ez az első oktatóanyag hello terjed.</span><span class="sxs-lookup"><span data-stu-id="aaa52-179">You can define additional properties of hello public IP address using this cmdlet, but that is beyond hello scope of this initial tutorial.</span></span> <span data-ttu-id="aaa52-180">Is létrehozhatja saját cím vagy a cím statikus címmel rendelkező, de, amely még nem ez az oktatóanyag hello terjed.</span><span class="sxs-lookup"><span data-stu-id="aaa52-180">You could also create a private address or an address with a static address, but that is also beyond hello scope of this tutorial.</span></span>
>
>

<span data-ttu-id="aaa52-181">Hajtsa végre a következő parancsmag toocreate hello a nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="aaa52-181">Execute hello following cmdlet toocreate your public IP address.</span></span>

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-hello-network-interface"></a><span data-ttu-id="aaa52-182">Hello hálózati illesztő létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaa52-182">Create hello network interface</span></span>
<span data-ttu-id="aaa52-183">Dolgozunk most már készen áll a toocreate hello hálózati illesztő a virtuális gép által használt.</span><span class="sxs-lookup"><span data-stu-id="aaa52-183">We are now ready toocreate hello network interface that our virtual machine will use.</span></span> <span data-ttu-id="aaa52-184">Hello használjuk [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) parancsmag toocreate a hálózati illesztő a korábban létrehozott erőforráscsoportot hello és hello nevű helyen, alhálózat és nyilvános IP-címet korábban már definiálva.</span><span class="sxs-lookup"><span data-stu-id="aaa52-184">We will use hello [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet toocreate our network interface in hello resource group created earlier and with hello name, location, subnet and public IP address previously defined.</span></span>

<span data-ttu-id="aaa52-185">Hajtsa végre a következő parancsmag toocreate hello a hálózati illesztő.</span><span class="sxs-lookup"><span data-stu-id="aaa52-185">Execute hello following cmdlet toocreate your network interface.</span></span>

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a><span data-ttu-id="aaa52-186">A Virtuálisgép-objektum konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aaa52-186">Configure a VM object</span></span>
<span data-ttu-id="aaa52-187">Most, hogy meghatározott tárolási és hálózati erőforrásokat, vagy folyamatban hello virtuális gép készen áll a toodefine számítási erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="aaa52-187">Now that we have storage and network resources defined, we are ready toodefine compute resources for hello virtual machine.</span></span> <span data-ttu-id="aaa52-188">A jelen oktatóanyagban azt fogja hello virtuálisgép-méret és a különböző operációs rendszer tulajdonságait adja meg, adja meg azt a korábban létrehozott hello hálózati illesztő, blob-tároló megadása, és adja hello operációsrendszer-lemez.</span><span class="sxs-lookup"><span data-stu-id="aaa52-188">For our tutorial, we will specify hello virtual machine size and various operating system properties, specify hello network interface that we previously created, define blob storage, and then specify hello operating system disk.</span></span>

### <a name="create-hello-vm-object"></a><span data-ttu-id="aaa52-189">Hello Virtuálisgép-objektum létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaa52-189">Create hello VM object</span></span>
<span data-ttu-id="aaa52-190">A Microsoft hello virtuálisgép-méret megadásával indul el.</span><span class="sxs-lookup"><span data-stu-id="aaa52-190">We will start by specifying hello virtual machine size.</span></span> <span data-ttu-id="aaa52-191">Ebben az oktatóanyagban egy DS13 meg azt.</span><span class="sxs-lookup"><span data-stu-id="aaa52-191">For this tutorial, we are specifying a DS13.</span></span> <span data-ttu-id="aaa52-192">Hello használjuk [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) parancsmag toocreate egy konfigurálható virtuális gép objektum hello neve és mérete megadott hello változókat, amelyek korábban inicializálták.</span><span class="sxs-lookup"><span data-stu-id="aaa52-192">We will use hello [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) cmdlet toocreate a configurable virtual machine object with hello name and size defined using hello variables that you previously initialized.</span></span>

<span data-ttu-id="aaa52-193">A következő parancsmag toocreate hello virtuálisgép-objektumot hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="aaa52-193">Execute hello following cmdlet toocreate hello virtual machine object.</span></span>

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-toohold-hello-name-and-password-for-hello-local-administrator-credentials"></a><span data-ttu-id="aaa52-194">A hitelesítő objektum toohold hello nevet és jelszót hello helyi rendszergazdai hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaa52-194">Create a credential object toohold hello name and password for hello local administrator credentials</span></span>
<span data-ttu-id="aaa52-195">Azt is beállíthatja a hello operációs rendszer tulajdonságokat hello virtuális géphez, igazolnia kell toosupply hello hitelesítő adatok hello helyi rendszergazdai fiók, egy biztonságos karakterláncot kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="aaa52-195">Before we can set hello operating system properties for hello virtual machine, we need toosupply hello credentials for hello local administrator account as a secure string.</span></span> <span data-ttu-id="aaa52-196">tooaccomplish, használjuk hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aaa52-196">tooaccomplish this, we will use hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

<span data-ttu-id="aaa52-197">A következő parancsmag hello hajtható végre, és hello Windows PowerShell hitelesítő adatok kérelem ablakban írja be a hello helyi rendszergazdai fiók hello Windows virtuális gép nevét és jelszavát toouse hello.</span><span class="sxs-lookup"><span data-stu-id="aaa52-197">Execute hello following cmdlet and, in hello Windows PowerShell credential request window, type hello name and password toouse for hello local administrator account in hello Windows virtual machine.</span></span>

    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."

### <a name="set-hello-operating-system-properties-for-hello-virtual-machine"></a><span data-ttu-id="aaa52-198">Hello hello virtuális gép operációs rendszer tulajdonságainak beállítása</span><span class="sxs-lookup"><span data-stu-id="aaa52-198">Set hello operating system properties for hello virtual machine</span></span>
<span data-ttu-id="aaa52-199">Most már készen áll a tooset hello virtuális gép operációs rendszer tulajdonságainak folyamatban.</span><span class="sxs-lookup"><span data-stu-id="aaa52-199">Now we are ready tooset hello virtual machine's operating system properties.</span></span> <span data-ttu-id="aaa52-200">Hello használjuk [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) parancsmag tooset hello típusú operációs rendszerrel, a Windows hello szükséges [virtuális gép ügynökének](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toobe telepítve, adja meg, hogy hello parancsmag lehetővé teszi, hogy az automatikus frissítés és hello virtuális gép neve, hello számítógép neve és hello hitelesítő adatok használatával hello változókat, amelyek korábban inicializálták.</span><span class="sxs-lookup"><span data-stu-id="aaa52-200">We will use hello [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet tooset hello type of operating system as Windows, require hello [virtual machine agent](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toobe installed, specify that hello cmdlet enables auto update and set hello virtual machine name, hello computer name, and hello credential using hello variables that you previously initialized.</span></span>

<span data-ttu-id="aaa52-201">Hajtsa végre a következő parancsmag tooset hello operációs rendszer tulajdonságai a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="aaa52-201">Execute hello following cmdlet tooset hello operating system properties for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-hello-network-interface-toohello-virtual-machine"></a><span data-ttu-id="aaa52-202">Hello hálózati illesztő toohello virtuális gép hozzáadása</span><span class="sxs-lookup"><span data-stu-id="aaa52-202">Add hello network interface toohello virtual machine</span></span>
<span data-ttu-id="aaa52-203">A következő adunk hozzá hello hálózati illesztő létrehozott korábban toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="aaa52-203">Next, we will add hello network interface that we created previously toohello virtual machine.</span></span> <span data-ttu-id="aaa52-204">Hello használjuk [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) parancsmag tooadd hello hálózati illesztő hello hálózati illesztő változóval, amelyet korábban megadott.</span><span class="sxs-lookup"><span data-stu-id="aaa52-204">We will use hello [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet tooadd hello network interface using hello network interface variable that you defined earlier.</span></span>

<span data-ttu-id="aaa52-205">A következő parancsmag tooset hello hálózati illesztő a virtuális gép hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="aaa52-205">Execute hello following cmdlet tooset hello network interface for your virtual machine.</span></span>

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-hello-blob-storage-location-for-hello-disk-toobe-used-by-hello-virtual-machine"></a><span data-ttu-id="aaa52-206">Hello blob storage helyének hello lemez toobe hello virtuális gép által használt beállítása</span><span class="sxs-lookup"><span data-stu-id="aaa52-206">Set hello blob storage location for hello disk toobe used by hello virtual machine</span></span>
<span data-ttu-id="aaa52-207">A következő azt állítja be, amelyet korábban megadott hello változók használata hello virtuális gép által használt hello lemez toobe hello blob tárolási helyét.</span><span class="sxs-lookup"><span data-stu-id="aaa52-207">Next, we will set hello blob storage location for hello disk toobe used by hello virtual machine using hello variables that you defined earlier.</span></span>

<span data-ttu-id="aaa52-208">A következő parancsmag tooset hello blob tároló helye hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="aaa52-208">Execute hello following cmdlet tooset hello blob storage location.</span></span>

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-hello-operating-system-disk-properties-for-hello-virtual-machine"></a><span data-ttu-id="aaa52-209">Hello operációs rendszer virtuális gép hello lemez tulajdonságainak beállítása</span><span class="sxs-lookup"><span data-stu-id="aaa52-209">Set hello operating system disk properties for hello virtual machine</span></span>
<span data-ttu-id="aaa52-210">A következő helyezünk hello operációs rendszer lemez tulajdonságainak hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="aaa52-210">Next, we will set hello operating system disk properties for hello virtual machine.</span></span> <span data-ttu-id="aaa52-211">Hello használjuk [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) parancsmag toospecify hello hello virtuális gép operációs rendszerét, kép, gyorsítótárazás csak tooread tooset határozza meg (mivel az SQL-kiszolgáló van telepítés alatt a hello ugyanazt a lemezt), és határozza meg hello virtuális gép nevét és a korábban meghatározott hello változók segítségével meghatározott hello operációsrendszer-lemez.</span><span class="sxs-lookup"><span data-stu-id="aaa52-211">We will use hello [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet toospecify that hello operating system for hello virtual machine will come from an image, tooset caching tooread only (because SQL Server is being installed on hello same disk) and define hello virtual machine name and hello operating system disk defined using hello variables that we defined earlier.</span></span>

<span data-ttu-id="aaa52-212">Hajtsa végre a következő parancsmag tooset hello operációs rendszer lemez tulajdonságai a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="aaa52-212">Execute hello following cmdlet tooset hello operating system disk properties for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-hello-platform-image-for-hello-virtual-machine"></a><span data-ttu-id="aaa52-213">Adja meg a hello platformlemezképet hello virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="aaa52-213">Specify hello platform image for hello virtual machine</span></span>
<span data-ttu-id="aaa52-214">Az utolsó konfigurációs lépés toospecify hello platformlemezképet a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="aaa52-214">Our last configuration step is toospecify hello platform image for our virtual machine.</span></span> <span data-ttu-id="aaa52-215">A jelen oktatóanyagban hello legújabb SQL Server 2016 CTP kép használunk.</span><span class="sxs-lookup"><span data-stu-id="aaa52-215">For our tutorial, we are using hello latest SQL Server 2016 CTP image.</span></span> <span data-ttu-id="aaa52-216">Hello használjuk [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) parancsmag toouse a kép határozzák meg, amelyet korábban megadott hello változók.</span><span class="sxs-lookup"><span data-stu-id="aaa52-216">We will use hello [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet toouse this image as defined by hello variables that you defined earlier.</span></span>

<span data-ttu-id="aaa52-217">A következő parancsmag toospecify hello platformlemezképet a virtuális gép hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="aaa52-217">Execute hello following cmdlet toospecify hello platform image for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-hello-sql-vm"></a><span data-ttu-id="aaa52-218">Hello SQL virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaa52-218">Create hello SQL VM</span></span>
<span data-ttu-id="aaa52-219">Most, hogy befejezte a hello konfigurációs lépésekkel, készen áll a toocreate hello virtuális gép áll.</span><span class="sxs-lookup"><span data-stu-id="aaa52-219">Now that you have finished hello configuration steps, you are ready toocreate hello virtual machine.</span></span> <span data-ttu-id="aaa52-220">Hello használjuk [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) parancsmag toocreate hello virtuális gépet, hogy definiáltuk hello változók használatával.</span><span class="sxs-lookup"><span data-stu-id="aaa52-220">We will use hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet toocreate hello virtual machine using hello variables that we have defined.</span></span>

<span data-ttu-id="aaa52-221">A következő parancsmag toocreate hello hajtható végre a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="aaa52-221">Execute hello following cmdlet toocreate your virtual machine.</span></span>

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

<span data-ttu-id="aaa52-222">hello a virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="aaa52-222">hello virtual machine is created.</span></span> <span data-ttu-id="aaa52-223">Figyelje meg, hogy egy standard szintű tárfiókot létre vonatkozó rendszerindítási diagnosztika, mert hello megadott hello virtuális gép lemezét tárfiók egy prémium szintű tárfiók.</span><span class="sxs-lookup"><span data-stu-id="aaa52-223">Notice that a standard storage account is created for boot diagnostics because hello specified storage account for hello virtual machine's disk is a premium storage account.</span></span>

<span data-ttu-id="aaa52-224">Most már megtekintheti az ezen a számítógépen a hello Azure Portal toosee [nyilvános IP-címének és a teljes tartománynév](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="aaa52-224">You can now view this machine in hello Azure Portal toosee [its public IP address and its fully qualified domain name](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

## <a name="example-script"></a><span data-ttu-id="aaa52-225">A példaként megadott parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="aaa52-225">Example script</span></span>
<span data-ttu-id="aaa52-226">hello következő parancsfájl hello teljes PowerShell-parancsfájlt tartalmazó ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="aaa52-226">hello following script contains hello complete PowerShell script for this tutorial.</span></span> <span data-ttu-id="aaa52-227">Azt feltételezi, hogy már telepítő hello Azure-előfizetés toouse a hello **Add-AzureRmAccount** és **Select-AzureRmSubscription** parancsok.</span><span class="sxs-lookup"><span data-stu-id="aaa52-227">It assumes that you have already setup hello Azure subscription toouse with hello **Add-AzureRmAccount** and **Select-AzureRmSubscription** commands.</span></span>

    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create hello VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a><span data-ttu-id="aaa52-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aaa52-228">Next steps</span></span>
<span data-ttu-id="aaa52-229">Hello virtuális gép létrehozása után készen áll a tooconnect toohello virtuális gép RDP és a telepítő kapcsolatot áll.</span><span class="sxs-lookup"><span data-stu-id="aaa52-229">After hello virtual machine is created, you are ready tooconnect toohello virtual machine using RDP and setup connectivity.</span></span> <span data-ttu-id="aaa52-230">További információkért lásd: [csatlakozzon az SQL Server virtuális gépet az Azure (erőforrás-kezelő) tooa](virtual-machines-windows-sql-connect.md).</span><span class="sxs-lookup"><span data-stu-id="aaa52-230">For more information, see [Connect tooa SQL Server Virtual Machine on Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).</span></span>

