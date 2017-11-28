---
title: "Az Azure App Service web app speciális konfigurálása és bővítményei"
description: "Az Azure App Service web app alkalmazásban az ApplicationHost.config fájl átalakítása és adja hozzá a magánhálózati kiterjesztésének egyéni felügyeleti feladatok engedélyezéséhez a XML-dokumentum Transformation(XDT) nyilatkozatok."
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
ms.openlocfilehash: 314d3a954e712b829e7cf5eb37b23b31670f976b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a><span data-ttu-id="7a806-103">Az Azure App Service web app speciális konfigurálása és bővítményei</span><span class="sxs-lookup"><span data-stu-id="7a806-103">Azure App Service web app advanced config and extensions</span></span>
<span data-ttu-id="7a806-104">A [XML-dokumentum átalakítása](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) nyilatkozatok, alakíthatja át a [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) fájl az Azure App Service szolgáltatásban a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7a806-104">By using [XML Document Transformation](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) declarations, you can transform the [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) file in your web app in Azure App Service.</span></span> <span data-ttu-id="7a806-105">XDT nyilatkozatok segítségével adja hozzá a magánhálózati kiterjesztésének egyéni web app felügyeleti műveletek engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="7a806-105">You can also use XDT declarations to add private extensions to enable custom web app administration actions.</span></span> <span data-ttu-id="7a806-106">A cikk tartalmaz egy mintát PHP Manager webalkalmazás-bővítmény, amely lehetővé teszi a webes felületen keresztül PHP-beállítások kezelését.</span><span class="sxs-lookup"><span data-stu-id="7a806-106">This article includes a sample PHP Manager web app extension that enables management of PHP settings through a web interface.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="7a806-107"><a id="transform"></a>Speciális konfigurációs ApplicationHost.config keresztül</span><span class="sxs-lookup"><span data-stu-id="7a806-107"><a id="transform"></a>Advanced configuration through ApplicationHost.config</span></span>
<span data-ttu-id="7a806-108">Az App Service platform biztosít a rugalmasság és a vezérlés webes alkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7a806-108">The App Service platform provides flexibility and control for web app configuration.</span></span> <span data-ttu-id="7a806-109">Bár a szabványos IIS ApplicationHost.config konfigurációs fájl nincs közvetlen szerkeszthető az App Service-ben, a platform támogatja deklaratív ApplicationHost.config átalakító modellben XML dokumentum átalakítása (XDT).</span><span class="sxs-lookup"><span data-stu-id="7a806-109">Although the standard IIS ApplicationHost.config configuration file is not available for direct editing in App Service, the platform supports a declarative ApplicationHost.config transform model based on XML Document Transformation (XDT).</span></span>

