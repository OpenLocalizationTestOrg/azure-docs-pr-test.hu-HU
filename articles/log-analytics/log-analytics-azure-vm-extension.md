---
title: "Az Azure virtuális gépek csatlakoztatása szolgáltatáshoz |} Microsoft Docs"
description: "Windows és Linux rendszerű virtuális gépek Azure-ban az ajánlott módszer a gyűjtött naplók, metrikák van, a napló Analytics Azure Virtuálisgép-bővítmény telepítésével. Az Azure portálon vagy a PowerShell segítségével telepítse az Azure virtuális gépek alakzatot Naplóelemzési virtuálisgép-bővítmény."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: ca39e586-a6af-42fe-862e-80978a58d9b1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: richrund
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cdae291b546fef4d7fdb8b067c8e4f4c9708d43f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="connect-azure-virtual-machines-to-log-analytics-with-a-log-analytics-agent"></a><span data-ttu-id="354a1-104">Az Azure virtuális gépek csatlakoztatása a Log Analyticshez ügynökkel szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="354a1-104">Connect Azure virtual machines to Log Analytics with a Log Analytics agent</span></span>

<span data-ttu-id="354a1-105">A Windows és Linux számítógépekre a naplók és a metrikák gyűjtéséhez az ajánlott módszer van a Naplóelemzési ügynök telepítésével.</span><span class="sxs-lookup"><span data-stu-id="354a1-105">For Windows and Linux computers, the recommended method for collecting logs and metrics is by installing the Log Analytics agent.</span></span>

<span data-ttu-id="354a1-106">A Naplóelemzési ügynök telepítése az Azure virtuális gépeken a legegyszerűbb módja a napló Analytics VM bővítményével.</span><span class="sxs-lookup"><span data-stu-id="354a1-106">The easiest way to install the Log Analytics agent on Azure virtual machines is through the Log Analytics VM Extension.</span></span>  <span data-ttu-id="354a1-107">A bővítmény használatával egyszerűbbé teszi a telepítési folyamat, és automatikusan konfigurálja az adatokat küldeni a Naplóelemzési munkaterület, melyet a az ügynök.</span><span class="sxs-lookup"><span data-stu-id="354a1-107">Using the extension simplifies the installation process and automatically configures the agent to send data to the Log Analytics workspace that you specify.</span></span> <span data-ttu-id="354a1-108">Az ügynök is frissítése automatikusan történik, győződjön meg arról, hogy rendelkezik-e a legújabb funkcióit és javításokat.</span><span class="sxs-lookup"><span data-stu-id="354a1-108">The agent is also upgraded automatically, ensuring that you have the latest features and fixes.</span></span>

<span data-ttu-id="354a1-109">Windows virtuális gépek esetében engedélyezi az *Microsoft Monitoring Agent* virtuálisgép-bővítményt.</span><span class="sxs-lookup"><span data-stu-id="354a1-109">For Windows virtual machines, you enable the *Microsoft Monitoring Agent* virtual machine extension.</span></span>
<span data-ttu-id="354a1-110">A Linux virtuális gépekhez, engedélyezi a *OMS ügynök a Linux* virtuálisgép-bővítményt.</span><span class="sxs-lookup"><span data-stu-id="354a1-110">For Linux virtual machines, you enable the *OMS Agent For Linux* virtual machine extension.</span></span>

