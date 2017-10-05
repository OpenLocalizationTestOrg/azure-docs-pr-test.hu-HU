---
title: "Automatikus skálázás Linux virtuálisgép-skálázási készletekben |} Microsoft Docs"
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
ms.openlocfilehash: eff4add1cb16fe25022787668dc1d2277845dd95
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="automatically-scale-linux-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="0db95-103">Automatikus méretezése a virtuálisgép-méretezési csoportban lévő Linux-gépek</span><span class="sxs-lookup"><span data-stu-id="0db95-103">Automatically scale Linux machines in a virtual machine scale set</span></span>
<span data-ttu-id="0db95-104">Virtuálisgép-méretezési csoportok könnyebben akkor helyezheti üzembe és felügyelheti az azonos virtuális gépek készletként.</span><span class="sxs-lookup"><span data-stu-id="0db95-104">Virtual machine scale sets make it easy for you to deploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="0db95-105">Méretezési csoportok hyperscale alkalmazások jól méretezhető és testre szabható számítási rétegét adja meg, és a Windows platform-lemezképek, Linux platformon képek, egyéni lemezképek és bővítmények támogatják.</span><span class="sxs-lookup"><span data-stu-id="0db95-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="0db95-106">További tudnivalókért lásd: [virtuális gépek méretezési készletek áttekintése](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0db95-106">To learn more, see [Virtual Machine Scale Sets Overview](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="0db95-107">Az oktatóanyag bemutatja, hogyan Ubuntu Linux legújabb verzióját használja a Linux virtuális gépek méretezési csoportjának létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0db95-107">This tutorial shows you how to create a scale set of Linux virtual machines using the latest version of Ubuntu Linux.</span></span> <span data-ttu-id="0db95-108">Az oktatóanyag is bemutatja, hogyan automatikus méretezése a gépek készletében.</span><span class="sxs-lookup"><span data-stu-id="0db95-108">The tutorial also shows you how to automatically scale the machines in the set.</span></span> <span data-ttu-id="0db95-109">A skála, állíthatók be az Azure Resource Manager-sablon létrehozása és telepítése az Azure parancssori felület használatával méretezhetővé hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0db95-109">You create the scale set and set up scaling by creating an Azure Resource Manager template and deploying it using Azure CLI.</span></span> <span data-ttu-id="0db95-110">A sablonokról további információkat az [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) (Azure Resource Manager-sablonok készítése) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="0db95-110">For more information about templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="0db95-111">Méretezési készlet automatikus skálázás kapcsolatos további információkért lásd: [automatikus skálázás és virtuálisgép-skálázási készletekben](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0db95-111">To learn more about automatic scaling of scale sets, see [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

<span data-ttu-id="0db95-112">Ebben az oktatóanyagban telepít, a következő erőforrások és bővítmények:</span><span class="sxs-lookup"><span data-stu-id="0db95-112">In this tutorial, you deploy the following resources and extensions:</span></span>

* <span data-ttu-id="0db95-113">Microsoft.Storage/storageAccounts</span><span class="sxs-lookup"><span data-stu-id="0db95-113">Microsoft.Storage/storageAccounts</span></span>
* <span data-ttu-id="0db95-114">Microsoft.Network/virtualNetworks</span><span class="sxs-lookup"><span data-stu-id="0db95-114">Microsoft.Network/virtualNetworks</span></span>
* <span data-ttu-id="0db95-115">Microsoft.Network/publicIPAddresses</span><span class="sxs-lookup"><span data-stu-id="0db95-115">Microsoft.Network/publicIPAddresses</span></span>
* <span data-ttu-id="0db95-116">Microsoft.Network/loadBalancers</span><span class="sxs-lookup"><span data-stu-id="0db95-116">Microsoft.Network/loadBalancers</span></span>
* <span data-ttu-id="0db95-117">Microsoft.Network/networkInterfaces</span><span class="sxs-lookup"><span data-stu-id="0db95-117">Microsoft.Network/networkInterfaces</span></span>
* <span data-ttu-id="0db95-118">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="0db95-118">Microsoft.Compute/virtualMachines</span></span>
* <span data-ttu-id="0db95-119">Microsoft.Compute/virtualMachineScaleSets</span><span class="sxs-lookup"><span data-stu-id="0db95-119">Microsoft.Compute/virtualMachineScaleSets</span></span>
* <span data-ttu-id="0db95-120">Microsoft.Insights.VMDiagnosticsSettings</span><span class="sxs-lookup"><span data-stu-id="0db95-120">Microsoft.Insights.VMDiagnosticsSettings</span></span>
* <span data-ttu-id="0db95-121">Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="0db95-121">Microsoft.Insights/autoscaleSettings</span></span>

<span data-ttu-id="0db95-122">Erőforrás-kezelő erőforrásokra vonatkozó további információkért lásd: [Azure Resource Manager és klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="0db95-122">For more information about Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

<span data-ttu-id="0db95-123">Mielőtt hozzáfogna a lépésekről, ebben az oktatóanyagban [az Azure parancssori felület telepítése](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="0db95-123">Before you get started with the steps in this tutorial, [install the Azure CLI](../cli-install-nodejs.md).</span></span>

## <a name="step-1-create-a-resource-group-and-a-storage-account"></a><span data-ttu-id="0db95-124">1. lépés: Az erőforráscsoportot és a storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="0db95-124">Step 1: Create a resource group and a storage account</span></span>

1. <span data-ttu-id="0db95-125">**Jelentkezzen be a Microsoft Azure**</span><span class="sxs-lookup"><span data-stu-id="0db95-125">**Sign in to Microsoft Azure**</span></span>  
<span data-ttu-id="0db95-126">A parancssori felület (Bash, terminál, parancssor), váltson a Resource Manager módra, majd [jelentkezzen be a munkahelyi vagy iskolai azonosítóját](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Kövesse az utasításokat egy interaktív bejelentkezési élmény a Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="0db95-126">In your command-line interface (Bash, Terminal, Command prompt), switch to Resource Manager mode, and then [log in with your work or school id](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Follow the prompts for an interactive login experience to your Azure account.</span></span>

    ```cli   
    azure config mode arm

    azure login
    ```
   
    > [!NOTE]
    > <span data-ttu-id="0db95-127">Ha rendelkezik munkahelyi vagy iskolai azonosítója, és nem kéttényezős hitelesítést, használja `azure login -u` nélkül egy interaktív munkamenet bejelentkezési azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="0db95-127">If you have a work or school ID and you do not have two-factor authentication enabled, use `azure login -u` with the ID to log in without an interactive session.</span></span> <span data-ttu-id="0db95-128">Ha nem rendelkezik munkahelyi vagy iskolai azonosítója, akkor [munkahelyi vagy iskolai azonosító létrehozása a személyes Microsoft-fiókhoz tartozó](../active-directory/active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0db95-128">If you don't have a work or school ID, you can [create a work or school id from your personal Microsoft account](../active-directory/active-directory-users-create-azure-portal.md).</span></span>
    
2. <span data-ttu-id="0db95-129">**Hozzon létre egy erőforráscsoportot**</span><span class="sxs-lookup"><span data-stu-id="0db95-129">**Create a resource group**</span></span>  
<span data-ttu-id="0db95-130">Minden erőforrás telepíteni kell egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="0db95-130">All resources must be deployed to a resource group.</span></span> <span data-ttu-id="0db95-131">Ebben az oktatóanyagban nevezze el az erőforráscsoportot **vmsstest1**.</span><span class="sxs-lookup"><span data-stu-id="0db95-131">For this tutorial, name the resource group **vmsstest1**.</span></span>
   
    ```cli
    azure group create vmsstestrg1 centralus
    ```

3. <span data-ttu-id="0db95-132">**A storage-fiók üzembe helyezés az új erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="0db95-132">**Deploy a storage account into the new resource group**</span></span>  
<span data-ttu-id="0db95-133">Ez a tárfiók, a sablon tárolására.</span><span class="sxs-lookup"><span data-stu-id="0db95-133">This storage account is where the template is stored.</span></span> <span data-ttu-id="0db95-134">Hozzon létre egy tárfiókot, nevű **vmsstestsa**.</span><span class="sxs-lookup"><span data-stu-id="0db95-134">Create a storage account named **vmsstestsa**.</span></span>
   
    ```cli
    azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa
    ```

## <a name="step-2-create-the-template"></a><span data-ttu-id="0db95-135">2. lépés: A sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="0db95-135">Step 2: Create the template</span></span>
<span data-ttu-id="0db95-136">Az Azure Resource Manager-sablon általi telepítéséhez és az Azure-erőforrások együtt kezelheti az erőforrásokat és a társított telepítési paraméterek JSON-leírásuk lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="0db95-136">An Azure Resource Manager template makes it possible for you to deploy and manage Azure resources together by using a JSON description of the resources and associated deployment parameters.</span></span>

1. <span data-ttu-id="0db95-137">A kedvenc szerkesztőben VMSSTemplate.json létrehozásához, és adja hozzá a sablon támogatásához a kezdeti JSON struktúrában.</span><span class="sxs-lookup"><span data-stu-id="0db95-137">In your favorite editor, create the file VMSSTemplate.json and add the initial JSON structure to support the template.</span></span>

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

2. <span data-ttu-id="0db95-138">Paraméterek nem mindig szükséges, de azok nyújtanak olyan értéket adjon meg a sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="0db95-138">Parameters are not always required, but they provide a way to input values when the template is deployed.</span></span> <span data-ttu-id="0db95-139">Adja hozzá ezeket a paramétereket a paraméterek szülőelem hozzáadott, a sablon alapján.</span><span class="sxs-lookup"><span data-stu-id="0db95-139">Add these parameters under the parameters parent element that you added to the template.</span></span>

    ```json
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
   * <span data-ttu-id="0db95-140">Állítsa be a külön, a méretezési gépek eléréséhez használt virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="0db95-140">A name for the separate virtual machine that is used to access the machines in the scale set.</span></span>
   * <span data-ttu-id="0db95-141">A tárfiók, amelyen megtalálható a sablon nevét.</span><span class="sxs-lookup"><span data-stu-id="0db95-141">A name for the storage account where the template is stored.</span></span>
   * <span data-ttu-id="0db95-142">Állítsa be a virtuális gépek létrehozásához először a skála példányainak száma.</span><span class="sxs-lookup"><span data-stu-id="0db95-142">The number of instances of virtual machines to initially create in the scale set.</span></span>
   * <span data-ttu-id="0db95-143">Egy nevet és jelszót a rendszergazdai fiók, a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="0db95-143">A name and password of the administrator account on the virtual machines.</span></span>
   * <span data-ttu-id="0db95-144">Az erőforrások, a méretezési támogatásához létrehozott előtagja.</span><span class="sxs-lookup"><span data-stu-id="0db95-144">A name prefix for the resources that are created to support the scale set.</span></span>

3. <span data-ttu-id="0db95-145">Változók segítségével sablonban adja meg az értékeket, amelyeket gyakran változhat vagy értékeket, amelyeket a paraméterértékek kombinációjából kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0db95-145">Variables can be used in a template to specify values that may change frequently or values that need to be created from a combination of parameter values.</span></span> <span data-ttu-id="0db95-146">Adja hozzá ezeket a változókat a változók szülőelem hozzáadott, a sablon alapján.</span><span class="sxs-lookup"><span data-stu-id="0db95-146">Add these variables under the variables parent element that you added to the template.</span></span>

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

   * <span data-ttu-id="0db95-147">A hálózati adapterek által használt DNS-neveket.</span><span class="sxs-lookup"><span data-stu-id="0db95-147">DNS names that are used by the network interfaces.</span></span>
   * <span data-ttu-id="0db95-148">Az IP-címet nevének és a virtuális hálózati és-alhálózatok előtagokat.</span><span class="sxs-lookup"><span data-stu-id="0db95-148">The IP address names and prefixes for the virtual network and subnets.</span></span>
   * <span data-ttu-id="0db95-149">A nevek és a virtuális hálózat azonosítók terheléselosztó és a hálózati adapterek betöltése.</span><span class="sxs-lookup"><span data-stu-id="0db95-149">The names and identifiers of the virtual network, load balancer, and network interfaces.</span></span>
   * <span data-ttu-id="0db95-150">Állítsa be a fiókok a skála gépén társított tárfiókok neve.</span><span class="sxs-lookup"><span data-stu-id="0db95-150">Storage account names for the accounts associated with the machines in the scale set.</span></span>
   * <span data-ttu-id="0db95-151">A diagnosztika bővítményt a virtuális gépeken telepített beállításait.</span><span class="sxs-lookup"><span data-stu-id="0db95-151">Settings for the Diagnostics extension that is installed on the virtual machines.</span></span> <span data-ttu-id="0db95-152">A diagnosztika bővítmény kapcsolatos további információkért lásd: [Windows virtuális gép létrehozása a figyelési és -diagnosztika Azure Resource Manager-sablon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0db95-152">For more information about the Diagnostics extension, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

4. <span data-ttu-id="0db95-153">Adja hozzá a tárolási fiók erőforrások az erőforrások szülőelem hozzáadott, a sablon alapján.</span><span class="sxs-lookup"><span data-stu-id="0db95-153">Add the storage account resource under the resources parent element that you added to the template.</span></span> <span data-ttu-id="0db95-154">Ez a sablon használatával hurkot ajánlott öt tárfiókok létrehozása az operációs rendszer lemezek és diagnosztikai adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="0db95-154">This template uses a loop to create the recommended five storage accounts where the operating system disks and diagnostic data are stored.</span></span> <span data-ttu-id="0db95-155">E fiókok csoportja, amelyek a jelenlegi maximális méretezési csoportban lévő támogathatja a legfeljebb 100 virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="0db95-155">This set of accounts can support up to 100 virtual machines in a scale set, which is the current maximum.</span></span> <span data-ttu-id="0db95-156">Minden tárfiók a utótagot, amelyet a sablon a paraméterek meg együtt változók definiált levél jelzéssel neve.</span><span class="sxs-lookup"><span data-stu-id="0db95-156">Each storage account is named with a letter designator that was defined in the variables combined with the suffix that you provide in the parameters for the template.</span></span>
   
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

5. <span data-ttu-id="0db95-157">Adja hozzá a virtuális hálózati erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="0db95-157">Add the virtual network resource.</span></span> <span data-ttu-id="0db95-158">További információkért lásd: [hálózati erőforrás-szolgáltató](../virtual-network/resource-groups-networking.md).</span><span class="sxs-lookup"><span data-stu-id="0db95-158">For more information, see [Network Resource Provider](../virtual-network/resource-groups-networking.md).</span></span>

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

6. <span data-ttu-id="0db95-159">Adja hozzá a nyilvános IP-cím erőforrás a terheléselosztó és a hálózati adapter által használt.</span><span class="sxs-lookup"><span data-stu-id="0db95-159">Add the public IP address resources that are used by the load balancer and network interface.</span></span>

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

7. <span data-ttu-id="0db95-160">Adja hozzá a terheléselosztó erőforrást a méretezési készlet által használt.</span><span class="sxs-lookup"><span data-stu-id="0db95-160">Add the load balancer resource that is used by the scale set.</span></span> <span data-ttu-id="0db95-161">További információkért lásd: [Azure Resource Manager támogatása a terheléselosztó](../load-balancer/load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="0db95-161">For more information, see [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md).</span></span>

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

8. <span data-ttu-id="0db95-162">Vegye fel a különálló virtuális gép által használt hálózati illesztő erőforrás.</span><span class="sxs-lookup"><span data-stu-id="0db95-162">Add the network interface resource that is used by the separate virtual machine.</span></span> <span data-ttu-id="0db95-163">Méretezési csoportban lévő gépek nem nyilvános IP-címnek keresztül érhető el, mert egy különálló virtuális gép jön létre az azonos virtuális hálózatban való távoli hozzáférés a gépek.</span><span class="sxs-lookup"><span data-stu-id="0db95-163">Because machines in a scale set aren't accessible through a public IP address, a separate virtual machine is created in the same virtual network to remotely access the machines.</span></span>

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

9. <span data-ttu-id="0db95-164">Vegye fel a különálló virtuális gép ugyanazon a hálózaton, a méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="0db95-164">Add the separate virtual machine in the same network as the scale set.</span></span>

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

10. <span data-ttu-id="0db95-165">A virtuálisgép-méretezési csoport erőforrás hozzáadása, és adja meg a diagnosztika-bővítménnyel, a méretezési csoportban lévő összes virtuális gép telepítve van.</span><span class="sxs-lookup"><span data-stu-id="0db95-165">Add the virtual machine scale set resource and specify the diagnostics extension that is installed on all virtual machines in the scale set.</span></span> <span data-ttu-id="0db95-166">Az erőforrás beállításainak jelentős része hasonlóak a virtuálisgép-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="0db95-166">Many of the settings for this resource are similar with the virtual machine resource.</span></span> <span data-ttu-id="0db95-167">A fő különbség a kapacitás elemhez tartozó virtuális gépek számát határozza meg a méretezési és upgradePolicy, amely meghatározza, hogyan végrehajtott frissítések virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="0db95-167">The main differences are the capacity element that specifies the number of virtual machines in the scale set and upgradePolicy that specifies how updates are made to virtual machines.</span></span> <span data-ttu-id="0db95-168">A méretezési készlet nem jön létre, amíg a storage-fiókok létrehozásához szükségesek a dependsOn elemmel megadottak szerint.</span><span class="sxs-lookup"><span data-stu-id="0db95-168">The scale set is not created until all the storage accounts are created as specified with the dependsOn element.</span></span>

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

11. <span data-ttu-id="0db95-169">Vegyen fel a autoscaleSettings erőforrást, amely meghatározza, hogyan a méretezési beállítja a készlet gépeken processzorhasználat alapján.</span><span class="sxs-lookup"><span data-stu-id="0db95-169">Add the autoscaleSettings resource that defines how the scale set adjusts based on processor usage on the machines in the set.</span></span>

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
    
    <span data-ttu-id="0db95-170">Ebben az oktatóanyagban fontosak a ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="0db95-170">For this tutorial, these values are important:</span></span>
    
    * <span data-ttu-id="0db95-171">**metricName**</span><span class="sxs-lookup"><span data-stu-id="0db95-171">**metricName**</span></span>  
    <span data-ttu-id="0db95-172">Ez az érték megegyezik a wadperfcounter változóban meghatározott teljesítményszámláló.</span><span class="sxs-lookup"><span data-stu-id="0db95-172">This value is the same as the performance counter that we defined in the wadperfcounter variable.</span></span> <span data-ttu-id="0db95-173">Használja ezt a változót, a diagnosztika bővítmény gyűjti a **Processor\PercentProcessorTime** számláló.</span><span class="sxs-lookup"><span data-stu-id="0db95-173">Using that variable, the Diagnostics extension collects the **Processor\PercentProcessorTime** counter.</span></span>
    
    * <span data-ttu-id="0db95-174">**metricResourceUri**</span><span class="sxs-lookup"><span data-stu-id="0db95-174">**metricResourceUri**</span></span>  
    <span data-ttu-id="0db95-175">Ezt az értéket az erőforrás-azonosítót a virtuálisgép-méretezési csoport.</span><span class="sxs-lookup"><span data-stu-id="0db95-175">This value is the resource identifier of the virtual machine scale set.</span></span>
    
    * <span data-ttu-id="0db95-176">**időkeretben vannak**</span><span class="sxs-lookup"><span data-stu-id="0db95-176">**timeGrain**</span></span>  
    <span data-ttu-id="0db95-177">Ez az érték a metrikák összegyűjtött granularitása.</span><span class="sxs-lookup"><span data-stu-id="0db95-177">This value is the granularity of the metrics that are collected.</span></span> <span data-ttu-id="0db95-178">Ez a sablon az érték egy perc alatt.</span><span class="sxs-lookup"><span data-stu-id="0db95-178">In this template, it is set to one minute.</span></span>
    
    * <span data-ttu-id="0db95-179">**statisztika**</span><span class="sxs-lookup"><span data-stu-id="0db95-179">**statistic**</span></span>  
    <span data-ttu-id="0db95-180">Ez az érték határozza meg, hogyan a metrikák egyesíti a rendszer megfeleljen az automatikus skálázási művelet.</span><span class="sxs-lookup"><span data-stu-id="0db95-180">This value determines how the metrics are combined to accommodate the automatic scaling action.</span></span> <span data-ttu-id="0db95-181">A lehetséges értékek: átlagos, Min, Max.</span><span class="sxs-lookup"><span data-stu-id="0db95-181">The possible values are: Average, Min, Max.</span></span> <span data-ttu-id="0db95-182">A sablonban a virtuális gépek átlagos teljes processzorhasználatot gyűjti.</span><span class="sxs-lookup"><span data-stu-id="0db95-182">In this template, the average total CPU usage of the virtual machines is collected.</span></span>

    * <span data-ttu-id="0db95-183">**Az időtartomány értékének**</span><span class="sxs-lookup"><span data-stu-id="0db95-183">**timeWindow**</span></span>  
    <span data-ttu-id="0db95-184">Ez az érték, amely a, amelyben példány adatgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="0db95-184">This value is the range of time in which instance data is collected.</span></span> <span data-ttu-id="0db95-185">5 perc és 12 óra között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0db95-185">It must be between 5 minutes and 12 hours.</span></span>
    
    * <span data-ttu-id="0db95-186">**timeAggregation**</span><span class="sxs-lookup"><span data-stu-id="0db95-186">**timeAggregation**</span></span>  
    <span data-ttu-id="0db95-187">az érték határozza meg, hogyan kell-e a gyűjtött adatok kombinált adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="0db95-187">his value determines how the data that is collected should be combined over time.</span></span> <span data-ttu-id="0db95-188">Alapértelmezett érték átlaga.</span><span class="sxs-lookup"><span data-stu-id="0db95-188">The default value is Average.</span></span> <span data-ttu-id="0db95-189">A lehetséges értékek: átlagos, Minimum, Maximum, utolsó, összesen, száma.</span><span class="sxs-lookup"><span data-stu-id="0db95-189">The possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span>
    
    * <span data-ttu-id="0db95-190">**operátor**</span><span class="sxs-lookup"><span data-stu-id="0db95-190">**operator**</span></span>  
    <span data-ttu-id="0db95-191">Az értéke az operátort, amelynek segítségével összehasonlíthatja a metrikaadatokat és a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="0db95-191">This value is the operator that is used to compare the metric data and the threshold.</span></span> <span data-ttu-id="0db95-192">A lehetséges értékek: egyenlő NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="0db95-192">The possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span>
    
    * <span data-ttu-id="0db95-193">**küszöbérték**</span><span class="sxs-lookup"><span data-stu-id="0db95-193">**threshold**</span></span>  
    <span data-ttu-id="0db95-194">Ez az érték elindítja a műveletet.</span><span class="sxs-lookup"><span data-stu-id="0db95-194">This value triggers the scale action.</span></span> <span data-ttu-id="0db95-195">Ez a sablon a gépeket adnak a méretezési készletben, ha a készlet gépek között az átlagos CPU-használat több mint 50 %.</span><span class="sxs-lookup"><span data-stu-id="0db95-195">In this template, machines are added to the scale set when the average CPU usage among machines in the set is over 50%.</span></span>
    
    * <span data-ttu-id="0db95-196">**iránya**</span><span class="sxs-lookup"><span data-stu-id="0db95-196">**direction**</span></span>  
    <span data-ttu-id="0db95-197">Ez az érték határozza meg, amikor érhető el, a küszöbérték végrehajtandó műveletet.</span><span class="sxs-lookup"><span data-stu-id="0db95-197">This value determines the action that is taken when the threshold value is achieved.</span></span> <span data-ttu-id="0db95-198">A lehetséges értékek a következők: növekedése vagy csökkenése.</span><span class="sxs-lookup"><span data-stu-id="0db95-198">The possible values are Increase or Decrease.</span></span> <span data-ttu-id="0db95-199">A sablonban a méretezési csoportban lévő virtuális gépek száma nő, amikor több mint 50 %-a meghatározott idő ablakban a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="0db95-199">In this template, the number of virtual machines in the scale set is increased if the threshold is over 50% in the defined time window.</span></span>

    * <span data-ttu-id="0db95-200">**típusa**</span><span class="sxs-lookup"><span data-stu-id="0db95-200">**type**</span></span>  
    <span data-ttu-id="0db95-201">Ez az érték műveletet kell végrehajtani, és ChangeCount értékre kell állítani.</span><span class="sxs-lookup"><span data-stu-id="0db95-201">This value is the type of action that should occur and must be set to ChangeCount.</span></span>
    
    * <span data-ttu-id="0db95-202">**érték**</span><span class="sxs-lookup"><span data-stu-id="0db95-202">**value**</span></span>  
    <span data-ttu-id="0db95-203">Ez az érték hozzáadásakor vagy eltávolításakor a méretezési csoport a virtuális gépek száma.</span><span class="sxs-lookup"><span data-stu-id="0db95-203">This value is the number of virtual machines that are added or removed from the scale set.</span></span> <span data-ttu-id="0db95-204">Ez az érték 1 vagy nagyobb lehet.</span><span class="sxs-lookup"><span data-stu-id="0db95-204">This value must be 1 or greater.</span></span> <span data-ttu-id="0db95-205">Az alapértelmezett értéke 1.</span><span class="sxs-lookup"><span data-stu-id="0db95-205">The default value is 1.</span></span> <span data-ttu-id="0db95-206">Ez a sablon a méretezési gépek számát be növekszik 1 amikor teljesül a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="0db95-206">In this template, the number of machines in the scale set increases by 1 when the threshold is met.</span></span>

    * <span data-ttu-id="0db95-207">**cooldown**</span><span class="sxs-lookup"><span data-stu-id="0db95-207">**cooldown**</span></span>  
    <span data-ttu-id="0db95-208">Ez az érték a szükséges időt a legutóbbi méretezési művelet óta várni, mielőtt a következő művelet történik.</span><span class="sxs-lookup"><span data-stu-id="0db95-208">This value is the amount of time to wait since the last scaling action before the next action occurs.</span></span> <span data-ttu-id="0db95-209">Ez az érték egy percet, és egy hét között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0db95-209">This value must be between one minute and one week.</span></span>

12. <span data-ttu-id="0db95-210">A sablonfájl mentési.</span><span class="sxs-lookup"><span data-stu-id="0db95-210">Save the template file.</span></span>    

## <a name="step-3-upload-the-template-to-storage"></a><span data-ttu-id="0db95-211">3. lépés: Töltse fel a sablon tárolási</span><span class="sxs-lookup"><span data-stu-id="0db95-211">Step 3: Upload the template to storage</span></span>
<span data-ttu-id="0db95-212">A sablon mindaddig, amíg nevét és az 1. lépésben létrehozott tárfiók elsődleges kulcs ismeri fel kell tölteni.</span><span class="sxs-lookup"><span data-stu-id="0db95-212">The template can be uploaded as long as you know the name and primary key of the storage account that you created in step 1.</span></span>

1. <span data-ttu-id="0db95-213">A parancssori felület (Bash, terminál, parancssor), futtassa az alábbi parancsokat a környezeti változók a tárfiók eléréséhez szükséges beállítani:</span><span class="sxs-lookup"><span data-stu-id="0db95-213">In your command-line interface (Bash, Terminal, Command prompt), run these commands to set the environment variables needed to access the storage account:</span></span>

    ```cli   
    export AZURE_STORAGE_ACCOUNT={account_name}
    export AZURE_STORAGE_ACCESS_KEY={key}
    ```
    
    <span data-ttu-id="0db95-214">A kulcs eléréséhez a kulcs ikonra kattint, a tárolási fiók erőforrások az Azure portálon megtekintésekor.</span><span class="sxs-lookup"><span data-stu-id="0db95-214">You can get the key by clicking the key icon when viewing the storage account resource in the Azure portal.</span></span> <span data-ttu-id="0db95-215">Ha egy Windows parancssori ablakba, írja be a **beállítása** exportálása helyett.</span><span class="sxs-lookup"><span data-stu-id="0db95-215">When using a Windows command prompt, type **set** instead of export.</span></span>

2. <span data-ttu-id="0db95-216">Az a sablon tárolására alkalmas a tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0db95-216">Create the container for storing the template.</span></span>
   
    ```cli
    azure storage container create -p Blob templates
    ```

3. <span data-ttu-id="0db95-217">A sablon fájlt feltölteni az új tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="0db95-217">Upload the template file to the new container.</span></span>
   
    ```cli
    azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json
    ```

## <a name="step-4-deploy-the-template"></a><span data-ttu-id="0db95-218">4. lépés: A sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="0db95-218">Step 4: Deploy the template</span></span>
<span data-ttu-id="0db95-219">Most, hogy létrehozta a sablont, indítsa el az erőforrásokat üzembe helyezi.</span><span class="sxs-lookup"><span data-stu-id="0db95-219">Now that you created the template, you can start deploying the resources.</span></span> <span data-ttu-id="0db95-220">A folyamat elindításához használt a parancs:</span><span class="sxs-lookup"><span data-stu-id="0db95-220">Use this command to start the process:</span></span>

```cli
azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1
```

<span data-ttu-id="0db95-221">Amikor lenyomja az adja meg, adjon meg értékeket a változók hozzárendelt kéri.</span><span class="sxs-lookup"><span data-stu-id="0db95-221">When you press enter, you are prompted to provide values for the variables you assigned.</span></span> <span data-ttu-id="0db95-222">Adja meg ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="0db95-222">Provide these values:</span></span>

```cli
vmName: vmsstestvm1
vmSSName: vmsstest1
instanceCount: 5
adminUserName: vmadmin1
adminPassword: VMpass1
resourcePrefix: vmsstest
```

<span data-ttu-id="0db95-223">Gondolja, hogy a szolgáltatás sikeresen telepíthető az összes erőforrás mintegy 15 percre leáll.</span><span class="sxs-lookup"><span data-stu-id="0db95-223">It should take about 15 minutes for all the resources to successfully be deployed.</span></span>

> [!NOTE]
> <span data-ttu-id="0db95-224">A portál képességét segítségével is telepítheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="0db95-224">You can also use the portal’s ability to deploy the resources.</span></span> <span data-ttu-id="0db95-225">A hivatkozás: https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template></span><span class="sxs-lookup"><span data-stu-id="0db95-225">Use this link: https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template></span></span>


## <a name="step-5-monitor-resources"></a><span data-ttu-id="0db95-226">5. lépés: A figyelő erőforrások</span><span class="sxs-lookup"><span data-stu-id="0db95-226">Step 5: Monitor resources</span></span>
<span data-ttu-id="0db95-227">Virtuálisgép-méretezési csoportok ezekkel a módszerekkel kapcsolatos információkat kaphat:</span><span class="sxs-lookup"><span data-stu-id="0db95-227">You can get some information about virtual machine scale sets using these methods:</span></span>

* <span data-ttu-id="0db95-228">Az Azure portál – jelenleg kaphat a korlátozott mennyiségű információt a portál használatával.</span><span class="sxs-lookup"><span data-stu-id="0db95-228">The Azure portal - You can currently get a limited amount of information using the portal.</span></span>

* <span data-ttu-id="0db95-229">A [Azure erőforrás-kezelő](https://resources.azure.com/) -Ez az eszköz a legjobb felderítését lehetővé tevő a méretezési csoport jelenlegi állapotában.</span><span class="sxs-lookup"><span data-stu-id="0db95-229">The [Azure Resource Explorer](https://resources.azure.com/) - This tool is the best for exploring the current state of your scale set.</span></span> <span data-ttu-id="0db95-230">Hajtsa végre ezt az elérési utat, és a létrehozott méretezési csoport példányait tartalmazó nézetet kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="0db95-230">Follow this path and you should see the instance view of the scale set that you created:</span></span>
  
    ```cli
    subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines
    ```

* <span data-ttu-id="0db95-231">Az Azure CLI - használja ezt a parancsot néhány adatra:</span><span class="sxs-lookup"><span data-stu-id="0db95-231">Azure CLI - Use this command to get some information:</span></span>

    ```cli  
    azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1
    ```

* <span data-ttu-id="0db95-232">Kapcsolódjon a jumpbox virtuális géphez, ugyanúgy, mint bármely más gép üzembe helyezése, és ezután távolról elérheti a virtuális gépek használata a méretezési készletben az egyes folyamatok figyelése.</span><span class="sxs-lookup"><span data-stu-id="0db95-232">Connect to the jumpbox virtual machine just like you would any other machine and then you can remotely access the virtual machines in the scale set to monitor individual processes.</span></span>

> [!NOTE]
> <span data-ttu-id="0db95-233">A teljes REST API-t a méretezési készlet vonatkozó adatok találhatók [virtuálisgép-skálázási készletekben](https://msdn.microsoft.com/library/mt589023.aspx).</span><span class="sxs-lookup"><span data-stu-id="0db95-233">A complete REST API for obtaining information about scale sets can be found in [Virtual Machine Scale Sets](https://msdn.microsoft.com/library/mt589023.aspx).</span></span>

## <a name="step-6-remove-the-resources"></a><span data-ttu-id="0db95-234">6. lépés: Távolítsa el az erőforrásokat</span><span class="sxs-lookup"><span data-stu-id="0db95-234">Step 6: Remove the resources</span></span>
<span data-ttu-id="0db95-235">Mivel az Azure-ban használt erőforrásokhoz van szó, ajánlott mindig egy törli az erőforrást, amely már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0db95-235">Because you are charged for resources used in Azure, it is always a good practice to delete resources that are no longer needed.</span></span> <span data-ttu-id="0db95-236">Nem kell törölnie az egyes erőforrások külön-külön egy erőforráscsoportból.</span><span class="sxs-lookup"><span data-stu-id="0db95-236">You don’t need to delete each resource separately from a resource group.</span></span> <span data-ttu-id="0db95-237">Az erőforráscsoport törlése, és az erőforrások automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="0db95-237">You can delete the resource group and all its resources are automatically deleted.</span></span>

```cli
azure group delete vmsstestrg1
```

## <a name="next-steps"></a><span data-ttu-id="0db95-238">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0db95-238">Next steps</span></span>
* <span data-ttu-id="0db95-239">Példák figyelési szolgáltatásokat az Azure-figyelő található [figyelő Azure platformfüggetlen parancssori felület gyors start minták](../monitoring-and-diagnostics/insights-cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="0db95-239">Find examples of Azure Monitor monitoring features in [Azure Monitor Cross-platform CLI quick start samples](../monitoring-and-diagnostics/insights-cli-samples.md)</span></span>
* <span data-ttu-id="0db95-240">További tudnivalók az értesítési szolgáltatások [automatikus skálázási műveletek segítségével e-mailek és a webhook riasztási értesítések küldése az Azure-figyelő](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="0db95-240">Learn about notification features in [Use autoscale actions to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span></span>
* <span data-ttu-id="0db95-241">Megtudhatja, hogyan [használata naplókat küldeni e-mailek és a webhook riasztási értesítések az Azure-figyelő](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="0db95-241">Learn how to [Use audit logs to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>
* <span data-ttu-id="0db95-242">Tekintse meg a [automatikus skálázás bemutató alkalmazás Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) sablont, amely egy Python/bottle app beállítja gyakorolni az automatikus skálázási funkciót a virtuálisgép-skálázási készletekben.</span><span class="sxs-lookup"><span data-stu-id="0db95-242">Check out the [Autoscale demo app on Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) template that sets up a Python/bottle app to exercise the automatic scaling functionality of Virtual Machine Scale Sets.</span></span>

