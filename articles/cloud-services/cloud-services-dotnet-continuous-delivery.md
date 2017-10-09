---
title: "aaaContinuous kézbesítési felhőalapú szolgáltatások és a TFS az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset be folyamatos kézbesítését az Azure felhőalapú alkalmazásokba. Kódminták MSBuild parancssori utasításokat és a PowerShell-parancsfájlokat."
services: cloud-services
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4f3c93c6-5c82-4779-9d19-7404a01e82ca
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/12/2017
ms.author: kraigb
ms.openlocfilehash: c0e5e72ffbd3c05b84ce1733068e92c528bcc4b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a><span data-ttu-id="ff653-104">Azure felhőszolgáltatások folyamatos kézbesítés</span><span class="sxs-lookup"><span data-stu-id="ff653-104">Continuous Delivery for Cloud Services in Azure</span></span>
<span data-ttu-id="ff653-105">hello ebben a cikkben leírt eljárás bemutatja, hogyan tooset be folyamatos kézbesítését az Azure felhőalapú alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="ff653-105">hello process described in this article shows you how tooset up continuous delivery for Azure cloud apps.</span></span> <span data-ttu-id="ff653-106">Ez a folyamat automatikusan a csomagok létrehozását és telepítését a hello csomag tooAzure után minden kód be teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="ff653-106">This process enables you to automatically create packages and deploy hello package tooAzure after every code check-in.</span></span> <span data-ttu-id="ff653-107">hello csomag felépítési folyamat cikkben leírt egyenértékű toohello **csomag** parancs a Visual Studio és a közzétételi lépéseket is egyenértékű toohello **közzététel** a Visual Studio parancsot.</span><span class="sxs-lookup"><span data-stu-id="ff653-107">hello package build process described in this article is equivalent toohello **Package** command in Visual Studio, and the publishing steps are equivalent toohello **Publish** command in Visual Studio.</span></span>
<span data-ttu-id="ff653-108">hello cikk magában foglalja az hello módszert szeretné használni toocreate build kiszolgáló MSBuild parancssori utasításokat és a Windows PowerShell-parancsfájlok, és azt is bemutatja, hogyan konfigurálja az toooptionally a Visual Studio Team Foundation Server - definíciók csoport létrehozása toouse hello MSBuild parancsok és a PowerShell-parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="ff653-108">hello article covers hello methods you would use toocreate a build server with MSBuild command-line statements and Windows PowerShell scripts, and it also demonstrates how toooptionally configure Visual Studio Team Foundation Server - Team Build definitions toouse hello MSBuild commands and PowerShell scripts.</span></span> <span data-ttu-id="ff653-109">hello testre szabható a összeállító környezetet és az Azure környezetekben történik.</span><span class="sxs-lookup"><span data-stu-id="ff653-109">hello process is customizable for your build environment and Azure target environments.</span></span>

<span data-ttu-id="ff653-110">Is használhatja a Visual Studio Team Services, egy, a TFS verziójának Azure szolgáltatásban üzemeltetett, toodo ez könnyebben.</span><span class="sxs-lookup"><span data-stu-id="ff653-110">You can also use Visual Studio Team Services, a version of TFS that is hosted in Azure, toodo this more easily.</span></span> 

<span data-ttu-id="ff653-111">Kezdés előtt kell közzé tenni az alkalmazást a Visual Studio eszközből.</span><span class="sxs-lookup"><span data-stu-id="ff653-111">Before you start, you should publish your application from Visual Studio.</span></span>
<span data-ttu-id="ff653-112">Ezzel biztosíthatja, hogy minden hello erőforrás elérhető-e és inicializálva-e amikor megpróbál tooautomate hello kiadvány folyamat.</span><span class="sxs-lookup"><span data-stu-id="ff653-112">This will ensure that all hello resources are available and initialized when you attempt tooautomate hello publication process.</span></span>

## <a name="1-configure-hello-build-server"></a><span data-ttu-id="ff653-113">1: hello Build kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ff653-113">1: Configure hello Build Server</span></span>
<span data-ttu-id="ff653-114">Egy Azure-csomag létrehozása MSBuild használatával, előtt telepítenie kell hello szükséges szoftverek és eszközök hello build kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="ff653-114">Before you can create an Azure package by using MSBuild, you must install hello required software and tools on hello build server.</span></span>

<span data-ttu-id="ff653-115">A Visual Studio nem szükséges toobe hello build kiszolgálón telepítve.</span><span class="sxs-lookup"><span data-stu-id="ff653-115">Visual Studio is not required toobe installed on hello build server.</span></span> <span data-ttu-id="ff653-116">Toouse Team Foundation Build Service toomanage build kiszolgálóját, hajtsa végre hello hello utasításait [Team Foundation Build Service] [ Team Foundation Build Service] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="ff653-116">If you want toouse Team Foundation Build Service toomanage your build server, follow hello instructions in hello [Team Foundation Build Service][Team Foundation Build Service] documentation.</span></span>

