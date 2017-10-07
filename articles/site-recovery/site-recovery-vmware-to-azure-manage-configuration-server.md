---
title: " Az Azure Site Recovery konfigurációs kiszolgáló kezelése |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset és kezelheti a konfigurációs kiszolgálót."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 2852bcd25409121be46a1ebf135ebfcdce6e5de5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-configuration-server"></a><span data-ttu-id="49835-103">A konfigurációs kiszolgáló kezelése</span><span class="sxs-lookup"><span data-stu-id="49835-103">Manage a Configuration Server</span></span>

<span data-ttu-id="49835-104">Konfigurációs kiszolgáló között hello Site Recovery services és a helyszíni infrastruktúra koordinátora működik.</span><span class="sxs-lookup"><span data-stu-id="49835-104">Configuration Server acts as a coordinator between hello Site Recovery services and your on-premises infrastructure.</span></span> <span data-ttu-id="49835-105">Ez a cikk ismerteti, hogyan állítsa be, konfigurálása és kezelése hello konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="49835-105">This article describes how you can set up, configure, and manage hello Configuration Server.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49835-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="49835-106">Prerequisites</span></span>
<span data-ttu-id="49835-107">hello az alábbiakban hello minimális hardver-, szoftver, és hálózati konfiguráció szükséges tooset konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="49835-107">hello following are hello minimum hardware, software, and network configuration required tooset up a Configuration Server.</span></span>

