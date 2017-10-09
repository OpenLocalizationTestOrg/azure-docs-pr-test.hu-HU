---
title: "aaaUse PowerShell toomanage Windows Server biztonsági mentése az Azure-ban |} Microsoft Docs"
description: "Telepítése és kezelése a PowerShell használatával Windows Server biztonsági mentés."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: e7e269e2-1f11-41a9-957b-a2155de6a1e0
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: saurse;markgal;nkolli;trinadhk
ms.openlocfilehash: 72292e510b0f059102440bd49a195be4ef700a6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-windows-serverwindows-client-using-powershell"></a><span data-ttu-id="d92db-103">Központi telepítése és kezelése a biztonsági mentési tooAzure Windows Server és Windows-ügyfél PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="d92db-103">Deploy and manage backup tooAzure for Windows Server/Windows Client using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d92db-104">ARM</span><span class="sxs-lookup"><span data-stu-id="d92db-104">ARM</span></span>](backup-client-automation.md)
> * [<span data-ttu-id="d92db-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="d92db-105">Classic</span></span>](backup-client-automation-classic.md)
>
>

<span data-ttu-id="d92db-106">Ez a cikk ismerteti, hogyan készíthet a Windows Server és a Windows munkaállomás adatok tooa toouse PowerShell tooback biztonsági mentési tároló.</span><span class="sxs-lookup"><span data-stu-id="d92db-106">This article explains how toouse PowerShell tooback up Windows Server or Windows workstation data tooa backup vault.</span></span> <span data-ttu-id="d92db-107">A Microsoft azt javasolja, hogy az összes új központi telepítéseknél Recovery Services-tárolók használatával.</span><span class="sxs-lookup"><span data-stu-id="d92db-107">Microsoft recommends using Recovery Services vaults for all new deployments.</span></span> <span data-ttu-id="d92db-108">Ha egy új Azure biztonságimásolat-felhasználó, és nem hozott létre egy mentési tárolót az előfizetéshez, használja a hello cikk [központi telepítése és kezelése a PowerShell használatával a Data Protection Manager-adatok tooAzure](backup-client-automation.md) , az adatok tárolása a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="d92db-108">If you are a new Azure Backup user and have not created a backup vault in your subscription, use hello article, [Deploy and manage Data Protection Manager data tooAzure using PowerShell](backup-client-automation.md) so you store your data in a Recovery Services vault.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="d92db-109">Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban.</span><span class="sxs-lookup"><span data-stu-id="d92db-109">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="d92db-110">További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="d92db-110">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="d92db-111">A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="d92db-111">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="d92db-112">2017. október 15. után PowerShell toocreate mentési tárolókban nem használható.</span><span class="sxs-lookup"><span data-stu-id="d92db-112">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="d92db-113">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="d92db-113">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="d92db-114">Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="d92db-114">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="d92db-115">Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="d92db-115">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="d92db-116">Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.</span><span class="sxs-lookup"><span data-stu-id="d92db-116">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="install-azure-powershell"></a><span data-ttu-id="d92db-117">Az Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="d92db-117">Install Azure PowerShell</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="d92db-118">2015 október, az Azure PowerShell 1.0-s verziójában jelent meg.</span><span class="sxs-lookup"><span data-stu-id="d92db-118">In October 2015, Azure PowerShell 1.0 was released.</span></span> <span data-ttu-id="d92db-119">Ebben a kiadásban hello 0.9.8-as kiadása sikeresen befejeződött, és néhány jelentős módosításokat, különösen a hello elnevezési hello parancsmagok állapotba.</span><span class="sxs-lookup"><span data-stu-id="d92db-119">This release succeeded hello 0.9.8 release and brought about some significant changes, especially in hello naming pattern of hello cmdlets.</span></span> <span data-ttu-id="d92db-120">1.0-ás parancsmagok kövesse hello elnevezési {ige}-AzureRm {főnév}; mivel a hello 0.9.8-as nevek viszont nem tartalmazzák **Rm** (például New-AzureRmResourceGroup New-használják helyett).</span><span class="sxs-lookup"><span data-stu-id="d92db-120">1.0 cmdlets follow hello naming pattern {verb}-AzureRm{noun}; whereas, hello 0.9.8 names do not include **Rm** (for example, New-AzureRmResourceGroup instead of New-AzureResourceGroup).</span></span> <span data-ttu-id="d92db-121">Ha az Azure PowerShell 0.9.8-as, először engedélyeznie kell hello Resource Manager módra hello futtatásával **Switch-AzureMode AzureResourceManager** parancsot.</span><span class="sxs-lookup"><span data-stu-id="d92db-121">When using Azure PowerShell 0.9.8, you must first enable hello Resource Manager mode by running hello **Switch-AzureMode AzureResourceManager** command.</span></span> <span data-ttu-id="d92db-122">Ez a parancs nincs szükség az 1.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d92db-122">This command is not necessary in 1.0 or later.</span></span>

<span data-ttu-id="d92db-123">Ha azt szeretné, toouse hello 0.9.8-as környezet hello 1.0-s vagy újabb környezetben, a parancsfájlokat körültekintően tesztelje hello parancsfájlok éles üzem előtti környezetben tooavoid éles használat előtt váratlan hatása.</span><span class="sxs-lookup"><span data-stu-id="d92db-123">If you want toouse your scripts written for hello 0.9.8 environment, in hello 1.0 or later environment, you should carefully test hello scripts in a pre-production environment before using them in production tooavoid unexpected impact.</span></span>

<span data-ttu-id="d92db-124">[Töltse le a legfrissebb verzióját hello a PowerShell](https://github.com/Azure/azure-powershell/releases) (szükséges minimális verziója: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="d92db-124">[Download hello latest PowerShell release](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-backup-vault"></a><span data-ttu-id="d92db-125">Backup-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="d92db-125">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="d92db-126">A felhasználó Azure Backup segítségével hello az első alkalommal esetén tooregister hello Azure biztonsági mentés szolgáltató toobe használja az előfizetéskor kell.</span><span class="sxs-lookup"><span data-stu-id="d92db-126">For customers using Azure Backup for hello first time, you need tooregister hello Azure Backup provider toobe used with your subscription.</span></span> <span data-ttu-id="d92db-127">Ezt megteheti hello a következő parancs futtatásával: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"</span><span class="sxs-lookup"><span data-stu-id="d92db-127">This can be done by running hello following command: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="d92db-128">Létrehozhat egy új mentési tárolót használó hello **New-AzureRMBackupVault** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d92db-128">You can create a new backup vault using hello **New-AzureRMBackupVault** cmdlet.</span></span> <span data-ttu-id="d92db-129">hello mentési tároló egy ARM-erőforrás, ezért meg kell tooplace az erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="d92db-129">hello backup vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="d92db-130">Egy rendszergazda jogú Azure PowerShell-konzolban futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="d92db-130">In an elevated Azure PowerShell console, run hello following commands:</span></span>

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

<span data-ttu-id="d92db-131">Használjon hello **Get-AzureRMBackupVault** parancsmag toolist hello mentési tárolókat egy előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="d92db-131">Use hello **Get-AzureRMBackupVault** cmdlet toolist hello backup vaults in a subscription.</span></span>

## <a name="installing-hello-azure-backup-agent"></a><span data-ttu-id="d92db-132">Hello Azure Backup szolgáltatás ügynökének telepítése</span><span class="sxs-lookup"><span data-stu-id="d92db-132">Installing hello Azure Backup agent</span></span>
<span data-ttu-id="d92db-133">Hello Azure Backup szolgáltatás ügynökének telepítése előtt kell toohave hello telepítő letöltött és a Windows Server hello megtalálható.</span><span class="sxs-lookup"><span data-stu-id="d92db-133">Before you install hello Azure Backup agent, you need toohave hello installer downloaded and present on hello Windows Server.</span></span> <span data-ttu-id="d92db-134">Hello installer legújabb verzióját hello letölthető hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) vagy hello mentési tároló irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="d92db-134">You can get hello latest version of hello installer from hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from hello backup vault's Dashboard page.</span></span> <span data-ttu-id="d92db-135">Mentés hello telepítő tooan könnyen hozzáférhető helyen, például * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="d92db-135">Save hello installer tooan easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="d92db-136">tooinstall hello ügynök, futtassa a következő parancsot egy rendszergazda jogú PowerShell-konzolban hello:</span><span class="sxs-lookup"><span data-stu-id="d92db-136">tooinstall hello agent, run hello following command in an elevated PowerShell console:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="d92db-137">Ezzel telepít hello ügynök minden hello alapértelmezett beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="d92db-137">This installs hello agent with all hello default options.</span></span> <span data-ttu-id="d92db-138">hello telepítési hello háttérben néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="d92db-138">hello installation takes a few minutes in hello background.</span></span> <span data-ttu-id="d92db-139">Ha nem adja meg a hello */nu* lehetőséget, majd hello **Windows Update** ablak nyílik meg hello telepítési toocheck bármely frissítések hello végén.</span><span class="sxs-lookup"><span data-stu-id="d92db-139">If you do not specify hello */nu* option then hello **Windows Update** window will open at hello end of hello installation toocheck for any updates.</span></span> <span data-ttu-id="d92db-140">A telepítést követően hello ügynök hello telepített programok listájában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="d92db-140">Once installed, hello agent will show in hello list of installed programs.</span></span>

<span data-ttu-id="d92db-141">a telepített programok toosee hello listája, nyissa meg túl**Vezérlőpult** > **programok** > **programok és szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="d92db-141">toosee hello list of installed programs, go too**Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Az ügynök telepítve](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="d92db-143">Telepítési beállítások</span><span class="sxs-lookup"><span data-stu-id="d92db-143">Installation options</span></span>
<span data-ttu-id="d92db-144">az összes elérhető beállítások hello toosee hello parancssori, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="d92db-144">toosee all hello options available via hello command-line, use hello following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="d92db-145">hello elérhető lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="d92db-145">hello available options include:</span></span>

| <span data-ttu-id="d92db-146">Beállítás</span><span class="sxs-lookup"><span data-stu-id="d92db-146">Option</span></span> | <span data-ttu-id="d92db-147">Részletek</span><span class="sxs-lookup"><span data-stu-id="d92db-147">Details</span></span> | <span data-ttu-id="d92db-148">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="d92db-148">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d92db-149">/q</span><span class="sxs-lookup"><span data-stu-id="d92db-149">/q</span></span> |<span data-ttu-id="d92db-150">Csendes telepítés</span><span class="sxs-lookup"><span data-stu-id="d92db-150">Quiet installation</span></span> |- |
| <span data-ttu-id="d92db-151">/ p: "hely"</span><span class="sxs-lookup"><span data-stu-id="d92db-151">/p:"location"</span></span> |<span data-ttu-id="d92db-152">Elérési út toohello telepítési mappáját hello Azure Backup szolgáltatás ügynöke.</span><span class="sxs-lookup"><span data-stu-id="d92db-152">Path toohello installation folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="d92db-153">C:\Program Files\Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="d92db-153">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="d92db-154">/ s: "hely"</span><span class="sxs-lookup"><span data-stu-id="d92db-154">/s:"location"</span></span> |<span data-ttu-id="d92db-155">Elérési út toohello gyorsítótármappája hello Azure Backup szolgáltatás ügynöke.</span><span class="sxs-lookup"><span data-stu-id="d92db-155">Path toohello cache folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="d92db-156">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="d92db-156">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="d92db-157">/m</span><span class="sxs-lookup"><span data-stu-id="d92db-157">/m</span></span> |<span data-ttu-id="d92db-158">Részt tooMicrosoft frissítés</span><span class="sxs-lookup"><span data-stu-id="d92db-158">Opt-in tooMicrosoft Update</span></span> |- |
| <span data-ttu-id="d92db-159">/Nu</span><span class="sxs-lookup"><span data-stu-id="d92db-159">/nu</span></span> |<span data-ttu-id="d92db-160">Ne keressen frissítéseket telepítésének befejezése után</span><span class="sxs-lookup"><span data-stu-id="d92db-160">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="d92db-161">/d</span><span class="sxs-lookup"><span data-stu-id="d92db-161">/d</span></span> |<span data-ttu-id="d92db-162">Eltávolítja a Microsoft Azure Recovery Services Agent ügynök</span><span class="sxs-lookup"><span data-stu-id="d92db-162">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="d92db-163">/pH</span><span class="sxs-lookup"><span data-stu-id="d92db-163">/ph</span></span> |<span data-ttu-id="d92db-164">Állomás proxycím</span><span class="sxs-lookup"><span data-stu-id="d92db-164">Proxy Host Address</span></span> |- |
| <span data-ttu-id="d92db-165">/po</span><span class="sxs-lookup"><span data-stu-id="d92db-165">/po</span></span> |<span data-ttu-id="d92db-166">Proxy Host Port száma</span><span class="sxs-lookup"><span data-stu-id="d92db-166">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="d92db-167">/Pu</span><span class="sxs-lookup"><span data-stu-id="d92db-167">/pu</span></span> |<span data-ttu-id="d92db-168">Proxy-állomás felhasználónév</span><span class="sxs-lookup"><span data-stu-id="d92db-168">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="d92db-169">/pW</span><span class="sxs-lookup"><span data-stu-id="d92db-169">/pw</span></span> |<span data-ttu-id="d92db-170">Proxy jelszava</span><span class="sxs-lookup"><span data-stu-id="d92db-170">Proxy Password</span></span> |- |

## <a name="registering-with-hello-azure-backup-service"></a><span data-ttu-id="d92db-171">Hello Azure Backup szolgáltatás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="d92db-171">Registering with hello Azure Backup service</span></span>
<span data-ttu-id="d92db-172">Az Azure Backup szolgáltatás hello regisztrálhatja, jóvá kell tooensure adott hello [Előfeltételek](backup-configure-vault.md) teljesülnek.</span><span class="sxs-lookup"><span data-stu-id="d92db-172">Before you can register with hello Azure Backup service, you need tooensure that hello [prerequisites](backup-configure-vault.md) are met.</span></span> <span data-ttu-id="d92db-173">A következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="d92db-173">You must:</span></span>

* <span data-ttu-id="d92db-174">Egy érvényes Azure-előfizetése</span><span class="sxs-lookup"><span data-stu-id="d92db-174">Have a valid Azure subscription</span></span>
* <span data-ttu-id="d92db-175">Rendelkezik egy biztonsági mentési tárolóval</span><span class="sxs-lookup"><span data-stu-id="d92db-175">Have a backup vault</span></span>

<span data-ttu-id="d92db-176">toodownload hello tárolói hitelesítő adatokat, futtassa a hello **Get-AzureRMBackupVaultCredentials** parancsmag egy Azure PowerShell-konzolban és a tároló azt egy tetszőleges helyen, például a * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="d92db-176">toodownload hello vault credentials, run hello **Get-AzureRMBackupVaultCredentials** cmdlet in an Azure PowerShell console and store it in a convenient location like *C:\Downloads\*.</span></span>

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

<span data-ttu-id="d92db-177">Regisztrálásakor hello gép hello tárolóban történik hello segítségével [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="d92db-177">Registering hello machine with hello vault is done using hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration -VaultCredentials $cred -Confirm:$false

CertThumbprint      : 7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName : test-vault
Region              : West US

Machine registration succeeded.
```

> [!IMPORTANT]
> <span data-ttu-id="d92db-178">Ne használjon relatív elérési utak toospecify hello tároló hitelesítési adatait tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="d92db-178">Do not use relative paths toospecify hello vault credentials file.</span></span> <span data-ttu-id="d92db-179">Egy bemeneti toohello parancsmagot, abszolút elérési utat kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="d92db-179">You must provide an absolute path as an input toohello cmdlet.</span></span>
>
>

## <a name="networking-settings"></a><span data-ttu-id="d92db-180">Hálózati beállítások</span><span class="sxs-lookup"><span data-stu-id="d92db-180">Networking settings</span></span>
<span data-ttu-id="d92db-181">Hello hello csatlakoztatásához Windows számítógép-toohello internet proxykiszolgálón keresztül történik, amikor hello proxybeállítások is megadható toohello ügynök.</span><span class="sxs-lookup"><span data-stu-id="d92db-181">When hello connectivity of hello Windows machine toohello internet is through a proxy server, hello proxy settings can also be provided toohello agent.</span></span> <span data-ttu-id="d92db-182">Ebben a példában nincs nincs proxykiszolgáló, explicit módon azt vannak törlésével bármely proxy kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="d92db-182">In this example, there is no proxy server, so we are explicitly clearing any proxy-related information.</span></span>

<span data-ttu-id="d92db-183">A sávszélesség-használat is szabályozhatja az hello beállításokkal ```work hour bandwidth``` és ```non-work hour bandwidth``` egy adott objektumcsoporthoz hello hét nap.</span><span class="sxs-lookup"><span data-stu-id="d92db-183">Bandwidth usage can also be controlled with hello options of ```work hour bandwidth``` and ```non-work hour bandwidth``` for a given set of days of hello week.</span></span>

<span data-ttu-id="d92db-184">Beállítás hello proxy- és sávszélesség részletei történik hello segítségével [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="d92db-184">Setting hello proxy and bandwidth details is done using hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a><span data-ttu-id="d92db-185">Titkosítási beállítások</span><span class="sxs-lookup"><span data-stu-id="d92db-185">Encryption settings</span></span>
<span data-ttu-id="d92db-186">hello küldött biztonsági mentési adatok tooAzure biztonsági mentés titkosított tooprotect hello adatok bizalmas mivoltát hello.</span><span class="sxs-lookup"><span data-stu-id="d92db-186">hello backup data sent tooAzure Backup is encrypted tooprotect hello confidentiality of hello data.</span></span> <span data-ttu-id="d92db-187">hello titkosítási jelszó visszaállítása a hello időpontjában az hello "password" toodecrypt hello adatai.</span><span class="sxs-lookup"><span data-stu-id="d92db-187">hello encryption passphrase is hello "password" toodecrypt hello data at hello time of restore.</span></span>

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
Server properties updated successfully
```

> [!IMPORTANT]
> <span data-ttu-id="d92db-188">Biztosíthatja hello jelszót adatok biztonságos be van állítva.</span><span class="sxs-lookup"><span data-stu-id="d92db-188">Keep hello passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="d92db-189">Csak akkor tudja toorestore adatokat az Azure-ból nélkül ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="d92db-189">You will not be able toorestore data from Azure without this passphrase.</span></span>
>
>

## <a name="back-up-files-and-folders"></a><span data-ttu-id="d92db-190">Fájlok és mappák biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="d92db-190">Back up files and folders</span></span>
<span data-ttu-id="d92db-191">Minden a biztonsági mentéseket Windows-kiszolgálók és ügyfelek tooAzure biztonsági mentési házirend vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="d92db-191">All your backups from Windows Servers and clients tooAzure Backup are governed by a policy.</span></span> <span data-ttu-id="d92db-192">hello házirend három részből áll:</span><span class="sxs-lookup"><span data-stu-id="d92db-192">hello policy comprises three parts:</span></span>

1. <span data-ttu-id="d92db-193">A **biztonsági mentés ütemezése** , amely megadja, amikor biztonsági mentést kell toobe venni, és szinkronizálva hello szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="d92db-193">A **backup schedule** that specifies when backups need toobe taken and synchronized with hello service.</span></span>
2. <span data-ttu-id="d92db-194">A **adatmegőrzési ütemterv** , amely meghatározza, mennyi ideig tooretain hello helyreállítási pontok az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d92db-194">A **retention schedule** that specifies how long tooretain hello recovery points in Azure.</span></span>
3. <span data-ttu-id="d92db-195">A **belefoglalási/kizárási megadására** , amely határozzák meg, hogy mi készüljön biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="d92db-195">A **file inclusion/exclusion specification** that dictates what should be backed up.</span></span>

<span data-ttu-id="d92db-196">Ebben a dokumentumban mivel azt még automatizálása a biztonsági mentés, feltételezzük, semmi nem konfiguráltak.</span><span class="sxs-lookup"><span data-stu-id="d92db-196">In this document, since we're automating backup, we'll assume nothing has been configured.</span></span> <span data-ttu-id="d92db-197">Hozzon létre egy új biztonsági mentési házirendet hello megkezdjük [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) parancsmag és használja azt.</span><span class="sxs-lookup"><span data-stu-id="d92db-197">We begin by creating a new backup policy using hello [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet and using it.</span></span>

```
PS C:\> $newpolicy = New-OBPolicy
```

<span data-ttu-id="d92db-198">: Az idő hello házirend üres, és más parancsmagok olyan szükséges toodefine milyen elemek fog kell, illetve tiltani szeretné, ha biztonsági mentések futtatja, és ahol hello biztonsági mentések tárolódik.</span><span class="sxs-lookup"><span data-stu-id="d92db-198">At this time hello policy is empty and other cmdlets are needed toodefine what items will be included or excluded, when backups will run, and where hello backups will be stored.</span></span>

### <a name="configuring-hello-backup-schedule"></a><span data-ttu-id="d92db-199">Biztonsági mentés ütemezése hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d92db-199">Configuring hello backup schedule</span></span>
<span data-ttu-id="d92db-200">először hello hello házirend 3 részből az hello biztonsági mentési ütemezést, amely hello létre [New-OBSchedule](https://technet.microsoft.com/library/hh770401) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d92db-200">hello first of hello 3 parts of a policy is hello backup schedule, which is created using hello [New-OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet.</span></span> <span data-ttu-id="d92db-201">hello biztonsági mentés ütemezése határozza meg, amikor biztonsági mentést kell venni toobe.</span><span class="sxs-lookup"><span data-stu-id="d92db-201">hello backup schedule defines when backups need toobe taken.</span></span> <span data-ttu-id="d92db-202">Ütemezés létrehozásakor kell toospecify 2 bemeneti paraméterek:</span><span class="sxs-lookup"><span data-stu-id="d92db-202">When creating a schedule you need toospecify 2 input parameters:</span></span>

* <span data-ttu-id="d92db-203">**Hello hét napjai** biztonsági mentését végző hello kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="d92db-203">**Days of hello week** that hello backup should run.</span></span> <span data-ttu-id="d92db-204">Hello biztonságimásolat-készítő feladat csak egy napon vagy hello hét minden napján, vagy tetszőleges kombinációját a közötti futtatása.</span><span class="sxs-lookup"><span data-stu-id="d92db-204">You can run hello backup job on just one day, or every day of hello week, or any combination in between.</span></span>
* <span data-ttu-id="d92db-205">**Hello napszakokban** mikor fusson a hello biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="d92db-205">**Times of hello day** when hello backup should run.</span></span> <span data-ttu-id="d92db-206">Másolatot too3 különböző napszakokban hello amikor hello biztonsági mentés akkor is kiváltódik adhat meg.</span><span class="sxs-lookup"><span data-stu-id="d92db-206">You can define up too3 different times of hello day when hello backup will be triggered.</span></span>

<span data-ttu-id="d92db-207">Például konfigurálhatja a biztonsági mentési házirend du. 4: futtat minden szombat és vasárnap.</span><span class="sxs-lookup"><span data-stu-id="d92db-207">For instance, you could configure a backup policy that runs at 4PM every Saturday and Sunday.</span></span>

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

<span data-ttu-id="d92db-208">hello biztonsági mentés ütemezése a házirendhez társított toobe van szüksége, és ez elérhető hello segítségével [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d92db-208">hello backup schedule needs toobe associated with a policy, and this can be achieved by using hello [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.</span></span>

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a><span data-ttu-id="d92db-209">Egy megőrzési házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d92db-209">Configuring a retention policy</span></span>
<span data-ttu-id="d92db-210">hello megőrzési házirend határozza meg, hogy mennyi ideig helyreállítási pont biztonsági mentési feladatok alapján létrehozott megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="d92db-210">hello retention policy defines how long recovery points created from backup jobs are retained.</span></span> <span data-ttu-id="d92db-211">Hello segítségével új megőrzési házirend létrehozásakor [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) parancsmaggal, megadhatja hello, hogy hány napig biztonsági mentések helyreállítási pontjait hello kell toobe megőrzi az Azure Backup szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="d92db-211">When creating a new retention policy using hello [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, you can specify hello number of days that hello backup recovery points need toobe retained with Azure Backup.</span></span> <span data-ttu-id="d92db-212">az alábbi példa hello 7 napos adatmegőrzési állítja be.</span><span class="sxs-lookup"><span data-stu-id="d92db-212">hello example below sets a retention policy of 7 days.</span></span>

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

<span data-ttu-id="d92db-213">hello adatmegőrzési hozzá kell rendelni hello parancsmaggal hello fő irányelvnek [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span><span class="sxs-lookup"><span data-stu-id="d92db-213">hello retention policy must be associated with hello main policy using hello cmdlet [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span></span>

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
### <a name="including-and-excluding-files-toobe-backed-up"></a><span data-ttu-id="d92db-214">Többek között, és a biztonsági másolatba mentett fájlok toobe kivételével</span><span class="sxs-lookup"><span data-stu-id="d92db-214">Including and excluding files toobe backed up</span></span>
<span data-ttu-id="d92db-215">Egy ```OBFileSpec``` objektum hello fájlok toobe és a biztonsági másolat nem határoz meg.</span><span class="sxs-lookup"><span data-stu-id="d92db-215">An ```OBFileSpec``` object defines hello files toobe included and excluded in a backup.</span></span> <span data-ttu-id="d92db-216">Ez olyan szabályok, hogy a kimenő hello hatókör védett fájlok és mappák a gépen.</span><span class="sxs-lookup"><span data-stu-id="d92db-216">This is a set of rules that scope out hello protected files and folders on a machine.</span></span> <span data-ttu-id="d92db-217">Akkor is, számos befoglalási vagy kizárási szabály szükség szerint fájlt, és rendelje hozzá őket egy házirendet.</span><span class="sxs-lookup"><span data-stu-id="d92db-217">You can have as many file inclusion or exclusion rules as required, and associate them with a policy.</span></span> <span data-ttu-id="d92db-218">Új OBFileSpec objektum létrehozásakor meg a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="d92db-218">When creating a new OBFileSpec object, you can:</span></span>

* <span data-ttu-id="d92db-219">Adja meg a fájlok és mappák szereplő toobe hello</span><span class="sxs-lookup"><span data-stu-id="d92db-219">Specify hello files and folders toobe included</span></span>
* <span data-ttu-id="d92db-220">Adja meg a hello kizárt fájlok és mappák toobe</span><span class="sxs-lookup"><span data-stu-id="d92db-220">Specify hello files and folders toobe excluded</span></span>
* <span data-ttu-id="d92db-221">Adja meg a rekurzív az adatok biztonsági mentése egy mappában (vagy) e csak hello legfelső szintű mappában lévő fájlok hello megadott biztonsági másolatot kell fel.</span><span class="sxs-lookup"><span data-stu-id="d92db-221">Specify recursive backup of data in a folder (or) whether only hello top-level files in hello specified folder should be backed up.</span></span>

<span data-ttu-id="d92db-222">Ez utóbbi hello hello - nem rekurzív jelzőt a New-OBFileSpec hello parancs használatával érhető el.</span><span class="sxs-lookup"><span data-stu-id="d92db-222">hello latter is achieved by using hello -NonRecursive flag in hello New-OBFileSpec command.</span></span>

<span data-ttu-id="d92db-223">Hello az alábbi példában azt bemutatjuk C: és d. kötet mentésére, hello OS bináris hello Windows-mappában, és ideiglenes mappák kizárása.</span><span class="sxs-lookup"><span data-stu-id="d92db-223">In hello example below, we'll back up volume C: and D: and exclude hello OS binaries in hello Windows folder and any temporary folders.</span></span> <span data-ttu-id="d92db-224">toodo, hozzon létre két fájl specifikációk hello segítségével [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) parancsmag - befoglalási és kizárási egyet.</span><span class="sxs-lookup"><span data-stu-id="d92db-224">toodo so we'll create two file specifications using hello [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - one for inclusion and one for exclusion.</span></span> <span data-ttu-id="d92db-225">Létrehozása után a hello fájl specifikációk, fontosságúak társított a hello szabályzatokkal hello [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d92db-225">Once hello file specifications have been created, they're associated with hello policy using hello [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.</span></span>

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

### <a name="applying-hello-policy"></a><span data-ttu-id="d92db-226">Hello házirend alkalmazása</span><span class="sxs-lookup"><span data-stu-id="d92db-226">Applying hello policy</span></span>
<span data-ttu-id="d92db-227">Hello csoportházirend-objektum most már befejeződött, és egy társított biztonsági mentés ütemezése, a megőrzési házirend és a fájlok belefoglalási/kizárási lista.</span><span class="sxs-lookup"><span data-stu-id="d92db-227">Now hello policy object is complete and has an associated backup schedule, retention policy, and an inclusion/exclusion list of files.</span></span> <span data-ttu-id="d92db-228">Ez a házirend most hajtható végre, az Azure Backup toouse is.</span><span class="sxs-lookup"><span data-stu-id="d92db-228">This policy can now be committed for Azure Backup toouse.</span></span> <span data-ttu-id="d92db-229">Az újonnan létrehozott hello alkalmazása előtt házirend gondoskodjon arról, hogy nincsenek-e hello segítségével hello kiszolgálóhoz tartozó már meglévő biztonsági mentési házirendek [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d92db-229">Before you apply hello newly created policy ensure that there are no existing backup policies associated with hello server by using hello [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet.</span></span> <span data-ttu-id="d92db-230">Jóváhagyás eltávolítása hello házirend fogja kérni.</span><span class="sxs-lookup"><span data-stu-id="d92db-230">Removing hello policy will prompt for confirmation.</span></span> <span data-ttu-id="d92db-231">tooskip hello megerősítő hello használata ```-Confirm:$false``` jelző hello parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="d92db-231">tooskip hello confirmation use hello ```-Confirm:$false``` flag with hello cmdlet.</span></span>

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want tooremove this backup policy? This will delete all hello backed up data. [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
```

<span data-ttu-id="d92db-232">Végrehajtása hello csoportházirend-objektum történik hello segítségével [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d92db-232">Committing hello policy object is done using hello [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet.</span></span> <span data-ttu-id="d92db-233">Ez is megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="d92db-233">This will also ask for confirmation.</span></span> <span data-ttu-id="d92db-234">tooskip hello megerősítő hello használata ```-Confirm:$false``` jelző hello parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="d92db-234">tooskip hello confirmation use hello ```-Confirm:$false``` flag with hello cmdlet.</span></span>

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want toosave this backup policy ? [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
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

<span data-ttu-id="d92db-235">Megtekintheti a meglévő biztonsági mentési házirend hello hello segítségével hello adatait [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d92db-235">You can view hello details of hello existing backup policy using hello [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet.</span></span> <span data-ttu-id="d92db-236">Akkor is leásási hello használatával [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) hello biztonsági mentési ütemezést és hello parancsmag [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) hello adatmegőrzési parancsmaggal</span><span class="sxs-lookup"><span data-stu-id="d92db-236">You can drill-down further using hello [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet for hello backup schedule and hello [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet for hello retention policies</span></span>

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

### <a name="performing-an-ad-hoc-backup"></a><span data-ttu-id="d92db-237">Az ad hoc biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="d92db-237">Performing an ad-hoc backup</span></span>
<span data-ttu-id="d92db-238">Miután a biztonsági mentési házirend van beállítva hello biztonsági mentések hello ütemezés / fog létrejönni.</span><span class="sxs-lookup"><span data-stu-id="d92db-238">Once a backup policy has been set hello backups will occur per hello schedule.</span></span> <span data-ttu-id="d92db-239">Az ad hoc biztonsági mentés indítására esetében is lehetséges hello segítségével [Start-OBBackup](https://technet.microsoft.com/library/hh770426) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="d92db-239">Triggering an ad-hoc backup is also possible using hello [Start-OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:</span></span>

```
PS C:> Get-OBPolicy | Start-OBBackup
Taking snapshot of volumes...
Preparing storage...
Estimating size of backup items...
Estimating size of backup items...
Transferring data...
Verifying backup...
Job completed.
hello backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a><span data-ttu-id="d92db-240">Állítsa vissza az adatokat az Azure Backup</span><span class="sxs-lookup"><span data-stu-id="d92db-240">Restore data from Azure Backup</span></span>
<span data-ttu-id="d92db-241">Ez a szakasz végigvezeti Önt hello lépéseket az Azure biztonsági mentés az adatok helyreállítás automatizálása.</span><span class="sxs-lookup"><span data-stu-id="d92db-241">This section will guide you through hello steps for automating recovery of data from Azure Backup.</span></span> <span data-ttu-id="d92db-242">Ezzel úgy magában foglalja a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d92db-242">Doing so involves hello following steps:</span></span>

1. <span data-ttu-id="d92db-243">Hello forráskötet kiválasztása</span><span class="sxs-lookup"><span data-stu-id="d92db-243">Pick hello source volume</span></span>
2. <span data-ttu-id="d92db-244">Válassza ki a biztonsági mentési pont toorestore</span><span class="sxs-lookup"><span data-stu-id="d92db-244">Choose a backup point toorestore</span></span>
3. <span data-ttu-id="d92db-245">Egy elem toorestore kiválasztása</span><span class="sxs-lookup"><span data-stu-id="d92db-245">Choose an item toorestore</span></span>
4. <span data-ttu-id="d92db-246">Eseményindító hello visszaállítási folyamat</span><span class="sxs-lookup"><span data-stu-id="d92db-246">Trigger hello restore process</span></span>

### <a name="picking-hello-source-volume"></a><span data-ttu-id="d92db-247">Kiadási hello forráskötet</span><span class="sxs-lookup"><span data-stu-id="d92db-247">Picking hello source volume</span></span>
<span data-ttu-id="d92db-248">A sorrend toorestore egy elemet az Azure Backup először hello elem tooidentify hello forrását.</span><span class="sxs-lookup"><span data-stu-id="d92db-248">In order toorestore an item from Azure Backup, you first need tooidentify hello source of hello item.</span></span> <span data-ttu-id="d92db-249">Mivel azt még hello parancsok végrehajtása a Windows Server vagy egy Windows ügyfél hello környezetében, hello gép már azonosítja.</span><span class="sxs-lookup"><span data-stu-id="d92db-249">Since we're executing hello commands in hello context of a Windows Server or a Windows client, hello machine is already identified.</span></span> <span data-ttu-id="d92db-250">hello következő lépése hello forrás azonosítása tooidentify hello kötetet tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="d92db-250">hello next step in identifying hello source is tooidentify hello volume containing it.</span></span> <span data-ttu-id="d92db-251">A kötetek adatforrásokat, illetve biztonsági másolatot készít a számítógépről kérhető hello végrehajtásával listáját [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d92db-251">A list of volumes or sources being backed up from this machine can be retrieved by executing hello [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet.</span></span> <span data-ttu-id="d92db-252">Ez a parancs minden hello adatforrások biztonsági mentése a kiszolgáló ügyfél tömbjét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d92db-252">This command returns an array of all hello sources backed up from this server/client.</span></span>

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

### <a name="choosing-a-backup-point-toorestore"></a><span data-ttu-id="d92db-253">A biztonsági mentési pont toorestore kiválasztása</span><span class="sxs-lookup"><span data-stu-id="d92db-253">Choosing a backup point toorestore</span></span>
<span data-ttu-id="d92db-254">hello biztonsági mentési pontok listájának lekérhetők hello végrehajtása [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) parancsmag megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="d92db-254">hello list of backup points can be retrieved by executing hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet with appropriate parameters.</span></span> <span data-ttu-id="d92db-255">A jelen példában mutatjuk be a legújabb biztonsági mentési pontok hello hello forráskötet *D:* és toorecover egy adott fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="d92db-255">In our example, we’ll choose hello latest backup point for hello source volume *D:* and use it toorecover a specific file.</span></span>

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
<span data-ttu-id="d92db-256">hello objektum ```$rps``` biztonsági mentési pontok tömbje.</span><span class="sxs-lookup"><span data-stu-id="d92db-256">hello object ```$rps``` is an array of backup points.</span></span> <span data-ttu-id="d92db-257">hello első eleme hello legutóbbi pontnak és hello n-edik elem hello legrégebbi pont.</span><span class="sxs-lookup"><span data-stu-id="d92db-257">hello first element is hello latest point and hello Nth element is hello oldest point.</span></span> <span data-ttu-id="d92db-258">toochoose hello legújabb pont használjuk ```$rps[0]```.</span><span class="sxs-lookup"><span data-stu-id="d92db-258">toochoose hello latest point, we will use ```$rps[0]```.</span></span>

### <a name="choosing-an-item-toorestore"></a><span data-ttu-id="d92db-259">Egy elem toorestore kiválasztása</span><span class="sxs-lookup"><span data-stu-id="d92db-259">Choosing an item toorestore</span></span>
<span data-ttu-id="d92db-260">tooidentify hello pontosan a fájl vagy mappa toorestore, rekurzív módon használja a hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d92db-260">tooidentify hello exact file or folder toorestore, recursively use hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet.</span></span> <span data-ttu-id="d92db-261">Adott módon hello mappahierarchia kizárólag a hello segítségével tallózható ```Get-OBRecoverableItem```.</span><span class="sxs-lookup"><span data-stu-id="d92db-261">That way hello folder hierarchy can be browsed solely using hello ```Get-OBRecoverableItem```.</span></span>

<span data-ttu-id="d92db-262">Ebben a példában, ha azt szeretné, hogy toorestore hello fájl *finances.xls* azt is hivatkozni lehessen, hogy használatával hello objektum ```$filesFolders[1]```.</span><span class="sxs-lookup"><span data-stu-id="d92db-262">In this example, if we want toorestore hello file *finances.xls* we can reference that using hello object ```$filesFolders[1]```.</span></span>

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

<span data-ttu-id="d92db-263">Elemek toorestore hello használatával is kereshet ```Get-OBRecoverableItem``` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d92db-263">You can also search for items toorestore using hello ```Get-OBRecoverableItem``` cmdlet.</span></span> <span data-ttu-id="d92db-264">A példánkban a toosearch *finances.xls* azt lehetett beolvasni a leíró hello fájl a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="d92db-264">In our example, toosearch for *finances.xls* we could get a handle on hello file by running this command:</span></span>

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-hello-restore-process"></a><span data-ttu-id="d92db-265">Eseményindító hello visszaállítási folyamat</span><span class="sxs-lookup"><span data-stu-id="d92db-265">Triggering hello restore process</span></span>
<span data-ttu-id="d92db-266">tootrigger hello visszaállítási folyamat, először kell toospecify hello helyreállítási beállítások.</span><span class="sxs-lookup"><span data-stu-id="d92db-266">tootrigger hello restore process, we first need toospecify hello recovery options.</span></span> <span data-ttu-id="d92db-267">Ezt megteheti a hello [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d92db-267">This can be done by using hello [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet.</span></span> <span data-ttu-id="d92db-268">Ebben a példában tételezzük fel, hogy toorestore hello fájlok túl szeretnénk*C:\temp*. Tegyük is fel, hogy szeretnénk tooskip már megtalálható fájlok hello rendeltetési mappára *C:\temp*. toocreate ilyen egy helyreállítási lehetőség, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="d92db-268">For this example, let's assume that we want toorestore hello files too*C:\temp*. Let's also assume that we want tooskip files that already exist on hello destination folder *C:\temp*. toocreate such a recovery option, use hello following command:</span></span>

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

<span data-ttu-id="d92db-269">Most hello segítségével a visszaállítás elindítása [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) kijelölt hello parancs ```$item``` hello hello kimenetét a ```Get-OBRecoverableItem``` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="d92db-269">Now trigger restore by using hello [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) command on hello selected ```$item``` from hello output of hello ```Get-OBRecoverableItem``` cmdlet:</span></span>

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
hello recovery operation completed successfully.
```


## <a name="uninstalling-hello-azure-backup-agent"></a><span data-ttu-id="d92db-270">Hello Azure Backup szolgáltatás ügynökének eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d92db-270">Uninstalling hello Azure Backup agent</span></span>
<span data-ttu-id="d92db-271">Eltávolítását hello Azure Backup szolgáltatás ügynökének hello a következő parancs használatával teheti meg:</span><span class="sxs-lookup"><span data-stu-id="d92db-271">Uninstalling hello Azure Backup agent can be done by using hello following command:</span></span>

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

<span data-ttu-id="d92db-272">Néhány következmények tooconsider hello ügynök bináris fájlok eltávolítása a hello gépről rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="d92db-272">Uninstalling hello agent binaries from hello machine has some consequences tooconsider:</span></span>

* <span data-ttu-id="d92db-273">Eltávolítja a hello fájlszűrő hello gépről, és az változások követését le van állítva.</span><span class="sxs-lookup"><span data-stu-id="d92db-273">It removes hello file-filter from hello machine, and tracking of changes is stopped.</span></span>
* <span data-ttu-id="d92db-274">Hello gépet eltávolítja az összes házirend az adatokat, de hello házirenddel kapcsolatos információk továbbra is toobe hello szolgáltatásban tárolja.</span><span class="sxs-lookup"><span data-stu-id="d92db-274">All policy information is removed from hello machine, but hello policy information continues toobe stored in hello service.</span></span>
* <span data-ttu-id="d92db-275">A mentési ütemezések törlődnek, és nincs további biztonsági mentés készül.</span><span class="sxs-lookup"><span data-stu-id="d92db-275">All backup schedules are removed, and no further backups are taken.</span></span>

<span data-ttu-id="d92db-276">Azonban hello Azure marad a adataihoz, és megőrzi hello megőrzési házirend beállítása adott meg.</span><span class="sxs-lookup"><span data-stu-id="d92db-276">However, hello data stored in Azure remains and is retained as per hello retention policy setup by you.</span></span> <span data-ttu-id="d92db-277">Régebbi pontok automatikusan van elavult.</span><span class="sxs-lookup"><span data-stu-id="d92db-277">Older points are automatically aged out.</span></span>

## <a name="remote-management"></a><span data-ttu-id="d92db-278">Távfelügyelet</span><span class="sxs-lookup"><span data-stu-id="d92db-278">Remote management</span></span>
<span data-ttu-id="d92db-279">Minden hello felügyeleti hello Azure Backup szolgáltatás ügynöke, a házirendek és az adatforrások távolról végezhető el a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d92db-279">All hello management around hello Azure Backup agent, policies, and data sources can be done remotely through PowerShell.</span></span> <span data-ttu-id="d92db-280">távolról felügyelt hello gép kell toobe megfelelően előkészítve.</span><span class="sxs-lookup"><span data-stu-id="d92db-280">hello machine that will be managed remotely needs toobe prepared correctly.</span></span>

<span data-ttu-id="d92db-281">Alapértelmezés szerint kézi indítási hello WinRM szolgáltatás van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="d92db-281">By default, hello WinRM service is configured for manual startup.</span></span> <span data-ttu-id="d92db-282">hello indítási típusa túl be kell állítani*automatikus* és hello szolgáltatást el kell indítani.</span><span class="sxs-lookup"><span data-stu-id="d92db-282">hello startup type must be set too*Automatic* and hello service should be started.</span></span> <span data-ttu-id="d92db-283">tooverify, amely hello a WinRM szolgáltatás fut, hello hello állapot tulajdonság értékének meg kell *futtató*.</span><span class="sxs-lookup"><span data-stu-id="d92db-283">tooverify that hello WinRM service is running, hello value of hello Status property should be *Running*.</span></span>

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

<span data-ttu-id="d92db-284">PowerShell távoli eljáráshívás kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="d92db-284">PowerShell should be configured for remoting.</span></span>

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up tooreceive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

<span data-ttu-id="d92db-285">hello gép most távolról felügyelhetők - hello ügynök telepítése hello kezdve.</span><span class="sxs-lookup"><span data-stu-id="d92db-285">hello machine can now be managed remotely - starting from hello installation of hello agent.</span></span> <span data-ttu-id="d92db-286">Például a következő parancsfájl hello hello ügynök toohello távoli számítógépre másolja, és telepíti azt.</span><span class="sxs-lookup"><span data-stu-id="d92db-286">For example, hello following script copies hello agent toohello remote machine and installs it.</span></span>

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a><span data-ttu-id="d92db-287">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d92db-287">Next steps</span></span>
<span data-ttu-id="d92db-288">További információ az Azure biztonsági mentés a Windows Server vagy Windows-ügyfélen lásd:</span><span class="sxs-lookup"><span data-stu-id="d92db-288">For more information about Azure Backup for Windows Server/Client see</span></span>

* [<span data-ttu-id="d92db-289">Bevezetés tooAzure biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="d92db-289">Introduction tooAzure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="d92db-290">Windows-kiszolgálók biztonsági mentését</span><span class="sxs-lookup"><span data-stu-id="d92db-290">Back up Windows Servers</span></span>](backup-configure-vault.md)
