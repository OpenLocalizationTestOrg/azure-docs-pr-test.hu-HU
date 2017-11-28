---
title: "Az Azure-ban a TFS-sel szolgáltatások folyamatos kézbesítési felhő |} Microsoft Docs"
description: "Ismerje meg, hogyan állíthat be az Azure felhőalapú alkalmazásokat folyamatos kézbesítési. Kódminták MSBuild parancssori utasításokat és a PowerShell-parancsfájlokat."
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
ms.openlocfilehash: 0979722b9ec715e91825c7aba74657451df6e83f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a><span data-ttu-id="6d348-104">Azure felhőszolgáltatások folyamatos kézbesítés</span><span class="sxs-lookup"><span data-stu-id="6d348-104">Continuous Delivery for Cloud Services in Azure</span></span>
<span data-ttu-id="6d348-105">A cikkben leírt eljárás bemutatja, hogyan állíthatja be az Azure felhőalapú alkalmazásokat folyamatos kézbesítési.</span><span class="sxs-lookup"><span data-stu-id="6d348-105">The process described in this article shows you how to set up continuous delivery for Azure cloud apps.</span></span> <span data-ttu-id="6d348-106">Ez a folyamat lehetővé teszi csomagok automatikus létrehozását, valamint csomagok Azure-beli telepítését minden kódbeadás után.</span><span class="sxs-lookup"><span data-stu-id="6d348-106">This process enables you to automatically create packages and deploy the package to Azure after every code check-in.</span></span> <span data-ttu-id="6d348-107">A jelen cikkben ismertetett csomag felépítési folyamat megegyezik a **csomag** parancs a Visual Studio és a közzétételi lépéseket egyenértékűek legyenek az **közzététel** parancsot a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d348-107">The package build process described in this article is equivalent to the **Package** command in Visual Studio, and the publishing steps are equivalent to the **Publish** command in Visual Studio.</span></span>
<span data-ttu-id="6d348-108">A cikk ismerteti a módszert szeretné használni a build kiszolgáló MSBuild parancssori utasításokat és a Windows PowerShell-parancsfájlok létrehozása, és azt is bemutatja, hogyan kell igény szerint a Visual Studio Team Foundation Server - definíciók Team Build használatára állítsa be a MSBuild parancsok és a PowerShell-parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="6d348-108">The article covers the methods you would use to create a build server with MSBuild command-line statements and Windows PowerShell scripts, and it also demonstrates how to optionally configure Visual Studio Team Foundation Server - Team Build definitions to use the MSBuild commands and PowerShell scripts.</span></span> <span data-ttu-id="6d348-109">A folyamat nem testre szabható a összeállító környezetet és a cél Azure környezetben.</span><span class="sxs-lookup"><span data-stu-id="6d348-109">The process is customizable for your build environment and Azure target environments.</span></span>

<span data-ttu-id="6d348-110">Visual Studio Team Services, a könnyebb ehhez az Azure-ban üzemeltetett TFS verziójának is használható.</span><span class="sxs-lookup"><span data-stu-id="6d348-110">You can also use Visual Studio Team Services, a version of TFS that is hosted in Azure, to do this more easily.</span></span> 

<span data-ttu-id="6d348-111">Kezdés előtt kell közzé tenni az alkalmazást a Visual Studio eszközből.</span><span class="sxs-lookup"><span data-stu-id="6d348-111">Before you start, you should publish your application from Visual Studio.</span></span>
<span data-ttu-id="6d348-112">Ezzel biztosíthatja, hogy az összes erőforrás elérhető-e és inicializálva amikor megpróbál a kiadvány folyamat automatizálására.</span><span class="sxs-lookup"><span data-stu-id="6d348-112">This will ensure that all the resources are available and initialized when you attempt to automate the publication process.</span></span>

## <a name="1-configure-the-build-server"></a><span data-ttu-id="6d348-113">1: a Build kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6d348-113">1: Configure the Build Server</span></span>
<span data-ttu-id="6d348-114">Egy Azure-csomag létrehozása MSBuild használatával, előtt telepítenie kell a szükséges szoftverek és az eszközök a build kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="6d348-114">Before you can create an Azure package by using MSBuild, you must install the required software and tools on the build server.</span></span>

<span data-ttu-id="6d348-115">A Visual Studio nincs szükség a build kiszolgálóra kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="6d348-115">Visual Studio is not required to be installed on the build server.</span></span> <span data-ttu-id="6d348-116">Ha a build kiszolgáló kezelését Team Foundation Build szolgáltatás használni kívánt, kövesse az utasításokat a a [Team Foundation Build Service] [ Team Foundation Build Service] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6d348-116">If you want to use Team Foundation Build Service to manage your build server, follow the instructions in the [Team Foundation Build Service][Team Foundation Build Service] documentation.</span></span>

