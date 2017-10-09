---
title: "aaaAzure webalkalmazást az App Service speciális konfigurálása és bővítményei"
description: "XML-dokumentum Transformation(XDT) nyilatkozatok tootransform hello ApplicationHost.config fájlt használja a az Azure App Service web app és tooadd magánhálózati kiterjesztésének tooenable egyéni felügyeleti műveletek."
author: cephalin
writer: cephalin
editor: mollybos
manager: erikre
services: app-service
documentationcenter: 
ms.assetid: b441a286-ef38-4abc-b102-cdb249baf5bc
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.openlocfilehash: 873347ac13113d1ac989cba29128382c81dcfcca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a><span data-ttu-id="b84bf-103">Az Azure App Service web app speciális konfigurálása és bővítményei</span><span class="sxs-lookup"><span data-stu-id="b84bf-103">Azure App Service web app advanced config and extensions</span></span>
<span data-ttu-id="b84bf-104">A [XML-dokumentum átalakítása](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) nyilatkozatok, hello alakíthatja át [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) fájl az Azure App Service szolgáltatásban a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b84bf-104">By using [XML Document Transformation](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) declarations, you can transform hello [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) file in your web app in Azure App Service.</span></span> <span data-ttu-id="b84bf-105">XDT nyilatkozatok tooadd magánhálózati kiterjesztésének tooenable egyéni web app felügyeleti műveletek is használható.</span><span class="sxs-lookup"><span data-stu-id="b84bf-105">You can also use XDT declarations tooadd private extensions tooenable custom web app administration actions.</span></span> <span data-ttu-id="b84bf-106">A cikk tartalmaz egy mintát PHP Manager webalkalmazás-bővítmény, amely lehetővé teszi a webes felületen keresztül PHP-beállítások kezelését.</span><span class="sxs-lookup"><span data-stu-id="b84bf-106">This article includes a sample PHP Manager web app extension that enables management of PHP settings through a web interface.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="b84bf-107"><a id="transform"></a>Speciális konfigurációs ApplicationHost.config keresztül</span><span class="sxs-lookup"><span data-stu-id="b84bf-107"><a id="transform"></a>Advanced configuration through ApplicationHost.config</span></span>
<span data-ttu-id="b84bf-108">App Service platform hello rugalmasságot és biztosít webes alkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b84bf-108">hello App Service platform provides flexibility and control for web app configuration.</span></span> <span data-ttu-id="b84bf-109">Bár az App Service közvetlen szerkesztésre hello szabványos IIS ApplicationHost.config konfigurációs fájl nem érhető el, hello platformja támogatja a deklaratív ApplicationHost.config átalakító modellben XML dokumentum átalakítása (XDT).</span><span class="sxs-lookup"><span data-stu-id="b84bf-109">Although hello standard IIS ApplicationHost.config configuration file is not available for direct editing in App Service, hello platform supports a declarative ApplicationHost.config transform model based on XML Document Transformation (XDT).</span></span>

