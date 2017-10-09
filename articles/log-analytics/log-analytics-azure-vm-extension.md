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
# <a name="connect-azure-virtual-machines-toolog-analytics-with-a-log-analytics-agent"></a>Csatlakozás az Azure virtuális gépek tooLog Analytics egy Log Analytics-ügynök

A Windows és Linux számítógépekre hello ajánlott módszer a naplógyűjtés időtartamát és metrikákat hello Naplóelemzési ügynök telepítésével.

hello legegyszerűbb módja tooinstall hello Naplóelemzési ügynök az Azure virtuális gépeken hello napló Analytics Virtuálisgép-bővítmény keresztül történik.  Hello kiterjesztéssel leegyszerűsíti hello telepítési folyamatot, és automatikusan konfigurálja a hello ügynök toosend adatok toohello Naplóelemzési munkaterület megadott. hello ügynök is frissítése automatikusan történik, győződjön meg arról, hogy rendelkezik a legújabb szolgáltatásokat hello és javításokat.

Windows virtuális gépek esetében engedélyezi hello *Microsoft Monitoring Agent* virtuálisgép-bővítményt.
A Linux virtuális gépekhez, engedélyezi a hello *OMS ügynök a Linux* virtuálisgép-bővítményt.

További információ [Azure virtuálisgép-bővítmények](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és hello [Linux-ügynök](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ha az ügynök-alapú gyűjtemény naplóadatokat használja, konfigurálnia kell [Naplóelemzési adatforrások](log-analytics-data-sources.md) toospecify hello naplókat, valamint, hogy szeretné-e toocollect metrikákat.

> [!IMPORTANT]
> Ha konfigurálását a Naplóelemzési tooindex naplóadatok [Azure diagnostics](log-analytics-azure-storage.md), és konfigurálnia hello ügynök toocollect hello azonos naplózza, akkor hello naplók összegyűjtését kétszer. Mindkét adatforrások van szó. Hello ügynök telepítve van, ha csak hello ügynök a napló adatgyűjtés - ne állítson be az Azure diagnostics Naplóelemzési toocollect naplóadatait.
>
>

Háromféleképpen könnyen tooenable hello Naplóelemzési virtuálisgép-bővítmény:

* Hello Azure-portál használatával
* Azure PowerShell használatával
* Az Azure Resource Manager-sablon használatával

## <a name="enable-hello-vm-extension-in-hello-azure-portal"></a>Az Azure-portálon hello hello Virtuálisgép-bővítmény engedélyezése
Naplóelemzési hello-ügynök telepítése, és csatlakozzon hello Azure virtuális gép futó, hello segítségével [Azure-portálon](https://portal.azure.com).

### <a name="tooinstall-hello-log-analytics-agent-and-connect-hello-virtual-machine-tooa-log-analytics-workspace"></a>tooinstall hello Naplóelemzési ügynök, és csatlakozzon a hello virtuális gép tooa Naplóelemzési munkaterület
1. Jelentkezzen be a hello [Azure-portálon](http://portal.azure.com).
2. Válassza ki **Tallózás** hello a bal oldali hello oldalán portál, és nyissa meg majd túl**Naplóelemzés (OMS)** , és jelölje ki.
3. A Naplóelemzési munkaterület, jelölje ki, hogy szeretné-e az Azure virtuális gép hello toouse egy hello.  
   ![OMS-munkaterület](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. A **analytics management naplózása**, jelölje be **virtuális gépek**.  
   ![Virtuális gépek](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5. Hello listájában **virtuális gépek**, válassza ki a virtuális gépre, amelyen szeretné tooinstall hello ügynök hello. Hello **OMS kapcsolat állapota** hello VM azt jelöli, hogy nem **nincs csatlakoztatva**.  
   ![Virtuális gép nincs csatlakoztatva](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6. Válassza ki a virtuális gép részletei hello, **Connect**. hello ügynök automatikusan telepítve és beállítva a Naplóelemzési munkaterület. Ez a folyamat néhány percet vesz igénybe, amely idő alatt hello OMS kapcsolat állapota van *csatlakozás...*  
   ![Csatlakozás a virtuális gép](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7. Miután telepíti, és csatlakozni tud hello ügynök hello **OMS kapcsolat** állapota lesz frissített tooshow **a munkaterület**.  
   ![Csatlakoztatva](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)

## <a name="enable-hello-vm-extension-using-powershell"></a>PowerShell-lel hello Virtuálisgép-bővítmény engedélyezése
A virtuális gép PowerShell használatával való konfigurálásakor kell tooprovide hello **workspaceId** és **workspaceKey**. a json-konfigurációt hello tulajdonságnevek megkülönböztetik **kis-és nagybetűket**.

Hello azonosítója és a hello kulcs található **beállítások** oldalán hello OMS-portálon, vagy a PowerShell használatával, ahogy az előző példa hello.

![Munkaterületének Azonosítóját és az elsődleges kulcs](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

Nincsenek Azure klasszikus virtuális gépek és a Resource Manager virtuális gépek különböző parancsokat. Az alábbiakban például a klasszikus és Resource Manager virtuális gépeket.

Klasszikus virtuális gépek esetében a következő PowerShell-példa hello használata:

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

Erőforrás-kezelő Linux virtuális gépek hello a következő parancssori felület használatával
```azurecli
az vm extension set --resource-group myRGMonitor --vm-name myMonitorVM --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --version 1.3 --protected-settings ‘{"workspaceKey": "<workspace-key>"}’ --settings ‘{"workspaceId": "<workspace-id>"}’ 
```

Erőforrás-kezelő virtuális gépek esetében a következő PowerShell-példa hello használata:

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


## <a name="deploy-hello-vm-extension-using-a-template"></a>Egy sablon használatával hello Virtuálisgép-bővítmény telepítése
Azure Resource Manager segítségével létrehozhat egy sablont (JSON formátumban), amely meghatározza az alkalmazás a hello telepítését és konfigurálását. Ez a sablon Resource Manager sablonként is ismert és deklaratív módon toodefine központi telepítés biztosít. Egy sablon használatával ismételten telepítheti az alkalmazást teljes hello alkalmazás-életciklus és biztos lehet abban, hogy az erőforrások telepítése konzisztens lesz folyamatban.

Hello Naplóelemzési ügynök beleértve a Resource Manager-sablon részeként, biztosítható, hogy minden virtuális géphez-e az előre konfigurált tooreport tooyour Naplóelemzési munkaterület.

Erőforrás-kezelő sablonokkal kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).

Az alábbiakban látható egy példa a Resource Manager-sablont, amely egy virtuális gépet, hogy fut-e a Windows hello-kiterjesztés telepítése a Microsoft Monitoring Agent telepítéséhez használt. Ez a sablon a következő tipikus virtuálisgép-sablon, a következő kiegészítéseket hello:

* workspaceId és workspaceName paraméterek
* Erőforrás-kiterjesztésben megadottaknak Microsoft.EnterpriseCloud.Monitoring
* Kimenetek toolook hello workspaceId és workspaceSharedKey

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

A sablon a következő PowerShell-paranccsal hello segítségével telepíthet:

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-hello-log-analytics-vm-extension"></a>Hibaelhárítás hello napló Analytics Virtuálisgép-bővítmény
Általában kap egy üzenetet, ha a probléma, az Azure portálon vagy az Azure powershell.

1. Jelentkezzen be a hello [Azure-portálon](http://portal.azure.com).
2. Hello virtuális gép található, és nyissa meg a virtuális gép részletei.
3. Kattintson a **bővítmények** toocheck, ha az OMS-bővítmény engedélyezve van-e.

   ![Virtuálisgép-bővítmények nézetet](./media/log-analytics-azure-vm-extension/oms-vmview-extensions.png)

4. Kattintson a hello *MicrosoftMonitoringAgent*(Windows) vagy *OmsAgentForLinux*(Linux) bővítményt, és a nézet részletek. 

   ![Virtuálisgép-bővítmény részletei](./media/log-analytics-azure-vm-extension/oms-vmview-extensiondetails.png)

### <a name="troubleshooting-windows-virtual-machines"></a>Hibaelhárítás Windows virtuális gépek
Ha hello *Microsoft Monitoring Agent* nem telepíti a Virtuálisgép-ügynök bővítmény vagy reporting, a következő lépéseket tootroubleshoot hello probléma hello hajthat végre.

1. Ellenőrizze, hogy hello Azure Virtuálisgép-ügynök telepítve van és megfelelően hello segítségével működő szükséges lépések [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * Emellett áttekintheti hello VM ügynök naplófájlját.`C:\WindowsAzure\logs\WaAppAgent.log`
   * Ha hello napló nem létezik, hello Virtuálisgép-ügynök nincs telepítve.
     * [Hello Azure Virtuálisgép-ügynök telepítése a klasszikus virtuális gépeken](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. Erősítse meg a lépéseket követve hello bővítmény szívverés feladat fut a Microsoft Monitoring Agent hello:
   * Toohello virtuálisgép jelentkezni
   * Nyissa meg a Feladatütemezőt, és megkeresi hello `update_azureoperationalinsight_agent_heartbeat` feladat
   * Erősítse meg hello feladat engedélyezve van, és egy percenként fut.
   * Ellenőrizze, hogy hello szívverés logfile szerepel`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. A hibaelhárításhoz hello Microsoft Monitoring Agent VM kiterjesztés található`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Győződjön meg arról, hello virtuális gép PowerShell-parancsfájlok is futtathatók.
5. Győződjön meg arról, nem módosította a C:\Windows\temp engedélyeinek
6. A Microsoft Monitoring Agent hello írja be a következő parancsot egy emelt szintű PowerShell-ablakban hello virtuális gépen hello hello állapotának megtekintése`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Hello Microsoft Monitoring Agent telepítő naplófájljaiban a`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

További információkért lásd: [hibaelhárítása a Windows](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="troubleshooting-linux-virtual-machines"></a>Linux virtuális gépek hibaelhárítása
Ha hello *Linux OMS-ügynököt* nem telepíti a Virtuálisgép-ügynök bővítmény vagy reporting, a következő lépéseket tootroubleshoot hello probléma hello hajthat végre.

1. Ha hello bővítmény állapota *ismeretlen* hello Azure Virtuálisgép-ügynök telepítésének ellenőrzése és megfelelően működik-e hello VM ügynök naplófájlját megtekintésével`/var/log/waagent.log`
   * Ha hello napló nem létezik, hello Virtuálisgép-ügynök nincs telepítve.
   * [Hello Azure Virtuálisgép-ügynök telepítése Linux virtuális gépeken](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. A többi nem kifogástalan állapot felülvizsgálati hello OMS-ügynököt Linux Virtuálisgép-bővítmény naplófájljai `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` és`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Ha hello bővítmény állapota kifogástalan, de nem feltöltött adatok áttekintéséhez hello OMS-ügynököt, a Linux a naplófájlokat`/var/opt/microsoft/omsagent/log/omsagent.log`

## <a name="next-steps"></a>Következő lépések
* Konfigurálása [Naplóelemzési adatforrások](log-analytics-data-sources.md) toospecify hello naplók és a metrikák toocollect.
* a virtuális gépekről toogather adatok [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).
* [A Azure diagnosztikai adatok gyűjtéséhez](log-analytics-azure-storage.md) más Azure-ban futó erőforrások.

A számítógépek számára, amelyek nem az Azure-ban elvégezheti a telepítést hello Naplóelemzési ügynök hello a következő cikkekben ismertetett módszerek hello használata:

* [Csatlakozás a Windows-számítógépek tooLog elemzés](log-analytics-windows-agents.md)
* [Csatlakozás a Linux rendszerű számítógépek tooLog elemzés](log-analytics-linux-agents.md)
