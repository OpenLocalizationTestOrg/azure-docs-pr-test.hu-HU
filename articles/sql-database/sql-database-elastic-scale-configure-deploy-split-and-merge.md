---
title: "Vegyes egyesítéses szolgáltatás telepítése |} Microsoft Docs"
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
ms.openlocfilehash: 6e2fea882c248fa095a9d450ed54a7b4e64b45e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-split-merge-service"></a><span data-ttu-id="66eab-103">Felosztási-egyesítési szolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="66eab-103">Deploy a split-merge service</span></span>
<span data-ttu-id="66eab-104">A felosztott egyesítéses eszköz lehetővé teszi az adatok áthelyezése a szilánkos adatbázisok között.</span><span class="sxs-lookup"><span data-stu-id="66eab-104">The split-merge tool lets you move data between sharded databases.</span></span> <span data-ttu-id="66eab-105">Lásd: [adatok kiterjesztett felhő adatbázisok közötti áthelyezése](sql-database-elastic-scale-overview-split-and-merge.md)</span><span class="sxs-lookup"><span data-stu-id="66eab-105">See [Moving data between scaled-out cloud databases](sql-database-elastic-scale-overview-split-and-merge.md)</span></span>

## <a name="download-the-split-merge-packages"></a><span data-ttu-id="66eab-106">A felosztott egyesítéses csomagok letöltése</span><span class="sxs-lookup"><span data-stu-id="66eab-106">Download the Split-Merge packages</span></span>
1. <span data-ttu-id="66eab-107">Töltse le a legfrissebb NuGet [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="66eab-107">Download the latest NuGet version from [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>
2. <span data-ttu-id="66eab-108">Nyisson meg egy parancssort, és keresse meg a könyvtárat, amelybe letöltötte nuget.exe.</span><span class="sxs-lookup"><span data-stu-id="66eab-108">Open a command prompt and navigate to the directory where you downloaded nuget.exe.</span></span> <span data-ttu-id="66eab-109">A letöltés PowerShell commmands tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="66eab-109">The download includes PowerShell commmands.</span></span>
3. <span data-ttu-id="66eab-110">Töltse le a legfrissebb vegyes egyesítéses csomagot be az aktuális könyvtár és az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="66eab-110">Download the latest Split-Merge package into the current directory with the below command:</span></span>
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

<span data-ttu-id="66eab-111">A fájlok kerülnek nevű **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** ahol *x.x.xxx.x* tükrözi a verziószámot.</span><span class="sxs-lookup"><span data-stu-id="66eab-111">The files are placed in a directory named **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** where *x.x.xxx.x* reflects the version number.</span></span> <span data-ttu-id="66eab-112">A felosztott egyesítéses szolgáltatásfájlokat található a **content\splitmerge\service** alkönyvtára, és a felosztott egyesítéses PowerShell parancsfájlok (és szükséges ügyfél .dll) a **content\splitmerge\powershell** alkönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="66eab-112">Find the split-merge Service files in the **content\splitmerge\service** sub-directory, and the Split-Merge PowerShell scripts (and required client .dlls) in the **content\splitmerge\powershell** sub-directory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66eab-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="66eab-113">Prerequisites</span></span>
1. <span data-ttu-id="66eab-114">A felosztott egyesítéses állapot adatbázisként használandó Azure SQL Database-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="66eab-114">Create an Azure SQL DB database that will be used as the split-merge status database.</span></span> <span data-ttu-id="66eab-115">Nyissa meg az [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="66eab-115">Go to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="66eab-116">Hozzon létre egy új **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="66eab-116">Create a new **SQL Database**.</span></span> <span data-ttu-id="66eab-117">Nevezze el az adatbázist, és hozzon létre egy új rendszergazda és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="66eab-117">Give the database a name and create a new administrator and password.</span></span> <span data-ttu-id="66eab-118">Ne felejtse el a név és jelszó későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="66eab-118">Be sure to record the name and password for later use.</span></span>
2. <span data-ttu-id="66eab-119">Győződjön meg arról, hogy az Azure SQL Database-kiszolgáló lehetővé teszi, hogy a csatlakozáshoz Azure-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="66eab-119">Ensure that your Azure SQL DB server allows Azure Services to connect to it.</span></span> <span data-ttu-id="66eab-120">A portálon a a **tűzfalbeállítások**, ügyeljen arra, hogy a **Azure-szolgáltatásokhoz való hozzáférés engedélyezése** beállítása **a**.</span><span class="sxs-lookup"><span data-stu-id="66eab-120">In the portal, in the **Firewall Settings**, ensure that the **Allow access to Azure Services** setting is set to **On**.</span></span> <span data-ttu-id="66eab-121">Kattintson a "Mentés" ikonra.</span><span class="sxs-lookup"><span data-stu-id="66eab-121">Click the "save" icon.</span></span>
   
   ![Engedélyezett szolgáltatások][1]
3. <span data-ttu-id="66eab-123">Azure-tárfiók létrehozása, amely jelzi a diagnosztikai kimenetet.</span><span class="sxs-lookup"><span data-stu-id="66eab-123">Create an Azure Storage account that will be used for diagnostics output.</span></span> <span data-ttu-id="66eab-124">Ugrás az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="66eab-124">Go to the Azure Portal.</span></span> <span data-ttu-id="66eab-125">A bal oldali sávon kattintson **új**, kattintson a **adatok + tárolás**, majd **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="66eab-125">In the left bar, click **New**, click **Data + Storage**, then **Storage**.</span></span>
4. <span data-ttu-id="66eab-126">Hozzon létre egy Azure felhőalapú szolgáltatás, amely a vegyes egyesítéses szolgáltatás fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="66eab-126">Create an Azure Cloud Service that will contain your Split-Merge service.</span></span>  <span data-ttu-id="66eab-127">Ugrás az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="66eab-127">Go to the Azure Portal.</span></span> <span data-ttu-id="66eab-128">A bal oldali sávon kattintson **új**, majd **számítási**, **Felhőszolgáltatás**, és **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="66eab-128">In the left bar, click **New**, then **Compute**, **Cloud Service**, and **Create**.</span></span> 

## <a name="configure-your-split-merge-service"></a><span data-ttu-id="66eab-129">A felosztott egyesítéses szolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="66eab-129">Configure your Split-Merge service</span></span>
### <a name="split-merge-service-configuration"></a><span data-ttu-id="66eab-130">Vegyes egyesítéses szolgáltatás konfigurációja</span><span class="sxs-lookup"><span data-stu-id="66eab-130">Split-Merge service configuration</span></span>
1. <span data-ttu-id="66eab-131">A mappában, amelybe letöltötte a felosztás egyesítéses szerelvényeket, készítsen másolatot a **ServiceConfiguration.Template.cscfg** fájl mellett szállított **SplitMergeService.cspkg** és adjon neki **ServiceConfiguration.cscfg**.</span><span class="sxs-lookup"><span data-stu-id="66eab-131">In the folder where you downloaded the Split-Merge assemblies, create a copy of the **ServiceConfiguration.Template.cscfg** file that shipped alongside **SplitMergeService.cspkg** and rename it **ServiceConfiguration.cscfg**.</span></span>
2. <span data-ttu-id="66eab-132">Nyissa meg **ServiceConfiguration.cscfg** egy szövegszerkesztőben, például a Visual Studio, amely a bemeneti adatok, például a tanúsítvány-ujjlenyomatok formátuma.</span><span class="sxs-lookup"><span data-stu-id="66eab-132">Open **ServiceConfiguration.cscfg** in a text editor such as Visual Studio that validates inputs such as the format of certificate thumbprints.</span></span>
3. <span data-ttu-id="66eab-133">Hozzon létre egy új adatbázist, vagy válasszon egy meglévő adatbázist a felosztott-Merge műveletek állapotának adatbázisként szolgál, és az adatbázis a kapcsolati karakterlánc lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="66eab-133">Create a new database or choose an existing database to serve as the status database for Split-Merge operations and retrieve the connection string of that database.</span></span> 
   
   > [!IMPORTANT]
   > <span data-ttu-id="66eab-134">Ilyenkor az állapot-adatbázis a Latin rendezést kell használnia (SQL\_Latin1\_általános\_CP1\_CI\_AS).</span><span class="sxs-lookup"><span data-stu-id="66eab-134">At this time, the status database must use the Latin  collation (SQL\_Latin1\_General\_CP1\_CI\_AS).</span></span> <span data-ttu-id="66eab-135">További információkért lásd: [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).</span><span class="sxs-lookup"><span data-stu-id="66eab-135">For more information, see [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).</span></span>
   >

   <span data-ttu-id="66eab-136">Az Azure SQL Database a kapcsolati karakterlánc általában a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="66eab-136">With Azure SQL DB, the connection string typically is of the form:</span></span>
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. <span data-ttu-id="66eab-137">Adja meg a kapcsolati karakterlánc mindkét cscfg-fájl a **SplitMergeWeb** és **SplitMergeWorker** szerepkör részei a ElasticScaleMetadata beállítást.</span><span class="sxs-lookup"><span data-stu-id="66eab-137">Enter this connection string in the cscfg file in both the **SplitMergeWeb** and **SplitMergeWorker** role sections in the ElasticScaleMetadata setting.</span></span>
5. <span data-ttu-id="66eab-138">Az a **SplitMergeWorker** szerepkör, adjon meg egy érvényes kapcsolati karakterláncot az Azure Storage a **WorkerRoleSynchronizationStorageAccountConnectionString** beállítást.</span><span class="sxs-lookup"><span data-stu-id="66eab-138">For the **SplitMergeWorker** role, enter a valid connection string to Azure storage for the **WorkerRoleSynchronizationStorageAccountConnectionString** setting.</span></span>

### <a name="configure-security"></a><span data-ttu-id="66eab-139">Biztonság konfigurálása</span><span class="sxs-lookup"><span data-stu-id="66eab-139">Configure security</span></span>
<span data-ttu-id="66eab-140">A szolgáltatás biztonsági konfigurálása részletes utasításokért tekintse meg a [vegyes egyesítéses biztonsági konfiguráció](sql-database-elastic-scale-split-merge-security-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="66eab-140">For detailed instructions to configure the security of the service, refer to the [Split-Merge security configuration](sql-database-elastic-scale-split-merge-security-configuration.md).</span></span>

<span data-ttu-id="66eab-141">Ebben az oktatóanyagban egy egyszerű próbatelepítés céljából a konfigurációs lépések minimális számú elvégezni a szolgáltatás eléréséhez és futó lesz.</span><span class="sxs-lookup"><span data-stu-id="66eab-141">For the purposes of a simple test deployment for this tutorial, a minimal set of configuration steps will be performed to get the service up and running.</span></span> <span data-ttu-id="66eab-142">Ezeket a lépéseket csak az egy gépen/fiók engedélyezése végrehajtása a szolgáltatással való kommunikációra őket.</span><span class="sxs-lookup"><span data-stu-id="66eab-142">These steps enable only the one machine/account executing them to communicate with the service.</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="66eab-143">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="66eab-143">Create a self-signed certificate</span></span>
<span data-ttu-id="66eab-144">Hozzon létre egy új könyvtárat, és a következő könyvtár a következő parancs használatával végrehajtani egy [Visual Studio Developer-parancssorból](http://msdn.microsoft.com/library/ms229859.aspx) ablakban:</span><span class="sxs-lookup"><span data-stu-id="66eab-144">Create a new directory and from this directory execute the following command using a [Developer Command Prompt for Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) window:</span></span>

   ```
    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

<span data-ttu-id="66eab-145">A titkos kulcs védelme jelszó kéri.</span><span class="sxs-lookup"><span data-stu-id="66eab-145">You are asked for a password to protect the private key.</span></span> <span data-ttu-id="66eab-146">Adjon meg egy erős jelszót, és erősítse meg.</span><span class="sxs-lookup"><span data-stu-id="66eab-146">Enter a strong password and confirm it.</span></span> <span data-ttu-id="66eab-147">Majd kéri a jelszót még egyszer ezt követően használható.</span><span class="sxs-lookup"><span data-stu-id="66eab-147">You are then prompted for the password to be used once more after that.</span></span> <span data-ttu-id="66eab-148">Kattintson a **Igen** ezzel importálja azt a megbízható hitelesítésszolgáltatók hitelesítésszolgáltatók legfelső szintű hitelesítésszolgáltatók tárolójába.</span><span class="sxs-lookup"><span data-stu-id="66eab-148">Click **Yes** at the end to import it to the Trusted Certification Authorities Root store.</span></span>

### <a name="create-a-pfx-file"></a><span data-ttu-id="66eab-149">A PFX-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="66eab-149">Create a PFX file</span></span>
<span data-ttu-id="66eab-150">Hajtsa végre a következő parancsot a azonos ablakban, ahol makecert végre lett hajtva; a tanúsítvány létrehozásához használt jelszavának használata:</span><span class="sxs-lookup"><span data-stu-id="66eab-150">Execute the following command from the same window where makecert was executed; use the same password that you used to create the certificate:</span></span>

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-the-client-certificate-into-the-personal-store"></a><span data-ttu-id="66eab-151">Importálja az ügyféltanúsítványt a személyes tárolóba</span><span class="sxs-lookup"><span data-stu-id="66eab-151">Import the client certificate into the personal store</span></span>
1. <span data-ttu-id="66eab-152">A Windows Intézőt, kattintson duplán a **MyCert.pfx**.</span><span class="sxs-lookup"><span data-stu-id="66eab-152">In Windows Explorer, double-click **MyCert.pfx**.</span></span>
2. <span data-ttu-id="66eab-153">Az a **Tanúsítványimportáló varázsló** válasszon **aktuális felhasználó** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="66eab-153">In the **Certificate Import Wizard** select **Current User** and click **Next**.</span></span>
3. <span data-ttu-id="66eab-154">Ellenőrizze a fájl elérési útját, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="66eab-154">Confirm the file path and click **Next**.</span></span>
4. <span data-ttu-id="66eab-155">Írja be a jelszót, hagyja **tartalmazza az összes kiterjesztett tulajdonságok** be van jelölve, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="66eab-155">Type the password, leave **Include all extended properties** checked and click **Next**.</span></span>
5. <span data-ttu-id="66eab-156">Hagyja **automatikusan a tanúsítvány választása [...]**  be van jelölve, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="66eab-156">Leave **Automatically select the certificate store[…]** checked and click **Next**.</span></span>
6. <span data-ttu-id="66eab-157">Kattintson a **Befejezés** és **OK**.</span><span class="sxs-lookup"><span data-stu-id="66eab-157">Click **Finish** and **OK**.</span></span>

### <a name="upload-the-pfx-file-to-the-cloud-service"></a><span data-ttu-id="66eab-158">A PFX-fájl feltöltése a felhőalapú szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="66eab-158">Upload the PFX file to the cloud service</span></span>
1. <span data-ttu-id="66eab-159">Nyissa meg az [Azure Portalt](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="66eab-159">Go to the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="66eab-160">Válassza ki **a felhőalapú szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="66eab-160">Select **Cloud Services**.</span></span>
3. <span data-ttu-id="66eab-161">Válassza ki a felhőszolgáltatást, a felosztás/egyesítés szolgáltatás számára létrehozott fent.</span><span class="sxs-lookup"><span data-stu-id="66eab-161">Select the cloud service you created above for the Split/Merge service.</span></span>
4. <span data-ttu-id="66eab-162">Kattintson a **tanúsítványok** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="66eab-162">Click **Certificates** on the top menu.</span></span>
5. <span data-ttu-id="66eab-163">Kattintson a **feltöltése** az alsó sáv a.</span><span class="sxs-lookup"><span data-stu-id="66eab-163">Click **Upload** in the bottom bar.</span></span>
6. <span data-ttu-id="66eab-164">Válassza ki a PFX-fájlt, és adja meg ugyanazt a jelszót a fenti.</span><span class="sxs-lookup"><span data-stu-id="66eab-164">Select the PFX file and enter the same password as above.</span></span>
7. <span data-ttu-id="66eab-165">Ezt követően másolja a tanúsítvány ujjlenyomata az új bejegyzést a listában.</span><span class="sxs-lookup"><span data-stu-id="66eab-165">Once completed, copy the certificate thumbprint from the new entry in the list.</span></span>

### <a name="update-the-service-configuration-file"></a><span data-ttu-id="66eab-166">A szolgáltatás konfigurációs fájljának frissítése</span><span class="sxs-lookup"><span data-stu-id="66eab-166">Update the service configuration file</span></span>
<span data-ttu-id="66eab-167">Illessze be a tanúsítvány ujjlenyomata fent átkerül az ujjlenyomat-érték attribútum ezeket a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="66eab-167">Paste the certificate thumbprint copied above into the thumbprint/value attribute of these settings.</span></span>
<span data-ttu-id="66eab-168">A feldolgozói szerepkör:</span><span class="sxs-lookup"><span data-stu-id="66eab-168">For the worker role:</span></span>
   ```
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

<span data-ttu-id="66eab-169">A webes szerepkör:</span><span class="sxs-lookup"><span data-stu-id="66eab-169">For the web role:</span></span>

   ```
    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

<span data-ttu-id="66eab-170">Vegye figyelembe, hogy az éles központi telepítések tanúsítványok önálló adja a hitelesítésszolgáltató, a titkosítási tanúsítvány és az ügyféltanúsítványok történjen.</span><span class="sxs-lookup"><span data-stu-id="66eab-170">Please note that for production deployments separate certificates should be used for the CA, for encryption, the Server certificate and client certificates.</span></span> <span data-ttu-id="66eab-171">Ez a részletes utasításokért lásd: [biztonsági konfiguráció](sql-database-elastic-scale-split-merge-security-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="66eab-171">For detailed instructions on this, see [Security Configuration](sql-database-elastic-scale-split-merge-security-configuration.md).</span></span>

## <a name="deploy-your-service"></a><span data-ttu-id="66eab-172">A szolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="66eab-172">Deploy your service</span></span>
1. <span data-ttu-id="66eab-173">Nyissa meg az [Azure Portal](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="66eab-173">Go to the [Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="66eab-174">Kattintson a **Felhőszolgáltatások** a bal oldali lapon, és válassza ki a korábban létrehozott felhőszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="66eab-174">Click the **Cloud Services** tab on the left, and select the cloud service that you created earlier.</span></span>
3. <span data-ttu-id="66eab-175">Kattintson a **irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="66eab-175">Click **Dashboard**.</span></span>
4. <span data-ttu-id="66eab-176">Válassza ki az átmeneti, majd kattintson a **töltse fel az új átmeneti üzembe helyezésének**.</span><span class="sxs-lookup"><span data-stu-id="66eab-176">Choose the staging environment, then click **Upload a new staging deployment**.</span></span>
   
   ![Átmeneti][3]
5. <span data-ttu-id="66eab-178">A párbeszédpanelen adja meg egy üzemelő példány címkéje.</span><span class="sxs-lookup"><span data-stu-id="66eab-178">In the dialog box, enter a deployment label.</span></span> <span data-ttu-id="66eab-179">"Csomag", mind a "Konfiguráció" részen kattintson "A helyi", és válassza ki a **SplitMergeService.cspkg** fájl- és a .cscfg fájlban korábban megadott értékektől.</span><span class="sxs-lookup"><span data-stu-id="66eab-179">For both 'Package' and 'Configuration', click 'From Local' and choose the **SplitMergeService.cspkg** file and your .cscfg file that you configured earlier.</span></span>
6. <span data-ttu-id="66eab-180">Győződjön meg arról, hogy a feliratú jelölőnégyzet **üzembe helyezés akkor is, ha egy vagy több szerepkör egyetlen példányt tartalmaz** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="66eab-180">Ensure that the checkbox labeled **Deploy even if one or more roles contain a single instance** is checked.</span></span>
7. <span data-ttu-id="66eab-181">Kattintson a jobb alsó a telepítésének megkezdéséhez a osztásjelek gombra.</span><span class="sxs-lookup"><span data-stu-id="66eab-181">Hit the tick button in the bottom right to begin the deployment.</span></span> <span data-ttu-id="66eab-182">Tehát az néhány percet igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="66eab-182">Expect it to take a few minutes to complete.</span></span>

   ![Feltöltés][4]

## <a name="troubleshoot-the-deployment"></a><span data-ttu-id="66eab-184">A telepítés hibáinak</span><span class="sxs-lookup"><span data-stu-id="66eab-184">Troubleshoot the deployment</span></span>
<span data-ttu-id="66eab-185">Ha a webes szerepkör nem hozható online állapotba, azt valószínűleg probléma a biztonsági beállítások.</span><span class="sxs-lookup"><span data-stu-id="66eab-185">If your web role fails to come online, it is likely a problem with the security configuration.</span></span> <span data-ttu-id="66eab-186">Ellenőrizze, hogy az SSL a fent leírt módon van-e konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="66eab-186">Check that the SSL is configured as described above.</span></span>

<span data-ttu-id="66eab-187">A feldolgozói szerepkör nem tudja ismét online elérhető, de a webes szerepkör sikeres, ha azt valószínűleg probléma a korábban létrehozott állapot-adatbázishoz szeretne csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="66eab-187">If your worker role fails to come online, but your web role succeeds, it is most likely a problem connecting to the status database that you created earlier.</span></span>

* <span data-ttu-id="66eab-188">Győződjön meg arról, hogy a kapcsolati karakterláncot a .cscfg a pontos.</span><span class="sxs-lookup"><span data-stu-id="66eab-188">Make sure that the connection string in your .cscfg is accurate.</span></span>
* <span data-ttu-id="66eab-189">Ellenőrizze, hogy a kiszolgáló és az adatbázis létezik, és a felhasználóazonosító és jelszó helyességét.</span><span class="sxs-lookup"><span data-stu-id="66eab-189">Check that the server and database exist, and that the user id and password are correct.</span></span>
* <span data-ttu-id="66eab-190">Az Azure SQL Database a kapcsolati karakterlánc a következő formátumban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="66eab-190">For Azure SQL DB, the connection string should be of the form:</span></span>

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* <span data-ttu-id="66eab-191">Győződjön meg arról, hogy a kiszolgáló neve nem kezdődhet **https://**.</span><span class="sxs-lookup"><span data-stu-id="66eab-191">Ensure that the server name does not begin with **https://**.</span></span>
* <span data-ttu-id="66eab-192">Győződjön meg arról, hogy az Azure SQL Database-kiszolgáló lehetővé teszi, hogy a csatlakozáshoz Azure-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="66eab-192">Ensure that your Azure SQL DB server allows Azure Services to connect to it.</span></span> <span data-ttu-id="66eab-193">Ehhez nyissa meg a https://manage.windowsazure.com, a bal oldalon kattintson az "SQL-adatbázisok", a lap tetején kattintson a "Kiszolgáló", és válassza ki a kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="66eab-193">To do this, open https://manage.windowsazure.com, click “SQL Databases” on the left, click “Servers” at the top, and select your server.</span></span> <span data-ttu-id="66eab-194">Kattintson a **konfigurálása** tetején, és győződjön meg arról, hogy a **Azure Services** beállítás "Yes" értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="66eab-194">Click **Configure** at the top and ensure that the **Azure Services** setting is set to “Yes”.</span></span> <span data-ttu-id="66eab-195">(Az Előfeltételek című Ez a cikk tetején).</span><span class="sxs-lookup"><span data-stu-id="66eab-195">(See the Prerequisites section at the top of this article).</span></span>

## <a name="test-the-service-deployment"></a><span data-ttu-id="66eab-196">A szolgáltatás központi telepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="66eab-196">Test the service deployment</span></span>
### <a name="connect-with-a-web-browser"></a><span data-ttu-id="66eab-197">Egy webböngészővel rendelkező csatlakozás</span><span class="sxs-lookup"><span data-stu-id="66eab-197">Connect with a web browser</span></span>
<span data-ttu-id="66eab-198">Határozza meg, hogy a vegyes egyesítéses szolgáltatás webes végpontja.</span><span class="sxs-lookup"><span data-stu-id="66eab-198">Determine the web endpoint of your Split-Merge service.</span></span> <span data-ttu-id="66eab-199">Ez az található a klasszikus Azure-portálon a a **irányítópult** a felhőszolgáltatás és a keresése **webhely URL-címe** jobb oldalán.</span><span class="sxs-lookup"><span data-stu-id="66eab-199">You can find this in the Azure Classic Portal by going to the **Dashboard** of your cloud service and looking under **Site URL** on the right side.</span></span> <span data-ttu-id="66eab-200">Cserélje le **http://** rendelkező **https://** óta az alapértelmezett biztonsági beállításairól, tiltsa le a HTTP-végpont.</span><span class="sxs-lookup"><span data-stu-id="66eab-200">Replace **http://** with **https://** since the default security settings disable the HTTP endpoint.</span></span> <span data-ttu-id="66eab-201">A lap betöltése a böngészőbe az URL-címhez.</span><span class="sxs-lookup"><span data-stu-id="66eab-201">Load the page for this URL into your browser.</span></span>

### <a name="test-with-powershell-scripts"></a><span data-ttu-id="66eab-202">A PowerShell-parancsfájlok tesztelése</span><span class="sxs-lookup"><span data-stu-id="66eab-202">Test with PowerShell scripts</span></span>
<span data-ttu-id="66eab-203">A központi telepítés és a környezet tesztelhető a mellékelt PowerShell-parancsfájlok futtatásával.</span><span class="sxs-lookup"><span data-stu-id="66eab-203">The deployment and your environment can be tested by running the included sample PowerShell scripts.</span></span>

<span data-ttu-id="66eab-204">A parancsfájlok tartalmazza a következők:</span><span class="sxs-lookup"><span data-stu-id="66eab-204">The script files included are:</span></span>

1. <span data-ttu-id="66eab-205">**SetupSampleSplitMergeEnvironment.ps1** -állítja be a teszt adatrétegbeli vegyes/egyesítés (lásd az alábbi táblázatban részletes leírása)</span><span class="sxs-lookup"><span data-stu-id="66eab-205">**SetupSampleSplitMergeEnvironment.ps1** - sets up a test data tier for Split/Merge (see table below for detailed description)</span></span>
2. <span data-ttu-id="66eab-206">**ExecuteSampleSplitMerge.ps1** -végrehajtja a teszteléshez használt teszt műveleteit adatok réteg (lásd az alábbi táblázatban részletes leírása)</span><span class="sxs-lookup"><span data-stu-id="66eab-206">**ExecuteSampleSplitMerge.ps1** - executes test operations on the test data tier (see table below for detailed description)</span></span>
3. <span data-ttu-id="66eab-207">**GetMappings.ps1** – legfelső szintű mintaparancsfájl a shard leképezések aktuális állapotát megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="66eab-207">**GetMappings.ps1** - top-level sample script that prints out the current state of the shard mappings.</span></span>
4. <span data-ttu-id="66eab-208">**ShardManagement.psm1** -segítő parancsfájl tördelve a ShardManagement API</span><span class="sxs-lookup"><span data-stu-id="66eab-208">**ShardManagement.psm1**  - helper script that wraps the ShardManagement API</span></span>
5. <span data-ttu-id="66eab-209">**SqlDatabaseHelpers.psm1** -segítő parancsfájl létrehozásához és kezeléséhez az SQL-adatbázisok</span><span class="sxs-lookup"><span data-stu-id="66eab-209">**SqlDatabaseHelpers.psm1** - helper script for creating and managing SQL databases</span></span>
   
   <table style="width:100%">
     <tr>
       <th><span data-ttu-id="66eab-210">PowerShell-fájl</span><span class="sxs-lookup"><span data-stu-id="66eab-210">PowerShell file</span></span></th>
       <th><span data-ttu-id="66eab-211">Lépések</span><span class="sxs-lookup"><span data-stu-id="66eab-211">Steps</span></span></th>
     </tr>
     <tr>
       <th rowspan="5"><span data-ttu-id="66eab-212">SetupSampleSplitMergeEnvironment.ps1</span><span class="sxs-lookup"><span data-stu-id="66eab-212">SetupSampleSplitMergeEnvironment.ps1</span></span></th>
       <td>1.    <span data-ttu-id="66eab-213">Létrehoz egy shard térkép manager adatbázis</span><span class="sxs-lookup"><span data-stu-id="66eab-213">Creates a shard map manager database</span></span></td>
     </tr>
     <tr>
       <td>2.    <span data-ttu-id="66eab-214">2 shard-adatbázisokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="66eab-214">Creates 2 shard databases.</span></span>
     </tr>
     <tr>
       <td>3.    <span data-ttu-id="66eab-215">Ezen adatbázis (törli ezeket az adatbázisokat a maps bármely létező szilánkok) shard leképezést hoz létre.</span><span class="sxs-lookup"><span data-stu-id="66eab-215">Creates a shard map for those database (deletes any existing shard maps on those databases).</span></span> </td>
     </tr>
     <tr>
       <td>4.    <span data-ttu-id="66eab-216">Táblát hoz létre egy kis minta mindkét a szilánkok, tölti fel a tábla a szilánkok egyikében.</span><span class="sxs-lookup"><span data-stu-id="66eab-216">Creates a small sample table in both the shards, and populates the table in one of the shards.</span></span></td>
     </tr>
     <tr>
       <td>5.    <span data-ttu-id="66eab-217">A szilánkos táblához SchemaInfo deklarálja.</span><span class="sxs-lookup"><span data-stu-id="66eab-217">Declares the SchemaInfo for the sharded table.</span></span></td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th><span data-ttu-id="66eab-218">PowerShell-fájl</span><span class="sxs-lookup"><span data-stu-id="66eab-218">PowerShell file</span></span></th>
       <th><span data-ttu-id="66eab-219">Lépések</span><span class="sxs-lookup"><span data-stu-id="66eab-219">Steps</span></span></th>
     </tr>
   <tr>
       <th rowspan="4"><span data-ttu-id="66eab-220">ExecuteSampleSplitMerge.ps1</span><span class="sxs-lookup"><span data-stu-id="66eab-220">ExecuteSampleSplitMerge.ps1</span></span> </th>
       <td>1.    <span data-ttu-id="66eab-221">Felosztja a fele az adatokat az első shard a második shard vegyes egyesítéses szolgáltatás webes előtér vegyes kérést küld.</span><span class="sxs-lookup"><span data-stu-id="66eab-221">Sends a split request to the Split-Merge Service web frontend, which splits half the data from the first shard to the second shard.</span></span></td>
     </tr>
     <tr>
       <td>2.    <span data-ttu-id="66eab-222">Kérdezze le a webes előtér vegyes kérés állapotát, és megvárja, amíg a kérelem befejeződött.</span><span class="sxs-lookup"><span data-stu-id="66eab-222">Polls the web frontend for the split request status and waits until the request completes.</span></span></td>
     </tr>
     <tr>
       <td>3.    <span data-ttu-id="66eab-223">Egyesítési kérést küld az áll át az adatokat a második shard vissza az első shard vegyes egyesítéses szolgáltatás webes előtér.</span><span class="sxs-lookup"><span data-stu-id="66eab-223">Sends a merge request to the Split-Merge Service web frontend, which moves the data from the second shard back to the first shard.</span></span></td>
     </tr>
     <tr>
       <td>4.    <span data-ttu-id="66eab-224">Kérdezze le a webes előtér az egyesítési kérelem állapot, és megvárja, amíg a kérelem befejeződött.</span><span class="sxs-lookup"><span data-stu-id="66eab-224">Polls the web frontend for the merge request status and waits until the request completes.</span></span></td>
     </tr>
   </table>
   
## <a name="use-powershell-to-verify-your-deployment"></a><span data-ttu-id="66eab-225">A telepítés ellenőrzése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="66eab-225">Use PowerShell to verify your deployment</span></span>
1. <span data-ttu-id="66eab-226">Új PowerShell ablakban keresse meg a könyvtárat, amelybe letöltötte a felosztás egyesítéses csomag és keresse meg a "powershell" könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="66eab-226">Open a new PowerShell window and navigate to the directory where you downloaded the Split-Merge package, and then navigate into the “powershell” directory.</span></span>
2. <span data-ttu-id="66eab-227">Hozzon létre egy Azure SQL adatbázis-kiszolgáló (vagy válasszon egy meglévő kiszolgálót) Ha a shard térkép manager és a szilánkok jön létre.</span><span class="sxs-lookup"><span data-stu-id="66eab-227">Create an Azure SQL database server (or choose an existing server) where the shard map manager and shards will be created.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="66eab-228">A SetupSampleSplitMergeEnvironment.ps1 parancsfájl hoz létre, tartsa a parancsfájl egyszerű alapértelmezés szerint ezek az adatbázisok ugyanazon a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="66eab-228">The SetupSampleSplitMergeEnvironment.ps1 script creates all these databases on the same server by default to keep the script simple.</span></span> <span data-ttu-id="66eab-229">Ez a korlátozás nem osztott egyesítéses szolgáltatás magát.</span><span class="sxs-lookup"><span data-stu-id="66eab-229">This is not a restriction of the Split-Merge Service itself.</span></span>
   >
   
   <span data-ttu-id="66eab-230">A felosztott egyesítéses szolgáltatás helyezi át az adatokat, és frissíti a shard leképezés egy SQL-hitelesítési bejelentkezési olvasási/írási hozzáférést a adatbázisok lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="66eab-230">A SQL authentication login with read/write access to the DBs will be needed for the Split-Merge service to move data and update the shard map.</span></span> <span data-ttu-id="66eab-231">A felosztott egyesítéses szolgáltatás fut a felhőben, mert azt jelenleg nem támogatja integrált hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="66eab-231">Since the Split-Merge Service runs in the cloud, it does not currently support Integrated Authentication.</span></span>
   
   <span data-ttu-id="66eab-232">Győződjön meg arról, hogy engedélyezzék az IP-cím a gép, ezek a parancsfájlok futtatása az Azure SQL-kiszolgáló van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="66eab-232">Make sure the Azure SQL server is configured to allow access from the IP address of the machine running these scripts.</span></span> <span data-ttu-id="66eab-233">Ezt a beállítást, az Azure SQL-kiszolgáló található / configuration / engedélyezett IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="66eab-233">You can find this setting under the Azure SQL server / configuration / allowed IP addresses.</span></span>
3. <span data-ttu-id="66eab-234">Hajtsa végre a SetupSampleSplitMergeEnvironment.ps1 parancsfájlt a minta környezetet hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="66eab-234">Execute the SetupSampleSplitMergeEnvironment.ps1 script to create the sample environment.</span></span>
   
   <span data-ttu-id="66eab-235">A parancsfájl futtatása lesz kitöröl shard térkép meglévő felügyeleti adatokat a shard manager adatbázist és a szilánkok struktúrákat.</span><span class="sxs-lookup"><span data-stu-id="66eab-235">Running this script will wipe out any existing shard map management data structures on the shard map manager database and the shards.</span></span> <span data-ttu-id="66eab-236">Hasznos, ha a shard térkép vagy szilánkok inicializálja újra kívánja a parancsfájl lehet.</span><span class="sxs-lookup"><span data-stu-id="66eab-236">It may be useful to rerun the script if you wish to re-initialize the shard map or shards.</span></span>
   
   <span data-ttu-id="66eab-237">Minta parancssor:</span><span class="sxs-lookup"><span data-stu-id="66eab-237">Sample command line:</span></span>

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. <span data-ttu-id="66eab-238">Hajtsa végre a Getmappings.ps1 parancsfájlt a jelenleg létező hozzárendelések megtekintése a minta-környezetben.</span><span class="sxs-lookup"><span data-stu-id="66eab-238">Execute the Getmappings.ps1 script to view the mappings that currently exist in the sample environment.</span></span>
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. <span data-ttu-id="66eab-239">Hajtsa végre a ExecuteSampleSplitMerge.ps1 parancsfájlt egy vegyes művelet (továbblép fele az adatokat az első shard a második szilánkok) és egy (az adatok áthelyezése vissza az első shard alakzatot) egyesítési művelet végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="66eab-239">Execute the ExecuteSampleSplitMerge.ps1 script to execute a split operation (moving half the data on the first shard to the second shard) and then a merge operation (moving the data back onto the first shard).</span></span> <span data-ttu-id="66eab-240">Ha SSL konfigurálva, és a bal oldali a http-végpont le van tiltva, győződjön meg arról, hogy inkább a https:// végpont.</span><span class="sxs-lookup"><span data-stu-id="66eab-240">If you configured SSL and left the http endpoint disabled, ensure that you use the https:// endpoint instead.</span></span>
   
   <span data-ttu-id="66eab-241">Minta parancssor:</span><span class="sxs-lookup"><span data-stu-id="66eab-241">Sample command line:</span></span>

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   <span data-ttu-id="66eab-242">Ha megjelenik a alatt hiba, akkor nagy valószínűséggel a webalkalmazás-végpontot tanúsítványával kapcsolatos problémára.</span><span class="sxs-lookup"><span data-stu-id="66eab-242">If you receive the below error, it is most likely a problem with your Web endpoint’s certificate.</span></span> <span data-ttu-id="66eab-243">Próbáljon meg csatlakozni a kedvenc webböngésző webes végpontjának, és ellenőrizze, hogy a tanúsítvány hiba.</span><span class="sxs-lookup"><span data-stu-id="66eab-243">Try connecting to the Web endpoint with your favorite Web browser and check if there is a certificate error.</span></span>
   
     ```
     Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLSsecure channel.
     ```
   
   <span data-ttu-id="66eab-244">Ha sikeres volt, a kimeneti hasonlóan kell kinéznie az alábbi:</span><span class="sxs-lookup"><span data-stu-id="66eab-244">If it succeeded, the output should look like the below:</span></span>
   
   ```
   > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' -CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
   > Sending split request
   > Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
   > Polling split-merge request status. Press Ctrl-C to end
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > Sending merge request
   > Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
   > Polling request status. Press Ctrl-C to end
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > 
   ```
6. <span data-ttu-id="66eab-245">Más adattípusok kísérletezhet!</span><span class="sxs-lookup"><span data-stu-id="66eab-245">Experiment with other data types!</span></span> <span data-ttu-id="66eab-246">Választható - ShardKeyType paraméter, amely lehetővé teszi a írja be mindegyik parancsfájl érvénybe.</span><span class="sxs-lookup"><span data-stu-id="66eab-246">All of these scripts take an optional -ShardKeyType parameter that allows you to specify the key type.</span></span> <span data-ttu-id="66eab-247">Az alapértelmezett érték Int32, de azt is megadhatja Int64, Guid vagy bináris.</span><span class="sxs-lookup"><span data-stu-id="66eab-247">The default is Int32, but you can also specify Int64, Guid, or Binary.</span></span>

## <a name="create-requests"></a><span data-ttu-id="66eab-248">Létrehozási kérelmek</span><span class="sxs-lookup"><span data-stu-id="66eab-248">Create requests</span></span>
<span data-ttu-id="66eab-249">A szolgáltatás használható a webes felhasználói felületen vagy importálásáról és használatáról a SplitMerge.psm1 PowerShell modult, amely a kéréseket a webes szerepkör keresztül.</span><span class="sxs-lookup"><span data-stu-id="66eab-249">The service can be used either by using the web UI or by importing and using the SplitMerge.psm1 PowerShell module which will submit your requests through the web role.</span></span>

<span data-ttu-id="66eab-250">A szolgáltatás mozgathatja az adatokat a szilánkos táblák és a hivatkozási táblák.</span><span class="sxs-lookup"><span data-stu-id="66eab-250">The service can move data in both sharded tables and reference tables.</span></span> <span data-ttu-id="66eab-251">A szilánkos táblához egy horizontális skálázási kulcsoszlopa, és különböző soradatok rendelkezzen minden szilánkcímtárban.</span><span class="sxs-lookup"><span data-stu-id="66eab-251">A sharded table has a sharding key column and has different row data on each shard.</span></span> <span data-ttu-id="66eab-252">A referenciatábla nincs szilánkos, a minden shard azonos sor adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="66eab-252">A reference table is not sharded so it contains the same row data on every shard.</span></span> <span data-ttu-id="66eab-253">Hivatkozási táblák nem változik gyakran, és csatlakozzon a lekérdezésekben szilánkos táblák használt adatok hasznosak.</span><span class="sxs-lookup"><span data-stu-id="66eab-253">Reference tables are useful for data that does not change often and is used to JOIN with sharded tables in queries.</span></span>

<span data-ttu-id="66eab-254">A felosztott-egyesítési művelet végrehajtásához a szilánkos táblákat és a referencia-táblázatot, amely került át szeretné kell deklarálni.</span><span class="sxs-lookup"><span data-stu-id="66eab-254">In order to perform a split-merge operation, you must declare the sharded tables and reference tables that you want to have moved.</span></span> <span data-ttu-id="66eab-255">Ez a érhető el a **SchemaInfo** API.</span><span class="sxs-lookup"><span data-stu-id="66eab-255">This is accomplished with the **SchemaInfo** API.</span></span> <span data-ttu-id="66eab-256">Az API-t a **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** névtér.</span><span class="sxs-lookup"><span data-stu-id="66eab-256">This API is in the **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** namespace.</span></span>

1. <span data-ttu-id="66eab-257">Minden szilánkos táblához, hozzon létre egy **ShardedTableInfo** a tábla szülő sémanév leíró objektum (nem kötelező, az alapértelmezett érték a "dbo"), a táblázat nevét, és az oszlop nevét a horizontális kulcsot tartalmazó tábla.</span><span class="sxs-lookup"><span data-stu-id="66eab-257">For each sharded table, create a **ShardedTableInfo** object describing the table’s parent schema name (optional, defaults to “dbo”), the table name, and the column name in that table that contains the sharding key.</span></span>
2. <span data-ttu-id="66eab-258">Minden egyes összefoglaló táblázatot is létrehozhat egy **ReferenceTableInfo** a tábla szülő sémanév leíró objektum (nem kötelező, az alapértelmezett érték a "dbo") és a tábla neve.</span><span class="sxs-lookup"><span data-stu-id="66eab-258">For each reference table, create a **ReferenceTableInfo** object describing the table’s parent schema name (optional, defaults to “dbo”) and the table name.</span></span>
3. <span data-ttu-id="66eab-259">A fenti TableInfo objektumokat hozzáadni egy új **SchemaInfo** objektum.</span><span class="sxs-lookup"><span data-stu-id="66eab-259">Add the above TableInfo objects to a new **SchemaInfo** object.</span></span>
4. <span data-ttu-id="66eab-260">A hivatkozás egy **ShardMapManager** objektum és hívás **GetSchemaInfoCollection**.</span><span class="sxs-lookup"><span data-stu-id="66eab-260">Get a reference to a **ShardMapManager** object, and call **GetSchemaInfoCollection**.</span></span>
5. <span data-ttu-id="66eab-261">Adja hozzá a **SchemaInfo** számára a **SchemaInfoCollection**, a shard leképezésnév biztosítása.</span><span class="sxs-lookup"><span data-stu-id="66eab-261">Add the **SchemaInfo** to the **SchemaInfoCollection**, providing the shard map name.</span></span>

<span data-ttu-id="66eab-262">Példa erre a SetupSampleSplitMergeEnvironment.ps1 parancsfájl látható.</span><span class="sxs-lookup"><span data-stu-id="66eab-262">An example of this can be seen in the SetupSampleSplitMergeEnvironment.ps1 script.</span></span>

<span data-ttu-id="66eab-263">A felosztott egyesítéses szolgáltatás nem a céladatbázis (vagy bármely táblák az adatbázisban séma) hozza létre.</span><span class="sxs-lookup"><span data-stu-id="66eab-263">The Split-Merge service does not create the target database (or schema for any tables in the database) for you.</span></span> <span data-ttu-id="66eab-264">Fel kell előre létrehozott egy kérést küld a szolgáltatás előtt.</span><span class="sxs-lookup"><span data-stu-id="66eab-264">They must be pre-created before sending a request to the service.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="66eab-265">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="66eab-265">Troubleshooting</span></span>
<span data-ttu-id="66eab-266">Megjelenik a alatt jelenik meg, amikor a minta powershell-parancsfájlok futtatásakor:</span><span class="sxs-lookup"><span data-stu-id="66eab-266">You may see the below message when running the sample powershell scripts:</span></span>

   ```
   Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.
   ```

<span data-ttu-id="66eab-267">Ez a hiba azt jelenti, hogy az SSL-tanúsítvány nincs megfelelően konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="66eab-267">This error means that your SSL certificate is not configured correctly.</span></span> <span data-ttu-id="66eab-268">Kérjük, kövesse a szakaszban található útmutatásokat "Böngészővel csatlakozás".</span><span class="sxs-lookup"><span data-stu-id="66eab-268">Please follow the instructions in section 'Connecting with a web browser'.</span></span>

<span data-ttu-id="66eab-269">Nem küldenek kéréseket jelenhet meg ezt:</span><span class="sxs-lookup"><span data-stu-id="66eab-269">If you cannot submit requests you may see this:</span></span>

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

<span data-ttu-id="66eab-270">Ebben az esetben ellenőrizze, a konfigurációs fájlban, különösen a beállítás a **WorkerRoleSynchronizationStorageAccountConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="66eab-270">In this case, check your configuration file, in particular the setting for **WorkerRoleSynchronizationStorageAccountConnectionString**.</span></span> <span data-ttu-id="66eab-271">Ez a hiba általában azt jelzi, hogy a feldolgozói szerepkör sikeresen inicializálása sikertelen első használatkor a metaadatokat tároló adatbázis.</span><span class="sxs-lookup"><span data-stu-id="66eab-271">This error typically indicates that the worker role could not successfully initialize the metadata database on first use.</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png

