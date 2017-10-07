---
title: "Windows virtuálisgép-méretezési csoportok aaaAutoscale |} Microsoft Docs"
description: "Egy Windows virtuálisgép-méretezési csoportban az Azure PowerShell automatikus skálázás beállítása"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: 67cf1c5063ceba4fc076dc270090defdbddbcfe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="18ee1-103">Automatikus méretezése a gépet egy virtuálisgép-méretezési csoportban lévő</span><span class="sxs-lookup"><span data-stu-id="18ee1-103">Automatically scale machines in a virtual machine scale set</span></span>
<span data-ttu-id="18ee1-104">Virtuálisgép-méretezési csoportok könnyítheti meg toodeploy, és egyetlen egységként azonos virtuális gépek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="18ee1-104">Virtual machine scale sets make it easy for you toodeploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="18ee1-105">Méretezési csoportok hyperscale alkalmazások jól méretezhető és testre szabható számítási rétegét adja meg, és a Windows platform-lemezképek, Linux platformon képek, egyéni lemezképek és bővítmények támogatják.</span><span class="sxs-lookup"><span data-stu-id="18ee1-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="18ee1-106">Méretezési csoportok kapcsolatos további információkért lásd: [virtuálisgép-skálázási készletekben](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="18ee1-106">For more information about scale sets, see [Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="18ee1-107">Az oktatóanyag bemutatja, hogyan toocreate egy méretezési Windows virtuális gép, és automatikusan méretezési hello gépek hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="18ee1-107">This tutorial shows you how toocreate a scale set of Windows virtual machines and automatically scale hello machines in hello set.</span></span> <span data-ttu-id="18ee1-108">Hello méretezési és állítva skálázása az Azure Resource Manager-sablon létrehozása és telepítése az Azure PowerShell használatával hoz létre.</span><span class="sxs-lookup"><span data-stu-id="18ee1-108">You create hello scale set and set up scaling by creating an Azure Resource Manager template and deploying it using Azure PowerShell.</span></span> <span data-ttu-id="18ee1-109">A sablonokról további információkat az [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) (Azure Resource Manager-sablonok készítése) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="18ee1-109">For more information about templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="18ee1-110">toolearn méretezési készlet automatikus skálázás kapcsolatos további információkért lásd: [automatikus skálázás és virtuálisgép-skálázási készletekben](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="18ee1-110">toolearn more about automatic scaling of scale sets, see [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

<span data-ttu-id="18ee1-111">Ebben a cikkben hello következő telepít erőforrások és bővítmények:</span><span class="sxs-lookup"><span data-stu-id="18ee1-111">In this article, you deploy hello following resources and extensions:</span></span>

* <span data-ttu-id="18ee1-112">Microsoft.Storage/storageAccounts</span><span class="sxs-lookup"><span data-stu-id="18ee1-112">Microsoft.Storage/storageAccounts</span></span>
* <span data-ttu-id="18ee1-113">Microsoft.Network/virtualNetworks</span><span class="sxs-lookup"><span data-stu-id="18ee1-113">Microsoft.Network/virtualNetworks</span></span>
* <span data-ttu-id="18ee1-114">Microsoft.Network/publicIPAddresses</span><span class="sxs-lookup"><span data-stu-id="18ee1-114">Microsoft.Network/publicIPAddresses</span></span>
* <span data-ttu-id="18ee1-115">Microsoft.Network/loadBalancers</span><span class="sxs-lookup"><span data-stu-id="18ee1-115">Microsoft.Network/loadBalancers</span></span>
* <span data-ttu-id="18ee1-116">Microsoft.Network/networkInterfaces</span><span class="sxs-lookup"><span data-stu-id="18ee1-116">Microsoft.Network/networkInterfaces</span></span>
* <span data-ttu-id="18ee1-117">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="18ee1-117">Microsoft.Compute/virtualMachines</span></span>
* <span data-ttu-id="18ee1-118">Microsoft.Compute/virtualMachineScaleSets</span><span class="sxs-lookup"><span data-stu-id="18ee1-118">Microsoft.Compute/virtualMachineScaleSets</span></span>
* <span data-ttu-id="18ee1-119">Microsoft.Insights.VMDiagnosticsSettings</span><span class="sxs-lookup"><span data-stu-id="18ee1-119">Microsoft.Insights.VMDiagnosticsSettings</span></span>
* <span data-ttu-id="18ee1-120">Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="18ee1-120">Microsoft.Insights/autoscaleSettings</span></span>

