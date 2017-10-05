---
title: " Az Azure Site Recovery konfigurációs kiszolgáló kezelése |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan kezelheti a konfigurációs kiszolgáló."
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
ms.openlocfilehash: 36da8c7d0f3ace194522e5288f26069cf46d470e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-a-configuration-server"></a><span data-ttu-id="532d2-103">A konfigurációs kiszolgáló kezelése</span><span class="sxs-lookup"><span data-stu-id="532d2-103">Manage a Configuration Server</span></span>

<span data-ttu-id="532d2-104">Konfigurációs kiszolgáló között a Site Recovery services és a helyszíni infrastruktúra koordinátora működik.</span><span class="sxs-lookup"><span data-stu-id="532d2-104">Configuration Server acts as a coordinator between the Site Recovery services and your on-premises infrastructure.</span></span> <span data-ttu-id="532d2-105">Ez a cikk ismerteti, hogyan állítsa be, konfigurálhatja és kezelheti a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="532d2-105">This article describes how you can set up, configure, and manage the Configuration Server.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="532d2-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="532d2-106">Prerequisites</span></span>
<span data-ttu-id="532d2-107">A minimális hardver-, szoftver, és a konfigurációs kiszolgáló beállításához szükséges hálózati konfiguráció az alábbiakban.</span><span class="sxs-lookup"><span data-stu-id="532d2-107">The following are the minimum hardware, software, and network configuration required to set up a Configuration Server.</span></span>

