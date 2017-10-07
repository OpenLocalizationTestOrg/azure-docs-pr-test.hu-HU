---
title: "a rugalmas adatbázis-feladatok aaaInstalling |} Microsoft Docs"
description: "Hello rugalmas feladat szolgáltatás telepítési útmutatót."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: cbe0aa2b-17e3-4b6f-a16f-6ebc1f5a66af
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 0349f66a4428f81d00d43681d7f2177f273ec032
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="installing-elastic-database-jobs-overview"></a><span data-ttu-id="13e85-103">Telepítése rugalmas feladatok – áttekintés</span><span class="sxs-lookup"><span data-stu-id="13e85-103">Installing Elastic Database jobs overview</span></span>
<span data-ttu-id="13e85-104">[**Rugalmas adatbázis-feladatok** ](sql-database-elastic-jobs-overview.md) PowerShell telepíthető vagy keresztül kaphatnak a klasszikus Azure-Portal.You hello hozzáférési toocreate és hello PowerShell API használata csak akkor, ha hello PowerShell csomag telepítése feladatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="13e85-104">[**Elastic Database jobs**](sql-database-elastic-jobs-overview.md) can be installed via PowerShell or through hello Azure Classic Portal.You can gain access toocreate and manage jobs using hello PowerShell API only if you install hello PowerShell package.</span></span> <span data-ttu-id="13e85-105">Emellett hello PowerShell API-k jóval több funkciót kínál mint hello portál ezen a ponton a időben.</span><span class="sxs-lookup"><span data-stu-id="13e85-105">Additionally, hello PowerShell APIs provide significantly more functionality than hello portal at this point in time.</span></span>