<span data-ttu-id="354a1-111">További információ [Azure virtuálisgép-bővítmények](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és a [Linux-ügynök](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="354a1-111">Learn more about [Azure virtual machine extensions](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and the [Linux agent](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="354a1-112">Ha az ügynök-alapú gyűjtemény naplóadatokat használja, konfigurálnia kell [Naplóelemzési adatforrások](log-analytics-data-sources.md) adhatja meg a naplók és metrikákat, szeretne gyűjteni.</span><span class="sxs-lookup"><span data-stu-id="354a1-112">When you use agent-based collection for log data, you must configure [data sources in Log Analytics](log-analytics-data-sources.md) to specify the logs and metrics that you want to collect.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="354a1-113">Ha konfigurálását a Log Analyticshez való index naplóadatok [Azure diagnostics](log-analytics-azure-storage.md), és konfigurálja az ügynököt, hogy az azonos gyűjtését, majd a naplók összegyűjtését kétszer.</span><span class="sxs-lookup"><span data-stu-id="354a1-113">If you configure Log Analytics to index log data by using [Azure diagnostics](log-analytics-azure-storage.md), and you configure the agent to collect the same logs, then the logs are collected twice.</span></span> <span data-ttu-id="354a1-114">Mindkét adatforrások van szó.</span><span class="sxs-lookup"><span data-stu-id="354a1-114">You are charged for both data sources.</span></span> <span data-ttu-id="354a1-115">Ha az ügynök telepítve van, csak az ügynök a napló adatgyűjtés - napló adatokat gyűjteni az Azure diagnostics Naplóelemzési ne állítson be.</span><span class="sxs-lookup"><span data-stu-id="354a1-115">If you have the agent installed, then collect log data by using only the agent - don't configure Log Analytics to collect log data from Azure diagnostics.</span></span>
>
>

<span data-ttu-id="354a1-116">Háromféleképpen könnyen Naplóelemzési virtuálisgép-bővítmény engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="354a1-116">There are three easy ways to enable the Log Analytics virtual machine extension:</span></span>

* <span data-ttu-id="354a1-117">Az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="354a1-117">By using the Azure portal</span></span>
* <span data-ttu-id="354a1-118">Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="354a1-118">By using Azure PowerShell</span></span>
* <span data-ttu-id="354a1-119">Az Azure Resource Manager-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="354a1-119">By using an Azure Resource Manager template</span></span>

## <a name="enable-the-vm-extension-in-the-azure-portal"></a><span data-ttu-id="354a1-120">Az Azure-portálon a Virtuálisgép-bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="354a1-120">Enable the VM extension in the Azure portal</span></span>
<span data-ttu-id="354a1-121">Telepítse az ügynököt a Naplóelemzési, és csatlakozzon az Azure virtuális géphez, amely segítségével az fut a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="354a1-121">You can install the agent for Log Analytics and connect the Azure virtual machine that it runs on by using the [Azure portal](https://portal.azure.com).</span></span>

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a><span data-ttu-id="354a1-122">A Log Analytics-ügynök telepítése, és csatlakoztassa a virtuális gépet a Naplóelemzési munkaterület</span><span class="sxs-lookup"><span data-stu-id="354a1-122">To install the Log Analytics agent and connect the virtual machine to a Log Analytics workspace</span></span>
1. <span data-ttu-id="354a1-123">Jelentkezzen be az [Azure Portalra](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="354a1-123">Sign into the [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="354a1-124">Válassza ki **Tallózás** a portálon, és lépjen a bal oldalon **Naplóelemzés (OMS)** , és jelölje ki.</span><span class="sxs-lookup"><span data-stu-id="354a1-124">Select **Browse** on the left side of the portal, and then go to **Log Analytics (OMS)** and select it.</span></span>
3. <span data-ttu-id="354a1-125">A Naplóelemzési munkaterület, jelölje ki azt, amelyik az Azure virtuális Géphez használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="354a1-125">In your list of Log Analytics workspaces, select the one that you want to use with the Azure VM.</span></span>  
   ![OMS-munkaterület](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. <span data-ttu-id="354a1-127">A **analytics management naplózása**, jelölje be **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="354a1-127">Under **Log analytics management**, select **Virtual machines**.</span></span>  
   <span data-ttu-id="354a1-128">![Virtuális gépek](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)</span><span class="sxs-lookup"><span data-stu-id="354a1-128">![Virtual machines](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)</span></span>
5. <span data-ttu-id="354a1-129">A közül **virtuális gépek**, válassza ki a virtuális gépre, amelyen szeretné az ügynök telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="354a1-129">In the list of **Virtual machines**, select the virtual machine on which you want to install the agent.</span></span> <span data-ttu-id="354a1-130">A **OMS kapcsolat állapota** a virtuális gép azt mutatja, hogy a **nincs csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="354a1-130">The **OMS connection status** for the VM indicates that it is **Not connected**.</span></span>  
   <span data-ttu-id="354a1-131">![Virtuális gép nincs csatlakoztatva](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)</span><span class="sxs-lookup"><span data-stu-id="354a1-131">![VM not connected](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)</span></span>
6. <span data-ttu-id="354a1-132">Válassza ki a virtuális gép részleteit, **Connect**.</span><span class="sxs-lookup"><span data-stu-id="354a1-132">In the details for your virtual machine, select **Connect**.</span></span> <span data-ttu-id="354a1-133">Az ügynök automatikusan telepítve és beállítva a Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="354a1-133">The agent is automatically installed and configured for your Log Analytics workspace.</span></span> <span data-ttu-id="354a1-134">Ez a folyamat néhány percet vesz igénybe, amely idő alatt az OMS-kapcsolat állapota *csatlakozás...*</span><span class="sxs-lookup"><span data-stu-id="354a1-134">This process takes a few minutes, during which time the OMS Connection status is *Connecting...*</span></span>  
   <span data-ttu-id="354a1-135">![Csatlakozás a virtuális gép](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)</span><span class="sxs-lookup"><span data-stu-id="354a1-135">![Connect VM](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)</span></span>
7. <span data-ttu-id="354a1-136">Csatlakoztassa az ügynököt, és telepítése után a **OMS kapcsolat** állapota frissülni fognak megjelenítése **a munkaterület**.</span><span class="sxs-lookup"><span data-stu-id="354a1-136">After you install and connect the agent, the **OMS connection** status will be updated to show **This workspace**.</span></span>  
   <span data-ttu-id="354a1-137">![Csatlakoztatva](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)</span><span class="sxs-lookup"><span data-stu-id="354a1-137">![Connected](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)</span></span>

## <a name="enable-the-vm-extension-using-powershell"></a><span data-ttu-id="354a1-138">A PowerShell használatával Virtuálisgép-bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="354a1-138">Enable the VM extension using PowerShell</span></span>
<span data-ttu-id="354a1-139">A virtuális gép PowerShell használatával történő konfigurálásakor meg kell adnia a **workspaceId** és **workspaceKey**.</span><span class="sxs-lookup"><span data-stu-id="354a1-139">When you configure your virtual machine by using PowerShell, you need to provide the **workspaceId** and **workspaceKey**.</span></span> <span data-ttu-id="354a1-140">A json-konfigurációt a tulajdonságnevek megkülönböztetik **kis-és nagybetűket**.</span><span class="sxs-lookup"><span data-stu-id="354a1-140">The property names in your json configuration are **case-sensitive**.</span></span>

<span data-ttu-id="354a1-141">Az azonosító található, és a kulcs a **beállítások** oldalán az OMS-portálon, vagy a PowerShell használatával az előző példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="354a1-141">You can find the Id and key on the **Settings** page of the OMS portal, or by using PowerShell as shown in the preceding example.</span></span>

![Munkaterületének Azonosítóját és az elsődleges kulcs](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

<span data-ttu-id="354a1-143">Nincsenek Azure klasszikus virtuális gépek és a Resource Manager virtuális gépek különböző parancsokat.</span><span class="sxs-lookup"><span data-stu-id="354a1-143">There are different commands for Azure classic virtual machines and Resource Manager virtual machines.</span></span> <span data-ttu-id="354a1-144">Az alábbiakban például a klasszikus és Resource Manager virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="354a1-144">Following are examples for both classic and Resource Manager virtual machines.</span></span>

<span data-ttu-id="354a1-145">Klasszikus virtuális gépeket használja a következő PowerShell-példa:</span><span class="sxs-lookup"><span data-stu-id="354a1-145">For classic virtual machines, use the following PowerShell example:</span></span>

```PowerShell
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

<span data-ttu-id="354a1-146">Erőforrás-kezelő Linux virtuális gépek a következő parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="354a1-146">For Resource Manager Linux VMs using the following CLI</span></span>
```azurecli
az vm extension set --resource-group myRGMonitor --vm-name myMonitorVM --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --version 1.3 --protected-settings ‘{"workspaceKey": "<workspace-key>"}’ --settings ‘{"workspaceId": "<workspace-id>"}’ 
```

<span data-ttu-id="354a1-147">A virtuális gépek erőforrás-kezelő használja a következő PowerShell-példa:</span><span class="sxs-lookup"><span data-stu-id="354a1-147">For Resource Manager virtual machines, use the following PowerShell example:</span></span>

```PowerShell
Login-AzureRMAccount
Select-AzureRMSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```


## <a name="deploy-the-vm-extension-using-a-template"></a><span data-ttu-id="354a1-148">A sablon használatával Virtuálisgép-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="354a1-148">Deploy the VM extension using a template</span></span>
<span data-ttu-id="354a1-149">Azure Resource Manager használatával létrehozhat egy sablont (JSON formátumban), amely meghatározza a központi telepítés és az alkalmazás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="354a1-149">By using Azure Resource Manager, you can create a template (in JSON format) that defines the deployment and configuration of your application.</span></span> <span data-ttu-id="354a1-150">Ez a sablon Resource Manager sablonként is ismert és deklaratív lehetőséget biztosít a telepítés meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="354a1-150">This template is known as a Resource Manager template and provides a declarative way to define deployment.</span></span> <span data-ttu-id="354a1-151">Egy sablon használatával ismételten telepítheti az alkalmazást, az alkalmazás életciklusa és biztos lehet abban, hogy az erőforrások telepítése konzisztens lesz alatt.</span><span class="sxs-lookup"><span data-stu-id="354a1-151">By using a template, you can repeatedly deploy your application throughout the app lifecycle and have confidence that your resources are being deployed in a consistent state.</span></span>

<span data-ttu-id="354a1-152">A Naplóelemzési ügynök beleértve a Resource Manager-sablon részeként, biztosítható, hogy előre konfigurálva, hogy a Naplóelemzési munkaterület-e minden virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="354a1-152">By including the Log Analytics agent as part of your Resource Manager template, you can ensure that each virtual machine is pre-configured to report to your Log Analytics workspace.</span></span>

<span data-ttu-id="354a1-153">Erőforrás-kezelő sablonokkal kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="354a1-153">For more information about Resource Manager templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="354a1-154">Az alábbiakban látható egy példa a Resource Manager-sablont, amely a Microsoft Monitoring Agent-kiterjesztés telepítése a Windows rendszerű virtuális gép telepítéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="354a1-154">Following is an example of a Resource Manager template that's used for deploying a virtual machine that's running Windows with the Microsoft Monitoring Agent extension installed.</span></span> <span data-ttu-id="354a1-155">Ez a sablon a következő tipikus virtuálisgép-sablont, az alábbi kiegészítésekkel:</span><span class="sxs-lookup"><span data-stu-id="354a1-155">This template is a typical virtual machine template, with the following additions:</span></span>

* <span data-ttu-id="354a1-156">workspaceId és workspaceName paraméterek</span><span class="sxs-lookup"><span data-stu-id="354a1-156">workspaceId and workspaceName parameters</span></span>
* <span data-ttu-id="354a1-157">Erőforrás-kiterjesztésben megadottaknak Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="354a1-157">Microsoft.EnterpriseCloud.Monitoring resource extension section</span></span>
* <span data-ttu-id="354a1-158">A workspaceId és workspaceSharedKey kereséséhez kimenetek</span><span class="sxs-lookup"><span data-stu-id="354a1-158">Outputs to look up the workspaceId and workspaceSharedKey</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMS workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
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
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

<span data-ttu-id="354a1-159">A sablon a következő PowerShell-parancs segítségével telepítheti:</span><span class="sxs-lookup"><span data-stu-id="354a1-159">You can deploy a template by using the following PowerShell command:</span></span>

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-the-log-analytics-vm-extension"></a><span data-ttu-id="354a1-160">Hibaelhárítás a napló Analytics Virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="354a1-160">Troubleshooting the Log Analytics VM extension</span></span>
<span data-ttu-id="354a1-161">Általában kap egy üzenetet, ha a probléma, az Azure portálon vagy az Azure powershell.</span><span class="sxs-lookup"><span data-stu-id="354a1-161">Usually you receive a message when things don't work, from either Azure portal or Azure powershell.</span></span>

1. <span data-ttu-id="354a1-162">Jelentkezzen be az [Azure Portalra](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="354a1-162">Sign into the [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="354a1-163">Keresse meg a virtuális Gépet, és nyissa meg a virtuális gép részletei.</span><span class="sxs-lookup"><span data-stu-id="354a1-163">Find the VM and open VM details.</span></span>
3. <span data-ttu-id="354a1-164">Kattintson a **bővítmények** ellenőrzése, ha az OMS-bővítmény engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="354a1-164">Click **Extensions** to check if OMS extension is enabled or not.</span></span>

   ![Virtuálisgép-bővítmények nézetet](./media/log-analytics-azure-vm-extension/oms-vmview-extensions.png)

4. <span data-ttu-id="354a1-166">Kattintson a *MicrosoftMonitoringAgent*(Windows) vagy *OmsAgentForLinux*(Linux) bővítményt, és a nézet részletek.</span><span class="sxs-lookup"><span data-stu-id="354a1-166">Click the *MicrosoftMonitoringAgent*(Windows) or *OmsAgentForLinux*(Linux) extension and view details.</span></span> 

   ![Virtuálisgép-bővítmény részletei](./media/log-analytics-azure-vm-extension/oms-vmview-extensiondetails.png)

### <a name="troubleshooting-windows-virtual-machines"></a><span data-ttu-id="354a1-168">Hibaelhárítás Windows virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="354a1-168">Troubleshooting Windows Virtual Machines</span></span>
<span data-ttu-id="354a1-169">Ha a *Microsoft Monitoring Agent* nem telepíti a Virtuálisgép-ügynök bővítmény vagy reporting, végezheti el a probléma elhárítása érdekében az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="354a1-169">If the *Microsoft Monitoring Agent* VM agent extension is not installing or reporting, you can perform the following steps to troubleshoot the issue.</span></span>

1. <span data-ttu-id="354a1-170">Az Azure Virtuálisgép-ügynök telepítésének ellenőrzése és működik a megfelelő segítségével lépéseit [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).</span><span class="sxs-lookup"><span data-stu-id="354a1-170">Check if the Azure VM agent is installed and working correctly by using the steps in [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).</span></span>
   * <span data-ttu-id="354a1-171">Emellett áttekintheti a Virtuálisgép-ügynök naplófájlját`C:\WindowsAzure\logs\WaAppAgent.log`</span><span class="sxs-lookup"><span data-stu-id="354a1-171">You can also review the VM agent log file `C:\WindowsAzure\logs\WaAppAgent.log`</span></span>
   * <span data-ttu-id="354a1-172">Ha a napló nem létezik, a Virtuálisgép-ügynök nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="354a1-172">If the log does not exist, the VM agent is not installed.</span></span>
     * [<span data-ttu-id="354a1-173">Az Azure Virtuálisgép-ügynök telepítése a klasszikus virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="354a1-173">Install the Azure VM Agent on classic VMs</span></span>](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. <span data-ttu-id="354a1-174">Ellenőrizze, hogy a Microsoft Monitoring Agent bővítmény szívverés feladat fut-e az alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="354a1-174">Confirm the Microsoft Monitoring Agent extension heartbeat task is running using the following steps:</span></span>
   * <span data-ttu-id="354a1-175">Jelentkezzen be a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="354a1-175">Log in to the virtual machine</span></span>
   * <span data-ttu-id="354a1-176">Nyissa meg a Feladatütemezőt, és keresse a `update_azureoperationalinsight_agent_heartbeat` feladat</span><span class="sxs-lookup"><span data-stu-id="354a1-176">Open task scheduler and find the `update_azureoperationalinsight_agent_heartbeat` task</span></span>
   * <span data-ttu-id="354a1-177">Erősítse meg a feladat engedélyezve van, és egy percenként fut.</span><span class="sxs-lookup"><span data-stu-id="354a1-177">Confirm the task is enabled and is running every one minute</span></span>
   * <span data-ttu-id="354a1-178">Ellenőrizze, hogy a szívverés logfile szerepel`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`</span><span class="sxs-lookup"><span data-stu-id="354a1-178">Check the heartbeat logfile in `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`</span></span>
3. <span data-ttu-id="354a1-179">Tekintse át a Microsoft Monitoring Agent VM bővítmény naplófájlokban`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`</span><span class="sxs-lookup"><span data-stu-id="354a1-179">Review the Microsoft Monitoring Agent VM extension log files in `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`</span></span>
4. <span data-ttu-id="354a1-180">Győződjön meg arról, a virtuális gép PowerShell-parancsfájlok is futtathatók.</span><span class="sxs-lookup"><span data-stu-id="354a1-180">Ensure the virtual machine can run PowerShell scripts</span></span>
5. <span data-ttu-id="354a1-181">Győződjön meg arról, nem módosította a C:\Windows\temp engedélyeinek</span><span class="sxs-lookup"><span data-stu-id="354a1-181">Ensure permissions on C:\Windows\temp haven’t been changed</span></span>
6. <span data-ttu-id="354a1-182">Írja be a következő parancsot egy emelt szintű PowerShell-ablakban a virtuális gépen a Microsoft Monitoring Agent állapotának megtekintése`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`</span><span class="sxs-lookup"><span data-stu-id="354a1-182">View the status of the Microsoft Monitoring Agent by typing the following command in an elevated PowerShell window on the virtual machine `  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`</span></span>
7. <span data-ttu-id="354a1-183">Tekintse át a Microsoft Monitoring Agent telepítési naplófájlok`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`</span><span class="sxs-lookup"><span data-stu-id="354a1-183">Review the Microsoft Monitoring Agent setup log files in `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`</span></span>

<span data-ttu-id="354a1-184">További információkért lásd: [hibaelhárítása a Windows](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="354a1-184">For more information, see [troubleshooting Windows extensions](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="troubleshooting-linux-virtual-machines"></a><span data-ttu-id="354a1-185">Linux virtuális gépek hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="354a1-185">Troubleshooting Linux Virtual Machines</span></span>
<span data-ttu-id="354a1-186">Ha a *Linux OMS-ügynököt* nem telepíti a Virtuálisgép-ügynök bővítmény vagy reporting, végezheti el a probléma elhárítása érdekében az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="354a1-186">If the *OMS Agent for Linux* VM agent extension is not installing or reporting, you can perform the following steps to troubleshoot the issue.</span></span>

1. <span data-ttu-id="354a1-187">Ha a bővítmény állapotát *ismeretlen* ellenőrizze, hogy ha az Azure Virtuálisgép-ügynök telepítve van és megfelelően működik-e a virtuális gép ügynök naplófájlját megtekintésével`/var/log/waagent.log`</span><span class="sxs-lookup"><span data-stu-id="354a1-187">If the extension status is *Unknown* check if the Azure VM agent is installed and working correctly by reviewing the VM agent log file `/var/log/waagent.log`</span></span>
   * <span data-ttu-id="354a1-188">Ha a napló nem létezik, a Virtuálisgép-ügynök nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="354a1-188">If the log does not exist, the VM agent is not installed.</span></span>
   * [<span data-ttu-id="354a1-189">Az Azure Virtuálisgép-ügynök telepítése Linux virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="354a1-189">Install the Azure VM Agent on Linux VMs</span></span>](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. <span data-ttu-id="354a1-190">A többi nem kifogástalan állapot, tekintse át az OMS-ügynököt, a Linux Virtuálisgép-bővítmény naplózza a fájlokat `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` és`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`</span><span class="sxs-lookup"><span data-stu-id="354a1-190">For other unhealthy statuses, review the OMS Agent for Linux VM extension logs files in `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` and `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`</span></span>
3. <span data-ttu-id="354a1-191">Ha a bővítmény állapota kifogástalan, de nem feltöltött adatok tekintse át az OMS-ügynököt a Linux-naplófájl`/var/opt/microsoft/omsagent/log/omsagent.log`</span><span class="sxs-lookup"><span data-stu-id="354a1-191">If the extension status is healthy, but data is not being uploaded review the OMS Agent for Linux log files in `/var/opt/microsoft/omsagent/log/omsagent.log`</span></span>

## <a name="next-steps"></a><span data-ttu-id="354a1-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="354a1-192">Next steps</span></span>
* <span data-ttu-id="354a1-193">Konfigurálása [Naplóelemzési adatforrások](log-analytics-data-sources.md) adhatja meg a naplók és a metrikák gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="354a1-193">Configure [data sources in Log Analytics](log-analytics-data-sources.md) to specify the logs and metrics to collect.</span></span>
* <span data-ttu-id="354a1-194">Az adatok gyűjtését a virtuális gépek [hozzáadni a Naplóelemzési megoldások a megoldások gyűjteményből](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="354a1-194">To gather data from virtual machines [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
* <span data-ttu-id="354a1-195">[A Azure diagnosztikai adatok gyűjtéséhez](log-analytics-azure-storage.md) más Azure-ban futó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="354a1-195">[Collect data by using Azure Diagnostics](log-analytics-azure-storage.md) for other resources that are running in Azure.</span></span>

<span data-ttu-id="354a1-196">A számítógépek számára, amelyek nem az Azure-ban a Naplóelemzési ügynök a következő cikkekben ismertetett módszerekkel telepíthető:</span><span class="sxs-lookup"><span data-stu-id="354a1-196">For computers that are not in Azure, you can install the Log Analytics agent by using the methods that are described in the following articles:</span></span>

* [<span data-ttu-id="354a1-197">Windows rendszerű számítógépek csatlakoztatása szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="354a1-197">Connect Windows computers to Log Analytics</span></span>](log-analytics-windows-agents.md)
* [<span data-ttu-id="354a1-198">Linux rendszerű számítógépek csatlakoztatása szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="354a1-198">Connect Linux computers to Log Analytics</span></span>](log-analytics-linux-agents.md)