<span data-ttu-id="b84bf-110">tooleverage ezt átalakítás a funkciót, akkor hozzon létre egy ApplicationHost.xdt fájlt XDT tartalom és hello webhely gyökeréhez (d:\home\site) a hello alá helyezni [Kudu konzol](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span><span class="sxs-lookup"><span data-stu-id="b84bf-110">tooleverage this transform functionality, you create an ApplicationHost.xdt file with XDT content and place under hello site root (d:\home\site) in hello [Kudu Console](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span></span> <span data-ttu-id="b84bf-111">Módosítások tootake hatás toorestart hello webalkalmazás szükséges.</span><span class="sxs-lookup"><span data-stu-id="b84bf-111">You may need toorestart hello Web App for changes tootake effect.</span></span>

<span data-ttu-id="b84bf-112">a következő applicationHost.xdt minta hello jeleníti meg, hogyan tooadd egy új egyéni környezeti változó tooa webalkalmazás, amely használja a PHP 5.4-es.</span><span class="sxs-lookup"><span data-stu-id="b84bf-112">hello following applicationHost.xdt sample shows how tooadd a new custom environment variable tooa web app that uses PHP 5.4.</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <fastCgi>
      <application>
        <environmentVariables>
          <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
        </environmentVariables>
      </application>
    </fastCgi>
  </system.webServer>
</configuration>
```

<span data-ttu-id="b84bf-113">A naplófájl átalakító állapotával és részleteivel kapcsolatban hello FTP gyökérkönyvtárából LogFiles\Transform alatt érhető el.</span><span class="sxs-lookup"><span data-stu-id="b84bf-113">A log file with transform status and details is available from hello FTP root under LogFiles\Transform.</span></span>

<span data-ttu-id="b84bf-114">További példákért lásd: [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).</span><span class="sxs-lookup"><span data-stu-id="b84bf-114">For additional samples, see [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).</span></span>

<span data-ttu-id="b84bf-115">**Megjegyzés**</span><span class="sxs-lookup"><span data-stu-id="b84bf-115">**Note**</span></span><br />
<span data-ttu-id="b84bf-116">Hello listájához a elemek `system.webServer` nem távolítható el, és újra, de kiegészítéseket toohello lista is előfordulhatnak.</span><span class="sxs-lookup"><span data-stu-id="b84bf-116">Elements from hello list of modules under `system.webServer` cannot be removed or reordered, but additions toohello list are possible.</span></span>

## <span data-ttu-id="b84bf-117"><a id="extend"></a>A webalkalmazás kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="b84bf-117"><a id="extend"></a> Extend your web app</span></span>
### <span data-ttu-id="b84bf-118"><a id="overview"></a>Saját webalkalmazás-bővítmény áttekintése</span><span class="sxs-lookup"><span data-stu-id="b84bf-118"><a id="overview"></a> Overview of private web app extensions</span></span>
<span data-ttu-id="b84bf-119">App Service webalkalmazás-bővítmény támogatja a felügyeleti műveletekhez bővítési pontjaként.</span><span class="sxs-lookup"><span data-stu-id="b84bf-119">App Service supports web app extensions as an extensibility point for administrative actions.</span></span> <span data-ttu-id="b84bf-120">Tulajdonképpen egy App Service platform funkciói előre telepített kiterjesztéseket valósíthatók meg.</span><span class="sxs-lookup"><span data-stu-id="b84bf-120">In fact, some App Service platform features are implemented as pre-installed extensions.</span></span> <span data-ttu-id="b84bf-121">Hello előre telepített platform bővítményei nem módosíthatók, amíg hozhat létre és magánhálózati kiterjesztésének konfigurálása a saját webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b84bf-121">While hello pre-installed platform extensions cannot be modified, you can create and configure private extensions for your own web app.</span></span> <span data-ttu-id="b84bf-122">Ez a funkció is XDT nyilatkozatok támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="b84bf-122">This functionality also relies on XDT declarations.</span></span> <span data-ttu-id="b84bf-123">hello kulcs egy saját webalkalmazás-bővítmény létrehozásának lépései a következők hello következő:</span><span class="sxs-lookup"><span data-stu-id="b84bf-123">hello key steps for creating a private web app extension are hello following:</span></span>

1. <span data-ttu-id="b84bf-124">Webalkalmazás-bővítmény **tartalom**: bármely támogatja az App Service webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b84bf-124">Web app extension **content**: create any web application supported by App Service</span></span>
2. <span data-ttu-id="b84bf-125">Webalkalmazás-bővítmény **deklaráció**: ApplicationHost.xdt-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="b84bf-125">Web app extension **declaration**: create an ApplicationHost.xdt file</span></span>
3. <span data-ttu-id="b84bf-126">Webalkalmazás-bővítmény **telepítési**: hello SiteExtensions mappában található a tartalom elhelyezése`root`</span><span class="sxs-lookup"><span data-stu-id="b84bf-126">Web app extension **deployment**: place content in hello SiteExtensions folder under `root`</span></span>

<span data-ttu-id="b84bf-127">Belső hivatkozások hello webalkalmazás tooa elérési útja relatív toohello alkalmazás elérési útja hello ApplicationHost.xdt fájlban megadott kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="b84bf-127">Internal links for hello web app should point tooa path relative toohello application path specified in hello ApplicationHost.xdt file.</span></span> <span data-ttu-id="b84bf-128">Minden olyan változás toohello ApplicationHost.xdt fájlt web app újrahasznosítást igényel.</span><span class="sxs-lookup"><span data-stu-id="b84bf-128">Any change toohello ApplicationHost.xdt file requires a web app recycle.</span></span>

<span data-ttu-id="b84bf-129">**Megjegyzés:**: további információt a fő elemei a [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).</span><span class="sxs-lookup"><span data-stu-id="b84bf-129">**Note**: Additional information for these key elements is available at [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).</span></span>

<span data-ttu-id="b84bf-130">A részletes belefoglalt tooillustrate hello lépései létrehozásával és engedélyezésével a saját webalkalmazás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="b84bf-130">A detailed example is included tooillustrate hello steps for creating and enabling a private web app extension.</span></span> <span data-ttu-id="b84bf-131">például hello PHP kezelő, amely a következőképpen letölthető a forráskód hello [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).</span><span class="sxs-lookup"><span data-stu-id="b84bf-131">hello source code for hello PHP Manager example that follows can be downloaded from [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).</span></span>

### <span data-ttu-id="b84bf-132"><a id="SiteSample"></a>Webes alkalmazás bővítmény példa: PHP-kezelő</span><span class="sxs-lookup"><span data-stu-id="b84bf-132"><a id="SiteSample"></a> Web app extension example: PHP Manager</span></span>
<span data-ttu-id="b84bf-133">PHP-kezelő rendszer egy webalkalmazás-bővítmény, amely lehetővé teszi, hogy a webes alkalmazás rendszergazdák tooeasily nézet, és a webes felületen keresztül közvetlenül toomodify PHP .ini-fájlokat ahelyett PHP-beállítások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b84bf-133">PHP Manager is a web app extension that allows web app administrators tooeasily view and configure their PHP settings using a web interface instead of having toomodify PHP .ini files directly.</span></span> <span data-ttu-id="b84bf-134">Php-hez tartozó általános konfigurációs fájljai közé hello php.ini fájlt a Program Files és hello található. a webalkalmazás gyökérmappájában hello user.ini fájlba.</span><span class="sxs-lookup"><span data-stu-id="b84bf-134">Common configuration files for PHP include hello php.ini file located under Program Files and hello .user.ini file located in hello root folder of your web app.</span></span> <span data-ttu-id="b84bf-135">Mivel hello php.ini fájl nem közvetlenül szerkeszthetők a hello App Service platform, a PHP-kezelő bővítmény hello használja-e a hello. user.ini fájl tooapply változás.</span><span class="sxs-lookup"><span data-stu-id="b84bf-135">Since hello php.ini file is not directly editable on hello App Service platform, hello PHP Manager extension uses hello .user.ini file tooapply setting changes.</span></span>

#### <span data-ttu-id="b84bf-136"><a id="PHPwebapp"></a>hello PHP Manager webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="b84bf-136"><a id="PHPwebapp"></a> hello PHP Manager web application</span></span>
<span data-ttu-id="b84bf-137">hello az alábbiakban látható hello kezdőlapján hello PHP Manager telepítése:</span><span class="sxs-lookup"><span data-stu-id="b84bf-137">hello following is hello home page of hello PHP Manager deployment:</span></span>

![TransformSitePHPUI][TransformSitePHPUI]

<span data-ttu-id="b84bf-139">Ahogy látja, a webalkalmazás-bővítmény hasonlóan rendszeres webalkalmazást, de egy további ApplicationHost.xdt fájl hello gyökérmappájában hello web app (hello ApplicationHost.xdt fájllal kapcsolatos további részletekért érhetők el az e hello a következő szakaszban van-e a cikk).</span><span class="sxs-lookup"><span data-stu-id="b84bf-139">As you can see, a web app extension is just like a regular web application, but with an additional ApplicationHost.xdt file placed in hello root folder of hello web app (more details about hello ApplicationHost.xdt file are available in hello next section of this article).</span></span>

<span data-ttu-id="b84bf-140">PHP-kezelő bővítmény hello hello Visual Studio ASP.NET MVC 4 webalkalmazás sablon használatával lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="b84bf-140">hello PHP Manager extension was created using hello Visual Studio ASP.NET MVC 4 Web Application template.</span></span> <span data-ttu-id="b84bf-141">hello következő nézet megoldáskezelőjében jeleníti meg a PHP-kezelő bővítmény hello hello szerkezete.</span><span class="sxs-lookup"><span data-stu-id="b84bf-141">hello following view from Solution Explorer shows hello structure of hello PHP Manager extension.</span></span>

![TransformSiteSolEx][TransformSiteSolEx]

<span data-ttu-id="b84bf-143">hello szükséges i/o-fájl csak speciális logikát tooindicate ahol hello wwwroot könyvtárat hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b84bf-143">hello only special logic needed for file I/O is tooindicate where hello wwwroot directory of hello web app is located.</span></span> <span data-ttu-id="b84bf-144">Módon hello következő példát mutat be, hello környezeti változó "OTTHONI" hello webes alkalmazás elérési útjának gyökeréhez és hello wwwroot elérési út lehet létrehozni "site\wwwroot" hozzáfűzésével jelzi:</span><span class="sxs-lookup"><span data-stu-id="b84bf-144">As hello following code example shows, hello environment variable "HOME" indicates hello web app's root path, and hello wwwroot path can be constructed by appending "site\wwwroot":</span></span>

```csharp
/// <summary>
/// Gives hello location of hello .user.ini file, even if one doesn't exist yet
/// </summary>
private static string GetUserSettingsFilePath()
{
  var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
  if (rootPath == null)
  {
    rootPath = System.IO.Path.GetTempPath(); // For testing purposes
  };
  var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
  return userSettingsFile;
}
```


<span data-ttu-id="b84bf-145">Miután hello könyvtár elérési útja, használhatja a rendszeres fájl i/o-műveletek tooread és toofiles írni.</span><span class="sxs-lookup"><span data-stu-id="b84bf-145">After you have hello directory path, you can use regular file I/O operations tooread and write toofiles.</span></span>

<span data-ttu-id="b84bf-146">Egy pont a figyelmeztetés a webalkalmazás-bővítmény hello kezelésére vonatkozó belső hivatkozások tekintetében.</span><span class="sxs-lookup"><span data-stu-id="b84bf-146">One point of caution with web app extensions regards hello handling of internal links.</span></span>  <span data-ttu-id="b84bf-147">Ha a HTML-fájlok, amelyek a webalkalmazásban toointernal hivatkozások abszolút elérési utakat biztosítanak a hivatkozásokat, győződjön meg ezeket a hivatkozásokat, a bővítmény neve, a legfelső szintű vannak $a.</span><span class="sxs-lookup"><span data-stu-id="b84bf-147">If you have any links in your HTML files that give absolute paths toointernal links on your web app, you must ensure those links are prepended with your extension name as your root.</span></span> <span data-ttu-id="b84bf-148">Ez van szükség, mert hello legfelső szintű a bővítmény jelenleg "/`[your-extension-name]`/" nem csak "/", ezért minden belső hivatkozások frissíteni kell ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="b84bf-148">This is needed because hello root for your extension is now "/`[your-extension-name]`/" rather than being just "/", so any internal links must be updated accordingly.</span></span> <span data-ttu-id="b84bf-149">Tegyük fel, hogy a kód egy hivatkozás toohello következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b84bf-149">For example, suppose your code includes a link toohello following:</span></span>

`"<a href="/Home/Settings">PHP Settings</a>"`

<span data-ttu-id="b84bf-150">Ha hello hivatkozás egy webalkalmazás-bővítmény része, hello hivatkozás kell a következő képernyő hello:</span><span class="sxs-lookup"><span data-stu-id="b84bf-150">When hello link is part of a web app extension, hello link must be in hello following form:</span></span>

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

<span data-ttu-id="b84bf-151">Ez a követelmény csak relatív elérési utakat a webes alkalmazás belül, vagy a kis-és ASP.NET-alkalmazások hello hello segítségével segítségével úgy kerülheti `@Html.ActionLink` hello megfelelő hivatkozásokat hoz meg metódust.</span><span class="sxs-lookup"><span data-stu-id="b84bf-151">You can work around this requirement by either using only relative paths within your web application, or in hello case of ASP.NET applications, by using hello `@Html.ActionLink` method which creates hello appropriate links for you.</span></span>

#### <span data-ttu-id="b84bf-152"><a id="XDT"></a>hello applicationHost.xdt fájl</span><span class="sxs-lookup"><span data-stu-id="b84bf-152"><a id="XDT"></a> hello applicationHost.xdt file</span></span>
<span data-ttu-id="b84bf-153">a webalkalmazás-bővítmény hello kódját kerül a %HOME%\SiteExtensions\[a bővítmény neve].</span><span class="sxs-lookup"><span data-stu-id="b84bf-153">hello code for your web app extension goes under %HOME%\SiteExtensions\[your-extension-name].</span></span> <span data-ttu-id="b84bf-154">Felhívjuk a hello bővítmény gyökér.</span><span class="sxs-lookup"><span data-stu-id="b84bf-154">We'll call this hello extension root.</span></span>  

<span data-ttu-id="b84bf-155">tooregister a webalkalmazás-bővítmény hello applicationHost.config fájlt, kell tooplace hello bővítmény legfelső szintű ApplicationHost.xdt nevű fájl.</span><span class="sxs-lookup"><span data-stu-id="b84bf-155">tooregister your web app extension with hello applicationHost.config file, you need tooplace a file called ApplicationHost.xdt in hello extension root.</span></span> <span data-ttu-id="b84bf-156">hello hello ApplicationHost.xdt fájl tartalmának az alábbinak kell lennie:</span><span class="sxs-lookup"><span data-stu-id="b84bf-156">hello content of hello ApplicationHost.xdt file should be as follows:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.applicationHost>
    <sites>
      <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
        <!-- NOTE: Add your extension name in hello application paths below -->
        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
          <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
        </application>
      </site>
    </sites>
  </system.applicationHost>
</configuration>
```

<span data-ttu-id="b84bf-157">hello nevét jelöli ki a bővítmény neve a bővítmény gyökérmappára hello ugyanazt a nevet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b84bf-157">hello name you select as your extension name should have hello same name as your extension root folder.</span></span>

<span data-ttu-id="b84bf-158">Ennek hatása hello hozzáadása egy új alkalmazás elérési útja toohello `system.applicationHost` hello SCM helyhez tartozik, a helyek listáját.</span><span class="sxs-lookup"><span data-stu-id="b84bf-158">This has hello effect of adding a new application path toohello `system.applicationHost` sites list under hello SCM site.</span></span> <span data-ttu-id="b84bf-159">hello SCM hely adott hozzáférési hitelesítő adatokkal rendelkező hely felügyeleti végpontját.</span><span class="sxs-lookup"><span data-stu-id="b84bf-159">hello SCM site is a site administration end point with specific access credentials.</span></span> <span data-ttu-id="b84bf-160">Hello URL-cím van `https://[your-site-name].scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="b84bf-160">It has hello URL `https://[your-site-name].scm.azurewebsites.net`.</span></span>  

```xml
<system.applicationHost>
  ...       
  <sites>
    <site name="~1[your-website]" id="1716402716">
      <bindings>
        <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
        <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
      </bindings>
      <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
      <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
      <logFile logSiteId="false" />
      <application path="/" applicationPool="[your-website]">
        <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
      </application>
      <!-- Note hello custom changes that go here -->
      <application path="/[your-extension-name]" applicationPool="[your-website]">
        <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
      </application>
    </site>
  </sites>
  ... 
</system.applicationHost>
```

### <span data-ttu-id="b84bf-161"><a id="deploy"></a>A webes alkalmazás bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="b84bf-161"><a id="deploy"></a> Web app extension deployment</span></span>
<span data-ttu-id="b84bf-162">tooinstall a webalkalmazás-bővítmény, FTP toocopy összes hello fájlokat használhatja, a webes alkalmazás toohello `\SiteExtensions\[your-extension-name]` kívánja tooinstall hello bővítmény hello webalkalmazás mappát.</span><span class="sxs-lookup"><span data-stu-id="b84bf-162">tooinstall your web app extension, you can use FTP toocopy all hello files of your web application toohello `\SiteExtensions\[your-extension-name]` folder of hello web app on which you want tooinstall hello extension.</span></span>  <span data-ttu-id="b84bf-163">Lehet, hogy toocopy hello ApplicationHost.xdt fájl toothis helyét is.</span><span class="sxs-lookup"><span data-stu-id="b84bf-163">Be sure toocopy hello ApplicationHost.xdt file toothis location as well.</span></span> <span data-ttu-id="b84bf-164">Indítsa újra a webalkalmazás-tooenable hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="b84bf-164">Restart your web app tooenable hello extension.</span></span>

<span data-ttu-id="b84bf-165">A webalkalmazás-bővítmény, képes toosee kell lennie:</span><span class="sxs-lookup"><span data-stu-id="b84bf-165">You should be able toosee your web app extension at:</span></span>

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

<span data-ttu-id="b84bf-166">Vegye figyelembe, hogy hello URL-cím a következőképpen néz csak hello URL-címet a webalkalmazás, azzal a különbséggel, hogy a HTTPS PROTOKOLLT használ, és a ".scm" tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b84bf-166">Note that hello URL looks just like hello URL for your web app, except that it uses HTTPS and contains ".scm".</span></span>

<span data-ttu-id="b84bf-167">A webalkalmazás-bővítmények lehetséges toodisable összes titkos (nincs előre telepítve) fejlesztési és a vizsgálatok alatt adja hozzá az alkalmazásbeállítások hello kulccsal `WEBSITE_PRIVATE_EXTENSIONS` és érték `0`.</span><span class="sxs-lookup"><span data-stu-id="b84bf-167">It is possible toodisable all private (not pre-installed) extensions for your web app during development and investigations by adding an app settings with hello key `WEBSITE_PRIVATE_EXTENSIONS` and a value of `0`.</span></span>

> [!NOTE]
> <span data-ttu-id="b84bf-168">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="b84bf-168">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="b84bf-169">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="b84bf-169">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="b84bf-170">A változások</span><span class="sxs-lookup"><span data-stu-id="b84bf-170">What's changed</span></span>
* <span data-ttu-id="b84bf-171">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="b84bf-171">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png

