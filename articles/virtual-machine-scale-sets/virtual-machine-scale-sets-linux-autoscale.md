---
title: "Linux virtuálisgép-méretezési csoportok aaaAutoscale |} Microsoft Docs"
description: "A Linux virtuális gépek méretezési csoportján Azure parancssori felület használatával automatikus skálázás beállítása"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 83e93d9c-cac0-41d3-8316-6016f5ed0ce4
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: 4352b94ec2973c37bc5616e3be25bd0c9442632b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-linux-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="73aef-103">Automatikus méretezése a virtuálisgép-méretezési csoportban lévő Linux-gépek</span><span class="sxs-lookup"><span data-stu-id="73aef-103">Automatically scale Linux machines in a virtual machine scale set</span></span>
<span data-ttu-id="73aef-104">Virtuálisgép-méretezési csoportok könnyítheti meg toodeploy, és egyetlen egységként azonos virtuális gépek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="73aef-104">Virtual machine scale sets make it easy for you toodeploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="73aef-105">Méretezési csoportok hyperscale alkalmazások jól méretezhető és testre szabható számítási rétegét adja meg, és a Windows platform-lemezképek, Linux platformon képek, egyéni lemezképek és bővítmények támogatják.</span><span class="sxs-lookup"><span data-stu-id="73aef-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="73aef-106">több, lásd: toolearn [virtuális gépek méretezési készletek áttekintése](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="73aef-106">toolearn more, see [Virtual Machine Scale Sets Overview](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="73aef-107">Az oktatóanyag bemutatja, hogyan toocreate terjedő skálán állítsa be a Linux virtuális gépek hello Ubuntu Linux legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="73aef-107">This tutorial shows you how toocreate a scale set of Linux virtual machines using hello latest version of Ubuntu Linux.</span></span> <span data-ttu-id="73aef-108">hello oktatóanyag is bemutatja, hogyan tooautomatically méretezési hello gépek hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="73aef-108">hello tutorial also shows you how tooautomatically scale hello machines in hello set.</span></span> <span data-ttu-id="73aef-109">Állítsa be, és állítsa be az Azure Resource Manager-sablon létrehozása és telepítése az Azure parancssori felület használatával méretezhetővé hello méretezési hoz létre.</span><span class="sxs-lookup"><span data-stu-id="73aef-109">You create hello scale set and set up scaling by creating an Azure Resource Manager template and deploying it using Azure CLI.</span></span> <span data-ttu-id="73aef-110">A sablonokról további információkat az [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) (Azure Resource Manager-sablonok készítése) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="73aef-110">For more information about templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="73aef-111">toolearn méretezési készlet automatikus skálázás kapcsolatos további információkért lásd: [automatikus skálázás és virtuálisgép-skálázási készletekben](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="73aef-111">toolearn more about automatic scaling of scale sets, see [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

<span data-ttu-id="73aef-112">Ebben az oktatóanyagban hello következő telepít erőforrások és bővítmények:</span><span class="sxs-lookup"><span data-stu-id="73aef-112">In this tutorial, you deploy hello following resources and extensions:</span></span>

* <span data-ttu-id="73aef-113">Microsoft.Storage/storageAccounts</span><span class="sxs-lookup"><span data-stu-id="73aef-113">Microsoft.Storage/storageAccounts</span></span>
* <span data-ttu-id="73aef-114">Microsoft.Network/virtualNetworks</span><span class="sxs-lookup"><span data-stu-id="73aef-114">Microsoft.Network/virtualNetworks</span></span>
* <span data-ttu-id="73aef-115">Microsoft.Network/publicIPAddresses</span><span class="sxs-lookup"><span data-stu-id="73aef-115">Microsoft.Network/publicIPAddresses</span></span>
* <span data-ttu-id="73aef-116">Microsoft.Network/loadBalancers</span><span class="sxs-lookup"><span data-stu-id="73aef-116">Microsoft.Network/loadBalancers</span></span>
* <span data-ttu-id="73aef-117">Microsoft.Network/networkInterfaces</span><span class="sxs-lookup"><span data-stu-id="73aef-117">Microsoft.Network/networkInterfaces</span></span>
* <span data-ttu-id="73aef-118">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="73aef-118">Microsoft.Compute/virtualMachines</span></span>
* <span data-ttu-id="73aef-119">Microsoft.Compute/virtualMachineScaleSets</span><span class="sxs-lookup"><span data-stu-id="73aef-119">Microsoft.Compute/virtualMachineScaleSets</span></span>
* <span data-ttu-id="73aef-120">Microsoft.Insights.VMDiagnosticsSettings</span><span class="sxs-lookup"><span data-stu-id="73aef-120">Microsoft.Insights.VMDiagnosticsSettings</span></span>
* <span data-ttu-id="73aef-121">Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="73aef-121">Microsoft.Insights/autoscaleSettings</span></span>

