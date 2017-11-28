---
title: "Linux virtuális Gépet egy Virtuálisgép-bővítménnyel figyelése |} Microsoft Docs"
description: "Útmutató: a Linux diagnosztikai bővítmény a figyelheti a teljesítmény- és a Linux virtuális gépek az Azure diagnosztikai adatokat."
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
ms.openlocfilehash: b8c6e2e22d8478b6e92e7b7942f15d37a840fed3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-linux-diagnostic-extension-to-monitor-the-performance-and-diagnostic-data-of-a-linux-vm"></a><span data-ttu-id="fb91c-103">Linuxos VM teljesítmény- és diagnosztikai adatainak monitorozása a linuxos diagnosztikai bővítménnyel</span><span class="sxs-lookup"><span data-stu-id="fb91c-103">Use the Linux Diagnostic Extension to monitor the performance and diagnostic data of a Linux VM</span></span>

<span data-ttu-id="fb91c-104">Ez a dokumentum ismerteti a Linux diagnosztikai bővítmény 2.3 verziója.</span><span class="sxs-lookup"><span data-stu-id="fb91c-104">This document describes version 2.3 of the Linux Diagnostic Extension.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb91c-105">Jelen verziója elavult, és lehet, hogy közzé nem tett 2018. június 30 követően.</span><span class="sxs-lookup"><span data-stu-id="fb91c-105">This version is deprecated, and it may be unpublished any time after June 30, 2018.</span></span> <span data-ttu-id="fb91c-106">Helyette 3.0-s verziója.</span><span class="sxs-lookup"><span data-stu-id="fb91c-106">It has been replaced by version 3.0.</span></span> <span data-ttu-id="fb91c-107">További információkért lásd: a [3.0-s verziója a Linux-diagnosztikai bővítmény dokumentációja](../diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="fb91c-107">For more information, see the [documentation for version 3.0 of the Linux Diagnostic Extension](../diagnostic-extension.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="fb91c-108">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="fb91c-108">Introduction</span></span>

<span data-ttu-id="fb91c-109">(**Megjegyzés**: A Linux diagnosztikai kiterjesztés nyílt forrása [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) , amennyiben először közzé a legfrissebb információkkal a kiterjesztést.</span><span class="sxs-lookup"><span data-stu-id="fb91c-109">(**Note**: The Linux Diagnostic Extension is open-sourced on [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) where the most current information on the extension is first published.</span></span> <span data-ttu-id="fb91c-110">Érdemes ellenőrizni a [GitHub-oldalon](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) első.)</span><span class="sxs-lookup"><span data-stu-id="fb91c-110">You might want to check the [GitHub page](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) first.)</span></span>

<span data-ttu-id="fb91c-111">A Linux diagnosztikai bővítmény segít a felhasználói figyelése a Microsoft Azure-on futó Linux virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="fb91c-111">The Linux Diagnostic Extension helps a user monitor the Linux VMs that are running on Microsoft Azure.</span></span> <span data-ttu-id="fb91c-112">A következő képességekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="fb91c-112">It has the following capabilities:</span></span>

* <span data-ttu-id="fb91c-113">Gyűjti, és feltölti a felhasználói tárolási tábla, diagnosztikai és a rendszernaplóba párbeszédeket a Linux virtuális Gépet a teljesítmény-információkat.</span><span class="sxs-lookup"><span data-stu-id="fb91c-113">Collects and uploads the system performance information from the Linux VM to the user's storage table, including diagnostic and syslog information.</span></span>
* <span data-ttu-id="fb91c-114">Lehetővé teszi a felhasználók a gyűjtött és a feltöltött adatok metrikák testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="fb91c-114">Enables users to customize the data metrics that will be collected and uploaded.</span></span>
* <span data-ttu-id="fb91c-115">Lehetővé teszi a felhasználók egy kijelölt tároló táblához megadott naplófájlok feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="fb91c-115">Enables users to upload specified log files to a designated storage table.</span></span>

<span data-ttu-id="fb91c-116">2.3 a jelenlegi verzióban szerepel:</span><span class="sxs-lookup"><span data-stu-id="fb91c-116">In the current version 2.3, the data includes:</span></span>