> [!NOTE]
> <span data-ttu-id="49835-108">[Kapacitástervezés](site-recovery-capacity-planner.md) van egy fontos lépés tooensure központi telepítését hello konfigurációs kiszolgáló konfigurálása, hogy a csomagok a terhelési követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="49835-108">[Capacity planning](site-recovery-capacity-planner.md) is an important step tooensure that you deploy hello Configuration Server with a configuration that suites your load requirements.</span></span> <span data-ttu-id="49835-109">Tudjon meg többet az [méretezése a konfigurációs kiszolgáló követelményei](#sizing-requirements-for-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="49835-109">Read more about [Sizing requirements for a Configuration Server](#sizing-requirements-for-a-configuration-server).</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-configuration-server-software"></a><span data-ttu-id="49835-110">Hello konfigurációs kiszolgáló szoftver letöltése</span><span class="sxs-lookup"><span data-stu-id="49835-110">Downloading hello Configuration Server software</span></span>
1. <span data-ttu-id="49835-111">Jelentkezzen be toohello az Azure portálon, és keresse meg tooyour Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="49835-111">Log on toohello Azure portal and browse tooyour Recovery Services Vault.</span></span>
2. <span data-ttu-id="49835-112">Keresse meg a túl**Site Recovery-infrastruktúra** > **konfigurációs kiszolgálók** (alatt a VMware és fizikai gépek).</span><span class="sxs-lookup"><span data-stu-id="49835-112">Browse too**Site Recovery Infrastructure** > **Configuration Servers** (under For VMware & Physical Machines).</span></span>

  ![Kiszolgálók lap hozzáadása](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. <span data-ttu-id="49835-114">Kattintson a hello **+ kiszolgálók** gombra.</span><span class="sxs-lookup"><span data-stu-id="49835-114">Click hello **+Servers** button.</span></span>
4. <span data-ttu-id="49835-115">A hello **kiszolgáló hozzáadása** lapján kattintson a hello Letöltés gomb toodownload hello regisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="49835-115">On hello **Add Server** page, click hello Download button toodownload hello Registration key.</span></span> <span data-ttu-id="49835-116">Ezt a kulcsot kell hello konfigurációs kiszolgáló telepítési tooregister során az Azure Site Recovery szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="49835-116">You need this key during hello Configuration Server installation tooregister it with Azure Site Recovery service.</span></span>
5. <span data-ttu-id="49835-117">Kattintson a hello **letöltési hello Microsoft Azure Site Recovery az egységes telepítő** hivatkozás toodownload hello legújabb verziójának hello konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="49835-117">Click hello **Download hello Microsoft Azure Site Recovery Unified Setup** link toodownload hello latest version of hello Configuration Server.</span></span>

  ![Letöltési oldala](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  <span data-ttu-id="49835-119">Konfigurációs kiszolgáló hello legújabb verziója letölthető közvetlenül [Microsoft Download Center letöltési oldala](http://aka.ms/unifiedsetup)</span><span class="sxs-lookup"><span data-stu-id="49835-119">Latest version of hello Configuration Server can be downloaded directly from [Microsoft Download Center download page](http://aka.ms/unifiedsetup)</span></span>

## <a name="installing-and-registering-a-configuration-server-from-gui"></a><span data-ttu-id="49835-120">Telepítése és regisztrálása a konfigurációs kiszolgáló grafikus felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="49835-120">Installing and Registering a Configuration Server from GUI</span></span>
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a><span data-ttu-id="49835-121">Telepítése és regisztrálása a konfigurációs kiszolgáló parancssor használatával</span><span class="sxs-lookup"><span data-stu-id="49835-121">Installing and registering a Configuration Server using Command line</span></span>

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a><span data-ttu-id="49835-122">Példa</span><span class="sxs-lookup"><span data-stu-id="49835-122">Sample usage</span></span>
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a><span data-ttu-id="49835-123">Konfigurációs kiszolgáló telepítő parancssori argumentumokat.</span><span class="sxs-lookup"><span data-stu-id="49835-123">Configuration Server installer command-line arguments.</span></span>
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a><span data-ttu-id="49835-124">Egy MySql hitelesítőadat-fájlt létrehozni</span><span class="sxs-lookup"><span data-stu-id="49835-124">Create a MySql credentials file</span></span>
<span data-ttu-id="49835-125">MySQLCredsFilePath paraméter egy fájl fogadja bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="49835-125">MySQLCredsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="49835-126">Használja a következő formátumú, és adja át bemeneti MySQLCredsFilePath paraméterként hello hello fájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="49835-126">Create hello file using hello following format and pass it as input MySQLCredsFilePath parameter.</span></span>
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a><span data-ttu-id="49835-127">Proxykiszolgáló-beállítások konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="49835-127">Create a proxy settings configuration file</span></span>
<span data-ttu-id="49835-128">ProxySettingsFilePath paraméter egy fájl fogadja bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="49835-128">ProxySettingsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="49835-129">Használja a következő formátumú, és adja át bemeneti ProxySettingsFilePath paraméterként hello hello fájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="49835-129">Create hello file using hello following format and pass it as input ProxySettingsFilePath parameter.</span></span>

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a><span data-ttu-id="49835-130">Konfigurációs kiszolgáló proxy beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="49835-130">Modifying proxy settings for Configuration Server</span></span>
1. <span data-ttu-id="49835-131">Bejelentkezési tooyour konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="49835-131">Login tooyour Configuration Server.</span></span>
2. <span data-ttu-id="49835-132">Indítsa el a hello parancsikon használatával hello cspsconfigtool.exe a.</span><span class="sxs-lookup"><span data-stu-id="49835-132">Launch hello cspsconfigtool.exe using hello shortcut on your.</span></span>
3. <span data-ttu-id="49835-133">Kattintson a hello **tároló regisztrációs** fülre.</span><span class="sxs-lookup"><span data-stu-id="49835-133">Click hello **Vault Registration** tab.</span></span>
4. <span data-ttu-id="49835-134">Töltsön le egy új tároló regisztrációs fájlt hello portálról, és adja meg a bemeneti toohello eszközként.</span><span class="sxs-lookup"><span data-stu-id="49835-134">Download a new Vault Registration file from hello portal and provide it as input toohello tool.</span></span>

  ![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. <span data-ttu-id="49835-136">Adja meg hello új proxykiszolgáló adatait, majd kattintson a hello **regisztrálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="49835-136">Provide hello new Proxy Server details and click hello **Register** button.</span></span>
6. <span data-ttu-id="49835-137">Nyisson meg egy rendszergazda PowerShell parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="49835-137">Open an Admin PowerShell command window.</span></span>
7. <span data-ttu-id="49835-138">Futtassa a következő parancs hello</span><span class="sxs-lookup"><span data-stu-id="49835-138">Run hello following command</span></span>
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  <span data-ttu-id="49835-139">Ha kibővített folyamat kiszolgálók csatolt toothis konfigurációs kiszolgáló, túl kell[hello proxybeállításai hárítsa el az összes hello kibővített folyamat kiszolgálók](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) a környezetben.</span><span class="sxs-lookup"><span data-stu-id="49835-139">If you have Scale-out Process servers attached toothis Configuration Server, you need too[fix hello proxy settings on all hello scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) in your deployment.</span></span>

## <a name="re-register-a-configuration-server-with-hello-same-recovery-services-vault"></a><span data-ttu-id="49835-140">Regisztrálja újra a konfigurációs kiszolgáló hello azonos Recovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="49835-140">Re-register a Configuration Server with hello same Recovery Services Vault</span></span>
  1. <span data-ttu-id="49835-141">Bejelentkezési tooyour konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="49835-141">Login tooyour Configuration Server.</span></span>
  2. <span data-ttu-id="49835-142">Indítsa el a hello cspsconfigtool.exe használatával hello parancsikont az asztalon.</span><span class="sxs-lookup"><span data-stu-id="49835-142">Launch hello cspsconfigtool.exe using hello shortcut on your desktop.</span></span>
  3. <span data-ttu-id="49835-143">Kattintson a hello **tároló regisztrációs** fülre.</span><span class="sxs-lookup"><span data-stu-id="49835-143">Click hello **Vault Registration** tab.</span></span>
  4. <span data-ttu-id="49835-144">Töltsön le új regisztrációs fájl hello portálról, és adja meg a bemeneti toohello eszközként.</span><span class="sxs-lookup"><span data-stu-id="49835-144">Download a new Registration file from hello portal and provide it as input toohello tool.</span></span>
        <span data-ttu-id="49835-145">![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span><span class="sxs-lookup"><span data-stu-id="49835-145">![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span></span>
  5. <span data-ttu-id="49835-146">Hello proxykiszolgáló részletesen, és kattintson a hello **regisztrálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="49835-146">Provide hello Proxy Server details and click hello **Register** button.</span></span>  
  6. <span data-ttu-id="49835-147">Nyisson meg egy rendszergazda PowerShell parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="49835-147">Open an Admin PowerShell command window.</span></span>
  7. <span data-ttu-id="49835-148">Futtassa a következő parancs hello</span><span class="sxs-lookup"><span data-stu-id="49835-148">Run hello following command</span></span>

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  <span data-ttu-id="49835-149">Ha kibővített folyamat kiszolgálók csatolt toothis konfigurációs kiszolgáló, túl kell[regisztrálja újra az összes hello kibővített folyamat kiszolgálók](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) a környezetben.</span><span class="sxs-lookup"><span data-stu-id="49835-149">If you have Scale-out Process servers attached toothis Configuration Server, you need too[re-register all hello scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) in your deployment.</span></span>

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a><span data-ttu-id="49835-150">A konfigurációs kiszolgáló regisztrálása egy másik Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="49835-150">Registering a Configuration Server with a different Recovery Services Vault.</span></span>
1. <span data-ttu-id="49835-151">Bejelentkezési tooyour konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="49835-151">Login tooyour Configuration Server.</span></span>
2. <span data-ttu-id="49835-152">egy rendszergazdai parancssorból futtassa a hello parancsot</span><span class="sxs-lookup"><span data-stu-id="49835-152">from an admin command prompt, run hello command</span></span>

```
reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
net stop dra
```
3. <span data-ttu-id="49835-153">Indítsa el a hello parancsikon használatával hello cspsconfigtool.exe a.</span><span class="sxs-lookup"><span data-stu-id="49835-153">Launch hello cspsconfigtool.exe using hello shortcut on your.</span></span>
4. <span data-ttu-id="49835-154">Kattintson a hello **tároló regisztrációs** fülre.</span><span class="sxs-lookup"><span data-stu-id="49835-154">Click hello **Vault Registration** tab.</span></span>
5. <span data-ttu-id="49835-155">Töltsön le új regisztrációs fájl hello portálról, és adja meg a bemeneti toohello eszközként.</span><span class="sxs-lookup"><span data-stu-id="49835-155">Download a new Registration file from hello portal and provide it as input toohello tool.</span></span>

    ![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. <span data-ttu-id="49835-157">Hello proxykiszolgáló részletesen, és kattintson a hello **regisztrálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="49835-157">Provide hello Proxy Server details and click hello **Register** button.</span></span>  
7. <span data-ttu-id="49835-158">Nyisson meg egy rendszergazda PowerShell parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="49835-158">Open an Admin PowerShell command window.</span></span>
8. <span data-ttu-id="49835-159">Futtassa a következő parancs hello</span><span class="sxs-lookup"><span data-stu-id="49835-159">Run hello following command</span></span>
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="decommissioning-a-configuration-server"></a><span data-ttu-id="49835-160">Konfigurációs kiszolgáló leszerelése</span><span class="sxs-lookup"><span data-stu-id="49835-160">Decommissioning a Configuration Server</span></span>
<span data-ttu-id="49835-161">Hello következő győződjön meg arról, a konfigurációs kiszolgáló leszerelése megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="49835-161">Ensure hello following before you start decommissioning your Configuration Server.</span></span>
1. <span data-ttu-id="49835-162">Tiltsa le a védelmet az összes virtuális gép ezen a konfigurációs kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="49835-162">Disable protection for all virtual machines under this Configuration Server.</span></span>
2. <span data-ttu-id="49835-163">Szüntesse meg a konfigurációs kiszolgáló hello összes replikációs házirendet.</span><span class="sxs-lookup"><span data-stu-id="49835-163">Disassociate all Replication policies from hello Configuration Server.</span></span>
3. <span data-ttu-id="49835-164">Törölje az összes Vcenter kiszolgáló vagy vSphere-gazdagépek társított toohello konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="49835-164">Delete all vCenters servers/vSphere hosts that are associated toohello Configuration Server.</span></span>

### <a name="delete-hello-configuration-server-from-azure-portal"></a><span data-ttu-id="49835-165">Törölje a hello konfigurációs kiszolgáló Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="49835-165">Delete hello Configuration Server from Azure portal</span></span>
1. <span data-ttu-id="49835-166">Azure-portálon Tallózás túl**Site Recovery-infrastruktúra** > **konfigurációs kiszolgálók** hello tároló menüből.</span><span class="sxs-lookup"><span data-stu-id="49835-166">In Azure portal, browse too**Site Recovery Infrastructure** > **Configuration Servers** from hello Vault menu.</span></span>
2. <span data-ttu-id="49835-167">Kattintson a konfigurációs kiszolgáló, amelyet az toodecommission hello.</span><span class="sxs-lookup"><span data-stu-id="49835-167">Click hello Configuration Server that you want toodecommission.</span></span>
3. <span data-ttu-id="49835-168">Hello konfigurációs kiszolgáló adatai lapon kattintson a hello Törlés gomb.</span><span class="sxs-lookup"><span data-stu-id="49835-168">On hello Configuration Server's details page, click hello Delete button.</span></span>

  ![törlés-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. <span data-ttu-id="49835-170">Kattintson a **Igen** hello server tooconfirm hello törlését.</span><span class="sxs-lookup"><span data-stu-id="49835-170">Click **Yes** tooconfirm hello deletion of hello server.</span></span>

  >[!WARNING]
  <span data-ttu-id="49835-171">Ha a virtuális gép, replikációs házirendek vagy vCenter-kiszolgáló vagy vSphere gazdagépek konfigurációs kiszolgálóhoz társított hello kiszolgáló nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="49835-171">If you have any virtual machines, Replication policies or vCenter servers/vSphere hosts associated with this Configuration Server, you cannot delete hello server.</span></span> <span data-ttu-id="49835-172">Törölje ezeket az entitásokat előtt toodelete hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="49835-172">Delete these entities before you try toodelete hello vault.</span></span>

### <a name="uninstall-hello-configuration-server-software-and-its-dependencies"></a><span data-ttu-id="49835-173">Távolítsa el a hello konfigurációs kiszolgálók és a függőségek</span><span class="sxs-lookup"><span data-stu-id="49835-173">Uninstall hello Configuration Server software and its dependencies</span></span>
  > [!TIP]
  <span data-ttu-id="49835-174">Ha újra tooreuse hello konfigurációs kiszolgáló Azure Site Recovery szolgáltatással, majd továbbléphet toostep 4 közvetlenül</span><span class="sxs-lookup"><span data-stu-id="49835-174">If you plan tooreuse hello Configuration Server with Azure Site Recovery again, then you can skip toostep 4 directly</span></span>

1. <span data-ttu-id="49835-175">Jelentkezzen be a konfigurációs kiszolgáló toohello rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="49835-175">Log on toohello Configuration Server as an Administrator.</span></span>
2. <span data-ttu-id="49835-176">Nyissa meg Vezérlőpult > Program > Programok eltávolítása</span><span class="sxs-lookup"><span data-stu-id="49835-176">Open up Control Panel > Program > Uninstall Programs</span></span>
3. <span data-ttu-id="49835-177">Távolítsa el a következő feladatütemezési hello hello programok:</span><span class="sxs-lookup"><span data-stu-id="49835-177">Uninstall hello programs in hello following sequence:</span></span>
  * <span data-ttu-id="49835-178">Microsoft Azure Recovery Services Agent</span><span class="sxs-lookup"><span data-stu-id="49835-178">Microsoft Azure Recovery Services Agent</span></span>
  * <span data-ttu-id="49835-179">Microsoft Azure Site helyreállítási mobilitási szolgáltatás/fő célkiszolgáló megadásával</span><span class="sxs-lookup"><span data-stu-id="49835-179">Microsoft Azure Site Recovery Mobility Service/Master Target server</span></span>
  * <span data-ttu-id="49835-180">A Microsoft Azure Site Recovery Providert</span><span class="sxs-lookup"><span data-stu-id="49835-180">Microsoft Azure Site Recovery Provider</span></span>
  * <span data-ttu-id="49835-181">Microsoft Azure Recovery konfigurációs kiszolgáló/folyamat helykiszolgáló</span><span class="sxs-lookup"><span data-stu-id="49835-181">Microsoft Azure Site Recovery Configuration Server/Process Server</span></span>
  * <span data-ttu-id="49835-182">A Microsoft Azure Site helyreállítási konfigurációs kiszolgáló függőségek</span><span class="sxs-lookup"><span data-stu-id="49835-182">Microsoft Azure Site Recovery Configuration Server Dependencies</span></span>
  * <span data-ttu-id="49835-183">MySQL Server 5.5</span><span class="sxs-lookup"><span data-stu-id="49835-183">MySQL Server 5.5</span></span>
4. <span data-ttu-id="49835-184">A következő parancsot a hello és rendszergazdai parancssorból futtatható.</span><span class="sxs-lookup"><span data-stu-id="49835-184">Run hello following command from and admin command prompt.</span></span>
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a><span data-ttu-id="49835-185">Konfigurációs kiszolgáló biztonságos szoftvercsatorna Layer(SSL) tanúsítványainak megújítása</span><span class="sxs-lookup"><span data-stu-id="49835-185">Renew Configuration Server Secure Socket Layer(SSL) Certificates</span></span>
<span data-ttu-id="49835-186">Konfigurációs kiszolgáló hello egy beépített webkiszolgálót, amely koordinálja a hello mobilitási szolgáltatás folyamat kiszolgálók hello tevékenységeit és a fő célkiszolgáló kiszolgálók csatlakoztatva toohello konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="49835-186">hello Configuration Server has an inbuilt webserver, which orchestrates hello activities of hello Mobility Service, Process Servers, and Master Target servers connected toohello Configuration Server.</span></span> <span data-ttu-id="49835-187">hello konfigurációs kiszolgáló webkiszolgálót használ egy SSL-tanúsítvány tooauthenticate az ügyfeleknek.</span><span class="sxs-lookup"><span data-stu-id="49835-187">hello Configuration Server's webserver uses an SSL certificate tooauthenticate its clients.</span></span> <span data-ttu-id="49835-188">Ez a tanúsítvány rendelkezik egy három év lejárta, és a következő metódus hello segítségével lehet megújítani:</span><span class="sxs-lookup"><span data-stu-id="49835-188">This certificate has an expiry of three years and can be renewed at any time using hello following method:</span></span>

> [!WARNING]
<span data-ttu-id="49835-189">Tanúsítványlejárat csak verziót lehetne végrehajtani 9.4.XXXX. X vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="49835-189">Certificate expiry can be performed only on version 9.4.XXXX.X or higher.</span></span> <span data-ttu-id="49835-190">Frissítse a hello Azure Site Recovery minden összetevőjét (konfigurációs kiszolgáló, a Folyamatkiszolgáló, fő célkiszolgáló, Mobilitásiszolgáltatás) hello tanúsítványok megújítása munkafolyamat megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="49835-190">Upgrade all hello Azure Site Recovery components (Configuration Server, Process Server, Master Target Server, Mobility Service) before you start hello Renew Certificates workflow.</span></span>

1. <span data-ttu-id="49835-191">A hello Azure-portálon, keresse meg a tároló tooyour > Site Recovery-infrastruktúra > konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="49835-191">On hello Azure portal, browse tooyour Vault > Site Recovery Infrastructure > Configuration Server.</span></span>
2. <span data-ttu-id="49835-192">Kattintson a hello konfigurációs kiszolgáló, amelyhez toorenew kell hello SSL-tanúsítványa.</span><span class="sxs-lookup"><span data-stu-id="49835-192">Click hello Configuration Server for which you need toorenew hello SSL Certificate for.</span></span>
3. <span data-ttu-id="49835-193">A konfigurációs kiszolgáló állapotának hello lásd: hello lejárati dátumának hello SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="49835-193">Under hello Configuration Server health, you can see hello expiry date for hello SSL Certificate.</span></span>
4. <span data-ttu-id="49835-194">Megújítás hello tanúsítvány hello kattintva **tanúsítványok megújítása** művelet látható hello kép a következő módon:</span><span class="sxs-lookup"><span data-stu-id="49835-194">Renew hello certificate by clicking hello **Renew Certificates** action as shown in hello following image:</span></span>

  ![törlés-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a><span data-ttu-id="49835-196">Secure Socket Layer tanúsítvány lejáratával kapcsolatos figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="49835-196">Secure Socket Layer certificate expiry warning</span></span>

> [!NOTE]
<span data-ttu-id="49835-197">hello SSL-tanúsítvány érvényességi 2016. május előtt történt összes telepítések be lett állítva tooone év.</span><span class="sxs-lookup"><span data-stu-id="49835-197">hello SSL Certificate's validity for all installations that happened before May 2016 was set tooone year.</span></span> <span data-ttu-id="49835-198">elindítását követően megtekintheti a tanúsítványok lejárati értesítéseinek hello Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="49835-198">you have started seeing certificate expiry notifications showing up in hello Azure portal.</span></span>

1. <span data-ttu-id="49835-199">Ha hello konfigurációs kiszolgáló SSL-tanúsítvány tooexpire hello a következő két hónapon, hello szolgáltatás elindul, értesítési hello Azure-portálon & e-mailek (kell előfizetett toobe tooAzure Site Recovery-értesítések).</span><span class="sxs-lookup"><span data-stu-id="49835-199">If hello Configuration Server's SSL certificate is going tooexpire in hello next two months, hello service starts notifying users via hello Azure portal & email (you need toobe subscribed tooAzure Site Recovery notifications).</span></span> <span data-ttu-id="49835-200">Amikor megkezdi a értesítésszalagról hello tároló-erőforrás lapon látható.</span><span class="sxs-lookup"><span data-stu-id="49835-200">You start seeing a notification banner on hello Vault's resource page.</span></span>

  ![tanúsítvány-értesítés](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. <span data-ttu-id="49835-202">Hello szalagcím tooget további részletek a hello tanúsítványlejárat gombra.</span><span class="sxs-lookup"><span data-stu-id="49835-202">Click hello banner tooget additional details on hello Certificate expiry.</span></span>

  ![tanúsítvány-részletek](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  <span data-ttu-id="49835-204">Ahelyett, hogy ha egy **megújítása most** gomb megjelenik egy **frissítés most** gombra.</span><span class="sxs-lookup"><span data-stu-id="49835-204">If instead of a **Renew Now** button you see an **Upgrade Now** button.</span></span> <span data-ttu-id="49835-205">Ez azt jelenti, hogy vannak-e néhány összetevőt a környezetben, amelyek még nincsenek frissített too9.4.xxxx.x vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="49835-205">This means that there are some components in your environment that have not yet been upgraded too9.4.xxxx.x or higher versions.</span></span>

## <a name="sizing-requirements-for-a-configuration-server"></a><span data-ttu-id="49835-206">A konfigurációs kiszolgáló méretezése követelményei</span><span class="sxs-lookup"><span data-stu-id="49835-206">Sizing requirements for a Configuration Server</span></span>

| <span data-ttu-id="49835-207">**PROCESSZOR**</span><span class="sxs-lookup"><span data-stu-id="49835-207">**CPU**</span></span> | <span data-ttu-id="49835-208">**Memória**</span><span class="sxs-lookup"><span data-stu-id="49835-208">**Memory**</span></span> | <span data-ttu-id="49835-209">**Gyorsítótár-lemez mérete**</span><span class="sxs-lookup"><span data-stu-id="49835-209">**Cache disk size**</span></span> | <span data-ttu-id="49835-210">**Adatváltozási sebesség**</span><span class="sxs-lookup"><span data-stu-id="49835-210">**Data change rate**</span></span> | <span data-ttu-id="49835-211">**Védett gépek**</span><span class="sxs-lookup"><span data-stu-id="49835-211">**Protected machines**</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="49835-212">8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag)</span><span class="sxs-lookup"><span data-stu-id="49835-212">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="49835-213">16 GB</span><span class="sxs-lookup"><span data-stu-id="49835-213">16 GB</span></span> |<span data-ttu-id="49835-214">300 GB</span><span class="sxs-lookup"><span data-stu-id="49835-214">300 GB</span></span> |<span data-ttu-id="49835-215">500 GB vagy kevesebb</span><span class="sxs-lookup"><span data-stu-id="49835-215">500 GB or less</span></span> |<span data-ttu-id="49835-216">100-nál kevesebb gépek replikálása.</span><span class="sxs-lookup"><span data-stu-id="49835-216">Replicate fewer than 100 machines.</span></span> |
| <span data-ttu-id="49835-217">12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag)</span><span class="sxs-lookup"><span data-stu-id="49835-217">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="49835-218">18 GB</span><span class="sxs-lookup"><span data-stu-id="49835-218">18 GB</span></span> |<span data-ttu-id="49835-219">600 GB</span><span class="sxs-lookup"><span data-stu-id="49835-219">600 GB</span></span> |<span data-ttu-id="49835-220">500 GB too1 TB</span><span class="sxs-lookup"><span data-stu-id="49835-220">500 GB too1 TB</span></span> |<span data-ttu-id="49835-221">100-150 gépek közti replikálásához.</span><span class="sxs-lookup"><span data-stu-id="49835-221">Replicate between 100-150 machines.</span></span> |
| <span data-ttu-id="49835-222">16 Vcpu (2 sockets * @ 2,5 GHz-es 8 mag)</span><span class="sxs-lookup"><span data-stu-id="49835-222">16 vCPUs (2 sockets * 8 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="49835-223">32 GB</span><span class="sxs-lookup"><span data-stu-id="49835-223">32 GB</span></span> |<span data-ttu-id="49835-224">1 TB</span><span class="sxs-lookup"><span data-stu-id="49835-224">1 TB</span></span> |<span data-ttu-id="49835-225">1 TB-os too2 TB</span><span class="sxs-lookup"><span data-stu-id="49835-225">1 TB too2 TB</span></span> |<span data-ttu-id="49835-226">150-200 gépek közti replikálásához.</span><span class="sxs-lookup"><span data-stu-id="49835-226">Replicate between 150-200 machines.</span></span> |

  >[!TIP]
  <span data-ttu-id="49835-227">Ha a napi adatforgalommal meghaladja a 2 TB, vagy több mint 200 olyan virtuális gépet tooreplicate tervezi, ajánlott toodeploy további folyamat kiszolgálók tooload egyenleg hello replikációs forgalmat.</span><span class="sxs-lookup"><span data-stu-id="49835-227">If your daily data churn exceeds 2 TB, or you plan tooreplicate more than 200 virtual machines, it is recommended toodeploy additional process servers tooload balance hello replication traffic.</span></span> <span data-ttu-id="49835-228">További tudnivalók hogyan toodeploy kibővített folyamat kiszolgálója.</span><span class="sxs-lookup"><span data-stu-id="49835-228">Learn more about How toodeploy Scale-out Process severs.</span></span>


## <a name="common-issues"></a><span data-ttu-id="49835-229">Gyakori problémák</span><span class="sxs-lookup"><span data-stu-id="49835-229">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