<span data-ttu-id="73aef-122">Erőforrás-kezelő erőforrásokra vonatkozó további információkért lásd: [Azure Resource Manager és klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="73aef-122">For more information about Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

<span data-ttu-id="73aef-123">Mielőtt hozzáfogna ebben az oktatóanyagban hello lépésben [hello Azure parancssori felület telepítése](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="73aef-123">Before you get started with hello steps in this tutorial, [install hello Azure CLI](../cli-install-nodejs.md).</span></span>

## <a name="step-1-create-a-resource-group-and-a-storage-account"></a><span data-ttu-id="73aef-124">1. lépés: Az erőforráscsoportot és a storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="73aef-124">Step 1: Create a resource group and a storage account</span></span>

1. <span data-ttu-id="73aef-125">**Jelentkezzen be Azure tooMicrosoft**</span><span class="sxs-lookup"><span data-stu-id="73aef-125">**Sign in tooMicrosoft Azure**</span></span>  
<span data-ttu-id="73aef-126">A parancssori felület (Bash, terminál, parancssor), a kapcsoló tooResource kezelő módra, majd [jelentkezzen be a munkahelyi vagy iskolai azonosítóját](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Hajtsa végre az interaktív bejelentkezési élmény tooyour Azure-fiók hello kér.</span><span class="sxs-lookup"><span data-stu-id="73aef-126">In your command-line interface (Bash, Terminal, Command prompt), switch tooResource Manager mode, and then [log in with your work or school id](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Follow hello prompts for an interactive login experience tooyour Azure account.</span></span>

    ```cli   
    azure config mode arm

    azure login
    ```
   
    > [!NOTE]
    > <span data-ttu-id="73aef-127">Ha rendelkezik munkahelyi vagy iskolai azonosítója, és nem kéttényezős hitelesítést, használja `azure login -u` a hello azonosító toolog a egy interaktív munkamenet nélkül.</span><span class="sxs-lookup"><span data-stu-id="73aef-127">If you have a work or school ID and you do not have two-factor authentication enabled, use `azure login -u` with hello ID toolog in without an interactive session.</span></span> <span data-ttu-id="73aef-128">Ha nem rendelkezik munkahelyi vagy iskolai azonosítója, akkor [munkahelyi vagy iskolai azonosító létrehozása a személyes Microsoft-fiókhoz tartozó](../active-directory/active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="73aef-128">If you don't have a work or school ID, you can [create a work or school id from your personal Microsoft account](../active-directory/active-directory-users-create-azure-portal.md).</span></span>
    
2. <span data-ttu-id="73aef-129">**Hozzon létre egy erőforráscsoportot**</span><span class="sxs-lookup"><span data-stu-id="73aef-129">**Create a resource group**</span></span>  
<span data-ttu-id="73aef-130">Összes erőforrást erőforráscsoportban telepített tooa kell lennie.</span><span class="sxs-lookup"><span data-stu-id="73aef-130">All resources must be deployed tooa resource group.</span></span> <span data-ttu-id="73aef-131">Ebben az oktatóanyagban nevezze el hello erőforráscsoportot **vmsstest1**.</span><span class="sxs-lookup"><span data-stu-id="73aef-131">For this tutorial, name hello resource group **vmsstest1**.</span></span>
   
    ```cli
    azure group create vmsstestrg1 centralus
    ```

3. <span data-ttu-id="73aef-132">**A storage-fiók üzembe helyezés hello új erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="73aef-132">**Deploy a storage account into hello new resource group**</span></span>  
<span data-ttu-id="73aef-133">Ez a tárfiók hello sablon tárolására.</span><span class="sxs-lookup"><span data-stu-id="73aef-133">This storage account is where hello template is stored.</span></span> <span data-ttu-id="73aef-134">Hozzon létre egy tárfiókot, nevű **vmsstestsa**.</span><span class="sxs-lookup"><span data-stu-id="73aef-134">Create a storage account named **vmsstestsa**.</span></span>
   
    ```cli
    azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa
    ```

## <a name="step-2-create-hello-template"></a><span data-ttu-id="73aef-135">2. lépés: Hello sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="73aef-135">Step 2: Create hello template</span></span>
<span data-ttu-id="73aef-136">Az Azure Resource Manager-sablon, toodeploy lehetővé teszi, és Azure-erőforrások együtt kezelheti a JSON-leírásuk hello erőforrások és a társított telepítési paramétereket.</span><span class="sxs-lookup"><span data-stu-id="73aef-136">An Azure Resource Manager template makes it possible for you toodeploy and manage Azure resources together by using a JSON description of hello resources and associated deployment parameters.</span></span>

1. <span data-ttu-id="73aef-137">A kedvenc szerkesztő hozzon létre VMSSTemplate.json hello fájlt, és hello kezdeti JSON struktúrában toosupport hello sablon hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="73aef-137">In your favorite editor, create hello file VMSSTemplate.json and add hello initial JSON structure toosupport hello template.</span></span>

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

2. <span data-ttu-id="73aef-138">Paraméterek nem mindig szükséges, de biztosítanak egy módon tooinput értékek hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="73aef-138">Parameters are not always required, but they provide a way tooinput values when hello template is deployed.</span></span> <span data-ttu-id="73aef-139">Adja hozzá ezeket a paramétereket, hogy hozzáadta a toohello sablon hello paraméterek szülő elem alatt.</span><span class="sxs-lookup"><span data-stu-id="73aef-139">Add these parameters under hello parameters parent element that you added toohello template.</span></span>

    ```json
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
   * <span data-ttu-id="73aef-140">Hello külön virtuális gépre, amelyik használt tooaccess hello gépek hello méretezési csoportban lévő nevét.</span><span class="sxs-lookup"><span data-stu-id="73aef-140">A name for hello separate virtual machine that is used tooaccess hello machines in hello scale set.</span></span>
   * <span data-ttu-id="73aef-141">Hello sablon tárolására hello storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="73aef-141">A name for hello storage account where hello template is stored.</span></span>
   * <span data-ttu-id="73aef-142">hello méretezési csoportban lévő virtuális gépek tooinitially példányainak száma hello hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="73aef-142">hello number of instances of virtual machines tooinitially create in hello scale set.</span></span>
   * <span data-ttu-id="73aef-143">A név és jelszó hello rendszergazdai fiókjának hello virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="73aef-143">A name and password of hello administrator account on hello virtual machines.</span></span>
   * <span data-ttu-id="73aef-144">Állítsa be a nevének előtagját toosupport hello méretezési létrehozott hello erőforrások.</span><span class="sxs-lookup"><span data-stu-id="73aef-144">A name prefix for hello resources that are created toosupport hello scale set.</span></span>

3. <span data-ttu-id="73aef-145">A sablon toospecify értékek, amelyek gyakran változhat a változók használhatók, vagy toobe igénylő értékek paraméterértékek kombinációja alapján létrehozott.</span><span class="sxs-lookup"><span data-stu-id="73aef-145">Variables can be used in a template toospecify values that may change frequently or values that need toobe created from a combination of parameter values.</span></span> <span data-ttu-id="73aef-146">Adja hozzá ezeket a változókat, hogy hozzáadta a toohello sablon hello változók szülő elem alatt.</span><span class="sxs-lookup"><span data-stu-id="73aef-146">Add these variables under hello variables parent element that you added toohello template.</span></span>

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
    "wadlogs": "<WadCfg><DiagnosticMonitorConfiguration>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor\\PercentProcessorTime\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU percentage guest OS\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```

   * <span data-ttu-id="73aef-147">DNS-nevek hello által használt hálózati illesztőt.</span><span class="sxs-lookup"><span data-stu-id="73aef-147">DNS names that are used by hello network interfaces.</span></span>
   * <span data-ttu-id="73aef-148">hello IP-címet nevének és a virtuális hálózati hello és-alhálózatok előtagok.</span><span class="sxs-lookup"><span data-stu-id="73aef-148">hello IP address names and prefixes for hello virtual network and subnets.</span></span>
   * <span data-ttu-id="73aef-149">hello nevek és hello virtuális hálózat, azonosítók betölteni a terheléselosztó és a hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="73aef-149">hello names and identifiers of hello virtual network, load balancer, and network interfaces.</span></span>
   * <span data-ttu-id="73aef-150">Hello fiókok hello gépek hello méretezési csoportban lévő társított tárfiókok neve.</span><span class="sxs-lookup"><span data-stu-id="73aef-150">Storage account names for hello accounts associated with hello machines in hello scale set.</span></span>
   * <span data-ttu-id="73aef-151">Hello hello virtuális gépeken telepített diagnosztika extension beállításait.</span><span class="sxs-lookup"><span data-stu-id="73aef-151">Settings for hello Diagnostics extension that is installed on hello virtual machines.</span></span> <span data-ttu-id="73aef-152">Hello diagnosztika bővítmény kapcsolatos további információkért lásd: [Windows virtuális gép létrehozása a figyelési és -diagnosztika Azure Resource Manager-sablon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="73aef-152">For more information about hello Diagnostics extension, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

4. <span data-ttu-id="73aef-153">Vegyen fel hello tárolási fiók erőforrást toohello sablon hozzáadásának hello erőforrások szülő elem alatt.</span><span class="sxs-lookup"><span data-stu-id="73aef-153">Add hello storage account resource under hello resources parent element that you added toohello template.</span></span> <span data-ttu-id="73aef-154">Ez a sablon használ egy hurok toocreate hello öt tárfiókok ajánlott hello operációs rendszer lemezek és diagnosztikai adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="73aef-154">This template uses a loop toocreate hello recommended five storage accounts where hello operating system disks and diagnostic data are stored.</span></span> <span data-ttu-id="73aef-155">E fiókok csoportja too100 virtuális gépek, amelyek hello aktuális maximális méretezési csoportban lévő képes támogatni.</span><span class="sxs-lookup"><span data-stu-id="73aef-155">This set of accounts can support up too100 virtual machines in a scale set, which is hello current maximum.</span></span> <span data-ttu-id="73aef-156">Minden tárfiók megadott hello paraméterek hello sablon hello utótaggal együtt hello változók definiált levél jelzéssel neve.</span><span class="sxs-lookup"><span data-stu-id="73aef-156">Each storage account is named with a letter designator that was defined in hello variables combined with hello suffix that you provide in hello parameters for hello template.</span></span>
   
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

5. <span data-ttu-id="73aef-157">Hello virtuális hálózati erőforrás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="73aef-157">Add hello virtual network resource.</span></span> <span data-ttu-id="73aef-158">További információkért lásd: [hálózati erőforrás-szolgáltató](../virtual-network/resource-groups-networking.md).</span><span class="sxs-lookup"><span data-stu-id="73aef-158">For more information, see [Network Resource Provider](../virtual-network/resource-groups-networking.md).</span></span>

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

6. <span data-ttu-id="73aef-159">Adja hozzá a hello nyilvános IP-cím erőforrás hello által használt terheléselosztó és a hálózati kapcsolat betöltése.</span><span class="sxs-lookup"><span data-stu-id="73aef-159">Add hello public IP address resources that are used by hello load balancer and network interface.</span></span>

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

7. <span data-ttu-id="73aef-160">Adja hozzá a hello méretezési készlet által használt hello terheléselosztó erőforrást.</span><span class="sxs-lookup"><span data-stu-id="73aef-160">Add hello load balancer resource that is used by hello scale set.</span></span> <span data-ttu-id="73aef-161">További információkért lásd: [Azure Resource Manager támogatása a terheléselosztó](../load-balancer/load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="73aef-161">For more information, see [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md).</span></span>

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
                "id": "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
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
              "backendPort": 22
            }
          }
        ]
      }
    },
    ```

8. <span data-ttu-id="73aef-162">Vegyen fel hello hálózati illesztő erőforrást hello különálló virtuális gép által használt.</span><span class="sxs-lookup"><span data-stu-id="73aef-162">Add hello network interface resource that is used by hello separate virtual machine.</span></span> <span data-ttu-id="73aef-163">Méretezési csoportban lévő gépek nem nyilvános IP-címnek keresztül érhető el, mert egy különálló virtuális gép jön létre hello azonos virtuális hálózati tooremotely hozzáférés hello gépek.</span><span class="sxs-lookup"><span data-stu-id="73aef-163">Because machines in a scale set aren't accessible through a public IP address, a separate virtual machine is created in hello same virtual network tooremotely access hello machines.</span></span>

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

9. <span data-ttu-id="73aef-164">Adja hozzá azonos hálózati hello méretezési beállított hello hello különálló virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="73aef-164">Add hello separate virtual machine in hello same network as hello scale set.</span></span>

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
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "14.04.4-LTS",
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

10. <span data-ttu-id="73aef-165">Hello virtuálisgép-méretezési csoport erőforrás hozzáadása, és adja meg a hello diagnosztika-bővítménnyel, amely hello méretezési csoportban lévő összes virtuális gép telepítve van.</span><span class="sxs-lookup"><span data-stu-id="73aef-165">Add hello virtual machine scale set resource and specify hello diagnostics extension that is installed on all virtual machines in hello scale set.</span></span> <span data-ttu-id="73aef-166">Ehhez az erőforráshoz hello beállításainak jelentős része hasonlóak a hello virtuálisgép-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="73aef-166">Many of hello settings for this resource are similar with hello virtual machine resource.</span></span> <span data-ttu-id="73aef-167">hello fő különbségek a következők: hello kapacitás elem hello méretezési csoportban lévő virtuális gépek hello számát megadó és upgradePolicy, amely meghatározza, hogyan frissítések válnak, toovirtual gépek.</span><span class="sxs-lookup"><span data-stu-id="73aef-167">hello main differences are hello capacity element that specifies hello number of virtual machines in hello scale set and upgradePolicy that specifies how updates are made toovirtual machines.</span></span> <span data-ttu-id="73aef-168">hello méretezési készlet nem jön létre az összes hello tárfiók létrehozásáig hello dependsOn elemmel megadottak szerint.</span><span class="sxs-lookup"><span data-stu-id="73aef-168">hello scale set is not created until all hello storage accounts are created as specified with hello dependsOn element.</span></span>

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
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4],'.blob.core.windows.net/vmss')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "Canonical",
              "offer": "UbuntuServer",
              "sku": "14.04.4-LTS",
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
                "name":"LinuxDiagnostic",
                "properties": {
                  "publisher":"Microsoft.OSTCExtensions",
                  "type":"LinuxDiagnostic",
                  "typeHandlerVersion":"2.1",
                  "autoUpgradeMinorVersion":false,
                  "settings": {
                    "xmlCfg":"[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount":"[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName":"[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey":"[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint":"https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. <span data-ttu-id="73aef-169">Vegyen fel hello autoscaleSettings erőforrást, amely meghatározza, hogyan hello méretezési processzor kihasználtsága hello gépek hello beállítása alapján állítja be.</span><span class="sxs-lookup"><span data-stu-id="73aef-169">Add hello autoscaleSettings resource that defines how hello scale set adjusts based on processor usage on hello machines in hello set.</span></span>

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
                  "metricName": "\\Processor\\PercentProcessorTime",
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
    
    <span data-ttu-id="73aef-170">Ebben az oktatóanyagban fontosak a ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="73aef-170">For this tutorial, these values are important:</span></span>
    
    * <span data-ttu-id="73aef-171">**metricName**</span><span class="sxs-lookup"><span data-stu-id="73aef-171">**metricName**</span></span>  
    <span data-ttu-id="73aef-172">Ez az érték azonos a hello wadperfcounter változóban meghatározott hello teljesítményszámláló van hello.</span><span class="sxs-lookup"><span data-stu-id="73aef-172">This value is hello same as hello performance counter that we defined in hello wadperfcounter variable.</span></span> <span data-ttu-id="73aef-173">Használja ezt a változót, hello diagnosztika bővítmény gyűjt hello **Processor\PercentProcessorTime** számláló.</span><span class="sxs-lookup"><span data-stu-id="73aef-173">Using that variable, hello Diagnostics extension collects hello **Processor\PercentProcessorTime** counter.</span></span>
    
    * <span data-ttu-id="73aef-174">**metricResourceUri**</span><span class="sxs-lookup"><span data-stu-id="73aef-174">**metricResourceUri**</span></span>  
    <span data-ttu-id="73aef-175">Ez az érték hello erőforrás-azonosítót a virtuálisgép-méretezési csoport hello.</span><span class="sxs-lookup"><span data-stu-id="73aef-175">This value is hello resource identifier of hello virtual machine scale set.</span></span>
    
    * <span data-ttu-id="73aef-176">**időkeretben vannak**</span><span class="sxs-lookup"><span data-stu-id="73aef-176">**timeGrain**</span></span>  
    <span data-ttu-id="73aef-177">Ez az érték hello metrikák összegyűjtött hello granularitása.</span><span class="sxs-lookup"><span data-stu-id="73aef-177">This value is hello granularity of hello metrics that are collected.</span></span> <span data-ttu-id="73aef-178">A sablon értéke tooone perc.</span><span class="sxs-lookup"><span data-stu-id="73aef-178">In this template, it is set tooone minute.</span></span>
    
    * <span data-ttu-id="73aef-179">**statisztika**</span><span class="sxs-lookup"><span data-stu-id="73aef-179">**statistic**</span></span>  
    <span data-ttu-id="73aef-180">Ez az érték határozza meg, hogyan hello adatok gyűjtése le kombinált tooaccommodate hello automatikus skálázás művelet.</span><span class="sxs-lookup"><span data-stu-id="73aef-180">This value determines how hello metrics are combined tooaccommodate hello automatic scaling action.</span></span> <span data-ttu-id="73aef-181">hello lehetséges értékek a következők: átlagos, Min, Max.</span><span class="sxs-lookup"><span data-stu-id="73aef-181">hello possible values are: Average, Min, Max.</span></span> <span data-ttu-id="73aef-182">Ez a sablon a hello átlagos teljes CPU-használat hello virtuális gépek gyűjti.</span><span class="sxs-lookup"><span data-stu-id="73aef-182">In this template, hello average total CPU usage of hello virtual machines is collected.</span></span>

    * <span data-ttu-id="73aef-183">**Az időtartomány értékének**</span><span class="sxs-lookup"><span data-stu-id="73aef-183">**timeWindow**</span></span>  
    <span data-ttu-id="73aef-184">Ez az érték, amely hello amelyben példány adatgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="73aef-184">This value is hello range of time in which instance data is collected.</span></span> <span data-ttu-id="73aef-185">5 perc és 12 óra között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="73aef-185">It must be between 5 minutes and 12 hours.</span></span>
    
    * <span data-ttu-id="73aef-186">**timeAggregation**</span><span class="sxs-lookup"><span data-stu-id="73aef-186">**timeAggregation**</span></span>  
    <span data-ttu-id="73aef-187">az érték szabja meg, hogyan hello adatokat gyűjtött össze kell vonni adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="73aef-187">his value determines how hello data that is collected should be combined over time.</span></span> <span data-ttu-id="73aef-188">hello alapértelmezett érték átlaga.</span><span class="sxs-lookup"><span data-stu-id="73aef-188">hello default value is Average.</span></span> <span data-ttu-id="73aef-189">hello lehetséges értékek a következők: átlagos, Minimum, Maximum, utolsó, összesen, száma.</span><span class="sxs-lookup"><span data-stu-id="73aef-189">hello possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span>
    
    * <span data-ttu-id="73aef-190">**operátor**</span><span class="sxs-lookup"><span data-stu-id="73aef-190">**operator**</span></span>  
    <span data-ttu-id="73aef-191">Ez az érték használt toocompare hello metrika adatok és hello küszöbérték, hello operátor.</span><span class="sxs-lookup"><span data-stu-id="73aef-191">This value is hello operator that is used toocompare hello metric data and hello threshold.</span></span> <span data-ttu-id="73aef-192">hello lehetséges értékek a következők: egyenlő NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="73aef-192">hello possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span>
    
    * <span data-ttu-id="73aef-193">**küszöbérték**</span><span class="sxs-lookup"><span data-stu-id="73aef-193">**threshold**</span></span>  
    <span data-ttu-id="73aef-194">Ez az érték hello skálázási műveletet váltja ki.</span><span class="sxs-lookup"><span data-stu-id="73aef-194">This value triggers hello scale action.</span></span> <span data-ttu-id="73aef-195">Ez a sablon a gépeket adnak toohello méretezési készletben, ha hello átlagos CPU-használat hello készlet gépek között több mint 50 %.</span><span class="sxs-lookup"><span data-stu-id="73aef-195">In this template, machines are added toohello scale set when hello average CPU usage among machines in hello set is over 50%.</span></span>
    
    * <span data-ttu-id="73aef-196">**iránya**</span><span class="sxs-lookup"><span data-stu-id="73aef-196">**direction**</span></span>  
    <span data-ttu-id="73aef-197">Ez az érték határozza meg a hello hello küszöbérték érhető el, amikor végrehajtandó műveletet.</span><span class="sxs-lookup"><span data-stu-id="73aef-197">This value determines hello action that is taken when hello threshold value is achieved.</span></span> <span data-ttu-id="73aef-198">hello lehetséges értékei növekedése vagy csökkenése.</span><span class="sxs-lookup"><span data-stu-id="73aef-198">hello possible values are Increase or Decrease.</span></span> <span data-ttu-id="73aef-199">Ez a sablon a hello hello méretezési csoportban lévő virtuális gépek száma nő, amikor több mint 50 %-a meghatározott hello időkerete hello küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="73aef-199">In this template, hello number of virtual machines in hello scale set is increased if hello threshold is over 50% in hello defined time window.</span></span>

    * <span data-ttu-id="73aef-200">**típusa**</span><span class="sxs-lookup"><span data-stu-id="73aef-200">**type**</span></span>  
    <span data-ttu-id="73aef-201">Ez az érték egy végrehajtandó műveletet kell végrehajtani, és be kell állítani tooChangeCount hello típusú.</span><span class="sxs-lookup"><span data-stu-id="73aef-201">This value is hello type of action that should occur and must be set tooChangeCount.</span></span>
    
    * <span data-ttu-id="73aef-202">**érték**</span><span class="sxs-lookup"><span data-stu-id="73aef-202">**value**</span></span>  
    <span data-ttu-id="73aef-203">Ez az érték hello hozzáadásakor vagy eltávolításakor hello méretezési csoport a virtuális gépek száma.</span><span class="sxs-lookup"><span data-stu-id="73aef-203">This value is hello number of virtual machines that are added or removed from hello scale set.</span></span> <span data-ttu-id="73aef-204">Ez az érték 1 vagy nagyobb lehet.</span><span class="sxs-lookup"><span data-stu-id="73aef-204">This value must be 1 or greater.</span></span> <span data-ttu-id="73aef-205">hello alapértelmezett értéke 1.</span><span class="sxs-lookup"><span data-stu-id="73aef-205">hello default value is 1.</span></span> <span data-ttu-id="73aef-206">A sablonban lévő hello méretezési gépek hello száma növekszik helyezésekor által beállított 1 hello küszöbérték elérése.</span><span class="sxs-lookup"><span data-stu-id="73aef-206">In this template, hello number of machines in hello scale set increases by 1 when hello threshold is met.</span></span>

    * <span data-ttu-id="73aef-207">**cooldown**</span><span class="sxs-lookup"><span data-stu-id="73aef-207">**cooldown**</span></span>  
    <span data-ttu-id="73aef-208">Ez az érték érték hello idő toowait óta hello utolsó méretezési művelet előtt hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="73aef-208">This value is hello amount of time toowait since hello last scaling action before hello next action occurs.</span></span> <span data-ttu-id="73aef-209">Ez az érték egy percet, és egy hét között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="73aef-209">This value must be between one minute and one week.</span></span>

12. <span data-ttu-id="73aef-210">Hello sablonfájl mentési.</span><span class="sxs-lookup"><span data-stu-id="73aef-210">Save hello template file.</span></span>    

## <a name="step-3-upload-hello-template-toostorage"></a><span data-ttu-id="73aef-211">3. lépés: Hello sablon toostorage feltöltése</span><span class="sxs-lookup"><span data-stu-id="73aef-211">Step 3: Upload hello template toostorage</span></span>
<span data-ttu-id="73aef-212">hello sablon mindaddig, amíg hello nevét és az 1. lépésben létrehozott hello tárfiók elsődleges kulcs ismeri fel kell tölteni.</span><span class="sxs-lookup"><span data-stu-id="73aef-212">hello template can be uploaded as long as you know hello name and primary key of hello storage account that you created in step 1.</span></span>

1. <span data-ttu-id="73aef-213">A parancssori felület (Bash, terminál, parancssor) futtassa az alábbi parancsokat tooset hello környezeti változók szükséges tooaccess hello storage-fiók:</span><span class="sxs-lookup"><span data-stu-id="73aef-213">In your command-line interface (Bash, Terminal, Command prompt), run these commands tooset hello environment variables needed tooaccess hello storage account:</span></span>

    ```cli   
    export AZURE_STORAGE_ACCOUNT={account_name}
    export AZURE_STORAGE_ACCESS_KEY={key}
    ```
    
    <span data-ttu-id="73aef-214">Hello kulcs kaphat hello kulcs ikonra kattintva, amikor megtekintik hello tárolási fiók erőforrás hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="73aef-214">You can get hello key by clicking hello key icon when viewing hello storage account resource in hello Azure portal.</span></span> <span data-ttu-id="73aef-215">Ha egy Windows parancssori ablakba, írja be a **beállítása** exportálása helyett.</span><span class="sxs-lookup"><span data-stu-id="73aef-215">When using a Windows command prompt, type **set** instead of export.</span></span>

2. <span data-ttu-id="73aef-216">Az hello sablon tárolására alkalmas hello tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="73aef-216">Create hello container for storing hello template.</span></span>
   
    ```cli
    azure storage container create -p Blob templates
    ```

3. <span data-ttu-id="73aef-217">Töltse fel a hello sablon fájl toohello új tárolót.</span><span class="sxs-lookup"><span data-stu-id="73aef-217">Upload hello template file toohello new container.</span></span>
   
    ```cli
    azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json
    ```

## <a name="step-4-deploy-hello-template"></a><span data-ttu-id="73aef-218">4. lépés: Hello sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="73aef-218">Step 4: Deploy hello template</span></span>
<span data-ttu-id="73aef-219">Most, hogy létrehozta a hello sablon, megkezdheti a hello erőforrásokat üzembe helyezi.</span><span class="sxs-lookup"><span data-stu-id="73aef-219">Now that you created hello template, you can start deploying hello resources.</span></span> <span data-ttu-id="73aef-220">A parancs toostart hello folyamat használja:</span><span class="sxs-lookup"><span data-stu-id="73aef-220">Use this command toostart hello process:</span></span>

```cli
azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1
```

<span data-ttu-id="73aef-221">Amikor lenyomja az adja meg, a hozzárendelt hello változók felszólító tooprovide értékei lesznek.</span><span class="sxs-lookup"><span data-stu-id="73aef-221">When you press enter, you are prompted tooprovide values for hello variables you assigned.</span></span> <span data-ttu-id="73aef-222">Adja meg ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="73aef-222">Provide these values:</span></span>

```cli
vmName: vmsstestvm1
vmSSName: vmsstest1
instanceCount: 5
adminUserName: vmadmin1
adminPassword: VMpass1
resourcePrefix: vmsstest
```

<span data-ttu-id="73aef-223">Mintegy 15 percre leáll az összes hello erőforrások toosuccessfully telepíteni kell vennie.</span><span class="sxs-lookup"><span data-stu-id="73aef-223">It should take about 15 minutes for all hello resources toosuccessfully be deployed.</span></span>

> [!NOTE]
> <span data-ttu-id="73aef-224">Hello portal képességét toodeploy hello erőforrásokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="73aef-224">You can also use hello portal’s ability toodeploy hello resources.</span></span> <span data-ttu-id="73aef-225">A hivatkozás: https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template></span><span class="sxs-lookup"><span data-stu-id="73aef-225">Use this link: https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template></span></span>


## <a name="step-5-monitor-resources"></a><span data-ttu-id="73aef-226">5. lépés: A figyelő erőforrások</span><span class="sxs-lookup"><span data-stu-id="73aef-226">Step 5: Monitor resources</span></span>
<span data-ttu-id="73aef-227">Virtuálisgép-méretezési csoportok ezekkel a módszerekkel kapcsolatos információkat kaphat:</span><span class="sxs-lookup"><span data-stu-id="73aef-227">You can get some information about virtual machine scale sets using these methods:</span></span>

* <span data-ttu-id="73aef-228">Azure-portálon hello – jelenleg korlátozott mennyiségű információt hello portál használatával kaphat.</span><span class="sxs-lookup"><span data-stu-id="73aef-228">hello Azure portal - You can currently get a limited amount of information using hello portal.</span></span>

* <span data-ttu-id="73aef-229">Hello [Azure erőforrás-kezelő](https://resources.azure.com/) -Ez az eszköz felderítését lehetővé tevő hello aktuális állapotát, a méretezési legjobb hello.</span><span class="sxs-lookup"><span data-stu-id="73aef-229">hello [Azure Resource Explorer](https://resources.azure.com/) - This tool is hello best for exploring hello current state of your scale set.</span></span> <span data-ttu-id="73aef-230">Hajtsa végre ezt az elérési utat, és megtekintheti az hello példányait tartalmazó nézetet hello méretezési beállítása létrehozott:</span><span class="sxs-lookup"><span data-stu-id="73aef-230">Follow this path and you should see hello instance view of hello scale set that you created:</span></span>
  
    ```cli
    subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines
    ```

* <span data-ttu-id="73aef-231">Az Azure CLI - használja a parancs tooget néhány információt:</span><span class="sxs-lookup"><span data-stu-id="73aef-231">Azure CLI - Use this command tooget some information:</span></span>

    ```cli  
    azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1
    ```

* <span data-ttu-id="73aef-232">Csatlakoztassa a toohello jumpbox virtuális gépet, ugyanúgy, mint bármely más gép üzembe helyezése, és ezután távolról elérheti hello virtuális gépek hello méretezési készlet toomonitor az egyes folyamatok.</span><span class="sxs-lookup"><span data-stu-id="73aef-232">Connect toohello jumpbox virtual machine just like you would any other machine and then you can remotely access hello virtual machines in hello scale set toomonitor individual processes.</span></span>

> [!NOTE]
> <span data-ttu-id="73aef-233">A teljes REST API-t a méretezési készlet vonatkozó adatok találhatók [virtuálisgép-skálázási készletekben](https://msdn.microsoft.com/library/mt589023.aspx).</span><span class="sxs-lookup"><span data-stu-id="73aef-233">A complete REST API for obtaining information about scale sets can be found in [Virtual Machine Scale Sets](https://msdn.microsoft.com/library/mt589023.aspx).</span></span>

## <a name="step-6-remove-hello-resources"></a><span data-ttu-id="73aef-234">6. lépés: Hello erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="73aef-234">Step 6: Remove hello resources</span></span>
<span data-ttu-id="73aef-235">Mivel van szó, az Azure-ban használt erőforrásokhoz, még mindig egy célszerű toodelete erőforrásokat, amelyek már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="73aef-235">Because you are charged for resources used in Azure, it is always a good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="73aef-236">Nincs szükség a toodelete az egyes erőforrások elkülönítve egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="73aef-236">You don’t need toodelete each resource separately from a resource group.</span></span> <span data-ttu-id="73aef-237">Hello erőforráscsoport törlése és a hozzá tartozó összes erőforrásnak automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="73aef-237">You can delete hello resource group and all its resources are automatically deleted.</span></span>

```cli
azure group delete vmsstestrg1
```

## <a name="next-steps"></a><span data-ttu-id="73aef-238">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73aef-238">Next steps</span></span>
* <span data-ttu-id="73aef-239">Példák figyelési szolgáltatásokat az Azure-figyelő található [figyelő Azure platformfüggetlen parancssori felület gyors start minták](../monitoring-and-diagnostics/insights-cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="73aef-239">Find examples of Azure Monitor monitoring features in [Azure Monitor Cross-platform CLI quick start samples](../monitoring-and-diagnostics/insights-cli-samples.md)</span></span>
* <span data-ttu-id="73aef-240">További tudnivalók az értesítési szolgáltatások [automatikus skálázási műveletek toosend e-mailek és a webhook riasztási értesítések használata az Azure-figyelő](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="73aef-240">Learn about notification features in [Use autoscale actions toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span></span>
* <span data-ttu-id="73aef-241">Ismerje meg, hogyan túl[használata auditnaplókat toosend e-mailek és a webhook riasztási értesítések az Azure-figyelő](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="73aef-241">Learn how too[Use audit logs toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>
* <span data-ttu-id="73aef-242">Tekintse meg a hello [automatikus skálázás bemutató alkalmazás Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) sablont, amely beállítja a Python/bottle app tooexercise hello az automatikus skálázás működésének virtuálisgép-skálázási készletekben.</span><span class="sxs-lookup"><span data-stu-id="73aef-242">Check out hello [Autoscale demo app on Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) template that sets up a Python/bottle app tooexercise hello automatic scaling functionality of Virtual Machine Scale Sets.</span></span>