<span data-ttu-id="7a806-110">Kihasználhatják ezt a funkciót átalakító, hozzon létre egy ApplicationHost.xdt fájlt XDT tartalom, és helyezze a webhely gyökeréhez (d:\home\site) alatt a [Kudu konzol](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span><span class="sxs-lookup"><span data-stu-id="7a806-110">To leverage this transform functionality, you create an ApplicationHost.xdt file with XDT content and place under the site root (d:\home\site) in the [Kudu Console](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span></span> <span data-ttu-id="7a806-111">Szükség lehet a Web App módosítások érvénybe lépéséhez indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="7a806-111">You may need to restart the Web App for changes to take effect.</span></span>

<span data-ttu-id="7a806-112">A következő applicationHost.xdt minta bemutatja, hogyan új egyéni környezeti változó hozzáadása a webes alkalmazás, amely használja a PHP 5.4-es.</span><span class="sxs-lookup"><span data-stu-id="7a806-112">The following applicationHost.xdt sample shows how to add a new custom environment variable to a web app that uses PHP 5.4.</span></span>

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

<span data-ttu-id="7a806-113">A naplófájl átalakító állapotával és részleteivel kapcsolatban az FTP gyökérkönyvtárából LogFiles\Transform alatt érhető el.</span><span class="sxs-lookup"><span data-stu-id="7a806-113">A log file with transform status and details is available from the FTP root under LogFiles\Transform.</span></span>

<span data-ttu-id="7a806-114">További példákért lásd: [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).</span><span class="sxs-lookup"><span data-stu-id="7a806-114">For additional samples, see [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).</span></span>

<span data-ttu-id="7a806-115">**Megjegyzés**</span><span class="sxs-lookup"><span data-stu-id="7a806-115">**Note**</span></span><br />
<span data-ttu-id="7a806-116">A modulok listájáról elemek `system.webServer` nem távolítható el, és újra, de kiegészítések az lehetségesek.</span><span class="sxs-lookup"><span data-stu-id="7a806-116">Elements from the list of modules under `system.webServer` cannot be removed or reordered, but additions to the list are possible.</span></span>

## <span data-ttu-id="7a806-117"><a id="extend"></a>A webalkalmazás kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="7a806-117"><a id="extend"></a> Extend your web app</span></span>
### <span data-ttu-id="7a806-118"><a id="overview"></a>Saját webalkalmazás-bővítmény áttekintése</span><span class="sxs-lookup"><span data-stu-id="7a806-118"><a id="overview"></a> Overview of private web app extensions</span></span>
<span data-ttu-id="7a806-119">App Service webalkalmazás-bővítmény támogatja a felügyeleti műveletekhez bővítési pontjaként.</span><span class="sxs-lookup"><span data-stu-id="7a806-119">App Service supports web app extensions as an extensibility point for administrative actions.</span></span> <span data-ttu-id="7a806-120">Tulajdonképpen egy App Service platform funkciói előre telepített kiterjesztéseket valósíthatók meg.</span><span class="sxs-lookup"><span data-stu-id="7a806-120">In fact, some App Service platform features are implemented as pre-installed extensions.</span></span> <span data-ttu-id="7a806-121">Az előre telepített platform bővítmények nem módosítható, amíg hozhat létre és magánhálózati kiterjesztésének konfigurálása a saját webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7a806-121">While the pre-installed platform extensions cannot be modified, you can create and configure private extensions for your own web app.</span></span> <span data-ttu-id="7a806-122">Ez a funkció is XDT nyilatkozatok támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="7a806-122">This functionality also relies on XDT declarations.</span></span> <span data-ttu-id="7a806-123">A kulcs egy saját webalkalmazás-bővítmény létrehozásának lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="7a806-123">The key steps for creating a private web app extension are the following:</span></span>

1. <span data-ttu-id="7a806-124">Webalkalmazás-bővítmény **tartalom**: bármely támogatja az App Service webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a806-124">Web app extension **content**: create any web application supported by App Service</span></span>
2. <span data-ttu-id="7a806-125">Webalkalmazás-bővítmény **deklaráció**: ApplicationHost.xdt-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a806-125">Web app extension **declaration**: create an ApplicationHost.xdt file</span></span>
3. <span data-ttu-id="7a806-126">Webalkalmazás-bővítmény **telepítési**: a SiteExtensions mappában található tartalom elhelyezése`root`</span><span class="sxs-lookup"><span data-stu-id="7a806-126">Web app extension **deployment**: place content in the SiteExtensions folder under `root`</span></span>

<span data-ttu-id="7a806-127">A webes alkalmazás belső hivatkozásokat az alkalmazás elérési útja a ApplicationHost.xdt fájl megadott relatív elérési kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="7a806-127">Internal links for the web app should point to a path relative to the application path specified in the ApplicationHost.xdt file.</span></span> <span data-ttu-id="7a806-128">Bármi is módosul az ApplicationHost.xdt fájl web app újrahasznosítást igényel.</span><span class="sxs-lookup"><span data-stu-id="7a806-128">Any change to the ApplicationHost.xdt file requires a web app recycle.</span></span>

<span data-ttu-id="7a806-129">**Megjegyzés:**: további információt a fő elemei a [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).</span><span class="sxs-lookup"><span data-stu-id="7a806-129">**Note**: Additional information for these key elements is available at [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).</span></span>

<span data-ttu-id="7a806-130">A részletes része létrehozásának és egy személyes webalkalmazás-bővítmény engedélyezése a lépéseit mutatja be.</span><span class="sxs-lookup"><span data-stu-id="7a806-130">A detailed example is included to illustrate the steps for creating and enabling a private web app extension.</span></span> <span data-ttu-id="7a806-131">A PHP Manager példa, amely a következő forráskódja letölthető [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).</span><span class="sxs-lookup"><span data-stu-id="7a806-131">The source code for the PHP Manager example that follows can be downloaded from [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).</span></span>

### <span data-ttu-id="7a806-132"><a id="SiteSample"></a>Webes alkalmazás bővítmény példa: PHP-kezelő</span><span class="sxs-lookup"><span data-stu-id="7a806-132"><a id="SiteSample"></a> Web app extension example: PHP Manager</span></span>
<span data-ttu-id="7a806-133">PHP-kezelő egy webalkalmazás-bővítmény, amely lehetővé teszi, hogy a webes alkalmazás rendszergazdák könnyen beállításainak megtekintése és konfigurálása a PHP webes felületen keresztül közvetlenül módosítja a PHP .ini fájlokat ahelyett, hogy.</span><span class="sxs-lookup"><span data-stu-id="7a806-133">PHP Manager is a web app extension that allows web app administrators to easily view and configure their PHP settings using a web interface instead of having to modify PHP .ini files directly.</span></span> <span data-ttu-id="7a806-134">Php-hez tartozó általános konfigurációs fájljai közé a php.ini fájlt a Program Files alatt és a. a webes alkalmazás gyökérkönyvtárában található user.ini fájlt.</span><span class="sxs-lookup"><span data-stu-id="7a806-134">Common configuration files for PHP include the php.ini file located under Program Files and the .user.ini file located in the root folder of your web app.</span></span> <span data-ttu-id="7a806-135">A php.ini fájl nincs az App Service platformon közvetlenül szerkeszthető, mert a PHP-kezelő bővítmény használja a. user.ini fájl beállítás módosítások alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="7a806-135">Since the php.ini file is not directly editable on the App Service platform, the PHP Manager extension uses the .user.ini file to apply setting changes.</span></span>

#### <span data-ttu-id="7a806-136"><a id="PHPwebapp"></a>A PHP-kezelő webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="7a806-136"><a id="PHPwebapp"></a> The PHP Manager web application</span></span>
<span data-ttu-id="7a806-137">Az alábbiakban áttekintjük a kezdőlap a PHP-kezelő központi telepítés:</span><span class="sxs-lookup"><span data-stu-id="7a806-137">The following is the home page of the PHP Manager deployment:</span></span>

![TransformSitePHPUI][TransformSitePHPUI]

<span data-ttu-id="7a806-139">Ahogy látja, a webalkalmazás-bővítmény van, hasonlóan egy rendszeres webalkalmazást, de egy további ApplicationHost.xdt fájl a gyökérmappában található azon a web app (további információt a ApplicationHost.xdt fájl érhetők el a cikk a következő szakaszban) helyezni.</span><span class="sxs-lookup"><span data-stu-id="7a806-139">As you can see, a web app extension is just like a regular web application, but with an additional ApplicationHost.xdt file placed in the root folder of the web app (more details about the ApplicationHost.xdt file are available in the next section of this article).</span></span>

<span data-ttu-id="7a806-140">A PHP-kezelő bővítmény létrejött, a Visual Studio ASP.NET MVC 4 webalkalmazás sablonja segítségével.</span><span class="sxs-lookup"><span data-stu-id="7a806-140">The PHP Manager extension was created using the Visual Studio ASP.NET MVC 4 Web Application template.</span></span> <span data-ttu-id="7a806-141">A következő nézetet a Megoldáskezelőből a PHP-kezelő bővítmény szerkezetét mutatja.</span><span class="sxs-lookup"><span data-stu-id="7a806-141">The following view from Solution Explorer shows the structure of the PHP Manager extension.</span></span>

![TransformSiteSolEx][TransformSiteSolEx]

<span data-ttu-id="7a806-143">A szükséges i/o-fájl csak speciális logikát annak jelzésére, hogy hol helyezkedik el a wwwroot könyvtárat a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7a806-143">The only special logic needed for file I/O is to indicate where the wwwroot directory of the web app is located.</span></span> <span data-ttu-id="7a806-144">Az alábbi példakód mutatja, a környezeti változó "OTTHONI" jelzi a webes alkalmazás gyökérútvonalát, és a wwwroot elérési út lehet létrehozni "site\wwwroot" hozzáfűzésével:</span><span class="sxs-lookup"><span data-stu-id="7a806-144">As the following code example shows, the environment variable "HOME" indicates the web app's root path, and the wwwroot path can be constructed by appending "site\wwwroot":</span></span>

```csharp
/// <summary>
/// Gives the location of the .user.ini file, even if one doesn't exist yet
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


<span data-ttu-id="7a806-145">Miután a könyvtár elérési útja, rendszeres fájl i/o-műveletek segítségével olvasási és írási fájlokat.</span><span class="sxs-lookup"><span data-stu-id="7a806-145">After you have the directory path, you can use regular file I/O operations to read and write to files.</span></span>

<span data-ttu-id="7a806-146">Egy pont a webalkalmazás-bővítmény járjon el a belső hivatkozások kezelésének tekintetében.</span><span class="sxs-lookup"><span data-stu-id="7a806-146">One point of caution with web app extensions regards the handling of internal links.</span></span>  <span data-ttu-id="7a806-147">Ha esetleges hivatkozások a HTML-fájlok, amelyek biztosítják a webes alkalmazás belső mutató hivatkozások abszolút elérési utakat is van, bizonyosodjon meg ezeket a hivatkozásokat, a bővítmény neve, a legfelső szintű vannak $a.</span><span class="sxs-lookup"><span data-stu-id="7a806-147">If you have any links in your HTML files that give absolute paths to internal links on your web app, you must ensure those links are prepended with your extension name as your root.</span></span> <span data-ttu-id="7a806-148">Ez szükséges, mert a bővítmény a legfelső szintű most "/`[your-extension-name]`/" nem csak "/", ezért minden belső hivatkozások frissíteni kell ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="7a806-148">This is needed because the root for your extension is now "/`[your-extension-name]`/" rather than being just "/", so any internal links must be updated accordingly.</span></span> <span data-ttu-id="7a806-149">Tegyük fel, hogy a kód a következő mutató hivatkozást tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="7a806-149">For example, suppose your code includes a link to the following:</span></span>

`"<a href="/Home/Settings">PHP Settings</a>"`

<span data-ttu-id="7a806-150">Ha a kapcsolat a webalkalmazás-bővítmény része, a hivatkozás a következő formátumban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="7a806-150">When the link is part of a web app extension, the link must be in the following form:</span></span>

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

<span data-ttu-id="7a806-151">Ennek a követelménynek úgy vagy kerülheti használatával csak relatív elérési utakat, a webes alkalmazás belül, illetve az ASP.NET alkalmazásokat, használja a `@Html.ActionLink` metódus, amely létrehozza a megfelelő hivatkozásokra.</span><span class="sxs-lookup"><span data-stu-id="7a806-151">You can work around this requirement by either using only relative paths within your web application, or in the case of ASP.NET applications, by using the `@Html.ActionLink` method which creates the appropriate links for you.</span></span>

#### <span data-ttu-id="7a806-152"><a id="XDT"></a>A applicationHost.xdt fájl</span><span class="sxs-lookup"><span data-stu-id="7a806-152"><a id="XDT"></a> The applicationHost.xdt file</span></span>
<span data-ttu-id="7a806-153">A webalkalmazás-bővítmény a kódját a %HOME%\SiteExtensions kerül\[a bővítmény neve].</span><span class="sxs-lookup"><span data-stu-id="7a806-153">The code for your web app extension goes under %HOME%\SiteExtensions\[your-extension-name].</span></span> <span data-ttu-id="7a806-154">Felhívjuk ezt a bővítményt legfelső szintű.</span><span class="sxs-lookup"><span data-stu-id="7a806-154">We'll call this the extension root.</span></span>  

<span data-ttu-id="7a806-155">A webalkalmazás-bővítmény regisztrálására az applicationHost.config fájl, szüksége helyezhető el a kiterjesztés legfelső szintű ApplicationHost.xdt nevű fájl.</span><span class="sxs-lookup"><span data-stu-id="7a806-155">To register your web app extension with the applicationHost.config file, you need to place a file called ApplicationHost.xdt in the extension root.</span></span> <span data-ttu-id="7a806-156">A ApplicationHost.xdt fájl tartalma a következő legyen:</span><span class="sxs-lookup"><span data-stu-id="7a806-156">The content of the ApplicationHost.xdt file should be as follows:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.applicationHost>
    <sites>
      <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
        <!-- NOTE: Add your extension name in the application paths below -->
        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
          <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
        </application>
      </site>
    </sites>
  </system.applicationHost>
</configuration>
```

<span data-ttu-id="7a806-157">A nevét jelöli ki a bővítmény neve a bővítmény gyökérmappában található azonos nevű kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7a806-157">The name you select as your extension name should have the same name as your extension root folder.</span></span>

<span data-ttu-id="7a806-158">Ennek a hatása hozzáadása egy új alkalmazás elérési útját a `system.applicationHost` az SCM helyhez tartozik, a helyek listáját.</span><span class="sxs-lookup"><span data-stu-id="7a806-158">This has the effect of adding a new application path to the `system.applicationHost` sites list under the SCM site.</span></span> <span data-ttu-id="7a806-159">Az SCM hely adott hozzáférési hitelesítő adatokkal rendelkező hely felügyeleti végpont.</span><span class="sxs-lookup"><span data-stu-id="7a806-159">The SCM site is a site administration end point with specific access credentials.</span></span> <span data-ttu-id="7a806-160">Az URL-cím van `https://[your-site-name].scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="7a806-160">It has the URL `https://[your-site-name].scm.azurewebsites.net`.</span></span>  

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
      <!-- Note the custom changes that go here -->
      <application path="/[your-extension-name]" applicationPool="[your-website]">
        <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
      </application>
    </site>
  </sites>
  ... 
</system.applicationHost>
```

### <span data-ttu-id="7a806-161"><a id="deploy"></a>A webes alkalmazás bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="7a806-161"><a id="deploy"></a> Web app extension deployment</span></span>
<span data-ttu-id="7a806-162">A webalkalmazás-bővítmény telepítése, az FTP használatával a webalkalmazás számára a fájlok másolása a `\SiteExtensions\[your-extension-name]` mappa a webes alkalmazás, amelyre szeretné, a bővítmény telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7a806-162">To install your web app extension, you can use FTP to copy all the files of your web application to the `\SiteExtensions\[your-extension-name]` folder of the web app on which you want to install the extension.</span></span>  <span data-ttu-id="7a806-163">Győződjön meg arról, a ApplicationHost.xdt fájl másolása ezen a helyen.</span><span class="sxs-lookup"><span data-stu-id="7a806-163">Be sure to copy the ApplicationHost.xdt file to this location as well.</span></span> <span data-ttu-id="7a806-164">Engedélyezi a bővítményt a webalkalmazás újraindítása.</span><span class="sxs-lookup"><span data-stu-id="7a806-164">Restart your web app to enable the extension.</span></span>

<span data-ttu-id="7a806-165">A webalkalmazás-bővítmény, látni kell:</span><span class="sxs-lookup"><span data-stu-id="7a806-165">You should be able to see your web app extension at:</span></span>

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

<span data-ttu-id="7a806-166">Vegye figyelembe, hogy az URL-cím a következőképpen néz csak az URL-címet a webalkalmazás, azzal a különbséggel, hogy HTTPS PROTOKOLLT használ, és tartalmazza a ".scm".</span><span class="sxs-lookup"><span data-stu-id="7a806-166">Note that the URL looks just like the URL for your web app, except that it uses HTTPS and contains ".scm".</span></span>

<span data-ttu-id="7a806-167">A webalkalmazás összes titkos (nem előre telepített) bővítmények letiltása fejlesztési és vizsgálatok során a kulccsal az alkalmazásbeállítások hozzáadásával lehetséges `WEBSITE_PRIVATE_EXTENSIONS` és érték `0`.</span><span class="sxs-lookup"><span data-stu-id="7a806-167">It is possible to disable all private (not pre-installed) extensions for your web app during development and investigations by adding an app settings with the key `WEBSITE_PRIVATE_EXTENSIONS` and a value of `0`.</span></span>

> [!NOTE]
> <span data-ttu-id="7a806-168">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="7a806-168">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="7a806-169">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="7a806-169">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="7a806-170">A változások</span><span class="sxs-lookup"><span data-stu-id="7a806-170">What's changed</span></span>
* <span data-ttu-id="7a806-171">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="7a806-171">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png