<span data-ttu-id="13e85-106">Ha már telepítette **rugalmas adatbázis-feladatok** keresztül hello Portal a meglévő **rugalmas készlet**, hello legújabb Powershell preview tartalmaz parancsfájlok tooupgrade a meglévő telepítést.</span><span class="sxs-lookup"><span data-stu-id="13e85-106">If you have already installed **Elastic Database jobs** through hello Portal from an existing **elastic pool**, hello latest Powershell preview includes scripts tooupgrade your existing installation.</span></span> <span data-ttu-id="13e85-107">Nagyon fontos ajánlott tooupgrade a telepítési toohello legújabb **rugalmas adatbázis-feladatok** keresztül új funkciók előnyeit rendelés tootake összetevők hello PowerShell API-k.</span><span class="sxs-lookup"><span data-stu-id="13e85-107">It is highly recommended tooupgrade your installation toohello latest **Elastic Database jobs** components in order tootake advantage of new functionality exposed via hello PowerShell APIs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13e85-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="13e85-108">Prerequisites</span></span>
* <span data-ttu-id="13e85-109">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="13e85-109">An Azure subscription.</span></span> <span data-ttu-id="13e85-110">Ingyenes próbaverzió, lásd: [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="13e85-110">For a free trial, see [Free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="13e85-111">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="13e85-111">Azure PowerShell.</span></span> <span data-ttu-id="13e85-112">Hello hello segítségével legújabb verziójának telepítése [Webplatform-telepítő](http://go.microsoft.com/fwlink/p/?linkid=320376).</span><span class="sxs-lookup"><span data-stu-id="13e85-112">Install hello latest version using hello [Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376).</span></span> <span data-ttu-id="13e85-113">Részletes információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="13e85-113">For detailed information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="13e85-114">[NuGet parancssori segédprogram](https://nuget.org/nuget.exe) használt tooinstall hello rugalmas adatbázis-feladatok csomagja.</span><span class="sxs-lookup"><span data-stu-id="13e85-114">[NuGet Command-line Utility](https://nuget.org/nuget.exe) is used tooinstall hello Elastic Database jobs package.</span></span> <span data-ttu-id="13e85-115">További információkért lásd: http://docs.nuget.org/docs/start-here/installing-nuget.</span><span class="sxs-lookup"><span data-stu-id="13e85-115">For more information, see http://docs.nuget.org/docs/start-here/installing-nuget.</span></span>

## <a name="download-and-import-hello-elastic-database-jobs-powershell-package"></a><span data-ttu-id="13e85-116">Töltse le és hello rugalmas feladatok PowerShell csomag importálása</span><span class="sxs-lookup"><span data-stu-id="13e85-116">Download and import hello Elastic Database jobs PowerShell package</span></span>
1. <span data-ttu-id="13e85-117">Indítsa el a Microsoft Azure PowerShell-parancsablakot, és keresse meg a toohello könyvtárat, amelybe letöltötte NuGet parancssori segédprogram (nuget.exe).</span><span class="sxs-lookup"><span data-stu-id="13e85-117">Launch Microsoft Azure PowerShell command window and navigate toohello directory where you downloaded NuGet Command-line Utility (nuget.exe).</span></span>
2. <span data-ttu-id="13e85-118">Töltse le és importálja **rugalmas adatbázis-feladatok** könyvtárba, amely hello aktuális hello a következő parancs a csomag:</span><span class="sxs-lookup"><span data-stu-id="13e85-118">Download and import **Elastic Database jobs** package into hello current directory with hello following command:</span></span>
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    <span data-ttu-id="13e85-119">Hello **rugalmas adatbázis-feladatok** fájl hello helyi könyvtárban nevű mappába kerül **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** ahol *x.x.xxxx.x* hello verziószám által adott jelentéseket tükrözik.</span><span class="sxs-lookup"><span data-stu-id="13e85-119">hello **Elastic Database jobs** files are placed in hello local directory in a folder named **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** where *x.x.xxxx.x* reflects hello version number.</span></span> <span data-ttu-id="13e85-120">hello PowerShell-parancsmagok (beleértve a szükséges ügyfél .dll) találhatók hello **tools\ElasticDatabaseJobs** alkönyvtárát és hello PowerShell parancsfájlok tooinstall, frissítése és eltávolítása is találhatók hello  **eszközök** alkönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="13e85-120">hello PowerShell cmdlets (including required client .dlls) are located in hello **tools\ElasticDatabaseJobs** sub-directory and hello PowerShell scripts tooinstall, upgrade and uninstall also reside in hello **tools** sub-directory.</span></span>
3. <span data-ttu-id="13e85-121">Keresse meg a toohello eszközök alkönyvtárát hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x mappa alatt írja be a cd-eszközök, például:</span><span class="sxs-lookup"><span data-stu-id="13e85-121">Navigate toohello tools sub-directory under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x folder by typing cd tools, for example:</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. <span data-ttu-id="13e85-122">Végrehajtás hello.\InstallElasticDatabaseJobsCmdlets.ps1 parancsfájl toocopy hello ElasticDatabaseJobs directory $home\Documents\WindowsPowerShell\Modules be.</span><span class="sxs-lookup"><span data-stu-id="13e85-122">Execute hello .\InstallElasticDatabaseJobsCmdlets.ps1 script toocopy hello ElasticDatabaseJobs directory into $home\Documents\WindowsPowerShell\Modules.</span></span> <span data-ttu-id="13e85-123">Ez is automatikusan importálni fogja hello modul használható, például:</span><span class="sxs-lookup"><span data-stu-id="13e85-123">This will also automatically import hello module for use, for example:</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-hello-elastic-database-jobs-components-using-powershell"></a><span data-ttu-id="13e85-124">PowerShell-lel hello rugalmas adatbázis-feladatok összetevőinek telepítése</span><span class="sxs-lookup"><span data-stu-id="13e85-124">Install hello Elastic Database jobs components using PowerShell</span></span>
1. <span data-ttu-id="13e85-125">Indítsa el a Microsoft Azure PowerShell-parancsablakot, és keresse meg a toohello \tools alkönyvtárát hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x mappában: írja be a cd \tools</span><span class="sxs-lookup"><span data-stu-id="13e85-125">Launch a Microsoft Azure PowerShell command window and navigate toohello \tools sub-directory under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x folder: Type cd \tools</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. <span data-ttu-id="13e85-126">Hello.\InstallElasticDatabaseJobs.ps1 PowerShell parancsfájlt, és adja meg a kért változók értékeit.</span><span class="sxs-lookup"><span data-stu-id="13e85-126">Execute hello .\InstallElasticDatabaseJobs.ps1 PowerShell script and supply values for its requested variables.</span></span> <span data-ttu-id="13e85-127">Ezt a parancsfájlt hoz létre hello ismertetett összetevőkön [rugalmas feladatok összetevők és az árképzés terén](sql-database-elastic-jobs-overview.md#components-and-pricing) hello Azure Cloud Service konfigurálása mellett tooappropriately hello függő összetevőket használnak.</span><span class="sxs-lookup"><span data-stu-id="13e85-127">This script will create hello components described in [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing) along with configuring hello Azure Cloud Service tooappropriately use hello dependent components.</span></span>

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

<span data-ttu-id="13e85-128">A parancs futtatásakor egy ablak megnyitása megadását kéri a **felhasználónév** és **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="13e85-128">When you run this command a window opens asking for a **user name** and **password**.</span></span> <span data-ttu-id="13e85-129">Ez nem az Azure hitelesítő adatait, hello felhasználónevet és jelszót, amely hello rendszergazdai hitelesítő adatokat szeretne toocreate hello új kiszolgáló lesz.</span><span class="sxs-lookup"><span data-stu-id="13e85-129">This is not your Azure credentials, enter hello user name and password that will be hello administrator credentials you want toocreate for hello new server.</span></span>

<span data-ttu-id="13e85-130">a minta betöltéshez megadott hello paraméterek módosíthatók a kívánt beállítást.</span><span class="sxs-lookup"><span data-stu-id="13e85-130">hello parameters provided on this sample invocation can be modified for your desired settings.</span></span> <span data-ttu-id="13e85-131">hello következő hello viselkedés minden paraméter nyújt részletesebb információt:</span><span class="sxs-lookup"><span data-stu-id="13e85-131">hello following provides more information on hello behavior of each parameter:</span></span>

<table style="width:100%">
  <tr>
    <th><span data-ttu-id="13e85-132">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13e85-132">Parameter</span></span></th>
    <th><span data-ttu-id="13e85-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="13e85-133">Description</span></span></th>
  </tr>

<tr>
    <td><span data-ttu-id="13e85-134">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="13e85-134">ResourceGroupName</span></span></td>
    <td><span data-ttu-id="13e85-135">Hello Azure erőforráscsoport-név létrehozása az újonnan létrehozott Azure összetevők toocontain hello biztosít.</span><span class="sxs-lookup"><span data-stu-id="13e85-135">Provides hello Azure resource group name created toocontain hello newly created Azure components.</span></span> <span data-ttu-id="13e85-136">Ez a paraméter alapértelmezett értéke túl "__ElasticDatabaseJob".</span><span class="sxs-lookup"><span data-stu-id="13e85-136">This parameter defaults too“__ElasticDatabaseJob”.</span></span> <span data-ttu-id="13e85-137">Nem ajánlott toochange ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="13e85-137">It is not recommended toochange this value.</span></span></td>
    </tr>

</tr>

    <tr>
    <td><span data-ttu-id="13e85-138">ResourceGroupLocation</span><span class="sxs-lookup"><span data-stu-id="13e85-138">ResourceGroupLocation</span></span></td>
    <td><span data-ttu-id="13e85-139">Az újonnan létrehozott Azure összetevők hello használt hello Azure-beli hely toobe biztosít.</span><span class="sxs-lookup"><span data-stu-id="13e85-139">Provides hello Azure location toobe used for hello newly created Azure components.</span></span> <span data-ttu-id="13e85-140">Ez a paraméter toohello USA középső RÉGIÓJA hely alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="13e85-140">This parameter defaults toohello Central US location.</span></span></td>
</tr>

<tr>
    <td><span data-ttu-id="13e85-141">ServiceWorkerCount</span><span class="sxs-lookup"><span data-stu-id="13e85-141">ServiceWorkerCount</span></span></td>
    <td><span data-ttu-id="13e85-142">Megadja a szolgáltatás munkavállalók tooinstall hello számát.</span><span class="sxs-lookup"><span data-stu-id="13e85-142">Provides hello number of service workers tooinstall.</span></span> <span data-ttu-id="13e85-143">Ez a paraméter alapértelmezett értéke: too1.</span><span class="sxs-lookup"><span data-stu-id="13e85-143">This parameter defaults too1.</span></span> <span data-ttu-id="13e85-144">Feldolgozók nagyobb számú használt tooscale hello szolgáltatást és tooprovide magas rendelkezésre állású ki lehet.</span><span class="sxs-lookup"><span data-stu-id="13e85-144">A higher number of workers can be used tooscale out hello service and tooprovide high availability.</span></span> <span data-ttu-id="13e85-145">"2" toouse hello szolgáltatás nagy rendelkezésre állást igénylő telepítésekhez javasolt.</span><span class="sxs-lookup"><span data-stu-id="13e85-145">It is recommended toouse “2” for deployments that require high availability of hello service.</span></span></td>
    </tr>

</tr>
    <tr>
    <td><span data-ttu-id="13e85-146">ServiceVmSize</span><span class="sxs-lookup"><span data-stu-id="13e85-146">ServiceVmSize</span></span></td>
    <td><span data-ttu-id="13e85-147">Hello Felhőszolgáltatás-felhasználásról hello Virtuálisgép-méretet biztosít.</span><span class="sxs-lookup"><span data-stu-id="13e85-147">Provides hello VM size for usage within hello Cloud Service.</span></span> <span data-ttu-id="13e85-148">Ez a paraméter alapértelmezett értéke: tooA0.</span><span class="sxs-lookup"><span data-stu-id="13e85-148">This parameter defaults tooA0.</span></span> <span data-ttu-id="13e85-149">A0/A1/A2 A3 paraméterek értékeit okozó hello feldolgozói szerepkör toouse egy: ExtraSmall/kis/közepes vagy nagy mérete, illetve elfogadottak.</span><span class="sxs-lookup"><span data-stu-id="13e85-149">Parameters values of A0/A1/A2/A3 are accepted which cause hello worker role toouse an ExtraSmall/Small/Medium/Large size, respectively.</span></span> <span data-ttu-id="13e85-150">Fő feldolgozói szerepkör méretét, a további tudnivalókat lásd a [rugalmas feladatok összetevők és az árképzés terén](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="13e85-150">Fo more information on worker role sizes, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="13e85-151">SqlServerDatabaseSlo</span><span class="sxs-lookup"><span data-stu-id="13e85-151">SqlServerDatabaseSlo</span></span></td>
    <td><span data-ttu-id="13e85-152">Standard edition hello szolgáltatásiszint-célkitűzés biztosít.</span><span class="sxs-lookup"><span data-stu-id="13e85-152">Provides hello service level objective for a Standard edition.</span></span> <span data-ttu-id="13e85-153">Ez a paraméter alapértelmezett értéke: tooS0.</span><span class="sxs-lookup"><span data-stu-id="13e85-153">This parameter defaults tooS0.</span></span> <span data-ttu-id="13e85-154">S0/S1/S2 S3 paraméter értékének elfogadottak, aminek következtében az Azure SQL Database hello toouse hello megfelelő slo-t.</span><span class="sxs-lookup"><span data-stu-id="13e85-154">Parameter values of S0/S1/S2/S3 are accepted which cause hello Azure SQL Database toouse hello respective SLO.</span></span> <span data-ttu-id="13e85-155">Az SQL-adatbázis segítségével további információkért lásd: [rugalmas feladatok összetevők és az árképzés terén](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="13e85-155">For more information on SQL Database SLOs, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="13e85-156">SqlServerAdministratorUserName</span><span class="sxs-lookup"><span data-stu-id="13e85-156">SqlServerAdministratorUserName</span></span></td>
    <td><span data-ttu-id="13e85-157">Itt hello rendszergazda felhasználóneve hello az újonnan létrehozott az Azure SQL Database-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="13e85-157">Provides hello admin user name for hello newly created Azure SQL Database server.</span></span> <span data-ttu-id="13e85-158">Ha nincs megadva, PowerShell hitelesítő adatok megnyílik egy tooprompt hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="13e85-158">When not specified, a PowerShell credentials window will open tooprompt for hello credentials.</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="13e85-159">SqlServerAdministratorPassword</span><span class="sxs-lookup"><span data-stu-id="13e85-159">SqlServerAdministratorPassword</span></span></td>
    <td><span data-ttu-id="13e85-160">Azure SQL adatbázis-kiszolgáló található, újonnan létrehozott hello hello rendszergazdai jelszó biztosít.</span><span class="sxs-lookup"><span data-stu-id="13e85-160">Provides hello admin password for hello newly created Azure SQL Database server.</span></span> <span data-ttu-id="13e85-161">Ha nincs megadva, PowerShell hitelesítő adatok megnyílik egy tooprompt hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="13e85-161">When not provided, a PowerShell credentials window will open tooprompt for hello credentials.</span></span></td>
</tr>
</table>

<span data-ttu-id="13e85-162">A megcélzó számos, az adatbázisok párhuzamosan futó feladatok nagy számú rendelkező rendszerek esetében javasolt toospecify paraméterek például: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.</span><span class="sxs-lookup"><span data-stu-id="13e85-162">For systems that target having large numbers of jobs running in parallel against a large number of databases, it is recommended toospecify parameters such as: -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2.</span></span>

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a><span data-ttu-id="13e85-163">Egy meglévő rugalmas feladatok összetevők telepítésének PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="13e85-163">Update an existing Elastic Database jobs components installation using PowerShell</span></span>
<span data-ttu-id="13e85-164">**Rugalmas adatbázis-feladatok** belül a méretezés és magas rendelkezésre állású egy példánya lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="13e85-164">**Elastic Database jobs** can be updated within an existing installation for scale and high-availability.</span></span> <span data-ttu-id="13e85-165">Ez a folyamat lehetővé teszi, hogy a szolgáltatás kód későbbi verziófrissítések toodrop nélkül, és hozza létre a feladatvezérlő adatbázishoz hello.</span><span class="sxs-lookup"><span data-stu-id="13e85-165">This process allows for future upgrades of service code without having toodrop and recreate hello control database.</span></span> <span data-ttu-id="13e85-166">Ez a folyamat is hello belül azonos toomodify hello szolgáltatás virtuális gép méretét vagy a hello kiszolgáló munkavégző verziószám.</span><span class="sxs-lookup"><span data-stu-id="13e85-166">This process can also be used within hello same version toomodify hello service VM size or hello server worker count.</span></span>

<span data-ttu-id="13e85-167">tooupdate hello Virtuálisgép-méretet egy telepítés, a következő paraméterekkel rendelkező parancsfájl futtatási hello frissítése az Ön által választott toohello értékeket.</span><span class="sxs-lookup"><span data-stu-id="13e85-167">tooupdate hello VM size of an installation, run hello following script with parameters updated toohello values of your choice.</span></span>

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th><span data-ttu-id="13e85-168">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13e85-168">Parameter</span></span></th>
  <th><span data-ttu-id="13e85-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="13e85-169">Description</span></span></th>
</tr>

  <tr>
    <td><span data-ttu-id="13e85-170">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="13e85-170">ResourceGroupName</span></span></td>
    <td><span data-ttu-id="13e85-171">Hello Azure erőforráscsoport-név használható, ha kezdetben megtörtént-e hello rugalmas feladat összetevők azonosítja.</span><span class="sxs-lookup"><span data-stu-id="13e85-171">Identifies hello Azure resource group name used when hello Elastic Database job components were initially installed.</span></span> <span data-ttu-id="13e85-172">Ez a paraméter alapértelmezett értéke túl "__ElasticDatabaseJob".</span><span class="sxs-lookup"><span data-stu-id="13e85-172">This parameter defaults too“__ElasticDatabaseJob”.</span></span> <span data-ttu-id="13e85-173">Mivel nem ajánlott toochange ezt az értéket kellett volna toospecify ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="13e85-173">Since it is not recommended toochange this value, you shouldn't have toospecify this parameter.</span></span></td>
    </tr>
</tr>

</tr>

  <tr>
    <td><span data-ttu-id="13e85-174">ServiceWorkerCount</span><span class="sxs-lookup"><span data-stu-id="13e85-174">ServiceWorkerCount</span></span></td>
    <td><span data-ttu-id="13e85-175">Megadja a szolgáltatás munkavállalók tooinstall hello számát.</span><span class="sxs-lookup"><span data-stu-id="13e85-175">Provides hello number of service workers tooinstall.</span></span>  <span data-ttu-id="13e85-176">Ez a paraméter alapértelmezett értéke: too1.</span><span class="sxs-lookup"><span data-stu-id="13e85-176">This parameter defaults too1.</span></span>  <span data-ttu-id="13e85-177">Feldolgozók nagyobb számú használt tooscale hello szolgáltatást és tooprovide magas rendelkezésre állású ki lehet.</span><span class="sxs-lookup"><span data-stu-id="13e85-177">A higher number of workers can be used tooscale out hello service and tooprovide high availability.</span></span>  <span data-ttu-id="13e85-178">"2" toouse hello szolgáltatás nagy rendelkezésre állást igénylő telepítésekhez javasolt.</span><span class="sxs-lookup"><span data-stu-id="13e85-178">It is recommended toouse “2” for deployments that require high availability of hello service.</span></span></td>
</tr>

</tr>

    <tr>
    <td><span data-ttu-id="13e85-179">ServiceVmSize</span><span class="sxs-lookup"><span data-stu-id="13e85-179">ServiceVmSize</span></span></td>
    <td><span data-ttu-id="13e85-180">Hello Felhőszolgáltatás-felhasználásról hello Virtuálisgép-méretet biztosít.</span><span class="sxs-lookup"><span data-stu-id="13e85-180">Provides hello VM size for usage within hello Cloud Service.</span></span> <span data-ttu-id="13e85-181">Ez a paraméter alapértelmezett értéke: tooA0.</span><span class="sxs-lookup"><span data-stu-id="13e85-181">This parameter defaults tooA0.</span></span> <span data-ttu-id="13e85-182">A0/A1/A2 A3 paraméterek értékeit okozó hello feldolgozói szerepkör toouse egy: ExtraSmall/kis/közepes vagy nagy mérete, illetve elfogadottak.</span><span class="sxs-lookup"><span data-stu-id="13e85-182">Parameters values of A0/A1/A2/A3 are accepted which cause hello worker role toouse an ExtraSmall/Small/Medium/Large size, respectively.</span></span> <span data-ttu-id="13e85-183">Fő feldolgozói szerepkör méretét, a további tudnivalókat lásd a [rugalmas feladatok összetevők és az árképzés terén](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="13e85-183">Fo more information on worker role sizes, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</table>

## <a name="install-hello-elastic-database-jobs-components-using-hello-portal"></a><span data-ttu-id="13e85-184">Hello Portal használatával hello rugalmas adatbázis-feladatok összetevőinek telepítése</span><span class="sxs-lookup"><span data-stu-id="13e85-184">Install hello Elastic Database jobs components using hello Portal</span></span>
<span data-ttu-id="13e85-185">Ha elvégezte [létrehozott egy rugalmas készlet](sql-database-elastic-pool-manage-portal.md), telepítése **rugalmas adatbázis-feladatok** összetevők tooenable hello rugalmas készlet minden egyes adatbázison felügyeleti feladatok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="13e85-185">Once you have [created an elastic pool](sql-database-elastic-pool-manage-portal.md), you can install **Elastic Database jobs** components tooenable execution of administrative tasks against each database in hello elastic pool.</span></span> <span data-ttu-id="13e85-186">Ha használatával hello eltérően **rugalmas adatbázis-feladatok** PowerShell API-k, hello portál illesztő egy meglévő készlet végrehajtása jelenleg korlátozott tooonly.</span><span class="sxs-lookup"><span data-stu-id="13e85-186">Unlike when using hello **Elastic Database jobs** PowerShell APIs, hello portal interface is currently restricted tooonly executing against an existing pool.</span></span>

<span data-ttu-id="13e85-187">**Becsült idő toocomplete:** 10 perc.</span><span class="sxs-lookup"><span data-stu-id="13e85-187">**Estimated time toocomplete:** 10 minutes.</span></span>

1. <span data-ttu-id="13e85-188">Az irányítópult-nézet hello hello rugalmas készlet keresztül hello [Azure Portal](https://portal.azure.com/#) , kattintson a **létrehozási feladat**.</span><span class="sxs-lookup"><span data-stu-id="13e85-188">From hello dashboard view of hello elastic pool via hello [Azure Portal](https://portal.azure.com/#) , click **Create job**.</span></span>
2. <span data-ttu-id="13e85-189">Ha hoz létre egy feladatot az hello először, telepítenie kell **rugalmas adatbázis-feladatok** kattintva **PREVIEW feltételek**.</span><span class="sxs-lookup"><span data-stu-id="13e85-189">If you are creating a job for hello first time, you must install **Elastic Database jobs** by clicking **PREVIEW TERMS**.</span></span>
3. <span data-ttu-id="13e85-190">Fogadja el a hello feltételek kattintva hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="13e85-190">Accept hello terms by clicking hello checkbox.</span></span>
4. <span data-ttu-id="13e85-191">A "telepítés szolgáltatások" hello nézetben kattintson **feladat hitelesítő adatai**.</span><span class="sxs-lookup"><span data-stu-id="13e85-191">In hello "Install services" view, click **JOB CREDENTIALS**.</span></span>
   
    ![Hello szolgáltatások telepítése][1]
5. <span data-ttu-id="13e85-193">Írjon be egy felhasználónevet és jelszót egy adatbázis-rendszergazdához. Hello telepítésének részeként egy új Azure SQL Database kiszolgáló akkor jön létre.</span><span class="sxs-lookup"><span data-stu-id="13e85-193">Type a user name and password for a database admin. As part of hello installation, a new Azure SQL Database server is created.</span></span> <span data-ttu-id="13e85-194">Az új kiszolgálón belül egy új adatbázist, hello feladatvezérlő adatbázishoz, néven jön létre, és toocontain hello metaadatokban használt rugalmas adatbázis-feladatok.</span><span class="sxs-lookup"><span data-stu-id="13e85-194">Within this new server, a new database, known as hello control database, is created and used toocontain hello meta data for Elastic Database jobs.</span></span> <span data-ttu-id="13e85-195">hello felhasználónevet és jelszót itt létrehozott hello célra toohello feladatvezérlő adatbázishoz bejelentkezéshez használhatók.</span><span class="sxs-lookup"><span data-stu-id="13e85-195">hello user name and password created here are used for hello purpose of logging in toohello control database.</span></span> <span data-ttu-id="13e85-196">Külön hitelesítő adatokat hello adatbázisokhoz belül hello parancsfájl végrehajtására szolgál.</span><span class="sxs-lookup"><span data-stu-id="13e85-196">A separate credential is used for script execution against hello databases within hello pool.</span></span>
   
    ![Felhasználónevet és jelszót hozhat létre][2]
6. <span data-ttu-id="13e85-198">Hello OK gombra.</span><span class="sxs-lookup"><span data-stu-id="13e85-198">Click hello OK button.</span></span> <span data-ttu-id="13e85-199">hello összetevők, a rendszer létrehozza az új néhány perc múlva [erőforráscsoport](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="13e85-199">hello components are created for you in a few minutes in a new [Resource group](../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="13e85-200">Új erőforráscsoport hello van rögzítve toohello üzenőfalon, indítható el, mert a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="13e85-200">hello new resource group is pinned toohello start board, as shown below.</span></span> <span data-ttu-id="13e85-201">Létrehozás után, a rugalmas adatbázis-feladatok (felhőalapú szolgáltatás, SQL-adatbázis, a Service Bus és tárolás) hello csoport létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="13e85-201">Once created, elastic database jobs (Cloud Service, SQL Database, Service Bus, and Storage) are all created in hello group.</span></span>
   
    ![a start Bizottsága erőforráscsoport][3]
7. <span data-ttu-id="13e85-203">Ha toocreate kísérlet vagy kezelése egy feladatot, amikor a rugalmas feladatok telepíti, így **hitelesítő adatok** hello a következő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="13e85-203">If you attempt toocreate or manage a job while elastic database jobs is installing, when providing **Credentials** you will see hello following message.</span></span>
   
    ![A folyamatban lévő telepítés][4]

<span data-ttu-id="13e85-205">Ha az Eltávolítás szükség, hello csoport törléséhez.</span><span class="sxs-lookup"><span data-stu-id="13e85-205">If uninstallation is required, delete hello resource group.</span></span> <span data-ttu-id="13e85-206">Lásd: [hogyan feladat toouninstall hello rugalmas adatbázis-összetevőinek](sql-database-elastic-jobs-uninstall.md).</span><span class="sxs-lookup"><span data-stu-id="13e85-206">See [How toouninstall hello Elastic Database job components](sql-database-elastic-jobs-uninstall.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="13e85-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13e85-207">Next steps</span></span>
<span data-ttu-id="13e85-208">A parancsfájl végrehajtása az összes adatbázisra hello csoportban, további információ: jön létre, győződjön meg arról, hogy hello megfelelő jogosultsággal rendelkező hitelesítő adatot [SQL-adatbázisok védelme](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="13e85-208">Ensure a credential with hello appropriate rights for script execution is created on each database in hello group, for more information see [Securing your SQL Database](sql-database-manage-logins.md).</span></span>
<span data-ttu-id="13e85-209">Lásd: [létrehozása és egy rugalmas adatbázis-feladatok kezelése](sql-database-elastic-jobs-create-and-manage.md) tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="13e85-209">See [Creating and managing an Elastic Database jobs](sql-database-elastic-jobs-create-and-manage.md) tooget started.</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
