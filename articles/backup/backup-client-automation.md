---
title: "Windows Server mentésére az Azure PowerShell használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan telepíthetnek és kezelhetnek az Azure Backup szolgáltatáshoz a PowerShell használatával"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 65218095-2996-44d9-917b-8c84fc9ac415
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: saurse;markgal;jimpark;nkolli;trinadhk
ms.openlocfilehash: d3f165c749af0553c4918b33b0d24cc1e21af2a9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a><span data-ttu-id="fd3b4-103">Az Azure-ba történő biztonsági mentés üzembe helyezése és kezelése Windows Server vagy Windows-ügyfél rendszereken a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="fd3b4-103">Deploy and manage backup to Azure for Windows Server/Windows Client using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fd3b4-104">ARM</span><span class="sxs-lookup"><span data-stu-id="fd3b4-104">ARM</span></span>](backup-client-automation.md)
> * [<span data-ttu-id="fd3b4-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="fd3b4-105">Classic</span></span>](backup-client-automation-classic.md)
>
>

<span data-ttu-id="fd3b4-106">Ez a cikk bemutatja, hogyan használja a Powershellt beállítása az Azure Backup szolgáltatás a Windows Server vagy egy Windows ügyfél és a biztonsági mentés és helyreállítás felügyelete.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-106">This article shows you how to use PowerShell for setting up Azure Backup on Windows Server or a Windows client, and managing backup and recovery.</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="fd3b4-107">Az Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="fd3b4-107">Install Azure PowerShell</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="fd3b4-108">Ez a cikk az Azure Resource Managerrel (ARM) és a MS Online biztonsági mentés PowerShell-parancsmagokat, amelyek lehetővé teszik egy erőforráscsoportot a Recovery Services-tároló használatára szolgál.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-108">This article focuses on the Azure Resource Manager (ARM) and the MS Online Backup PowerShell cmdlets that enable you to use a Recovery Services vault in a resource group.</span></span>

<span data-ttu-id="fd3b4-109">2015 október, az Azure PowerShell 1.0-s verziójában jelent meg.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-109">In October 2015, Azure PowerShell 1.0 was released.</span></span> <span data-ttu-id="fd3b4-110">Ebben a kiadásban a 0.9.8-as sikeres szabadítsa fel, és néhány jelentős módosításokat, különösen a parancsmagok elnevezési mintát állapotba.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-110">This release succeeded the 0.9.8 release and brought about some significant changes, especially in the naming pattern of the cmdlets.</span></span> <span data-ttu-id="fd3b4-111">Az 1.0-ás parancsmagok az {ige}-AzureRm{főnév} elnevezési mintát használják; a 0.9.8-as nevek viszont nem tartalmazzák az **Rm** tagot (például New-AzureRmResourceGroup New-AzureResourceGroup helyett).</span><span class="sxs-lookup"><span data-stu-id="fd3b4-111">1.0 cmdlets follow the naming pattern {verb}-AzureRm{noun}; whereas, the 0.9.8 names do not include **Rm** (for example, New-AzureRmResourceGroup instead of New-AzureResourceGroup).</span></span> <span data-ttu-id="fd3b4-112">Ha az Azure PowerShell 0.9.8-as verzióját használja, először engedélyeznie kell az erőforrás-kezelői módot a **Switch-AzureMode AzureResourceManager** parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-112">When using Azure PowerShell 0.9.8, you must first enable the Resource Manager mode by running the **Switch-AzureMode AzureResourceManager** command.</span></span> <span data-ttu-id="fd3b4-113">Ez a parancs nincs szükség az 1.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-113">This command is not necessary in 1.0 or later.</span></span>

<span data-ttu-id="fd3b4-114">Ha a parancsfájlok a 0.9.8-as írt használni kívánt környezet, 1.0-s vagy újabb környezetben kell gondosan frissíti, és a parancsfájlok tesztelése egy üzem előtti környezetben váratlan hatás elkerülése érdekében az éles használat előtt.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-114">If you want to use your scripts written for the 0.9.8 environment, in the 1.0 or later environment, you should carefully update and test the scripts in a pre-production environment before using them in production to avoid unexpected impact.</span></span>

<span data-ttu-id="fd3b4-115">[Töltse le a legfrissebb verzióját a PowerShell](https://github.com/Azure/azure-powershell/releases) (szükséges minimális verziója: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="fd3b4-115">[Download the latest PowerShell release](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="fd3b4-116">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd3b4-116">Create a recovery services vault</span></span>
<span data-ttu-id="fd3b4-117">A következő lépések alapján a Recovery Services-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-117">The following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="fd3b4-118">Recovery Services-tároló nem egyezik egy biztonsági mentési tárolót.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-118">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="fd3b4-119">Ha az Azure biztonsági mentés először használ, kell használnia a **Register-AzureRMResourceProvider** parancsmag futtatásával regisztrálja az Azure Recovery szolgáltató az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-119">If you are using Azure Backup for the first time, you must use the **Register-AzureRMResourceProvider** cmdlet to register the Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="fd3b4-120">A Recovery Services-tároló egy ARM-erőforrás, ezért el kell helyezni az erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-120">The Recovery Services vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="fd3b4-121">Használjon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-121">You can use an existing resource group, or create a new one.</span></span> <span data-ttu-id="fd3b4-122">Új erőforráscsoport létrehozása esetén adja meg a nevét és helyét, ahhoz az erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-122">When creating a new resource group, specify the name and location for the resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "WestUS"
    ```
3. <span data-ttu-id="fd3b4-123">Használja a **New-AzureRmRecoveryServicesVault** parancsmaggal hozhat létre az új tárolóba.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-123">Use the **New-AzureRmRecoveryServicesVault** cmdlet to create the new vault.</span></span> <span data-ttu-id="fd3b4-124">Ne felejtse el ugyanazon a helyen, a tároló adja meg, mint az erőforráscsoport használt.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-124">Be sure to specify the same location for the vault as was used for the resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "WestUS"
    ```
4. <span data-ttu-id="fd3b4-125">Megadhatja a használandó; adattároló redundanciája, amely használhat [helyileg redundáns tárolás (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) vagy [földrajzi redundáns tárolás (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="fd3b4-125">Specify the type of storage redundancy to use; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="fd3b4-126">A következő példa bemutatja a - BackupStorageRedundancy beállítás a testVault GeoRedundant értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-126">The following example shows the -BackupStorageRedundancy option for testVault is set to GeoRedundant.</span></span>

   > [!TIP]
   > <span data-ttu-id="fd3b4-127">Sok Azure biztonsági mentést készítő parancsmagok bemeneti adatokként a Recovery Services-tároló objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-127">Many Azure Backup cmdlets require the Recovery Services vault object as an input.</span></span> <span data-ttu-id="fd3b4-128">Emiatt célszerű a Recovery Services biztonsági másolat tároló objektum tárolható egy változóban.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-128">For this reason, it is convenient to store the Backup Recovery Services vault object in a variable.</span></span>
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a><span data-ttu-id="fd3b4-129">A tárolók előfizetés megtekintése</span><span class="sxs-lookup"><span data-stu-id="fd3b4-129">View the vaults in a subscription</span></span>
<span data-ttu-id="fd3b4-130">Használjon **Get-AzureRmRecoveryServicesVault** megtekintéséhez az összes tárolók listája az aktuális előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-130">Use **Get-AzureRmRecoveryServicesVault** to view the list of all vaults in the current subscription.</span></span> <span data-ttu-id="fd3b4-131">Ezt a parancsot használhatja, ellenőrizze, hogy létrejött-e egy új tárolót, vagy, hogy milyen tárolók-előfizetésben elérhető.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-131">You can use this command to check that a new  vault was created, or to see what vaults are available in the subscription.</span></span>

<span data-ttu-id="fd3b4-132">Futtassa a parancsot, **Get-AzureRmRecoveryServicesVault**, és az előfizetés összes tárolók vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-132">Run the command, **Get-AzureRmRecoveryServicesVault**, and all vaults in the subscription are listed.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="installing-the-azure-backup-agent"></a><span data-ttu-id="fd3b4-133">Az Azure Backup szolgáltatás ügynökének telepítése</span><span class="sxs-lookup"><span data-stu-id="fd3b4-133">Installing the Azure Backup agent</span></span>
<span data-ttu-id="fd3b4-134">Az Azure Backup szolgáltatás ügynökének telepítése előtt kell rendelkeznie a telepítő letöltött és található a Windows Server.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-134">Before you install the Azure Backup agent, you need to have the installer downloaded and present on the Windows Server.</span></span> <span data-ttu-id="fd3b4-135">Kaphat, hogy a telepítő a legújabb verzióját a [Microsoft Download Center](http://aka.ms/azurebackup_agent) vagy a Recovery Services-tároló irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-135">You can get the latest version of the installer from the [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from the Recovery Services vault's Dashboard page.</span></span> <span data-ttu-id="fd3b4-136">A telepítő menteni egy könnyen elérhető helyre, például a * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-136">Save the installer to an easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="fd3b4-137">Azt is megteheti a PowerShell használatával a letöltési segédprogramját beolvasása:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-137">Alternatively, use PowerShell to get the downloader:</span></span>
 
 ```
 $MarsAURL = 'Http://Aka.Ms/Azurebackup_Agent'
 $WC = New-Object System.Net.WebClient
 $WC.DownloadFile($MarsAURL,'C:\downloads\MARSAgentInstaller.EXE')
 C:\Downloads\MARSAgentInstaller.EXE /q
 ```

<span data-ttu-id="fd3b4-138">Az ügynök telepítéséhez futtassa a következő parancsot egy emelt szintű PowerShell-konzolon:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-138">To install the agent, run the following command in an elevated PowerShell console:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="fd3b4-139">Ezzel telepíti az ügynököt az összes alapértelmezett beállítást.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-139">This installs the agent with all the default options.</span></span> <span data-ttu-id="fd3b4-140">A telepítés néhány percet vesz igénybe a háttérben.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-140">The installation takes a few minutes in the background.</span></span> <span data-ttu-id="fd3b4-141">Ha nem adja meg a */nu* lehetőséget, majd a **Windows Update** ablak nyílik meg a frissítések keresését a telepítés végén.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-141">If you do not specify the */nu* option then the **Windows Update** window will open at the end of the installation to check for any updates.</span></span> <span data-ttu-id="fd3b4-142">A telepítést követően az ügynök a telepített programok listájában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-142">Once installed, the agent will show in the list of installed programs.</span></span>

<span data-ttu-id="fd3b4-143">A telepített programok listájának megtekintéséhez keresse fel **Vezérlőpult** > **programok** > **programok és szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-143">To see the list of installed programs, go to **Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Az ügynök telepítve](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="fd3b4-145">Telepítési beállítások</span><span class="sxs-lookup"><span data-stu-id="fd3b4-145">Installation options</span></span>
<span data-ttu-id="fd3b4-146">A parancssori keresztül elérhető lehetőségekről, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-146">To see all the options available via the command-line, use the following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="fd3b4-147">Az elérhető lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-147">The available options include:</span></span>

| <span data-ttu-id="fd3b4-148">Beállítás</span><span class="sxs-lookup"><span data-stu-id="fd3b4-148">Option</span></span> | <span data-ttu-id="fd3b4-149">Részletek</span><span class="sxs-lookup"><span data-stu-id="fd3b4-149">Details</span></span> | <span data-ttu-id="fd3b4-150">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="fd3b4-150">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fd3b4-151">/q</span><span class="sxs-lookup"><span data-stu-id="fd3b4-151">/q</span></span> |<span data-ttu-id="fd3b4-152">Csendes telepítés</span><span class="sxs-lookup"><span data-stu-id="fd3b4-152">Quiet installation</span></span> |- |
| <span data-ttu-id="fd3b4-153">/ p: "hely"</span><span class="sxs-lookup"><span data-stu-id="fd3b4-153">/p:"location"</span></span> |<span data-ttu-id="fd3b4-154">Az Azure Backup szolgáltatás ügynökének a telepítési mappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-154">Path to the installation folder for the Azure Backup agent.</span></span> |<span data-ttu-id="fd3b4-155">C:\Program Files\Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="fd3b4-155">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="fd3b4-156">/ s: "hely"</span><span class="sxs-lookup"><span data-stu-id="fd3b4-156">/s:"location"</span></span> |<span data-ttu-id="fd3b4-157">Az Azure Backup szolgáltatás ügynökének a gyorsítótár mappájának elérési útja.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-157">Path to the cache folder for the Azure Backup agent.</span></span> |<span data-ttu-id="fd3b4-158">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="fd3b4-158">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="fd3b4-159">/m</span><span class="sxs-lookup"><span data-stu-id="fd3b4-159">/m</span></span> |<span data-ttu-id="fd3b4-160">Részvétel a Microsoft Update</span><span class="sxs-lookup"><span data-stu-id="fd3b4-160">Opt-in to Microsoft Update</span></span> |- |
| <span data-ttu-id="fd3b4-161">/Nu</span><span class="sxs-lookup"><span data-stu-id="fd3b4-161">/nu</span></span> |<span data-ttu-id="fd3b4-162">Ne keressen frissítéseket telepítésének befejezése után</span><span class="sxs-lookup"><span data-stu-id="fd3b4-162">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="fd3b4-163">/d</span><span class="sxs-lookup"><span data-stu-id="fd3b4-163">/d</span></span> |<span data-ttu-id="fd3b4-164">Eltávolítja a Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="fd3b4-164">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="fd3b4-165">/pH</span><span class="sxs-lookup"><span data-stu-id="fd3b4-165">/ph</span></span> |<span data-ttu-id="fd3b4-166">Állomás proxycím</span><span class="sxs-lookup"><span data-stu-id="fd3b4-166">Proxy Host Address</span></span> |- |
| <span data-ttu-id="fd3b4-167">/po</span><span class="sxs-lookup"><span data-stu-id="fd3b4-167">/po</span></span> |<span data-ttu-id="fd3b4-168">Proxy Host Port száma</span><span class="sxs-lookup"><span data-stu-id="fd3b4-168">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="fd3b4-169">/Pu</span><span class="sxs-lookup"><span data-stu-id="fd3b4-169">/pu</span></span> |<span data-ttu-id="fd3b4-170">Proxy-állomás felhasználónév</span><span class="sxs-lookup"><span data-stu-id="fd3b4-170">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="fd3b4-171">/pW</span><span class="sxs-lookup"><span data-stu-id="fd3b4-171">/pw</span></span> |<span data-ttu-id="fd3b4-172">Proxy jelszava</span><span class="sxs-lookup"><span data-stu-id="fd3b4-172">Proxy Password</span></span> |- |

## <a name="registering-windows-server-or-windows-client-machine-to-a-recovery-services-vault"></a><span data-ttu-id="fd3b4-173">Windows Server vagy a Windows ügyfél gép Recovery Services-tároló regisztrálása</span><span class="sxs-lookup"><span data-stu-id="fd3b4-173">Registering Windows Server or Windows client machine to a Recovery Services Vault</span></span>
<span data-ttu-id="fd3b4-174">A Recovery Services-tároló létrehozását követően töltse le a legújabb ügynököt és a tárolói hitelesítő adatokat, és C:\Downloads például egy tetszőleges helyen tárolja.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-174">After you created the Recovery Services vault, download the latest agent and the vault credentials and store it in a convenient location like C:\Downloads.</span></span>

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
```

<span data-ttu-id="fd3b4-175">A Windows Server vagy a Windows ügyfél gépén, futtassa a [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) parancsmag regisztrálni a gépet a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-175">On the Windows Server or Windows client machine, run the [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet to register the machine with the vault.</span></span>
<span data-ttu-id="fd3b4-176">Ezzel és más parancsmagok a biztonsági, amelyek a MSONLINE modulból, amely a Mars AgentInstaller hozzá a telepítési folyamat részeként.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-176">This, and other cmdlets used for backup, are from the MSONLINE module which the Mars AgentInstaller added as part of the installation process.</span></span> 

<span data-ttu-id="fd3b4-177">Az ügynök telepítőprogramját nem frissíti a $Env: PSModulePath változó.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-177">The Agent installer does not update the $Env:PSModulePath variable.</span></span> <span data-ttu-id="fd3b4-178">Ez azt jelenti, hogy a modul automatikus-betöltése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-178">This means module auto-load fails.</span></span> <span data-ttu-id="fd3b4-179">A probléma megoldásához a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-179">To resolve this you can do the following:</span></span>

```
PS C:\>  $Env:psmodulepath += ';C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules
```

<span data-ttu-id="fd3b4-180">Másik lehetőségként manuálisan betöltése a modul a parancsfájlban az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-180">Alternatively, you can manually load the module in your script as follows:</span></span>

```
PS C:\>  Import-Module  'C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup'

```

<span data-ttu-id="fd3b4-181">Az Online biztonsági mentést készítő parancsmagok tölthető be, ha regisztrál a tárolói hitelesítő adatokat:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-181">Once you load the Online Backup cmdlets, you register the vault credentials:</span></span>


```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :WestUS
Machine registration succeeded.
```

> [!IMPORTANT]
> <span data-ttu-id="fd3b4-182">Ne használjon relatív elérési utak adhatja meg a tároló hitelesítési adatait tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-182">Do not use relative paths to specify the vault credentials file.</span></span> <span data-ttu-id="fd3b4-183">A parancsmag bemenetként abszolút elérési utat kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-183">You must provide an absolute path as an input to the cmdlet.</span></span>
>
>

## <a name="networking-settings"></a><span data-ttu-id="fd3b4-184">Hálózati beállítások</span><span class="sxs-lookup"><span data-stu-id="fd3b4-184">Networking settings</span></span>
<span data-ttu-id="fd3b4-185">Ha a Windows számítógép az internethez csatlakoztatásához proxykiszolgálón keresztül, a proxykiszolgáló beállításait is megadható az ügynököt.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-185">When the connectivity of the Windows machine to the internet is through a proxy server, the proxy settings can also be provided to the agent.</span></span> <span data-ttu-id="fd3b4-186">Ebben a példában nincs nincs proxykiszolgáló, explicit módon azt vannak törlésével bármely proxy kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-186">In this example, there is no proxy server, so we are explicitly clearing any proxy-related information.</span></span>

<span data-ttu-id="fd3b4-187">Sávszélesség is szabályozható lehetőségét ```work hour bandwidth``` és ```non-work hour bandwidth``` napokat a hét adott számú.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-187">Bandwidth usage can also be controlled with the options of ```work hour bandwidth``` and ```non-work hour bandwidth``` for a given set of days of the week.</span></span>

<span data-ttu-id="fd3b4-188">Beállítás a proxy- és sávszélesség részletei történik használatával a [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-188">Setting the proxy and bandwidth details is done using the [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a><span data-ttu-id="fd3b4-189">Titkosítási beállítások</span><span class="sxs-lookup"><span data-stu-id="fd3b4-189">Encryption settings</span></span>
<span data-ttu-id="fd3b4-190">A biztonsági mentési Azure biztonsági mentési küldött adatok védelmét az adatok bizalmas mivoltát titkosított.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-190">The backup data sent to Azure Backup is encrypted to protect the confidentiality of the data.</span></span> <span data-ttu-id="fd3b4-191">A titkosítás jelszava a "password" vissza kell fejtenie az adatokat a visszaállítás időpontjában.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-191">The encryption passphrase is the "password" to decrypt the data at the time of restore.</span></span>

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
PS C:\> $PassPhrase = ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force 
PS C:\> $PassCode   = 'AzureR0ckx'
PS C:\> Set-OBMachineSetting -EncryptionPassPhrase $PassPhrase
Server properties updated successfully
```

> [!IMPORTANT]
> <span data-ttu-id="fd3b4-192">Jelszó információinak megtartása biztonságos be van állítva.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-192">Keep the passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="fd3b4-193">Vannak nem fogja tudni adatok visszaállítása az Azure-ból nélkül ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-193">You are not be able to restore data from Azure without this passphrase.</span></span>
>
>

## <a name="back-up-files-and-folders"></a><span data-ttu-id="fd3b4-194">Fájlok és mappák biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="fd3b4-194">Back up files and folders</span></span>
<span data-ttu-id="fd3b4-195">Az Azure Backup szolgáltatásba a Windows-kiszolgálók és ügyfelek összes biztonsági mentés a házirend vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-195">All backups from Windows Servers and clients to Azure Backup are governed by a policy.</span></span> <span data-ttu-id="fd3b4-196">A házirend három részből áll:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-196">The policy comprises three parts:</span></span>

1. <span data-ttu-id="fd3b4-197">A **biztonsági mentés ütemezése** , amely megadja, amikor biztonsági mentést kell venni és szinkronizálása a szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-197">A **backup schedule** that specifies when backups need to be taken and synchronized with the service.</span></span>
2. <span data-ttu-id="fd3b4-198">A **adatmegőrzési ütemterv** , amely megadja, mennyi ideig szeretné megőrizni a helyreállítási pontok az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-198">A **retention schedule** that specifies how long to retain the recovery points in Azure.</span></span>
3. <span data-ttu-id="fd3b4-199">A **belefoglalási/kizárási megadására** , amely határozzák meg, hogy mi készüljön biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-199">A **file inclusion/exclusion specification** that dictates what should be backed up.</span></span>

<span data-ttu-id="fd3b4-200">Ebben a dokumentumban mivel azt még automatizálása a biztonsági mentés, feltételezzük, semmi nem konfiguráltak.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-200">In this document, since we're automating backup, we'll assume nothing has been configured.</span></span> <span data-ttu-id="fd3b4-201">Hozzon létre egy új biztonsági mentési házirend használatával megkezdjük a [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-201">We begin by creating a new backup policy using the [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet.</span></span>

```
PS C:\> $newpolicy = New-OBPolicy
```

<span data-ttu-id="fd3b4-202">Jelenleg a házirend üres, és más parancsmagok olyan szükséges meghatározásához, hogy mely elemeket kell befoglalt, sem a kizárt, amikor biztonsági mentést fog futni, és egyes esetekben a biztonsági mentések tárolódik.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-202">At this time the policy is empty and other cmdlets are needed to define what items will be included or excluded, when backups will run, and where the backups will be stored.</span></span>

### <a name="configuring-the-backup-schedule"></a><span data-ttu-id="fd3b4-203">A biztonsági mentési ütemezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd3b4-203">Configuring the backup schedule</span></span>
<span data-ttu-id="fd3b4-204">Az első 3 részből álló házirend a biztonsági mentési ütemezést, és létre kell hozni a [New-OBSchedule](https://technet.microsoft.com/library/hh770401) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-204">The first of the 3 parts of a policy is the backup schedule, which is created using the [New-OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet.</span></span> <span data-ttu-id="fd3b4-205">A biztonsági mentés ütemezése határozza meg, amikor biztonsági mentést kell tenni.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-205">The backup schedule defines when backups need to be taken.</span></span> <span data-ttu-id="fd3b4-206">Ütemezés létrehozásakor meg kell adnia 2 bemeneti paraméterek:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-206">When creating a schedule you need to specify 2 input parameters:</span></span>

* <span data-ttu-id="fd3b4-207">**A hét napjai** , amely a biztonsági mentést kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-207">**Days of the week** that the backup should run.</span></span> <span data-ttu-id="fd3b4-208">A biztonsági mentési feladat csak egy napon, vagy a hét minden napján, vagy tetszőleges kombinációját a közötti futtatása.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-208">You can run the backup job on just one day, or every day of the week, or any combination in between.</span></span>
* <span data-ttu-id="fd3b4-209">**A nap** mikor fusson a biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-209">**Times of the day** when the backup should run.</span></span> <span data-ttu-id="fd3b4-210">Ha a biztonsági mentés akkor is kiváltódik 3 különböző napszakokban adhat meg.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-210">You can define up to 3 different times of the day when the backup will be triggered.</span></span>

<span data-ttu-id="fd3b4-211">Például konfigurálhatja a biztonsági mentési házirend du. 4: futtat minden szombat és vasárnap.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-211">For instance, you could configure a backup policy that runs at 4PM every Saturday and Sunday.</span></span>

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

<span data-ttu-id="fd3b4-212">A biztonsági mentés ütemezése kell lennie a házirendhez társított, és ez használatával érhető el a [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-212">The backup schedule needs to be associated with a policy, and this can be achieved by using the [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.</span></span>

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a><span data-ttu-id="fd3b4-213">Egy megőrzési házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd3b4-213">Configuring a retention policy</span></span>
<span data-ttu-id="fd3b4-214">A megőrzési házirend határozza meg, mennyi ideig megőrzi a biztonsági mentési feladatok létrehozott helyreállítási pontok.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-214">The retention policy defines how long recovery points created from backup jobs are retained.</span></span> <span data-ttu-id="fd3b4-215">Egy új megőrzési házirend használatával létrehozásakor a [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) parancsmaggal, megadhatja az, hogy hány napig őrzi meg az Azure biztonsági mentési kell a biztonsági mentések helyreállítási pontjait.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-215">When creating a new retention policy using the [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, you can specify the number of days that the backup recovery points need to be retained with Azure Backup.</span></span> <span data-ttu-id="fd3b4-216">Az alábbi példában 7 napos adatmegőrzési állítja be.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-216">The example below sets a retention policy of 7 days.</span></span>

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

<span data-ttu-id="fd3b4-217">Az adatmegőrzési hozzá kell rendelni a fő házirend-parancsmag segítségével [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span><span class="sxs-lookup"><span data-stu-id="fd3b4-217">The retention policy must be associated with the main policy using the cmdlet [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span></span>

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-to-be-backed-up"></a><span data-ttu-id="fd3b4-218">Többek között, és a fájlok biztonsági mentése nélkül</span><span class="sxs-lookup"><span data-stu-id="fd3b4-218">Including and excluding files to be backed up</span></span>
<span data-ttu-id="fd3b4-219">Egy ```OBFileSpec``` objektum meghatározása és a biztonsági másolat nem lehet a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-219">An ```OBFileSpec``` object defines the files to be included and excluded in a backup.</span></span> <span data-ttu-id="fd3b4-220">Ez a szabálykészlet, amely a védett fájlok és mappák gépen kimenő hatókör.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-220">This is a set of rules that scope out the protected files and folders on a machine.</span></span> <span data-ttu-id="fd3b4-221">Akkor is, számos befoglalási vagy kizárási szabály szükség szerint fájlt, és rendelje hozzá őket egy házirendet.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-221">You can have as many file inclusion or exclusion rules as required, and associate them with a policy.</span></span> <span data-ttu-id="fd3b4-222">Új OBFileSpec objektum létrehozásakor meg a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-222">When creating a new OBFileSpec object, you can:</span></span>

* <span data-ttu-id="fd3b4-223">Adja meg a fájlok és mappák része</span><span class="sxs-lookup"><span data-stu-id="fd3b4-223">Specify the files and folders to be included</span></span>
* <span data-ttu-id="fd3b4-224">Adja meg a fájlok és mappák ki lesznek zárva</span><span class="sxs-lookup"><span data-stu-id="fd3b4-224">Specify the files and folders to be excluded</span></span>
* <span data-ttu-id="fd3b4-225">Adja meg a rekurzív az adatok biztonsági mentése egy mappában (vagy) e a megadott mappában csak a legfelső szintű fájlok biztonsági másolatot kell fel.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-225">Specify recursive backup of data in a folder (or) whether only the top-level files in the specified folder should be backed up.</span></span>

<span data-ttu-id="fd3b4-226">Ez utóbbi használatával érhető el a nem rekurzív - jelzőt a New-OBFileSpec parancsban.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-226">The latter is achieved by using the -NonRecursive flag in the New-OBFileSpec command.</span></span>

<span data-ttu-id="fd3b4-227">Az alábbi példa azt fogja biztonsági mentése kötet C: és D: és a az operációs rendszer bináris ideiglenes mappákat a Windows mappában található fájlok kizárása.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-227">In the example below, we'll back up volume C: and D: and exclude the OS binaries in the Windows folder and any temporary folders.</span></span> <span data-ttu-id="fd3b4-228">Ennek érdekében hozzon létre két fájl használatával specifikációk a [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) parancsmag - befoglalási és kizárási egyet.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-228">To do so we'll create two file specifications using the [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - one for inclusion and one for exclusion.</span></span> <span data-ttu-id="fd3b4-229">A fájl specifikációk létrehozása után, fontosságúak a szabályzat használatához a [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-229">Once the file specifications have been created, they're associated with the policy using the [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.</span></span>

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-the-policy"></a><span data-ttu-id="fd3b4-230">A házirend alkalmazása</span><span class="sxs-lookup"><span data-stu-id="fd3b4-230">Applying the policy</span></span>
<span data-ttu-id="fd3b4-231">A csoportházirend-objektum most már befejeződött, és egy társított biztonsági mentés ütemezése, a megőrzési házirend és a fájlok belefoglalási/kizárási lista.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-231">Now the policy object is complete and has an associated backup schedule, retention policy, and an inclusion/exclusion list of files.</span></span> <span data-ttu-id="fd3b4-232">Ez a házirend most lehet véglegesíteni az Azure Backup használatához.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-232">This policy can now be committed for Azure Backup to use.</span></span> <span data-ttu-id="fd3b4-233">Alkalmazása előtt az újonnan létrehozott házirend gondoskodjon arról, hogy nincsenek-e a kiszolgálóhoz társított használatával meglévő biztonsági mentési házirendek a [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-233">Before you apply the newly created policy ensure that there are no existing backup policies associated with the server by using the [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet.</span></span> <span data-ttu-id="fd3b4-234">A házirend eltávolítása megerősítő fogja kérni.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-234">Removing the policy will prompt for confirmation.</span></span> <span data-ttu-id="fd3b4-235">Kihagyja a megerősítő használja a ```-Confirm:$false``` jelzőt mellékel a parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-235">To skip the confirmation use the ```-Confirm:$false``` flag with the cmdlet.</span></span>

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

<span data-ttu-id="fd3b4-236">A csoportházirend-objektum életbe léptetése történik használatával a [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-236">Committing the policy object is done using the [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet.</span></span> <span data-ttu-id="fd3b4-237">Ez is megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-237">This will also ask for confirmation.</span></span> <span data-ttu-id="fd3b4-238">Kihagyja a megerősítő használja a ```-Confirm:$false``` jelzőt mellékel a parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-238">To skip the confirmation use the ```-Confirm:$false``` flag with the cmdlet.</span></span>

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

<span data-ttu-id="fd3b4-239">A részleteket a meglévő biztonsági mentési házirend használatával megtekintheti a [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-239">You can view the details of the existing backup policy using the [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet.</span></span> <span data-ttu-id="fd3b4-240">Akkor is leásási használatával a [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) parancsmag a biztonsági mentési ütemezés és a [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) parancsmag az adatmegőrzési házirendek</span><span class="sxs-lookup"><span data-stu-id="fd3b4-240">You can drill-down further using the [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet for the backup schedule and the [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet for the retention policies</span></span>

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a><span data-ttu-id="fd3b4-241">Az ad hoc biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="fd3b4-241">Performing an ad-hoc backup</span></span>
<span data-ttu-id="fd3b4-242">Miután a biztonsági mentési házirend van beállítva a biztonsági mentések száma az ütemezésben fog létrejönni.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-242">Once a backup policy has been set the backups will occur per the schedule.</span></span> <span data-ttu-id="fd3b4-243">Az ad hoc biztonsági mentés indítására az is lehetséges használatával a [Start-OBBackup](https://technet.microsoft.com/library/hh770426) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-243">Triggering an ad-hoc backup is also possible using the [Start-OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:</span></span>

```
PS C:> Get-OBPolicy | Start-OBBackup
Initializing
Taking snapshot of volumes...
Preparing storage...
Generating backup metadata information and preparing the metadata VHD...
Data transfer is in progress. It might take longer since it is the first backup and all data needs to be transferred...
Data transfer completed and all backed up data is in the cloud. Verifying data integrity...
Data transfer completed
In progress...
Job completed.
The backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a><span data-ttu-id="fd3b4-244">Állítsa vissza az adatokat az Azure Backup</span><span class="sxs-lookup"><span data-stu-id="fd3b4-244">Restore data from Azure Backup</span></span>
<span data-ttu-id="fd3b4-245">Ez a szakasz végigvezeti az Azure biztonsági mentés az adatok helyreállítás automatizálása lépéseit.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-245">This section will guide you through the steps for automating recovery of data from Azure Backup.</span></span> <span data-ttu-id="fd3b4-246">Ennek során a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-246">Doing so involves the following steps:</span></span>

1. <span data-ttu-id="fd3b4-247">Válassza ki a forráskötet</span><span class="sxs-lookup"><span data-stu-id="fd3b4-247">Pick the source volume</span></span>
2. <span data-ttu-id="fd3b4-248">Válassza ki a visszaállítandó biztonsági mentési pontok</span><span class="sxs-lookup"><span data-stu-id="fd3b4-248">Choose a backup point to restore</span></span>
3. <span data-ttu-id="fd3b4-249">Válasszon ki egy elemet visszaállítása</span><span class="sxs-lookup"><span data-stu-id="fd3b4-249">Choose an item to restore</span></span>
4. <span data-ttu-id="fd3b4-250">A visszaállítási folyamat eseményindító</span><span class="sxs-lookup"><span data-stu-id="fd3b4-250">Trigger the restore process</span></span>

### <a name="picking-the-source-volume"></a><span data-ttu-id="fd3b4-251">A forráskötet válassza háttérszínnek.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-251">Picking the source volume</span></span>
<span data-ttu-id="fd3b4-252">Ahhoz, hogy az Azure Backup elem visszaállításához először kell azonosítani a cikk forrását.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-252">In order to restore an item from Azure Backup, you first need to identify the source of the item.</span></span> <span data-ttu-id="fd3b4-253">Azt éppen végrehajtás alatt a parancsokat egy Windows Server vagy egy Windows ügyfél környezetben, mert a gép már azonosítja.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-253">Since we're executing the commands in the context of a Windows Server or a Windows client, the machine is already identified.</span></span> <span data-ttu-id="fd3b4-254">A következő lépése a forrás azonosítása, hogy az azt tartalmazó kötet azonosításához.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-254">The next step in identifying the source is to identify the volume containing it.</span></span> <span data-ttu-id="fd3b4-255">Erről a gépről biztonsági mentés alatt lekérhetők végrehajtása adatforrásokat, illetve kötetek listáját a [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-255">A list of volumes or sources being backed up from this machine can be retrieved by executing the [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet.</span></span> <span data-ttu-id="fd3b4-256">Ez a parancs minden, a biztonsági másolatot készített a kiszolgáló ügyfél források tömbjét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-256">This command returns an array of all the sources backed up from this server/client.</span></span>

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-from-which-to-restore"></a><span data-ttu-id="fd3b4-257">A visszaállítandó biztonsági mentési pont kiválasztása</span><span class="sxs-lookup"><span data-stu-id="fd3b4-257">Choosing a backup point from which to restore</span></span>
<span data-ttu-id="fd3b4-258">Célvárólistából végrehajtásával biztonsági mentési pontok listáját a [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) parancsmag megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-258">You retreive a list of backup points by executing the [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet with appropriate parameters.</span></span> <span data-ttu-id="fd3b4-259">A jelen példában mutatjuk be a legújabb biztonsági mentési pont a forráskötet *D:* , és visszaállít egy adott fájlt.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-259">In our example, we’ll choose the latest backup point for the source volume *D:* and use it to recover a specific file.</span></span>

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
<span data-ttu-id="fd3b4-260">Az objektum ```$rps``` biztonsági mentési pontok tömbje.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-260">The object ```$rps``` is an array of backup points.</span></span> <span data-ttu-id="fd3b4-261">Az első elem a legutóbbi pontnak, az n-edik elemének pedig a legrégebbi pont.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-261">The first element is the latest point and the Nth element is the oldest point.</span></span> <span data-ttu-id="fd3b4-262">Válassza ki a legutóbbi pontnak, hogy használjuk ```$rps[0]```.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-262">To choose the latest point, we will use ```$rps[0]```.</span></span>

### <a name="choosing-an-item-to-restore"></a><span data-ttu-id="fd3b4-263">Visszaállítás elem kiválasztása</span><span class="sxs-lookup"><span data-stu-id="fd3b4-263">Choosing an item to restore</span></span>
<span data-ttu-id="fd3b4-264">A pontos fájl vagy mappa visszaállításához rekurzív módon használja azonosítására a [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-264">To identify the exact file or folder to restore, recursively use the [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet.</span></span> <span data-ttu-id="fd3b4-265">A mappahierarchia kizárólag használatával tallózható így a ```Get-OBRecoverableItem```.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-265">That way the folder hierarchy can be browsed solely using the ```Get-OBRecoverableItem```.</span></span>

<span data-ttu-id="fd3b4-266">Ebben a példában, ha azt szeretné, hogy állítsa vissza a fájlt *finances.xls* azt hivatkozhat, amely az objektum ```$filesFolders[1]```.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-266">In this example, if we want to restore the file *finances.xls* we can reference that using the object ```$filesFolders[1]```.</span></span>

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

<span data-ttu-id="fd3b4-267">Az elemek visszaállítás segítségével is kereshet a ```Get-OBRecoverableItem``` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-267">You can also search for items to restore using the ```Get-OBRecoverableItem``` cmdlet.</span></span> <span data-ttu-id="fd3b4-268">A jelen példában keressen rá a *finances.xls* azt sikerült leírót a fájl a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-268">In our example, to search for *finances.xls* we could get a handle on the file by running this command:</span></span>

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a><span data-ttu-id="fd3b4-269">A visszaállítási folyamat időt.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-269">Triggering the restore process</span></span>
<span data-ttu-id="fd3b4-270">A visszaállítási folyamat indításához először kell adja meg a helyreállítási beállításokat.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-270">To trigger the restore process, we first need to specify the recovery options.</span></span> <span data-ttu-id="fd3b4-271">Ezt megteheti a a [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-271">This can be done by using the [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet.</span></span> <span data-ttu-id="fd3b4-272">Ehhez a példához tegyük fel, hogy a fájlok visszaállítására szeretnénk *C:\temp*. Tegyük is fel, hogy szeretnénk már megtalálható fájlok kihagyása a rendeltetési mappára *C:\temp*. Hozzon létre egy helyreállítási lehetőség, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-272">For this example, let's assume that we want to restore the files to *C:\temp*. Let's also assume that we want to skip files that already exist on the destination folder *C:\temp*. To create such a recovery option, use the following command:</span></span>

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

<span data-ttu-id="fd3b4-273">Most elindítani a visszaállítási folyamat használatával a [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) parancs a kiválasztott ```$item``` kimenetében a ```Get-OBRecoverableItem``` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-273">Now trigger the restore process by using the [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) command on the selected ```$item``` from the output of the ```Get-OBRecoverableItem``` cmdlet:</span></span>

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## <a name="uninstalling-the-azure-backup-agent"></a><span data-ttu-id="fd3b4-274">Az Azure Backup szolgáltatás ügynökének eltávolítása</span><span class="sxs-lookup"><span data-stu-id="fd3b4-274">Uninstalling the Azure Backup agent</span></span>
<span data-ttu-id="fd3b4-275">Az Azure Backup-ügynök eltávolítása végezhető a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-275">Uninstalling the Azure Backup agent can be done by using the following command:</span></span>

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

<span data-ttu-id="fd3b4-276">Az ügynök bináris fájlok eltávolítása a számítógépről következménnyel néhány kell figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-276">Uninstalling the agent binaries from the machine has some consequences to consider:</span></span>

* <span data-ttu-id="fd3b4-277">A fájl-szűrő eltávolítja a gépet, és a változások követését le van állítva.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-277">It removes the file-filter from the machine, and tracking of changes is stopped.</span></span>
* <span data-ttu-id="fd3b4-278">A gép eltávolítja az összes házirend az adatokat, de a házirenddel kapcsolatos információk továbbra is a szolgáltatásban kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-278">All policy information is removed from the machine, but the policy information continues to be stored in the service.</span></span>
* <span data-ttu-id="fd3b4-279">A mentési ütemezések törlődnek, és nincs további biztonsági mentés készül.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-279">All backup schedules are removed, and no further backups are taken.</span></span>

<span data-ttu-id="fd3b4-280">Azonban az adatok Azure marad tárolja, és megőrzi a megőrzési házirend beállítása adott meg.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-280">However, the data stored in Azure remains and is retained as per the retention policy setup by you.</span></span> <span data-ttu-id="fd3b4-281">Régebbi pontok automatikusan van elavult.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-281">Older points are automatically aged out.</span></span>

## <a name="remote-management"></a><span data-ttu-id="fd3b4-282">Távfelügyelet</span><span class="sxs-lookup"><span data-stu-id="fd3b4-282">Remote management</span></span>
<span data-ttu-id="fd3b4-283">Az Azure Backup agent, a házirendek és a adatforrások minden felügyeleti távolról végezhető el a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-283">All the management around the Azure Backup agent, policies, and data sources can be done remotely through PowerShell.</span></span> <span data-ttu-id="fd3b4-284">A távolról felügyelt számítógép kell megfelelően elő kell készíteni.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-284">The machine that will be managed remotely needs to be prepared correctly.</span></span>

<span data-ttu-id="fd3b4-285">Alapértelmezés szerint a WinRM szolgáltatás kézi indításra van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-285">By default, the WinRM service is configured for manual startup.</span></span> <span data-ttu-id="fd3b4-286">Az indítási típust kell beállítani *automatikus* és kell indítani a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-286">The startup type must be set to *Automatic* and the service should be started.</span></span> <span data-ttu-id="fd3b4-287">Győződjön meg arról, hogy a WinRM szolgáltatás fut, az állapot tulajdonság értékének kell *futtató*.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-287">To verify that the WinRM service is running, the value of the Status property should be *Running*.</span></span>

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

<span data-ttu-id="fd3b4-288">PowerShell távoli eljáráshívás kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-288">PowerShell should be configured for remoting.</span></span>

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

<span data-ttu-id="fd3b4-289">A számítógép most távolról felügyelhetők - az ügynök telepítésének indítása.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-289">The machine can now be managed remotely - starting from the installation of the agent.</span></span> <span data-ttu-id="fd3b4-290">A következő parancsfájl például másolja át az ügynököt a távoli számítógépre, és telepíti azt.</span><span class="sxs-lookup"><span data-stu-id="fd3b4-290">For example, the following script copies the agent to the remote machine and installs it.</span></span>

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a><span data-ttu-id="fd3b4-291">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fd3b4-291">Next steps</span></span>
<span data-ttu-id="fd3b4-292">További információ az Azure biztonsági mentés a Windows Server vagy Windows-ügyfélen lásd:</span><span class="sxs-lookup"><span data-stu-id="fd3b4-292">For more information about Azure Backup for Windows Server/Client see</span></span>

* [<span data-ttu-id="fd3b4-293">Az Azure Backup bemutatása</span><span class="sxs-lookup"><span data-stu-id="fd3b4-293">Introduction to Azure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="fd3b4-294">Windows-kiszolgálók biztonsági mentését</span><span class="sxs-lookup"><span data-stu-id="fd3b4-294">Back up Windows Servers</span></span>](backup-configure-vault.md)
