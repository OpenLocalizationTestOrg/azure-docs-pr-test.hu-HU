---
title: "hálózati biztonsági csoport – Azure Resource Manager-sablon aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és központi telepítése a hálózati biztonsági csoportok Azure Resource Manager-sablonnal."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: f3e7385d-717c-44ff-be20-f9aa450aa99b
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3750168284fea7b41c8c0f908b0d31a9da5e38ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-an-azure-resource-manager-template"></a><span data-ttu-id="a8e1e-103">Hálózati biztonsági csoportok használata az Azure Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="a8e1e-103">Create network security groups using an Azure Resource Manager template</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="a8e1e-104">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-104">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="a8e1e-105">Emellett [NSG-k létrehozása hello klasszikus üzembe helyezési modellel](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a8e1e-105">You can also [create NSGs in hello classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a><span data-ttu-id="a8e1e-106">Egy sablonfájlban NSG-erőforrások</span><span class="sxs-lookup"><span data-stu-id="a8e1e-106">NSG resources in a template file</span></span>
<span data-ttu-id="a8e1e-107">Tekintheti meg és töltse le a hello [mintasablon](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).</span><span class="sxs-lookup"><span data-stu-id="a8e1e-107">You can view and download hello [sample template](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).</span></span>

<span data-ttu-id="a8e1e-108">hello következő szakasz azt mutatja be hello hello definíciója előtér-NSG hello forgatókönyv alapján.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-108">hello following section shows hello definition of hello front-end NSG, based on hello scenario.</span></span>

```json
"apiVersion": "2015-06-15",
"type": "Microsoft.Network/networkSecurityGroups",
"name": "[parameters('frontEndNSGName')]",
"location": "[resourceGroup().location]",
"tags": {
  "displayName": "NSG - Front End"
},
"properties": {
  "securityRules": [
    {
      "name": "rdp-rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 100,
        "direction": "Inbound"
      }
    },
    {
      "name": "web-rule",
      "properties": {
        "description": "Allow WEB",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 101,
        "direction": "Inbound"
      }
    }
  ]
}
```
<span data-ttu-id="a8e1e-109">tooassociate hello NSG toohello előtér-alhálózathoz, hogy a toochange hello alhálózati definíció hello sablonban és hello-hivatkozás esetén használja a hello NSG.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-109">tooassociate hello NSG toohello front-end subnet, you have toochange hello subnet definition in hello template, and use hello reference id for hello NSG.</span></span>

```json
"subnets": [
  {
    "name": "[parameters('frontEndSubnetName')]",
    "properties": {
      "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
      "networkSecurityGroup": {
      "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
      }
    }
  }, 
```

<span data-ttu-id="a8e1e-110">Figyelje meg, hello azonos hello háttér-NSG és hello háttér-alhálózat hello sablonban számára történik.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-110">Notice hello same being done for hello back-end NSG and hello back-end subnet in hello template.</span></span>

## <a name="deploy-hello-arm-template-by-using-click-toodeploy"></a><span data-ttu-id="a8e1e-111">Központi telepítése hello ARM-sablon használatával toodeploy kattintson</span><span class="sxs-lookup"><span data-stu-id="a8e1e-111">Deploy hello ARM template by using click toodeploy</span></span>
<span data-ttu-id="a8e1e-112">hello mintasablon elérhető hello nyilvános tárházban hello alapértelmezett értékeket használja toogenerate hello forgatókönyv a fent leírt tartalmazó paraméter fájlt használ.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-112">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="a8e1e-113">toodeploy toodeploy, a sablon használatával kattintson kövesse [erre a hivatkozásra](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek szükség és hello portal hello utasításait kövesse.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-113">toodeploy this template using click toodeploy, follow [this link](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

## <a name="deploy-hello-arm-template-by-using-powershell"></a><span data-ttu-id="a8e1e-114">Telepítését hello ARM-sablon PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a8e1e-114">Deploy hello ARM template by using PowerShell</span></span>
<span data-ttu-id="a8e1e-115">toodeploy hello letöltött ARM-sablon PowerShell használatával hajtsa végre a hello alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-115">toodeploy hello ARM template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="a8e1e-116">Ha még sosem használta az Azure PowerShell, kövesse a hello hello utasításait [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) tooinstall és konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-116">If you have never used Azure PowerShell, follow hello instructions in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) tooinstall and configure it.</span></span>
2. <span data-ttu-id="a8e1e-117">Futtassa a hello  **`New-AzureRmResourceGroup`**  erőforrás csoport használatával parancsmag toocreate hello sablont.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-117">Run hello **`New-AzureRmResourceGroup`** cmdlet toocreate a resource group using hello template.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location uswest `
    -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
    -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    <span data-ttu-id="a8e1e-118">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="a8e1e-118">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  
   
        Resources         :
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            sqlAvSet            Microsoft.Compute/availabilitySets       westus  
                            webAvSet            Microsoft.Compute/availabilitySets       westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            Web1                Microsoft.Compute/virtualMachines        westus  
                            Web2                Microsoft.Compute/virtualMachines        westus  
                            TestNICSQL1         Microsoft.Network/networkInterfaces      westus  
                            TestNICSQL2         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb1         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb2         Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            TestPIPSQL1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPSQL2         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb2         Microsoft.Network/publicIPAddresses      westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  
   
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG

## <a name="deploy-hello-arm-template-by-using-hello-azure-cli"></a><span data-ttu-id="a8e1e-119">Hello ARM-sablon telepítése hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="a8e1e-119">Deploy hello ARM template by using hello Azure CLI</span></span>
<span data-ttu-id="a8e1e-120">toodeploy hello ARM-sablon hello Azure parancssori felület használatával kövesse hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-120">toodeploy hello ARM template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="a8e1e-121">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-121">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="a8e1e-122">Futtassa a hello  **`azure config mode`**  tooswitch tooResource Manager üzemmód, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-122">Run hello **`azure config mode`** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="a8e1e-123">hello az alábbiakban látható hello parancs kimenetét várt hello:</span><span class="sxs-lookup"><span data-stu-id="a8e1e-123">hello following is hello expected output for hello command:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="a8e1e-124">Futtassa a hello  **`azure group deployment create`**  parancsmag toodeploy hello hello sablonnal és paraméterfájlokkal segítségével új virtuális hálózat fájlokat fent letöltött és módosított.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-124">Run hello **`azure group deployment create`** cmdlet toodeploy hello new VNet by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="a8e1e-125">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-125">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    <span data-ttu-id="a8e1e-126">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="a8e1e-126">Expected output:</span></span>
   
        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Creating resource group TestRG
        info:    Created resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK
   
   * <span data-ttu-id="a8e1e-127">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-127">**-n (or --name)**.</span></span> <span data-ttu-id="a8e1e-128">Hello erőforrás csoport toobe létrehozott neve.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-128">Name of hello resource group toobe created.</span></span>
   * <span data-ttu-id="a8e1e-129">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-129">**-l (or --location)**.</span></span> <span data-ttu-id="a8e1e-130">Azure-régió, ahol hello erőforráscsoport létrejön.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-130">Azure region where hello resource group will be created.</span></span>
   * <span data-ttu-id="a8e1e-131">**-f (vagy --template-file)**.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-131">**-f (or --template-file)**.</span></span> <span data-ttu-id="a8e1e-132">Elérési út tooyour ARM-sablonfájl.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-132">Path tooyour ARM template file.</span></span>
   * <span data-ttu-id="a8e1e-133">**-e (vagy --parameters-file)**.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-133">**-e (or --parameters-file)**.</span></span> <span data-ttu-id="a8e1e-134">Elérési út tooyour ARM-paraméterfájl.</span><span class="sxs-lookup"><span data-stu-id="a8e1e-134">Path tooyour ARM parameters file.</span></span>