<span data-ttu-id="18ee1-121">Erőforrás-kezelő erőforrásokra vonatkozó további információkért lásd: [Azure Resource Manager és klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="18ee1-121">For more information about Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="step-1-install-azure-powershell"></a><span data-ttu-id="18ee1-122">1. lépés: Az Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="18ee1-122">Step 1: Install Azure PowerShell</span></span>
<span data-ttu-id="18ee1-123">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello Azure PowerShell legújabb verziójának telepítése, az előfizetés kiválasztásával és tooAzure bejelentkezés kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="18ee1-123">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for information about installing hello latest version of Azure PowerShell, selecting your subscription, and signing in tooAzure.</span></span>

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a><span data-ttu-id="18ee1-124">2. lépés: Az erőforráscsoportot és a storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="18ee1-124">Step 2: Create a resource group and a storage account</span></span>
1. <span data-ttu-id="18ee1-125">**Hozzon létre egy erőforráscsoportot** – összes erőforrást erőforráscsoportban telepített tooa kell lennie.</span><span class="sxs-lookup"><span data-stu-id="18ee1-125">**Create a resource group** – All resources must be deployed tooa resource group.</span></span> <span data-ttu-id="18ee1-126">Használjon [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) erőforráscsoport nevű toocreate **vmsstestrg1**.</span><span class="sxs-lookup"><span data-stu-id="18ee1-126">Use [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) toocreate a resource group named **vmsstestrg1**.</span></span>
2. <span data-ttu-id="18ee1-127">**Hozzon létre egy tárfiókot** – Ez a tárfiók hello sablon tárolására.</span><span class="sxs-lookup"><span data-stu-id="18ee1-127">**Create a storage account** – This storage account is where hello template is stored.</span></span> <span data-ttu-id="18ee1-128">Használjon [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) tárfiók nevű toocreate **vmsstestsa**.</span><span class="sxs-lookup"><span data-stu-id="18ee1-128">Use [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) toocreate a storage account named **vmsstestsa**.</span></span>

## <a name="step-3-create-hello-template"></a><span data-ttu-id="18ee1-129">3. lépés: Hello sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="18ee1-129">Step 3: Create hello template</span></span>
<span data-ttu-id="18ee1-130">Az Azure Resource Manager-sablon, toodeploy lehetővé teszi, és Azure-erőforrások együtt kezelheti a JSON-leírásuk hello erőforrások és a társított telepítési paramétereket.</span><span class="sxs-lookup"><span data-stu-id="18ee1-130">An Azure Resource Manager template makes it possible for you toodeploy and manage Azure resources together by using a JSON description of hello resources and associated deployment parameters.</span></span>

1. <span data-ttu-id="18ee1-131">A kedvenc szerkesztő hozzon létre C:\VMSSTemplate.json hello fájlt, és hello kezdeti JSON struktúrában toosupport hello sablon hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="18ee1-131">In your favorite editor, create hello file C:\VMSSTemplate.json and add hello initial JSON structure toosupport hello template.</span></span>

    ```json
    {
      "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      },
      "variables": {
      },
      "resources": [
      ]
    }
    ```

2. <span data-ttu-id="18ee1-132">Paraméterek nem mindig szükséges, de biztosítanak egy módon tooinput értékek hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="18ee1-132">Parameters are not always required, but they provide a way tooinput values when hello template is deployed.</span></span> <span data-ttu-id="18ee1-133">Adja hozzá ezeket a paramétereket, hogy hozzáadta a toohello sablon hello paraméterek szülő elem alatt.</span><span class="sxs-lookup"><span data-stu-id="18ee1-133">Add these parameters under hello parameters parent element that you added toohello template.</span></span>

    ```json   
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
    * <span data-ttu-id="18ee1-134">Hello külön virtuális gépre, amelyik használt tooaccess hello gépek hello méretezési csoportban lévő nevét.</span><span class="sxs-lookup"><span data-stu-id="18ee1-134">A name for hello separate virtual machine that is used tooaccess hello machines in hello scale set.</span></span>
    * <span data-ttu-id="18ee1-135">hello hello sablon tárolására hello tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="18ee1-135">hello name of hello storage account where hello template is stored.</span></span>
    * <span data-ttu-id="18ee1-136">hello méretezési csoportban lévő virtuális gépek tooinitially hello száma hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="18ee1-136">hello number of virtual machines tooinitially create in hello scale set.</span></span>
    * <span data-ttu-id="18ee1-137">hello nevét és jelszavát hello rendszergazdai fiókjának hello virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="18ee1-137">hello name and password of hello administrator account on hello virtual machines.</span></span>
    * <span data-ttu-id="18ee1-138">Állítsa be a nevének előtagját toosupport hello méretezési létrehozott hello erőforrások.</span><span class="sxs-lookup"><span data-stu-id="18ee1-138">A name prefix for hello resources that are created toosupport hello scale set.</span></span>

3. <span data-ttu-id="18ee1-139">A sablon toospecify értékek, amelyek gyakran változhat a változók használhatók, vagy toobe igénylő értékek paraméterértékek kombinációja alapján létrehozott.</span><span class="sxs-lookup"><span data-stu-id="18ee1-139">Variables can be used in a template toospecify values that may change frequently or values that need toobe created from a combination of parameter values.</span></span> <span data-ttu-id="18ee1-140">Adja hozzá ezeket a változókat, hogy hozzáadta a toohello sablon hello változók szülő elem alatt.</span><span class="sxs-lookup"><span data-stu-id="18ee1-140">Add these variables under hello variables parent element that you added toohello template.</span></span>

    ```json
    "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
    "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
    "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
    "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
    "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
    "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
    "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
    "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
      "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```
   
    * <span data-ttu-id="18ee1-141">DNS-nevek hello által használt hálózati illesztőt.</span><span class="sxs-lookup"><span data-stu-id="18ee1-141">DNS names that are used by hello network interfaces.</span></span>

        * <span data-ttu-id="18ee1-142">hello IP-címet nevének és a virtuális hálózati hello és-alhálózatok előtagok.</span><span class="sxs-lookup"><span data-stu-id="18ee1-142">hello IP address names and prefixes for hello virtual network and subnets.</span></span>
        * <span data-ttu-id="18ee1-143">hello nevek és hello virtuális hálózat, azonosítók betölteni a terheléselosztó és a hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="18ee1-143">hello names and identifiers of hello virtual network, load balancer, and network interfaces.</span></span>
        * <span data-ttu-id="18ee1-144">Hello fiókok hello gépek hello méretezési csoportban lévő társított tárfiókok neve.</span><span class="sxs-lookup"><span data-stu-id="18ee1-144">Storage account names for hello accounts associated with hello machines in hello scale set.</span></span>
        * <span data-ttu-id="18ee1-145">Hello hello virtuális gépeken telepített diagnosztika extension beállításait.</span><span class="sxs-lookup"><span data-stu-id="18ee1-145">Settings for hello Diagnostics extension that is installed on hello virtual machines.</span></span> <span data-ttu-id="18ee1-146">Hello diagnosztika bővítmény kapcsolatos további információkért lásd: [Windows virtuális gép létrehozása a figyelési és -diagnosztika Azure Resource Manager-sablon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="18ee1-146">For more information about hello Diagnostics extension, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

4. <span data-ttu-id="18ee1-147">Vegyen fel hello tárolási fiók erőforrást toohello sablon hozzáadásának hello erőforrások szülő elem alatt.</span><span class="sxs-lookup"><span data-stu-id="18ee1-147">Add hello storage account resource under hello resources parent element that you added toohello template.</span></span> <span data-ttu-id="18ee1-148">Ez a sablon használ egy hurok toocreate hello öt tárfiókok ajánlott hello operációs rendszer lemezek és diagnosztikai adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="18ee1-148">This template uses a loop toocreate hello recommended five storage accounts where hello operating system disks and diagnostic data are stored.</span></span> <span data-ttu-id="18ee1-149">E fiókok csoportja too100 virtuális gépek, amelyek hello aktuális maximális méretezési csoportban lévő képes támogatni.</span><span class="sxs-lookup"><span data-stu-id="18ee1-149">This set of accounts can support up too100 virtual machines in a scale set, which is hello current maximum.</span></span> <span data-ttu-id="18ee1-150">Minden tárfiók egy levél jelzéssel hello előtagjával hello paraméterek hello sablon együtt hello változók definiált neve.</span><span class="sxs-lookup"><span data-stu-id="18ee1-150">Each storage account is named with a letter designator that was defined in hello variables combined with hello prefix that you provide in hello parameters for hello template.</span></span>

    ```json
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
      "apiVersion": "2015-06-15",
      "copy": {
        "name": "storageLoop",
        "count": 5
      },
      "location": "[resourceGroup().location]",
      "properties": { "accountType": "Standard_LRS" }
    },
    ```

5. <span data-ttu-id="18ee1-151">Hello virtuális hálózati erőforrás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="18ee1-151">Add hello virtual network resource.</span></span> <span data-ttu-id="18ee1-152">További információkért lásd: [hálózati erőforrás-szolgáltató](../virtual-network/resource-groups-networking.md).</span><span class="sxs-lookup"><span data-stu-id="18ee1-152">For more information, see [Network Resource Provider](../virtual-network/resource-groups-networking.md).</span></span>

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
        "subnets": [
          {
            "name": "subnet1",
            "properties": { "addressPrefix": "10.0.0.0/24" }
          }
        ]
      }
    },
    ```

6. <span data-ttu-id="18ee1-153">Adja hozzá a hello nyilvános IP-cím erőforrás hello által használt terheléselosztó és a hálózati kapcsolat betöltése.</span><span class="sxs-lookup"><span data-stu-id="18ee1-153">Add hello public IP address resources that are used by hello load balancer and network interface.</span></span>

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP1')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName1')]"
        }
      }
    },
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP2')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName2')]"
        }
      }
    },
    ```

