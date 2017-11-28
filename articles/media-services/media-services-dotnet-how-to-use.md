---
title: "Media Services a .NET Development számítógép beállítása"
description: "További információk a Media Services .NET-keretrendszerhez készült Media Services SDK segítségével előfeltételeit. Is megtudhatja, hogyan hozzon létre egy Visual Studio alkalmazást."
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
ms.openlocfilehash: 15828bc74937a036871b26493498232ec7cf6f06
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="media-services-development-with-net"></a><span data-ttu-id="252c6-104">A .NET Media Services-fejlesztés</span><span class="sxs-lookup"><span data-stu-id="252c6-104">Media Services development with .NET</span></span>
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

<span data-ttu-id="252c6-105">Ez a témakör ismerteti, hogyan kell elindítani a .NET használatával Media Services-alkalmazások fejlesztésével.</span><span class="sxs-lookup"><span data-stu-id="252c6-105">This topic discusses how to start developing Media Services applications using .NET.</span></span>

<span data-ttu-id="252c6-106">A **Azure Media Services .NET SDK** library lehetővé teszi a Media Services .NET segítségével elleni programhoz.</span><span class="sxs-lookup"><span data-stu-id="252c6-106">The **Azure Media Services .NET SDK** library enables you to program against Media Services using .NET.</span></span> <span data-ttu-id="252c6-107">Még jobban könnyebb a használatával történő fejlesztést .NET, a **Azure Media Services .NET SDK-bővítmények** könyvtár valósul meg.</span><span class="sxs-lookup"><span data-stu-id="252c6-107">To make it even easier to develop with .NET, the **Azure Media Services .NET SDK Extensions** library is provided.</span></span> <span data-ttu-id="252c6-108">Ezt a szalagtárat kiegészítő módszerek és segédfüggvények találhatók, amelyek egyszerűbbé teszik a .NET-kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="252c6-108">This library contains a set of extension methods and helper functions that simplify your .NET code.</span></span> <span data-ttu-id="252c6-109">Mindkét szalagtárak keresztül érhetők el **NuGet** és **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="252c6-109">Both libraries are available through **NuGet** and **GitHub**.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="252c6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="252c6-110">Prerequisites</span></span>
* <span data-ttu-id="252c6-111">Egy Media Services-fiók egy új vagy meglévő Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="252c6-111">A Media Services account in a new or existing Azure subscription.</span></span> <span data-ttu-id="252c6-112">További információ: [Media Services-fiók létrehozása](media-services-portal-create-account.md)</span><span class="sxs-lookup"><span data-stu-id="252c6-112">See the topic [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="252c6-113">Operációs rendszerek: Windows 10, Windows 7, Windows 2008 R2 vagy Windows 8.</span><span class="sxs-lookup"><span data-stu-id="252c6-113">Operating Systems: Windows 10, Windows 7, Windows 2008 R2, or Windows 8.</span></span>
* <span data-ttu-id="252c6-114">.NET-keretrendszer 4.5.</span><span class="sxs-lookup"><span data-stu-id="252c6-114">.NET Framework 4.5.</span></span>
* <span data-ttu-id="252c6-115">Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="252c6-115">Visual Studio.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="252c6-116">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="252c6-116">Create and configure a Visual Studio project</span></span>
<span data-ttu-id="252c6-117">Ez a szakasz bemutatja, hogyan elvégezze a Media Services-fejlesztés és a Visual Studio-projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="252c6-117">This section shows you how to create a project in Visual Studio and set it up for Media Services development.</span></span>  <span data-ttu-id="252c6-118">Ebben az esetben a projekt egy Windows C# konzolalkalmazást, de a telepítő lépéseket Itt hozhat létre a Media Services-alkalmazások (például a Windows Forms alkalmazások vagy az ASP.NET webalkalmazás) projektek más típusú vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="252c6-118">In this case, the project is a C# Windows console application, but the same setup steps shown here apply to other types of projects you can create for Media Services applications (for example, a Windows Forms application or an ASP.NET Web application).</span></span>

<span data-ttu-id="252c6-119">Ez a szakasz bemutatja, hogyan használható **NuGet** Media Services .NET SDK-bővítmények és más függő kódtárak hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="252c6-119">This section shows how to use **NuGet** to add Media Services .NET SDK extensions and other dependent libraries.</span></span>

<span data-ttu-id="252c6-120">Azt is megteheti, a Media Services .NET SDK legújabb bits kaphat a Githubról ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) vagy [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), a megoldás felépítéséhez és az ügyfélprojekt hivatkozásokat adni.</span><span class="sxs-lookup"><span data-stu-id="252c6-120">Alternatively, you can get the latest Media Services .NET SDK bits from GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) or [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), build the solution, and add the references to the client project.</span></span> <span data-ttu-id="252c6-121">A szükséges függőségek letöltött és kibontott automatikusan.</span><span class="sxs-lookup"><span data-stu-id="252c6-121">All the necessary dependencies get downloaded and extracted automatically.</span></span>

