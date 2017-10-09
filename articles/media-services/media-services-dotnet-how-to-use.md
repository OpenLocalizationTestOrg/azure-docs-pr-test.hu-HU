---
title: "a .NET Media Services-fejlesztés mentése számítógép aaaHow tooSet"
description: "További információk a Media Services .NET-keretrendszerhez készült Media Services SDK hello segítségével hello előfeltételeit. Is megtudhatja, hogyan toocreate egy Visual Studio alkalmazást."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ec2804c7-c656-4fbf-b3e4-3f0f78599a7f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2017
ms.author: juliako
ms.openlocfilehash: a5a2af3211d8156fd7dea99831fb769df4130d41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="media-services-development-with-net"></a><span data-ttu-id="58a4a-104">A .NET Media Services-fejlesztés</span><span class="sxs-lookup"><span data-stu-id="58a4a-104">Media Services development with .NET</span></span>
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

<span data-ttu-id="58a4a-105">Ez a témakör ismerteti, hogyan toostart fejlesztését Media Services .NET használó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="58a4a-105">This topic discusses how toostart developing Media Services applications using .NET.</span></span>

<span data-ttu-id="58a4a-106">Hello **Azure Media Services .NET SDK** library lehetővé teszi a Media Services .NET segítségével elleni tooprogram.</span><span class="sxs-lookup"><span data-stu-id="58a4a-106">hello **Azure Media Services .NET SDK** library enables you tooprogram against Media Services using .NET.</span></span> <span data-ttu-id="58a4a-107">Ez akkor is igaz, a .NET, könnyebben toodevelop hello toomake **Azure Media Services .NET SDK-bővítmények** könyvtár valósul meg.</span><span class="sxs-lookup"><span data-stu-id="58a4a-107">toomake it even easier toodevelop with .NET, hello **Azure Media Services .NET SDK Extensions** library is provided.</span></span> <span data-ttu-id="58a4a-108">Ezt a szalagtárat kiegészítő módszerek és segédfüggvények találhatók, amelyek egyszerűbbé teszik a .NET-kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="58a4a-108">This library contains a set of extension methods and helper functions that simplify your .NET code.</span></span> <span data-ttu-id="58a4a-109">Mindkét szalagtárak keresztül érhetők el **NuGet** és **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="58a4a-109">Both libraries are available through **NuGet** and **GitHub**.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58a4a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="58a4a-110">Prerequisites</span></span>
* <span data-ttu-id="58a4a-111">Egy Media Services-fiók egy új vagy meglévő Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="58a4a-111">A Media Services account in a new or existing Azure subscription.</span></span> <span data-ttu-id="58a4a-112">Hello témakörben [hogyan tooCreate Media Services-fiók](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="58a4a-112">See hello topic [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="58a4a-113">Operációs rendszerek: Windows 10, Windows 7, Windows 2008 R2 vagy Windows 8.</span><span class="sxs-lookup"><span data-stu-id="58a4a-113">Operating Systems: Windows 10, Windows 7, Windows 2008 R2, or Windows 8.</span></span>
* <span data-ttu-id="58a4a-114">.NET-keretrendszer 4.5.</span><span class="sxs-lookup"><span data-stu-id="58a4a-114">.NET Framework 4.5.</span></span>
* <span data-ttu-id="58a4a-115">Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58a4a-115">Visual Studio.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="58a4a-116">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="58a4a-116">Create and configure a Visual Studio project</span></span>
<span data-ttu-id="58a4a-117">Ez a szakasz bemutatja, hogyan toocreate a projektre a Visual Studio és a Media Services-fejlesztés beállításához.</span><span class="sxs-lookup"><span data-stu-id="58a4a-117">This section shows you how toocreate a project in Visual Studio and set it up for Media Services development.</span></span>  <span data-ttu-id="58a4a-118">Ebben az esetben hello projekt egy Windows C# konzolalkalmazást, de hello itt látható lépéseket telepítő alkalmazása tooother típusú projekteket hozhat létre a Media Services-alkalmazások (például a Windows Forms alkalmazások vagy az ASP.NET webalkalmazás).</span><span class="sxs-lookup"><span data-stu-id="58a4a-118">In this case, hello project is a C# Windows console application, but hello same setup steps shown here apply tooother types of projects you can create for Media Services applications (for example, a Windows Forms application or an ASP.NET Web application).</span></span>

<span data-ttu-id="58a4a-119">Ez a szakasz bemutatja, hogyan toouse **NuGet** tooadd Media Services .NET SDK bővítmények és más függő szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="58a4a-119">This section shows how toouse **NuGet** tooadd Media Services .NET SDK extensions and other dependent libraries.</span></span>

<span data-ttu-id="58a4a-120">Azt is megteheti, a Githubról kaphat hello legújabb Media Services .NET SDK bits ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) vagy [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), hello megoldás kiépítését, és adja hozzá a hello hivatkozások toohello ügyfélprojekt.</span><span class="sxs-lookup"><span data-stu-id="58a4a-120">Alternatively, you can get hello latest Media Services .NET SDK bits from GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) or [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), build hello solution, and add hello references toohello client project.</span></span> <span data-ttu-id="58a4a-121">Minden hello szükséges függőségek letöltött és kibontott automatikusan.</span><span class="sxs-lookup"><span data-stu-id="58a4a-121">All hello necessary dependencies get downloaded and extracted automatically.</span></span>

