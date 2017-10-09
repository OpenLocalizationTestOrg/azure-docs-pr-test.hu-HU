---
title: "aaaConnect Azure virtuális gépek tooLog Analytics |} Microsoft Docs"
description: "A Windows és Linux virtuális gépek Azure-ban futó, hello ajánlott módszer a gyűjtött naplók és metrikákat hello napló Analytics Azure Virtuálisgép-bővítmény telepítésével. Hello Azure-portál vagy PowerShell tooinstall hello Naplóelemzési virtuálisgép-bővítmény, Azure virtuális gépeken is használhatja."
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
ms.openlocfilehash: ac96c242d03ed3a22ca96368e5a8cc53f9a993db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-azure-virtual-machines-toolog-analytics-with-a-log-analytics-agent"></a><span data-ttu-id="3c761-104">Csatlakozás az Azure virtuális gépek tooLog Analytics egy Log Analytics-ügynök</span><span class="sxs-lookup"><span data-stu-id="3c761-104">Connect Azure virtual machines tooLog Analytics with a Log Analytics agent</span></span>

<span data-ttu-id="3c761-105">A Windows és Linux számítógépekre hello ajánlott módszer a naplógyűjtés időtartamát és metrikákat hello Naplóelemzési ügynök telepítésével.</span><span class="sxs-lookup"><span data-stu-id="3c761-105">For Windows and Linux computers, hello recommended method for collecting logs and metrics is by installing hello Log Analytics agent.</span></span>

<span data-ttu-id="3c761-106">hello legegyszerűbb módja tooinstall hello Naplóelemzési ügynök az Azure virtuális gépeken hello napló Analytics Virtuálisgép-bővítmény keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="3c761-106">hello easiest way tooinstall hello Log Analytics agent on Azure virtual machines is through hello Log Analytics VM Extension.</span></span>  <span data-ttu-id="3c761-107">Hello kiterjesztéssel leegyszerűsíti hello telepítési folyamatot, és automatikusan konfigurálja a hello ügynök toosend adatok toohello Naplóelemzési munkaterület megadott.</span><span class="sxs-lookup"><span data-stu-id="3c761-107">Using hello extension simplifies hello installation process and automatically configures hello agent toosend data toohello Log Analytics workspace that you specify.</span></span> <span data-ttu-id="3c761-108">hello ügynök is frissítése automatikusan történik, győződjön meg arról, hogy rendelkezik a legújabb szolgáltatásokat hello és javításokat.</span><span class="sxs-lookup"><span data-stu-id="3c761-108">hello agent is also upgraded automatically, ensuring that you have hello latest features and fixes.</span></span>

<span data-ttu-id="3c761-109">Windows virtuális gépek esetében engedélyezi hello *Microsoft Monitoring Agent* virtuálisgép-bővítményt.</span><span class="sxs-lookup"><span data-stu-id="3c761-109">For Windows virtual machines, you enable hello *Microsoft Monitoring Agent* virtual machine extension.</span></span>
<span data-ttu-id="3c761-110">A Linux virtuális gépekhez, engedélyezi a hello *OMS ügynök a Linux* virtuálisgép-bővítményt.</span><span class="sxs-lookup"><span data-stu-id="3c761-110">For Linux virtual machines, you enable hello *OMS Agent For Linux* virtual machine extension.</span></span>