1. <span data-ttu-id="252c6-122">A Visual Studióban hozzon létre egy új Visual C#-konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="252c6-122">Create a new C# Console Application in Visual Studio.</span></span> <span data-ttu-id="252c6-123">Adja meg a **neve**, **hely**, és **megoldásnév**, majd kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="252c6-123">Enter the **Name**, **Location**, and **Solution name**, and then click OK.</span></span>
2. <span data-ttu-id="252c6-124">A megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="252c6-124">Build the solution.</span></span>
3. <span data-ttu-id="252c6-125">Használjon **NuGet** telepítése és hozzáadása **Azure Media Services .NET SDK-bővítmények** (**windowsazure.mediaservices.extensions**).</span><span class="sxs-lookup"><span data-stu-id="252c6-125">Use **NuGet** to install and add **Azure Media Services .NET SDK Extensions** (**windowsazure.mediaservices.extensions**).</span></span> <span data-ttu-id="252c6-126">Ennek a csomagnak a telepítése a **Media Services .NET SDK**csomagot és az összes további szükséges függőséget is feltelepíti</span><span class="sxs-lookup"><span data-stu-id="252c6-126">Installing this package, also installs **Media Services .NET SDK** and adds all other required dependencies.</span></span>
   
    <span data-ttu-id="252c6-127">Győződjön meg arról, hogy rendelkezik-e telepítve NuGet legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="252c6-127">Ensure that you have the newest version of NuGet installed.</span></span> <span data-ttu-id="252c6-128">További információk és a telepítési utasításokért lásd: [NuGet](http://nuget.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="252c6-128">For more information and installation instructions, see [NuGet](http://nuget.codeplex.com/).</span></span>
4. <span data-ttu-id="252c6-129">A Megoldáskezelőben kattintson a jobb gombbal a projekt nevét, és válassza ki a kezelni NuGet-csomagok.</span><span class="sxs-lookup"><span data-stu-id="252c6-129">In Solution Explorer, right-click the name of the project and choose Manage NuGet packages.</span></span>
   
    <span data-ttu-id="252c6-130">A NuGet-csomagok kezelése párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="252c6-130">The Manage NuGet Packages dialog box appears.</span></span>
5. <span data-ttu-id="252c6-131">Az Online katalógusban Azure MediaServices bővítmények keressen, válassza ki az Azure Media Services .NET SDK-bővítmények, majd kattintson a telepítés gombra.</span><span class="sxs-lookup"><span data-stu-id="252c6-131">In the Online gallery, search for Azure MediaServices Extensions, choose Azure Media Services .NET SDK Extensions, and then click the Install button.</span></span>
   
    <span data-ttu-id="252c6-132">A projekt módosul, és a Media Services .NET SDK-bővítmények, a Media Services .NET SDK-t és a többi függő szerelvényeket kerülnek.</span><span class="sxs-lookup"><span data-stu-id="252c6-132">The project is modified and references to the Media Services .NET SDK Extensions,  Media Services .NET SDK, and other dependent assemblies are added.</span></span>
6. <span data-ttu-id="252c6-133">A tisztító környezet előléptetéséhez, fontolja meg a NuGet-csomagok visszaállításának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="252c6-133">To promote a cleaner development environment, consider enabling NuGet Package Restore.</span></span> <span data-ttu-id="252c6-134">További információkért lásd: [NuGet-csomagok visszaállításának "](http://docs.nuget.org/consume/package-restore).</span><span class="sxs-lookup"><span data-stu-id="252c6-134">For more information, see [NuGet Package Restore"](http://docs.nuget.org/consume/package-restore).</span></span>
7. <span data-ttu-id="252c6-135">Adjon hozzá egy hivatkozást, **System.Configuration** szerelvény.</span><span class="sxs-lookup"><span data-stu-id="252c6-135">Add a reference to **System.Configuration** assembly.</span></span> <span data-ttu-id="252c6-136">Ez a szerelvény tartalmazza a System.Configuration. **ConfigurationManager** osztály, amely a konfigurációs fájlok (például App.config) elérésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="252c6-136">This assembly contains the System.Configuration.**ConfigurationManager** class that is used to access configuration files (for example, App.config).</span></span>
   
    <span data-ttu-id="252c6-137">A hivatkozások kezelése párbeszédpanelen hivatkozások hozzáadásához kattintson a jobb gombbal a projekt nevére a Megoldáskezelőben.</span><span class="sxs-lookup"><span data-stu-id="252c6-137">To add references using the Manage References dialog, right-click the project name in the Solution Explorer.</span></span> <span data-ttu-id="252c6-138">Ezt követően válassza hozzáadása és hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="252c6-138">Then, select Add and References.</span></span>
   
    <span data-ttu-id="252c6-139">A hivatkozások kezelése párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="252c6-139">The Manage References dialog appears.</span></span>
8. <span data-ttu-id="252c6-140">A .NET-keretrendszer szerelvényei közé keresse meg és válassza ki a System.Configuration szerelvényre, és az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="252c6-140">Under .NET framework assemblies, find and select the System.Configuration assembly and press OK.</span></span>
9. <span data-ttu-id="252c6-141">Nyissa meg az App.config fájlt, és adja hozzá egy *appSettings* a fájl a szakaszban található.</span><span class="sxs-lookup"><span data-stu-id="252c6-141">Open the App.config file and add an *appSettings* section to the file.</span></span>     
   
    <span data-ttu-id="252c6-142">Állítsa be az értékeket, amelyek szükségesek ahhoz, hogy a Media Services API csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="252c6-142">Set the values that are needed to connect to the Media Services API.</span></span> <span data-ttu-id="252c6-143">További információkért lásd: [elérni az Azure Media Services API-t az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="252c6-143">For more information, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

    <span data-ttu-id="252c6-144">Ha használ [felhasználóhitelesítés](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) a konfigurációs fájlban kell az Azure AD-bérlő tartomány és az AMS REST API-végpont értékeit.</span><span class="sxs-lookup"><span data-stu-id="252c6-144">If you are using [user authentication](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) your config file will probably have values for your Azure AD tenant domain and the AMS REST API endpoint.</span></span>
    
    >[!Important]
    ><span data-ttu-id="252c6-145">Az Azure Media Services dokumentációjában legtöbb mintakódok beállításához csatlakozhatnak egy felhasználói (interaktív) típusú hitelesítés az AMS API-t.</span><span class="sxs-lookup"><span data-stu-id="252c6-145">Most code samples in the Azure Media Services documentation set, use a user (interactive) type of authentication to connect to the AMS API.</span></span> <span data-ttu-id="252c6-146">Ez a hitelesítési módszer alkalmas felügyeleti vagy a natív alkalmazások figyeléséről: mobilalkalmazások, a Windows-alkalmazások és a konzol alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="252c6-146">This authentication method will work well for management or monitoring native apps: mobile apps, Windows apps, and Console applications.</span></span> <span data-ttu-id="252c6-147">Ez a hitelesítési módszer nem alkalmas kiszolgáló, web services, az alkalmazások API-k típusának.</span><span class="sxs-lookup"><span data-stu-id="252c6-147">This authentication method is not suitable for server, web services, APIs type of applications.</span></span>  <span data-ttu-id="252c6-148">További információkért lásd: [elérni az AMS API-t az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="252c6-148">For more information, see [Access the AMS API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

10. <span data-ttu-id="252c6-149">Írja felül a meglévő **használati** nyilatkozatokat a Program.cs fájl elején a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="252c6-149">Overwrite the existing **using** statements at the beginning of the Program.cs file with the following code.</span></span>
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

<span data-ttu-id="252c6-150">Ekkor készen áll a Media Services-alkalmazás elkezdje.</span><span class="sxs-lookup"><span data-stu-id="252c6-150">At this point, you are ready to start developing a Media Services application.</span></span>    

## <a name="example"></a><span data-ttu-id="252c6-151">Példa</span><span class="sxs-lookup"><span data-stu-id="252c6-151">Example</span></span>

<span data-ttu-id="252c6-152">Íme egy kis példa, az AMS API-hoz csatlakoztatja, és megjeleníti az összes elérhető Media processzor.</span><span class="sxs-lookup"><span data-stu-id="252c6-152">Here is a small example that connects to the AMS API and lists all available Media Processors.</span></span>
    
    class Program
    {
        // Read values from the App.config file.
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

##<a name="next-steps"></a><span data-ttu-id="252c6-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="252c6-153">Next steps</span></span>

<span data-ttu-id="252c6-154">Most [csatlakozhat az AMS API](media-services-use-aad-auth-to-access-ams-api.md) máris [fejlesztése](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="252c6-154">Now [you can connect to the AMS API](media-services-use-aad-auth-to-access-ams-api.md) and start [developing](media-services-dotnet-get-started.md).</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="252c6-155">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="252c6-155">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="252c6-156">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="252c6-156">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

