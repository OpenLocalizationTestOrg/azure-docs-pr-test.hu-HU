---
title: "Linux virtuális Gépet egy Virtuálisgép-bővítménnyel aaaMonitoring |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Linux diagnosztikai bővítmény toomonitor hello teljesítmény- és a Linux virtuális gépek az Azure diagnosztikai adatokat."
services: virtual-machines-linux
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f54a11c5-5a0e-40ff-af6c-e60bd464058b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: Ning
ms.openlocfilehash: cf7bfebca8c0367941f7a975417f60fe2e2dab25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-linux-diagnostic-extension-toomonitor-hello-performance-and-diagnostic-data-of-a-linux-vm"></a><span data-ttu-id="e57b7-103">Hello Linux diagnosztikai bővítmény toomonitor hello teljesítmény- és a Linux virtuális gépek diagnosztikai adatok használata</span><span class="sxs-lookup"><span data-stu-id="e57b7-103">Use hello Linux Diagnostic Extension toomonitor hello performance and diagnostic data of a Linux VM</span></span>

<span data-ttu-id="e57b7-104">Ez a dokumentum ismerteti a hello Linux diagnosztikai bővítmény 2.3 verzióját.</span><span class="sxs-lookup"><span data-stu-id="e57b7-104">This document describes version 2.3 of hello Linux Diagnostic Extension.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e57b7-105">Jelen verziója elavult, és lehet, hogy közzé nem tett 2018. június 30 követően.</span><span class="sxs-lookup"><span data-stu-id="e57b7-105">This version is deprecated, and it may be unpublished any time after June 30, 2018.</span></span> <span data-ttu-id="e57b7-106">Helyette 3.0-s verziója.</span><span class="sxs-lookup"><span data-stu-id="e57b7-106">It has been replaced by version 3.0.</span></span> <span data-ttu-id="e57b7-107">További információkért lásd: hello [hello Linux diagnosztikai bővítmény 3.0-s verziója című dokumentációban](../diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="e57b7-107">For more information, see hello [documentation for version 3.0 of hello Linux Diagnostic Extension](../diagnostic-extension.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="e57b7-108">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="e57b7-108">Introduction</span></span>

<span data-ttu-id="e57b7-109">(**Megjegyzés**: hello Linux diagnosztikai bővítmény nem nyílt forrása [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) , ahol az első közzététel a hello hello bővítmény legújabb információkat.</span><span class="sxs-lookup"><span data-stu-id="e57b7-109">(**Note**: hello Linux Diagnostic Extension is open-sourced on [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) where hello most current information on hello extension is first published.</span></span> <span data-ttu-id="e57b7-110">Érdemes lehet toocheck hello [GitHub-oldalon](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) első.)</span><span class="sxs-lookup"><span data-stu-id="e57b7-110">You might want toocheck hello [GitHub page](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) first.)</span></span>

<span data-ttu-id="e57b7-111">Linux diagnosztikai bővítmény hello segítségével egy felhasználó figyelő hello a Microsoft Azure futó Linux virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e57b7-111">hello Linux Diagnostic Extension helps a user monitor hello Linux VMs that are running on Microsoft Azure.</span></span> <span data-ttu-id="e57b7-112">Hello a következő képességekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e57b7-112">It has hello following capabilities:</span></span>

* <span data-ttu-id="e57b7-113">Gyűjti, és feltölti a rendszer teljesítményadatok hello hello Linux virtuális gép toohello felhasználói tárolási táblából, ideértve a diagnosztikai és syslog adatokat.</span><span class="sxs-lookup"><span data-stu-id="e57b7-113">Collects and uploads hello system performance information from hello Linux VM toohello user's storage table, including diagnostic and syslog information.</span></span>
* <span data-ttu-id="e57b7-114">Lehetővé teszi, hogy a felhasználók toocustomize hello adatok metrikák összegyűjtésének és feltöltve.</span><span class="sxs-lookup"><span data-stu-id="e57b7-114">Enables users toocustomize hello data metrics that will be collected and uploaded.</span></span>
* <span data-ttu-id="e57b7-115">Lehetővé teszi, hogy a felhasználók tooupload megadott napló fájlok tooa kijelölt tároló tábla.</span><span class="sxs-lookup"><span data-stu-id="e57b7-115">Enables users tooupload specified log files tooa designated storage table.</span></span>

