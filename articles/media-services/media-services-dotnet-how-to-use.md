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
# <a name="media-services-development-with-net"></a>A .NET Media Services-fejlesztés
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Ez a témakör ismerteti, hogyan toostart fejlesztését Media Services .NET használó alkalmazások.

Hello **Azure Media Services .NET SDK** library lehetővé teszi a Media Services .NET segítségével elleni tooprogram. Ez akkor is igaz, a .NET, könnyebben toodevelop hello toomake **Azure Media Services .NET SDK-bővítmények** könyvtár valósul meg. Ezt a szalagtárat kiegészítő módszerek és segédfüggvények találhatók, amelyek egyszerűbbé teszik a .NET-kódot tartalmaz. Mindkét szalagtárak keresztül érhetők el **NuGet** és **GitHub**.

## <a name="prerequisites"></a>Előfeltételek
* Egy Media Services-fiók egy új vagy meglévő Azure-előfizetésben. Hello témakörben [hogyan tooCreate Media Services-fiók](media-services-portal-create-account.md).
* Operációs rendszerek: Windows 10, Windows 7, Windows 2008 R2 vagy Windows 8.
* .NET-keretrendszer 4.5.
* Visual Studio.

## <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása
Ez a szakasz bemutatja, hogyan toocreate a projektre a Visual Studio és a Media Services-fejlesztés beállításához.  Ebben az esetben hello projekt egy Windows C# konzolalkalmazást, de hello itt látható lépéseket telepítő alkalmazása tooother típusú projekteket hozhat létre a Media Services-alkalmazások (például a Windows Forms alkalmazások vagy az ASP.NET webalkalmazás).

Ez a szakasz bemutatja, hogyan toouse **NuGet** tooadd Media Services .NET SDK bővítmények és más függő szalagtárak.

Azt is megteheti, a Githubról kaphat hello legújabb Media Services .NET SDK bits ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) vagy [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), hello megoldás kiépítését, és adja hozzá a hello hivatkozások toohello ügyfélprojekt. Minden hello szükséges függőségek letöltött és kibontott automatikusan.

1. A Visual Studióban hozzon létre egy új Visual C#-konzolalkalmazást. Adja meg a hello **neve**, **hely**, és **megoldás neve**, majd kattintson az OK gombra.
2. Hello megoldás felépítéséhez.
3. Használjon **NuGet** tooinstall, és adja hozzá **Azure Media Services .NET SDK-bővítmények** (**windowsazure.mediaservices.extensions**). Ennek a csomagnak a telepítése a **Media Services .NET SDK**csomagot és az összes további szükséges függőséget is feltelepíti
   
    Győződjön meg arról, hogy rendelkezik-e telepítve NuGet hello legújabb verzióját. További információk és a telepítési utasításokért lásd: [NuGet](http://nuget.codeplex.com/).
4. A Megoldáskezelőben kattintson a jobb gombbal a projekt hello hello nevét, és válassza a kezelése NuGet-csomagok.
   
    hello NuGet-csomagok kezelése párbeszédpanel jelenik meg.
5. Hello Online katalógusban, keresse meg az Azure-bővítményekkel MediaServices, válassza ki az Azure Media Services .NET SDK-bővítmények, és kattintson a hello telepítése gombra.
   
    hello projekt módosul, és hivatkozik arra toohello Media Services .NET SDK-bővítmények, Media Services .NET SDK-t, és az egyéb függő szerelvényeket.
6. toopromote tisztító fejlesztési környezet, fontolja meg a NuGet-csomagok visszaállításának engedélyezése. További információkért lásd: [NuGet-csomagok visszaállításának "](http://docs.nuget.org/consume/package-restore).
7. Adjon hozzá egy hivatkozást túl**System.Configuration** szerelvény. Ez a szerelvény hello System.Configuration tartalmazza. **ConfigurationManager** osztály, amely használt tooaccess konfigurációs fájljait (például az App.config fájlt).
   
    tooadd hivatkozások hello hivatkozások kezelése párbeszédpanelen, kattintson a jobb gombbal hello projekt nevére a Megoldáskezelőben hello. Ezt követően válassza hozzáadása és hivatkozásokat.
   
    hello hivatkozások kezelése párbeszédpanel jelenik meg.
8. A .NET-keretrendszer szerelvényei közé keresése és kiválasztása hello System.Configuration szerelvényre, és kattintson az OK.
9. Nyissa meg a hello App.config fájlt, és adja hozzá egy *appSettings* toohello fájlt.     
   
    Állítsa be a szükséges tooconnect toohello Media Services API hello értékeket. További információkért lásd: [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md). 

    Ha használ [felhasználóhitelesítés](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) a konfigurációs fájl szerepel az Azure AD-bérlő tartomány érték és hello AMS REST API-végpont valószínűleg lesz.
    
    >[!Important]
    >Hello Azure Media Services dokumentációjában a legtöbb mintakódok beállítása, a felhasználó (interaktív) típusú hitelesítés tooconnect toohello AMS API. Ez a hitelesítési módszer alkalmas felügyeleti vagy a natív alkalmazások figyeléséről: mobilalkalmazások, a Windows-alkalmazások és a konzol alkalmazások. Ez a hitelesítési módszer nem alkalmas kiszolgáló, web services, az alkalmazások API-k típusának.  További információkért lásd: [hozzáférés hello AMS API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

10. Felülírja a meglévő hello **használatával** hello Program.cs fájl a következő kód hello hello elején utasításokat.
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

Ekkor készen áll a Media Services-alkalmazások fejlesztése toostart áll.    

## <a name="example"></a>Példa

Íme egy kis példa, toohello AMS API csatlakozik, és megjeleníti az összes elérhető Media processzor.
    
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

##<a name="next-steps"></a>Következő lépések

Most [toohello AMS API a kapcsolódás](media-services-use-aad-auth-to-access-ams-api.md) máris [fejlesztése](media-services-dotnet-get-started.md).


## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

