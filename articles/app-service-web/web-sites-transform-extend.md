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
# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Az Azure App Service web app speciális konfigurálása és bővítményei
A [XML-dokumentum átalakítása](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) nyilatkozatok, alakíthatja át a [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) fájl az Azure App Service szolgáltatásban a webalkalmazásban. XDT nyilatkozatok segítségével adja hozzá a magánhálózati kiterjesztésének egyéni web app felügyeleti műveletek engedélyezéséhez. A cikk tartalmaz egy mintát PHP Manager webalkalmazás-bővítmény, amely lehetővé teszi a webes felületen keresztül PHP-beállítások kezelését.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a id="transform"></a>Speciális konfigurációs ApplicationHost.config keresztül
Az App Service platform biztosít a rugalmasság és a vezérlés webes alkalmazások konfigurálása. Bár a szabványos IIS ApplicationHost.config konfigurációs fájl nincs közvetlen szerkeszthető az App Service-ben, a platform támogatja deklaratív ApplicationHost.config átalakító modellben XML dokumentum átalakítása (XDT).

Kihasználhatják ezt a funkciót átalakító, hozzon létre egy ApplicationHost.xdt fájlt XDT tartalom, és helyezze a webhely gyökeréhez (d:\home\site) alatt a [Kudu konzol](https://github.com/projectkudu/kudu/wiki/Kudu-console). Szükség lehet a Web App módosítások érvénybe lépéséhez indítsa újra.

A következő applicationHost.xdt minta bemutatja, hogyan új egyéni környezeti változó hozzáadása a webes alkalmazás, amely használja a PHP 5.4-es.

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

A naplófájl átalakító állapotával és részleteivel kapcsolatban az FTP gyökérkönyvtárából LogFiles\Transform alatt érhető el.

További példákért lásd: [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Megjegyzés**<br />
A modulok listájáról elemek `system.webServer` nem távolítható el, és újra, de kiegészítések az lehetségesek.

## <a id="extend"></a>A webalkalmazás kiterjesztése
### <a id="overview"></a>Saját webalkalmazás-bővítmény áttekintése
App Service webalkalmazás-bővítmény támogatja a felügyeleti műveletekhez bővítési pontjaként. Tulajdonképpen egy App Service platform funkciói előre telepített kiterjesztéseket valósíthatók meg. Az előre telepített platform bővítmények nem módosítható, amíg hozhat létre és magánhálózati kiterjesztésének konfigurálása a saját webes alkalmazást. Ez a funkció is XDT nyilatkozatok támaszkodik. A kulcs egy saját webalkalmazás-bővítmény létrehozásának lépései a következők:

1. Webalkalmazás-bővítmény **tartalom**: bármely támogatja az App Service webalkalmazás létrehozása
2. Webalkalmazás-bővítmény **deklaráció**: ApplicationHost.xdt-fájl létrehozása
3. Webalkalmazás-bővítmény **telepítési**: a SiteExtensions mappában található tartalom elhelyezése`root`

A webes alkalmazás belső hivatkozásokat az alkalmazás elérési útja a ApplicationHost.xdt fájl megadott relatív elérési kell mutatnia. Bármi is módosul az ApplicationHost.xdt fájl web app újrahasznosítást igényel.

**Megjegyzés:**: további információt a fő elemei a [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

A részletes része létrehozásának és egy személyes webalkalmazás-bővítmény engedélyezése a lépéseit mutatja be. A PHP Manager példa, amely a következő forráskódja letölthető [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

### <a id="SiteSample"></a>Webes alkalmazás bővítmény példa: PHP-kezelő
PHP-kezelő egy webalkalmazás-bővítmény, amely lehetővé teszi, hogy a webes alkalmazás rendszergazdák könnyen beállításainak megtekintése és konfigurálása a PHP webes felületen keresztül közvetlenül módosítja a PHP .ini fájlokat ahelyett, hogy. Php-hez tartozó általános konfigurációs fájljai közé a php.ini fájlt a Program Files alatt és a. a webes alkalmazás gyökérkönyvtárában található user.ini fájlt. A php.ini fájl nincs az App Service platformon közvetlenül szerkeszthető, mert a PHP-kezelő bővítmény használja a. user.ini fájl beállítás módosítások alkalmazásához.

#### <a id="PHPwebapp"></a>A PHP-kezelő webalkalmazás
Az alábbiakban áttekintjük a kezdőlap a PHP-kezelő központi telepítés:

![TransformSitePHPUI][TransformSitePHPUI]

Ahogy látja, a webalkalmazás-bővítmény van, hasonlóan egy rendszeres webalkalmazást, de egy további ApplicationHost.xdt fájl a gyökérmappában található azon a web app (további információt a ApplicationHost.xdt fájl érhetők el a cikk a következő szakaszban) helyezni.

A PHP-kezelő bővítmény létrejött, a Visual Studio ASP.NET MVC 4 webalkalmazás sablonja segítségével. A következő nézetet a Megoldáskezelőből a PHP-kezelő bővítmény szerkezetét mutatja.

![TransformSiteSolEx][TransformSiteSolEx]

A szükséges i/o-fájl csak speciális logikát annak jelzésére, hogy hol helyezkedik el a wwwroot könyvtárat a webalkalmazás. Az alábbi példakód mutatja, a környezeti változó "OTTHONI" jelzi a webes alkalmazás gyökérútvonalát, és a wwwroot elérési út lehet létrehozni "site\wwwroot" hozzáfűzésével:

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


Miután a könyvtár elérési útja, rendszeres fájl i/o-műveletek segítségével olvasási és írási fájlokat.

Egy pont a webalkalmazás-bővítmény járjon el a belső hivatkozások kezelésének tekintetében.  Ha esetleges hivatkozások a HTML-fájlok, amelyek biztosítják a webes alkalmazás belső mutató hivatkozások abszolút elérési utakat is van, bizonyosodjon meg ezeket a hivatkozásokat, a bővítmény neve, a legfelső szintű vannak $a. Ez szükséges, mert a bővítmény a legfelső szintű most "/`[your-extension-name]`/" nem csak "/", ezért minden belső hivatkozások frissíteni kell ennek megfelelően. Tegyük fel, hogy a kód a következő mutató hivatkozást tartalmaz:

`"<a href="/Home/Settings">PHP Settings</a>"`

Ha a kapcsolat a webalkalmazás-bővítmény része, a hivatkozás a következő formátumban kell lennie:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Ennek a követelménynek úgy vagy kerülheti használatával csak relatív elérési utakat, a webes alkalmazás belül, illetve az ASP.NET alkalmazásokat, használja a `@Html.ActionLink` metódus, amely létrehozza a megfelelő hivatkozásokra.

#### <a id="XDT"></a>A applicationHost.xdt fájl
A webalkalmazás-bővítmény a kódját a %HOME%\SiteExtensions kerül\[a bővítmény neve]. Felhívjuk ezt a bővítményt legfelső szintű.  

A webalkalmazás-bővítmény regisztrálására az applicationHost.config fájl, szüksége helyezhető el a kiterjesztés legfelső szintű ApplicationHost.xdt nevű fájl. A ApplicationHost.xdt fájl tartalma a következő legyen:

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

A nevét jelöli ki a bővítmény neve a bővítmény gyökérmappában található azonos nevű kell lennie.

Ennek a hatása hozzáadása egy új alkalmazás elérési útját a `system.applicationHost` az SCM helyhez tartozik, a helyek listáját. Az SCM hely adott hozzáférési hitelesítő adatokkal rendelkező hely felügyeleti végpont. Az URL-cím van `https://[your-site-name].scm.azurewebsites.net`.  

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

### <a id="deploy"></a>A webes alkalmazás bővítmény telepítése
A webalkalmazás-bővítmény telepítése, az FTP használatával a webalkalmazás számára a fájlok másolása a `\SiteExtensions\[your-extension-name]` mappa a webes alkalmazás, amelyre szeretné, a bővítmény telepítéséhez.  Győződjön meg arról, a ApplicationHost.xdt fájl másolása ezen a helyen. Engedélyezi a bővítményt a webalkalmazás újraindítása.

A webalkalmazás-bővítmény, látni kell:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Vegye figyelembe, hogy az URL-cím a következőképpen néz csak az URL-címet a webalkalmazás, azzal a különbséggel, hogy HTTPS PROTOKOLLT használ, és tartalmazza a ".scm".

A webalkalmazás összes titkos (nem előre telepített) bővítmények letiltása fejlesztési és vizsgálatok során a kulccsal az alkalmazásbeállítások hozzáadásával lehetséges `WEBSITE_PRIVATE_EXTENSIONS` és érték `0`.

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png

