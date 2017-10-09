---
title: " Az Azure Site Recovery kibővített folyamat kiszolgáló kezelése |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset és egy kibővített Folyamatkiszolgáló az Azure Site Recovery kezelése."
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
ms.openlocfilehash: 3d72f9c2c7014a4ff2fa2af168aa55ad1452eae5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-scale-out-process-server"></a><span data-ttu-id="36f2a-103">Kibővített folyamat kiszolgáló kezelése</span><span class="sxs-lookup"><span data-stu-id="36f2a-103">Manage a Scale-out Process Server</span></span>

<span data-ttu-id="36f2a-104">Kibővített folyamat kiszolgáló hello Site Recovery services és a helyszíni infrastruktúra közötti adatátvitel koordinátora működik.</span><span class="sxs-lookup"><span data-stu-id="36f2a-104">Scale-out Process Server acts as a coordinator for data transfer between hello Site Recovery services and your on-premises infrastructure.</span></span> <span data-ttu-id="36f2a-105">Ez a cikk ismerteti, hogyan állítsa be, konfigurálása és kibővített folyamat kiszolgáló kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="36f2a-105">This article describes how you can set up, configure, and manage a Scale-out Process Server.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36f2a-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="36f2a-106">Prerequisites</span></span>
<span data-ttu-id="36f2a-107">hello az alábbiakban hello hardveres, szoftveres és hálózati konfiguráció szükséges tooset kibővített folyamat kiszolgáló ajánlott.</span><span class="sxs-lookup"><span data-stu-id="36f2a-107">hello following are hello recommended hardware, software, and network configuration required tooset up a Scale-out Process Server.</span></span>

