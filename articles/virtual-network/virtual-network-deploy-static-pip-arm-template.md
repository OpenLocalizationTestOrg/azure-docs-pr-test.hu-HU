---
title: "Hozzon létre egy virtuális Gépet egy statikus nyilvános IP-cím - Azure Resource Manager-sablon |} Microsoft Docs"
description: "Útmutató: virtuális gép létrehozása az Azure Resource Manager-sablonnal statikus nyilvános IP-cím."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d551085a-c7ed-4ec6-b4c3-e9e1cebb774c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2f503aa60fdd9b7cf66ef482a1041e34c88e5c01
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a><span data-ttu-id="86bb8-103">Virtuális gép létrehozása az Azure Resource Manager-sablonnal statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="86bb8-103">Create a VM with a static public IP address using an Azure Resource Manager template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="86bb8-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="86bb8-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="86bb8-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="86bb8-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="86bb8-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="86bb8-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="86bb8-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="86bb8-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="86bb8-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="86bb8-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="86bb8-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="86bb8-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="86bb8-110">Ez a cikk a Microsoft azt javasolja, hogy a klasszikus üzembe helyezési modellel helyett az új telepítések esetén a Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="86bb8-110">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a><span data-ttu-id="86bb8-111">Nyilvános IP-cím erőforrás egy sablonfájlban</span><span class="sxs-lookup"><span data-stu-id="86bb8-111">Public IP address resources in a template file</span></span>
<span data-ttu-id="86bb8-112">Tekintheti meg és töltse le a [mintasablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="86bb8-112">You can view and download the [sample template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).</span></span>

<span data-ttu-id="86bb8-113">A következő szakasz meghatározása a nyilvános IP-erőforrás, a fenti forgatókönyv alapján jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="86bb8-113">The following section shows the definition of the public IP resource, based on the scenario above:</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('webVMSetting').pipName]",
  "location": "[variables('location')]",
  "properties": {
    "publicIPAllocationMethod": "Static"
  },
  "tags": {
    "displayName": "PublicIPAddress - Web"
  }
},
```

<span data-ttu-id="86bb8-114">Figyelje meg a **publicIPAllocationMethod** tulajdonság, melynek értéke *statikus*.</span><span class="sxs-lookup"><span data-stu-id="86bb8-114">Notice the **publicIPAllocationMethod** property, which is set to *Static*.</span></span> <span data-ttu-id="86bb8-115">Ez a tulajdonság lehet *dinamikus* (alapértelmezett érték) vagy *statikus*.</span><span class="sxs-lookup"><span data-stu-id="86bb8-115">This property can be either *Dynamic* (default value) or *Static*.</span></span> <span data-ttu-id="86bb8-116">Értékre állítaná statikus biztosítja, hogy az a nyilvános IP-cím lesz soha ne módosuljanak.</span><span class="sxs-lookup"><span data-stu-id="86bb8-116">Setting it to static guarantees that the public IP address assigned will never change.</span></span>

<span data-ttu-id="86bb8-117">A következő szakasz jeleníti meg egy hálózati adapter társítását az nyilvános IP-cím:</span><span class="sxs-lookup"><span data-stu-id="86bb8-117">The following section shows the association of the public IP address with a network interface:</span></span>

```json
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('webVMSetting').nicName]",
    "location": "[variables('location')]",
    "tags": {
    "displayName": "NetworkInterface - Web"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
      "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
          "privateIPAllocationMethod": "Static",
          "privateIPAddress": "[variables('webVMSetting').ipAddress]",
          "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
          },
          "subnet": {
            "id": "[variables('frontEndSubnetRef')]"
          }
        }
      }
    ]
  }
},
```

<span data-ttu-id="86bb8-118">Figyelje meg a **publicIPAddress** mutató tulajdonság a **azonosító** nevű erőforrás **variables('webVMSetting').pipName**.</span><span class="sxs-lookup"><span data-stu-id="86bb8-118">Notice the **publicIPAddress** property pointing to the **Id** of a resource named **variables('webVMSetting').pipName**.</span></span> <span data-ttu-id="86bb8-119">Ez a fenti nyilvános IP-erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="86bb8-119">That is the name of the public IP resource shown above.</span></span>

<span data-ttu-id="86bb8-120">Végezetül, a fenti hálózati adapter szerepel-e a **networkProfile** tulajdonság a virtuális gép létrehozása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="86bb8-120">Finally, the network interface above is listed in the **networkProfile** property of the VM being created.</span></span>

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-the-template-by-using-click-to-deploy"></a><span data-ttu-id="86bb8-121">A sablon üzembe helyezése kattintással végrehajtható üzembe helyezéssel</span><span class="sxs-lookup"><span data-stu-id="86bb8-121">Deploy the template by using click to deploy</span></span>

<span data-ttu-id="86bb8-122">A nyilvános tárházban elérhető mintasablon a fent leírt forgatókönyv létrehozásához használt alapértelmezett értékeket tartalmazó paraméterfájlt használja.</span><span class="sxs-lookup"><span data-stu-id="86bb8-122">The sample template available in the public repository uses a parameter file containing the default values used to generate the scenario described above.</span></span> <span data-ttu-id="86bb8-123">Ez a sablon üzembe helyezése kattintással végrehajtható telepítése telepítéséhez kattintson **az Azure telepítéséhez** Readme.md fájljában a [statikus PIP rendelkező virtuális gépet](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) sablont.</span><span class="sxs-lookup"><span data-stu-id="86bb8-123">To deploy this template using click to deploy, click **Deploy to Azure** in the Readme.md file for the [VM with static PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) template.</span></span> <span data-ttu-id="86bb8-124">Cserélje le az alapértelmezett paraméterértékek, ha szükséges, és adja meg az üres paraméterek értékeit.</span><span class="sxs-lookup"><span data-stu-id="86bb8-124">Replace the default parameter values if desired and enter values for the blank parameters.</span></span>  <span data-ttu-id="86bb8-125">Kövesse az utasításokat a portálon hozzon létre egy virtuális gépet egy statikus nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="86bb8-125">Follow the instructions in the portal to create a virtual machine with a static public IP address.</span></span>

## <a name="deploy-the-template-by-using-powershell"></a><span data-ttu-id="86bb8-126">A sablon üzembe helyezése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="86bb8-126">Deploy the template by using PowerShell</span></span>

<span data-ttu-id="86bb8-127">A letöltött sablon PowerShell használatával történő üzembe helyezéséhez kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="86bb8-127">To deploy the template you downloaded by using PowerShell, follow the steps below.</span></span>

1. <span data-ttu-id="86bb8-128">Ha még sosem használta az Azure PowerShell, hajtsa végre a a [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="86bb8-128">If you have never used Azure PowerShell, complete the steps in the [How to Install and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="86bb8-129">A PowerShell-konzolban, futtassa a `New-AzureRmResourceGroup` parancsmag segítségével hozzon létre egy új erőforráscsoportot, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="86bb8-129">In a PowerShell console, run the `New-AzureRmResourceGroup` cmdlet to create a new resource group, if necessary.</span></span> <span data-ttu-id="86bb8-130">Ha már létrehozott egy erőforráscsoport, folytassa a 3.</span><span class="sxs-lookup"><span data-stu-id="86bb8-130">If you already have a resource group created, go to step 3.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    <span data-ttu-id="86bb8-131">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="86bb8-131">Expected output:</span></span>
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. <span data-ttu-id="86bb8-132">A PowerShell-konzolban, futtassa a `New-AzureRmResourceGroupDeployment` parancsmagot, hogy a sablon telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="86bb8-132">In a PowerShell console, run the `New-AzureRmResourceGroupDeployment` cmdlet to deploy the template.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    <span data-ttu-id="86bb8-133">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="86bb8-133">Expected output:</span></span>
   
        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0
   
        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         
   
        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a><span data-ttu-id="86bb8-134">A sablon üzembe helyezése az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="86bb8-134">Deploy the template by using the Azure CLI</span></span>
<span data-ttu-id="86bb8-135">A sablon telepítéséhez az Azure parancssori felület használatával, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="86bb8-135">To deploy the template by using the Azure CLI, complete the following steps:</span></span>

1. <span data-ttu-id="86bb8-136">Ha még sosem használta az Azure parancssori felület, kövesse a lépéseket a [telepítése és konfigurálása az Azure parancssori felület](../cli-install-nodejs.md) cikk telepítéséhez és konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="86bb8-136">If you have never used Azure CLI, follow the steps in the [Install and Configure the Azure CLI](../cli-install-nodejs.md) article to install and configure it.</span></span>
2. <span data-ttu-id="86bb8-137">Futtassa a `azure config mode` parancs futtatásával váltson Resource Manager módra a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="86bb8-137">Run the `azure config mode` command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="86bb8-138">A fenti parancs várható kimenete:</span><span class="sxs-lookup"><span data-stu-id="86bb8-138">The expected output for the command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="86bb8-139">Nyissa meg a [paraméterfájl](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), válassza ki annak tartalmát, és mentse a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="86bb8-139">Open the [parameter file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), select its content, and save it to a file in your computer.</span></span> <span data-ttu-id="86bb8-140">Az ebben a példában a paraméterek nevű fájlba menti *parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="86bb8-140">For this example, the parameters are saved to a file named *parameters.json*.</span></span> <span data-ttu-id="86bb8-141">Módosítsa a paraméterértékeket a fájlban, ha szükséges, de legalább javasoljuk, hogy egy egyedi, összetett jelszót módosítja a adminPassword paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="86bb8-141">Change the parameter values within the file if desired, but at a minimum, it's recommended that you change the value for the adminPassword parameter to a unique, complex password.</span></span>
4. <span data-ttu-id="86bb8-142">Futtassa a `azure group deployment create` cmd telepíteni az új VNet sablonnal és paraméterfájlokkal meg a fent letöltött és módosított fájlok.</span><span class="sxs-lookup"><span data-stu-id="86bb8-142">Run the `azure group deployment create` cmd to deploy the new VNet by using the template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="86bb8-143">Az alábbi parancsban cserélje le <path> az elérési úttal rendelkező a fájlt mentette.</span><span class="sxs-lookup"><span data-stu-id="86bb8-143">In the command below, replace <path> with the path you saved the file to.</span></span> 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    <span data-ttu-id="86bb8-144">Várt kimenet (használt paraméterértékeket ismerteti):</span><span class="sxs-lookup"><span data-stu-id="86bb8-144">Expected output (lists parameter values used):</span></span>

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

