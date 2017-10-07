---
title: "az Azure Redis cache az Azure App Service aaaSession állapota"
description: "Ismerje meg, hogyan toouse hello Azure Cache Service toosupport az ASP.NET munkamenet-állapotának gyorsítótárazásához."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
manager: erikre
editor: none
ms.assetid: 4f98d289-2698-464d-85cd-99ec40fad1f2
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 06/27/2016
ms.author: riande
ms.openlocfilehash: f689b6754ea072aa195f822ab6482f4bf2748375
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Munkamenet-állapot az Azure Redis Cache-ben, az Azure App Service szolgáltatásban
Ez a témakör azt ismerteti, hogyan toouse hello Azure Redis Cache szolgáltatás a munkamenet-állapot.

Ha az ASP.NET webalkalmazás munkamenet-állapotot használ, akkor tooconfigure egy külső állapotszolgáltatót (hello Redis Cache Service vagy egy SQL Server munkamenetállapot-szolgáltatóját). Munkamenet-állapotot használ, és nem használ külső szolgáltatót, ha a webalkalmazás korlátozott tooone példányát fogja. hello Redis Cache Service hello leggyorsabb és legegyszerűbb tooenable.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a id="createcache"></a>Hello gyorsítótár létrehozása
Hajtsa végre a [ezt az útmutatót](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) toocreate hello gyorsítótár.

## <a id="configureproject"></a>Hello RedisSessionStateProvider NuGet csomag tooyour webalkalmazás hozzáadása
Telepítse a hello NuGet `RedisSessionStateProvider` csomag.  Használjon hello következő parancsot a tooinstall hello package manager konzolról (**eszközök** > **NuGet-Csomagkezelő** > **Csomagkezelő konzol**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`

a tooinstall **eszközök** > **NuGet-Csomagkezelő** > **Nuget-csomagok kezelése megoldáshoz**, keressen `RedisSessionStateProvider`.

További információ: hello [NuGet RedisSessionStateProvider oldal](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) és [hello gyorsítótárügyfél konfigurálása](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

## <a id="configurewebconfig"></a>Hello Web.Config fájl módosítása
Ezenkívül toomaking szerelvény által hivatkozott gyorsítótár, a NuGet-csomag hello csonkbejegyzéseket ad a hello *web.config* fájlt. 

1. Nyissa meg hello *web.config* és hello hello található **sessionState** elemet.
2. Adja meg a hello értékeket `host`, `accessKey`, `port` (hello SSL-port legyen 6380), és állítsa be `SSL` túl`true`. Ezek az értékek hello szerezhető [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) a gyorsítótárpéldány panelen. További információkért lásd: [toohello gyorsítótár csatlakozás](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Vegye figyelembe, hogy az új gyorsítótárakhoz alapértelmezés szerint le van-e tiltva hello nem SSL port. Hello nem SSL port engedélyezésével kapcsolatos további információkért lásd: hello [hozzáférési portok](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) hello szakasz [gyorsítótár konfigurálása az Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) témakör. hello következő kódban láthatók hello módosítások toohello *web.config* fájlt, kifejezetten hello módosítások túl*port*, *állomás*, accessKey *, és *ssl*.
   
          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;

## <a id="usesessionobject"></a>Hello munkamenet-objektum használata a kódban
utolsó lépésként hello toobegin hello munkamenet-objektum használatát az ASP.NET-kódban. Objektumok toosession állapot hello segítségével adja hozzá **Session.Add** metódust. Ez a metódus kulcs-érték párok toostore elemeket használja a hello munkamenetállapot-gyorsítótárban.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

hello a következő kód lekéri ezt az értéket a munkamenet-állapotból.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue;    

A webes alkalmazás is használhatja hello Redis Cache toocache objektumok. További információk: [MVC movie app with Azure Redis Cache in 15 minutes](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/) (MVC filmalkalmazás 15 perc alatt az Azure Redis Cache segítségével).
További részletes információt toouse az ASP.NET munkamenet-állapot, lásd: [ASP.NET munkamenet-állapot – áttekintés][ASP.NET Session State Overview].

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)
  
  *Szerző: [Rick Anderson](https://twitter.com/RickAndMSFT)*

[installed hello latest]: http://www.windowsazure.com/downloads/?sdk=net  
[ASP.NET Session State Overview]: http://msdn.microsoft.com/library/ms178581.aspx

[NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
[NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
[CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
[NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
[OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
[CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
[EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
[ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png

