---
title: "a virtuális gép statikus nyilvános IP-címmel – Azure Resource Manager sablon aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy virtuális Gépet egy statikus nyilvános IP-cím, egy Azure Resource Manager-sablon használatával."
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
ms.openlocfilehash: 6a8640ed4fad06b0e09820e6114fd6789db73847
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a><span data-ttu-id="7de52-103">Virtuális gép létrehozása az Azure Resource Manager-sablonnal statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="7de52-103">Create a VM with a static public IP address using an Azure Resource Manager template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7de52-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7de52-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="7de52-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7de52-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="7de52-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7de52-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="7de52-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="7de52-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="7de52-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="7de52-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="7de52-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7de52-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="7de52-110">Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="7de52-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a><span data-ttu-id="7de52-111">Nyilvános IP-cím erőforrás egy sablonfájlban</span><span class="sxs-lookup"><span data-stu-id="7de52-111">Public IP address resources in a template file</span></span>
<span data-ttu-id="7de52-112">Tekintheti meg és töltse le a hello [mintasablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="7de52-112">You can view and download hello [sample template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).</span></span>

<span data-ttu-id="7de52-113">hello következő szakasz bemutatja hello nyilvános IP-erőforrás, a fenti hello forgatókönyv alapján hello definíciója:</span><span class="sxs-lookup"><span data-stu-id="7de52-113">hello following section shows hello definition of hello public IP resource, based on hello scenario above:</span></span>

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

<span data-ttu-id="7de52-114">Értesítés hello **publicIPAllocationMethod** tulajdonságot, amelynek értéke túl*statikus*.</span><span class="sxs-lookup"><span data-stu-id="7de52-114">Notice hello **publicIPAllocationMethod** property, which is set too*Static*.</span></span> <span data-ttu-id="7de52-115">Ez a tulajdonság lehet *dinamikus* (alapértelmezett érték) vagy *statikus*.</span><span class="sxs-lookup"><span data-stu-id="7de52-115">This property can be either *Dynamic* (default value) or *Static*.</span></span> <span data-ttu-id="7de52-116">A beállítás azt toostatic biztosítja, hogy az hello nyilvános IP-cím lesz soha ne módosuljanak.</span><span class="sxs-lookup"><span data-stu-id="7de52-116">Setting it toostatic guarantees that hello public IP address assigned will never change.</span></span>

<span data-ttu-id="7de52-117">hello következő szakasz bemutatja egy adott hálózati csatoló hello nyilvános IP-cím hello társítást:</span><span class="sxs-lookup"><span data-stu-id="7de52-117">hello following section shows hello association of hello public IP address with a network interface:</span></span>

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

<span data-ttu-id="7de52-118">Értesítés hello **publicIPAddress** toohello mutató tulajdonság **azonosító** nevű erőforrás **variables('webVMSetting').pipName**.</span><span class="sxs-lookup"><span data-stu-id="7de52-118">Notice hello **publicIPAddress** property pointing toohello **Id** of a resource named **variables('webVMSetting').pipName**.</span></span> <span data-ttu-id="7de52-119">Ez az hello hello nyilvános IP-erőforrás nevét fent látható.</span><span class="sxs-lookup"><span data-stu-id="7de52-119">That is hello name of hello public IP resource shown above.</span></span>

<span data-ttu-id="7de52-120">Végezetül, a fenti hello hálózati illesztő szerepel hello **networkProfile** tulajdonsága hello virtuális gép létrehozása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="7de52-120">Finally, hello network interface above is listed in hello **networkProfile** property of hello VM being created.</span></span>

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-hello-template-by-using-click-toodeploy"></a><span data-ttu-id="7de52-121">Hello sablon üzembe helyezése a toodeploy kattintson</span><span class="sxs-lookup"><span data-stu-id="7de52-121">Deploy hello template by using click toodeploy</span></span>

<span data-ttu-id="7de52-122">hello mintasablon elérhető hello nyilvános tárházban hello alapértelmezett értékeket használja toogenerate hello forgatókönyv a fent leírt tartalmazó paraméter fájlt használ.</span><span class="sxs-lookup"><span data-stu-id="7de52-122">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="7de52-123">toodeploy toodeploy, a sablon használatával kattintson kattintson **tooAzure telepítése** hello Readme.md fájl hello [statikus PIP rendelkező virtuális gépet](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) sablont.</span><span class="sxs-lookup"><span data-stu-id="7de52-123">toodeploy this template using click toodeploy, click **Deploy tooAzure** in hello Readme.md file for hello [VM with static PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) template.</span></span> <span data-ttu-id="7de52-124">Cserélje le a hello alapértelmezett paraméterértékek, ha szükséges, és adja meg a hello üres paraméterek.</span><span class="sxs-lookup"><span data-stu-id="7de52-124">Replace hello default parameter values if desired and enter values for hello blank parameters.</span></span>  <span data-ttu-id="7de52-125">Kövesse a hello utasításait hello portál toocreate egy virtuális gép egy statikus nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="7de52-125">Follow hello instructions in hello portal toocreate a virtual machine with a static public IP address.</span></span>

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="7de52-126">Hello sablon üzembe helyezése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7de52-126">Deploy hello template by using PowerShell</span></span>

<span data-ttu-id="7de52-127">toodeploy hello sablon PowerShell használatával hajtsa végre az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="7de52-127">toodeploy hello template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="7de52-128">Ha még sosem használta az Azure PowerShell, teljes hello hello szükséges lépések [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="7de52-128">If you have never used Azure PowerShell, complete hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="7de52-129">PowerShell-konzolban, futtassa a hello `New-AzureRmResourceGroup` parancsmag toocreate egy új erőforráscsoportot, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="7de52-129">In a PowerShell console, run hello `New-AzureRmResourceGroup` cmdlet toocreate a new resource group, if necessary.</span></span> <span data-ttu-id="7de52-130">Ha már létrehozott egy erőforráscsoport, folytassa a toostep 3.</span><span class="sxs-lookup"><span data-stu-id="7de52-130">If you already have a resource group created, go toostep 3.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    <span data-ttu-id="7de52-131">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="7de52-131">Expected output:</span></span>
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. <span data-ttu-id="7de52-132">PowerShell-konzolban, futtassa a hello `New-AzureRmResourceGroupDeployment` parancsmag toodeploy hello sablont.</span><span class="sxs-lookup"><span data-stu-id="7de52-132">In a PowerShell console, run hello `New-AzureRmResourceGroupDeployment` cmdlet toodeploy hello template.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    <span data-ttu-id="7de52-133">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="7de52-133">Expected output:</span></span>
   
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

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="7de52-134">Hello sablon üzembe helyezése hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="7de52-134">Deploy hello template by using hello Azure CLI</span></span>
<span data-ttu-id="7de52-135">toodeploy hello sablon hello Azure CLI, teljes hello lépések segítségével:</span><span class="sxs-lookup"><span data-stu-id="7de52-135">toodeploy hello template by using hello Azure CLI, complete hello following steps:</span></span>

1. <span data-ttu-id="7de52-136">Ha még sosem használta az Azure parancssori felület, kövesse hello hello [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) tooinstall a következő cikket, és állítsa be.</span><span class="sxs-lookup"><span data-stu-id="7de52-136">If you have never used Azure CLI, follow hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md) article tooinstall and configure it.</span></span>
2. <span data-ttu-id="7de52-137">Futtassa a hello `azure config mode` tooswitch tooResource Manager üzemmód, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="7de52-137">Run hello `azure config mode` command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="7de52-138">hello várt fenti hello parancs kimenetét:</span><span class="sxs-lookup"><span data-stu-id="7de52-138">hello expected output for hello command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="7de52-139">Nyissa meg hello [paraméterfájl](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), válassza ki annak tartalmát, és mentse azokat a számítógép tooa fájlt.</span><span class="sxs-lookup"><span data-stu-id="7de52-139">Open hello [parameter file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), select its content, and save it tooa file in your computer.</span></span> <span data-ttu-id="7de52-140">Ehhez a példához hello paraméterek nevű tooa fájl mentett *parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="7de52-140">For this example, hello parameters are saved tooa file named *parameters.json*.</span></span> <span data-ttu-id="7de52-141">Hello paraméterértékek hello fájlban szükség esetén módosítsa, de legalább, javasoljuk, hogy megváltoztatja hello hello adminPassword paraméter tooa egyedi, összetett jelszót.</span><span class="sxs-lookup"><span data-stu-id="7de52-141">Change hello parameter values within hello file if desired, but at a minimum, it's recommended that you change hello value for hello adminPassword parameter tooa unique, complex password.</span></span>
4. <span data-ttu-id="7de52-142">Futtassa a hello `azure group deployment create` cmd toodeploy hello hello sablonnal és paraméterfájlokkal segítségével új virtuális hálózat fájlokat fent letöltött és módosított.</span><span class="sxs-lookup"><span data-stu-id="7de52-142">Run hello `azure group deployment create` cmd toodeploy hello new VNet by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="7de52-143">Az alábbi hello parancs, cserélje le <path> hello elérési úttal rendelkező hello fájlt mentette.</span><span class="sxs-lookup"><span data-stu-id="7de52-143">In hello command below, replace <path> with hello path you saved hello file to.</span></span> 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    <span data-ttu-id="7de52-144">Várt kimenet (használt paraméterértékeket ismerteti):</span><span class="sxs-lookup"><span data-stu-id="7de52-144">Expected output (lists parameter values used):</span></span>

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

