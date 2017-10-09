---
title: "vegyes egyesítéses szolgáltatás aaaDeploy |} Microsoft Docs"
description: "A felosztás és rugalmas adatbáziseszközöket egyesítés"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 9a993c0f-7052-46cd-aa59-073bea8d535a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 6fef641cbc1e73ce34a851c53298a072dade8f9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-split-merge-service"></a><span data-ttu-id="57b18-103">Felosztási-egyesítési szolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="57b18-103">Deploy a split-merge service</span></span>
<span data-ttu-id="57b18-104">hello vegyes egyesítéses eszköz lehetővé teszi az adatok áthelyezése a szilánkos adatbázisok között.</span><span class="sxs-lookup"><span data-stu-id="57b18-104">hello split-merge tool lets you move data between sharded databases.</span></span> <span data-ttu-id="57b18-105">Lásd: [adatok kiterjesztett felhő adatbázisok közötti áthelyezése](sql-database-elastic-scale-overview-split-and-merge.md)</span><span class="sxs-lookup"><span data-stu-id="57b18-105">See [Moving data between scaled-out cloud databases](sql-database-elastic-scale-overview-split-and-merge.md)</span></span>

## <a name="download-hello-split-merge-packages"></a><span data-ttu-id="57b18-106">Hello vegyes egyesítéses csomagok letöltése</span><span class="sxs-lookup"><span data-stu-id="57b18-106">Download hello Split-Merge packages</span></span>
1. <span data-ttu-id="57b18-107">Töltse le a NuGet legfrissebb hello a [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="57b18-107">Download hello latest NuGet version from [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>
2. <span data-ttu-id="57b18-108">Nyisson meg egy parancssort, és keresse meg a toohello könyvtárat, amelybe letöltötte nuget.exe.</span><span class="sxs-lookup"><span data-stu-id="57b18-108">Open a command prompt and navigate toohello directory where you downloaded nuget.exe.</span></span> <span data-ttu-id="57b18-109">hello letöltése a PowerShell commmands tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="57b18-109">hello download includes PowerShell commmands.</span></span>
3. <span data-ttu-id="57b18-110">Töltse le a hello legújabb vegyes egyesítéses csomagot az alábbi parancs hello hello aktuális könyvtárba:</span><span class="sxs-lookup"><span data-stu-id="57b18-110">Download hello latest Split-Merge package into hello current directory with hello below command:</span></span>
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

<span data-ttu-id="57b18-111">hello fájlok kerülnek nevű **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** ahol *x.x.xxx.x* hello verziószám tükrözi.</span><span class="sxs-lookup"><span data-stu-id="57b18-111">hello files are placed in a directory named **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** where *x.x.xxx.x* reflects hello version number.</span></span> <span data-ttu-id="57b18-112">Található hello vegyes egyesítéses szolgáltatásfájlokat hello **content\splitmerge\service** alkönyvtára, és hello vegyes egyesítéses PowerShell-parancsfájlok (és a szükséges ügyfél .dll) a hello **content\splitmerge\powershell** alkönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="57b18-112">Find hello split-merge Service files in hello **content\splitmerge\service** sub-directory, and hello Split-Merge PowerShell scripts (and required client .dlls) in hello **content\splitmerge\powershell** sub-directory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="57b18-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="57b18-113">Prerequisites</span></span>
1. <span data-ttu-id="57b18-114">Hello vegyes egyesítéses állapot adatbázisként használható Azure SQL Database-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="57b18-114">Create an Azure SQL DB database that will be used as hello split-merge status database.</span></span> <span data-ttu-id="57b18-115">Nyissa meg toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="57b18-115">Go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="57b18-116">Hozzon létre egy új **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="57b18-116">Create a new **SQL Database**.</span></span> <span data-ttu-id="57b18-117">Nevezze el hello adatbázis, és hozzon létre egy új rendszergazda és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="57b18-117">Give hello database a name and create a new administrator and password.</span></span> <span data-ttu-id="57b18-118">Lehet, hogy toorecord hello nevét és jelszavát későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="57b18-118">Be sure toorecord hello name and password for later use.</span></span>
2. <span data-ttu-id="57b18-119">Győződjön meg arról, hogy az Azure SQL Database-kiszolgáló lehetővé teszi, hogy Azure Services tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="57b18-119">Ensure that your Azure SQL DB server allows Azure Services tooconnect tooit.</span></span> <span data-ttu-id="57b18-120">Hello portálon, a hello **tűzfalbeállítások**, győződjön meg arról, hogy hello **hozzáférést tooAzure szolgáltatások** beállítás értéke túl**a**.</span><span class="sxs-lookup"><span data-stu-id="57b18-120">In hello portal, in hello **Firewall Settings**, ensure that hello **Allow access tooAzure Services** setting is set too**On**.</span></span> <span data-ttu-id="57b18-121">Kattintson a "Mentés" ikonra hello.</span><span class="sxs-lookup"><span data-stu-id="57b18-121">Click hello "save" icon.</span></span>
   
   ![Engedélyezett szolgáltatások][1]
3. <span data-ttu-id="57b18-123">Azure-tárfiók létrehozása, amely jelzi a diagnosztikai kimenetet.</span><span class="sxs-lookup"><span data-stu-id="57b18-123">Create an Azure Storage account that will be used for diagnostics output.</span></span> <span data-ttu-id="57b18-124">Nyissa meg toohello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="57b18-124">Go toohello Azure Portal.</span></span> <span data-ttu-id="57b18-125">Hello bal oldali sávon kattintson **új**, kattintson a **adatok + tárolás**, majd **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="57b18-125">In hello left bar, click **New**, click **Data + Storage**, then **Storage**.</span></span>
4. <span data-ttu-id="57b18-126">Hozzon létre egy Azure felhőalapú szolgáltatás, amely a vegyes egyesítéses szolgáltatás fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="57b18-126">Create an Azure Cloud Service that will contain your Split-Merge service.</span></span>  <span data-ttu-id="57b18-127">Nyissa meg toohello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="57b18-127">Go toohello Azure Portal.</span></span> <span data-ttu-id="57b18-128">Hello bal oldali sávon kattintson **új**, majd **számítási**, **Felhőszolgáltatás**, és **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="57b18-128">In hello left bar, click **New**, then **Compute**, **Cloud Service**, and **Create**.</span></span> 

## <a name="configure-your-split-merge-service"></a><span data-ttu-id="57b18-129">A felosztott egyesítéses szolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="57b18-129">Configure your Split-Merge service</span></span>
### <a name="split-merge-service-configuration"></a><span data-ttu-id="57b18-130">Vegyes egyesítéses szolgáltatás konfigurációja</span><span class="sxs-lookup"><span data-stu-id="57b18-130">Split-Merge service configuration</span></span>
1. <span data-ttu-id="57b18-131">Hello mappát, ahova letöltötte hello vegyes egyesítéses szerelvényeket, hozzon létre hello másolatát **ServiceConfiguration.Template.cscfg** fájl mellett szállított **SplitMergeService.cspkg** és adjon neki **ServiceConfiguration.cscfg**.</span><span class="sxs-lookup"><span data-stu-id="57b18-131">In hello folder where you downloaded hello Split-Merge assemblies, create a copy of hello **ServiceConfiguration.Template.cscfg** file that shipped alongside **SplitMergeService.cspkg** and rename it **ServiceConfiguration.cscfg**.</span></span>
2. <span data-ttu-id="57b18-132">Nyissa meg **ServiceConfiguration.cscfg** egy szövegszerkesztőben, például a Visual Studio, amely a bemeneti adatok, például a tanúsítvány-ujjlenyomatok hello formátuma.</span><span class="sxs-lookup"><span data-stu-id="57b18-132">Open **ServiceConfiguration.cscfg** in a text editor such as Visual Studio that validates inputs such as hello format of certificate thumbprints.</span></span>
3. <span data-ttu-id="57b18-133">Hozzon létre egy új adatbázist, vagy válasszon ki egy létező adatbázis tooserve hello állapot adatbázis vegyes-Merge műveletek legyen, és az adatbázis hello kapcsolat-karakterlánc beolvasása.</span><span class="sxs-lookup"><span data-stu-id="57b18-133">Create a new database or choose an existing database tooserve as hello status database for Split-Merge operations and retrieve hello connection string of that database.</span></span> 
   
   > [!IMPORTANT]
   > <span data-ttu-id="57b18-134">Jelenleg állapot-adatbázis hello hello Latin rendezést kell használnia (SQL\_Latin1\_általános\_CP1\_CI\_AS).</span><span class="sxs-lookup"><span data-stu-id="57b18-134">At this time, hello status database must use hello Latin  collation (SQL\_Latin1\_General\_CP1\_CI\_AS).</span></span> <span data-ttu-id="57b18-135">További információkért lásd: [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).</span><span class="sxs-lookup"><span data-stu-id="57b18-135">For more information, see [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).</span></span>
   >

   <span data-ttu-id="57b18-136">Az Azure SQL Database hello kapcsolati karakterlánc általában: hello űrlap:</span><span class="sxs-lookup"><span data-stu-id="57b18-136">With Azure SQL DB, hello connection string typically is of hello form:</span></span>
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. <span data-ttu-id="57b18-137">Adja meg a kapcsolati karakterlánc hello cscfg-fájl a mindkét hello **SplitMergeWeb** és **SplitMergeWorker** hello ElasticScaleMetadata beállítás szerepkör részei.</span><span class="sxs-lookup"><span data-stu-id="57b18-137">Enter this connection string in hello cscfg file in both hello **SplitMergeWeb** and **SplitMergeWorker** role sections in hello ElasticScaleMetadata setting.</span></span>
5. <span data-ttu-id="57b18-138">A hello **SplitMergeWorker** szerepkör, adjon meg egy érvényes kapcsolati karakterlánc tooAzure tárolási hello **WorkerRoleSynchronizationStorageAccountConnectionString** beállítást.</span><span class="sxs-lookup"><span data-stu-id="57b18-138">For hello **SplitMergeWorker** role, enter a valid connection string tooAzure storage for hello **WorkerRoleSynchronizationStorageAccountConnectionString** setting.</span></span>

### <a name="configure-security"></a><span data-ttu-id="57b18-139">Biztonság konfigurálása</span><span class="sxs-lookup"><span data-stu-id="57b18-139">Configure security</span></span>
<span data-ttu-id="57b18-140">Részletes utasítások tooconfigure hello biztonsági hello szolgáltatást, tekintse meg a toohello [vegyes egyesítéses biztonsági konfiguráció](sql-database-elastic-scale-split-merge-security-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="57b18-140">For detailed instructions tooconfigure hello security of hello service, refer toohello [Split-Merge security configuration](sql-database-elastic-scale-split-merge-security-configuration.md).</span></span>

<span data-ttu-id="57b18-141">Ebben az oktatóanyagban egy egyszerű próbatelepítés hello céljából a minimális konfigurációs készlet vonatkozik, lépés végre tooget hello szolgáltatás lépéseivel.</span><span class="sxs-lookup"><span data-stu-id="57b18-141">For hello purposes of a simple test deployment for this tutorial, a minimal set of configuration steps will be performed tooget hello service up and running.</span></span> <span data-ttu-id="57b18-142">Ezeket a lépéseket csak hello egy számítógép vagy fiók engedélyezése végrehajtása őket toocommunicate hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="57b18-142">These steps enable only hello one machine/account executing them toocommunicate with hello service.</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="57b18-143">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="57b18-143">Create a self-signed certificate</span></span>
<span data-ttu-id="57b18-144">Hozzon létre egy új könyvtárat és a könyvtár végrehajtás hello a következő parancs használatával egy [Visual Studio Developer-parancssorból](http://msdn.microsoft.com/library/ms229859.aspx) ablakban:</span><span class="sxs-lookup"><span data-stu-id="57b18-144">Create a new directory and from this directory execute hello following command using a [Developer Command Prompt for Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) window:</span></span>

   ```
    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

<span data-ttu-id="57b18-145">A jelszó tooprotect hello titkos kulcs kell adnia.</span><span class="sxs-lookup"><span data-stu-id="57b18-145">You are asked for a password tooprotect hello private key.</span></span> <span data-ttu-id="57b18-146">Adjon meg egy erős jelszót, és erősítse meg.</span><span class="sxs-lookup"><span data-stu-id="57b18-146">Enter a strong password and confirm it.</span></span> <span data-ttu-id="57b18-147">Majd kéri hello jelszó toobe még egyszer ezt követően használható.</span><span class="sxs-lookup"><span data-stu-id="57b18-147">You are then prompted for hello password toobe used once more after that.</span></span> <span data-ttu-id="57b18-148">Kattintson a **Igen** : hello end tooimport azt toohello megbízható hitelesítésszolgáltató hatóságok gyökérszintű tárolóban.</span><span class="sxs-lookup"><span data-stu-id="57b18-148">Click **Yes** at hello end tooimport it toohello Trusted Certification Authorities Root store.</span></span>

### <a name="create-a-pfx-file"></a><span data-ttu-id="57b18-149">A PFX-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="57b18-149">Create a PFX file</span></span>
<span data-ttu-id="57b18-150">Hajtható végre a következő parancsot a hello hello ugyanazon ablakra, ahol makecert végre lett hajtva; használja az Ön használt toocreate hello tanúsítvány hello ugyanazt a jelszót:</span><span class="sxs-lookup"><span data-stu-id="57b18-150">Execute hello following command from hello same window where makecert was executed; use hello same password that you used toocreate hello certificate:</span></span>

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-hello-client-certificate-into-hello-personal-store"></a><span data-ttu-id="57b18-151">Hello személyes tárolóba hello ügyféltanúsítvány importálása</span><span class="sxs-lookup"><span data-stu-id="57b18-151">Import hello client certificate into hello personal store</span></span>
1. <span data-ttu-id="57b18-152">A Windows Intézőt, kattintson duplán a **MyCert.pfx**.</span><span class="sxs-lookup"><span data-stu-id="57b18-152">In Windows Explorer, double-click **MyCert.pfx**.</span></span>
2. <span data-ttu-id="57b18-153">A hello **Tanúsítványimportáló varázsló** válasszon **aktuális felhasználó** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="57b18-153">In hello **Certificate Import Wizard** select **Current User** and click **Next**.</span></span>
3. <span data-ttu-id="57b18-154">Ellenőrizze a hello fájl elérési útját, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="57b18-154">Confirm hello file path and click **Next**.</span></span>
4. <span data-ttu-id="57b18-155">Írja be a hello jelszót, hagyja **tartalmazza az összes kiterjesztett tulajdonságok** be van jelölve, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="57b18-155">Type hello password, leave **Include all extended properties** checked and click **Next**.</span></span>
5. <span data-ttu-id="57b18-156">Hagyja **automatikusan választ hello tanúsítványtároló [...]**  be van jelölve, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="57b18-156">Leave **Automatically select hello certificate store[…]** checked and click **Next**.</span></span>
6. <span data-ttu-id="57b18-157">Kattintson a **Befejezés** és **OK**.</span><span class="sxs-lookup"><span data-stu-id="57b18-157">Click **Finish** and **OK**.</span></span>

### <a name="upload-hello-pfx-file-toohello-cloud-service"></a><span data-ttu-id="57b18-158">Hello PFX fájl toohello felhőszolgáltatás feltöltése</span><span class="sxs-lookup"><span data-stu-id="57b18-158">Upload hello PFX file toohello cloud service</span></span>
1. <span data-ttu-id="57b18-159">Nyissa meg toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="57b18-159">Go toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="57b18-160">Válassza ki **a felhőalapú szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="57b18-160">Select **Cloud Services**.</span></span>
3. <span data-ttu-id="57b18-161">Válassza ki a fent hello vegyes/egyesítés szolgáltatás számára létrehozott hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="57b18-161">Select hello cloud service you created above for hello Split/Merge service.</span></span>
4. <span data-ttu-id="57b18-162">Kattintson a **tanúsítványok** hello felső menüben.</span><span class="sxs-lookup"><span data-stu-id="57b18-162">Click **Certificates** on hello top menu.</span></span>
5. <span data-ttu-id="57b18-163">Kattintson a **feltöltése** a hello alsó sáv.</span><span class="sxs-lookup"><span data-stu-id="57b18-163">Click **Upload** in hello bottom bar.</span></span>
6. <span data-ttu-id="57b18-164">Válassza ki a hello PFX-fájlt, és írja be a hello a fenti ugyanazt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="57b18-164">Select hello PFX file and enter hello same password as above.</span></span>
7. <span data-ttu-id="57b18-165">Ezt követően másolása hello új bejegyzést hello lista hello tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="57b18-165">Once completed, copy hello certificate thumbprint from hello new entry in hello list.</span></span>

### <a name="update-hello-service-configuration-file"></a><span data-ttu-id="57b18-166">Hello szolgáltatás konfigurációs fájljának frissítése</span><span class="sxs-lookup"><span data-stu-id="57b18-166">Update hello service configuration file</span></span>
<span data-ttu-id="57b18-167">Illessze be a fenti másol hello ujjlenyomat-érték attribútum ezeket a beállításokat hello tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="57b18-167">Paste hello certificate thumbprint copied above into hello thumbprint/value attribute of these settings.</span></span>
<span data-ttu-id="57b18-168">A feldolgozói szerepkör hello:</span><span class="sxs-lookup"><span data-stu-id="57b18-168">For hello worker role:</span></span>
   ```
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

<span data-ttu-id="57b18-169">Hello webes szerepkör:</span><span class="sxs-lookup"><span data-stu-id="57b18-169">For hello web role:</span></span>

   ```
    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

<span data-ttu-id="57b18-170">Kérjük, vegye figyelembe, hogy az üzemi környezetek külön tanúsítványokat kell használni hello hitelesítésszolgáltató, a titkosításhoz, a kiszolgálói tanúsítvány és az ügyféltanúsítványok hello.</span><span class="sxs-lookup"><span data-stu-id="57b18-170">Please note that for production deployments separate certificates should be used for hello CA, for encryption, hello Server certificate and client certificates.</span></span> <span data-ttu-id="57b18-171">Ez a részletes utasításokért lásd: [biztonsági konfiguráció](sql-database-elastic-scale-split-merge-security-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="57b18-171">For detailed instructions on this, see [Security Configuration](sql-database-elastic-scale-split-merge-security-configuration.md).</span></span>

## <a name="deploy-your-service"></a><span data-ttu-id="57b18-172">A szolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="57b18-172">Deploy your service</span></span>
1. <span data-ttu-id="57b18-173">Nyissa meg toohello [Azure-portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="57b18-173">Go toohello [Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="57b18-174">Kattintson a hello **Felhőszolgáltatások** hello bal oldali lapon, és válassza ki a korábban létrehozott hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="57b18-174">Click hello **Cloud Services** tab on hello left, and select hello cloud service that you created earlier.</span></span>
3. <span data-ttu-id="57b18-175">Kattintson a **irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="57b18-175">Click **Dashboard**.</span></span>
4. <span data-ttu-id="57b18-176">Átmeneti környezet hello válasszon, majd kattintson a **töltse fel az új átmeneti üzembe helyezésének**.</span><span class="sxs-lookup"><span data-stu-id="57b18-176">Choose hello staging environment, then click **Upload a new staging deployment**.</span></span>
   
   ![Átmeneti][3]
5. <span data-ttu-id="57b18-178">A hello párbeszédpanelen adjon meg egy üzemelő példány címkéje.</span><span class="sxs-lookup"><span data-stu-id="57b18-178">In hello dialog box, enter a deployment label.</span></span> <span data-ttu-id="57b18-179">"Csomag", mind a "Konfiguráció" részen kattintson "A helyi", és válassza ki a hello **SplitMergeService.cspkg** fájl- és a .cscfg fájlban korábban megadott értékektől.</span><span class="sxs-lookup"><span data-stu-id="57b18-179">For both 'Package' and 'Configuration', click 'From Local' and choose hello **SplitMergeService.cspkg** file and your .cscfg file that you configured earlier.</span></span>
6. <span data-ttu-id="57b18-180">Győződjön meg arról, hogy hello feliratú jelölőnégyzet **üzembe helyezés akkor is, ha egy vagy több szerepkör egyetlen példányt tartalmaz** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="57b18-180">Ensure that hello checkbox labeled **Deploy even if one or more roles contain a single instance** is checked.</span></span>
7. <span data-ttu-id="57b18-181">Találati hello osztásjelek gomb hello alsó jobb toobegin hello telepítésben.</span><span class="sxs-lookup"><span data-stu-id="57b18-181">Hit hello tick button in hello bottom right toobegin hello deployment.</span></span> <span data-ttu-id="57b18-182">Tehát az tootake néhány perc toocomplete.</span><span class="sxs-lookup"><span data-stu-id="57b18-182">Expect it tootake a few minutes toocomplete.</span></span>

   ![Feltöltés][4]

## <a name="troubleshoot-hello-deployment"></a><span data-ttu-id="57b18-184">Hello telepítési hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="57b18-184">Troubleshoot hello deployment</span></span>
<span data-ttu-id="57b18-185">A webes szerepkör toocome online nem sikerül, esetén valószínűleg probléma hello biztonsági beállításaival.</span><span class="sxs-lookup"><span data-stu-id="57b18-185">If your web role fails toocome online, it is likely a problem with hello security configuration.</span></span> <span data-ttu-id="57b18-186">Ellenőrizze, hogy hello SSL van konfigurálva a fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="57b18-186">Check that hello SSL is configured as described above.</span></span>

<span data-ttu-id="57b18-187">A feldolgozói szerepkör toocome online meghiúsul, de a webes szerepkör sikeres, esetén nagy valószínűséggel egy korábban létrehozott toohello állapot adatbázis kapcsolódás hibája.</span><span class="sxs-lookup"><span data-stu-id="57b18-187">If your worker role fails toocome online, but your web role succeeds, it is most likely a problem connecting toohello status database that you created earlier.</span></span>

* <span data-ttu-id="57b18-188">Győződjön meg arról, hogy a .cscfg hello kapcsolati karakterlánc pontos.</span><span class="sxs-lookup"><span data-stu-id="57b18-188">Make sure that hello connection string in your .cscfg is accurate.</span></span>
* <span data-ttu-id="57b18-189">Ellenőrizze, hogy hello server és adatbázis létezik, és hello felhasználóazonosító és jelszó helyességét.</span><span class="sxs-lookup"><span data-stu-id="57b18-189">Check that hello server and database exist, and that hello user id and password are correct.</span></span>
* <span data-ttu-id="57b18-190">Az Azure SQL Database Szolgáltatásnak hello kapcsolati karakterlánc kötelező forma: hello:</span><span class="sxs-lookup"><span data-stu-id="57b18-190">For Azure SQL DB, hello connection string should be of hello form:</span></span>

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* <span data-ttu-id="57b18-191">Győződjön meg arról, hogy hello kiszolgáló neve nem kezdődhet **https://**.</span><span class="sxs-lookup"><span data-stu-id="57b18-191">Ensure that hello server name does not begin with **https://**.</span></span>
* <span data-ttu-id="57b18-192">Győződjön meg arról, hogy az Azure SQL Database-kiszolgáló lehetővé teszi, hogy Azure Services tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="57b18-192">Ensure that your Azure SQL DB server allows Azure Services tooconnect tooit.</span></span> <span data-ttu-id="57b18-193">toodo, nyissa meg a https://manage.windowsazure.com, balra kattintson "SQL-adatbázisok" hello, hello tetején kattintson a "Kiszolgáló", és jelölje ki a kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="57b18-193">toodo this, open https://manage.windowsazure.com, click “SQL Databases” on hello left, click “Servers” at hello top, and select your server.</span></span> <span data-ttu-id="57b18-194">Kattintson a **konfigurálása** : hello top, és győződjön meg arról, hogy hello **Azure Services** beállítás értéke túl "Yes".</span><span class="sxs-lookup"><span data-stu-id="57b18-194">Click **Configure** at hello top and ensure that hello **Azure Services** setting is set too“Yes”.</span></span> <span data-ttu-id="57b18-195">(Lásd: hello Előfeltételek szakasz ebben a cikkben hello tetején).</span><span class="sxs-lookup"><span data-stu-id="57b18-195">(See hello Prerequisites section at hello top of this article).</span></span>

## <a name="test-hello-service-deployment"></a><span data-ttu-id="57b18-196">Hello szolgáltatás központi telepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="57b18-196">Test hello service deployment</span></span>
### <a name="connect-with-a-web-browser"></a><span data-ttu-id="57b18-197">Egy webböngészővel rendelkező csatlakozás</span><span class="sxs-lookup"><span data-stu-id="57b18-197">Connect with a web browser</span></span>
<span data-ttu-id="57b18-198">Határozza meg, hogy a vegyes egyesítéses szolgáltatás hello webes végpontjának.</span><span class="sxs-lookup"><span data-stu-id="57b18-198">Determine hello web endpoint of your Split-Merge service.</span></span> <span data-ttu-id="57b18-199">Ez az található hello klasszikus Azure portálon is toohello által **irányítópult** a felhőszolgáltatás és a keresése **webhely URL-címe** hello jobb oldalán.</span><span class="sxs-lookup"><span data-stu-id="57b18-199">You can find this in hello Azure Classic Portal by going toohello **Dashboard** of your cloud service and looking under **Site URL** on hello right side.</span></span> <span data-ttu-id="57b18-200">Cserélje le **http://** rendelkező **https://** óta hello alapértelmezett biztonsági beállítások letiltása hello HTTP-végpont.</span><span class="sxs-lookup"><span data-stu-id="57b18-200">Replace **http://** with **https://** since hello default security settings disable hello HTTP endpoint.</span></span> <span data-ttu-id="57b18-201">A böngészőbe az URL-cím hello oldal betöltése.</span><span class="sxs-lookup"><span data-stu-id="57b18-201">Load hello page for this URL into your browser.</span></span>

### <a name="test-with-powershell-scripts"></a><span data-ttu-id="57b18-202">A PowerShell-parancsfájlok tesztelése</span><span class="sxs-lookup"><span data-stu-id="57b18-202">Test with PowerShell scripts</span></span>
<span data-ttu-id="57b18-203">hello telepítési és a környezet által tartalmazott hello minta PowerShell-parancsfájlok futtatásakor tesztelhető.</span><span class="sxs-lookup"><span data-stu-id="57b18-203">hello deployment and your environment can be tested by running hello included sample PowerShell scripts.</span></span>

<span data-ttu-id="57b18-204">tartalmazott hello parancsprogramok a következők:</span><span class="sxs-lookup"><span data-stu-id="57b18-204">hello script files included are:</span></span>

1. <span data-ttu-id="57b18-205">**SetupSampleSplitMergeEnvironment.ps1** -állítja be a teszt adatrétegbeli vegyes/egyesítés (lásd az alábbi táblázatban részletes leírása)</span><span class="sxs-lookup"><span data-stu-id="57b18-205">**SetupSampleSplitMergeEnvironment.ps1** - sets up a test data tier for Split/Merge (see table below for detailed description)</span></span>
2. <span data-ttu-id="57b18-206">**ExecuteSampleSplitMerge.ps1** -hello teszt teszt műveleteket hajt végre adatok réteg (lásd az alábbi táblázatban részletes leírása)</span><span class="sxs-lookup"><span data-stu-id="57b18-206">**ExecuteSampleSplitMerge.ps1** - executes test operations on hello test data tier (see table below for detailed description)</span></span>
3. <span data-ttu-id="57b18-207">**GetMappings.ps1** – legfelső szintű mintaparancsfájl hello shard hozzárendelések hello aktuális állapotát megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="57b18-207">**GetMappings.ps1** - top-level sample script that prints out hello current state of hello shard mappings.</span></span>
4. <span data-ttu-id="57b18-208">**ShardManagement.psm1** -segítő parancsfájl nagyságúra hello ShardManagement API</span><span class="sxs-lookup"><span data-stu-id="57b18-208">**ShardManagement.psm1**  - helper script that wraps hello ShardManagement API</span></span>
5. <span data-ttu-id="57b18-209">**SqlDatabaseHelpers.psm1** -segítő parancsfájl létrehozásához és kezeléséhez az SQL-adatbázisok</span><span class="sxs-lookup"><span data-stu-id="57b18-209">**SqlDatabaseHelpers.psm1** - helper script for creating and managing SQL databases</span></span>
   
   <table style="width:100%">
     <tr>
       <th><span data-ttu-id="57b18-210">PowerShell-fájl</span><span class="sxs-lookup"><span data-stu-id="57b18-210">PowerShell file</span></span></th>
       <th><span data-ttu-id="57b18-211">Lépések</span><span class="sxs-lookup"><span data-stu-id="57b18-211">Steps</span></span></th>
     </tr>
     <tr>
       <th rowspan="5"><span data-ttu-id="57b18-212">SetupSampleSplitMergeEnvironment.ps1</span><span class="sxs-lookup"><span data-stu-id="57b18-212">SetupSampleSplitMergeEnvironment.ps1</span></span></th>
       <td>1.    <span data-ttu-id="57b18-213">Létrehoz egy shard térkép manager adatbázis</span><span class="sxs-lookup"><span data-stu-id="57b18-213">Creates a shard map manager database</span></span></td>
     </tr>
     <tr>
       <td>2.    <span data-ttu-id="57b18-214">2 shard-adatbázisokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="57b18-214">Creates 2 shard databases.</span></span>
     </tr>
     <tr>
       <td>3.    <span data-ttu-id="57b18-215">Ezen adatbázis (törli ezeket az adatbázisokat a maps bármely létező szilánkok) shard leképezést hoz létre.</span><span class="sxs-lookup"><span data-stu-id="57b18-215">Creates a shard map for those database (deletes any existing shard maps on those databases).</span></span> </td>
     </tr>
     <tr>
       <td>4.    <span data-ttu-id="57b18-216">Táblát hoz létre egy kis minta mindkét hello szilánkok, és feltölti a hello tábla hello szilánkok egyikében.</span><span class="sxs-lookup"><span data-stu-id="57b18-216">Creates a small sample table in both hello shards, and populates hello table in one of hello shards.</span></span></td>
     </tr>
     <tr>
       <td>5.    <span data-ttu-id="57b18-217">Deklarálja hello SchemaInfo hello szilánkos táblához.</span><span class="sxs-lookup"><span data-stu-id="57b18-217">Declares hello SchemaInfo for hello sharded table.</span></span></td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th><span data-ttu-id="57b18-218">PowerShell-fájl</span><span class="sxs-lookup"><span data-stu-id="57b18-218">PowerShell file</span></span></th>
       <th><span data-ttu-id="57b18-219">Lépések</span><span class="sxs-lookup"><span data-stu-id="57b18-219">Steps</span></span></th>
     </tr>
   <tr>
       <th rowspan="4"><span data-ttu-id="57b18-220">ExecuteSampleSplitMerge.ps1</span><span class="sxs-lookup"><span data-stu-id="57b18-220">ExecuteSampleSplitMerge.ps1</span></span> </th>
       <td>1.    <span data-ttu-id="57b18-221">Vegyes kérelem toohello vegyes egyesítéses szolgáltatás webes időtúllépést, amely felosztja a hello első shard toohello második shard fele hello adatokat küld.</span><span class="sxs-lookup"><span data-stu-id="57b18-221">Sends a split request toohello Split-Merge Service web frontend, which splits half hello data from hello first shard toohello second shard.</span></span></td>
     </tr>
     <tr>
       <td>2.    <span data-ttu-id="57b18-222">Szavazások hello webes előtér a hello ossza fel a kérelem állapotának és vár csak hello kérelem befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="57b18-222">Polls hello web frontend for hello split request status and waits until hello request completes.</span></span></td>
     </tr>
     <tr>
       <td>3.    <span data-ttu-id="57b18-223">Egyesítési kérelem toohello vegyes egyesítéses szolgáltatás webes időtúllépést, amely hello második shard hátsó toohello első shard helyez hello adatokat küld.</span><span class="sxs-lookup"><span data-stu-id="57b18-223">Sends a merge request toohello Split-Merge Service web frontend, which moves hello data from hello second shard back toohello first shard.</span></span></td>
     </tr>
     <tr>
       <td>4.    <span data-ttu-id="57b18-224">Szavazások hello webes előtér hello egyesítési kérelem állapotának és megvárja, amíg hello kérelem befejeződött.</span><span class="sxs-lookup"><span data-stu-id="57b18-224">Polls hello web frontend for hello merge request status and waits until hello request completes.</span></span></td>
     </tr>
   </table>
   
## <a name="use-powershell-tooverify-your-deployment"></a><span data-ttu-id="57b18-225">A központi telepítés PowerShell tooverify használata</span><span class="sxs-lookup"><span data-stu-id="57b18-225">Use PowerShell tooverify your deployment</span></span>
1. <span data-ttu-id="57b18-226">Új PowerShell ablakban keresse meg a toohello könyvtárat, amelybe letöltötte hello vegyes egyesítéses csomag és hello "powershell" könyvtárba, majd lépjen.</span><span class="sxs-lookup"><span data-stu-id="57b18-226">Open a new PowerShell window and navigate toohello directory where you downloaded hello Split-Merge package, and then navigate into hello “powershell” directory.</span></span>
2. <span data-ttu-id="57b18-227">Hozzon létre egy Azure SQL adatbázis-kiszolgáló (vagy válasszon egy meglévő kiszolgálót) ahol hello shard térkép manager és a szilánkok létrejön.</span><span class="sxs-lookup"><span data-stu-id="57b18-227">Create an Azure SQL database server (or choose an existing server) where hello shard map manager and shards will be created.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="57b18-228">hello SetupSampleSplitMergeEnvironment.ps1 parancsfájl ezeket az adatbázisokat hoz létre hello alapértelmezett tookeep hello parancsfájl egyszerű ugyanarra a kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="57b18-228">hello SetupSampleSplitMergeEnvironment.ps1 script creates all these databases on hello same server by default tookeep hello script simple.</span></span> <span data-ttu-id="57b18-229">Ez a korlátozás nem hello vegyes egyesítéses szolgáltatás a saját magát.</span><span class="sxs-lookup"><span data-stu-id="57b18-229">This is not a restriction of hello Split-Merge Service itself.</span></span>
   >
   
   <span data-ttu-id="57b18-230">Egy SQL hitelesítési-bejelentkezési az olvasási/írási hozzáférést toohello adatbázisok hello vegyes egyesítéses toomove szolgáltatásadatok és frissítési hello shard leképezés szükséges.</span><span class="sxs-lookup"><span data-stu-id="57b18-230">A SQL authentication login with read/write access toohello DBs will be needed for hello Split-Merge service toomove data and update hello shard map.</span></span> <span data-ttu-id="57b18-231">Hello felhő hello vegyes egyesítéses szolgáltatás fut, mert azt jelenleg nem támogatja integrált hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="57b18-231">Since hello Split-Merge Service runs in hello cloud, it does not currently support Integrated Authentication.</span></span>
   
   <span data-ttu-id="57b18-232">Ellenőrizze, hogy hello Azure SQL-kiszolgálót tooallow a hozzáférést a hello számítógépen, amelyen ezek a parancsfájlok hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="57b18-232">Make sure hello Azure SQL server is configured tooallow access from hello IP address of hello machine running these scripts.</span></span> <span data-ttu-id="57b18-233">Ez a beállítás hello Azure SQL-kiszolgálón található / configuration / engedélyezett IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="57b18-233">You can find this setting under hello Azure SQL server / configuration / allowed IP addresses.</span></span>
3. <span data-ttu-id="57b18-234">Hello SetupSampleSplitMergeEnvironment.ps1 toocreate hello minta parancsfájlkörnyezetet hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="57b18-234">Execute hello SetupSampleSplitMergeEnvironment.ps1 script toocreate hello sample environment.</span></span>
   
   <span data-ttu-id="57b18-235">A parancsfájl futtatása lesz kitöröl shard térkép meglévő felügyeleti adatokat hello shard manager adatbázist és hello szilánkok struktúrákat.</span><span class="sxs-lookup"><span data-stu-id="57b18-235">Running this script will wipe out any existing shard map management data structures on hello shard map manager database and hello shards.</span></span> <span data-ttu-id="57b18-236">Ha toore inicializálásának hello shard térkép vagy szilánkok lehet hasznos toorerun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="57b18-236">It may be useful toorerun hello script if you wish toore-initialize hello shard map or shards.</span></span>
   
   <span data-ttu-id="57b18-237">Minta parancssor:</span><span class="sxs-lookup"><span data-stu-id="57b18-237">Sample command line:</span></span>

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. <span data-ttu-id="57b18-238">Végrehajtás hello Getmappings.ps1 parancsfájl tooview hello hozzárendelések jelenleg létező hello minta környezetben.</span><span class="sxs-lookup"><span data-stu-id="57b18-238">Execute hello Getmappings.ps1 script tooview hello mappings that currently exist in hello sample environment.</span></span>
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. <span data-ttu-id="57b18-239">Hello ExecuteSampleSplitMerge.ps1 parancsfájl tooexecute hajtható végre, egy vegyes művelet (hello első shard toohello második shard helyezze át fele hello adatokat), majd egy partícióegyesítési művelet (adatmozgatás hello vissza alakzatot hello első szilánkok).</span><span class="sxs-lookup"><span data-stu-id="57b18-239">Execute hello ExecuteSampleSplitMerge.ps1 script tooexecute a split operation (moving half hello data on hello first shard toohello second shard) and then a merge operation (moving hello data back onto hello first shard).</span></span> <span data-ttu-id="57b18-240">Ha konfigurálta az SSL és a bal oldali hello http-végpont le van tiltva, győződjön meg arról, hello https:// végpont helyette használja.</span><span class="sxs-lookup"><span data-stu-id="57b18-240">If you configured SSL and left hello http endpoint disabled, ensure that you use hello https:// endpoint instead.</span></span>
   
   <span data-ttu-id="57b18-241">Minta parancssor:</span><span class="sxs-lookup"><span data-stu-id="57b18-241">Sample command line:</span></span>

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   <span data-ttu-id="57b18-242">Hiba alább hello kapja, esetén valószínűleg a webalkalmazás-végpontot tanúsítványával kapcsolatos problémára.</span><span class="sxs-lookup"><span data-stu-id="57b18-242">If you receive hello below error, it is most likely a problem with your Web endpoint’s certificate.</span></span> <span data-ttu-id="57b18-243">Próbáljon meg toohello webes végpontjának a kedvenc webböngészőt, és ellenőrizze, hogy a tanúsítvány hiba.</span><span class="sxs-lookup"><span data-stu-id="57b18-243">Try connecting toohello Web endpoint with your favorite Web browser and check if there is a certificate error.</span></span>
   
     ```
     Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLSsecure channel.
     ```
   
   <span data-ttu-id="57b18-244">Ha sikeres volt, hello kimeneti alábbi hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="57b18-244">If it succeeded, hello output should look like hello below:</span></span>
   
   ```
   > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' -CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
   > Sending split request
   > Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
   > Polling split-merge request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > Sending merge request
   > Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
   > Polling request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > 
   ```
6. <span data-ttu-id="57b18-245">Más adattípusok kísérletezhet!</span><span class="sxs-lookup"><span data-stu-id="57b18-245">Experiment with other data types!</span></span> <span data-ttu-id="57b18-246">Mindegyik parancsfájl vennie a választható - ShardKeyType paraméter, amely lehetővé teszi a toospecify hello kulcs típusa.</span><span class="sxs-lookup"><span data-stu-id="57b18-246">All of these scripts take an optional -ShardKeyType parameter that allows you toospecify hello key type.</span></span> <span data-ttu-id="57b18-247">hello alapértelmezett Int32, de azt is megadhatja Int64, Guid vagy bináris.</span><span class="sxs-lookup"><span data-stu-id="57b18-247">hello default is Int32, but you can also specify Int64, Guid, or Binary.</span></span>

## <a name="create-requests"></a><span data-ttu-id="57b18-248">Létrehozási kérelmek</span><span class="sxs-lookup"><span data-stu-id="57b18-248">Create requests</span></span>
<span data-ttu-id="57b18-249">hello szolgáltatás használható hello webes felhasználói felület használatával vagy importálni és hello SplitMerge.psm1 PowerShell-modult, amely a kéréseket keresztül hello webes szerepkör használatával.</span><span class="sxs-lookup"><span data-stu-id="57b18-249">hello service can be used either by using hello web UI or by importing and using hello SplitMerge.psm1 PowerShell module which will submit your requests through hello web role.</span></span>

<span data-ttu-id="57b18-250">hello szolgáltatás mozgathatja az adatokat a szilánkos táblák és a hivatkozási táblák.</span><span class="sxs-lookup"><span data-stu-id="57b18-250">hello service can move data in both sharded tables and reference tables.</span></span> <span data-ttu-id="57b18-251">A szilánkos táblához egy horizontális skálázási kulcsoszlopa, és különböző soradatok rendelkezzen minden szilánkcímtárban.</span><span class="sxs-lookup"><span data-stu-id="57b18-251">A sharded table has a sharding key column and has different row data on each shard.</span></span> <span data-ttu-id="57b18-252">Egy hivatkozási tábla nincs szilánkos hello tartalmaz, ugyanazt a sort adatok a minden szilánkcímtárban.</span><span class="sxs-lookup"><span data-stu-id="57b18-252">A reference table is not sharded so it contains hello same row data on every shard.</span></span> <span data-ttu-id="57b18-253">Hivatkozási táblák hasznosak, amely nem változik gyakran, és a lekérdezésekben szilánkos táblákkal használt tooJOIN adatokat.</span><span class="sxs-lookup"><span data-stu-id="57b18-253">Reference tables are useful for data that does not change often and is used tooJOIN with sharded tables in queries.</span></span>

<span data-ttu-id="57b18-254">A sorrend tooperform vegyes egyesítésével hello szilánkos táblák és hivatkozási táblák toohave áthelyezni kívánt kell deklarálnia.</span><span class="sxs-lookup"><span data-stu-id="57b18-254">In order tooperform a split-merge operation, you must declare hello sharded tables and reference tables that you want toohave moved.</span></span> <span data-ttu-id="57b18-255">Mindez a hello **SchemaInfo** API.</span><span class="sxs-lookup"><span data-stu-id="57b18-255">This is accomplished with hello **SchemaInfo** API.</span></span> <span data-ttu-id="57b18-256">Ez az API van hello **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** névtér.</span><span class="sxs-lookup"><span data-stu-id="57b18-256">This API is in hello **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** namespace.</span></span>

1. <span data-ttu-id="57b18-257">Minden szilánkos táblához, hozzon létre egy **ShardedTableInfo** hello tábla szülő sémanév leíró objektum (nem kötelező, alapértelmezés szerint használt érték túl "dbo"), táblanév hello és hello oszlopnév hello horizontális kulcsot tartalmazó tábla.</span><span class="sxs-lookup"><span data-stu-id="57b18-257">For each sharded table, create a **ShardedTableInfo** object describing hello table’s parent schema name (optional, defaults too“dbo”), hello table name, and hello column name in that table that contains hello sharding key.</span></span>
2. <span data-ttu-id="57b18-258">Minden egyes összefoglaló táblázatot is létrehozhat egy **ReferenceTableInfo** hello tábla szülő sémanév leíró objektum (nem kötelező, alapértelmezés szerint használt érték túl "dbo") és hello tábla neve.</span><span class="sxs-lookup"><span data-stu-id="57b18-258">For each reference table, create a **ReferenceTableInfo** object describing hello table’s parent schema name (optional, defaults too“dbo”) and hello table name.</span></span>
3. <span data-ttu-id="57b18-259">Adja hozzá a fenti TableInfo objektumok tooa új hello **SchemaInfo** objektum.</span><span class="sxs-lookup"><span data-stu-id="57b18-259">Add hello above TableInfo objects tooa new **SchemaInfo** object.</span></span>
4. <span data-ttu-id="57b18-260">Egy hivatkozási tooa beolvasása **ShardMapManager** objektum és hívás **GetSchemaInfoCollection**.</span><span class="sxs-lookup"><span data-stu-id="57b18-260">Get a reference tooa **ShardMapManager** object, and call **GetSchemaInfoCollection**.</span></span>
5. <span data-ttu-id="57b18-261">Adja hozzá a hello **SchemaInfo** toohello **SchemaInfoCollection**, hello shard leképezésnév biztosítása.</span><span class="sxs-lookup"><span data-stu-id="57b18-261">Add hello **SchemaInfo** toohello **SchemaInfoCollection**, providing hello shard map name.</span></span>

<span data-ttu-id="57b18-262">Például az hello SetupSampleSplitMergeEnvironment.ps1 parancsfájl látható.</span><span class="sxs-lookup"><span data-stu-id="57b18-262">An example of this can be seen in hello SetupSampleSplitMergeEnvironment.ps1 script.</span></span>

<span data-ttu-id="57b18-263">hello vegyes egyesítéses szolgáltatás nem hello céladatbázis (vagy bármely hello adatbázis tábláit sémáját) hozza létre.</span><span class="sxs-lookup"><span data-stu-id="57b18-263">hello Split-Merge service does not create hello target database (or schema for any tables in hello database) for you.</span></span> <span data-ttu-id="57b18-264">Fel kell előre létrehozott egy kérést toohello szolgáltatás elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="57b18-264">They must be pre-created before sending a request toohello service.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="57b18-265">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="57b18-265">Troubleshooting</span></span>
<span data-ttu-id="57b18-266">Hello minta powershell-parancsfájlok futtatásakor alatt üzenet hello jelenhetnek meg:</span><span class="sxs-lookup"><span data-stu-id="57b18-266">You may see hello below message when running hello sample powershell scripts:</span></span>

   ```
   Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLS secure channel.
   ```

<span data-ttu-id="57b18-267">Ez a hiba azt jelenti, hogy az SSL-tanúsítvány nincs megfelelően konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="57b18-267">This error means that your SSL certificate is not configured correctly.</span></span> <span data-ttu-id="57b18-268">Kérjük, kövesse a részben található útmutatást hello "Böngészővel csatlakozás".</span><span class="sxs-lookup"><span data-stu-id="57b18-268">Please follow hello instructions in section 'Connecting with a web browser'.</span></span>

<span data-ttu-id="57b18-269">Nem küldenek kéréseket jelenhet meg ezt:</span><span class="sxs-lookup"><span data-stu-id="57b18-269">If you cannot submit requests you may see this:</span></span>

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

<span data-ttu-id="57b18-270">Ebben az esetben ellenőrizze a konfigurációs fájlban, az adott hello beállítása **WorkerRoleSynchronizationStorageAccountConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="57b18-270">In this case, check your configuration file, in particular hello setting for **WorkerRoleSynchronizationStorageAccountConnectionString**.</span></span> <span data-ttu-id="57b18-271">Ez a hiba általában azt jelzi, hogy hello feldolgozói szerepkör sikeresen inicializálása hello metaadatokat tároló adatbázis első használatkor.</span><span class="sxs-lookup"><span data-stu-id="57b18-271">This error typically indicates that hello worker role could not successfully initialize hello metadata database on first use.</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png