1. <span data-ttu-id="ff653-117">Hello build kiszolgálón telepítse a hello [.NET-keretrendszer 4.5.2-es][.NET Framework 4.5.2], mert az MSBuild tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ff653-117">On hello build server, install hello [.NET Framework 4.5.2][.NET Framework 4.5.2], which includes MSBuild.</span></span>
2. <span data-ttu-id="ff653-118">Telepítse legújabb hello [.NET-keretrendszerhez készült Azure szerzői eszközök](https://azure.microsoft.com/develop/net/).</span><span class="sxs-lookup"><span data-stu-id="ff653-118">Install hello latest [Azure Authoring Tools for .NET](https://azure.microsoft.com/develop/net/).</span></span>
3. <span data-ttu-id="ff653-119">Telepítse a hello [.NET Azure-könyvtárakban](http://go.microsoft.com/fwlink/?LinkId=623519).</span><span class="sxs-lookup"><span data-stu-id="ff653-119">Install hello [Azure Libraries for .NET](http://go.microsoft.com/fwlink/?LinkId=623519).</span></span>
4. <span data-ttu-id="ff653-120">Másolás hello Microsoft.WebApplication.targets fájlt a Visual Studio telepítési toohello kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ff653-120">Copy hello Microsoft.WebApplication.targets file from a Visual Studio installation toohello build server.</span></span>

   <span data-ttu-id="ff653-121">A telepített Visual Studio számítógépen, a fájl C: hello könyvtárban található\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications.</span><span class="sxs-lookup"><span data-stu-id="ff653-121">On a computer with Visual Studio installed, this file is located in hello directory C:\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications.</span></span> <span data-ttu-id="ff653-122">Másolja az toohello hello build kiszolgálón ugyanabban a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="ff653-122">You should copy it toohello same directory on hello build server.</span></span>
5. <span data-ttu-id="ff653-123">Telepítse a hello [Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="ff653-123">Install hello [Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).</span></span>

## <a name="2-build-a-package-using-msbuild-commands"></a><span data-ttu-id="ff653-124">2: MSBuild parancsokkal csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="ff653-124">2: Build a Package using MSBuild Commands</span></span>
<span data-ttu-id="ff653-125">Ez a szakasz ismerteti, hogyan tooconstruct az MSBuild parancsot, amely egy Azure-csomagot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ff653-125">This section describes how tooconstruct an MSBuild command that builds an Azure package.</span></span> <span data-ttu-id="ff653-126">Futtassa ezt a lépést a hello build server tooverify, hogy minden helyesen van-e állítva, és hogy hello MSBuild parancs valóban azt toodo.</span><span class="sxs-lookup"><span data-stu-id="ff653-126">Run this step on hello build server tooverify that everything is configured correctly and that hello MSBuild command does what you want it toodo.</span></span> <span data-ttu-id="ff653-127">Hello a következő szakaszban leírt módon a parancssor tooexisting fordítási parancsprogramot hello build kiszolgálón, vagy használhatja a TFS Build definícióban hello parancssori vagy hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="ff653-127">You can either add this command line tooexisting build scripts on hello build server, or you can use hello command line in a TFS Build Definition, as described in hello next section.</span></span> <span data-ttu-id="ff653-128">Parancssori paraméterek és MSBuild kapcsolatos további információkért lásd: [MSBuild parancssori hivatkozás](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="ff653-128">For more information about command-line parameters and MSBuild, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span></span>

1. <span data-ttu-id="ff653-129">Visual Studio hello build kiszolgálóra van telepítve, keresse meg és válassza a **Visual Studio parancssorból** a hello **Visual Studio eszközök** Windows-mappa.</span><span class="sxs-lookup"><span data-stu-id="ff653-129">If Visual Studio is installed on hello build server, locate and choose **Visual Studio Command Prompt** in hello **Visual Studio Tools** folder in Windows.</span></span>

   <span data-ttu-id="ff653-130">Ha a Visual Studio hello build kiszolgálón nincs telepítve, nyisson meg egy parancssort, és győződjön meg arról, hogy MSBuild.exe elérhető-e az elérési út.</span><span class="sxs-lookup"><span data-stu-id="ff653-130">If Visual Studio is not installed on hello build server, open a command prompt and make sure that MSBuild.exe is accessible on the path.</span></span> <span data-ttu-id="ff653-131">MSBuild telepítve van a .NET-keretrendszer hello hello elérési útja: % WINDIR %\\Microsoft.NET\\keretrendszer\\*verzió*.</span><span class="sxs-lookup"><span data-stu-id="ff653-131">MSBuild is installed with hello .NET Framework in hello path %WINDIR%\\Microsoft.NET\\Framework\\*Version*.</span></span> <span data-ttu-id="ff653-132">Például ha .NET Framework 4 telepítve van a MSBuild.exe toohello PATH környezeti változóba hozzáadásához írja be a hello parancs hello parancssorba a következő:</span><span class="sxs-lookup"><span data-stu-id="ff653-132">For example, to add MSBuild.exe toohello PATH environment variable when you have .NET Framework 4 installed, type hello following command at hello command prompt:</span></span>

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. <span data-ttu-id="ff653-133">Hello parancssorban keresse meg a toohello toobuild kívánt Azure-projekt tartalmazó mappára.</span><span class="sxs-lookup"><span data-stu-id="ff653-133">At hello command prompt, navigate toohello folder containing the Azure project file that you want toobuild.</span></span>
3. <span data-ttu-id="ff653-134">Futtassa a MSBuild hello/TARGET: Publish beállítást, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ff653-134">Run MSBuild with hello /target:Publish option as in hello following example:</span></span>

       MSBuild /target:Publish

   <span data-ttu-id="ff653-135">Ez a beállítás rövidíthető /t: közzététele.</span><span class="sxs-lookup"><span data-stu-id="ff653-135">This option can be abbreviated as /t:Publish.</span></span> <span data-ttu-id="ff653-136">MSBuild hello /t:Publish beállítást kell nem keverendő össze a hello közzététel parancsok, amely a Visual Studio Ha hello Azure SDK telepítve van.</span><span class="sxs-lookup"><span data-stu-id="ff653-136">hello /t:Publish option in MSBuild should not be confused with hello Publish commands available in Visual Studio when you have hello Azure SDK installed.</span></span> <span data-ttu-id="ff653-137">hello /t: Publish beállítást, csak a buildek hello Azure csomagok.</span><span class="sxs-lookup"><span data-stu-id="ff653-137">hello /t:Publish option only builds hello Azure packages.</span></span> <span data-ttu-id="ff653-138">Azt nem kell telepítenie hello csomagok, mint a Visual Studio hello közzététel parancsok.</span><span class="sxs-lookup"><span data-stu-id="ff653-138">It does not deploy hello packages as hello Publish commands in Visual Studio do.</span></span>

   <span data-ttu-id="ff653-139">Szükség esetén megadhatja az MSBuild paraméter hello projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="ff653-139">Optionally, you can specify hello project name as an MSBuild parameter.</span></span> <span data-ttu-id="ff653-140">Ha nincs megadva, az aktuális könyvtár hello szolgál.</span><span class="sxs-lookup"><span data-stu-id="ff653-140">If not specified, hello current directory is used.</span></span> <span data-ttu-id="ff653-141">MSBuild parancssori beállításokkal kapcsolatos további információkért lásd: [MSBuild parancssori hivatkozás](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="ff653-141">For more information about MSBuild command line options, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span></span>
4. <span data-ttu-id="ff653-142">Keresse meg a hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="ff653-142">Locate hello output.</span></span> <span data-ttu-id="ff653-143">Alapértelmezés szerint ez a parancs létrehoz egy könyvtárat kapcsolat toohello gyökérmappában hello projekthez, például a *ProjectDir*\\bin\\*konfigurációs* \\ App.publish\\.</span><span class="sxs-lookup"><span data-stu-id="ff653-143">By default, this command creates a directory in relation toohello root folder for hello project, such as *ProjectDir*\\bin\\*Configuration*\\app.publish\\.</span></span> <span data-ttu-id="ff653-144">Azure-projekt összeállításakor létrehozhat két fájlt, a hello csomagfájl és a konfigurációs fájl kísérő hello:</span><span class="sxs-lookup"><span data-stu-id="ff653-144">When you build an Azure project, you generate two files, hello package file itself and hello accompanying configuration file:</span></span>

   * <span data-ttu-id="ff653-145">Project.cspkg</span><span class="sxs-lookup"><span data-stu-id="ff653-145">Project.cspkg</span></span>
   * <span data-ttu-id="ff653-146">ServiceConfiguration. *TargetProfile*.cscfg</span><span class="sxs-lookup"><span data-stu-id="ff653-146">ServiceConfiguration.*TargetProfile*.cscfg</span></span>

   <span data-ttu-id="ff653-147">Alapértelmezés szerint minden Azure-projekt tartalmazza egy szolgáltatás konfigurációs fájljában (.cscfg fájl) (hibakereséshez) helyi buildek és egy másikat a felhő (átmeneti vagy üzemi) buildek, de adhat hozzá vagy távolítsa el a szolgáltatás konfigurációs fájlokat, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="ff653-147">By default, each Azure project includes one service configuration file (.cscfg file) for local (debugging) builds and another for cloud (staging or production) builds, but you can add or remove service configuration files as needed.</span></span> <span data-ttu-id="ff653-148">Visual Studio csomag összeállításakor kérni fogja melyik szolgáltatás konfigurációs fájl tooinclude hello csomag mellett.</span><span class="sxs-lookup"><span data-stu-id="ff653-148">When you build a package within Visual Studio, you will be asked which service configuration file tooinclude alongside hello package.</span></span>
5. <span data-ttu-id="ff653-149">Adja meg a hello szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="ff653-149">Specify hello service configuration file.</span></span> <span data-ttu-id="ff653-150">Ha egy csomag MSBuild hozhat létre, hello helyi szolgáltatás konfigurációs fájlja veszi fel alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="ff653-150">When you build a package by using MSBuild, hello local service configuration file is included by default.</span></span> <span data-ttu-id="ff653-151">egy másik szolgáltatás konfigurációs fájlja tooinclude hello MSBuild parancs, mint például a következő hello TargetProfile tulajdonságának beállítása:</span><span class="sxs-lookup"><span data-stu-id="ff653-151">tooinclude a different service configuration file, set the TargetProfile property of hello MSBuild command, as in hello following example:</span></span>

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. <span data-ttu-id="ff653-152">Adjon meg hello kimeneti hello helyet.</span><span class="sxs-lookup"><span data-stu-id="ff653-152">Specify hello location for hello output.</span></span> <span data-ttu-id="ff653-153">A /p:PublishDir használatával set hello elérési út =*Directory* \\ beállítást, többek között a záró perjelet elválasztó, mint például a következő hello hello:</span><span class="sxs-lookup"><span data-stu-id="ff653-153">Set hello path by using the /p:PublishDir=*Directory*\\ option, including hello trailing backslash separator, as in hello following example:</span></span>

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   <span data-ttu-id="ff653-154">Amennyiben az előre összeállított és tesztelt egy megfelelő MSBuild sor toobuild a projektek parancsot, és összefogni azokat az Azure-csomag, a parancssor tooyour build parancsprogramokat is megadhat.</span><span class="sxs-lookup"><span data-stu-id="ff653-154">Once you've constructed and tested an appropriate MSBuild command line toobuild your projects and combine them into an Azure package, you can add this command line tooyour build scripts.</span></span> <span data-ttu-id="ff653-155">Ha a build server egyéni parancsfájlokat használ, ez a folyamat az egyéni létrehozási folyamata során a mintaadatokról függ.</span><span class="sxs-lookup"><span data-stu-id="ff653-155">If your build server uses custom scripts, this process will depend on the specifics of your custom build process.</span></span> <span data-ttu-id="ff653-156">Ha a TFS egy összeállító környezetet használ, majd kövesse hello tovább lépés tooadd hello Azure-csomag létrehozási tooyour felépítési folyamat hello utasításait.</span><span class="sxs-lookup"><span data-stu-id="ff653-156">If you are using TFS as a build environment, then you can follow hello instructions in hello next step tooadd hello Azure package build tooyour build process.</span></span>

## <a name="3-build-a-package-using-tfs-team-build"></a><span data-ttu-id="ff653-157">3: build a csomagot, a TFS-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ff653-157">3: Build a Package using TFS Team Build</span></span>
<span data-ttu-id="ff653-158">Ha TFS létrehozási gépként beállítása, a build vezérlő és hello készítsen a kiszolgáló beállítása Team Foundation Server (TFS), majd igény szerint állíthatja be az automatikus létrehozási az Azure-csomagnak a.</span><span class="sxs-lookup"><span data-stu-id="ff653-158">If you have Team Foundation Server (TFS) set up as a build controller and hello build server set up as a TFS build machine, then you can optionally set up an automated build for your Azure package.</span></span> <span data-ttu-id="ff653-159">Hogyan tooset be és a Team Foundation server használatát a buildelési rendszer: kapcsolatos [a buildelési rendszer kibővítési][Scale out your build system].</span><span class="sxs-lookup"><span data-stu-id="ff653-159">For information on how tooset up and use Team Foundation server as a build system, see [Scale out your build system][Scale out your build system].</span></span> <span data-ttu-id="ff653-160">Különösen a következő eljárás feltételezi, hogy konfigurálta a build kiszolgáló leírtak [központi telepítése és build kiszolgálók][Deploy and configure a build server], és létrehozott egy csapatprojekt létrehozott felhő Projekt hello csapatprojektben.</span><span class="sxs-lookup"><span data-stu-id="ff653-160">In particular, the following procedure assumes that you have configured your build server as described in [Deploy and configure a build server][Deploy and configure a build server], and that you have created a team project, created a cloud service project in hello team project.</span></span>

<span data-ttu-id="ff653-161">tooconfigure TFS toobuild Azure csomagok, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="ff653-161">tooconfigure TFS toobuild Azure packages, perform hello following steps:</span></span>

1. <span data-ttu-id="ff653-162">Válassza ki a Visual Studio a fejlesztési számítógépen hello Nézet menü **Team Explorer**, vagy válassza a Ctrl +\\, Ctrl + M.</span><span class="sxs-lookup"><span data-stu-id="ff653-162">In Visual Studio on your development computer, on hello View menu, choose **Team Explorer**, or choose Ctrl+\\, Ctrl+M.</span></span> <span data-ttu-id="ff653-163">A csapat Explorer-ablakban bontsa ki a hello **buildek** csomópont, vagy válasszon hello **buildek** lapon, és válassza a **új Build Definition**.</span><span class="sxs-lookup"><span data-stu-id="ff653-163">In the Team Explorer window, expand hello **Builds** node or choose hello **Builds** page, and choose **New Build Definition**.</span></span>

   ![Új beállítás található definíció létrehozása][0]
2. <span data-ttu-id="ff653-165">Válassza ki a hello **eseményindító** lapot, és adja meg a hello szükséges feltételeit, ha azt szeretné hello beépített csomag toobe.</span><span class="sxs-lookup"><span data-stu-id="ff653-165">Choose hello **Trigger** tab, and specify hello desired conditions for when you want hello package toobe built.</span></span> <span data-ttu-id="ff653-166">Adja meg például **folyamatos integrációt** akkor fordul elő, amikor egy forrás szabályozása be toobuild hello csomag.</span><span class="sxs-lookup"><span data-stu-id="ff653-166">For example, specify **Continuous Integration** toobuild hello package whenever a source control check-in occurs.</span></span>
3. <span data-ttu-id="ff653-167">Válassza ki a hello **adatforrás-beállítások** lapot, és győződjön meg arról, hogy a projektmappa hello szerepel **vezérlő forrásmappa** oszlop, és hello állapota **Active**.</span><span class="sxs-lookup"><span data-stu-id="ff653-167">Choose hello **Source Settings** tab, and make sure your project folder is listed in hello **Source Control Folder** column, and hello status is **Active**.</span></span>
4. <span data-ttu-id="ff653-168">Válassza ki a hello **Build alapértelmezett** fülre, és a Build vezérlő, ellenőrizze a hello hello build kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="ff653-168">Choose hello **Build Defaults** tab, and under Build controller, verify hello name of hello build server.</span></span>  <span data-ttu-id="ff653-169">Válassza ki, hello beállítás **másolási build kimeneti toohello következő drop mappa** , és adja meg a szükséges hello eldobott üzenetek helye.</span><span class="sxs-lookup"><span data-stu-id="ff653-169">Also, choose hello option **Copy build output toohello following drop folder** and specify hello desired drop location.</span></span>
5. <span data-ttu-id="ff653-170">Válassza ki a hello **folyamat** fülre. Hello folyamat lapon válassza a hello alapértelmezett sablon, a **Build**, hello projekt válassza, ha nincs bejelölve, és bontsa ki a hello **speciális** hello szakasz **Build**hello rács szakasza.</span><span class="sxs-lookup"><span data-stu-id="ff653-170">Choose hello **Process** tab. On hello Process tab, choose hello default template, under **Build**, choose hello project if it is not already selected, and expand hello **Advanced** section in hello **Build** section of hello grid.</span></span>
6. <span data-ttu-id="ff653-171">Válasszon **MSBuild-argumentumok**, és állítsa be a megfelelő MSBuild parancssori argumentumok hello fenti a 2. lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="ff653-171">Choose **MSBuild Arguments**, and set hello appropriate MSBuild command line arguments as described in Step 2 above.</span></span> <span data-ttu-id="ff653-172">Adja meg például **/t: /p:PublishDir közzététele =\\\\myserver\\esik\\**  toobuild egy csomagot, és másolja hello csomag fájlok toohello hely \\ \\myserver\\esik\\:</span><span class="sxs-lookup"><span data-stu-id="ff653-172">For example, enter **/t:Publish /p:PublishDir=\\\\myserver\\drops\\** toobuild a package and copy hello package files toohello location \\\\myserver\\drops\\:</span></span>

   ![MSBuild-argumentumok][2]

   > [!NOTE]
   > <span data-ttu-id="ff653-174">Másolás hello fájlok tooa nyilvános megosztás teszi egyszerűbbé toomanually a fejlesztési számítógépen hello csomagok központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="ff653-174">Copying hello files tooa public share makes it easier toomanually deploy hello packages from your development computer.</span></span>
7. <span data-ttu-id="ff653-175">A build lépés hello sikeres teszteléséhez módosítása tooyour projektben ellenőrzésével, vagy egy új build sorba.</span><span class="sxs-lookup"><span data-stu-id="ff653-175">Test hello success of your build step by checking in a change tooyour project, or queue up a new build.</span></span> <span data-ttu-id="ff653-176">mentése új buildverziót, a csapat Explorer tooqueue kattintson a jobb gombbal **összes Build definíciókat,** majd **várólista új Build**.</span><span class="sxs-lookup"><span data-stu-id="ff653-176">tooqueue up a new build, in the Team Explorer, right-click **All Build Definitions,** and then choose **Queue New Build**.</span></span>

## <a name="4-publish-a-package-using-a-powershell-script"></a><span data-ttu-id="ff653-177">4: a PowerShell-parancsfájl használatával csomag közzététele</span><span class="sxs-lookup"><span data-stu-id="ff653-177">4: Publish a Package using a PowerShell Script</span></span>
<span data-ttu-id="ff653-178">Ez a szakasz ismerteti, hogyan tooconstruct hello felhő alkalmazáscsomag közzétehető Windows PowerShell-parancsfájl kimeneti tooAzure opcionális paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="ff653-178">This section describes how tooconstruct a Windows PowerShell script that will publish hello Cloud app package output tooAzure using optional parameters.</span></span> <span data-ttu-id="ff653-179">Ez a parancsfájl hello build a build egyéni automatizálás lépés után hívható.</span><span class="sxs-lookup"><span data-stu-id="ff653-179">This script can be called after hello build step in your custom build automation.</span></span> <span data-ttu-id="ff653-180">Azt a folyamatsablon munkafolyamat tevékenységei a Visual Studio TFS Team Build is hívható.</span><span class="sxs-lookup"><span data-stu-id="ff653-180">It can also be called from Process Template workflow activities in Visual Studio TFS Team Build.</span></span>

1. <span data-ttu-id="ff653-181">Telepítse a hello [Azure PowerShell-parancsmagok] [ Azure PowerShell cmdlets] (v0.6.1 vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="ff653-181">Install hello [Azure PowerShell cmdlets][Azure PowerShell cmdlets] (v0.6.1 or higher).</span></span>
   <span data-ttu-id="ff653-182">Hello parancsmag telepítési fázis során válassza tooinstall egy beépülő modulként.</span><span class="sxs-lookup"><span data-stu-id="ff653-182">During hello cmdlet setup phase, choose tooinstall as a snap-in.</span></span> <span data-ttu-id="ff653-183">Vegye figyelembe, hogy a hivatalosan támogatott felváltja hello régebbi kínált a Codeplex webhelyen, bár a korábbi verziók hello volt számozott 2.x.x.</span><span class="sxs-lookup"><span data-stu-id="ff653-183">Note that this officially supported version replaces hello older version offered through CodePlex, although hello previous versions were numbered 2.x.x.</span></span>
2. <span data-ttu-id="ff653-184">Indítsa el az Azure PowerShell hello Start menüből vagy a kezdőlapot.</span><span class="sxs-lookup"><span data-stu-id="ff653-184">Start Azure PowerShell using hello Start menu or Start page.</span></span> <span data-ttu-id="ff653-185">Ily módon indul el, ha hello Azure PowerShell-parancsmagok lesz betöltve.</span><span class="sxs-lookup"><span data-stu-id="ff653-185">If you start in this way, hello Azure PowerShell cmdlets will be loaded.</span></span>
3. <span data-ttu-id="ff653-186">Hello PowerShell-parancssorba, győződjön meg arról, hogy a PowerShell-parancsmagok hello hello részleges parancs megadásával vannak betöltve `Get-Azure` és hello majd nyomja le a Tab billentyű utasítás befejezésére.</span><span class="sxs-lookup"><span data-stu-id="ff653-186">At hello PowerShell prompt, verify that hello PowerShell cmdlets are loaded by entering hello partial command `Get-Azure` and then pressing hello Tab key for statement completion.</span></span>

   <span data-ttu-id="ff653-187">Ha ismételten hello Tab billentyű lenyomása, különböző Azure PowerShell-parancsokat kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="ff653-187">If you press hello Tab key repeatedly, you should see various Azure PowerShell commands.</span></span>
4. <span data-ttu-id="ff653-188">Győződjön meg arról, hogy Azure-előfizetés tooyour csatlakozhasson ehhez hello .publishsettings fájlt importálja az előfizetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="ff653-188">Verify that you can connect tooyour Azure subscription by importing your subscription information from hello .publishsettings file.</span></span>

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   <span data-ttu-id="ff653-189">Majd adja meg a hello parancsot</span><span class="sxs-lookup"><span data-stu-id="ff653-189">Then enter hello command</span></span>

   `Get-AzureSubscription`

   <span data-ttu-id="ff653-190">Ez az előfizetés információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ff653-190">This shows information about your subscription.</span></span> <span data-ttu-id="ff653-191">Győződjön meg arról, hogy minden rendben.</span><span class="sxs-lookup"><span data-stu-id="ff653-191">Verify that everything is correct.</span></span>
5. <span data-ttu-id="ff653-192">Ez a cikk a parancsfájlok mappába c: hello végén megadott hello parancsfájl sablon mentése\\parancsfájlok\\WindowsAzure\\**PublishCloudService.ps1**.</span><span class="sxs-lookup"><span data-stu-id="ff653-192">Save hello script template provided at hello end of this article to your scripts folder as c:\\scripts\\WindowsAzure\\**PublishCloudService.ps1**.</span></span>
6. <span data-ttu-id="ff653-193">Tekintse át a hello paraméterek szakaszban hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="ff653-193">Review hello parameters section of hello script.</span></span> <span data-ttu-id="ff653-194">Adja hozzá, vagy módosítsa az alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="ff653-194">Add or modify any default values.</span></span> <span data-ttu-id="ff653-195">Sikeres explicit paraméterek mindig felülbírálhatja ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ff653-195">These values can always be overridden by passing in explicit parameters.</span></span>
7. <span data-ttu-id="ff653-196">Győződjön meg arról, érvényes felhőalapú szolgáltatás, és az előfizetés, amely telepíthető hello által létrehozott tárfiókok közzététele parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="ff653-196">Ensure there are valid cloud service and storage accounts created in your subscription that can be targeted by hello publish script.</span></span> <span data-ttu-id="ff653-197">A tárfiók (blob-tároló) használt tooupload kell, és a központi telepítési csomag- és konfigurációs fájl hello ideiglenesen tárolja, a központi telepítés létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="ff653-197">The storage account (blob storage) will be used tooupload and temporarily store hello deployment package and config file while the deployment is being created.</span></span>

   * <span data-ttu-id="ff653-198">új felhőalapú szolgáltatás toocreate, hívása a parancsfájl vagy használata hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ff653-198">toocreate a new cloud service, you can call this script or use hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="ff653-199">egy teljesen minősített tartománynevet az előtag hello felhőszolgáltatás neve lesz, és ezért egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ff653-199">hello cloud service name will be used as a prefix in a fully qualified domain name and hence it must be unique.</span></span>

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * <span data-ttu-id="ff653-200">új tárfiók toocreate, hívása a parancsfájl vagy használata hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ff653-200">toocreate a new storage account, you can call this script or use hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="ff653-201">egy teljesen minősített tartománynevet az előtag hello tárfiók neve lesz, és ezért egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ff653-201">hello storage account name will be used as a prefix in a fully qualified domain name and hence it must be unique.</span></span> <span data-ttu-id="ff653-202">Próbálja meg hello ugyanazt a nevet használja, mint a felhőalapú szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="ff653-202">You can try using hello same name as the cloud service.</span></span>

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. <span data-ttu-id="ff653-203">Hello parancsfájl hívása közvetlenül az Azure PowerShell, illetve a hello csomag build után be a parancsfájl tooyour állomás build automation toooccur vezetékes.</span><span class="sxs-lookup"><span data-stu-id="ff653-203">Call hello script directly from Azure PowerShell, or wire up this script tooyour host build automation toooccur after hello package build.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="ff653-204">hello parancsfájl mindig törölje, vagy cserélje le a meglévő telepítések alapértelmezés szerint, ha azokat.</span><span class="sxs-lookup"><span data-stu-id="ff653-204">hello script will always delete or replace your existing deployments by default if they are detected.</span></span> <span data-ttu-id="ff653-205">Ez akkor szükségesek, hogy lehetővé teszik a folyamatos Automation ahol nincs felhasználói adatkérésekhez lehetséges.</span><span class="sxs-lookup"><span data-stu-id="ff653-205">This is necessary to enable continuous delivery from automation where no user prompting is possible.</span></span>
   >
   >

   <span data-ttu-id="ff653-206">**1. példa:** átmeneti környezetben a szolgáltatások folyamatos üzembe helyezés toohello:</span><span class="sxs-lookup"><span data-stu-id="ff653-206">**Example scenario 1:** continuous deployment toohello staging environment of a service:</span></span>

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   <span data-ttu-id="ff653-207">Ez általában következnek teszt futtatásakor ellenőrzési és a virtuális IP-címcsere.</span><span class="sxs-lookup"><span data-stu-id="ff653-207">This is typically followed up by test run verification and a VIP swap.</span></span> <span data-ttu-id="ff653-208">hello VIP swap megteheti a hello [Azure-portálon](https://portal.azure.com) vagy hello áthelyezés telepítési parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="ff653-208">hello VIP swap can be done via hello [Azure portal](https://portal.azure.com) or by using hello Move-Deployment cmdlet.</span></span>

   <span data-ttu-id="ff653-209">**2. példa:** folyamatos üzembe helyezés toohello éles környezetben található dedikált teszt szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ff653-209">**Example scenario 2:** continuous deployment toohello production environment of a dedicated test service</span></span>

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   <span data-ttu-id="ff653-210">**Távoli asztali:**</span><span class="sxs-lookup"><span data-stu-id="ff653-210">**Remote Desktop:**</span></span>

   <span data-ttu-id="ff653-211">Ha a távoli asztal engedélyezve van az Azure-projekt tooperform további egyszeri lépéseket tooensure hello felhőalapú szolgáltatás megfelelő tanúsítvány van feltöltve. Ez a parancsfájl által megcélzott tooall felhőszolgáltatások szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="ff653-211">If Remote Desktop is enabled in your Azure project you will need tooperform additional one-time steps tooensure hello correct Cloud Service Certificate is uploaded tooall cloud services targeted by this script.</span></span>

   <span data-ttu-id="ff653-212">Keresse meg a szerepkörök által várt hello tanúsítvány ujjlenyomata értékeket.</span><span class="sxs-lookup"><span data-stu-id="ff653-212">Locate hello certificate thumbprint values expected by your roles.</span></span> <span data-ttu-id="ff653-213">Az ujjlenyomat értékek láthatók a hello tanúsítványok szakasz a felhőalapú konfigurációs fájl (pl. ServiceConfiguration.Cloud.cscfg).</span><span class="sxs-lookup"><span data-stu-id="ff653-213">The thumbprint values are visible in hello Certificates section of the cloud config file (i.e. ServiceConfiguration.Cloud.cscfg).</span></span> <span data-ttu-id="ff653-214">Egyben hello távoli asztali konfigurálása párbeszédpanel a Visual Studio alkalmazásban látható beállítások megjelenítése és a nézet hello kiválasztott tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="ff653-214">It is also visible in hello Remote Desktop Configuration dialog in Visual Studio when you Show Options and view hello selected certificate.</span></span>

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   <span data-ttu-id="ff653-215">A távoli asztal-tanúsítványok feltöltésére használja a következő parancsmag parancsfájl hello egyszeri telepítő lépésként:</span><span class="sxs-lookup"><span data-stu-id="ff653-215">Upload Remote Desktop certificates as a one-time setup step using hello following cmdlet script:</span></span>

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   <span data-ttu-id="ff653-216">Példa:</span><span class="sxs-lookup"><span data-stu-id="ff653-216">For example:</span></span>

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   <span data-ttu-id="ff653-217">Másik lehetőségként exportálhatja hello tanúsítványfájl PFX a titkos kulcs és a feltöltés tanúsítványok tooeach cél felhőalapú szolgáltatás használata a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ff653-217">Alternatively you can export hello certificate file PFX with private key and upload certificates tooeach target cloud service using the [Azure portal](https://portal.azure.com).</span></span>

   <!---
   Fixing broken links for Azure content migration from ACOM tooDOCS. I'm unable toofind a replacement links, so I'm commenting out this reference for now. hello author can investigate in hello future. "Read hello following article toolearn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   <span data-ttu-id="ff653-218">**Frissítés a központi telepítés vagy. Törölje a központi telepítés –\> új központi telepítés**</span><span class="sxs-lookup"><span data-stu-id="ff653-218">**Upgrade Deployment vs. Delete Deployment -\> New Deployment**</span></span>

   <span data-ttu-id="ff653-219">hello parancsprogrammal alapértelmezés szerint végezheti az frissítés központi telepítése ($enableDeploymentUpgrade = 1) Ha nem paraméter átadott vagy explicit módon átadott 1 érték.</span><span class="sxs-lookup"><span data-stu-id="ff653-219">hello script will by default perform an Upgrade Deployment ($enableDeploymentUpgrade = 1) when no parameter is passed in or the value 1 is passed explicitly.</span></span> <span data-ttu-id="ff653-220">Az egyetlen példány Ez azt az előnyt, teljes körű telepítésére rövidebb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="ff653-220">For single instances this has the advantage of taking less time than a full deployment.</span></span> <span data-ttu-id="ff653-221">A példányok hello előnye, hogy bizonyos esetekben fut, míg mások is annak magas rendelkezésre állást igénylő frissítése (a frissítési tartomány érdekében), valamint a VIP nem lesz törölve.</span><span class="sxs-lookup"><span data-stu-id="ff653-221">For instances that require high availability this also has hello advantage of leaving some instances running while others are upgraded (walking your update domain), plus your VIP will not be deleted.</span></span>

   <span data-ttu-id="ff653-222">Frissítés telepítése is le kell tiltani a hello parancsfájl ($enableDeploymentUpgrade = 0), vagy úgy, hogy *- enableDeploymentUpgrade 0* paraméterként, amely megváltoztatja a parancsfájl viselkedés toofirst törölje a meglévő telepítést, és hozzon létre egy Új központi telepítést.</span><span class="sxs-lookup"><span data-stu-id="ff653-222">Upgrade Deployment can be disabled in hello script ($enableDeploymentUpgrade = 0) or by passing *-enableDeploymentUpgrade 0* as a parameter, which alters the script behavior toofirst delete any existing deployment and then create a new deployment.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="ff653-223">hello parancsfájl mindig törölje, vagy cserélje le a meglévő telepítések alapértelmezés szerint, ha azokat.</span><span class="sxs-lookup"><span data-stu-id="ff653-223">hello script will always delete or replace your existing deployments by default if they are detected.</span></span> <span data-ttu-id="ff653-224">Ez az Automation folyamatos kézbesítési ahhoz szükséges, hogy nincs felhasználói/operátor kérdés esetén lehetséges.</span><span class="sxs-lookup"><span data-stu-id="ff653-224">This is necessary to enable continuous delivery from automation where no user/operator prompting is possible.</span></span>
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a><span data-ttu-id="ff653-225">5: közzétenni a csomagot, a TFS-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ff653-225">5: Publish a Package using TFS Team Build</span></span>
<span data-ttu-id="ff653-226">Ez az opcionális lépés csatlakozik a TFS-csoport létrehozása toohello parancsfájl a 4, amely kezeli a hello csomag build tooAzure közzétételét.</span><span class="sxs-lookup"><span data-stu-id="ff653-226">This optional step connects TFS Team Build toohello script created in step 4, which handles publishing of hello package build tooAzure.</span></span> <span data-ttu-id="ff653-227">Ez a módosítása hello használják a build definition hello végén lévő hello munkafolyamat futtatása egy közzétételi tevékenység folyamatsablon terjed ki.</span><span class="sxs-lookup"><span data-stu-id="ff653-227">This entails modifying hello Process Template used by your build definition so that it runs a Publish activity at hello end of hello workflow.</span></span> <span data-ttu-id="ff653-228">Közzététel tevékenység hello végrehajtja a hello build a paraméterek átadása a PowerShell-parancsot.</span><span class="sxs-lookup"><span data-stu-id="ff653-228">hello Publish activity will execute your PowerShell command passing in parameters from hello build.</span></span> <span data-ttu-id="ff653-229">Hello MSBuild célozza, és tegye közzé a parancsfájl kimenete fog kell adatcsatornán hello szabványos felépítési művelet kimenetében be.</span><span class="sxs-lookup"><span data-stu-id="ff653-229">Output of hello MSBuild targets and publish script will be piped into hello standard build output.</span></span>

1. <span data-ttu-id="ff653-230">Hello felelős Build definíciójának szerkesztése folyamatos központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="ff653-230">Edit hello Build Definition responsible for continuous deploy.</span></span>
2. <span data-ttu-id="ff653-231">Jelölje be hello **folyamat** fülre.</span><span class="sxs-lookup"><span data-stu-id="ff653-231">Select hello **Process** tab.</span></span>
3. <span data-ttu-id="ff653-232">Hajtsa végre a [ezeket az utasításokat](http://msdn.microsoft.com/library/dd647551.aspx) tooadd hello tevékenység projektben folyamatsablon felépítéséhez, töltse le a hello alapértelmezett sablon, adja hozzá a projekthez hello és be.</span><span class="sxs-lookup"><span data-stu-id="ff653-232">Follow [these instructions](http://msdn.microsoft.com/library/dd647551.aspx) tooadd an Activity project for hello build process template, download hello default template, add it to hello project and check it in.</span></span> <span data-ttu-id="ff653-233">Adjon meg új nevet, például a AzureBuildProcessTemplate hello build folyamatsablon.</span><span class="sxs-lookup"><span data-stu-id="ff653-233">Give hello build process template a new name, such as AzureBuildProcessTemplate.</span></span>
4. <span data-ttu-id="ff653-234">Térjen vissza a toohello **folyamat** lapot, és használja **részletek megjelenítése** tooshow érhető el összeállítási folyamat sablonok listájának.</span><span class="sxs-lookup"><span data-stu-id="ff653-234">Return toohello **Process** tab, and use **Show Details** tooshow a list of available build process templates.</span></span> <span data-ttu-id="ff653-235">Válassza ki a hello **új...**  gombra, és keresse meg a most hozzáadott és be van jelölve, toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="ff653-235">Choose hello **New...** button, and navigate toohello project you just added and checked in.</span></span> <span data-ttu-id="ff653-236">Keresse meg az imént létrehozott hello sablont, és válassza a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff653-236">Locate hello template you just created and choose **OK**.</span></span>
5. <span data-ttu-id="ff653-237">Nyissa meg hello folyamatsablon kijelölt szerkesztésre.</span><span class="sxs-lookup"><span data-stu-id="ff653-237">Open hello selected Process Template for editing.</span></span> <span data-ttu-id="ff653-238">Úgy is megnyithatja közvetlenül hello munkafolyamat-tervezőben vagy hello XML-szerkesztő toowork a hello XAML-kódot.</span><span class="sxs-lookup"><span data-stu-id="ff653-238">You can open directly in hello Workflow designer or in hello XML editor toowork with hello XAML.</span></span>
6. <span data-ttu-id="ff653-239">Adja hozzá a következő új argumentumok listájának hello argumentumok lapján hello munkafolyamat-Tervező külön sorként hello.</span><span class="sxs-lookup"><span data-stu-id="ff653-239">Add hello following list of new arguments as separate line items in hello arguments tab of hello workflow designer.</span></span> <span data-ttu-id="ff653-240">Minden argumentum irányba kell rendelkeznie, és írja be a = = karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="ff653-240">All arguments should have direction=In and type=String.</span></span> <span data-ttu-id="ff653-241">Ezek mely majd get használt toocall hello közzététele parancsfájl hello munkafolyamatba hello build definícióból használt tooflow paraméterek lesz.</span><span class="sxs-lookup"><span data-stu-id="ff653-241">These will be used tooflow parameters from hello build definition into hello workflow, which then get used toocall hello publish script.</span></span>

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Argumentumok listájának megjelenítése][3]

   <span data-ttu-id="ff653-243">hello megfelelő XAML néz ki:</span><span class="sxs-lookup"><span data-stu-id="ff653-243">hello corresponding XAML looks like this:</span></span>

       <Activity  _ />
         <x:Members>
           <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
           <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
           <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
           <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
           <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
           <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
           <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
           <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
           <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
           <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
           <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
           <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
           <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
           <x:Property Name="GetVersion" Type="InArgument(x:String)" />
           <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
           <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
           <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
           <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
           <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
           <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
           <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
           <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
           <x:Property Name="Environment" Type="InArgument(x:String)" />
           <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
           <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
           <x:Property Name="ServiceName" Type="InArgument(x:String)" />
         </x:Members>

         <this:Process.MSBuildArguments>
7. <span data-ttu-id="ff653-244">Felvehet egy új sorozatot futtassa az ügynök hello végén:</span><span class="sxs-lookup"><span data-stu-id="ff653-244">Add a new sequence at hello end of Run On Agent:</span></span>

   1. <span data-ttu-id="ff653-245">Először vegyen fel egy Ha utasítás tevékenység toocheck meg egy érvényes parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="ff653-245">Start by adding an If Statement activity toocheck for a valid script file.</span></span> <span data-ttu-id="ff653-246">Hello feltétel toothis érték beállítása:</span><span class="sxs-lookup"><span data-stu-id="ff653-246">Set hello condition toothis value:</span></span>

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. <span data-ttu-id="ff653-247">Hello majd hello Ha utasítás esetében adja hozzá egy új feladatütemezési tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="ff653-247">In hello Then case of hello If Statement, add a new Sequence activity.</span></span> <span data-ttu-id="ff653-248">Set hello megjelenítési név too'Start közzététele "</span><span class="sxs-lookup"><span data-stu-id="ff653-248">Set hello display name too'Start publish'</span></span>
   3. <span data-ttu-id="ff653-249">A Start közzététele a kijelölt feladatütemezési hello adja hozzá az alábbi listán szereplő új változók a munkafolyamat-Tervező hello lapján elemeinek külön sorban.</span><span class="sxs-lookup"><span data-stu-id="ff653-249">With hello Start publish sequence still selected, add the following list of new variables as separate line items in the variables tab of hello workflow designer.</span></span> <span data-ttu-id="ff653-250">Minden változót kell változótípus = karakterlánc és a hatókör = Start közzététele.</span><span class="sxs-lookup"><span data-stu-id="ff653-250">All variables should have Variable type =String and Scope=Start publish.</span></span> <span data-ttu-id="ff653-251">Ezek a munkafolyamatba, mely majd get használt toocall hello közzététele parancsfájl hello build definícióból használt tooflow paraméterek lesz.</span><span class="sxs-lookup"><span data-stu-id="ff653-251">These will be used tooflow parameters from hello build definition into the workflow, which then get used toocall hello publish script.</span></span>

      * <span data-ttu-id="ff653-252">SubscriptionDataFilePath, karakterlánc típusú</span><span class="sxs-lookup"><span data-stu-id="ff653-252">SubscriptionDataFilePath, of type String</span></span>
      * <span data-ttu-id="ff653-253">PublishScriptFilePath, karakterlánc típusú</span><span class="sxs-lookup"><span data-stu-id="ff653-253">PublishScriptFilePath, of type String</span></span>

        ![Új változók][4]
   4. <span data-ttu-id="ff653-255">Ha a TFS 2012 használ, vagy korábbi, adjon hozzá egy ConvertWorkspaceItem tevékenységet a hello elején hello új feladatütemezési.</span><span class="sxs-lookup"><span data-stu-id="ff653-255">If you are using TFS 2012 or earlier, add a ConvertWorkspaceItem activity at hello beginning of hello new Sequence.</span></span> <span data-ttu-id="ff653-256">Ha a TFS 2013, vagy később, adjon hozzá egy GetLocalPath tevékenységet hello új feladatütemezési hello elején.</span><span class="sxs-lookup"><span data-stu-id="ff653-256">If you are using TFS 2013 or later, add a GetLocalPath activity at hello beginning of hello new sequence.</span></span> <span data-ttu-id="ff653-257">Egy ConvertWorkspaceItem beállítása hello tulajdonságok az alábbiak szerint: irányát ServerToLocal, DisplayName = = 'Convert közzététele parancsfájl fájlnév', bemeneti = "PublishScriptLocation", az eredmény = "PublishScriptFilePath", a munkaterület = "Munkaterület".</span><span class="sxs-lookup"><span data-stu-id="ff653-257">For a ConvertWorkspaceItem, set hello properties as follows: Direction=ServerToLocal, DisplayName='Convert publish script filename', Input=' PublishScriptLocation', Result='PublishScriptFilePath', Workspace='Workspace'.</span></span> <span data-ttu-id="ff653-258">GetLocalPath tevékenységhez, állítsa be a hello tulajdonság IncomingPath too'PublishScriptLocation ", és az eredmény too'PublishScriptFilePath hello".</span><span class="sxs-lookup"><span data-stu-id="ff653-258">For a GetLocalPath activity, set hello property IncomingPath too'PublishScriptLocation', and hello Result too'PublishScriptFilePath'.</span></span> <span data-ttu-id="ff653-259">A tevékenység konvertálja hello elérési toohello közzététele TFS helyekről parancsfájl (ha van ilyen) tooa szabványos helyi lemez elérési útja.</span><span class="sxs-lookup"><span data-stu-id="ff653-259">This activity converts hello path toohello publish script from TFS server locations (if applicable) tooa standard local disk path.</span></span>
   5. <span data-ttu-id="ff653-260">Ha a TFS 2012 használ, vagy korábbi, adjon hozzá egy másik ConvertWorkspaceItem tevékenységet, hello végén lévő hello új feladatütemezési.</span><span class="sxs-lookup"><span data-stu-id="ff653-260">If you are using TFS 2012 or earlier, add another ConvertWorkspaceItem activity at hello end of hello new Sequence.</span></span> <span data-ttu-id="ff653-261">Irány ServerToLocal, DisplayName = = "Átalakításáról előfizetés fájlnév" bemeneti = "SubscriptionDataFileLocation", az eredmény = "SubscriptionDataFilePath" munkaterület = "Munkaterület".</span><span class="sxs-lookup"><span data-stu-id="ff653-261">Direction=ServerToLocal, DisplayName='Convert subscription filename', Input=' SubscriptionDataFileLocation', Result= 'SubscriptionDataFilePath', Workspace='Workspace'.</span></span> <span data-ttu-id="ff653-262">Ha a TFS 2013, vagy később, a másik GetLocalPath hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="ff653-262">If you are using TFS 2013 or later, add another GetLocalPath.</span></span> <span data-ttu-id="ff653-263">IncomingPath = "SubscriptionDataFileLocation", és az eredmény = "SubscriptionDataFilePath."</span><span class="sxs-lookup"><span data-stu-id="ff653-263">IncomingPath='SubscriptionDataFileLocation', and Result='SubscriptionDataFilePath.'</span></span>
   6. <span data-ttu-id="ff653-264">Hello hello végén InvokeProcess tevékenység hozzáadása új feladatütemezési.</span><span class="sxs-lookup"><span data-stu-id="ff653-264">Add an InvokeProcess activity at hello end of hello new Sequence.</span></span>
      <span data-ttu-id="ff653-265">Build Definition hello által átadott e tevékenység hívások PowerShell.exe hello argumentumokkal.</span><span class="sxs-lookup"><span data-stu-id="ff653-265">This activity calls PowerShell.exe with hello arguments passed in by hello Build Definition.</span></span>

      + <span data-ttu-id="ff653-266">Argumentumok = String.Format ("-""{0}" "- szolgáltatásnév {1} - storageAccountName {2} - packageLocation""{3}" "- cloudConfigLocation""{4}" "– subscriptionDataFile""{5}" "– selectedSubscription {6} fájlt-környezet""{7}" "",  PublishScriptFilePath, ServiceName, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, környezet)</span><span class="sxs-lookup"><span data-stu-id="ff653-266">Arguments = String.Format(" -File ""{0}"" -serviceName {1}  -storageAccountName {2} -packageLocation ""{3}""  -cloudConfigLocation ""{4}"" -subscriptionDataFile ""{5}""  -selectedSubscription {6} -environment ""{7}""",  PublishScriptFilePath, ServiceName, StorageAccountName,  PackageLocation, CloudConfigLocation,  SubscriptionDataFilePath, SubscriptionName, Environment)</span></span>
      + <span data-ttu-id="ff653-267">DisplayName = Execute parancsfájl közzététele</span><span class="sxs-lookup"><span data-stu-id="ff653-267">DisplayName = Execute publish script</span></span>
      + <span data-ttu-id="ff653-268">FileName = "PowerShell" (a hello idézőjelek között)</span><span class="sxs-lookup"><span data-stu-id="ff653-268">FileName = "PowerShell" (include hello quotes)</span></span>
      + <span data-ttu-id="ff653-269">OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)</span><span class="sxs-lookup"><span data-stu-id="ff653-269">OutputEncoding=  System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)</span></span>
   7. <span data-ttu-id="ff653-270">A hello **szabványos kimeneti kezelni** a InvokeProcess a szövegmező területen állítsa be a hello szövegmező érték too'data ".</span><span class="sxs-lookup"><span data-stu-id="ff653-270">In hello **Handle Standard Output** section textbox of the InvokeProcess, set hello textbox value too'data'.</span></span> <span data-ttu-id="ff653-271">Ez az egy változó toostore hello szabványos kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="ff653-271">This is a variable toostore hello standard output data.</span></span>
   8. <span data-ttu-id="ff653-272">Adjon hozzá egy WriteBuildMessage tevékenységet hello alatt **szabványos kimeneti kezelni** szakasz.</span><span class="sxs-lookup"><span data-stu-id="ff653-272">Add a WriteBuildMessage activity just below hello **Handle Standard Output** section.</span></span> <span data-ttu-id="ff653-273">Fontos hello beállítása = "Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High" és a hello Message = "data".</span><span class="sxs-lookup"><span data-stu-id="ff653-273">Set hello Importance = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' and hello Message='data'.</span></span> <span data-ttu-id="ff653-274">Ez biztosítja a szabványos kimeneti hello parancsfájl fog írása toohello felépítési művelet kimenetében.</span><span class="sxs-lookup"><span data-stu-id="ff653-274">This ensures hello standard output of the script will get written toohello build output.</span></span>
   9. <span data-ttu-id="ff653-275">A hello **kezelni hiba kimeneti** a InvokeProcess a szövegmező területen állítsa be a hello szövegmező érték too'data ".</span><span class="sxs-lookup"><span data-stu-id="ff653-275">In hello **Handle Error Output** section textbox of the InvokeProcess, set hello textbox value too'data'.</span></span> <span data-ttu-id="ff653-276">Ez az egy változó toostore hello standard hiba adatok.</span><span class="sxs-lookup"><span data-stu-id="ff653-276">This is a variable toostore hello standard error data.</span></span>
   10. <span data-ttu-id="ff653-277">Adjon hozzá egy WriteBuildError tevékenységet hello alatt **kezelni hiba kimeneti** szakasz.</span><span class="sxs-lookup"><span data-stu-id="ff653-277">Add a WriteBuildError activity just below hello **Handle Error Output** section.</span></span> <span data-ttu-id="ff653-278">Állítsa be az üdvözlő üzenet = "data".</span><span class="sxs-lookup"><span data-stu-id="ff653-278">Set hello Message='data'.</span></span> <span data-ttu-id="ff653-279">Ez biztosítja, hogy toohello build hiba kimeneti hello standard hibák hello parancsfájl fog írása.</span><span class="sxs-lookup"><span data-stu-id="ff653-279">This ensures hello standard errors of hello script will get written toohello build error output.</span></span>
   11. <span data-ttu-id="ff653-280">Javítsa ki a hibákat, kék felkiáltójel jelek jelölik.</span><span class="sxs-lookup"><span data-stu-id="ff653-280">Correct any errors, indicated by blue exclamation marks.</span></span> <span data-ttu-id="ff653-281">Mutasson a felkiáltójel jelek tooget hello hiba vonatkozó javaslat.</span><span class="sxs-lookup"><span data-stu-id="ff653-281">Hover over the exclamation marks tooget a hint about hello error.</span></span> <span data-ttu-id="ff653-282">Törölje a hibák hello munkafolyamat mentése.</span><span class="sxs-lookup"><span data-stu-id="ff653-282">Save hello workflow to clear errors.</span></span>

   <span data-ttu-id="ff653-283">hello végeredményét hello munkafolyamat-tevékenységek fog kinézni hello Designer közzététele:</span><span class="sxs-lookup"><span data-stu-id="ff653-283">hello final result of hello publish workflow activities will look like this in hello designer:</span></span>

   ![Munkafolyamat-tevékenységek][5]

   <span data-ttu-id="ff653-285">hello végeredményét hello közzététele a munkafolyamat-tevékenységek fog kinézni XAML-kódban:</span><span class="sxs-lookup"><span data-stu-id="ff653-285">hello final result of hello publish workflow activities will look like this in XAML:</span></span>

       <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
           <If.Then>
             <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
               <Sequence.Variables>
                 <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                 <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
               </Sequence.Variables>
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
               <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                 <mtbwa:InvokeProcess.ErrorDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.ErrorDataReceived>
                 <mtbwa:InvokeProcess.OutputDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.OutputDataReceived>
               </mtbwa:InvokeProcess>
             </Sequence>
           </If.Then>
         </If>
       </Sequence>
8. <span data-ttu-id="ff653-286">Hello létrehozási munkafolyamat sablont, és ellenőrizze a fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="ff653-286">Save hello build process template workflow and Check In this file.</span></span>
9. <span data-ttu-id="ff653-287">Hello build definíciójának szerkesztése (lezárhatja, amennyiben már meg nyitva), és jelölje be hello **új** gombra kattint, ha még nem látja hello új sablon folyamatsablonok hello listájában.</span><span class="sxs-lookup"><span data-stu-id="ff653-287">Edit hello build definition (close it if it is already open), and select hello **New** button if you do not yet see hello new template in hello list of Process Templates.</span></span>
10. <span data-ttu-id="ff653-288">Hello paraméter tulajdonságértékei állíthatók hello egyebek szakasz a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="ff653-288">Set hello parameter property values in hello Misc section as follows:</span></span>

    1. <span data-ttu-id="ff653-289">CloudConfigLocation = "c:\\esik\\app.publish\\ServiceConfiguration.Cloud.cscfg" *ezt az értéket, amelyből származik: ($PublishDir)ServiceConfiguration.Cloud.cscfg*</span><span class="sxs-lookup"><span data-stu-id="ff653-289">CloudConfigLocation ='c:\\drops\\app.publish\\ServiceConfiguration.Cloud.cscfg' *This value is derived from: ($PublishDir)ServiceConfiguration.Cloud.cscfg*</span></span>
    2. <span data-ttu-id="ff653-290">PackageLocation = "c:\\esik\\app.publish\\ContactManager.Azure.cspkg" *ezt az értéket, amelyből származik: ($PublishDir)($ProjectName) .cspkg*</span><span class="sxs-lookup"><span data-stu-id="ff653-290">PackageLocation = 'c:\\drops\\app.publish\\ContactManager.Azure.cspkg' *This value is derived from: ($PublishDir)($ProjectName).cspkg*</span></span>
    3. <span data-ttu-id="ff653-291">PublishScriptLocation = "c:\\parancsfájlok\\WindowsAzure\\PublishCloudService.ps1"</span><span class="sxs-lookup"><span data-stu-id="ff653-291">PublishScriptLocation = 'c:\\scripts\\WindowsAzure\\PublishCloudService.ps1'</span></span>
    4. <span data-ttu-id="ff653-292">ServiceName = "mycloudservicename" *használata hello megfelelő felhőszolgáltatás neve itt*</span><span class="sxs-lookup"><span data-stu-id="ff653-292">ServiceName = 'mycloudservicename' *Use hello appropriate cloud service name here*</span></span>
    5. <span data-ttu-id="ff653-293">Környezet = "Átmeneti"</span><span class="sxs-lookup"><span data-stu-id="ff653-293">Environment = 'Staging'</span></span>
    6. <span data-ttu-id="ff653-294">StorageAccountName = "mystorageaccountname" *használata hello megfelelő tárfióknév Itt*</span><span class="sxs-lookup"><span data-stu-id="ff653-294">StorageAccountName = 'mystorageaccountname' *Use hello appropriate storage account name here*</span></span>
    7. <span data-ttu-id="ff653-295">SubscriptionDataFileLocation = "c:\\parancsfájlok\\WindowsAzure\\Subscription.xml"</span><span class="sxs-lookup"><span data-stu-id="ff653-295">SubscriptionDataFileLocation = 'c:\\scripts\\WindowsAzure\\Subscription.xml'</span></span>
    8. <span data-ttu-id="ff653-296">SubscriptionName = "alapértelmezett"</span><span class="sxs-lookup"><span data-stu-id="ff653-296">SubscriptionName = 'default'</span></span>

    ![Tulajdonság értékei][6]
11. <span data-ttu-id="ff653-298">Hello módosítások toohello Build definíció mentése.</span><span class="sxs-lookup"><span data-stu-id="ff653-298">Save hello changes toohello Build Definition.</span></span>
12. <span data-ttu-id="ff653-299">Feldolgozási sor egy Build tooexecute mindkét hello csomag létrehozási, és tegye közzé.</span><span class="sxs-lookup"><span data-stu-id="ff653-299">Queue a Build tooexecute both hello package build and publish.</span></span> <span data-ttu-id="ff653-300">Ha egy eseményindító tooContinuous integráció beállítása, végrehajtja a Ez a viselkedés a minden egyes bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="ff653-300">If you have a trigger set tooContinuous Integration, you will execute this behavior on every check-in.</span></span>

### <a name="publishcloudserviceps1-script-template"></a><span data-ttu-id="ff653-301">PublishCloudService.ps1 parancsfájl sablon</span><span class="sxs-lookup"><span data-stu-id="ff653-301">PublishCloudService.ps1 script template</span></span>
```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy too$servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according too$alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress tooactivity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a><span data-ttu-id="ff653-302">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ff653-302">Next steps</span></span>
<span data-ttu-id="ff653-303">tooenable távoli hibakeresés folyamatos kézbesítés használata esetén lásd: [folyamatos kézbesítési toopublish tooAzure használatakor távoli hibakeresésének engedélyezése](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).</span><span class="sxs-lookup"><span data-stu-id="ff653-303">tooenable remote debugging when using continuous delivery, see [Enable remote debugging when using continuous delivery toopublish tooAzure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).</span></span>

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[hello .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