> [!NOTE]
> <span data-ttu-id="36f2a-108">[Kapacitástervezés](site-recovery-capacity-planner.md) van egy fontos lépés tooensure központi telepítését hello konfigurálása kibővített Folyamatkiszolgáló, hogy a csomagok a terhelési követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="36f2a-108">[Capacity planning](site-recovery-capacity-planner.md) is an important step tooensure that you deploy hello Scale-out Process Server with a configuration that suites your load requirements.</span></span> <span data-ttu-id="36f2a-109">Tudjon meg többet az [jellemzők skálázás kibővített folyamat kiszolgáló](#sizing-requirements-for-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="36f2a-109">Read more about [Scaling characteristics for a Scale-out Process Server](#sizing-requirements-for-a-configuration-server).</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-scale-out-process-server-software"></a><span data-ttu-id="36f2a-110">Hello kibővített Folyamatkiszolgáló szoftver letöltése</span><span class="sxs-lookup"><span data-stu-id="36f2a-110">Downloading hello Scale-out Process Server software</span></span>
1. <span data-ttu-id="36f2a-111">Jelentkezzen be toohello az Azure portálon, és keresse meg tooyour Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="36f2a-111">Log on toohello Azure portal and browse tooyour Recovery Services Vault.</span></span>
2. <span data-ttu-id="36f2a-112">Keresse meg a túl**Site Recovery-infrastruktúra** > **konfigurációs kiszolgálók** (alatt a VMware és fizikai gépek).</span><span class="sxs-lookup"><span data-stu-id="36f2a-112">Browse too**Site Recovery Infrastructure** > **Configuration Servers** (under For VMware & Physical Machines).</span></span>
3. <span data-ttu-id="36f2a-113">Válassza ki a konfigurációs kiszolgáló toodrill le hello konfigurációs kiszolgáló részletei lapon.</span><span class="sxs-lookup"><span data-stu-id="36f2a-113">Select your configuration server toodrill down into hello configuration server's details page.</span></span>
4. <span data-ttu-id="36f2a-114">Kattintson a hello **+ Folyamatkiszolgáló** gombra.</span><span class="sxs-lookup"><span data-stu-id="36f2a-114">Click hello **+ Process Server** button.</span></span>
5. <span data-ttu-id="36f2a-115">A hello **adja hozzá a folyamatkiszolgáló** lapon jelölje be **helyszíni telepítés kibővített Folyamatkiszolgáló** hello kapcsolót **válassza ki a kívánt toodeploy a folyamatkiszolgáló** legördülő lista.</span><span class="sxs-lookup"><span data-stu-id="36f2a-115">In hello **Add Process server** page, select **Deploy a Scale-out Process Server on-premises** option from hello **Choose where you want toodeploy your process server** drop-down.</span></span>

  ![Kiszolgálók lap hozzáadása](./media/site-recovery-vmware-to-azure-manage-scaleout-process-server/add-process-server.png)
6. <span data-ttu-id="36f2a-117">Kattintson a hello **letöltési hello Microsoft Azure Site Recovery az egységes telepítő** hivatkozás toodownload hello legújabb verziójának hello kibővített Folyamatkiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="36f2a-117">Click hello **Download hello Microsoft Azure Site Recovery Unified Setup** link toodownload hello latest version of hello Scale-out Process Server installation.</span></span>

  > [!WARNING]
  <span data-ttu-id="36f2a-118">hello a kibővített Folyamatkiszolgáló verziójának meg kell egyeznie a környezetben futó hello konfigurációs kiszolgáló verziónál régebbi tooor.</span><span class="sxs-lookup"><span data-stu-id="36f2a-118">hello version of your Scale-out Process Server should be equal tooor lesser than hello Configuration Server version running in your environment.</span></span> <span data-ttu-id="36f2a-119">Egy egyszerű módon tooensure verziókompatibilitás toouse hello azonos telepítő bit, hogy nemrég használt tooinstall/frissítés a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="36f2a-119">A simple way tooensure version compatibility is toouse hello same installer bits that you recently used tooinstall/update your Configuration Server.</span></span>

## <a name="installing-and-registering-a-scale-out-process-server-from-gui"></a><span data-ttu-id="36f2a-120">Telepítése és regisztrálása a kibővített folyamat kiszolgáló grafikus felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="36f2a-120">Installing and registering a Scale-out Process Server from GUI</span></span>
<span data-ttu-id="36f2a-121">Ha ki a központi telepítés meghaladja a 200 forrásgépek tooscale rendelkezik, vagy egy teljes napi kavarog egyidejűleg több, mint 2 TB-os aránya, akkor további folyamat kiszolgálók toohandle hello adatforgalma.</span><span class="sxs-lookup"><span data-stu-id="36f2a-121">If you have tooscale out your deployment beyond 200 source machines, or a total daily churn rate of more than 2 TB, you need additional process servers toohandle hello traffic volume.</span></span>

<span data-ttu-id="36f2a-122">Ellenőrizze a hello [vonatkozó javaslatok kiszolgálókhoz folyamat méretezés](#size-recommendations-for-the-process-server), és hajtsa végre az ezen utasításokat tooset hello folyamat kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="36f2a-122">Check hello [size recommendations for process servers](#size-recommendations-for-the-process-server), and then follow these instructions tooset up hello process server.</span></span> <span data-ttu-id="36f2a-123">Hello kiszolgáló telepítése után telepít át forrás gépek toouse azt.</span><span class="sxs-lookup"><span data-stu-id="36f2a-123">After setting up hello server, you migrate source machines toouse it.</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="installing-and-registering-a-scale-out-process-server-using-command-line"></a><span data-ttu-id="36f2a-124">Telepítése és regisztrálása a kibővített Folyamatkiszolgáló parancssori használata</span><span class="sxs-lookup"><span data-stu-id="36f2a-124">Installing and registering a Scale-out Process Server using command-line</span></span>

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
```

### <a name="sample-usage"></a><span data-ttu-id="36f2a-125">Példa</span><span class="sxs-lookup"><span data-stu-id="36f2a-125">Sample usage</span></span>
```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```

### <a name="scale-out-process-server-installer-command-line-arguments"></a><span data-ttu-id="36f2a-126">Kibővített Folyamatkiszolgáló telepítő parancssori argumentumokat.</span><span class="sxs-lookup"><span data-stu-id="36f2a-126">Scale-out Process Server installer command-line arguments.</span></span>
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-proxy-settings-configuration-file"></a><span data-ttu-id="36f2a-127">Proxykiszolgáló-beállítások konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="36f2a-127">Create a proxy settings configuration file</span></span>
<span data-ttu-id="36f2a-128">ProxySettingsFilePath paraméter egy fájl fogadja bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="36f2a-128">ProxySettingsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="36f2a-129">Használja a következő formátumú, és adja át bemeneti ProxySettingsFilePath paraméterként hello fájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="36f2a-129">Create file using hello following format and pass it as input ProxySettingsFilePath parameter.</span></span>
```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-scale-out-process-server"></a><span data-ttu-id="36f2a-130">Kibővített Folyamatkiszolgáló proxy beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="36f2a-130">Modifying proxy settings for Scale-out Process Server</span></span>
1. <span data-ttu-id="36f2a-131">Jelentkezzen be a kibővített Folyamatkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="36f2a-131">Login  into your Scale-out Process Server.</span></span>
2. <span data-ttu-id="36f2a-132">Nyisson meg egy rendszergazda PowerShell parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="36f2a-132">Open an Admin PowerShell command window.</span></span>
3. <span data-ttu-id="36f2a-133">Futtassa a következő parancs hello</span><span class="sxs-lookup"><span data-stu-id="36f2a-133">Run hello following command</span></span>
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
4. <span data-ttu-id="36f2a-134">Ezután a toohello könyvtár tallózása **%PROGRAMDATA%\ASR\Agent** és a következő parancs futtatása hello</span><span class="sxs-lookup"><span data-stu-id="36f2a-134">Next browse toohello directory **%PROGRAMDATA%\ASR\Agent** and run hello following command</span></span>
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```

## <a name="re-registering-a-scale-out-process-server"></a><span data-ttu-id="36f2a-135">A kibővített folyamat-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="36f2a-135">Re-registering a Scale-out Process Server</span></span>
[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

* <span data-ttu-id="36f2a-136">Ezután nyisson meg egy rendszergazdai parancssort.</span><span class="sxs-lookup"><span data-stu-id="36f2a-136">Next open an Admin command prompt.</span></span>
* <span data-ttu-id="36f2a-137">Toohello könyvtár tallózása **%PROGRAMDATA%\ASR\Agent** és hello parancs</span><span class="sxs-lookup"><span data-stu-id="36f2a-137">Browse toohello directory **%PROGRAMDATA%\ASR\Agent** and run hello command</span></span>

```
cdpcli.exe --registermt

net stop obengine

net start obengine
```

## <a name="upgrading-a-scale-out-process-server"></a><span data-ttu-id="36f2a-138">A kibővített folyamat-kiszolgáló frissítése</span><span class="sxs-lookup"><span data-stu-id="36f2a-138">Upgrading a Scale-out Process Server</span></span>
[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

## <a name="decommissioning-a-scale-out-process-server"></a><span data-ttu-id="36f2a-139">Kibővített folyamat kiszolgáló leszerelése</span><span class="sxs-lookup"><span data-stu-id="36f2a-139">Decommissioning a Scale-out Process Server</span></span>
1. <span data-ttu-id="36f2a-140">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="36f2a-140">Ensure that:</span></span>
  - <span data-ttu-id="36f2a-141">Konfigurációs kiszolgáló kapcsolat állapota jeleníti meg, mint a **csatlakoztatva** a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="36f2a-141">Configuration Server's connection state shows as **Connected** in hello Azure portal</span></span>
  - <span data-ttu-id="36f2a-142">Folyamat kiszolgáló ennek ellenére toocommunicate hello konfigurációs kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="36f2a-142">Process Server's is still able toocommunicate with hello Configuration server.</span></span>
2. <span data-ttu-id="36f2a-143">Jelentkezzen be a folyamatkiszolgáló toohello rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="36f2a-143">Log in toohello process server as an administrator</span></span>
3. <span data-ttu-id="36f2a-144">Nyissa meg Vezérlőpult > Program > Programok eltávolítása</span><span class="sxs-lookup"><span data-stu-id="36f2a-144">Open up Control Panel > Program > Uninstall Programs</span></span>
4. <span data-ttu-id="36f2a-145">Távolítsa el a hello programokat hello sorrendben megadva a következő:</span><span class="sxs-lookup"><span data-stu-id="36f2a-145">Uninstall hello programs in hello sequence given following:</span></span>
  * <span data-ttu-id="36f2a-146">Microsoft Azure Recovery konfigurációs kiszolgáló/folyamat helykiszolgáló</span><span class="sxs-lookup"><span data-stu-id="36f2a-146">Microsoft Azure Site Recovery Configuration Server/Process Server</span></span>
  * <span data-ttu-id="36f2a-147">A Microsoft Azure Site helyreállítási konfigurációs kiszolgáló függőségek</span><span class="sxs-lookup"><span data-stu-id="36f2a-147">Microsoft Azure Site Recovery Configuration Server Dependencies</span></span>
  * <span data-ttu-id="36f2a-148">Microsoft Azure Recovery Services Agent</span><span class="sxs-lookup"><span data-stu-id="36f2a-148">Microsoft Azure Recovery Services Agent</span></span>

<span data-ttu-id="36f2a-149">Hello Folyamatkiszolgáló törlési tooreflect felfelé-too15 percig hello Azure-portálon a is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="36f2a-149">It can take up-too15 minutes for hello Process Server deletion tooreflect in hello Azure portal.</span></span>

  > [!NOTE]
  <span data-ttu-id="36f2a-150">Ha hello folyamatkiszolgáló-e a konfigurációs kiszolgáló hello nem toocommunicate (a portál kapcsolati állapota Disconnected), akkor szükséges, hogy toofollow hello következő lépések toopurge azt a konfigurációs kiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="36f2a-150">If hello Process server is unable toocommunicate with hello Configuration Server (Connection State in portal is Disconnected), then you need toofollow hello following steps toopurge it from hello Configuration Server.</span></span>

## <a name="unregistering-a-disconnected-scale-out-process-server-from-a-configuration-server"></a><span data-ttu-id="36f2a-151">A konfigurációs kiszolgálóról a leválasztott kibővített folyamat-kiszolgáló regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="36f2a-151">Unregistering a disconnected Scale-out Process server from a Configuration Server</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="sizing-requirements-for-a-scale-out-process-server"></a><span data-ttu-id="36f2a-152">A kibővített Folyamatkiszolgáló méretezési követelményei</span><span class="sxs-lookup"><span data-stu-id="36f2a-152">Sizing requirements for a Scale-out Process Server</span></span>

| <span data-ttu-id="36f2a-153">**További folyamatkiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="36f2a-153">**Additional process server**</span></span> | <span data-ttu-id="36f2a-154">**Gyorsítótár-lemez mérete**</span><span class="sxs-lookup"><span data-stu-id="36f2a-154">**Cache disk size**</span></span> | <span data-ttu-id="36f2a-155">**Adatváltozási sebesség**</span><span class="sxs-lookup"><span data-stu-id="36f2a-155">**Data change rate**</span></span> | <span data-ttu-id="36f2a-156">**Védett gépek**</span><span class="sxs-lookup"><span data-stu-id="36f2a-156">**Protected machines**</span></span> |
| --- | --- | --- | --- |
|<span data-ttu-id="36f2a-157">4 Vcpu (2 sockets * @ 2,5 GHz-es 2 mag), 8 GB-os memória</span><span class="sxs-lookup"><span data-stu-id="36f2a-157">4 vCPUs (2 sockets * 2 cores @ 2.5 GHz), 8-GB memory</span></span> |<span data-ttu-id="36f2a-158">300 GB</span><span class="sxs-lookup"><span data-stu-id="36f2a-158">300 GB</span></span> |<span data-ttu-id="36f2a-159">250 GB vagy kevesebb</span><span class="sxs-lookup"><span data-stu-id="36f2a-159">250 GB or less</span></span> |<span data-ttu-id="36f2a-160">Kisebb vagy 85 gépek replikálása.</span><span class="sxs-lookup"><span data-stu-id="36f2a-160">Replicate 85 or less machines.</span></span> |
|<span data-ttu-id="36f2a-161">8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 12-GB memória</span><span class="sxs-lookup"><span data-stu-id="36f2a-161">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 12-GB memory</span></span> |<span data-ttu-id="36f2a-162">600 GB</span><span class="sxs-lookup"><span data-stu-id="36f2a-162">600 GB</span></span> |<span data-ttu-id="36f2a-163">250 GB too1 TB</span><span class="sxs-lookup"><span data-stu-id="36f2a-163">250 GB too1 TB</span></span> |<span data-ttu-id="36f2a-164">Replikálja a 85-150 gépek között.</span><span class="sxs-lookup"><span data-stu-id="36f2a-164">Replicate between 85-150 machines.</span></span> |
|<span data-ttu-id="36f2a-165">12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag) 24-GB memória</span><span class="sxs-lookup"><span data-stu-id="36f2a-165">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz) 24-GB memory</span></span> |<span data-ttu-id="36f2a-166">1 TB</span><span class="sxs-lookup"><span data-stu-id="36f2a-166">1 TB</span></span> |<span data-ttu-id="36f2a-167">1 TB-os too2 TB</span><span class="sxs-lookup"><span data-stu-id="36f2a-167">1 TB too2 TB</span></span> |<span data-ttu-id="36f2a-168">150-225 gépek közti replikálásához.</span><span class="sxs-lookup"><span data-stu-id="36f2a-168">Replicate between 150-225 machines.</span></span> |