1. <span data-ttu-id="58a4a-122">A Visual Studióban hozzon létre egy új Visual C#-konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="58a4a-122">Create a new C# Console Application in Visual Studio.</span></span> <span data-ttu-id="58a4a-123">Adja meg a hello **neve**, **hely**, és **megoldás neve**, majd kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="58a4a-123">Enter hello **Name**, **Location**, and **Solution name**, and then click OK.</span></span>
2. <span data-ttu-id="58a4a-124">Hello megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="58a4a-124">Build hello solution.</span></span>
3. <span data-ttu-id="58a4a-125">Használjon **NuGet** tooinstall, és adja hozzá **Azure Media Services .NET SDK-bővítmények** (**windowsazure.mediaservices.extensions**).</span><span class="sxs-lookup"><span data-stu-id="58a4a-125">Use **NuGet** tooinstall and add **Azure Media Services .NET SDK Extensions** (**windowsazure.mediaservices.extensions**).</span></span> <span data-ttu-id="58a4a-126">Ennek a csomagnak a telepítése a **Media Services .NET SDK**csomagot és az összes további szükséges függőséget is feltelepíti</span><span class="sxs-lookup"><span data-stu-id="58a4a-126">Installing this package, also installs **Media Services .NET SDK** and adds all other required dependencies.</span></span>
   
    <span data-ttu-id="58a4a-127">Győződjön meg arról, hogy rendelkezik-e telepítve NuGet hello legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="58a4a-127">Ensure that you have hello newest version of NuGet installed.</span></span> <span data-ttu-id="58a4a-128">További információk és a telepítési utasításokért lásd: [NuGet](http://nuget.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="58a4a-128">For more information and installation instructions, see [NuGet](http://nuget.codeplex.com/).</span></span>
4. <span data-ttu-id="58a4a-129">A Megoldáskezelőben kattintson a jobb gombbal a projekt hello hello nevét, és válassza a kezelése NuGet-csomagok.</span><span class="sxs-lookup"><span data-stu-id="58a4a-129">In Solution Explorer, right-click hello name of hello project and choose Manage NuGet packages.</span></span>
   
    <span data-ttu-id="58a4a-130">hello NuGet-csomagok kezelése párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="58a4a-130">hello Manage NuGet Packages dialog box appears.</span></span>
5. <span data-ttu-id="58a4a-131">Hello Online katalógusban, keresse meg az Azure-bővítményekkel MediaServices, válassza ki az Azure Media Services .NET SDK-bővítmények, és kattintson a hello telepítése gombra.</span><span class="sxs-lookup"><span data-stu-id="58a4a-131">In hello Online gallery, search for Azure MediaServices Extensions, choose Azure Media Services .NET SDK Extensions, and then click hello Install button.</span></span>
   
    <span data-ttu-id="58a4a-132">hello projekt módosul, és hivatkozik arra toohello Media Services .NET SDK-bővítmények, Media Services .NET SDK-t, és az egyéb függő szerelvényeket.</span><span class="sxs-lookup"><span data-stu-id="58a4a-132">hello project is modified and references toohello Media Services .NET SDK Extensions,  Media Services .NET SDK, and other dependent assemblies are added.</span></span>
6. <span data-ttu-id="58a4a-133">toopromote tisztító fejlesztési környezet, fontolja meg a NuGet-csomagok visszaállításának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="58a4a-133">toopromote a cleaner development environment, consider enabling NuGet Package Restore.</span></span> <span data-ttu-id="58a4a-134">További információkért lásd: [NuGet-csomagok visszaállításának "](http://docs.nuget.org/consume/package-restore).</span><span class="sxs-lookup"><span data-stu-id="58a4a-134">For more information, see [NuGet Package Restore"](http://docs.nuget.org/consume/package-restore).</span></span>
7. <span data-ttu-id="58a4a-135">Adjon hozzá egy hivatkozást túl**System.Configuration** szerelvény.</span><span class="sxs-lookup"><span data-stu-id="58a4a-135">Add a reference too**System.Configuration** assembly.</span></span> <span data-ttu-id="58a4a-136">Ez a szerelvény hello System.Configuration tartalmazza. **ConfigurationManager** osztály, amely használt tooaccess konfigurációs fájljait (például az App.config fájlt).</span><span class="sxs-lookup"><span data-stu-id="58a4a-136">This assembly contains hello System.Configuration.**ConfigurationManager** class that is used tooaccess configuration files (for example, App.config).</span></span>
   
    <span data-ttu-id="58a4a-137">tooadd hivatkozások hello hivatkozások kezelése párbeszédpanelen, kattintson a jobb gombbal hello projekt nevére a Megoldáskezelőben hello.</span><span class="sxs-lookup"><span data-stu-id="58a4a-137">tooadd references using hello Manage References dialog, right-click hello project name in hello Solution Explorer.</span></span> <span data-ttu-id="58a4a-138">Ezt követően válassza hozzáadása és hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="58a4a-138">Then, select Add and References.</span></span>
   
    <span data-ttu-id="58a4a-139">hello hivatkozások kezelése párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="58a4a-139">hello Manage References dialog appears.</span></span>
8. <span data-ttu-id="58a4a-140">A .NET-keretrendszer szerelvényei közé keresése és kiválasztása hello System.Configuration szerelvényre, és kattintson az OK.</span><span class="sxs-lookup"><span data-stu-id="58a4a-140">Under .NET framework assemblies, find and select hello System.Configuration assembly and press OK.</span></span>
9. <span data-ttu-id="58a4a-141">Nyissa meg a hello App.config fájlt, és adja hozzá egy *appSettings* toohello fájlt.</span><span class="sxs-lookup"><span data-stu-id="58a4a-141">Open hello App.config file and add an *appSettings* section toohello file.</span></span>     
   
    <span data-ttu-id="58a4a-142">Állítsa be a szükséges tooconnect toohello Media Services API hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="58a4a-142">Set hello values that are needed tooconnect toohello Media Services API.</span></span> <span data-ttu-id="58a4a-143">További információkért lásd: [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="58a4a-143">For more information, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

    <span data-ttu-id="58a4a-144">Ha használ [felhasználóhitelesítés](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) a konfigurációs fájl szerepel az Azure AD-bérlő tartomány érték és hello AMS REST API-végpont valószínűleg lesz.</span><span class="sxs-lookup"><span data-stu-id="58a4a-144">If you are using [user authentication](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) your config file will probably have values for your Azure AD tenant domain and hello AMS REST API endpoint.</span></span>
    
    >[!Important]
    ><span data-ttu-id="58a4a-145">Hello Azure Media Services dokumentációjában a legtöbb mintakódok beállítása, a felhasználó (interaktív) típusú hitelesítés tooconnect toohello AMS API.</span><span class="sxs-lookup"><span data-stu-id="58a4a-145">Most code samples in hello Azure Media Services documentation set, use a user (interactive) type of authentication tooconnect toohello AMS API.</span></span> <span data-ttu-id="58a4a-146">Ez a hitelesítési módszer alkalmas felügyeleti vagy a natív alkalmazások figyeléséről: mobilalkalmazások, a Windows-alkalmazások és a konzol alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="58a4a-146">This authentication method will work well for management or monitoring native apps: mobile apps, Windows apps, and Console applications.</span></span> <span data-ttu-id="58a4a-147">Ez a hitelesítési módszer nem alkalmas kiszolgáló, web services, az alkalmazások API-k típusának.</span><span class="sxs-lookup"><span data-stu-id="58a4a-147">This authentication method is not suitable for server, web services, APIs type of applications.</span></span>  <span data-ttu-id="58a4a-148">További információkért lásd: [hozzáférés hello AMS API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="58a4a-148">For more information, see [Access hello AMS API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

10. <span data-ttu-id="58a4a-149">Felülírja a meglévő hello **használatával** hello Program.cs fájl a következő kód hello hello elején utasításokat.</span><span class="sxs-lookup"><span data-stu-id="58a4a-149">Overwrite hello existing **using** statements at hello beginning of hello Program.cs file with hello following code.</span></span>
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

<span data-ttu-id="58a4a-150">Ekkor készen áll a Media Services-alkalmazások fejlesztése toostart áll.</span><span class="sxs-lookup"><span data-stu-id="58a4a-150">At this point, you are ready toostart developing a Media Services application.</span></span>    

## <a name="example"></a><span data-ttu-id="58a4a-151">Példa</span><span class="sxs-lookup"><span data-stu-id="58a4a-151">Example</span></span>

<span data-ttu-id="58a4a-152">Íme egy kis példa, toohello AMS API csatlakozik, és megjeleníti az összes elérhető Media processzor.</span><span class="sxs-lookup"><span data-stu-id="58a4a-152">Here is a small example that connects toohello AMS API and lists all available Media Processors.</span></span>
    
    class Program
    {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];
    
        private static CloudMediaContext _context = null;
        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
    
            // List all available Media Processors
            foreach (var mp in _context.MediaProcessors)
            {
                Console.WriteLine(mp.Name);
            }
    
        }

##<a name="next-steps"></a><span data-ttu-id="58a4a-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="58a4a-153">Next steps</span></span>

<span data-ttu-id="58a4a-154">Most [toohello AMS API a kapcsolódás](media-services-use-aad-auth-to-access-ams-api.md) máris [fejlesztése](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="58a4a-154">Now [you can connect toohello AMS API](media-services-use-aad-auth-to-access-ams-api.md) and start [developing](media-services-dotnet-get-started.md).</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="58a4a-155">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="58a4a-155">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="58a4a-156">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="58a4a-156">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

