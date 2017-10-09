---
title: "a virtuális hálózati aaaCreate |} Az Azure Resource Manager-sablon |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy virtuális hálózathoz, az Azure Resource Manager-sablon használatával."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 69530861-2f97-4a6e-b336-a7baf2690044
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b9c289433ff2a84bec19eac25fa28ab40d131c7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-an-azure-resource-manager-template"></a><span data-ttu-id="24bcc-103">Azure Resource Manager-sablonnal virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="24bcc-103">Create a virtual network using an Azure Resource Manager template</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="24bcc-104">Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="24bcc-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="24bcc-105">A Microsoft azt javasolja, hello Resource Manager üzembe helyezési modellben erőforrásoknak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="24bcc-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="24bcc-106">hello arról toolearn hello hello két modellek közötti különbséget olvasási [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="24bcc-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>
 
<span data-ttu-id="24bcc-107">Ez a cikk azt ismerteti, hogyan toocreate egy Vnetet hello erőforrás-kezelő központi modellhez tartozó Azure Resource Manager-sablonnal.</span><span class="sxs-lookup"><span data-stu-id="24bcc-107">This article explains how toocreate a VNet through hello Resource Manager deployment model using an Azure Resource Manager template.</span></span> <span data-ttu-id="24bcc-108">Hozzon létre egy Vnetet Resource Manageren keresztül más eszközök használatával vagy hello klasszikus telepítési modell használatával VNet létrehozása a következő lista hello egy másik lehetőség kiválasztásával is:</span><span class="sxs-lookup"><span data-stu-id="24bcc-108">You can also create a VNet through Resource Manager using other tools or create a VNet through hello classic deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
- [<span data-ttu-id="24bcc-109">Portál</span><span class="sxs-lookup"><span data-stu-id="24bcc-109">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
- [<span data-ttu-id="24bcc-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="24bcc-110">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
- [<span data-ttu-id="24bcc-111">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="24bcc-111">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
- [<span data-ttu-id="24bcc-112">Sablon</span><span class="sxs-lookup"><span data-stu-id="24bcc-112">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
- [<span data-ttu-id="24bcc-113">Portál (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="24bcc-113">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
- [<span data-ttu-id="24bcc-114">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="24bcc-114">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [<span data-ttu-id="24bcc-115">Parancssori felület (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="24bcc-115">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

<span data-ttu-id="24bcc-116">Megtudhatja, hogyan toodownload módosításához és a meglévő ARM-sablont a Githubból, és a GitHub, a PowerShell és a hello Azure CLI hello sablon üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="24bcc-116">You will learn how toodownload and modify and existing ARM template from GitHub, and deploy hello template from GitHub, PowerShell, and hello Azure CLI.</span></span>

<span data-ttu-id="24bcc-117">Ha egyszerűen telepít hello ARM-sablon közvetlenül a Githubból, változtatások nélkül, hagyja ki túl[központi telepítése egy sablont a githubból](#deploy-the-arm-template-by-using-click-to-deploy).</span><span class="sxs-lookup"><span data-stu-id="24bcc-117">If you are simply deploying hello ARM template directly from GitHub, without any changes, skip too[deploy a template from github](#deploy-the-arm-template-by-using-click-to-deploy).</span></span>

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="download-and-understand-hello-azure-resource-manager-template"></a><span data-ttu-id="24bcc-118">Töltse le és hello Azure Resource Manager sablon ismertetése</span><span class="sxs-lookup"><span data-stu-id="24bcc-118">Download and understand hello Azure Resource Manager template</span></span>
<span data-ttu-id="24bcc-119">Létrehozhat egy Vnetet két alhálózattal a Githubról meglévő sablon hello letöltése, hajtsa végre a módosításokat, előfordulhat, hogy szeretné, és újra felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="24bcc-119">You can download hello existing template for creating a VNet and two subnets from GitHub, make any changes you might want, and reuse it.</span></span> <span data-ttu-id="24bcc-120">toodo Igen, végezze el a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="24bcc-120">toodo so, complete hello following steps:</span></span>

1. <span data-ttu-id="24bcc-121">Keresse meg a túl[hello mintasablon lapot](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span><span class="sxs-lookup"><span data-stu-id="24bcc-121">Navigate too[hello sample template page](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span></span>
2. <span data-ttu-id="24bcc-122">Kattintson az **azuredeploy.json**, majd a **RAW** elemre.</span><span class="sxs-lookup"><span data-stu-id="24bcc-122">Click **azuredeploy.json**, and then click **RAW**.</span></span>
3. <span data-ttu-id="24bcc-123">Mentés hello fájl tooa egy helyi mappába a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="24bcc-123">Save hello file tooa a local folder on your computer.</span></span>
4. <span data-ttu-id="24bcc-124">Ha ismeri a sablonok, ugorjon a toostep 7.</span><span class="sxs-lookup"><span data-stu-id="24bcc-124">If you are familiar with templates, skip toostep 7.</span></span>
5. <span data-ttu-id="24bcc-125">Nyissa meg a hello előbb mentett fájlt, és nézze meg a hello tartalma **paraméterek** 5. sorban.</span><span class="sxs-lookup"><span data-stu-id="24bcc-125">Open hello file you just saved and look at hello contents under **parameters** in line 5.</span></span> <span data-ttu-id="24bcc-126">Az ARM-sablonparaméterek az üzembe helyezés során kitölthető paraméterek helyőrzőiként működnek.</span><span class="sxs-lookup"><span data-stu-id="24bcc-126">ARM template parameters provide a placeholder for values that can be filled out during deployment.</span></span>
   
   | <span data-ttu-id="24bcc-127">Paraméter</span><span class="sxs-lookup"><span data-stu-id="24bcc-127">Parameter</span></span> | <span data-ttu-id="24bcc-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="24bcc-128">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="24bcc-129">**hely**</span><span class="sxs-lookup"><span data-stu-id="24bcc-129">**location**</span></span> |<span data-ttu-id="24bcc-130">Azure-régió, ahol hello VNet létrejön</span><span class="sxs-lookup"><span data-stu-id="24bcc-130">Azure region where hello VNet will be created</span></span> |
   | <span data-ttu-id="24bcc-131">**vnetName**</span><span class="sxs-lookup"><span data-stu-id="24bcc-131">**vnetName**</span></span> |<span data-ttu-id="24bcc-132">Hello nevét új virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="24bcc-132">Name for hello new VNet</span></span> |
   | <span data-ttu-id="24bcc-133">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="24bcc-133">**addressPrefix**</span></span> |<span data-ttu-id="24bcc-134">Címtartomány CIDR formátumban VNet hello</span><span class="sxs-lookup"><span data-stu-id="24bcc-134">Address space for hello VNet, in CIDR format</span></span> |
   | <span data-ttu-id="24bcc-135">**subnet1Name**</span><span class="sxs-lookup"><span data-stu-id="24bcc-135">**subnet1Name**</span></span> |<span data-ttu-id="24bcc-136">Hello nevét első virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="24bcc-136">Name for hello first VNet</span></span> |
   | <span data-ttu-id="24bcc-137">**subnet1Prefix**</span><span class="sxs-lookup"><span data-stu-id="24bcc-137">**subnet1Prefix**</span></span> |<span data-ttu-id="24bcc-138">Hello első alhálózat CIDR-blokkja</span><span class="sxs-lookup"><span data-stu-id="24bcc-138">CIDR block for hello first subnet</span></span> |
   | <span data-ttu-id="24bcc-139">**subnet2Name**</span><span class="sxs-lookup"><span data-stu-id="24bcc-139">**subnet2Name**</span></span> |<span data-ttu-id="24bcc-140">Hello nevét a második virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="24bcc-140">Name for hello second VNet</span></span> |
   | <span data-ttu-id="24bcc-141">**subnet2Prefix**</span><span class="sxs-lookup"><span data-stu-id="24bcc-141">**subnet2Prefix**</span></span> |<span data-ttu-id="24bcc-142">Hello második alhálózat CIDR-blokkja</span><span class="sxs-lookup"><span data-stu-id="24bcc-142">CIDR block for hello second subnet</span></span> |
   
   > [!IMPORTANT]
   > <span data-ttu-id="24bcc-143">A GitHubban fenntartott Azure Resource Manager-sablonok idővel módosulhatnak.</span><span class="sxs-lookup"><span data-stu-id="24bcc-143">Azure Resource Manager templates maintained in GitHub can change over time.</span></span> <span data-ttu-id="24bcc-144">Győződjön meg arról, hogy hello sablon ellenőrizze annak használata előtt.</span><span class="sxs-lookup"><span data-stu-id="24bcc-144">Make sure you check hello template before using it.</span></span>
   > 
   > 
6. <span data-ttu-id="24bcc-145">Hello tartalom alapján ellenőrizze **erőforrások** , és figyelje meg a következő hello:</span><span class="sxs-lookup"><span data-stu-id="24bcc-145">Check hello content under **resources** and notice hello following:</span></span>
   
   * <span data-ttu-id="24bcc-146">**type**.</span><span class="sxs-lookup"><span data-stu-id="24bcc-146">**type**.</span></span> <span data-ttu-id="24bcc-147">Hello sablon által létrehozott erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="24bcc-147">Type of resource being created by hello template.</span></span> <span data-ttu-id="24bcc-148">Ebben az esetben a **Microsoft.Network/virtualNetworks**, ami egy VNetet jelöl.</span><span class="sxs-lookup"><span data-stu-id="24bcc-148">In this case, **Microsoft.Network/virtualNetworks**, which represent a VNet.</span></span>
   * <span data-ttu-id="24bcc-149">**Név**</span><span class="sxs-lookup"><span data-stu-id="24bcc-149">**name**.</span></span> <span data-ttu-id="24bcc-150">Hello erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="24bcc-150">Name for hello resource.</span></span> <span data-ttu-id="24bcc-151">Értesítés hello használata **[parameters('vnetName')]**, ami azt jelenti, hogy hello neve lesz a bemenetként megadott hello felhasználó vagy egy paraméterfájl üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="24bcc-151">Notice hello use of **[parameters('vnetName')]**, which means hello name will provided as input by hello user or a parameter file during deployment.</span></span>
   * <span data-ttu-id="24bcc-152">**properties**.</span><span class="sxs-lookup"><span data-stu-id="24bcc-152">**properties**.</span></span> <span data-ttu-id="24bcc-153">Hello erőforrás tulajdonságainak listája.</span><span class="sxs-lookup"><span data-stu-id="24bcc-153">List of properties for hello resource.</span></span> <span data-ttu-id="24bcc-154">Ez a sablon hello cím és az alhálózati tulajdonságokat használja a VNet létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="24bcc-154">This template uses hello address space and subnet properties during VNet creation.</span></span>
7. <span data-ttu-id="24bcc-155">Lépjen vissza túl[hello mintasablon lapot](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span><span class="sxs-lookup"><span data-stu-id="24bcc-155">Navigate back too[hello sample template page](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span></span>
8. <span data-ttu-id="24bcc-156">Kattintson az **azuredeploy-paremeters.json**, majd a **RAW** elemre.</span><span class="sxs-lookup"><span data-stu-id="24bcc-156">Click **azuredeploy-paremeters.json**, and then click **RAW**.</span></span>
9. <span data-ttu-id="24bcc-157">Mentés hello fájl tooa egy helyi mappába a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="24bcc-157">Save hello file tooa a local folder on your computer.</span></span>
10. <span data-ttu-id="24bcc-158">Nyissa meg a hello előbb mentett fájlt, és hello értékeinek hello paraméterek szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="24bcc-158">Open hello file you just saved and edit hello values for hello parameters.</span></span> <span data-ttu-id="24bcc-159">A következő értékek toodeploy hello hello forgatókönyvben bemutatott VNet alá hello használata:</span><span class="sxs-lookup"><span data-stu-id="24bcc-159">Use hello following values below toodeploy hello VNet described in hello scenario:</span></span>

    ```json
        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
    ```

11. <span data-ttu-id="24bcc-160">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="24bcc-160">Save hello file.</span></span>


## <a name="deploy-hello-template-using-powershell"></a><span data-ttu-id="24bcc-161">PowerShell-lel hello sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="24bcc-161">Deploy hello template using PowerShell</span></span>

<span data-ttu-id="24bcc-162">Hajtsa végre a következő lépéseket a PowerShell használatával a letöltött toodeploy hello sablon hello:</span><span class="sxs-lookup"><span data-stu-id="24bcc-162">Complete hello following steps toodeploy hello template you downloaded by using PowerShell:</span></span>

1. <span data-ttu-id="24bcc-163">Telepítse és konfigurálja az Azure PowerShell hello lépések végrehajtásával a hello [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="24bcc-163">Install and configure Azure PowerShell by completing hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="24bcc-164">Futtassa a következő parancs toocreate hello új erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="24bcc-164">Run hello following command toocreate a new resource group:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    <span data-ttu-id="24bcc-165">hello parancs létrehoz egy erőforráscsoportot *TestRG* a hello *USA középső RÉGIÓJA* azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="24bcc-165">hello command creates a resource group named *TestRG* in hello *Central US* azure region.</span></span> <span data-ttu-id="24bcc-166">További információ az erőforráscsoportokkal kapcsolatban: [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md) (Az Azure Resource Manager áttekintése).</span><span class="sxs-lookup"><span data-stu-id="24bcc-166">For more information about resource groups, visit [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    <span data-ttu-id="24bcc-167">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="24bcc-167">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/[Id]/resourceGroups/TestRG

3. <span data-ttu-id="24bcc-168">Futtassa a következő parancs toodeploy hello hello sablonnal és paraméterfájlokkal fájlokkal, fent letöltött és módosított új virtuális hálózat hello:</span><span class="sxs-lookup"><span data-stu-id="24bcc-168">Run hello following command toodeploy hello new VNet using hello template and parameter files you downloaded and modified above:</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

    <span data-ttu-id="24bcc-169">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="24bcc-169">Expected output:</span></span>
   
        DeploymentName    : TestVNetDeployment
        ResourceGroupName : TestRG
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            location         String                     Central US
                            vnetName         String                     TestVNet
                            addressPrefix    String                     192.168.0.0/16
                            subnet1Prefix    String                     192.168.1.0/24
                            subnet1Name      String                     FrontEnd
                            subnet2Prefix    String                     192.168.2.0/24
                            subnet2Name      String                     BackEnd
   
        Outputs           :
4. <span data-ttu-id="24bcc-170">Futtatási hello következő parancsot a tooview hello tulajdonságainak hello új virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="24bcc-170">Run hello following command tooview hello properties of hello new VNet:</span></span>

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

    <span data-ttu-id="24bcc-171">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="24bcc-171">Expected output:</span></span>

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="deploy-hello-template-using-click-to-deploy"></a><span data-ttu-id="24bcc-172">Kattintson a központi telepítése segítségével hello sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="24bcc-172">Deploy hello template using click-to-deploy</span></span>

<span data-ttu-id="24bcc-173">Előre definiált Azure Resource Manager sablonok feltöltött tooa GitHub-tárházban tartja fenn a Microsoft és a közösségi Megnyitás toohello is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="24bcc-173">You can reuse pre-defined Azure Resource Manager templates uploaded tooa GitHub repository maintained by Microsoft and open toohello community.</span></span> <span data-ttu-id="24bcc-174">Ezeket a sablonokat is telepíthető közvetlenül a github webhelyen, kívül, vagy letöltött, és módosított toofit igényeinek.</span><span class="sxs-lookup"><span data-stu-id="24bcc-174">These templates can be deployed straight out of GitHub, or downloaded and modified toofit your needs.</span></span> <span data-ttu-id="24bcc-175">toodeploy a sablont, amely hoz létre egy Vnetet két alhálózattal, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="24bcc-175">toodeploy a template that creates a VNet with two subnets, complete hello following steps:</span></span>

1. <span data-ttu-id="24bcc-176">Egy böngészőből keresse túl[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="24bcc-176">From a browser, navigate too[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span>
2. <span data-ttu-id="24bcc-177">Görgessen lefelé hello sablonok listáján, majd kattintson a **101-vnet-two-subnets**.</span><span class="sxs-lookup"><span data-stu-id="24bcc-177">Scroll down hello list of templates, and click **101-vnet-two-subnets**.</span></span> <span data-ttu-id="24bcc-178">Ellenőrizze a hello **README.md** fájlt a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="24bcc-178">Check hello **README.md** file, as shown below.</span></span>

    ![A README.md fájl a GitHubban](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. <span data-ttu-id="24bcc-180">Kattintson a **tooAzure telepítése**.</span><span class="sxs-lookup"><span data-stu-id="24bcc-180">Click **Deploy tooAzure**.</span></span> <span data-ttu-id="24bcc-181">Szükség esetén adja meg az Azure bejelentkezési hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="24bcc-181">If necessary, enter your Azure login credentials.</span></span> 
4. <span data-ttu-id="24bcc-182">A hello **paraméterek** panelen adja meg a hello értékek toouse toocreate szeretné, hogy az új Vnetet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="24bcc-182">In hello **Parameters** blade, enter hello values you want toouse toocreate your new VNet, and then click **OK**.</span></span> <span data-ttu-id="24bcc-183">a következő ábra azt mutatja be hello értékek hello forgatókönyvhöz hello:</span><span class="sxs-lookup"><span data-stu-id="24bcc-183">hello following figure shows hello values for hello scenario:</span></span>
   
    ![Az ARM-sablon paraméterei](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

5. <span data-ttu-id="24bcc-185">Kattintson a **erőforráscsoport** , és válassza a erőforrás csoport tooadd hello VNet, vagy kattintson a **hozzon létre új** tooadd hello VNet tooa új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="24bcc-185">Click **Resource group** and select a resource group tooadd hello VNet to, or click **Create new** tooadd hello VNet tooa new resource group.</span></span> <span data-ttu-id="24bcc-186">hello következő ábrán látható hello erőforrás nevű új erőforráscsoport tartozó beállítások **TestRG**:</span><span class="sxs-lookup"><span data-stu-id="24bcc-186">hello following figure shows hello resource group settings for a new resource group called **TestRG**:</span></span>

    ![Erőforráscsoport](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

6. <span data-ttu-id="24bcc-188">Szükség esetén módosítsa a hello **előfizetés** és **hely** a VNet beállításait.</span><span class="sxs-lookup"><span data-stu-id="24bcc-188">If necessary, change hello **Subscription** and **Location** settings for your VNet.</span></span>
7. <span data-ttu-id="24bcc-189">Ha nem szeretné, hogy toosee hello VNet csempe megjelenjen hello **Kezdőpulton**, tiltsa le a **PIN-kód tooStartboard**.</span><span class="sxs-lookup"><span data-stu-id="24bcc-189">If you do not want toosee hello VNet as a tile in hello **Startboard**, disable **Pin tooStartboard**.</span></span>
8. <span data-ttu-id="24bcc-190">Kattintson a **jogi feltételeket**, olvassa el hello feltételeit, és kattintson **megvásárlása** tooagree.</span><span class="sxs-lookup"><span data-stu-id="24bcc-190">Click **Legal terms**, read hello terms, and click **Buy** tooagree.</span></span> 
9. <span data-ttu-id="24bcc-191">Kattintson a **létrehozása** toocreate hello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="24bcc-191">Click **Create** toocreate hello VNet.</span></span>
   
    ![Üzembe helyezési csempe küldése a betekintő portálban](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

10. <span data-ttu-id="24bcc-193">Hello központi telepítés befejezése után a hello Azure-portálon kattintson **további szolgáltatások**, típus *virtuális hálózatok* hello szűrő mezőbe, amely akkor jelenik meg, kattintson a virtuális hálózatok toosee hello virtuális hálózatok panelen.</span><span class="sxs-lookup"><span data-stu-id="24bcc-193">Once hello deployment is complete, in hello Azure portal click **More services**, type *virtual networks* in hello filter box that appears, then click Virtual networks toosee hello Virtual networks blade.</span></span> <span data-ttu-id="24bcc-194">Hello paneljén kattintson *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="24bcc-194">In hello blade, click *TestVNet*.</span></span> <span data-ttu-id="24bcc-195">A hello *TestVNet* panelen kattintson a **alhálózatok** toosee létrehozott hello alhálózatok, ahogy az alábbi képen hello:</span><span class="sxs-lookup"><span data-stu-id="24bcc-195">In hello *TestVNet* blade, click **Subnets** toosee hello created subnets, as shown in hello following picture:</span></span>
    
     ![VNet létrehozása a betekintő portálon](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.png)

## <a name="next-steps"></a><span data-ttu-id="24bcc-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24bcc-197">Next steps</span></span>

<span data-ttu-id="24bcc-198">Megtudhatja, hogyan tooconnect:</span><span class="sxs-lookup"><span data-stu-id="24bcc-198">Learn how tooconnect:</span></span>

- <span data-ttu-id="24bcc-199">A virtuális gép (VM) tooa virtuális hálózatot, olvasási hello [Windows virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md) vagy [hozzon létre egy Linux virtuális Gépet](../virtual-machines/linux/quick-create-portal.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="24bcc-199">A virtual machine (VM) tooa virtual network by reading hello [Create a Windows VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) or [Create a Linux VM](../virtual-machines/linux/quick-create-portal.md) articles.</span></span> <span data-ttu-id="24bcc-200">Egy VNet és alhálózat létrehozása hello cikkek hello lépésekben, helyett választhatja egy meglévő VNet és alhálózat tooconnect egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="24bcc-200">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="24bcc-201">virtuális hálózat tooother virtuális hálózatok hello hello olvasásával [csatlakozás Vnetek](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="24bcc-201">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) article.</span></span>
- <span data-ttu-id="24bcc-202">hello virtuális hálózati tooan a helyi hálózati helyek virtuális magánhálózati (VPN) vagy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="24bcc-202">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="24bcc-203">Megtudhatja, hogyan hello olvasásával [VNet tooan a helyi hálózat a telephelyek közötti VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) és [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](../expressroute/expressroute-howto-linkvnet-arm.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="24bcc-203">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) articles.</span></span>
