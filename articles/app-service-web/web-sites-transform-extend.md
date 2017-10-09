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
# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Az Azure App Service web app speciális konfigurálása és bővítményei
A [XML-dokumentum átalakítása](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) nyilatkozatok, hello alakíthatja át [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) fájl az Azure App Service szolgáltatásban a webalkalmazásban. XDT nyilatkozatok tooadd magánhálózati kiterjesztésének tooenable egyéni web app felügyeleti műveletek is használható. A cikk tartalmaz egy mintát PHP Manager webalkalmazás-bővítmény, amely lehetővé teszi a webes felületen keresztül PHP-beállítások kezelését.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a id="transform"></a>Speciális konfigurációs ApplicationHost.config keresztül
App Service platform hello rugalmasságot és biztosít webes alkalmazások konfigurálása. Bár az App Service közvetlen szerkesztésre hello szabványos IIS ApplicationHost.config konfigurációs fájl nem érhető el, hello platformja támogatja a deklaratív ApplicationHost.config átalakító modellben XML dokumentum átalakítása (XDT).

tooleverage ezt átalakítás a funkciót, akkor hozzon létre egy ApplicationHost.xdt fájlt XDT tartalom és hello webhely gyökeréhez (d:\home\site) a hello alá helyezni [Kudu konzol](https://github.com/projectkudu/kudu/wiki/Kudu-console). Módosítások tootake hatás toorestart hello webalkalmazás szükséges.

a következő applicationHost.xdt minta hello jeleníti meg, hogyan tooadd egy új egyéni környezeti változó tooa webalkalmazás, amely használja a PHP 5.4-es.

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

A naplófájl átalakító állapotával és részleteivel kapcsolatban hello FTP gyökérkönyvtárából LogFiles\Transform alatt érhető el.

További példákért lásd: [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Megjegyzés**<br />
Hello listájához a elemek `system.webServer` nem távolítható el, és újra, de kiegészítéseket toohello lista is előfordulhatnak.

## <a id="extend"></a>A webalkalmazás kiterjesztése
### <a id="overview"></a>Saját webalkalmazás-bővítmény áttekintése
App Service webalkalmazás-bővítmény támogatja a felügyeleti műveletekhez bővítési pontjaként. Tulajdonképpen egy App Service platform funkciói előre telepített kiterjesztéseket valósíthatók meg. Hello előre telepített platform bővítményei nem módosíthatók, amíg hozhat létre és magánhálózati kiterjesztésének konfigurálása a saját webes alkalmazást. Ez a funkció is XDT nyilatkozatok támaszkodik. hello kulcs egy saját webalkalmazás-bővítmény létrehozásának lépései a következők hello következő:

1. Webalkalmazás-bővítmény **tartalom**: bármely támogatja az App Service webalkalmazás létrehozása
2. Webalkalmazás-bővítmény **deklaráció**: ApplicationHost.xdt-fájl létrehozása
3. Webalkalmazás-bővítmény **telepítési**: hello SiteExtensions mappában található a tartalom elhelyezése`root`

Belső hivatkozások hello webalkalmazás tooa elérési útja relatív toohello alkalmazás elérési útja hello ApplicationHost.xdt fájlban megadott kell mutatnia. Minden olyan változás toohello ApplicationHost.xdt fájlt web app újrahasznosítást igényel.

**Megjegyzés:**: további információt a fő elemei a [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

A részletes belefoglalt tooillustrate hello lépései létrehozásával és engedélyezésével a saját webalkalmazás-bővítmény. például hello PHP kezelő, amely a következőképpen letölthető a forráskód hello [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

### <a id="SiteSample"></a>Webes alkalmazás bővítmény példa: PHP-kezelő
PHP-kezelő rendszer egy webalkalmazás-bővítmény, amely lehetővé teszi, hogy a webes alkalmazás rendszergazdák tooeasily nézet, és a webes felületen keresztül közvetlenül toomodify PHP .ini-fájlokat ahelyett PHP-beállítások konfigurálása. Php-hez tartozó általános konfigurációs fájljai közé hello php.ini fájlt a Program Files és hello található. a webalkalmazás gyökérmappájában hello user.ini fájlba. Mivel hello php.ini fájl nem közvetlenül szerkeszthetők a hello App Service platform, a PHP-kezelő bővítmény hello használja-e a hello. user.ini fájl tooapply változás.

#### <a id="PHPwebapp"></a>hello PHP Manager webalkalmazás
hello az alábbiakban látható hello kezdőlapján hello PHP Manager telepítése:

![TransformSitePHPUI][TransformSitePHPUI]

Ahogy látja, a webalkalmazás-bővítmény hasonlóan rendszeres webalkalmazást, de egy további ApplicationHost.xdt fájl hello gyökérmappájában hello web app (hello ApplicationHost.xdt fájllal kapcsolatos további részletekért érhetők el az e hello a következő szakaszban van-e a cikk).

PHP-kezelő bővítmény hello hello Visual Studio ASP.NET MVC 4 webalkalmazás sablon használatával lett létrehozva. hello következő nézet megoldáskezelőjében jeleníti meg a PHP-kezelő bővítmény hello hello szerkezete.

![TransformSiteSolEx][TransformSiteSolEx]

hello szükséges i/o-fájl csak speciális logikát tooindicate ahol hello wwwroot könyvtárat hello webalkalmazás. Módon hello következő példát mutat be, hello környezeti változó "OTTHONI" hello webes alkalmazás elérési útjának gyökeréhez és hello wwwroot elérési út lehet létrehozni "site\wwwroot" hozzáfűzésével jelzi:

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


Miután hello könyvtár elérési útja, használhatja a rendszeres fájl i/o-műveletek tooread és toofiles írni.

Egy pont a figyelmeztetés a webalkalmazás-bővítmény hello kezelésére vonatkozó belső hivatkozások tekintetében.  Ha a HTML-fájlok, amelyek a webalkalmazásban toointernal hivatkozások abszolút elérési utakat biztosítanak a hivatkozásokat, győződjön meg ezeket a hivatkozásokat, a bővítmény neve, a legfelső szintű vannak $a. Ez van szükség, mert hello legfelső szintű a bővítmény jelenleg "/`[your-extension-name]`/" nem csak "/", ezért minden belső hivatkozások frissíteni kell ennek megfelelően. Tegyük fel, hogy a kód egy hivatkozás toohello következőket tartalmazza:

`"<a href="/Home/Settings">PHP Settings</a>"`

Ha hello hivatkozás egy webalkalmazás-bővítmény része, hello hivatkozás kell a következő képernyő hello:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Ez a követelmény csak relatív elérési utakat a webes alkalmazás belül, vagy a kis-és ASP.NET-alkalmazások hello hello segítségével segítségével úgy kerülheti `@Html.ActionLink` hello megfelelő hivatkozásokat hoz meg metódust.

#### <a id="XDT"></a>hello applicationHost.xdt fájl
a webalkalmazás-bővítmény hello kódját kerül a %HOME%\SiteExtensions\[a bővítmény neve]. Felhívjuk a hello bővítmény gyökér.  

tooregister a webalkalmazás-bővítmény hello applicationHost.config fájlt, kell tooplace hello bővítmény legfelső szintű ApplicationHost.xdt nevű fájl. hello hello ApplicationHost.xdt fájl tartalmának az alábbinak kell lennie:

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

hello nevét jelöli ki a bővítmény neve a bővítmény gyökérmappára hello ugyanazt a nevet kell lennie.

Ennek hatása hello hozzáadása egy új alkalmazás elérési útja toohello `system.applicationHost` hello SCM helyhez tartozik, a helyek listáját. hello SCM hely adott hozzáférési hitelesítő adatokkal rendelkező hely felügyeleti végpontját. Hello URL-cím van `https://[your-site-name].scm.azurewebsites.net`.  

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

### <a id="deploy"></a>A webes alkalmazás bővítmény telepítése
tooinstall a webalkalmazás-bővítmény, FTP toocopy összes hello fájlokat használhatja, a webes alkalmazás toohello `\SiteExtensions\[your-extension-name]` kívánja tooinstall hello bővítmény hello webalkalmazás mappát.  Lehet, hogy toocopy hello ApplicationHost.xdt fájl toothis helyét is. Indítsa újra a webalkalmazás-tooenable hello bővítmény.

A webalkalmazás-bővítmény, képes toosee kell lennie:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Vegye figyelembe, hogy hello URL-cím a következőképpen néz csak hello URL-címet a webalkalmazás, azzal a különbséggel, hogy a HTTPS PROTOKOLLT használ, és a ".scm" tartalmazza.

A webalkalmazás-bővítmények lehetséges toodisable összes titkos (nincs előre telepítve) fejlesztési és a vizsgálatok alatt adja hozzá az alkalmazásbeállítások hello kulccsal `WEBSITE_PRIVATE_EXTENSIONS` és érték `0`.

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png