7. <span data-ttu-id="18ee1-154">Adja hozzá a hello méretezési készlet által használt hello terheléselosztó erőforrást.</span><span class="sxs-lookup"><span data-stu-id="18ee1-154">Add hello load balancer resource that is used by hello scale set.</span></span> <span data-ttu-id="18ee1-155">További információkért lásd: [Azure Resource Manager támogatása a terheléselosztó](../load-balancer/load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="18ee1-155">For more information, see [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md).</span></span>

    ```json   
    {
      "apiVersion": "2015-06-15",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
              }
            }
          }
        ],
        "backendAddressPools": [ { "name": "bepool1" } ],
        "inboundNatPools": [
          {
            "name": "natpool1",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPortRangeStart": 50000,
              "frontendPortRangeEnd": 50500,
              "backendPort": 3389
            }
          }
        ]
      }
    },
    ```

8. <span data-ttu-id="18ee1-156">Vegyen fel hello hálózati illesztő erőforrást hello különálló virtuális gép által használt.</span><span class="sxs-lookup"><span data-stu-id="18ee1-156">Add hello network interface resource that is used by hello separate virtual machine.</span></span> <span data-ttu-id="18ee1-157">Méretezési csoportban lévő gépek nem nyilvános IP-címnek keresztül érhető el, mert egy különálló virtuális gép jön létre hello azonos virtuális hálózati tooremotely hozzáférés hello gépek.</span><span class="sxs-lookup"><span data-stu-id="18ee1-157">Because machines in a scale set aren't accessible through a public IP address, a separate virtual machine is created in hello same virtual network tooremotely access hello machines.</span></span>

    ```json  
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
              },
              "subnet": {
                "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
              }
            }
          }
        ]
      }
    },
    ```

9. <span data-ttu-id="18ee1-158">Adja hozzá azonos hálózati hello méretezési beállított hello hello különálló virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="18ee1-158">Add hello separate virtual machine in hello same network as hello scale set.</span></span>

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": { "vmSize": "Standard_A1" },
        "osProfile": {
          "computername": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2012-R2-Datacenter",
            "version": "latest"
          },
          "osDisk": {
            "name": "[concat(parameters('resourcePrefix'), 'os1')]",
            "vhd": {
              "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"        
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        }
      }
    },
    ```

10. <span data-ttu-id="18ee1-159">Hello virtuálisgép-méretezési csoport erőforrás hozzáadása, és adja meg a hello diagnosztika-bővítménnyel, amely hello méretezési csoportban lévő összes virtuális gép telepítve van.</span><span class="sxs-lookup"><span data-stu-id="18ee1-159">Add hello virtual machine scale set resource and specify hello diagnostics extension that is installed on all virtual machines in hello scale set.</span></span> <span data-ttu-id="18ee1-160">Ehhez az erőforráshoz hello beállításainak jelentős része hasonlóak a hello virtuálisgép-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="18ee1-160">Many of hello settings for this resource are similar with hello virtual machine resource.</span></span> <span data-ttu-id="18ee1-161">hello fő különbségek a következők: hello kapacitás elem hello méretezési csoportban lévő virtuális gépek hello számát megadó és upgradePolicy, amely meghatározza, hogyan frissítések válnak, toovirtual gépek.</span><span class="sxs-lookup"><span data-stu-id="18ee1-161">hello main differences are hello capacity element that specifies hello number of virtual machines in hello scale set, and upgradePolicy that specifies how updates are made toovirtual machines.</span></span> <span data-ttu-id="18ee1-162">hello méretezési készlet nem jön létre az összes hello tárfiók létrehozásáig hello dependsOn elemmel megadottak szerint.</span><span class="sxs-lookup"><span data-stu-id="18ee1-162">hello scale set is not created until all hello storage accounts are created as specified with hello dependsOn element.</span></span>

    ```json
    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "apiVersion": "2016-03-30",
      "name": "[parameters('vmSSName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "sku": {
        "name": "Standard_A1",
        "tier": "Standard",
        "capacity": "[parameters('instanceCount')]"
      },
      "properties": {
        "upgradePolicy": {
          "mode": "Manual"
        },
        "virtualMachineProfile": {
          "storageProfile": {
            "osDisk": {
              "vhdContainers": [
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "MicrosoftWindowsServer",
              "offer": "WindowsServer",
              "sku": "2012-R2-Datacenter",
              "version": "latest"
            }
          },
          "osProfile": {
            "computerNamePrefix": "[parameters('vmSSName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "networkconfig1",
                "properties": {
                  "primary": "true",
                  "ipConfigurations": [
                    {
                      "name": "ip1",
                      "properties": {
                        "subnet": {
                          "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          },
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Insights.VMDiagnosticsSettings",
                "properties": {
                  "publisher": "Microsoft.Azure.Diagnostics",
                  "type": "IaaSDiagnostics",
                  "typeHandlerVersion": "1.5",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint": "https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. <span data-ttu-id="18ee1-163">Vegyen fel hello autoscaleSettings erőforrást, amely meghatározza, hogyan hello méretezési hello processzor kihasználtsága hello méretezési csoportban lévő hello gépeken alapján állítja be.</span><span class="sxs-lookup"><span data-stu-id="18ee1-163">Add hello autoscaleSettings resource that defines how hello scale set adjusts based on hello processor usage on hello machines in hello scale set.</span></span>

    ```json
    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Processor(_Total)\\% Processor Time",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 50.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      }
    }
    ```

    <span data-ttu-id="18ee1-164">Ebben az oktatóanyagban fontosak a ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="18ee1-164">For this tutorial, these values are important:</span></span>
    
    * <span data-ttu-id="18ee1-165">**metricName**</span><span class="sxs-lookup"><span data-stu-id="18ee1-165">**metricName**</span></span>  
    <span data-ttu-id="18ee1-166">Ez az érték azonos a hello wadperfcounter változóban meghatározott hello teljesítményszámláló van hello.</span><span class="sxs-lookup"><span data-stu-id="18ee1-166">This value is hello same as hello performance counter that we defined in hello wadperfcounter variable.</span></span> <span data-ttu-id="18ee1-167">Használja ezt a változót, hello diagnosztika bővítmény gyűjt hello **processzor(_Total)\% processzoridő** számláló.</span><span class="sxs-lookup"><span data-stu-id="18ee1-167">Using that variable, hello Diagnostics extension collects hello  **Processor(_Total)\% Processor Time** counter.</span></span>
    
    * <span data-ttu-id="18ee1-168">**metricResourceUri**</span><span class="sxs-lookup"><span data-stu-id="18ee1-168">**metricResourceUri**</span></span>  
    <span data-ttu-id="18ee1-169">Ez az érték hello erőforrás-azonosítót a virtuálisgép-méretezési csoport hello.</span><span class="sxs-lookup"><span data-stu-id="18ee1-169">This value is hello resource identifier of hello virtual machine scale set.</span></span>
    
    * <span data-ttu-id="18ee1-170">**időkeretben vannak**</span><span class="sxs-lookup"><span data-stu-id="18ee1-170">**timeGrain**</span></span>  
    <span data-ttu-id="18ee1-171">Ez az érték hello metrikák összegyűjtött hello granularitása.</span><span class="sxs-lookup"><span data-stu-id="18ee1-171">This value is hello granularity of hello metrics that are collected.</span></span> <span data-ttu-id="18ee1-172">A sablon értéke tooone perc.</span><span class="sxs-lookup"><span data-stu-id="18ee1-172">In this template, it is set tooone minute.</span></span>
    
    * <span data-ttu-id="18ee1-173">**statisztika**</span><span class="sxs-lookup"><span data-stu-id="18ee1-173">**statistic**</span></span>  
    <span data-ttu-id="18ee1-174">Ez az érték határozza meg, hogyan hello adatok gyűjtése le kombinált tooaccommodate hello automatikus skálázás művelet.</span><span class="sxs-lookup"><span data-stu-id="18ee1-174">This value determines how hello metrics are combined tooaccommodate hello automatic scaling action.</span></span> <span data-ttu-id="18ee1-175">hello lehetséges értékek a következők: átlagos, Min, Max.</span><span class="sxs-lookup"><span data-stu-id="18ee1-175">hello possible values are: Average, Min, Max.</span></span> <span data-ttu-id="18ee1-176">Ez a sablon a hello átlagos teljes CPU-használat hello virtuális gépek gyűjti.</span><span class="sxs-lookup"><span data-stu-id="18ee1-176">In this template, hello average total CPU usage of hello virtual machines is collected.</span></span>
    
    * <span data-ttu-id="18ee1-177">**Az időtartomány értékének**</span><span class="sxs-lookup"><span data-stu-id="18ee1-177">**timeWindow**</span></span>  
    <span data-ttu-id="18ee1-178">Ez az érték, amely hello amelyben példány adatgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="18ee1-178">This value is hello range of time in which instance data is collected.</span></span> <span data-ttu-id="18ee1-179">5 perc és 12 óra között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="18ee1-179">It must be between 5 minutes and 12 hours.</span></span>
    
    * <span data-ttu-id="18ee1-180">**timeAggregation**</span><span class="sxs-lookup"><span data-stu-id="18ee1-180">**timeAggregation**</span></span>  
    <span data-ttu-id="18ee1-181">az érték szabja meg, hogyan hello adatokat gyűjtött össze kell vonni adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="18ee1-181">his value determines how hello data that is collected should be combined over time.</span></span> <span data-ttu-id="18ee1-182">hello alapértelmezett érték átlaga.</span><span class="sxs-lookup"><span data-stu-id="18ee1-182">hello default value is Average.</span></span> <span data-ttu-id="18ee1-183">hello lehetséges értékek a következők: átlagos, Minimum, Maximum, utolsó, összesen, száma.</span><span class="sxs-lookup"><span data-stu-id="18ee1-183">hello possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span>
    
    * <span data-ttu-id="18ee1-184">**operátor**</span><span class="sxs-lookup"><span data-stu-id="18ee1-184">**operator**</span></span>  
    <span data-ttu-id="18ee1-185">Ez az érték használt toocompare hello metrika adatok és hello küszöbérték, hello operátor.</span><span class="sxs-lookup"><span data-stu-id="18ee1-185">This value is hello operator that is used toocompare hello metric data and hello threshold.</span></span> <span data-ttu-id="18ee1-186">hello lehetséges értékek a következők: egyenlő NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="18ee1-186">hello possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span>
    
    * <span data-ttu-id="18ee1-187">**küszöbérték**</span><span class="sxs-lookup"><span data-stu-id="18ee1-187">**threshold**</span></span>  
    <span data-ttu-id="18ee1-188">Ez az érték hello érték, amely elindítja a hello skálázási művelet.</span><span class="sxs-lookup"><span data-stu-id="18ee1-188">This value is hello value that triggers hello scale action.</span></span> <span data-ttu-id="18ee1-189">Ez a sablon a gépeket adnak toohello méretezési készletben, ha hello átlagos CPU-használat hello készlet gépek között több mint 50 %.</span><span class="sxs-lookup"><span data-stu-id="18ee1-189">In this template, machines are added toohello scale set when hello average CPU usage among machines in hello set is over 50%.</span></span>
    
    * <span data-ttu-id="18ee1-190">**iránya**</span><span class="sxs-lookup"><span data-stu-id="18ee1-190">**direction**</span></span>  
    <span data-ttu-id="18ee1-191">Ez az érték határozza meg a hello hello küszöbérték érhető el, amikor végrehajtandó műveletet.</span><span class="sxs-lookup"><span data-stu-id="18ee1-191">This value determines hello action that is taken when hello threshold value is achieved.</span></span> <span data-ttu-id="18ee1-192">hello lehetséges értékei növekedése vagy csökkenése.</span><span class="sxs-lookup"><span data-stu-id="18ee1-192">hello possible values are Increase or Decrease.</span></span> <span data-ttu-id="18ee1-193">Ez a sablon a hello hello méretezési csoportban lévő virtuális gépek száma nő, amikor több mint 50 %-a meghatározott hello időkerete hello küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="18ee1-193">In this template, hello number of virtual machines in hello scale set is increased if hello threshold is over 50% in hello defined time window.</span></span>
    
    * <span data-ttu-id="18ee1-194">**típusa**</span><span class="sxs-lookup"><span data-stu-id="18ee1-194">**type**</span></span>  
    <span data-ttu-id="18ee1-195">Ez az érték egy végrehajtandó műveletet kell végrehajtani, és be kell állítani tooChangeCount hello típusú.</span><span class="sxs-lookup"><span data-stu-id="18ee1-195">This value is hello type of action that should occur and must be set tooChangeCount.</span></span>
    
    * <span data-ttu-id="18ee1-196">**érték**</span><span class="sxs-lookup"><span data-stu-id="18ee1-196">**value**</span></span>  
    <span data-ttu-id="18ee1-197">Ez az érték hello hozzáadásakor vagy eltávolításakor hello méretezési csoport a virtuális gépek száma.</span><span class="sxs-lookup"><span data-stu-id="18ee1-197">This value is hello number of virtual machines that are added or removed from hello scale set.</span></span> <span data-ttu-id="18ee1-198">Ez az érték 1 vagy nagyobb lehet.</span><span class="sxs-lookup"><span data-stu-id="18ee1-198">This value must be 1 or greater.</span></span> <span data-ttu-id="18ee1-199">hello alapértelmezett értéke 1.</span><span class="sxs-lookup"><span data-stu-id="18ee1-199">hello default value is 1.</span></span> <span data-ttu-id="18ee1-200">A sablonban lévő hello méretezési gépek hello száma növekszik helyezésekor által beállított 1 hello küszöbérték elérése.</span><span class="sxs-lookup"><span data-stu-id="18ee1-200">In this template, hello number of machines in hello scale set increases by 1 when hello threshold is met.</span></span>
    
    * <span data-ttu-id="18ee1-201">**cooldown**</span><span class="sxs-lookup"><span data-stu-id="18ee1-201">**cooldown**</span></span>  
    <span data-ttu-id="18ee1-202">Ez az érték érték hello idő toowait óta hello utolsó méretezési művelet előtt hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="18ee1-202">This value is hello amount of time toowait since hello last scaling action before hello next action occurs.</span></span> <span data-ttu-id="18ee1-203">Ez az érték egy percet, és egy hét között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="18ee1-203">This value must be between one minute and one week.</span></span>

12. <span data-ttu-id="18ee1-204">Hello sablonfájl mentési.</span><span class="sxs-lookup"><span data-stu-id="18ee1-204">Save hello template file.</span></span>    

## <a name="step-4-upload-hello-template-toostorage"></a><span data-ttu-id="18ee1-205">4. lépés: Töltse fel a hello sablon toostorage</span><span class="sxs-lookup"><span data-stu-id="18ee1-205">Step 4: Upload hello template toostorage</span></span>
<span data-ttu-id="18ee1-206">hello sablon mindaddig, amíg hello nevét és az 1. lépésben létrehozott hello tárfiók elsődleges kulcs ismeri fel kell tölteni.</span><span class="sxs-lookup"><span data-stu-id="18ee1-206">hello template can be uploaded as long as you know hello name and primary key of hello storage account that you created in step 1.</span></span>

1. <span data-ttu-id="18ee1-207">Hello Microsoft Azure PowerShell-ablakot állítson be egy változó, amely megadja az 1. lépésben létrehozott tárfiók hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="18ee1-207">In hello Microsoft Azure PowerShell window, set a variable that specifies hello name of hello storage account that you created in step 1.</span></span>
   
    ```powershell
    $storageAccountName = "vmstestsa"
    ```

2. <span data-ttu-id="18ee1-208">Állítsa be a változó, amely meghatározza az elsődleges kulcs hello hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="18ee1-208">Set a variable that specifies hello primary key of hello storage account.</span></span>
   
    ```powershell
    $storageAccountKey = "<primary-account-key>"
    ```
   
   <span data-ttu-id="18ee1-209">Ez a kulcs kaphat hello kulcs ikonra kattintva, amikor megtekintik hello tárolási fiók erőforrás hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="18ee1-209">You can get this key by clicking hello key icon when viewing hello storage account resource in hello Azure portal.</span></span>
3. <span data-ttu-id="18ee1-210">Hello fiók környezetben tárolóobjektum létrehozásához használt toovalidate műveletek hello storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="18ee1-210">Create hello storage account context object that is used toovalidate operations with hello storage account.</span></span>
   
    ```powershell
    $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
    ```

4. <span data-ttu-id="18ee1-211">Az hello sablon tárolására alkalmas hello tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="18ee1-211">Create hello container for storing hello template.</span></span>

    ```powershell
    $containerName = "templates"
    New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob
    ```

5. <span data-ttu-id="18ee1-212">Töltse fel a hello sablon fájl toohello új tárolót.</span><span class="sxs-lookup"><span data-stu-id="18ee1-212">Upload hello template file toohello new container.</span></span>

    ```powershell   
    $blobName = "VMSSTemplate.json"
    $fileName = "C:\" + $BlobName
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx
    ```

## <a name="step-5-deploy-hello-template"></a><span data-ttu-id="18ee1-213">5. lépés: Hello sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="18ee1-213">Step 5: Deploy hello template</span></span>
<span data-ttu-id="18ee1-214">Most, hogy létrehozta a hello sablon, megkezdheti a hello erőforrásokat üzembe helyezi.</span><span class="sxs-lookup"><span data-stu-id="18ee1-214">Now that you created hello template, you can start deploying hello resources.</span></span> <span data-ttu-id="18ee1-215">A parancs toostart hello folyamat használja:</span><span class="sxs-lookup"><span data-stu-id="18ee1-215">Use this command toostart hello process:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"
```

<span data-ttu-id="18ee1-216">Amikor lenyomja az adja meg, a hozzárendelt hello változók felszólító tooprovide értékei lesznek.</span><span class="sxs-lookup"><span data-stu-id="18ee1-216">When you press enter, you are prompted tooprovide values for hello variables you assigned.</span></span> <span data-ttu-id="18ee1-217">Adja meg ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="18ee1-217">Provide these values:</span></span>

```powershell
vmName: vmsstestvm1
  vmSSName: vmsstest1
  instanceCount: 5
  adminUserName: vmadmin1
  adminPassword: VMpass1
  resourcePrefix: vmsstest
```

<span data-ttu-id="18ee1-218">Mintegy 15 percre leáll az összes hello erőforrások toosuccessfully telepíteni kell vennie.</span><span class="sxs-lookup"><span data-stu-id="18ee1-218">It should take about 15 minutes for all hello resources toosuccessfully be deployed.</span></span>

> [!NOTE]
> <span data-ttu-id="18ee1-219">Hello portal képességét toodeploy hello erőforrásokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="18ee1-219">You can also use hello portal’s ability toodeploy hello resources.</span></span> <span data-ttu-id="18ee1-220">A hivatkozás: "https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template>"</span><span class="sxs-lookup"><span data-stu-id="18ee1-220">Use this link: "https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template>"</span></span>
> 
> 

## <a name="step-6-monitor-resources"></a><span data-ttu-id="18ee1-221">6. lépés: A figyelő erőforrások</span><span class="sxs-lookup"><span data-stu-id="18ee1-221">Step 6: Monitor resources</span></span>
<span data-ttu-id="18ee1-222">Virtuálisgép-méretezési csoportok ezekkel a módszerekkel kapcsolatos információkat kaphat:</span><span class="sxs-lookup"><span data-stu-id="18ee1-222">You can get some information about virtual machine scale sets using these methods:</span></span>

* <span data-ttu-id="18ee1-223">Azure-portálon hello – jelenleg korlátozott mennyiségű információt hello portál használatával kaphat.</span><span class="sxs-lookup"><span data-stu-id="18ee1-223">hello Azure portal - You can currently get a limited amount of information using hello portal.</span></span>
* <span data-ttu-id="18ee1-224">Hello [Azure erőforrás-kezelő](https://resources.azure.com/) -Ez az eszköz felderítését lehetővé tevő hello aktuális állapotát, a méretezési legjobb hello.</span><span class="sxs-lookup"><span data-stu-id="18ee1-224">hello [Azure Resource Explorer](https://resources.azure.com/) - This tool is hello best for exploring hello current state of your scale set.</span></span> <span data-ttu-id="18ee1-225">Hajtsa végre ezt az elérési utat, és megtekintheti az hello példányait tartalmazó nézetet hello méretezési beállítása létrehozott:</span><span class="sxs-lookup"><span data-stu-id="18ee1-225">Follow this path and you should see hello instance view of hello scale set that you created:</span></span>
  
    <span data-ttu-id="18ee1-226">előfizetések > {az előfizetés} > resourceGroups > vmsstestrg1 > szolgáltatók > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtuális gépek vannak</span><span class="sxs-lookup"><span data-stu-id="18ee1-226">subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines</span></span>

* <span data-ttu-id="18ee1-227">Az Azure PowerShell - használja a parancs tooget néhány információt:</span><span class="sxs-lookup"><span data-stu-id="18ee1-227">Azure PowerShell - Use this command tooget some information:</span></span>

  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  ```
  
  <span data-ttu-id="18ee1-228">Vagy</span><span class="sxs-lookup"><span data-stu-id="18ee1-228">Or</span></span>
  
  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView
  ```

* <span data-ttu-id="18ee1-229">Csatlakoztassa a toohello külön virtuális gépet, ugyanúgy, mint bármely más gép üzembe helyezése, és ezután távolról elérheti hello virtuális gépek hello méretezési készlet toomonitor az egyes folyamatok.</span><span class="sxs-lookup"><span data-stu-id="18ee1-229">Connect toohello separate virtual machine just like you would any other machine and then you can remotely access hello virtual machines in hello scale set toomonitor individual processes.</span></span>

> [!NOTE]
> <span data-ttu-id="18ee1-230">A teljes REST API-t a méretezési készlet vonatkozó adatok találhatók [virtuálisgép-skálázási készletekben](https://msdn.microsoft.com/library/mt589023.aspx)</span><span class="sxs-lookup"><span data-stu-id="18ee1-230">A complete REST API for obtaining information about scale sets can be found in [Virtual Machine Scale Sets](https://msdn.microsoft.com/library/mt589023.aspx)</span></span>

## <a name="step-7-remove-hello-resources"></a><span data-ttu-id="18ee1-231">7. lépés: Hello erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="18ee1-231">Step 7: Remove hello resources</span></span>
<span data-ttu-id="18ee1-232">Mivel van szó, az Azure-ban használt erőforrásokhoz, még mindig egy célszerű toodelete erőforrásokat, amelyek már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="18ee1-232">Because you are charged for resources used in Azure, it is always a good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="18ee1-233">Nincs szükség a toodelete az egyes erőforrások elkülönítve egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="18ee1-233">You don’t need toodelete each resource separately from a resource group.</span></span> <span data-ttu-id="18ee1-234">Hello erőforráscsoport törlése és a hozzá tartozó összes erőforrásnak automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="18ee1-234">You can delete hello resource group and all its resources are automatically deleted.</span></span>

  ```powershell
  Remove-AzureRmResourceGroup -Name vmsstestrg1
  ```

<span data-ttu-id="18ee1-235">Ha azt szeretné, tookeep az erőforráscsoport, törölheti hello méretezési készletben csak.</span><span class="sxs-lookup"><span data-stu-id="18ee1-235">If you want tookeep your resource group, you can delete hello scale set only.</span></span>

  ```powershell
  Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"
  ```

## <a name="next-steps"></a><span data-ttu-id="18ee1-236">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18ee1-236">Next steps</span></span>
* <span data-ttu-id="18ee1-237">Kezelheti az imént hello méretezési hello témakörben található információk alapján létrehozott [egy virtuálisgép-méretezési csoportban lévő virtuális gépeket](virtual-machine-scale-sets-windows-manage.md).</span><span class="sxs-lookup"><span data-stu-id="18ee1-237">Manage hello scale set that you just created using hello information in [Manage virtual machines in a Virtual Machine Scale Set](virtual-machine-scale-sets-windows-manage.md).</span></span>
* <span data-ttu-id="18ee1-238">További információ a vertikális skálázásáról: [Vertikális automatikus méretezés a virtuálisgép-méretezési csoportokkal](virtual-machine-scale-sets-vertical-scale-reprovision.md)</span><span class="sxs-lookup"><span data-stu-id="18ee1-238">Learn more about vertical scaling by reviewing [Vertical autoscale with Virtual Machine Scale sets](virtual-machine-scale-sets-vertical-scale-reprovision.md)</span></span>
* <span data-ttu-id="18ee1-239">Példák figyelési szolgáltatásokat az Azure-figyelő található [Azure figyelő PowerShell quick start minták](../monitoring-and-diagnostics/insights-powershell-samples.md)</span><span class="sxs-lookup"><span data-stu-id="18ee1-239">Find examples of Azure Monitor monitoring features in [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md)</span></span>
* <span data-ttu-id="18ee1-240">További tudnivalók az értesítési szolgáltatások [automatikus skálázási műveletek toosend e-mailek és a webhook riasztási értesítések használata az Azure-figyelő](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="18ee1-240">Learn about notification features in [Use autoscale actions toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span></span>
* <span data-ttu-id="18ee1-241">Ismerje meg, hogyan túl[használata auditnaplókat toosend e-mailek és a webhook riasztási értesítések az Azure-figyelő](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="18ee1-241">Learn how too[Use audit logs toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>