* <span data-ttu-id="fb91c-117">Az összes Linux Rsyslog naplókat, beleértve a rendszer, a biztonsági és az alkalmazás naplóiban.</span><span class="sxs-lookup"><span data-stu-id="fb91c-117">All Linux Rsyslog logs, including system, security, and application logs.</span></span>
* <span data-ttu-id="fb91c-118">Összes rendszer adat, amely meg van adva [a System Center Cross Platform megoldások hely](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="fb91c-118">All system data that's specified on [the System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
* <span data-ttu-id="fb91c-119">Felhasználó által meghatározott naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="fb91c-119">User-specified log files.</span></span>

<span data-ttu-id="fb91c-120">Ezt a bővítményt a klasszikus és Resource Manager üzembe helyezési modellel működik.</span><span class="sxs-lookup"><span data-stu-id="fb91c-120">This extension works with both the classic and Resource Manager deployment models.</span></span>

### <a name="current-version-of-the-extension-and-deprecation-of-old-versions"></a><span data-ttu-id="fb91c-121">A bővítményt, és a régi változatainak elavulása jelenlegi verziója</span><span class="sxs-lookup"><span data-stu-id="fb91c-121">Current version of the extension and deprecation of old versions</span></span>

<span data-ttu-id="fb91c-122">A bővítmény legújabb verziója **2.3**, és **a régi verziókat (2.0, 2.1-es és 2.2) a rendszer elavult, majd közzé nem tett az év (2017) végéig**.</span><span class="sxs-lookup"><span data-stu-id="fb91c-122">The latest version of the extension is **2.3**, and **any old versions (2.0, 2.1, and 2.2) will be deprecated and unpublished by end of this year (2017)**.</span></span> <span data-ttu-id="fb91c-123">Ha telepítette a Linux diagnosztikai bővítmény automatikus alverzió frissítés le van tiltva, erősen ajánlott, hogy távolítsa el a bővítményt, és engedélyezve van az automatikus alverzió frissítést újra kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="fb91c-123">If you installed the Linux Diagnostic extension with automatic minor version upgrade disabled, it's strongly recommended that you uninstall the extension and reinstall it with automatic minor version upgrade enabled.</span></span> <span data-ttu-id="fb91c-124">(ASM) klasszikus virtuális gépeken érhető el ez a bővítmény Azure XPLAT parancssori felületen vagy a Powershellen keresztül telepítésekor "2.*" megadásával verzióval.</span><span class="sxs-lookup"><span data-stu-id="fb91c-124">On classic (ASM) VMs, you can achieve this by specifying '2.*' as the version if you are installing the extension through Azure XPLAT CLI or Powershell.</span></span> <span data-ttu-id="fb91c-125">ARM gépeken lehet ezt elérni-ot ""autoUpgradeMinorVersion": true" az a virtuális gép központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="fb91c-125">On ARM VMs, you can achieve this by including '"autoUpgradeMinorVersion": true' in the VM deployment template.</span></span> <span data-ttu-id="fb91c-126">A bővítmény bármely új telepítése is, az automatikus frissítési beállítás engedélyezve van a alverzió kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="fb91c-126">Also, any new installation of the extension should have the auto minor version upgrade option turned on.</span></span>

## <a name="enable-the-extension"></a><span data-ttu-id="fb91c-127">A bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="fb91c-127">Enable the extension</span></span>

<span data-ttu-id="fb91c-128">A bővítmény használatával engedélyezheti a [Azure-portálon](https://portal.azure.com/#), az Azure PowerShell vagy Azure CLI-parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="fb91c-128">You can enable this extension by using the [Azure portal](https://portal.azure.com/#), Azure PowerShell, or Azure CLI scripts.</span></span>

<span data-ttu-id="fb91c-129">Tekintheti meg és konfigurálhatja a rendszer és a teljesítmény adatoknak közvetlenül az Azure-portálon, hajtsa végre a [ezeket a lépéseket az Azure blog](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span><span class="sxs-lookup"><span data-stu-id="fb91c-129">To view and configure the system and performance data directly from the Azure portal, follow [these steps on the Azure blog](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span></span>

<span data-ttu-id="fb91c-130">Ez a cikk foglalkozik, hogyan engedélyezése és konfigurálása a bővítmény Azure CLI-parancsok segítségével.</span><span class="sxs-lookup"><span data-stu-id="fb91c-130">This article focuses on how to enable and configure the extension by using Azure CLI commands.</span></span> <span data-ttu-id="fb91c-131">Ez lehetővé teszi, hogy olvassa el, és tekintse meg az adatok közvetlenül a tároló tábla.</span><span class="sxs-lookup"><span data-stu-id="fb91c-131">This allows you to read and view the data directly from the storage table.</span></span>

<span data-ttu-id="fb91c-132">Vegye figyelembe, hogy az itt ismertetett konfiguráció módszerek az Azure portál nem fognak működni.</span><span class="sxs-lookup"><span data-stu-id="fb91c-132">Note that the configuration methods that are described here won't work for the Azure portal.</span></span> <span data-ttu-id="fb91c-133">Tekintheti meg és konfigurálhatja a rendszer és a teljesítmény adatoknak közvetlenül az Azure-portálon, a bővítmény engedélyezni kell a portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="fb91c-133">To view and configure the system and performance data directly from the Azure portal, the extension must be enabled through the portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb91c-134">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fb91c-134">Prerequisites</span></span>

* <span data-ttu-id="fb91c-135">**Az Azure Linux ügynök 2.0.6 verzió vagy újabb**.</span><span class="sxs-lookup"><span data-stu-id="fb91c-135">**Azure Linux Agent version 2.0.6 or later**.</span></span>

  <span data-ttu-id="fb91c-136">Vegye figyelembe, hogy a legtöbb Azure virtuális gép Linux gyűjtemény lemezképei tartalmazzák 2.0.6 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="fb91c-136">Note that most Azure VM Linux gallery images include version 2.0.6 or later.</span></span> <span data-ttu-id="fb91c-137">Futtathat **WAAgent-verzió** annak ellenőrzéséhez, hogy melyik verzióját a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="fb91c-137">You can run **WAAgent -version** to confirm which version is installed on the VM.</span></span> <span data-ttu-id="fb91c-138">Ha a virtuális gép 2.0.6 korábbi verziót futtat, amelyeket követve [ezeket az utasításokat a Githubon](https://github.com/Azure/WALinuxAgent "utasításokat") frissíti.</span><span class="sxs-lookup"><span data-stu-id="fb91c-138">If the VM is running a version that's earlier than 2.0.6, you can follow [these instructions on GitHub](https://github.com/Azure/WALinuxAgent "instructions") to update it.</span></span>
* <span data-ttu-id="fb91c-139">**Azure parancssori felület (CLI)**.</span><span class="sxs-lookup"><span data-stu-id="fb91c-139">**Azure CLI**.</span></span> <span data-ttu-id="fb91c-140">Hajtsa végre a [Ez az útmutató a parancssori felület telepítése](../../../cli-install-nodejs.md) az Azure CLI-környezetet a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fb91c-140">Follow [this guidance for installing CLI](../../../cli-install-nodejs.md) to set up the Azure CLI environment on your machine.</span></span> <span data-ttu-id="fb91c-141">Az Azure parancssori felület telepítése után is használhatja a **azure** parancsot a parancssori felület (a Bash, a Terminálszolgáltatások vagy a parancssorból) az Azure parancssori felület parancsait eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="fb91c-141">After Azure CLI is installed, you can use the **azure** command from your command-line interface (Bash, Terminal, or command prompt) to access the Azure CLI commands.</span></span> <span data-ttu-id="fb91c-142">Példa:</span><span class="sxs-lookup"><span data-stu-id="fb91c-142">For example:</span></span>

  * <span data-ttu-id="fb91c-143">Futtatás **azure virtuálisgép-bővítmény készlet – súgó** vonatkozó részletes információk.</span><span class="sxs-lookup"><span data-stu-id="fb91c-143">Run **azure vm extension set --help** for detailed help information.</span></span>
  * <span data-ttu-id="fb91c-144">Futtatás **azure bejelentkezési** bejelentkezni az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="fb91c-144">Run **azure login** to sign in to Azure.</span></span>
  * <span data-ttu-id="fb91c-145">Futtatás **azure virtuális gép lista** rendelkezik az Azure virtuális gépek listáját.</span><span class="sxs-lookup"><span data-stu-id="fb91c-145">Run **azure vm list** to list all the virtual machines that you have on Azure.</span></span>
* <span data-ttu-id="fb91c-146">Egy tárfiókot, az adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="fb91c-146">A storage account to store the data.</span></span> <span data-ttu-id="fb91c-147">A korábban létrehozott tárfiók neve és a hívóbetű feltölteni az adatokat a tárhelyre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="fb91c-147">You will need a storage account name that was created previously and an access key to upload the data to your storage.</span></span>

## <a name="use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension"></a><span data-ttu-id="fb91c-148">Az Azure CLI parancs segítségével a Linux diagnosztikai bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="fb91c-148">Use the Azure CLI command to enable the Linux Diagnostic Extension</span></span>

### <a name="scenario-1-enable-the-extension-with-the-default-data-set"></a><span data-ttu-id="fb91c-149">1. forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="fb91c-149">Scenario 1.</span></span> <span data-ttu-id="fb91c-150">Engedélyezi a bővítményt, az alapértelmezett adatkészlet</span><span class="sxs-lookup"><span data-stu-id="fb91c-150">Enable the extension with the default data set</span></span>

<span data-ttu-id="fb91c-151">2.3-as vagy újabb verziója, az alapértelmezett gyűjtendő szerepel:</span><span class="sxs-lookup"><span data-stu-id="fb91c-151">In version 2.3 or later, the default data that will be collected includes:</span></span>

* <span data-ttu-id="fb91c-152">Minden Rsyslog információk (beleértve a rendszer, a biztonsági és az alkalmazásnaplók).</span><span class="sxs-lookup"><span data-stu-id="fb91c-152">All Rsyslog information (including system, security, and application logs).</span></span>  
* <span data-ttu-id="fb91c-153">Egy sor alapján a rendszer adatai.</span><span class="sxs-lookup"><span data-stu-id="fb91c-153">A core set of basis system data.</span></span> <span data-ttu-id="fb91c-154">Vegye figyelembe, hogy a teljes adatkészlet a leírása a [System Center Cross Platform megoldások hely](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="fb91c-154">Note that the full data set is described on the [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
  <span data-ttu-id="fb91c-155">Ha további adatokat engedélyezni szeretné, további lépéseket a forgatókönyvek 2 és 3.</span><span class="sxs-lookup"><span data-stu-id="fb91c-155">If you want to enable extra data, continue with the steps in Scenarios 2 and 3.</span></span>

<span data-ttu-id="fb91c-156">1. lépés</span><span class="sxs-lookup"><span data-stu-id="fb91c-156">Step 1.</span></span> <span data-ttu-id="fb91c-157">Hozzon létre egy fájlt PrivateConfig.json a következő tartalommal:</span><span class="sxs-lookup"><span data-stu-id="fb91c-157">Create a file named PrivateConfig.json with the following content:</span></span>

    {
        "storageAccountName" : "the storage account to receive data",
        "storageAccountKey" : "the key of the account"
    }

<span data-ttu-id="fb91c-158">2. lépés</span><span class="sxs-lookup"><span data-stu-id="fb91c-158">Step 2.</span></span> <span data-ttu-id="fb91c-159">Futtatás  **azure virtuálisgép-bővítmény beállítása vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --magánfelhő-config-path PrivateConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="fb91c-159">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --private-config-path PrivateConfig.json**.</span></span>

### <a name="scenario-2-customize-the-performance-monitor-metrics"></a><span data-ttu-id="fb91c-160">2. forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="fb91c-160">Scenario 2.</span></span> <span data-ttu-id="fb91c-161">A figyelő metrikák testreszabása</span><span class="sxs-lookup"><span data-stu-id="fb91c-161">Customize the performance monitor metrics</span></span>

<span data-ttu-id="fb91c-162">Ez a szakasz ismerteti, hogyan szabhatja testre a teljesítményt, és diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="fb91c-162">This section describes how to customize the performance and diagnostic data table.</span></span>

<span data-ttu-id="fb91c-163">1. lépés</span><span class="sxs-lookup"><span data-stu-id="fb91c-163">Step 1.</span></span> <span data-ttu-id="fb91c-164">A tartalom 1. forgatókönyv ismertetett PrivateConfig.json nevű fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fb91c-164">Create a file named PrivateConfig.json with the content that was described in Scenario 1.</span></span> <span data-ttu-id="fb91c-165">PublicConfig.json nevű fájlt is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="fb91c-165">Also create a file named PublicConfig.json.</span></span> <span data-ttu-id="fb91c-166">Adja meg az adott adatokat szeretne gyűjteni.</span><span class="sxs-lookup"><span data-stu-id="fb91c-166">Specify the particular data you want to collect.</span></span>

<span data-ttu-id="fb91c-167">Minden támogatott szolgáltatók és változók, lásd a [System Center Cross Platform megoldások hely](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="fb91c-167">For all supported providers and variables, reference the [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span> <span data-ttu-id="fb91c-168">Több lekérdezés van, és további lekérdezések hozzáfűzésével a parancsfájl több táblát is tárolhatja őket.</span><span class="sxs-lookup"><span data-stu-id="fb91c-168">You can have multiple queries and store them in multiple tables by appending more queries to the script.</span></span>

<span data-ttu-id="fb91c-169">Alapértelmezés szerint a Rsyslog mindig adatgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="fb91c-169">By default, the Rsyslog data is always collected.</span></span>

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


<span data-ttu-id="fb91c-170">2. lépés</span><span class="sxs-lookup"><span data-stu-id="fb91c-170">Step 2.</span></span> <span data-ttu-id="fb91c-171">Futtatás  **azure virtuálisgép-bővítmény beállítása vm_name LinuxDiagnostic Microsoft.OSTCExtensions "2.*"--magánfelhő-config-path PrivateConfig.json – nyilvános-config-path PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="fb91c-171">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

### <a name="scenario-3-upload-your-own-log-files"></a><span data-ttu-id="fb91c-172">3. forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="fb91c-172">Scenario 3.</span></span> <span data-ttu-id="fb91c-173">Töltse fel a saját naplófájlok</span><span class="sxs-lookup"><span data-stu-id="fb91c-173">Upload your own log files</span></span>

<span data-ttu-id="fb91c-174">Ez a szakasz ismerteti, hogyan összegyűjtésére, és töltse fel a megadott naplófájlokat a tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="fb91c-174">This section describes how to collect and upload specific log files to your storage account.</span></span> <span data-ttu-id="fb91c-175">Meg kell adnia az elérési útját, a naplófájl, és a tábla, hol szeretné tárolni a napló nevét.</span><span class="sxs-lookup"><span data-stu-id="fb91c-175">You need to specify both the path to your log file and the name of the table where you want to store your log.</span></span> <span data-ttu-id="fb91c-176">A parancsfájl több fájl vagy a tábla bejegyzés hozzáadásával több naplófájl is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="fb91c-176">You can create multiple log files by adding multiple file/table entries to the script.</span></span>

<span data-ttu-id="fb91c-177">1. lépés</span><span class="sxs-lookup"><span data-stu-id="fb91c-177">Step 1.</span></span> <span data-ttu-id="fb91c-178">A tartalom 1. forgatókönyv ismertetett PrivateConfig.json nevű fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fb91c-178">Create a file named PrivateConfig.json with the content that was described in Scenario 1.</span></span> <span data-ttu-id="fb91c-179">Ezután hozzon létre egy másik nevű PublicConfig.json a következő tartalommal:</span><span class="sxs-lookup"><span data-stu-id="fb91c-179">Then create another file named PublicConfig.json with the following content:</span></span>

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

<span data-ttu-id="fb91c-180">2. lépés</span><span class="sxs-lookup"><span data-stu-id="fb91c-180">Step 2.</span></span> <span data-ttu-id="fb91c-181">Futtassa az `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json` parancsot.</span><span class="sxs-lookup"><span data-stu-id="fb91c-181">Run `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.</span></span>

<span data-ttu-id="fb91c-182">Vegye figyelembe, hogy ezzel a beállítással a bővítmény verzióin 2.3 előtt minden naplóz írt `/var/log/mysql.err` előfordulhat, hogy másolja, `/var/log/syslog` (vagy `/var/log/messages` attól függően, hogy a Linux distro) is.</span><span class="sxs-lookup"><span data-stu-id="fb91c-182">Note that with this setting on the extension versions prior to 2.3, all logs written to `/var/log/mysql.err` might be duplicated to `/var/log/syslog` (or `/var/log/messages` depending on the Linux distro) as well.</span></span> <span data-ttu-id="fb91c-183">Ha azt szeretné, az ismétlődő naplózási elkerülésére, kizárhatja naplózása `local6` létesítmény rsyslog konfigurációs naplózza.</span><span class="sxs-lookup"><span data-stu-id="fb91c-183">If you'd like to avoid this duplicate logging, you can exclude logging of `local6` facility logs in your rsyslog configuration.</span></span> <span data-ttu-id="fb91c-184">A Linux distro függ, de az Ubuntu 14.04 rendszeren, a fájl módosítása `/etc/rsyslog.d/50-default.conf` és a sor lecserélheti `*.*;auth,authpriv.none -/var/log/syslog` való `*.*;auth,authpriv,local6.none -/var/log/syslog`.</span><span class="sxs-lookup"><span data-stu-id="fb91c-184">It depends on the Linux distro, but on an Ubuntu 14.04 system, the file to modify is `/etc/rsyslog.d/50-default.conf` and you can replace the line `*.*;auth,authpriv.none -/var/log/syslog` to `*.*;auth,authpriv,local6.none -/var/log/syslog`.</span></span> <span data-ttu-id="fb91c-185">Ez a probléma fennáll a legújabb gyorsjavítások kiadása a 2.3 (2.3.9007), így a bővítmény 2.3 verziója van, ha a probléma nem következik.</span><span class="sxs-lookup"><span data-stu-id="fb91c-185">This issue is fixed in the latest hotfix release of 2.3 (2.3.9007), so if you have the extension version 2.3, this issue should not happen.</span></span> <span data-ttu-id="fb91c-186">Ha továbbra is támogatja a virtuális gép újraindítása után is, lépjen velünk kapcsolatba, és segítsen hibaelhárítása, ezért a legújabb gyorsjavítások nem települ automatikusan.</span><span class="sxs-lookup"><span data-stu-id="fb91c-186">If it still does even after restarting your VM, please contact us and help us troubleshoot why the latest hotfix version is not installed automatically.</span></span>

### <a name="scenario-4-stop-the-extension-from-collecting-any-logs"></a><span data-ttu-id="fb91c-187">4. forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="fb91c-187">Scenario 4.</span></span> <span data-ttu-id="fb91c-188">A bővítmény a naplók gyűjtésére leállítása</span><span class="sxs-lookup"><span data-stu-id="fb91c-188">Stop the extension from collecting any logs</span></span>

<span data-ttu-id="fb91c-189">Ez a szakasz ismerteti, hogy a kiterjesztést a naplók gyűjtésére.</span><span class="sxs-lookup"><span data-stu-id="fb91c-189">This section describes how to stop the extension from collecting logs.</span></span> <span data-ttu-id="fb91c-190">Vegye figyelembe, hogy a figyelési ügynök folyamat továbbra is működik, és az újrakonfigurálás mellett is.</span><span class="sxs-lookup"><span data-stu-id="fb91c-190">Note that the monitoring agent process will be still up and running even with this reconfiguration.</span></span> <span data-ttu-id="fb91c-191">Ha szeretné a figyelési ügynök folyamat teljesen leállhat, a bővítmény letiltásával is megteheti.</span><span class="sxs-lookup"><span data-stu-id="fb91c-191">If you'd like to stop the monitoring agent process completely, you can do so by disabling the extension.</span></span> <span data-ttu-id="fb91c-192">Letiltja a bővítményt a parancs `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span><span class="sxs-lookup"><span data-stu-id="fb91c-192">The command to disable the extension is `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span></span>

<span data-ttu-id="fb91c-193">1. lépés</span><span class="sxs-lookup"><span data-stu-id="fb91c-193">Step 1.</span></span> <span data-ttu-id="fb91c-194">A tartalom 1. forgatókönyv ismertetett PrivateConfig.json nevű fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fb91c-194">Create a file named PrivateConfig.json with the content that was described in Scenario 1.</span></span> <span data-ttu-id="fb91c-195">Hozzon létre egy másik nevű PublicConfig.json a következő tartalommal:</span><span class="sxs-lookup"><span data-stu-id="fb91c-195">Create another file named PublicConfig.json with the following content:</span></span>

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


<span data-ttu-id="fb91c-196">2. lépés</span><span class="sxs-lookup"><span data-stu-id="fb91c-196">Step 2.</span></span> <span data-ttu-id="fb91c-197">Futtatás  **azure virtuálisgép-bővítmény beállítása vm_name LinuxDiagnostic Microsoft.OSTCExtensions "2.*"--magánfelhő-config-path PrivateConfig.json – nyilvános-config-path PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="fb91c-197">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

## <a name="review-your-data"></a><span data-ttu-id="fb91c-198">Tekintse át az adatokat</span><span class="sxs-lookup"><span data-stu-id="fb91c-198">Review your data</span></span>

<span data-ttu-id="fb91c-199">A teljesítmény- és diagnosztikai adatokat egy Azure Storage tábla vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="fb91c-199">The performance and diagnostic data are stored in an Azure Storage table.</span></span> <span data-ttu-id="fb91c-200">Felülvizsgálati [használata az Azure Table Storage-ának Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) megtudhatja, hogyan férhet hozzá a tárolási tábla Azure CLI-parancsfájlok használatával.</span><span class="sxs-lookup"><span data-stu-id="fb91c-200">Review [How to use Azure Table Storage from Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) to learn how to access the data in the storage table by using Azure CLI scripts.</span></span>

<span data-ttu-id="fb91c-201">Ezenkívül segítségével következő felhasználói felületi eszközökkel érik el az adatokat:</span><span class="sxs-lookup"><span data-stu-id="fb91c-201">In addition, you can use following UI tools to access the data:</span></span>

1. <span data-ttu-id="fb91c-202">A Visual Studio Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="fb91c-202">Visual Studio Server Explorer.</span></span> <span data-ttu-id="fb91c-203">Lépjen a tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="fb91c-203">Go to your storage account.</span></span> <span data-ttu-id="fb91c-204">Miután a virtuális gép fut, körülbelül 5 percig, látni fogja, a négy alapértelmezett táblák: "LinuxCpu", "LinuxDisk", "LinuxMemory" és "Linuxsyslog".</span><span class="sxs-lookup"><span data-stu-id="fb91c-204">After the VM runs for about five minutes, you'll see the four default tables: “LinuxCpu”, ”LinuxDisk”, ”LinuxMemory”, and ”Linuxsyslog”.</span></span> <span data-ttu-id="fb91c-205">Kattintson duplán a táblanevek, az adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="fb91c-205">Double-click the table names to view the data.</span></span>
1. <span data-ttu-id="fb91c-206">[Az Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Tártallózó").</span><span class="sxs-lookup"><span data-stu-id="fb91c-206">[Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span></span>

![Kép](./media/diagnostic-extension/no1.png)

<span data-ttu-id="fb91c-208">Ha engedélyezte a fileCfg vagy perfCfg (leírt forgatókönyvek 2 és 3), a Visual Studio Server Explorer és az Azure Tártallózó használhatja nem alapértelmezett adatainak megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="fb91c-208">If you've enabled fileCfg or perfCfg (as described in Scenarios 2 and 3), you can use Visual Studio Server Explorer and Azure Storage Explorer to view non-default data.</span></span>

## <a name="known-issues"></a><span data-ttu-id="fb91c-209">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="fb91c-209">Known issues</span></span>

* <span data-ttu-id="fb91c-210">A Rsyslog információkat és az ügyfél által megadott naplófájl csak használható parancsprogramok használatával.</span><span class="sxs-lookup"><span data-stu-id="fb91c-210">The Rsyslog information and customer-specified log file can only be accessed via scripting.</span></span>