<span data-ttu-id="e57b7-116">Hello aktuális verziójában 2.3 hello adatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="e57b7-116">In hello current version 2.3, hello data includes:</span></span>

* <span data-ttu-id="e57b7-117">Az összes Linux Rsyslog naplókat, beleértve a rendszer, a biztonsági és az alkalmazás naplóiban.</span><span class="sxs-lookup"><span data-stu-id="e57b7-117">All Linux Rsyslog logs, including system, security, and application logs.</span></span>
* <span data-ttu-id="e57b7-118">Összes rendszer adat, amely meg van adva [hello System Center Cross Platform megoldások hely](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="e57b7-118">All system data that's specified on [hello System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
* <span data-ttu-id="e57b7-119">Felhasználó által meghatározott naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="e57b7-119">User-specified log files.</span></span>

<span data-ttu-id="e57b7-120">A bővítmény hello klasszikus és Resource Manager üzembe helyezési modellel működik.</span><span class="sxs-lookup"><span data-stu-id="e57b7-120">This extension works with both hello classic and Resource Manager deployment models.</span></span>

### <a name="current-version-of-hello-extension-and-deprecation-of-old-versions"></a><span data-ttu-id="e57b7-121">Hello bővítményt, és a régi változatainak elavulása jelenlegi verziója</span><span class="sxs-lookup"><span data-stu-id="e57b7-121">Current version of hello extension and deprecation of old versions</span></span>

<span data-ttu-id="e57b7-122">hello hello bővítmény legújabb verziója van **2.3**, és **a régi verziókat (2.0, 2.1-es és 2.2) a rendszer elavult, majd közzé nem tett az év (2017) végéig**.</span><span class="sxs-lookup"><span data-stu-id="e57b7-122">hello latest version of hello extension is **2.3**, and **any old versions (2.0, 2.1, and 2.2) will be deprecated and unpublished by end of this year (2017)**.</span></span> <span data-ttu-id="e57b7-123">Ha telepítette a hello Linux diagnosztikai bővítmény automatikus alverzió frissítés le van tiltva, erősen ajánlott hello-kiterjesztés eltávolítása, és engedélyezve van az automatikus alverzió frissítést újra kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="e57b7-123">If you installed hello Linux Diagnostic extension with automatic minor version upgrade disabled, it's strongly recommended that you uninstall hello extension and reinstall it with automatic minor version upgrade enabled.</span></span> <span data-ttu-id="e57b7-124">A klasszikus (ASM) virtuális gépeken érhet el ez Azure XPLAT parancssori felületen vagy a Powershellen keresztül hello bővítmény telepítésekor "2.*" megadásával hello verzióként.</span><span class="sxs-lookup"><span data-stu-id="e57b7-124">On classic (ASM) VMs, you can achieve this by specifying '2.*' as hello version if you are installing hello extension through Azure XPLAT CLI or Powershell.</span></span> <span data-ttu-id="e57b7-125">ARM gépeken lehet ezt elérni-ot ""autoUpgradeMinorVersion": true" hello VM központi telepítési sablon.</span><span class="sxs-lookup"><span data-stu-id="e57b7-125">On ARM VMs, you can achieve this by including '"autoUpgradeMinorVersion": true' in hello VM deployment template.</span></span> <span data-ttu-id="e57b7-126">Emellett bármely új hello-bővítmény telepítése hello automatikus alverzió beállítás engedélyezve van a frissítés kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="e57b7-126">Also, any new installation of hello extension should have hello auto minor version upgrade option turned on.</span></span>

## <a name="enable-hello-extension"></a><span data-ttu-id="e57b7-127">Hello bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e57b7-127">Enable hello extension</span></span>

<span data-ttu-id="e57b7-128">A bővítmény hello használatával engedélyezheti [Azure-portálon](https://portal.azure.com/#), az Azure PowerShell vagy Azure CLI parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="e57b7-128">You can enable this extension by using hello [Azure portal](https://portal.azure.com/#), Azure PowerShell, or Azure CLI scripts.</span></span>

<span data-ttu-id="e57b7-129">tooview és hello rendszer konfigurálása, valamint teljesítményadatokat közvetlenül a hello Azure-portálon, hajtsa végre a [ezeket a lépéseket a hello Azure blog](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span><span class="sxs-lookup"><span data-stu-id="e57b7-129">tooview and configure hello system and performance data directly from hello Azure portal, follow [these steps on hello Azure blog](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span></span>

<span data-ttu-id="e57b7-130">Ez a cikk foglalkozik, hogyan tooenable és hello-kiterjesztés konfigurálása az Azure parancssori felület parancsait használva.</span><span class="sxs-lookup"><span data-stu-id="e57b7-130">This article focuses on how tooenable and configure hello extension by using Azure CLI commands.</span></span> <span data-ttu-id="e57b7-131">Ez lehetővé teszi tooread és nézet hello közvetlenül hello tárolási tábla adatait.</span><span class="sxs-lookup"><span data-stu-id="e57b7-131">This allows you tooread and view hello data directly from hello storage table.</span></span>

<span data-ttu-id="e57b7-132">Vegye figyelembe, hogy hello konfigurációs itt ismertetett módszerek hello Azure-portálon az nem fog működni.</span><span class="sxs-lookup"><span data-stu-id="e57b7-132">Note that hello configuration methods that are described here won't work for hello Azure portal.</span></span> <span data-ttu-id="e57b7-133">tooview és hello rendszer és a teljesítmény adatok közvetlenül az Azure-portálon hello konfigurálása, hello bővítmény hello portálon keresztül engedélyezve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e57b7-133">tooview and configure hello system and performance data directly from hello Azure portal, hello extension must be enabled through hello portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e57b7-134">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e57b7-134">Prerequisites</span></span>

* <span data-ttu-id="e57b7-135">**Az Azure Linux ügynök 2.0.6 verzió vagy újabb**.</span><span class="sxs-lookup"><span data-stu-id="e57b7-135">**Azure Linux Agent version 2.0.6 or later**.</span></span>

  <span data-ttu-id="e57b7-136">Vegye figyelembe, hogy a legtöbb Azure virtuális gép Linux gyűjtemény lemezképei tartalmazzák 2.0.6 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="e57b7-136">Note that most Azure VM Linux gallery images include version 2.0.6 or later.</span></span> <span data-ttu-id="e57b7-137">Futtathat **WAAgent-verzió** tooconfirm hello VM melyik verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="e57b7-137">You can run **WAAgent -version** tooconfirm which version is installed on hello VM.</span></span> <span data-ttu-id="e57b7-138">Ha a virtuális gép hello 2.0.6 korábbi verziót futtat, kövesse [ezeket az utasításokat a Githubon](https://github.com/Azure/WALinuxAgent "utasításokat") tooupdate azt.</span><span class="sxs-lookup"><span data-stu-id="e57b7-138">If hello VM is running a version that's earlier than 2.0.6, you can follow [these instructions on GitHub](https://github.com/Azure/WALinuxAgent "instructions") tooupdate it.</span></span>
* <span data-ttu-id="e57b7-139">**Azure parancssori felület (CLI)**.</span><span class="sxs-lookup"><span data-stu-id="e57b7-139">**Azure CLI**.</span></span> <span data-ttu-id="e57b7-140">Hajtsa végre a [Ez az útmutató a parancssori felület telepítése](../../../cli-install-nodejs.md) tooset hello Azure CLI környezetet a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e57b7-140">Follow [this guidance for installing CLI](../../../cli-install-nodejs.md) tooset up hello Azure CLI environment on your machine.</span></span> <span data-ttu-id="e57b7-141">Az Azure parancssori felület telepítése után is használhatja hello **azure** parancsot a parancssori felület (a Bash, a Terminálszolgáltatások vagy a parancssorból) tooaccess hello Azure parancssori felület parancsait.</span><span class="sxs-lookup"><span data-stu-id="e57b7-141">After Azure CLI is installed, you can use hello **azure** command from your command-line interface (Bash, Terminal, or command prompt) tooaccess hello Azure CLI commands.</span></span> <span data-ttu-id="e57b7-142">Példa:</span><span class="sxs-lookup"><span data-stu-id="e57b7-142">For example:</span></span>

  * <span data-ttu-id="e57b7-143">Futtatás **azure virtuálisgép-bővítmény készlet – súgó** vonatkozó részletes információk.</span><span class="sxs-lookup"><span data-stu-id="e57b7-143">Run **azure vm extension set --help** for detailed help information.</span></span>
  * <span data-ttu-id="e57b7-144">Futtatás **azure bejelentkezési** a tooAzure toosign.</span><span class="sxs-lookup"><span data-stu-id="e57b7-144">Run **azure login** toosign in tooAzure.</span></span>
  * <span data-ttu-id="e57b7-145">Futtatás **azure virtuális gép lista** toolist összes hello Azure rendelkező virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e57b7-145">Run **azure vm list** toolist all hello virtual machines that you have on Azure.</span></span>
* <span data-ttu-id="e57b7-146">A tárolási fiók toostore hello adatait.</span><span class="sxs-lookup"><span data-stu-id="e57b7-146">A storage account toostore hello data.</span></span> <span data-ttu-id="e57b7-147">Szüksége lesz egy korábban létrehozott tárfiók neve és egy hozzáférési kulcs tooupload hello tooyour-tároló.</span><span class="sxs-lookup"><span data-stu-id="e57b7-147">You will need a storage account name that was created previously and an access key tooupload hello data tooyour storage.</span></span>

## <a name="use-hello-azure-cli-command-tooenable-hello-linux-diagnostic-extension"></a><span data-ttu-id="e57b7-148">Hello Azure CLI parancs tooenable hello Linux diagnosztikai bővítmény használatára</span><span class="sxs-lookup"><span data-stu-id="e57b7-148">Use hello Azure CLI command tooenable hello Linux Diagnostic Extension</span></span>

### <a name="scenario-1-enable-hello-extension-with-hello-default-data-set"></a><span data-ttu-id="e57b7-149">1. forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="e57b7-149">Scenario 1.</span></span> <span data-ttu-id="e57b7-150">Hello alapértelmezett adatkészlet hello bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e57b7-150">Enable hello extension with hello default data set</span></span>

<span data-ttu-id="e57b7-151">2.3-as vagy újabb verziója, a gyűjtendő hello alapértelmezett adatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="e57b7-151">In version 2.3 or later, hello default data that will be collected includes:</span></span>

* <span data-ttu-id="e57b7-152">Minden Rsyslog információk (beleértve a rendszer, a biztonsági és az alkalmazásnaplók).</span><span class="sxs-lookup"><span data-stu-id="e57b7-152">All Rsyslog information (including system, security, and application logs).</span></span>  
* <span data-ttu-id="e57b7-153">Egy sor alapján a rendszer adatai.</span><span class="sxs-lookup"><span data-stu-id="e57b7-153">A core set of basis system data.</span></span> <span data-ttu-id="e57b7-154">Vegye figyelembe, hogy a teljes adatkészlet hello leírása a hello [System Center Cross Platform megoldások hely](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="e57b7-154">Note that hello full data set is described on hello [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
  <span data-ttu-id="e57b7-155">Ha azt szeretné, hogy tooenable további adatokat, folytassa a hello lépésekkel forgatókönyvek 2 és 3.</span><span class="sxs-lookup"><span data-stu-id="e57b7-155">If you want tooenable extra data, continue with hello steps in Scenarios 2 and 3.</span></span>

<span data-ttu-id="e57b7-156">1. lépés</span><span class="sxs-lookup"><span data-stu-id="e57b7-156">Step 1.</span></span> <span data-ttu-id="e57b7-157">A következő hello PrivateConfig.json nevű fájl létrehozása tartalom:</span><span class="sxs-lookup"><span data-stu-id="e57b7-157">Create a file named PrivateConfig.json with hello following content:</span></span>

    {
        "storageAccountName" : "hello storage account tooreceive data",
        "storageAccountKey" : "hello key of hello account"
    }

<span data-ttu-id="e57b7-158">2. lépés</span><span class="sxs-lookup"><span data-stu-id="e57b7-158">Step 2.</span></span> <span data-ttu-id="e57b7-159">Futtatás  **azure virtuálisgép-bővítmény beállítása vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --magánfelhő-config-path PrivateConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="e57b7-159">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --private-config-path PrivateConfig.json**.</span></span>

### <a name="scenario-2-customize-hello-performance-monitor-metrics"></a><span data-ttu-id="e57b7-160">2. forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="e57b7-160">Scenario 2.</span></span> <span data-ttu-id="e57b7-161">Hello figyelő teljesítménymutatók testreszabása</span><span class="sxs-lookup"><span data-stu-id="e57b7-161">Customize hello performance monitor metrics</span></span>

<span data-ttu-id="e57b7-162">Ez a szakasz ismerteti, hogyan toocustomize hello teljesítményt, és diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="e57b7-162">This section describes how toocustomize hello performance and diagnostic data table.</span></span>

<span data-ttu-id="e57b7-163">1. lépés</span><span class="sxs-lookup"><span data-stu-id="e57b7-163">Step 1.</span></span> <span data-ttu-id="e57b7-164">1. forgatókönyv ismertetett hello tartalmú PrivateConfig.json nevű fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e57b7-164">Create a file named PrivateConfig.json with hello content that was described in Scenario 1.</span></span> <span data-ttu-id="e57b7-165">PublicConfig.json nevű fájlt is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="e57b7-165">Also create a file named PublicConfig.json.</span></span> <span data-ttu-id="e57b7-166">Adja meg a kívánt toocollect hello adott adatok.</span><span class="sxs-lookup"><span data-stu-id="e57b7-166">Specify hello particular data you want toocollect.</span></span>

<span data-ttu-id="e57b7-167">Az összes támogatott szolgáltatók és változók, hivatkozhat hello [System Center Cross Platform megoldások hely](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="e57b7-167">For all supported providers and variables, reference hello [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span> <span data-ttu-id="e57b7-168">Több lekérdezés van, és további lekérdezések toohello parancsfájl hozzáfűzésével több táblát is tárolhatja őket.</span><span class="sxs-lookup"><span data-stu-id="e57b7-168">You can have multiple queries and store them in multiple tables by appending more queries toohello script.</span></span>

<span data-ttu-id="e57b7-169">Alapértelmezés szerint mindig gyűjtött hello Rsyslog adatokat.</span><span class="sxs-lookup"><span data-stu-id="e57b7-169">By default, hello Rsyslog data is always collected.</span></span>

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


<span data-ttu-id="e57b7-170">2. lépés</span><span class="sxs-lookup"><span data-stu-id="e57b7-170">Step 2.</span></span> <span data-ttu-id="e57b7-171">Futtatás  **azure virtuálisgép-bővítmény beállítása vm_name LinuxDiagnostic Microsoft.OSTCExtensions "2.*"--magánfelhő-config-path PrivateConfig.json – nyilvános-config-path PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="e57b7-171">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

### <a name="scenario-3-upload-your-own-log-files"></a><span data-ttu-id="e57b7-172">3. forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="e57b7-172">Scenario 3.</span></span> <span data-ttu-id="e57b7-173">Töltse fel a saját naplófájlok</span><span class="sxs-lookup"><span data-stu-id="e57b7-173">Upload your own log files</span></span>

<span data-ttu-id="e57b7-174">Ez a szakasz ismerteti, hogyan toocollect és feltöltése adott naplófájljai tooyour tárfiók.</span><span class="sxs-lookup"><span data-stu-id="e57b7-174">This section describes how toocollect and upload specific log files tooyour storage account.</span></span> <span data-ttu-id="e57b7-175">Toospecify kell mindkét hello tooyour napló fájl- és hello elérési útjának hello táblázatban, ahol toostore a naplót.</span><span class="sxs-lookup"><span data-stu-id="e57b7-175">You need toospecify both hello path tooyour log file and hello name of hello table where you want toostore your log.</span></span> <span data-ttu-id="e57b7-176">Több naplófájl is létrehozhat több fájl vagy a tábla bejegyzések toohello parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="e57b7-176">You can create multiple log files by adding multiple file/table entries toohello script.</span></span>

<span data-ttu-id="e57b7-177">1. lépés</span><span class="sxs-lookup"><span data-stu-id="e57b7-177">Step 1.</span></span> <span data-ttu-id="e57b7-178">1. forgatókönyv ismertetett hello tartalmú PrivateConfig.json nevű fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e57b7-178">Create a file named PrivateConfig.json with hello content that was described in Scenario 1.</span></span> <span data-ttu-id="e57b7-179">Ezután hozzon létre egy másik fájlt PublicConfig.json hello követően a tartalom:</span><span class="sxs-lookup"><span data-stu-id="e57b7-179">Then create another file named PublicConfig.json with hello following content:</span></span>

```json
{
    "fileCfg" :
    [
        {
            "file" : "/var/log/mysql.err",
            "table" : "mysqlerr"
            }
    ]
}
```

<span data-ttu-id="e57b7-180">2. lépés</span><span class="sxs-lookup"><span data-stu-id="e57b7-180">Step 2.</span></span> <span data-ttu-id="e57b7-181">Futtassa az `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e57b7-181">Run `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.</span></span>

<span data-ttu-id="e57b7-182">Vegye figyelembe, hogy ezt a beállítást az hello bővítmény verziók előzetes too2.3, az összes napló írása túl`/var/log/mysql.err` előfordulhat, hogy túl duplikált`/var/log/syslog` (vagy `/var/log/messages` hello Linux distro függően) is.</span><span class="sxs-lookup"><span data-stu-id="e57b7-182">Note that with this setting on hello extension versions prior too2.3, all logs written too`/var/log/mysql.err` might be duplicated too`/var/log/syslog` (or `/var/log/messages` depending on hello Linux distro) as well.</span></span> <span data-ttu-id="e57b7-183">Ha szeretné tooavoid a naplózás duplikált, kizárhatja naplózása `local6` létesítmény rsyslog konfigurációs naplózza.</span><span class="sxs-lookup"><span data-stu-id="e57b7-183">If you'd like tooavoid this duplicate logging, you can exclude logging of `local6` facility logs in your rsyslog configuration.</span></span> <span data-ttu-id="e57b7-184">Hello Linux distro függ, de az Ubuntu 14.04 rendszeren hello fájl toomodify `/etc/rsyslog.d/50-default.conf` és lecserélheti hello sor `*.*;auth,authpriv.none -/var/log/syslog` túl`*.*;auth,authpriv,local6.none -/var/log/syslog`.</span><span class="sxs-lookup"><span data-stu-id="e57b7-184">It depends on hello Linux distro, but on an Ubuntu 14.04 system, hello file toomodify is `/etc/rsyslog.d/50-default.conf` and you can replace hello line `*.*;auth,authpriv.none -/var/log/syslog` too`*.*;auth,authpriv,local6.none -/var/log/syslog`.</span></span> <span data-ttu-id="e57b7-185">Ez a probléma fennáll a legújabb gyorsjavítások kiadásának hello 2.3 (2.3.9007), így ha hello bővítmény verzió 2.3, a probléma nem következik.</span><span class="sxs-lookup"><span data-stu-id="e57b7-185">This issue is fixed in hello latest hotfix release of 2.3 (2.3.9007), so if you have hello extension version 2.3, this issue should not happen.</span></span> <span data-ttu-id="e57b7-186">Ha továbbra is támogatja a virtuális gép újraindítása után is, lépjen velünk kapcsolatba, és segítsen hibaelhárítása, ezért hello gyorsjavítás letöltéséhez nem települ automatikusan.</span><span class="sxs-lookup"><span data-stu-id="e57b7-186">If it still does even after restarting your VM, please contact us and help us troubleshoot why hello latest hotfix version is not installed automatically.</span></span>

### <a name="scenario-4-stop-hello-extension-from-collecting-any-logs"></a><span data-ttu-id="e57b7-187">4. forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="e57b7-187">Scenario 4.</span></span> <span data-ttu-id="e57b7-188">Állítsa le a naplók gyűjtésére hello bővítmény</span><span class="sxs-lookup"><span data-stu-id="e57b7-188">Stop hello extension from collecting any logs</span></span>

<span data-ttu-id="e57b7-189">Ez a szakasz ismerteti, hogyan naplózza az toostop hello bővítmény gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="e57b7-189">This section describes how toostop hello extension from collecting logs.</span></span> <span data-ttu-id="e57b7-190">Vegye figyelembe, hogy a figyelési ügynök folyamat hello lesz továbbra is működik, és az újrakonfigurálás mellett is.</span><span class="sxs-lookup"><span data-stu-id="e57b7-190">Note that hello monitoring agent process will be still up and running even with this reconfiguration.</span></span> <span data-ttu-id="e57b7-191">Ha szeretné, hogy teljesen figyelési ügynök folyamat toostop hello, hello bővítmény letiltásával is megteheti.</span><span class="sxs-lookup"><span data-stu-id="e57b7-191">If you'd like toostop hello monitoring agent process completely, you can do so by disabling hello extension.</span></span> <span data-ttu-id="e57b7-192">hello parancs toodisable hello kiterjesztés `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span><span class="sxs-lookup"><span data-stu-id="e57b7-192">hello command toodisable hello extension is `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span></span>

<span data-ttu-id="e57b7-193">1. lépés</span><span class="sxs-lookup"><span data-stu-id="e57b7-193">Step 1.</span></span> <span data-ttu-id="e57b7-194">1. forgatókönyv ismertetett hello tartalmú PrivateConfig.json nevű fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e57b7-194">Create a file named PrivateConfig.json with hello content that was described in Scenario 1.</span></span> <span data-ttu-id="e57b7-195">Egy másik PublicConfig.json nevű hello a következő fájl tartalmának létrehozása:</span><span class="sxs-lookup"><span data-stu-id="e57b7-195">Create another file named PublicConfig.json with hello following content:</span></span>

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


<span data-ttu-id="e57b7-196">2. lépés</span><span class="sxs-lookup"><span data-stu-id="e57b7-196">Step 2.</span></span> <span data-ttu-id="e57b7-197">Futtatás  **azure virtuálisgép-bővítmény beállítása vm_name LinuxDiagnostic Microsoft.OSTCExtensions "2.*"--magánfelhő-config-path PrivateConfig.json – nyilvános-config-path PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="e57b7-197">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

## <a name="review-your-data"></a><span data-ttu-id="e57b7-198">Tekintse át az adatokat</span><span class="sxs-lookup"><span data-stu-id="e57b7-198">Review your data</span></span>

<span data-ttu-id="e57b7-199">hello teljesítmény- és diagnosztikai adatokat egy Azure Storage tábla vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="e57b7-199">hello performance and diagnostic data are stored in an Azure Storage table.</span></span> <span data-ttu-id="e57b7-200">Felülvizsgálati [hogyan toouse Azure Table Storage-ának Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) toolearn tooaccess hello adatok hello tábla Azure CLI-parancsfájlok használatával.</span><span class="sxs-lookup"><span data-stu-id="e57b7-200">Review [How toouse Azure Table Storage from Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) toolearn how tooaccess hello data in hello storage table by using Azure CLI scripts.</span></span>

<span data-ttu-id="e57b7-201">Ezenkívül használhatja következő felhasználói felületi eszközöket tooaccess hello adatokat:</span><span class="sxs-lookup"><span data-stu-id="e57b7-201">In addition, you can use following UI tools tooaccess hello data:</span></span>

1. <span data-ttu-id="e57b7-202">A Visual Studio Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="e57b7-202">Visual Studio Server Explorer.</span></span> <span data-ttu-id="e57b7-203">Nyissa meg a tárfiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="e57b7-203">Go tooyour storage account.</span></span> <span data-ttu-id="e57b7-204">Miután hello VM körülbelül 5 percig, látni fogja a hello négy alapértelmezett táblák: "LinuxCpu", "LinuxDisk", "LinuxMemory" és "Linuxsyslog".</span><span class="sxs-lookup"><span data-stu-id="e57b7-204">After hello VM runs for about five minutes, you'll see hello four default tables: “LinuxCpu”, ”LinuxDisk”, ”LinuxMemory”, and ”Linuxsyslog”.</span></span> <span data-ttu-id="e57b7-205">Kattintson duplán a hello nevek tooview hello adatait.</span><span class="sxs-lookup"><span data-stu-id="e57b7-205">Double-click hello table names tooview hello data.</span></span>
1. <span data-ttu-id="e57b7-206">[Az Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Tártallózó").</span><span class="sxs-lookup"><span data-stu-id="e57b7-206">[Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span></span>

![Kép](./media/diagnostic-extension/no1.png)

<span data-ttu-id="e57b7-208">Ha engedélyezte a fileCfg vagy perfCfg (leírt forgatókönyvek 2 és 3), a Visual Studio Server Explorer és az Azure Tártallózó tooview nem alapértelmezett adatokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e57b7-208">If you've enabled fileCfg or perfCfg (as described in Scenarios 2 and 3), you can use Visual Studio Server Explorer and Azure Storage Explorer tooview non-default data.</span></span>

## <a name="known-issues"></a><span data-ttu-id="e57b7-209">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="e57b7-209">Known issues</span></span>

* <span data-ttu-id="e57b7-210">hello Rsyslog információkat, és naplófájlt ügyfél által megadott csak parancsprogramok használatával érhető el.</span><span class="sxs-lookup"><span data-stu-id="e57b7-210">hello Rsyslog information and customer-specified log file can only be accessed via scripting.</span></span>