1. <span data-ttu-id="6d348-117">A build kiszolgálón telepítse a [.NET-keretrendszer 4.5.2-es][.NET Framework 4.5.2], mert az MSBuild tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6d348-117">On the build server, install the [.NET Framework 4.5.2][.NET Framework 4.5.2], which includes MSBuild.</span></span>
2. <span data-ttu-id="6d348-118">Telepítse a legújabb [.NET-keretrendszerhez készült Azure szerzői eszközök](https://azure.microsoft.com/develop/net/).</span><span class="sxs-lookup"><span data-stu-id="6d348-118">Install the latest [Azure Authoring Tools for .NET](https://azure.microsoft.com/develop/net/).</span></span>
3. <span data-ttu-id="6d348-119">Telepítse a [Azure-könyvtárakban .NET](http://go.microsoft.com/fwlink/?LinkId=623519).</span><span class="sxs-lookup"><span data-stu-id="6d348-119">Install the [Azure Libraries for .NET](http://go.microsoft.com/fwlink/?LinkId=623519).</span></span>
4. <span data-ttu-id="6d348-120">Másolja a Microsoft.WebApplication.targets fájlt a Visual Studio telepítése a build kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6d348-120">Copy the Microsoft.WebApplication.targets file from a Visual Studio installation to the build server.</span></span>

   <span data-ttu-id="6d348-121">A számítógépen telepített Visual Studio ezt a fájlt a C: könyvtárában található\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications.</span><span class="sxs-lookup"><span data-stu-id="6d348-121">On a computer with Visual Studio installed, this file is located in the directory C:\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications.</span></span> <span data-ttu-id="6d348-122">Másolja az ugyanabban a könyvtárban, a build kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="6d348-122">You should copy it to the same directory on the build server.</span></span>
5. <span data-ttu-id="6d348-123">Telepítse a [Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d348-123">Install the [Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).</span></span>

## <a name="2-build-a-package-using-msbuild-commands"></a><span data-ttu-id="6d348-124">2: MSBuild parancsokkal csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d348-124">2: Build a Package using MSBuild Commands</span></span>
<span data-ttu-id="6d348-125">Ez a szakasz ismerteti, hogyan az MSBuild parancs egy Azure-csomag épülő összeállításához.</span><span class="sxs-lookup"><span data-stu-id="6d348-125">This section describes how to construct an MSBuild command that builds an Azure package.</span></span> <span data-ttu-id="6d348-126">Futtassa ezt a lépést a build kiszolgálón győződjön meg arról, hogy minden megfelelően van konfigurálva, és hogy az MSBuild parancs valóban rá.</span><span class="sxs-lookup"><span data-stu-id="6d348-126">Run this step on the build server to verify that everything is configured correctly and that the MSBuild command does what you want it to do.</span></span> <span data-ttu-id="6d348-127">Ez a parancssor vagy hozzáadása meglévő build parancsfájlok a build kiszolgálón, vagy használhatja a parancssorban a TFS Build definícióban, a következő szakaszban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="6d348-127">You can either add this command line to existing build scripts on the build server, or you can use the command line in a TFS Build Definition, as described in the next section.</span></span> <span data-ttu-id="6d348-128">Parancssori paraméterek és MSBuild kapcsolatos további információkért lásd: [MSBuild parancssori hivatkozás](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d348-128">For more information about command-line parameters and MSBuild, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span></span>

1. <span data-ttu-id="6d348-129">Visual Studio a build kiszolgálóra van telepítve, keresse meg és válassza a **Visual Studio parancssorból** a a **Visual Studio eszközök** Windows-mappa.</span><span class="sxs-lookup"><span data-stu-id="6d348-129">If Visual Studio is installed on the build server, locate and choose **Visual Studio Command Prompt** in the **Visual Studio Tools** folder in Windows.</span></span>

   <span data-ttu-id="6d348-130">Ha a build kiszolgálón nincs telepítve a Visual Studio, nyisson meg egy parancssort, és győződjön meg arról, hogy MSBuild.exe elérhető-e az elérési út.</span><span class="sxs-lookup"><span data-stu-id="6d348-130">If Visual Studio is not installed on the build server, open a command prompt and make sure that MSBuild.exe is accessible on the path.</span></span> <span data-ttu-id="6d348-131">MSBuild van telepítve a .NET-keretrendszer, az elérési út: % WINDIR %\\Microsoft.NET\\keretrendszer\\*verzió*.</span><span class="sxs-lookup"><span data-stu-id="6d348-131">MSBuild is installed with the .NET Framework in the path %WINDIR%\\Microsoft.NET\\Framework\\*Version*.</span></span> <span data-ttu-id="6d348-132">Például MSBuild.exe hozzáadása a PATH környezeti változóba, ha a .NET Framework 4 telepítve van, írja be a következő parancsot a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="6d348-132">For example, to add MSBuild.exe to the PATH environment variable when you have .NET Framework 4 installed, type the following command at the command prompt:</span></span>

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. <span data-ttu-id="6d348-133">A parancssorban keresse meg a mappát, amely a használni kívánt Azure-projekt fájlt tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="6d348-133">At the command prompt, navigate to the folder containing the Azure project file that you want to build.</span></span>
3. <span data-ttu-id="6d348-134">MSBuild futtassa a/TARGET: Publish beállítást, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="6d348-134">Run MSBuild with the /target:Publish option as in the following example:</span></span>

       MSBuild /target:Publish

   <span data-ttu-id="6d348-135">Ez a beállítás rövidíthető /t: közzététele.</span><span class="sxs-lookup"><span data-stu-id="6d348-135">This option can be abbreviated as /t:Publish.</span></span> <span data-ttu-id="6d348-136">Az MSBuild /t:Publish beállítást kell nem keverendő össze a Visual Studio Publish parancsai Ha az Azure SDK telepítve van.</span><span class="sxs-lookup"><span data-stu-id="6d348-136">The /t:Publish option in MSBuild should not be confused with the Publish commands available in Visual Studio when you have the Azure SDK installed.</span></span> <span data-ttu-id="6d348-137">A /t: a beállítás csak buildek közzététele az Azure csomagokat.</span><span class="sxs-lookup"><span data-stu-id="6d348-137">The /t:Publish option only builds the Azure packages.</span></span> <span data-ttu-id="6d348-138">Azt nem kell telepítenie a csomagokat, mint a Visual Studio Publish parancsokat.</span><span class="sxs-lookup"><span data-stu-id="6d348-138">It does not deploy the packages as the Publish commands in Visual Studio do.</span></span>

   <span data-ttu-id="6d348-139">Szükség esetén megadhatja az MSBuild paraméter a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="6d348-139">Optionally, you can specify the project name as an MSBuild parameter.</span></span> <span data-ttu-id="6d348-140">Ha nincs megadva, az aktuális könyvtárat használja.</span><span class="sxs-lookup"><span data-stu-id="6d348-140">If not specified, the current directory is used.</span></span> <span data-ttu-id="6d348-141">MSBuild parancssori beállításokkal kapcsolatos további információkért lásd: [MSBuild parancssori hivatkozás](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d348-141">For more information about MSBuild command line options, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span></span>
4. <span data-ttu-id="6d348-142">Keresse meg a kimenetet.</span><span class="sxs-lookup"><span data-stu-id="6d348-142">Locate the output.</span></span> <span data-ttu-id="6d348-143">Alapértelmezés szerint ez a parancs létrehoz egy könyvtárat a projekthez, amely a gyökérmappában található viszonyítva, mint *ProjectDir*\\bin\\*konfigurációs*\\app.publish \\.</span><span class="sxs-lookup"><span data-stu-id="6d348-143">By default, this command creates a directory in relation to the root folder for the project, such as *ProjectDir*\\bin\\*Configuration*\\app.publish\\.</span></span> <span data-ttu-id="6d348-144">Azure-projekt összeállításakor létrehozhat két fájlt, a csomagfájl és a hozzá tartozó konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="6d348-144">When you build an Azure project, you generate two files, the package file itself and the accompanying configuration file:</span></span>

   * <span data-ttu-id="6d348-145">Project.cspkg</span><span class="sxs-lookup"><span data-stu-id="6d348-145">Project.cspkg</span></span>
   * <span data-ttu-id="6d348-146">ServiceConfiguration. *TargetProfile*.cscfg</span><span class="sxs-lookup"><span data-stu-id="6d348-146">ServiceConfiguration.*TargetProfile*.cscfg</span></span>

   <span data-ttu-id="6d348-147">Alapértelmezés szerint minden Azure-projekt tartalmazza egy szolgáltatás konfigurációs fájljában (.cscfg fájl) (hibakereséshez) helyi buildek és egy másikat a felhő (átmeneti vagy üzemi) buildek, de adhat hozzá vagy távolítsa el a szolgáltatás konfigurációs fájlokat, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="6d348-147">By default, each Azure project includes one service configuration file (.cscfg file) for local (debugging) builds and another for cloud (staging or production) builds, but you can add or remove service configuration files as needed.</span></span> <span data-ttu-id="6d348-148">Visual Studio csomag összeállításakor kérni fogja melyik szolgáltatás konfigurációs fájlja mellett a csomag tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6d348-148">When you build a package within Visual Studio, you will be asked which service configuration file to include alongside the package.</span></span>
5. <span data-ttu-id="6d348-149">Adja meg a szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="6d348-149">Specify the service configuration file.</span></span> <span data-ttu-id="6d348-150">Ha egy csomag MSBuild hozhat létre, a helyi szolgáltatás konfigurációs fájlja veszi fel alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="6d348-150">When you build a package by using MSBuild, the local service configuration file is included by default.</span></span> <span data-ttu-id="6d348-151">Egy másik szolgáltatáskonfigurációs fájlt, adja meg a TargetProfile tulajdonság MSBuild parancs az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="6d348-151">To include a different service configuration file, set the TargetProfile property of the MSBuild command, as in the following example:</span></span>

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. <span data-ttu-id="6d348-152">Adja meg a kimeneti helyét.</span><span class="sxs-lookup"><span data-stu-id="6d348-152">Specify the location for the output.</span></span> <span data-ttu-id="6d348-153">A /p:PublishDir használatával beállítani az elérési utat =*Directory* \\ beállítást, többek között a záró perjelet elválasztó, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="6d348-153">Set the path by using the /p:PublishDir=*Directory*\\ option, including the trailing backslash separator, as in the following example:</span></span>

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   <span data-ttu-id="6d348-154">Miután össze, és egy megfelelő MSBuild parancssort a projekt felépítéséhez és összefogni azokat az Azure-csomag tesztelni, ez a parancssor a build parancsfájlokat is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="6d348-154">Once you've constructed and tested an appropriate MSBuild command line to build your projects and combine them into an Azure package, you can add this command line to your build scripts.</span></span> <span data-ttu-id="6d348-155">Ha a build server egyéni parancsfájlokat használ, ez a folyamat az egyéni létrehozási folyamata során a mintaadatokról függ.</span><span class="sxs-lookup"><span data-stu-id="6d348-155">If your build server uses custom scripts, this process will depend on the specifics of your custom build process.</span></span> <span data-ttu-id="6d348-156">Ha TFS egy összeállító környezetet használ, akkor is kövesse az utasításokat a következő lépésben az Azure-csomag létrehozási hozzáadása a felépítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="6d348-156">If you are using TFS as a build environment, then you can follow the instructions in the next step to add the Azure package build to your build process.</span></span>

## <a name="3-build-a-package-using-tfs-team-build"></a><span data-ttu-id="6d348-157">3: build a csomagot, a TFS-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d348-157">3: Build a Package using TFS Team Build</span></span>
<span data-ttu-id="6d348-158">Ha rendelkezik TFS létrehozási gépként beállítása Team Foundation Server (TFS) állítsa be a build tartományvezérlő és a build kiszolgálóként, majd igény szerint állíthatja be az automatikus létrehozási az Azure-csomagnak a.</span><span class="sxs-lookup"><span data-stu-id="6d348-158">If you have Team Foundation Server (TFS) set up as a build controller and the build server set up as a TFS build machine, then you can optionally set up an automated build for your Azure package.</span></span> <span data-ttu-id="6d348-159">Állítsa be, és a Team Foundation server használja a buildelési rendszer módjáról további információkért lásd: [a buildelési rendszer kibővítési][Scale out your build system].</span><span class="sxs-lookup"><span data-stu-id="6d348-159">For information on how to set up and use Team Foundation server as a build system, see [Scale out your build system][Scale out your build system].</span></span> <span data-ttu-id="6d348-160">Különösen a következő eljárás feltételezi, hogy konfigurálta a build kiszolgáló leírtak [központi telepítése és build kiszolgálók][Deploy and configure a build server], és létrehozott egy csapatprojekt létrehozott felhő a csapatprojekt projektre.</span><span class="sxs-lookup"><span data-stu-id="6d348-160">In particular, the following procedure assumes that you have configured your build server as described in [Deploy and configure a build server][Deploy and configure a build server], and that you have created a team project, created a cloud service project in the team project.</span></span>

<span data-ttu-id="6d348-161">Az Azure csomagok TFS konfigurálásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6d348-161">To configure TFS to build Azure packages, perform the following steps:</span></span>

1. <span data-ttu-id="6d348-162">Válassza ki a Visual Studio a fejlesztési számítógépen, a Nézet menü **Team Explorer**, vagy válassza a Ctrl +\\, Ctrl + M.</span><span class="sxs-lookup"><span data-stu-id="6d348-162">In Visual Studio on your development computer, on the View menu, choose **Team Explorer**, or choose Ctrl+\\, Ctrl+M.</span></span> <span data-ttu-id="6d348-163">A csapat Explorer-ablakban bontsa ki a **buildek** csomópont, vagy válasszon a **buildek** lapon, és válassza a **új Build Definition**.</span><span class="sxs-lookup"><span data-stu-id="6d348-163">In the Team Explorer window, expand the **Builds** node or choose the **Builds** page, and choose **New Build Definition**.</span></span>

   ![Új beállítás található definíció létrehozása][0]
2. <span data-ttu-id="6d348-165">Válassza ki a **eseményindító** lapot, és adja meg, ha azt szeretné, hogy a csomag létrehozása a kívánt feltételeket.</span><span class="sxs-lookup"><span data-stu-id="6d348-165">Choose the **Trigger** tab, and specify the desired conditions for when you want the package to be built.</span></span> <span data-ttu-id="6d348-166">Adja meg például **folyamatos integrációt** a csomag létrehozásához, amikor egy forrás szabályozása be következik be.</span><span class="sxs-lookup"><span data-stu-id="6d348-166">For example, specify **Continuous Integration** to build the package whenever a source control check-in occurs.</span></span>
3. <span data-ttu-id="6d348-167">Válassza ki a **adatforrás-beállítások** lapot, és győződjön meg arról, hogy a projektmappa szerepel-e a **vezérlő forrásmappa** oszlop, és a állapotát **aktív**.</span><span class="sxs-lookup"><span data-stu-id="6d348-167">Choose the **Source Settings** tab, and make sure your project folder is listed in the **Source Control Folder** column, and the status is **Active**.</span></span>
4. <span data-ttu-id="6d348-168">Válassza ki a **Build alapértelmezett** fülre, és a Build vezérlő, ellenőrizze a build kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="6d348-168">Choose the **Build Defaults** tab, and under Build controller, verify the name of the build server.</span></span>  <span data-ttu-id="6d348-169">Válassza ki, a beállítás **másolat létrehozása a következő eldobási mappába kimeneti** , és adja meg a kívánt eldobott üzenetek helye.</span><span class="sxs-lookup"><span data-stu-id="6d348-169">Also, choose the option **Copy build output to the following drop folder** and specify the desired drop location.</span></span>
5. <span data-ttu-id="6d348-170">Válassza ki a **folyamat** fülre. A folyamat lapon válassza ki az alapértelmezett sablon, a **Build**, válassza ki a projekt, ha nincs bejelölve, és bontsa ki a **speciális** szakasz a **Build** szakasz a rács.</span><span class="sxs-lookup"><span data-stu-id="6d348-170">Choose the **Process** tab. On the Process tab, choose the default template, under **Build**, choose the project if it is not already selected, and expand the **Advanced** section in the **Build** section of the grid.</span></span>
6. <span data-ttu-id="6d348-171">Válasszon **MSBuild-argumentumok**, és állítsa be a megfelelő MSBuild parancssori argumentumok fenti a 2. lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="6d348-171">Choose **MSBuild Arguments**, and set the appropriate MSBuild command line arguments as described in Step 2 above.</span></span> <span data-ttu-id="6d348-172">Adja meg például **/t: /p:PublishDir közzététele =\\\\myserver\\esik\\**  csomag létrehozásához, és másolja a csomagfájlok a helyre \\ \\ myserver\\esik\\:</span><span class="sxs-lookup"><span data-stu-id="6d348-172">For example, enter **/t:Publish /p:PublishDir=\\\\myserver\\drops\\** to build a package and copy the package files to the location \\\\myserver\\drops\\:</span></span>

   ![MSBuild-argumentumok][2]

   > [!NOTE]
   > <span data-ttu-id="6d348-174">A fájlok másolása egy nyilvános megosztás megkönnyíti a csomagot a fejlesztési számítógépen manuális telepítése.</span><span class="sxs-lookup"><span data-stu-id="6d348-174">Copying the files to a public share makes it easier to manually deploy the packages from your development computer.</span></span>
7. <span data-ttu-id="6d348-175">Tesztelje a build lépés sikeres ellenőrzés módosítva lett a projekt, vagy egy új build sorba.</span><span class="sxs-lookup"><span data-stu-id="6d348-175">Test the success of your build step by checking in a change to your project, or queue up a new build.</span></span> <span data-ttu-id="6d348-176">Új buildverziót, a csapat Explorer sorba kattintson a jobb gombbal **összes Build definíciók** majd **várólista új Build**.</span><span class="sxs-lookup"><span data-stu-id="6d348-176">To queue up a new build, in the Team Explorer, right-click **All Build Definitions,** and then choose **Queue New Build**.</span></span>

## <a name="4-publish-a-package-using-a-powershell-script"></a><span data-ttu-id="6d348-177">4: a PowerShell-parancsfájl használatával csomag közzététele</span><span class="sxs-lookup"><span data-stu-id="6d348-177">4: Publish a Package using a PowerShell Script</span></span>
<span data-ttu-id="6d348-178">Ez a szakasz ismerteti, hogyan közzétehető a Cloud app csomag kimeneti Azure-ban a választható paraméterek: Windows PowerShell-parancsfájl összeállításához.</span><span class="sxs-lookup"><span data-stu-id="6d348-178">This section describes how to construct a Windows PowerShell script that will publish the Cloud app package output to Azure using optional parameters.</span></span> <span data-ttu-id="6d348-179">Ezt a parancsfájlt a build a build egyéni automatizálás lépés után hívható.</span><span class="sxs-lookup"><span data-stu-id="6d348-179">This script can be called after the build step in your custom build automation.</span></span> <span data-ttu-id="6d348-180">Azt a folyamatsablon munkafolyamat tevékenységei a Visual Studio TFS Team Build is hívható.</span><span class="sxs-lookup"><span data-stu-id="6d348-180">It can also be called from Process Template workflow activities in Visual Studio TFS Team Build.</span></span>

1. <span data-ttu-id="6d348-181">Telepítse a [Azure PowerShell-parancsmagok] [ Azure PowerShell cmdlets] (v0.6.1 vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="6d348-181">Install the [Azure PowerShell cmdlets][Azure PowerShell cmdlets] (v0.6.1 or higher).</span></span>
   <span data-ttu-id="6d348-182">A parancsmag a telepítés fázisban szeretné telepíteni, a beépülő modulként.</span><span class="sxs-lookup"><span data-stu-id="6d348-182">During the cmdlet setup phase, choose to install as a snap-in.</span></span> <span data-ttu-id="6d348-183">Vegye figyelembe, hogy a hivatalosan támogatott verziójú váltja fel a régebbi kínált a Codeplex webhelyen, bár a korábbi verziók volt számozott 2.x.x.</span><span class="sxs-lookup"><span data-stu-id="6d348-183">Note that this officially supported version replaces the older version offered through CodePlex, although the previous versions were numbered 2.x.x.</span></span>
2. <span data-ttu-id="6d348-184">Indítsa el a Start menü használatával Azure PowerShell vagy a Start lap.</span><span class="sxs-lookup"><span data-stu-id="6d348-184">Start Azure PowerShell using the Start menu or Start page.</span></span> <span data-ttu-id="6d348-185">Ily módon indul el, ha az Azure PowerShell-parancsmagok lesz betöltve.</span><span class="sxs-lookup"><span data-stu-id="6d348-185">If you start in this way, the Azure PowerShell cmdlets will be loaded.</span></span>
3. <span data-ttu-id="6d348-186">A PowerShell-parancssorba, győződjön meg arról, hogy a PowerShell-parancsmagok a részleges parancs vannak betöltve `Get-Azure` majd nyomja le a Tab billentyűt a nyilatkozatot befejezésére.</span><span class="sxs-lookup"><span data-stu-id="6d348-186">At the PowerShell prompt, verify that the PowerShell cmdlets are loaded by entering the partial command `Get-Azure` and then pressing the Tab key for statement completion.</span></span>

   <span data-ttu-id="6d348-187">Ha ismételten nyomja meg a Tab billentyűt, különböző Azure PowerShell-parancsokat kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="6d348-187">If you press the Tab key repeatedly, you should see various Azure PowerShell commands.</span></span>
4. <span data-ttu-id="6d348-188">Győződjön meg arról, hogy a ehhez az előfizetési adatai a .publishsettings fájlt importálja az Azure-előfizetéshez csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="6d348-188">Verify that you can connect to your Azure subscription by importing your subscription information from the .publishsettings file.</span></span>

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   <span data-ttu-id="6d348-189">Írja be a parancsot</span><span class="sxs-lookup"><span data-stu-id="6d348-189">Then enter the command</span></span>

   `Get-AzureSubscription`

   <span data-ttu-id="6d348-190">Ez az előfizetés információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6d348-190">This shows information about your subscription.</span></span> <span data-ttu-id="6d348-191">Győződjön meg arról, hogy minden rendben.</span><span class="sxs-lookup"><span data-stu-id="6d348-191">Verify that everything is correct.</span></span>
5. <span data-ttu-id="6d348-192">Ez a cikk a parancsfájlok mappába c: végéig elérhető parancsfájl-sablon mentése\\parancsfájlok\\WindowsAzure\\**PublishCloudService.ps1**.</span><span class="sxs-lookup"><span data-stu-id="6d348-192">Save the script template provided at the end of this article to your scripts folder as c:\\scripts\\WindowsAzure\\**PublishCloudService.ps1**.</span></span>
6. <span data-ttu-id="6d348-193">Tekintse át a parancsfájl a Paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="6d348-193">Review the parameters section of the script.</span></span> <span data-ttu-id="6d348-194">Adja hozzá, vagy módosítsa az alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="6d348-194">Add or modify any default values.</span></span> <span data-ttu-id="6d348-195">Sikeres explicit paraméterek mindig felülbírálhatja ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="6d348-195">These values can always be overridden by passing in explicit parameters.</span></span>
7. <span data-ttu-id="6d348-196">Győződjön meg arról nincs érvényes felhőalapú szolgáltatás és a tárolási fiók létrejönnek az előfizetés, amely a közzététel parancsfájl célpontja is lehet.</span><span class="sxs-lookup"><span data-stu-id="6d348-196">Ensure there are valid cloud service and storage accounts created in your subscription that can be targeted by the publish script.</span></span> <span data-ttu-id="6d348-197">A tárfiók (blob-tároló) feltöltése és a központi telepítési csomag- és konfigurációs fájl ideiglenesen tárolja, a központi telepítés létrehozása közben használható.</span><span class="sxs-lookup"><span data-stu-id="6d348-197">The storage account (blob storage) will be used to upload and temporarily store the deployment package and config file while the deployment is being created.</span></span>

   * <span data-ttu-id="6d348-198">Új felhőalapú szolgáltatás létrehozása, hívja meg ezt a parancsfájlt vagy használja a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6d348-198">To create a new cloud service, you can call this script or use the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="6d348-199">A felhőszolgáltatás neve lesz egy teljesen minősített tartománynevet az előtag, és ezért egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6d348-199">The cloud service name will be used as a prefix in a fully qualified domain name and hence it must be unique.</span></span>

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * <span data-ttu-id="6d348-200">Hozzon létre egy új tárfiókot, hogy a parancsprogram hívása vagy használja a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6d348-200">To create a new storage account, you can call this script or use the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="6d348-201">A tárfiók neve lesz egy teljesen minősített tartománynevet az előtag, és ezért egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6d348-201">The storage account name will be used as a prefix in a fully qualified domain name and hence it must be unique.</span></span> <span data-ttu-id="6d348-202">Próbálja meg az azonos név használata a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6d348-202">You can try using the same name as the cloud service.</span></span>

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. <span data-ttu-id="6d348-203">A parancsprogram hívása közvetlenül az Azure PowerShell vagy a gazdagép build automatizálás a csomag létrehozás után a parancsfájl kábelezést.</span><span class="sxs-lookup"><span data-stu-id="6d348-203">Call the script directly from Azure PowerShell, or wire up this script to your host build automation to occur after the package build.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="6d348-204">A parancsfájl mindig törölje, vagy cserélje le a meglévő telepítések alapértelmezés szerint, ha azokat.</span><span class="sxs-lookup"><span data-stu-id="6d348-204">The script will always delete or replace your existing deployments by default if they are detected.</span></span> <span data-ttu-id="6d348-205">Ez akkor szükségesek, hogy lehetővé teszik a folyamatos Automation ahol nincs felhasználói adatkérésekhez lehetséges.</span><span class="sxs-lookup"><span data-stu-id="6d348-205">This is necessary to enable continuous delivery from automation where no user prompting is possible.</span></span>
   >
   >

   <span data-ttu-id="6d348-206">**1. példa:** a szolgáltatás átmeneti környezet folyamatos üzembe helyezés:</span><span class="sxs-lookup"><span data-stu-id="6d348-206">**Example scenario 1:** continuous deployment to the staging environment of a service:</span></span>

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   <span data-ttu-id="6d348-207">Ez általában következnek teszt futtatásakor ellenőrzési és a virtuális IP-címcsere.</span><span class="sxs-lookup"><span data-stu-id="6d348-207">This is typically followed up by test run verification and a VIP swap.</span></span> <span data-ttu-id="6d348-208">A virtuális IP-címcsere megteheti a [Azure-portálon](https://portal.azure.com) vagy a Move-üzembe helyezési parancsmagjával.</span><span class="sxs-lookup"><span data-stu-id="6d348-208">The VIP swap can be done via the [Azure portal](https://portal.azure.com) or by using the Move-Deployment cmdlet.</span></span>

   <span data-ttu-id="6d348-209">**2. példa:** folyamatos üzembe helyezést az éles környezetbe, egy dedikált teszt szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="6d348-209">**Example scenario 2:** continuous deployment to the production environment of a dedicated test service</span></span>

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   <span data-ttu-id="6d348-210">**Távoli asztali:**</span><span class="sxs-lookup"><span data-stu-id="6d348-210">**Remote Desktop:**</span></span>

   <span data-ttu-id="6d348-211">Ha a távoli asztal engedélyezve van az Azure-projekt további egyszeri lépések végrehajtásával győződjön meg arról, ez a parancsfájl által megcélzott összes felhőszolgáltatás a megfelelő tanúsítványt a felhőalapú szolgáltatás feltöltött kell.</span><span class="sxs-lookup"><span data-stu-id="6d348-211">If Remote Desktop is enabled in your Azure project you will need to perform additional one-time steps to ensure the correct Cloud Service Certificate is uploaded to all cloud services targeted by this script.</span></span>

   <span data-ttu-id="6d348-212">Keresse meg a tanúsítvány ujjlenyomata értékeket, amelyet a szerepkörök várt.</span><span class="sxs-lookup"><span data-stu-id="6d348-212">Locate the certificate thumbprint values expected by your roles.</span></span> <span data-ttu-id="6d348-213">Az ujjlenyomat értékei látható a felhő konfigurációs fájl (pl. ServiceConfiguration.Cloud.cscfg) tanúsítványok szakaszában.</span><span class="sxs-lookup"><span data-stu-id="6d348-213">The thumbprint values are visible in the Certificates section of the cloud config file (i.e. ServiceConfiguration.Cloud.cscfg).</span></span> <span data-ttu-id="6d348-214">Célszerű is látható, a távoli asztali konfigurálása párbeszédpanel a Visual Studio megjelenítésekor beállítások és megtekintése a kiválasztott tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="6d348-214">It is also visible in the Remote Desktop Configuration dialog in Visual Studio when you Show Options and view the selected certificate.</span></span>

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   <span data-ttu-id="6d348-215">A távoli asztal-tanúsítványok feltöltésére használja a következő parancsmag parancsfájlt egyszeri telepítő lépésként:</span><span class="sxs-lookup"><span data-stu-id="6d348-215">Upload Remote Desktop certificates as a one-time setup step using the following cmdlet script:</span></span>

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   <span data-ttu-id="6d348-216">Példa:</span><span class="sxs-lookup"><span data-stu-id="6d348-216">For example:</span></span>

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   <span data-ttu-id="6d348-217">Másik lehetőségként exportálása a titkos kulcsot tartalmazó PFX tanúsítványfájl és -tanúsítványok feltöltésére minden cél felhőalapú szolgáltatás használata a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6d348-217">Alternatively you can export the certificate file PFX with private key and upload certificates to each target cloud service using the [Azure portal](https://portal.azure.com).</span></span>

   <!---
   Fixing broken links for Azure content migration from ACOM to DOCS. I'm unable to find a replacement links, so I'm commenting out this reference for now. The author can investigate in the future. "Read the following article to learn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   <span data-ttu-id="6d348-218">**Frissítés a központi telepítés vagy. Törölje a központi telepítés –\> új központi telepítés**</span><span class="sxs-lookup"><span data-stu-id="6d348-218">**Upgrade Deployment vs. Delete Deployment -\> New Deployment**</span></span>

   <span data-ttu-id="6d348-219">A parancsfájl alapértelmezés szerint végrehajtja az frissítés központi telepítése ($enableDeploymentUpgrade = 1) Ha nem paraméter átadott vagy explicit módon átadott 1 érték.</span><span class="sxs-lookup"><span data-stu-id="6d348-219">The script will by default perform an Upgrade Deployment ($enableDeploymentUpgrade = 1) when no parameter is passed in or the value 1 is passed explicitly.</span></span> <span data-ttu-id="6d348-220">Az egyetlen példány Ez azt az előnyt, teljes körű telepítésére rövidebb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="6d348-220">For single instances this has the advantage of taking less time than a full deployment.</span></span> <span data-ttu-id="6d348-221">A magas rendelkezésre állású, ez is azzal az előnnyel jár, hogy bizonyos esetekben, mások pedig futtató igénylő példányok frissítése (a frissítési tartomány érdekében), valamint a VIP nem lesz törölve.</span><span class="sxs-lookup"><span data-stu-id="6d348-221">For instances that require high availability this also has the advantage of leaving some instances running while others are upgraded (walking your update domain), plus your VIP will not be deleted.</span></span>

   <span data-ttu-id="6d348-222">Frissítés telepítése le kell tiltani a parancsfájl ($enableDeploymentUpgrade = 0), vagy úgy, hogy *- enableDeploymentUpgrade 0* paraméterként, amely megváltoztatja a parancsfájl viselkedését, először törölje a meglévő telepítést, és ezután hozzon létre egy új központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="6d348-222">Upgrade Deployment can be disabled in the script ($enableDeploymentUpgrade = 0) or by passing *-enableDeploymentUpgrade 0* as a parameter, which alters the script behavior to first delete any existing deployment and then create a new deployment.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="6d348-223">A parancsfájl mindig törölje, vagy cserélje le a meglévő telepítések alapértelmezés szerint, ha azokat.</span><span class="sxs-lookup"><span data-stu-id="6d348-223">The script will always delete or replace your existing deployments by default if they are detected.</span></span> <span data-ttu-id="6d348-224">Ez az Automation folyamatos kézbesítési ahhoz szükséges, hogy nincs felhasználói/operátor kérdés esetén lehetséges.</span><span class="sxs-lookup"><span data-stu-id="6d348-224">This is necessary to enable continuous delivery from automation where no user/operator prompting is possible.</span></span>
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a><span data-ttu-id="6d348-225">5: közzétenni a csomagot, a TFS-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d348-225">5: Publish a Package using TFS Team Build</span></span>
<span data-ttu-id="6d348-226">Ez az opcionális lépés csatlakozik a TFS csoport létrehozása a 4. lépésében létrehozott parancsfájlt, amely a csomag build Azure közzétételi kezeli.</span><span class="sxs-lookup"><span data-stu-id="6d348-226">This optional step connects TFS Team Build to the script created in step 4, which handles publishing of the package build to Azure.</span></span> <span data-ttu-id="6d348-227">Ez terjed ki, hogy fut a közzététel tevékenység a munkafolyamat végén build definition használt folyamat sablon módosításával.</span><span class="sxs-lookup"><span data-stu-id="6d348-227">This entails modifying the Process Template used by your build definition so that it runs a Publish activity at the end of the workflow.</span></span> <span data-ttu-id="6d348-228">A közzététel tevékenység végrehajtása addig a build a paraméterek átadása a PowerShell-parancsot.</span><span class="sxs-lookup"><span data-stu-id="6d348-228">The Publish activity will execute your PowerShell command passing in parameters from the build.</span></span> <span data-ttu-id="6d348-229">Az MSBuild kimenete célozza, és parancsfájl közzé lesz a szabványos felépítési művelet kimenetében be kell adatcsatornán.</span><span class="sxs-lookup"><span data-stu-id="6d348-229">Output of the MSBuild targets and publish script will be piped into the standard build output.</span></span>

1. <span data-ttu-id="6d348-230">Szerkessze a Build definíciót felelős folyamatos központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="6d348-230">Edit the Build Definition responsible for continuous deploy.</span></span>
2. <span data-ttu-id="6d348-231">Válassza ki a **folyamat** fülre.</span><span class="sxs-lookup"><span data-stu-id="6d348-231">Select the **Process** tab.</span></span>
3. <span data-ttu-id="6d348-232">Hajtsa végre a [ezeket az utasításokat](http://msdn.microsoft.com/library/dd647551.aspx) egy tevékenység-projektjét, amely az összeállítási folyamat sablon hozzáadásához töltse le az alapértelmezett sablon, adja hozzá a projekthez, és jelölje be.</span><span class="sxs-lookup"><span data-stu-id="6d348-232">Follow [these instructions](http://msdn.microsoft.com/library/dd647551.aspx) to add an Activity project for the build process template, download the default template, add it to the project and check it in.</span></span> <span data-ttu-id="6d348-233">A létrehozási folyamat sablon adjon új nevet, például a AzureBuildProcessTemplate.</span><span class="sxs-lookup"><span data-stu-id="6d348-233">Give the build process template a new name, such as AzureBuildProcessTemplate.</span></span>
4. <span data-ttu-id="6d348-234">Lépjen vissza a **folyamat** lapot, és használjon **részletek megjelenítése** elérhető összeállítási folyamat sablonok listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6d348-234">Return to the **Process** tab, and use **Show Details** to show a list of available build process templates.</span></span> <span data-ttu-id="6d348-235">Válassza ki a **új...**  gombra, és keresse meg a most hozzáadott és be van jelölve, projekt.</span><span class="sxs-lookup"><span data-stu-id="6d348-235">Choose the **New...** button, and navigate to the project you just added and checked in.</span></span> <span data-ttu-id="6d348-236">Keresse meg az újonnan létrehozott sablont, és válassza a **OK**.</span><span class="sxs-lookup"><span data-stu-id="6d348-236">Locate the template you just created and choose **OK**.</span></span>
5. <span data-ttu-id="6d348-237">Nyissa meg a kijelölt folyamatsablont szerkesztésre.</span><span class="sxs-lookup"><span data-stu-id="6d348-237">Open the selected Process Template for editing.</span></span> <span data-ttu-id="6d348-238">Úgy is megnyithatja közvetlenül a munkafolyamat-tervezőben vagy a XAML használható az XML-szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="6d348-238">You can open directly in the Workflow designer or in the XML editor to work with the XAML.</span></span>
6. <span data-ttu-id="6d348-239">Adja hozzá az alábbi listán szereplő új argumentumok külön sorban elemeket a munkafolyamat-Tervező argumentumok lapján.</span><span class="sxs-lookup"><span data-stu-id="6d348-239">Add the following list of new arguments as separate line items in the arguments tab of the workflow designer.</span></span> <span data-ttu-id="6d348-240">Minden argumentum irányba kell rendelkeznie, és írja be a = = karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="6d348-240">All arguments should have direction=In and type=String.</span></span> <span data-ttu-id="6d348-241">Ezek használandó adatfolyam paramétereit a munkafolyamatba build definícióból használt majd beolvasni a közzététel parancsfájl hívni.</span><span class="sxs-lookup"><span data-stu-id="6d348-241">These will be used to flow parameters from the build definition into the workflow, which then get used to call the publish script.</span></span>

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Argumentumok listájának megjelenítése][3]

   <span data-ttu-id="6d348-243">A megfelelő XAML így néz ki:</span><span class="sxs-lookup"><span data-stu-id="6d348-243">The corresponding XAML looks like this:</span></span>

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
7. <span data-ttu-id="6d348-244">Felvehet egy új sorozatot futtassa az ügynök végén:</span><span class="sxs-lookup"><span data-stu-id="6d348-244">Add a new sequence at the end of Run On Agent:</span></span>

   1. <span data-ttu-id="6d348-245">Először vegyen fel egy érvényes parancsfájl kereséséhez Ha utasítás tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="6d348-245">Start by adding an If Statement activity to check for a valid script file.</span></span> <span data-ttu-id="6d348-246">Erre az értékre állítsa be a következő feltételt:</span><span class="sxs-lookup"><span data-stu-id="6d348-246">Set the condition to this value:</span></span>

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. <span data-ttu-id="6d348-247">A Then esetben ha utasítás adjon hozzá egy új feladatütemezési tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="6d348-247">In the Then case of the If Statement, add a new Sequence activity.</span></span> <span data-ttu-id="6d348-248">Állítsa be a kezdő közzététele megjelenített neve</span><span class="sxs-lookup"><span data-stu-id="6d348-248">Set the display name to 'Start publish'</span></span>
   3. <span data-ttu-id="6d348-249">A kezdő és a kijelölt feladatütemezési közzétételéhez adja hozzá az alábbi listán szereplő új változók a munkafolyamat-Tervező változók lapján külön sorként.</span><span class="sxs-lookup"><span data-stu-id="6d348-249">With the Start publish sequence still selected, add the following list of new variables as separate line items in the variables tab of the workflow designer.</span></span> <span data-ttu-id="6d348-250">Minden változót kell változótípus = karakterlánc és a hatókör = Start közzététele.</span><span class="sxs-lookup"><span data-stu-id="6d348-250">All variables should have Variable type =String and Scope=Start publish.</span></span> <span data-ttu-id="6d348-251">Ezek használandó adatfolyam paramétereit a munkafolyamatba build definícióból használt majd beolvasni a közzététel parancsfájl hívni.</span><span class="sxs-lookup"><span data-stu-id="6d348-251">These will be used to flow parameters from the build definition into the workflow, which then get used to call the publish script.</span></span>

      * <span data-ttu-id="6d348-252">SubscriptionDataFilePath, karakterlánc típusú</span><span class="sxs-lookup"><span data-stu-id="6d348-252">SubscriptionDataFilePath, of type String</span></span>
      * <span data-ttu-id="6d348-253">PublishScriptFilePath, karakterlánc típusú</span><span class="sxs-lookup"><span data-stu-id="6d348-253">PublishScriptFilePath, of type String</span></span>

        ![Új változók][4]
   4. <span data-ttu-id="6d348-255">Ha a TFS 2012 vagy régebbi használ, egy ConvertWorkspaceItem tevékenység hozzáadása az új feladatütemezési elején.</span><span class="sxs-lookup"><span data-stu-id="6d348-255">If you are using TFS 2012 or earlier, add a ConvertWorkspaceItem activity at the beginning of the new Sequence.</span></span> <span data-ttu-id="6d348-256">Ha a TFS 2013 vagy újabb verzió használata esetén egy GetLocalPath tevékenység hozzáadása az új feladatütemezési elején.</span><span class="sxs-lookup"><span data-stu-id="6d348-256">If you are using TFS 2013 or later, add a GetLocalPath activity at the beginning of the new sequence.</span></span> <span data-ttu-id="6d348-257">Egy ConvertWorkspaceItem beállítása a tulajdonságok az alábbiak szerint: irány ServerToLocal, DisplayName = = "Convert közzétételéhez parancsfájl fájlnév" bemeneti = "PublishScriptLocation", az eredmény = "PublishScriptFilePath" munkaterület = "Munkaterület".</span><span class="sxs-lookup"><span data-stu-id="6d348-257">For a ConvertWorkspaceItem, set the properties as follows: Direction=ServerToLocal, DisplayName='Convert publish script filename', Input=' PublishScriptLocation', Result='PublishScriptFilePath', Workspace='Workspace'.</span></span> <span data-ttu-id="6d348-258">GetLocalPath tevékenységhez a "PublishScriptLocation" IncomingPath, és az eredményt a "PublishScriptFilePath" tulajdonságának beállítása.</span><span class="sxs-lookup"><span data-stu-id="6d348-258">For a GetLocalPath activity, set the property IncomingPath to 'PublishScriptLocation', and the Result to 'PublishScriptFilePath'.</span></span> <span data-ttu-id="6d348-259">Ez a tevékenység az elérési út a közzététel parancsfájlt a kiszolgáló helyét (ha van ilyen) TFS egy normál helyi lemez elérési útja alakítja.</span><span class="sxs-lookup"><span data-stu-id="6d348-259">This activity converts the path to the publish script from TFS server locations (if applicable) to a standard local disk path.</span></span>
   5. <span data-ttu-id="6d348-260">Ha TFS 2012 vagy régebbi használ, adja hozzá egy másik ConvertWorkspaceItem tevékenység az új feladatütemezési végén.</span><span class="sxs-lookup"><span data-stu-id="6d348-260">If you are using TFS 2012 or earlier, add another ConvertWorkspaceItem activity at the end of the new Sequence.</span></span> <span data-ttu-id="6d348-261">Irány ServerToLocal, DisplayName = = "Átalakításáról előfizetés fájlnév" bemeneti = "SubscriptionDataFileLocation", az eredmény = "SubscriptionDataFilePath" munkaterület = "Munkaterület".</span><span class="sxs-lookup"><span data-stu-id="6d348-261">Direction=ServerToLocal, DisplayName='Convert subscription filename', Input=' SubscriptionDataFileLocation', Result= 'SubscriptionDataFilePath', Workspace='Workspace'.</span></span> <span data-ttu-id="6d348-262">Ha a TFS 2013, vagy később, a másik GetLocalPath hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="6d348-262">If you are using TFS 2013 or later, add another GetLocalPath.</span></span> <span data-ttu-id="6d348-263">IncomingPath = "SubscriptionDataFileLocation", és az eredmény = "SubscriptionDataFilePath."</span><span class="sxs-lookup"><span data-stu-id="6d348-263">IncomingPath='SubscriptionDataFileLocation', and Result='SubscriptionDataFilePath.'</span></span>
   6. <span data-ttu-id="6d348-264">InvokeProcess tevékenység hozzáadása az új feladatütemezési végén.</span><span class="sxs-lookup"><span data-stu-id="6d348-264">Add an InvokeProcess activity at the end of the new Sequence.</span></span>
      <span data-ttu-id="6d348-265">Ez a tevékenység PowerShell.exe meghívja a Build definíció által átadott argumentumok.</span><span class="sxs-lookup"><span data-stu-id="6d348-265">This activity calls PowerShell.exe with the arguments passed in by the Build Definition.</span></span>

      + <span data-ttu-id="6d348-266">Argumentumok = String.Format ("-""{0}" "- szolgáltatásnév {1} - storageAccountName {2} - packageLocation""{3}" "- cloudConfigLocation""{4}" "– subscriptionDataFile""{5}" "– selectedSubscription {6} fájlt-környezet""{7}" "",  PublishScriptFilePath, ServiceName, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, környezet)</span><span class="sxs-lookup"><span data-stu-id="6d348-266">Arguments = String.Format(" -File ""{0}"" -serviceName {1}  -storageAccountName {2} -packageLocation ""{3}""  -cloudConfigLocation ""{4}"" -subscriptionDataFile ""{5}""  -selectedSubscription {6} -environment ""{7}""",  PublishScriptFilePath, ServiceName, StorageAccountName,  PackageLocation, CloudConfigLocation,  SubscriptionDataFilePath, SubscriptionName, Environment)</span></span>
      + <span data-ttu-id="6d348-267">DisplayName = Execute parancsfájl közzététele</span><span class="sxs-lookup"><span data-stu-id="6d348-267">DisplayName = Execute publish script</span></span>
      + <span data-ttu-id="6d348-268">FileName = "PowerShell" (idézőjelek között)</span><span class="sxs-lookup"><span data-stu-id="6d348-268">FileName = "PowerShell" (include the quotes)</span></span>
      + <span data-ttu-id="6d348-269">OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)</span><span class="sxs-lookup"><span data-stu-id="6d348-269">OutputEncoding=  System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)</span></span>
   7. <span data-ttu-id="6d348-270">Az a **szabványos kimeneti kezelni** a InvokeProcess a szövegmező területen állítsa a szövegmező "data".</span><span class="sxs-lookup"><span data-stu-id="6d348-270">In the **Handle Standard Output** section textbox of the InvokeProcess, set the textbox value to 'data'.</span></span> <span data-ttu-id="6d348-271">Ez a változó a szabványos kimeneti adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="6d348-271">This is a variable to store the standard output data.</span></span>
   8. <span data-ttu-id="6d348-272">Adjon hozzá egy WriteBuildMessage tevékenységet, csak az alábbiakban a **szabványos kimeneti kezelni** szakasz.</span><span class="sxs-lookup"><span data-stu-id="6d348-272">Add a WriteBuildMessage activity just below the **Handle Standard Output** section.</span></span> <span data-ttu-id="6d348-273">Állítsa be a fontosság = "Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High" és az üzenet = "data".</span><span class="sxs-lookup"><span data-stu-id="6d348-273">Set the Importance = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' and the Message='data'.</span></span> <span data-ttu-id="6d348-274">Ez biztosítja, hogy a normál a kimenetbe a parancsfájl a felépítési művelet kimenetében beolvasása írni.</span><span class="sxs-lookup"><span data-stu-id="6d348-274">This ensures the standard output of the script will get written to the build output.</span></span>
   9. <span data-ttu-id="6d348-275">Az a **kezelni hiba kimeneti** a InvokeProcess a szövegmező területen állítsa a szövegmező "data".</span><span class="sxs-lookup"><span data-stu-id="6d348-275">In the **Handle Error Output** section textbox of the InvokeProcess, set the textbox value to 'data'.</span></span> <span data-ttu-id="6d348-276">Ez a változó a standard hiba adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="6d348-276">This is a variable to store the standard error data.</span></span>
   10. <span data-ttu-id="6d348-277">Adjon hozzá egy WriteBuildError tevékenységet, csak az alábbiakban a **kezelni hiba kimeneti** szakasz.</span><span class="sxs-lookup"><span data-stu-id="6d348-277">Add a WriteBuildError activity just below the **Handle Error Output** section.</span></span> <span data-ttu-id="6d348-278">Állítsa be a Message = "data".</span><span class="sxs-lookup"><span data-stu-id="6d348-278">Set the Message='data'.</span></span> <span data-ttu-id="6d348-279">Ez biztosítja, a standard hibák, a parancsfájl a build hiba kimeneti beolvasása írni.</span><span class="sxs-lookup"><span data-stu-id="6d348-279">This ensures the standard errors of the script will get written to the build error output.</span></span>
   11. <span data-ttu-id="6d348-280">Javítsa ki a hibákat, kék felkiáltójel jelek jelölik.</span><span class="sxs-lookup"><span data-stu-id="6d348-280">Correct any errors, indicated by blue exclamation marks.</span></span> <span data-ttu-id="6d348-281">A hibával kapcsolatos javaslat lekérni az felkiáltójel közötti mutasson.</span><span class="sxs-lookup"><span data-stu-id="6d348-281">Hover over the exclamation marks to get a hint about the error.</span></span> <span data-ttu-id="6d348-282">Törölje a hibákat a munkafolyamat mentése.</span><span class="sxs-lookup"><span data-stu-id="6d348-282">Save the workflow to clear errors.</span></span>

   <span data-ttu-id="6d348-283">A közzététel munkafolyamat-tevékenység a végeredményt fog kinézni a tervezőben:</span><span class="sxs-lookup"><span data-stu-id="6d348-283">The final result of the publish workflow activities will look like this in the designer:</span></span>

   ![Munkafolyamat-tevékenységek][5]

   <span data-ttu-id="6d348-285">A közzététel munkafolyamat-tevékenység a végeredményt fog kinézni XAML-kódban:</span><span class="sxs-lookup"><span data-stu-id="6d348-285">The final result of the publish workflow activities will look like this in XAML:</span></span>

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
8. <span data-ttu-id="6d348-286">Mentse a build munkafolyamat-sablon és a felvétel ezt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="6d348-286">Save the build process template workflow and Check In this file.</span></span>
9. <span data-ttu-id="6d348-287">Szerkessze a build definíciót (lezárhatja, amennyiben már meg nyitva), és válassza ki a **új** gombra kattint, ha még nem látja az új sablon folyamat sablonok listáján.</span><span class="sxs-lookup"><span data-stu-id="6d348-287">Edit the build definition (close it if it is already open), and select the **New** button if you do not yet see the new template in the list of Process Templates.</span></span>
10. <span data-ttu-id="6d348-288">A paraméter tulajdonságértékei állíthatók egyebek szakaszában az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6d348-288">Set the parameter property values in the Misc section as follows:</span></span>

    1. <span data-ttu-id="6d348-289">CloudConfigLocation = "c:\\esik\\app.publish\\ServiceConfiguration.Cloud.cscfg" *ezt az értéket, amelyből származik: ($PublishDir)ServiceConfiguration.Cloud.cscfg*</span><span class="sxs-lookup"><span data-stu-id="6d348-289">CloudConfigLocation ='c:\\drops\\app.publish\\ServiceConfiguration.Cloud.cscfg' *This value is derived from: ($PublishDir)ServiceConfiguration.Cloud.cscfg*</span></span>
    2. <span data-ttu-id="6d348-290">PackageLocation = "c:\\esik\\app.publish\\ContactManager.Azure.cspkg" *ezt az értéket, amelyből származik: ($PublishDir)($ProjectName) .cspkg*</span><span class="sxs-lookup"><span data-stu-id="6d348-290">PackageLocation = 'c:\\drops\\app.publish\\ContactManager.Azure.cspkg' *This value is derived from: ($PublishDir)($ProjectName).cspkg*</span></span>
    3. <span data-ttu-id="6d348-291">PublishScriptLocation = "c:\\parancsfájlok\\WindowsAzure\\PublishCloudService.ps1"</span><span class="sxs-lookup"><span data-stu-id="6d348-291">PublishScriptLocation = 'c:\\scripts\\WindowsAzure\\PublishCloudService.ps1'</span></span>
    4. <span data-ttu-id="6d348-292">ServiceName = "mycloudservicename" *itt használja a megfelelő felhőszolgáltatás neve*</span><span class="sxs-lookup"><span data-stu-id="6d348-292">ServiceName = 'mycloudservicename' *Use the appropriate cloud service name here*</span></span>
    5. <span data-ttu-id="6d348-293">Környezet = "Átmeneti"</span><span class="sxs-lookup"><span data-stu-id="6d348-293">Environment = 'Staging'</span></span>
    6. <span data-ttu-id="6d348-294">StorageAccountName = "mystorageaccountname" *itt használja a megfelelő tárfiók neve*</span><span class="sxs-lookup"><span data-stu-id="6d348-294">StorageAccountName = 'mystorageaccountname' *Use the appropriate storage account name here*</span></span>
    7. <span data-ttu-id="6d348-295">SubscriptionDataFileLocation = "c:\\parancsfájlok\\WindowsAzure\\Subscription.xml"</span><span class="sxs-lookup"><span data-stu-id="6d348-295">SubscriptionDataFileLocation = 'c:\\scripts\\WindowsAzure\\Subscription.xml'</span></span>
    8. <span data-ttu-id="6d348-296">SubscriptionName = "alapértelmezett"</span><span class="sxs-lookup"><span data-stu-id="6d348-296">SubscriptionName = 'default'</span></span>

    ![Tulajdonság értékei][6]
11. <span data-ttu-id="6d348-298">Menti a módosításokat a Build definíciójához.</span><span class="sxs-lookup"><span data-stu-id="6d348-298">Save the changes to the Build Definition.</span></span>
12. <span data-ttu-id="6d348-299">Feldolgozási sor végrehajtása mindkét a csomag összeállítása és közzététele Build.</span><span class="sxs-lookup"><span data-stu-id="6d348-299">Queue a Build to execute both the package build and publish.</span></span> <span data-ttu-id="6d348-300">Ha egy eseményindító folyamatos integráció beállítása, végrehajtja a Ez a viselkedés a minden egyes bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="6d348-300">If you have a trigger set to Continuous Integration, you will execute this behavior on every check-in.</span></span>

### <a name="publishcloudserviceps1-script-template"></a><span data-ttu-id="6d348-301">PublishCloudService.ps1 parancsfájl sablon</span><span class="sxs-lookup"><span data-stu-id="6d348-301">PublishCloudService.ps1 script template</span></span>
```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
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
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
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

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a><span data-ttu-id="6d348-302">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d348-302">Next steps</span></span>
<span data-ttu-id="6d348-303">Engedélyezheti a távoli hibakeresés folyamatos kézbesítés használata esetén [folyamatos kézbesítési közzététele az Azure-bA használatakor távoli hibakeresésének engedélyezése](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).</span><span class="sxs-lookup"><span data-stu-id="6d348-303">To enable remote debugging when using continuous delivery, see [Enable remote debugging when using continuous delivery to publish to Azure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).</span></span>

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