> [!NOTE]
> <span data-ttu-id="532d2-108">[Kapacitástervezés](site-recovery-capacity-planner.md) egy fontos lépés annak érdekében, hogy telepít a konfigurációs kiszolgáló konfigurálása, hogy a csomagok a terhelési követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="532d2-108">[Capacity planning](site-recovery-capacity-planner.md) is an important step to ensure that you deploy the Configuration Server with a configuration that suites your load requirements.</span></span> <span data-ttu-id="532d2-109">Tudjon meg többet az [méretezése a konfigurációs kiszolgáló követelményei](#sizing-requirements-for-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="532d2-109">Read more about [Sizing requirements for a Configuration Server](#sizing-requirements-for-a-configuration-server).</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-configuration-server-software"></a><span data-ttu-id="532d2-110">A konfigurációs kiszolgáló szoftver letöltése</span><span class="sxs-lookup"><span data-stu-id="532d2-110">Downloading the Configuration Server software</span></span>
1. <span data-ttu-id="532d2-111">Jelentkezzen be az Azure-portálon, és keresse meg a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="532d2-111">Log on to the Azure portal and browse to your Recovery Services Vault.</span></span>
2. <span data-ttu-id="532d2-112">Keresse meg a **Site Recovery-infrastruktúra** > **konfigurációs kiszolgálók** (a VMware és fizikai gépek).</span><span class="sxs-lookup"><span data-stu-id="532d2-112">Browse to **Site Recovery Infrastructure** > **Configuration Servers** (under For VMware & Physical Machines).</span></span>

  ![Kiszolgálók lap hozzáadása](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. <span data-ttu-id="532d2-114">Kattintson a **+ kiszolgálók** gombra.</span><span class="sxs-lookup"><span data-stu-id="532d2-114">Click the **+Servers** button.</span></span>
4. <span data-ttu-id="532d2-115">Az a **kiszolgáló hozzáadása** lapon, a Letöltés gombra kattintva töltse le a regisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="532d2-115">On the **Add Server** page, click the Download button to download the Registration key.</span></span> <span data-ttu-id="532d2-116">A konfigurációs kiszolgáló telepítése az Azure Site Recovery szolgáltatásban való regisztrálása során kell ezt a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="532d2-116">You need this key during the Configuration Server installation to register it with Azure Site Recovery service.</span></span>
5. <span data-ttu-id="532d2-117">Kattintson a **töltse le a Microsoft Azure Site Recovery az egységes telepítő** hivatkozást a konfigurációs kiszolgáló legújabb verziójának letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="532d2-117">Click the **Download the Microsoft Azure Site Recovery Unified Setup** link to download the latest version of the Configuration Server.</span></span>

  ![Letöltési oldala](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  <span data-ttu-id="532d2-119">A konfigurációs kiszolgáló legújabb verziója letölthető közvetlenül [Microsoft Download Center letöltési oldala](http://aka.ms/unifiedsetup)</span><span class="sxs-lookup"><span data-stu-id="532d2-119">Latest version of the Configuration Server can be downloaded directly from [Microsoft Download Center download page](http://aka.ms/unifiedsetup)</span></span>

## <a name="installing-and-registering-a-configuration-server-from-gui"></a><span data-ttu-id="532d2-120">Telepítése és regisztrálása a konfigurációs kiszolgáló grafikus felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="532d2-120">Installing and Registering a Configuration Server from GUI</span></span>
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a><span data-ttu-id="532d2-121">Telepítése és regisztrálása a konfigurációs kiszolgáló parancssor használatával</span><span class="sxs-lookup"><span data-stu-id="532d2-121">Installing and registering a Configuration Server using Command line</span></span>

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a><span data-ttu-id="532d2-122">Példa</span><span class="sxs-lookup"><span data-stu-id="532d2-122">Sample usage</span></span>
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a><span data-ttu-id="532d2-123">Konfigurációs kiszolgáló telepítő parancssori argumentumokat.</span><span class="sxs-lookup"><span data-stu-id="532d2-123">Configuration Server installer command-line arguments.</span></span>
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a><span data-ttu-id="532d2-124">Egy MySql hitelesítőadat-fájlt létrehozni</span><span class="sxs-lookup"><span data-stu-id="532d2-124">Create a MySql credentials file</span></span>
<span data-ttu-id="532d2-125">MySQLCredsFilePath paraméter egy fájl fogadja bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="532d2-125">MySQLCredsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="532d2-126">Hozza létre a fájlt a következő formátumban, és adja át bemeneti MySQLCredsFilePath paraméterként.</span><span class="sxs-lookup"><span data-stu-id="532d2-126">Create the file using the following format and pass it as input MySQLCredsFilePath parameter.</span></span>
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a><span data-ttu-id="532d2-127">Proxykiszolgáló-beállítások konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="532d2-127">Create a proxy settings configuration file</span></span>
<span data-ttu-id="532d2-128">ProxySettingsFilePath paraméter egy fájl fogadja bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="532d2-128">ProxySettingsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="532d2-129">Hozza létre a fájlt a következő formátumban, és adja át bemeneti ProxySettingsFilePath paraméterként.</span><span class="sxs-lookup"><span data-stu-id="532d2-129">Create the file using the following format and pass it as input ProxySettingsFilePath parameter.</span></span>

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a><span data-ttu-id="532d2-130">Konfigurációs kiszolgáló proxy beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="532d2-130">Modifying proxy settings for Configuration Server</span></span>
1. <span data-ttu-id="532d2-131">A bejelentkezési és a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="532d2-131">Login to your Configuration Server.</span></span>
2. <span data-ttu-id="532d2-132">Indítsa el a használatával a helyi cspsconfigtool.exe a.</span><span class="sxs-lookup"><span data-stu-id="532d2-132">Launch the cspsconfigtool.exe using the shortcut on your.</span></span>
3. <span data-ttu-id="532d2-133">Kattintson a **tároló regisztrációs** fülre.</span><span class="sxs-lookup"><span data-stu-id="532d2-133">Click the **Vault Registration** tab.</span></span>
4. <span data-ttu-id="532d2-134">Töltsön le egy új tároló regisztrációs fájlt a portálról, és adja meg az eszköz számára.</span><span class="sxs-lookup"><span data-stu-id="532d2-134">Download a new Vault Registration file from the portal and provide it as input to the tool.</span></span>

  ![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. <span data-ttu-id="532d2-136">Adja meg az új proxykiszolgáló adatait, és kattintson a **regisztrálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="532d2-136">Provide the new Proxy Server details and click the **Register** button.</span></span>
6. <span data-ttu-id="532d2-137">Nyisson meg egy rendszergazda PowerShell parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="532d2-137">Open an Admin PowerShell command window.</span></span>
7. <span data-ttu-id="532d2-138">A következő parancsot</span><span class="sxs-lookup"><span data-stu-id="532d2-138">Run the following command</span></span>
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  <span data-ttu-id="532d2-139">Ha a konfigurációs kiszolgálóhoz kapcsolódó kibővített folyamat kiszolgálóval rendelkezik, akkor [hárítsa el a proxykiszolgáló beállításait a kibővített folyamat összes kiszolgálójára](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) a környezetben.</span><span class="sxs-lookup"><span data-stu-id="532d2-139">If you have Scale-out Process servers attached to this Configuration Server, you need to [fix the proxy settings on all the scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) in your deployment.</span></span>

## <a name="re-register-a-configuration-server-with-the-same-recovery-services-vault"></a><span data-ttu-id="532d2-140">Regisztrálja újra a konfigurációs kiszolgáló a azonos Recovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="532d2-140">Re-register a Configuration Server with the same Recovery Services Vault</span></span>
  1. <span data-ttu-id="532d2-141">A bejelentkezési és a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="532d2-141">Login to your Configuration Server.</span></span>
  2. <span data-ttu-id="532d2-142">Indítsa el a cspsconfigtool.exe használatával a parancsikont az asztalon.</span><span class="sxs-lookup"><span data-stu-id="532d2-142">Launch the cspsconfigtool.exe using the shortcut on your desktop.</span></span>
  3. <span data-ttu-id="532d2-143">Kattintson a **tároló regisztrációs** fülre.</span><span class="sxs-lookup"><span data-stu-id="532d2-143">Click the **Vault Registration** tab.</span></span>
  4. <span data-ttu-id="532d2-144">Töltsön le új regisztrációs fájlt a portálról, és adja meg az eszköz számára.</span><span class="sxs-lookup"><span data-stu-id="532d2-144">Download a new Registration file from the portal and provide it as input to the tool.</span></span>
        <span data-ttu-id="532d2-145">![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span><span class="sxs-lookup"><span data-stu-id="532d2-145">![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span></span>
  5. <span data-ttu-id="532d2-146">Adja meg a proxykiszolgáló adatait, és kattintson a **regisztrálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="532d2-146">Provide the Proxy Server details and click the **Register** button.</span></span>  
  6. <span data-ttu-id="532d2-147">Nyisson meg egy rendszergazda PowerShell parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="532d2-147">Open an Admin PowerShell command window.</span></span>
  7. <span data-ttu-id="532d2-148">A következő parancsot</span><span class="sxs-lookup"><span data-stu-id="532d2-148">Run the following command</span></span>

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  <span data-ttu-id="532d2-149">Ha a konfigurációs kiszolgálóhoz kapcsolódó kibővített folyamat kiszolgálóval rendelkezik, akkor [regisztrálja újra a kibővített folyamat kiszolgálók](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) a környezetben.</span><span class="sxs-lookup"><span data-stu-id="532d2-149">If you have Scale-out Process servers attached to this Configuration Server, you need to [re-register all the scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) in your deployment.</span></span>

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a><span data-ttu-id="532d2-150">A konfigurációs kiszolgáló regisztrálása egy másik Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="532d2-150">Registering a Configuration Server with a different Recovery Services Vault.</span></span>
1. <span data-ttu-id="532d2-151">A bejelentkezési és a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="532d2-151">Login to your Configuration Server.</span></span>
2. <span data-ttu-id="532d2-152">Futtassa a parancsot egy rendszergazdai parancssorból</span><span class="sxs-lookup"><span data-stu-id="532d2-152">from an admin command prompt, run the command</span></span>

```
reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
net stop dra
```
3. <span data-ttu-id="532d2-153">Indítsa el a használatával a helyi cspsconfigtool.exe a.</span><span class="sxs-lookup"><span data-stu-id="532d2-153">Launch the cspsconfigtool.exe using the shortcut on your.</span></span>
4. <span data-ttu-id="532d2-154">Kattintson a **tároló regisztrációs** fülre.</span><span class="sxs-lookup"><span data-stu-id="532d2-154">Click the **Vault Registration** tab.</span></span>
5. <span data-ttu-id="532d2-155">Töltsön le új regisztrációs fájlt a portálról, és adja meg az eszköz számára.</span><span class="sxs-lookup"><span data-stu-id="532d2-155">Download a new Registration file from the portal and provide it as input to the tool.</span></span>

    ![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. <span data-ttu-id="532d2-157">Adja meg a proxykiszolgáló adatait, és kattintson a **regisztrálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="532d2-157">Provide the Proxy Server details and click the **Register** button.</span></span>  
7. <span data-ttu-id="532d2-158">Nyisson meg egy rendszergazda PowerShell parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="532d2-158">Open an Admin PowerShell command window.</span></span>
8. <span data-ttu-id="532d2-159">A következő parancsot</span><span class="sxs-lookup"><span data-stu-id="532d2-159">Run the following command</span></span>
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="decommissioning-a-configuration-server"></a><span data-ttu-id="532d2-160">Konfigurációs kiszolgáló leszerelése</span><span class="sxs-lookup"><span data-stu-id="532d2-160">Decommissioning a Configuration Server</span></span>
<span data-ttu-id="532d2-161">Ellenőrizze a konfigurációs kiszolgáló leszerelése megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="532d2-161">Ensure the following before you start decommissioning your Configuration Server.</span></span>
1. <span data-ttu-id="532d2-162">Tiltsa le a védelmet az összes virtuális gép ezen a konfigurációs kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="532d2-162">Disable protection for all virtual machines under this Configuration Server.</span></span>
2. <span data-ttu-id="532d2-163">Szüntesse meg az összes replikációs házirendek a konfigurációs kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="532d2-163">Disassociate all Replication policies from the Configuration Server.</span></span>
3. <span data-ttu-id="532d2-164">Törli a Vcenter kiszolgáló vagy vSphere minden gazdagép tartozó és a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="532d2-164">Delete all vCenters servers/vSphere hosts that are associated to the Configuration Server.</span></span>

### <a name="delete-the-configuration-server-from-azure-portal"></a><span data-ttu-id="532d2-165">Törölje a konfigurációs kiszolgáló Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="532d2-165">Delete the Configuration Server from Azure portal</span></span>
1. <span data-ttu-id="532d2-166">Azure-portálon keresse meg a **Site Recovery-infrastruktúra** > **konfigurációs kiszolgálók** a tároló menüből.</span><span class="sxs-lookup"><span data-stu-id="532d2-166">In Azure portal, browse to **Site Recovery Infrastructure** > **Configuration Servers** from the Vault menu.</span></span>
2. <span data-ttu-id="532d2-167">Kattintson a kiszolgáló leszerelése kívánt.</span><span class="sxs-lookup"><span data-stu-id="532d2-167">Click the Configuration Server that you want to decommission.</span></span>
3. <span data-ttu-id="532d2-168">A konfigurációs kiszolgáló adatai lapon kattintson a Törlés gomb.</span><span class="sxs-lookup"><span data-stu-id="532d2-168">On the Configuration Server's details page, click the Delete button.</span></span>

  ![törlés-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. <span data-ttu-id="532d2-170">Kattintson a **Igen** annak a kiszolgálónak a törlés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="532d2-170">Click **Yes** to confirm the deletion of the server.</span></span>

  >[!WARNING]
  <span data-ttu-id="532d2-171">Ha a virtuális gépek, replikációs házirendek vagy konfigurációs kiszolgálóhoz társított vCenter-kiszolgáló vagy vSphere gazdagépek, a kiszolgáló nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="532d2-171">If you have any virtual machines, Replication policies or vCenter servers/vSphere hosts associated with this Configuration Server, you cannot delete the server.</span></span> <span data-ttu-id="532d2-172">Törölje ezeket az entitásokat, mielőtt törli a tárolót.</span><span class="sxs-lookup"><span data-stu-id="532d2-172">Delete these entities before you try to delete the vault.</span></span>

### <a name="uninstall-the-configuration-server-software-and-its-dependencies"></a><span data-ttu-id="532d2-173">Távolítsa el a konfigurációs kiszolgáló szoftver és annak függőségeit</span><span class="sxs-lookup"><span data-stu-id="532d2-173">Uninstall the Configuration Server software and its dependencies</span></span>
  > [!TIP]
  <span data-ttu-id="532d2-174">Ha szeretne újra felhasználhatja a konfigurációs kiszolgáló Azure Site Recovery szolgáltatással, majd továbbléphet a 4. lépés közvetlenül</span><span class="sxs-lookup"><span data-stu-id="532d2-174">If you plan to reuse the Configuration Server with Azure Site Recovery again, then you can skip to step 4 directly</span></span>

1. <span data-ttu-id="532d2-175">Jelentkezzen be rendszergazdaként a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="532d2-175">Log on to the Configuration Server as an Administrator.</span></span>
2. <span data-ttu-id="532d2-176">Nyissa meg Vezérlőpult > Program > Programok eltávolítása</span><span class="sxs-lookup"><span data-stu-id="532d2-176">Open up Control Panel > Program > Uninstall Programs</span></span>
3. <span data-ttu-id="532d2-177">Távolítsa el a programokat, az alábbi sorrendben:</span><span class="sxs-lookup"><span data-stu-id="532d2-177">Uninstall the programs in the following sequence:</span></span>
  * <span data-ttu-id="532d2-178">Microsoft Azure Recovery Services Agent</span><span class="sxs-lookup"><span data-stu-id="532d2-178">Microsoft Azure Recovery Services Agent</span></span>
  * <span data-ttu-id="532d2-179">Microsoft Azure Site helyreállítási mobilitási szolgáltatás/fő célkiszolgáló megadásával</span><span class="sxs-lookup"><span data-stu-id="532d2-179">Microsoft Azure Site Recovery Mobility Service/Master Target server</span></span>
  * <span data-ttu-id="532d2-180">A Microsoft Azure Site Recovery Providert</span><span class="sxs-lookup"><span data-stu-id="532d2-180">Microsoft Azure Site Recovery Provider</span></span>
  * <span data-ttu-id="532d2-181">Microsoft Azure Recovery konfigurációs kiszolgáló/folyamat helykiszolgáló</span><span class="sxs-lookup"><span data-stu-id="532d2-181">Microsoft Azure Site Recovery Configuration Server/Process Server</span></span>
  * <span data-ttu-id="532d2-182">A Microsoft Azure Site helyreállítási konfigurációs kiszolgáló függőségek</span><span class="sxs-lookup"><span data-stu-id="532d2-182">Microsoft Azure Site Recovery Configuration Server Dependencies</span></span>
  * <span data-ttu-id="532d2-183">MySQL Server 5.5</span><span class="sxs-lookup"><span data-stu-id="532d2-183">MySQL Server 5.5</span></span>
4. <span data-ttu-id="532d2-184">Futtassa a következő parancsot a és a rendszergazdai parancssorba.</span><span class="sxs-lookup"><span data-stu-id="532d2-184">Run the following command from and admin command prompt.</span></span>
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a><span data-ttu-id="532d2-185">Konfigurációs kiszolgáló biztonságos szoftvercsatorna Layer(SSL) tanúsítványainak megújítása</span><span class="sxs-lookup"><span data-stu-id="532d2-185">Renew Configuration Server Secure Socket Layer(SSL) Certificates</span></span>
<span data-ttu-id="532d2-186">A kiszolgáló rendelkezik egy beépített webkiszolgálót, amely a mobilitási szolgáltatás, a folyamat kiszolgálók és a fő célkiszolgáló kiszolgálók, a konfigurációs kiszolgálóhoz kapcsolódó tevékenységeket koordinálja.</span><span class="sxs-lookup"><span data-stu-id="532d2-186">The Configuration Server has an inbuilt webserver, which orchestrates the activities of the Mobility Service, Process Servers, and Master Target servers connected to the Configuration Server.</span></span> <span data-ttu-id="532d2-187">A konfigurációs kiszolgáló webkiszolgáló SSL-tanúsítványt használ az ügyfelek hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="532d2-187">The Configuration Server's webserver uses an SSL certificate to authenticate its clients.</span></span> <span data-ttu-id="532d2-188">Ez a tanúsítvány rendelkezik egy három év lejárta, és az alábbi módszerrel bármikor újítható:</span><span class="sxs-lookup"><span data-stu-id="532d2-188">This certificate has an expiry of three years and can be renewed at any time using the following method:</span></span>

> [!WARNING]
<span data-ttu-id="532d2-189">Tanúsítványlejárat csak verziót lehetne végrehajtani 9.4.XXXX. X vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="532d2-189">Certificate expiry can be performed only on version 9.4.XXXX.X or higher.</span></span> <span data-ttu-id="532d2-190">Az Azure Site Recovery minden összetevőjét (konfigurációs kiszolgáló, a Folyamatkiszolgáló, fő célkiszolgáló, mobilitási szolgáltatás) frissítése a tanúsítványok megújítása munkafolyamat megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="532d2-190">Upgrade all the Azure Site Recovery components (Configuration Server, Process Server, Master Target Server, Mobility Service) before you start the Renew Certificates workflow.</span></span>

1. <span data-ttu-id="532d2-191">Az Azure-portálon keresse meg a tároló > Site Recovery-infrastruktúra > konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="532d2-191">On the Azure portal, browse to your Vault > Site Recovery Infrastructure > Configuration Server.</span></span>
2. <span data-ttu-id="532d2-192">Kattintson a konfigurációs kiszolgáló, amelyhez újítsa meg az SSL-tanúsítványt kell.</span><span class="sxs-lookup"><span data-stu-id="532d2-192">Click the Configuration Server for which you need to renew the SSL Certificate for.</span></span>
3. <span data-ttu-id="532d2-193">A konfigurációs kiszolgáló állapota tekintse meg a lejárat dátumát az SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="532d2-193">Under the Configuration Server health, you can see the expiry date for the SSL Certificate.</span></span>
4. <span data-ttu-id="532d2-194">A tanúsítvány megújításához kattintson a **tanúsítványok megújítása** művelet a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="532d2-194">Renew the certificate by clicking the **Renew Certificates** action as shown in the following image:</span></span>

  ![törlés-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a><span data-ttu-id="532d2-196">Secure Socket Layer tanúsítvány lejáratával kapcsolatos figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="532d2-196">Secure Socket Layer certificate expiry warning</span></span>

> [!NOTE]
<span data-ttu-id="532d2-197">Az összes telepítés előtt 2016. május történt az SSL-tanúsítvány érvényességi egy évre lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="532d2-197">The SSL Certificate's validity for all installations that happened before May 2016 was set to one year.</span></span> <span data-ttu-id="532d2-198">már elindította az Azure-portálon jelenik meg a tanúsítványok lejárati értesítéseinek jelent.</span><span class="sxs-lookup"><span data-stu-id="532d2-198">you have started seeing certificate expiry notifications showing up in the Azure portal.</span></span>

1. <span data-ttu-id="532d2-199">Ha a konfigurációs kiszolgáló SSL-tanúsítvány a következő két hónapon belül lejár, a szolgáltatás elindul, értesíti a felhasználókat az Azure-portálon & e-mailek (kell lehet feliratkozni az Azure Site Recovery értesítésekre).</span><span class="sxs-lookup"><span data-stu-id="532d2-199">If the Configuration Server's SSL certificate is going to expire in the next two months, the service starts notifying users via the Azure portal & email (you need to be subscribed to Azure Site Recovery notifications).</span></span> <span data-ttu-id="532d2-200">A tároló-erőforrás oldalon értesítésszalagról kezdenek el azt.</span><span class="sxs-lookup"><span data-stu-id="532d2-200">You start seeing a notification banner on the Vault's resource page.</span></span>

  ![tanúsítvány-értesítés](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. <span data-ttu-id="532d2-202">Kattintson a szalagcím, és kérjen további részleteket a tanúsítvány lejárta.</span><span class="sxs-lookup"><span data-stu-id="532d2-202">Click the banner to get additional details on the Certificate expiry.</span></span>

  ![tanúsítvány-részletek](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  <span data-ttu-id="532d2-204">Ahelyett, hogy ha egy **megújítása most** gomb megjelenik egy **frissítés most** gombra.</span><span class="sxs-lookup"><span data-stu-id="532d2-204">If instead of a **Renew Now** button you see an **Upgrade Now** button.</span></span> <span data-ttu-id="532d2-205">Ez azt jelenti, hogy vannak-e néhány összetevőt a környezetben, amelyek még nincsenek frissítve az 9.4.xxxx.x vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="532d2-205">This means that there are some components in your environment that have not yet been upgraded to 9.4.xxxx.x or higher versions.</span></span>

## <a name="sizing-requirements-for-a-configuration-server"></a><span data-ttu-id="532d2-206">A konfigurációs kiszolgáló méretezése követelményei</span><span class="sxs-lookup"><span data-stu-id="532d2-206">Sizing requirements for a Configuration Server</span></span>

| <span data-ttu-id="532d2-207">**PROCESSZOR**</span><span class="sxs-lookup"><span data-stu-id="532d2-207">**CPU**</span></span> | <span data-ttu-id="532d2-208">**Memória**</span><span class="sxs-lookup"><span data-stu-id="532d2-208">**Memory**</span></span> | <span data-ttu-id="532d2-209">**Gyorsítótár-lemez mérete**</span><span class="sxs-lookup"><span data-stu-id="532d2-209">**Cache disk size**</span></span> | <span data-ttu-id="532d2-210">**Adatváltozási sebesség**</span><span class="sxs-lookup"><span data-stu-id="532d2-210">**Data change rate**</span></span> | <span data-ttu-id="532d2-211">**Védett gépek**</span><span class="sxs-lookup"><span data-stu-id="532d2-211">**Protected machines**</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="532d2-212">8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag)</span><span class="sxs-lookup"><span data-stu-id="532d2-212">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="532d2-213">16 GB</span><span class="sxs-lookup"><span data-stu-id="532d2-213">16 GB</span></span> |<span data-ttu-id="532d2-214">300 GB</span><span class="sxs-lookup"><span data-stu-id="532d2-214">300 GB</span></span> |<span data-ttu-id="532d2-215">500 GB vagy kevesebb</span><span class="sxs-lookup"><span data-stu-id="532d2-215">500 GB or less</span></span> |<span data-ttu-id="532d2-216">100-nál kevesebb gépek replikálása.</span><span class="sxs-lookup"><span data-stu-id="532d2-216">Replicate fewer than 100 machines.</span></span> |
| <span data-ttu-id="532d2-217">12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag)</span><span class="sxs-lookup"><span data-stu-id="532d2-217">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="532d2-218">18 GB</span><span class="sxs-lookup"><span data-stu-id="532d2-218">18 GB</span></span> |<span data-ttu-id="532d2-219">600 GB</span><span class="sxs-lookup"><span data-stu-id="532d2-219">600 GB</span></span> |<span data-ttu-id="532d2-220">1 TB 500 GB</span><span class="sxs-lookup"><span data-stu-id="532d2-220">500 GB to 1 TB</span></span> |<span data-ttu-id="532d2-221">100-150 gépek közti replikálásához.</span><span class="sxs-lookup"><span data-stu-id="532d2-221">Replicate between 100-150 machines.</span></span> |
| <span data-ttu-id="532d2-222">16 Vcpu (2 sockets * @ 2,5 GHz-es 8 mag)</span><span class="sxs-lookup"><span data-stu-id="532d2-222">16 vCPUs (2 sockets * 8 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="532d2-223">32 GB</span><span class="sxs-lookup"><span data-stu-id="532d2-223">32 GB</span></span> |<span data-ttu-id="532d2-224">1 TB</span><span class="sxs-lookup"><span data-stu-id="532d2-224">1 TB</span></span> |<span data-ttu-id="532d2-225">1 TB-os és 2 TB</span><span class="sxs-lookup"><span data-stu-id="532d2-225">1 TB to 2 TB</span></span> |<span data-ttu-id="532d2-226">150-200 gépek közti replikálásához.</span><span class="sxs-lookup"><span data-stu-id="532d2-226">Replicate between 150-200 machines.</span></span> |

  >[!TIP]
  <span data-ttu-id="532d2-227">Ha a napi adatforgalommal meghaladja a 2 TB, vagy több mint 200 olyan virtuális gépet replikálni tervezi, javasoljuk, hogy a replikációs forgalom további folyamat-kiszolgálót telepíthet.</span><span class="sxs-lookup"><span data-stu-id="532d2-227">If your daily data churn exceeds 2 TB, or you plan to replicate more than 200 virtual machines, it is recommended to deploy additional process servers to load balance the replication traffic.</span></span> <span data-ttu-id="532d2-228">Ismerje meg, további kibővített folyamat telepítésével kapcsolatos kiszolgálója.</span><span class="sxs-lookup"><span data-stu-id="532d2-228">Learn more about How to deploy Scale-out Process severs.</span></span>


## <a name="common-issues"></a><span data-ttu-id="532d2-229">Gyakori problémák</span><span class="sxs-lookup"><span data-stu-id="532d2-229">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