<span data-ttu-id="3c761-111">További információ [Azure virtuálisgép-bővítmények](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és hello [Linux-ügynök](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3c761-111">Learn more about [Azure virtual machine extensions](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and hello [Linux agent](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="3c761-112">Ha az ügynök-alapú gyűjtemény naplóadatokat használja, konfigurálnia kell [Naplóelemzési adatforrások](log-analytics-data-sources.md) toospecify hello naplókat, valamint, hogy szeretné-e toocollect metrikákat.</span><span class="sxs-lookup"><span data-stu-id="3c761-112">When you use agent-based collection for log data, you must configure [data sources in Log Analytics](log-analytics-data-sources.md) toospecify hello logs and metrics that you want toocollect.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c761-113">Ha konfigurálását a Naplóelemzési tooindex naplóadatok [Azure diagnostics](log-analytics-azure-storage.md), és konfigurálnia hello ügynök toocollect hello azonos naplózza, akkor hello naplók összegyűjtését kétszer.</span><span class="sxs-lookup"><span data-stu-id="3c761-113">If you configure Log Analytics tooindex log data by using [Azure diagnostics](log-analytics-azure-storage.md), and you configure hello agent toocollect hello same logs, then hello logs are collected twice.</span></span> <span data-ttu-id="3c761-114">Mindkét adatforrások van szó.</span><span class="sxs-lookup"><span data-stu-id="3c761-114">You are charged for both data sources.</span></span> <span data-ttu-id="3c761-115">Hello ügynök telepítve van, ha csak hello ügynök a napló adatgyűjtés - ne állítson be az Azure diagnostics Naplóelemzési toocollect naplóadatait.</span><span class="sxs-lookup"><span data-stu-id="3c761-115">If you have hello agent installed, then collect log data by using only hello agent - don't configure Log Analytics toocollect log data from Azure diagnostics.</span></span>
>
>

<span data-ttu-id="3c761-116">Háromféleképpen könnyen tooenable hello Naplóelemzési virtuálisgép-bővítmény:</span><span class="sxs-lookup"><span data-stu-id="3c761-116">There are three easy ways tooenable hello Log Analytics virtual machine extension:</span></span>

* <span data-ttu-id="3c761-117">Hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="3c761-117">By using hello Azure portal</span></span>
* <span data-ttu-id="3c761-118">Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="3c761-118">By using Azure PowerShell</span></span>
* <span data-ttu-id="3c761-119">Az Azure Resource Manager-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="3c761-119">By using an Azure Resource Manager template</span></span>

## <a name="enable-hello-vm-extension-in-hello-azure-portal"></a><span data-ttu-id="3c761-120">Az Azure-portálon hello hello Virtuálisgép-bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3c761-120">Enable hello VM extension in hello Azure portal</span></span>
<span data-ttu-id="3c761-121">Naplóelemzési hello-ügynök telepítése, és csatlakozzon hello Azure virtuális gép futó, hello segítségével [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3c761-121">You can install hello agent for Log Analytics and connect hello Azure virtual machine that it runs on by using hello [Azure portal](https://portal.azure.com).</span></span>

### <a name="tooinstall-hello-log-analytics-agent-and-connect-hello-virtual-machine-tooa-log-analytics-workspace"></a><span data-ttu-id="3c761-122">tooinstall hello Naplóelemzési ügynök, és csatlakozzon a hello virtuális gép tooa Naplóelemzési munkaterület</span><span class="sxs-lookup"><span data-stu-id="3c761-122">tooinstall hello Log Analytics agent and connect hello virtual machine tooa Log Analytics workspace</span></span>
1. <span data-ttu-id="3c761-123">Jelentkezzen be a hello [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3c761-123">Sign into hello [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="3c761-124">Válassza ki **Tallózás** hello a bal oldali hello oldalán portál, és nyissa meg majd túl**Naplóelemzés (OMS)** , és jelölje ki.</span><span class="sxs-lookup"><span data-stu-id="3c761-124">Select **Browse** on hello left side of hello portal, and then go too**Log Analytics (OMS)** and select it.</span></span>
3. <span data-ttu-id="3c761-125">A Naplóelemzési munkaterület, jelölje ki, hogy szeretné-e az Azure virtuális gép hello toouse egy hello.</span><span class="sxs-lookup"><span data-stu-id="3c761-125">In your list of Log Analytics workspaces, select hello one that you want toouse with hello Azure VM.</span></span>  
   ![OMS-munkaterület](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. <span data-ttu-id="3c761-127">A **analytics management naplózása**, jelölje be **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="3c761-127">Under **Log analytics management**, select **Virtual machines**.</span></span>  
   <span data-ttu-id="3c761-128">![Virtuális gépek](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)</span><span class="sxs-lookup"><span data-stu-id="3c761-128">![Virtual machines](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)</span></span>
5. <span data-ttu-id="3c761-129">Hello listájában **virtuális gépek**, válassza ki a virtuális gépre, amelyen szeretné tooinstall hello ügynök hello.</span><span class="sxs-lookup"><span data-stu-id="3c761-129">In hello list of **Virtual machines**, select hello virtual machine on which you want tooinstall hello agent.</span></span> <span data-ttu-id="3c761-130">Hello **OMS kapcsolat állapota** hello VM azt jelöli, hogy nem **nincs csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="3c761-130">hello **OMS connection status** for hello VM indicates that it is **Not connected**.</span></span>  
   <span data-ttu-id="3c761-131">![Virtuális gép nincs csatlakoztatva](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)</span><span class="sxs-lookup"><span data-stu-id="3c761-131">![VM not connected](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)</span></span>
6. <span data-ttu-id="3c761-132">Válassza ki a virtuális gép részletei hello, **Connect**.</span><span class="sxs-lookup"><span data-stu-id="3c761-132">In hello details for your virtual machine, select **Connect**.</span></span> <span data-ttu-id="3c761-133">hello ügynök automatikusan telepítve és beállítva a Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="3c761-133">hello agent is automatically installed and configured for your Log Analytics workspace.</span></span> <span data-ttu-id="3c761-134">Ez a folyamat néhány percet vesz igénybe, amely idő alatt hello OMS kapcsolat állapota van *csatlakozás...*</span><span class="sxs-lookup"><span data-stu-id="3c761-134">This process takes a few minutes, during which time hello OMS Connection status is *Connecting...*</span></span>  
   <span data-ttu-id="3c761-135">![Csatlakozás a virtuális gép](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)</span><span class="sxs-lookup"><span data-stu-id="3c761-135">![Connect VM](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)</span></span>
7. <span data-ttu-id="3c761-136">Miután telepíti, és csatlakozni tud hello ügynök hello **OMS kapcsolat** állapota lesz frissített tooshow **a munkaterület**.</span><span class="sxs-lookup"><span data-stu-id="3c761-136">After you install and connect hello agent, hello **OMS connection** status will be updated tooshow **This workspace**.</span></span>  
   <span data-ttu-id="3c761-137">![Csatlakoztatva](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)</span><span class="sxs-lookup"><span data-stu-id="3c761-137">![Connected](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)</span></span>

## <a name="enable-hello-vm-extension-using-powershell"></a><span data-ttu-id="3c761-138">PowerShell-lel hello Virtuálisgép-bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3c761-138">Enable hello VM extension using PowerShell</span></span>
<span data-ttu-id="3c761-139">A virtuális gép PowerShell használatával való konfigurálásakor kell tooprovide hello **workspaceId** és **workspaceKey**.</span><span class="sxs-lookup"><span data-stu-id="3c761-139">When you configure your virtual machine by using PowerShell, you need tooprovide hello **workspaceId** and **workspaceKey**.</span></span> <span data-ttu-id="3c761-140">a json-konfigurációt hello tulajdonságnevek megkülönböztetik **kis-és nagybetűket**.</span><span class="sxs-lookup"><span data-stu-id="3c761-140">hello property names in your json configuration are **case-sensitive**.</span></span>

<span data-ttu-id="3c761-141">Hello azonosítója és a hello kulcs található **beállítások** oldalán hello OMS-portálon, vagy a PowerShell használatával, ahogy az előző példa hello.</span><span class="sxs-lookup"><span data-stu-id="3c761-141">You can find hello Id and key on hello **Settings** page of hello OMS portal, or by using PowerShell as shown in hello preceding example.</span></span>

![Munkaterületének Azonosítóját és az elsődleges kulcs](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

<span data-ttu-id="3c761-143">Nincsenek Azure klasszikus virtuális gépek és a Resource Manager virtuális gépek különböző parancsokat.</span><span class="sxs-lookup"><span data-stu-id="3c761-143">There are different commands for Azure classic virtual machines and Resource Manager virtual machines.</span></span> <span data-ttu-id="3c761-144">Az alábbiakban például a klasszikus és Resource Manager virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="3c761-144">Following are examples for both classic and Resource Manager virtual machines.</span></span>

<span data-ttu-id="3c761-145">Klasszikus virtuális gépek esetében a következő PowerShell-példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="3c761-145">For classic virtual machines, use hello following PowerShell example:</span></span>

```PowerShell
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment hello following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment hello following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

<span data-ttu-id="3c761-146">Erőforrás-kezelő Linux virtuális gépek hello a következő parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="3c761-146">For Resource Manager Linux VMs using hello following CLI</span></span>
```azurecli
az vm extension set --resource-group myRGMonitor --vm-name myMonitorVM --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --version 1.3 --protected-settings ‘{"workspaceKey": "<workspace-key>"}’ --settings ‘{"workspaceId": "<workspace-id>"}’ 
```

<span data-ttu-id="3c761-147">Erőforrás-kezelő virtuális gépek esetében a következő PowerShell-példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="3c761-147">For Resource Manager virtual machines, use hello following PowerShell example:</span></span>

```PowerShell
Login-AzureRMAccount
Select-AzureRMSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable toofind OMS Workspace $workspaceName. Do you need toorun Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment hello following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment hello following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```


## <a name="deploy-hello-vm-extension-using-a-template"></a><span data-ttu-id="3c761-148">Egy sablon használatával hello Virtuálisgép-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="3c761-148">Deploy hello VM extension using a template</span></span>
<span data-ttu-id="3c761-149">Azure Resource Manager segítségével létrehozhat egy sablont (JSON formátumban), amely meghatározza az alkalmazás a hello telepítését és konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="3c761-149">By using Azure Resource Manager, you can create a template (in JSON format) that defines hello deployment and configuration of your application.</span></span> <span data-ttu-id="3c761-150">Ez a sablon Resource Manager sablonként is ismert és deklaratív módon toodefine központi telepítés biztosít.</span><span class="sxs-lookup"><span data-stu-id="3c761-150">This template is known as a Resource Manager template and provides a declarative way toodefine deployment.</span></span> <span data-ttu-id="3c761-151">Egy sablon használatával ismételten telepítheti az alkalmazást teljes hello alkalmazás-életciklus és biztos lehet abban, hogy az erőforrások telepítése konzisztens lesz folyamatban.</span><span class="sxs-lookup"><span data-stu-id="3c761-151">By using a template, you can repeatedly deploy your application throughout hello app lifecycle and have confidence that your resources are being deployed in a consistent state.</span></span>

<span data-ttu-id="3c761-152">Hello Naplóelemzési ügynök beleértve a Resource Manager-sablon részeként, biztosítható, hogy minden virtuális géphez-e az előre konfigurált tooreport tooyour Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="3c761-152">By including hello Log Analytics agent as part of your Resource Manager template, you can ensure that each virtual machine is pre-configured tooreport tooyour Log Analytics workspace.</span></span>

<span data-ttu-id="3c761-153">Erőforrás-kezelő sablonokkal kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3c761-153">For more information about Resource Manager templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="3c761-154">Az alábbiakban látható egy példa a Resource Manager-sablont, amely egy virtuális gépet, hogy fut-e a Windows hello-kiterjesztés telepítése a Microsoft Monitoring Agent telepítéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="3c761-154">Following is an example of a Resource Manager template that's used for deploying a virtual machine that's running Windows with hello Microsoft Monitoring Agent extension installed.</span></span> <span data-ttu-id="3c761-155">Ez a sablon a következő tipikus virtuálisgép-sablon, a következő kiegészítéseket hello:</span><span class="sxs-lookup"><span data-stu-id="3c761-155">This template is a typical virtual machine template, with hello following additions:</span></span>

* <span data-ttu-id="3c761-156">workspaceId és workspaceName paraméterek</span><span class="sxs-lookup"><span data-stu-id="3c761-156">workspaceId and workspaceName parameters</span></span>
* <span data-ttu-id="3c761-157">Erőforrás-kiterjesztésben megadottaknak Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="3c761-157">Microsoft.EnterpriseCloud.Monitoring resource extension section</span></span>
* <span data-ttu-id="3c761-158">Kimenetek toolook hello workspaceId és workspaceSharedKey</span><span class="sxs-lookup"><span data-stu-id="3c761-158">Outputs toolook up hello workspaceId and workspaceSharedKey</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for hello Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for hello Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for hello Public IP. Must be lowercase. It should match with hello following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
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
        "description": "hello Windows version for hello VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
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

<span data-ttu-id="3c761-159">A sablon a következő PowerShell-paranccsal hello segítségével telepíthet:</span><span class="sxs-lookup"><span data-stu-id="3c761-159">You can deploy a template by using hello following PowerShell command:</span></span>

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-hello-log-analytics-vm-extension"></a><span data-ttu-id="3c761-160">Hibaelhárítás hello napló Analytics Virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="3c761-160">Troubleshooting hello Log Analytics VM extension</span></span>
<span data-ttu-id="3c761-161">Általában kap egy üzenetet, ha a probléma, az Azure portálon vagy az Azure powershell.</span><span class="sxs-lookup"><span data-stu-id="3c761-161">Usually you receive a message when things don't work, from either Azure portal or Azure powershell.</span></span>

1. <span data-ttu-id="3c761-162">Jelentkezzen be a hello [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3c761-162">Sign into hello [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="3c761-163">Hello virtuális gép található, és nyissa meg a virtuális gép részletei.</span><span class="sxs-lookup"><span data-stu-id="3c761-163">Find hello VM and open VM details.</span></span>
3. <span data-ttu-id="3c761-164">Kattintson a **bővítmények** toocheck, ha az OMS-bővítmény engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="3c761-164">Click **Extensions** toocheck if OMS extension is enabled or not.</span></span>

   ![Virtuálisgép-bővítmények nézetet](./media/log-analytics-azure-vm-extension/oms-vmview-extensions.png)

4. <span data-ttu-id="3c761-166">Kattintson a hello *MicrosoftMonitoringAgent*(Windows) vagy *OmsAgentForLinux*(Linux) bővítményt, és a nézet részletek.</span><span class="sxs-lookup"><span data-stu-id="3c761-166">Click hello *MicrosoftMonitoringAgent*(Windows) or *OmsAgentForLinux*(Linux) extension and view details.</span></span> 

   ![Virtuálisgép-bővítmény részletei](./media/log-analytics-azure-vm-extension/oms-vmview-extensiondetails.png)

### <a name="troubleshooting-windows-virtual-machines"></a><span data-ttu-id="3c761-168">Hibaelhárítás Windows virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="3c761-168">Troubleshooting Windows Virtual Machines</span></span>
<span data-ttu-id="3c761-169">Ha hello *Microsoft Monitoring Agent* nem telepíti a Virtuálisgép-ügynök bővítmény vagy reporting, a következő lépéseket tootroubleshoot hello probléma hello hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="3c761-169">If hello *Microsoft Monitoring Agent* VM agent extension is not installing or reporting, you can perform hello following steps tootroubleshoot hello issue.</span></span>

1. <span data-ttu-id="3c761-170">Ellenőrizze, hogy hello Azure Virtuálisgép-ügynök telepítve van és megfelelően hello segítségével működő szükséges lépések [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).</span><span class="sxs-lookup"><span data-stu-id="3c761-170">Check if hello Azure VM agent is installed and working correctly by using hello steps in [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).</span></span>
   * <span data-ttu-id="3c761-171">Emellett áttekintheti hello VM ügynök naplófájlját.`C:\WindowsAzure\logs\WaAppAgent.log`</span><span class="sxs-lookup"><span data-stu-id="3c761-171">You can also review hello VM agent log file `C:\WindowsAzure\logs\WaAppAgent.log`</span></span>
   * <span data-ttu-id="3c761-172">Ha hello napló nem létezik, hello Virtuálisgép-ügynök nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="3c761-172">If hello log does not exist, hello VM agent is not installed.</span></span>
     * [<span data-ttu-id="3c761-173">Hello Azure Virtuálisgép-ügynök telepítése a klasszikus virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="3c761-173">Install hello Azure VM Agent on classic VMs</span></span>](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. <span data-ttu-id="3c761-174">Erősítse meg a lépéseket követve hello bővítmény szívverés feladat fut a Microsoft Monitoring Agent hello:</span><span class="sxs-lookup"><span data-stu-id="3c761-174">Confirm hello Microsoft Monitoring Agent extension heartbeat task is running using hello following steps:</span></span>
   * <span data-ttu-id="3c761-175">Toohello virtuálisgép jelentkezni</span><span class="sxs-lookup"><span data-stu-id="3c761-175">Log in toohello virtual machine</span></span>
   * <span data-ttu-id="3c761-176">Nyissa meg a Feladatütemezőt, és megkeresi hello `update_azureoperationalinsight_agent_heartbeat` feladat</span><span class="sxs-lookup"><span data-stu-id="3c761-176">Open task scheduler and find hello `update_azureoperationalinsight_agent_heartbeat` task</span></span>
   * <span data-ttu-id="3c761-177">Erősítse meg hello feladat engedélyezve van, és egy percenként fut.</span><span class="sxs-lookup"><span data-stu-id="3c761-177">Confirm hello task is enabled and is running every one minute</span></span>
   * <span data-ttu-id="3c761-178">Ellenőrizze, hogy hello szívverés logfile szerepel`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`</span><span class="sxs-lookup"><span data-stu-id="3c761-178">Check hello heartbeat logfile in `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`</span></span>
3. <span data-ttu-id="3c761-179">A hibaelhárításhoz hello Microsoft Monitoring Agent VM kiterjesztés található`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`</span><span class="sxs-lookup"><span data-stu-id="3c761-179">Review hello Microsoft Monitoring Agent VM extension log files in `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`</span></span>
4. <span data-ttu-id="3c761-180">Győződjön meg arról, hello virtuális gép PowerShell-parancsfájlok is futtathatók.</span><span class="sxs-lookup"><span data-stu-id="3c761-180">Ensure hello virtual machine can run PowerShell scripts</span></span>
5. <span data-ttu-id="3c761-181">Győződjön meg arról, nem módosította a C:\Windows\temp engedélyeinek</span><span class="sxs-lookup"><span data-stu-id="3c761-181">Ensure permissions on C:\Windows\temp haven’t been changed</span></span>
6. <span data-ttu-id="3c761-182">A Microsoft Monitoring Agent hello írja be a következő parancsot egy emelt szintű PowerShell-ablakban hello virtuális gépen hello hello állapotának megtekintése`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`</span><span class="sxs-lookup"><span data-stu-id="3c761-182">View hello status of hello Microsoft Monitoring Agent by typing hello following command in an elevated PowerShell window on hello virtual machine `  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`</span></span>
7. <span data-ttu-id="3c761-183">Hello Microsoft Monitoring Agent telepítő naplófájljaiban a`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`</span><span class="sxs-lookup"><span data-stu-id="3c761-183">Review hello Microsoft Monitoring Agent setup log files in `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`</span></span>

<span data-ttu-id="3c761-184">További információkért lásd: [hibaelhárítása a Windows](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3c761-184">For more information, see [troubleshooting Windows extensions](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="troubleshooting-linux-virtual-machines"></a><span data-ttu-id="3c761-185">Linux virtuális gépek hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="3c761-185">Troubleshooting Linux Virtual Machines</span></span>
<span data-ttu-id="3c761-186">Ha hello *Linux OMS-ügynököt* nem telepíti a Virtuálisgép-ügynök bővítmény vagy reporting, a következő lépéseket tootroubleshoot hello probléma hello hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="3c761-186">If hello *OMS Agent for Linux* VM agent extension is not installing or reporting, you can perform hello following steps tootroubleshoot hello issue.</span></span>

1. <span data-ttu-id="3c761-187">Ha hello bővítmény állapota *ismeretlen* hello Azure Virtuálisgép-ügynök telepítésének ellenőrzése és megfelelően működik-e hello VM ügynök naplófájlját megtekintésével`/var/log/waagent.log`</span><span class="sxs-lookup"><span data-stu-id="3c761-187">If hello extension status is *Unknown* check if hello Azure VM agent is installed and working correctly by reviewing hello VM agent log file `/var/log/waagent.log`</span></span>
   * <span data-ttu-id="3c761-188">Ha hello napló nem létezik, hello Virtuálisgép-ügynök nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="3c761-188">If hello log does not exist, hello VM agent is not installed.</span></span>
   * [<span data-ttu-id="3c761-189">Hello Azure Virtuálisgép-ügynök telepítése Linux virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="3c761-189">Install hello Azure VM Agent on Linux VMs</span></span>](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. <span data-ttu-id="3c761-190">A többi nem kifogástalan állapot felülvizsgálati hello OMS-ügynököt Linux Virtuálisgép-bővítmény naplófájljai `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` és`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`</span><span class="sxs-lookup"><span data-stu-id="3c761-190">For other unhealthy statuses, review hello OMS Agent for Linux VM extension logs files in `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` and `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`</span></span>
3. <span data-ttu-id="3c761-191">Ha hello bővítmény állapota kifogástalan, de nem feltöltött adatok áttekintéséhez hello OMS-ügynököt, a Linux a naplófájlokat`/var/opt/microsoft/omsagent/log/omsagent.log`</span><span class="sxs-lookup"><span data-stu-id="3c761-191">If hello extension status is healthy, but data is not being uploaded review hello OMS Agent for Linux log files in `/var/opt/microsoft/omsagent/log/omsagent.log`</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c761-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3c761-192">Next steps</span></span>
* <span data-ttu-id="3c761-193">Konfigurálása [Naplóelemzési adatforrások](log-analytics-data-sources.md) toospecify hello naplók és a metrikák toocollect.</span><span class="sxs-lookup"><span data-stu-id="3c761-193">Configure [data sources in Log Analytics](log-analytics-data-sources.md) toospecify hello logs and metrics toocollect.</span></span>
* <span data-ttu-id="3c761-194">a virtuális gépekről toogather adatok [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="3c761-194">toogather data from virtual machines [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
* <span data-ttu-id="3c761-195">[A Azure diagnosztikai adatok gyűjtéséhez](log-analytics-azure-storage.md) más Azure-ban futó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="3c761-195">[Collect data by using Azure Diagnostics](log-analytics-azure-storage.md) for other resources that are running in Azure.</span></span>

<span data-ttu-id="3c761-196">A számítógépek számára, amelyek nem az Azure-ban elvégezheti a telepítést hello Naplóelemzési ügynök hello a következő cikkekben ismertetett módszerek hello használata:</span><span class="sxs-lookup"><span data-stu-id="3c761-196">For computers that are not in Azure, you can install hello Log Analytics agent by using hello methods that are described in hello following articles:</span></span>

* [<span data-ttu-id="3c761-197">Csatlakozás a Windows-számítógépek tooLog elemzés</span><span class="sxs-lookup"><span data-stu-id="3c761-197">Connect Windows computers tooLog Analytics</span></span>](log-analytics-windows-agents.md)
* [<span data-ttu-id="3c761-198">Csatlakozás a Linux rendszerű számítógépek tooLog elemzés</span><span class="sxs-lookup"><span data-stu-id="3c761-198">Connect Linux computers tooLog Analytics</span></span>](log-analytics-linux-agents.md)
