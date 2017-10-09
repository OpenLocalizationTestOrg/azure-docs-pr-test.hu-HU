---
title: "aaaBind egy meglévő egyéni SSL-tanúsítvány tooAzure webalkalmazások |} Microsoft Docs"
description: "Ismerje meg tootoobind, egy egyéni SSL tanúsítvány tooyour webalkalmazás, mobil-háttéralkalmazás vagy az Azure App Service API-alkalmazásba."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 3503ba9f96c8ea8d18451e8bf9a9b441797ef44d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-tooazure-web-apps"></a>Egy meglévő egyéni SSL tanúsítvány tooAzure webalkalmazások kötése

Az Azure Web Apps jól skálázható, önálló javítási a webhelyszolgáltató biztosít. Az oktatóanyag bemutatja, hogyan toobind egyéni SSL tanúsítvány, amikor egy megbízható hitelesítésszolgáltatótól származó túl vásárolt[Azure Web Apps](app-service-web-overview.md). Amikor végzett, be fog tudni tooaccess: a webalkalmazás HTTPS-végpont hello az egyéni DNS-tartományt.

![Egyéni SSL-tanúsítvánnyal webalkalmazás](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Frissítse az alkalmazás tarifacsomag kiválasztása
> * Az egyéni SSL-tanúsítvány tooApp szolgáltatás kötése
> * Az alkalmazás HTTPS kényszerítése
> * SSL-tanúsítvány kötés parancsfájlokkal automatizálhatja

> [!NOTE]
> Tooget egy egyéni SSL-tanúsítványra van szükség, ha közvetlenül lekérni egy-egy hello Azure-portálon, és kösse tooyour webalkalmazás. Hajtsa végre a hello [App Service-tanúsítványokkal oktatóanyag](web-sites-purchase-ssl-web-site.md).

## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

- [Az App Service-alkalmazás létrehozása](/azure/app-service/)
- [Egy egyéni DNS nevét tooyour webalkalmazás leképezése](app-service-web-tutorial-custom-domain.md)
- Szerezzen be egy megbízható hitelesítésszolgáltatótól származó SSL-tanúsítvány

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>Az SSL-tanúsítványra vonatkozó követelményekről

toouse egy tanúsítványt az App Service-ben, hello tanúsítvány összes hello követelményeknek kell megfelelniük:

* Megbízható hitelesítésszolgáltató által aláírt
* Jelszóval védett PFX-fájlba exportált
* Titkos kulcs legalább 2048 bit hosszúságú tartalmazza
* Hello tanúsítványláncában szereplő összes köztes tanúsítvány tartalmazza

> [!NOTE]
> **Elliptikus görbe alapú titkosítást (ECC) tanúsítványok** App Service képes együttműködni, de ez a cikk nem vonatkozik. A hello szövegéről toocreate ECC tanúsítványokat a hitelesítésszolgáltató működik.

## <a name="prepare-your-web-app"></a>A webes alkalmazás előkészítése

toobind egyéni SSL-tanúsítvány tooyour web app alkalmazásban a [App Service-csomag](https://azure.microsoft.com/pricing/details/app-service/) hello kell **alapvető**, **szabványos**, vagy **prémium** réteg. Ezt a lépést akkor győződjön meg arról, hogy a webalkalmazás a hello támogatott IP-címek.

### <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Nyissa meg hello [Azure-portálon](https://portal.azure.com).

### <a name="navigate-tooyour-web-app"></a>Keresse meg a tooyour webalkalmazás

Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson a webalkalmazás hello nevét.

![Webalkalmazás kiválasztása](./media/app-service-web-tutorial-custom-ssl/select-app.png)

Hello kezelése lapon webalkalmazás rendelkezik ki.  

### <a name="check-hello-pricing-tier"></a>IP-címek hello ellenőrzése

A weblap app hello bal oldali navigációs, görgessen toohello **beállítások** válassza ki azt **vertikális felskálázás (App Service-csomag)**.

![Méretezett menü](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

Ellenőrizze, hogy a webalkalmazás nem hello toomake **szabad** vagy **megosztott** réteg. A webalkalmazás jelenlegi rétegtől ki van jelölve, egy kékkel által.

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

Egyéni SSL nem támogatja a hello **szabad** vagy **megosztott** réteg. Ha be kell tooscale, kövesse hello hello következő szakaszban található lépéseket. Ellenkező esetben zárja be a hello **válasszon tarifacsomagot** lapon, és hagyja ki túl[töltse fel, és az SSL-tanúsítvány kötését](#upload).

### <a name="scale-up-your-app-service-plan"></a>Vertikális felskálázás App Service-csomag

Válasszon ki egy hello **alapvető**, **szabványos**, vagy **prémium** rétegek.

Kattintson a **Kiválasztás** gombra.

![Tarifacsomag kiválasztása](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

Amikor megjelenik a következő értesítési hello, hello skálázási művelet be nem fejeződött.

![Vertikális felskálázás értesítés](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>Az SSL-tanúsítvány kötése

Akkor van kész tooupload az SSL-tanúsítvány tooyour webalkalmazás.

### <a name="merge-intermediate-certificates"></a>Köztes tanúsítványok egyesítése

Ha a hitelesítésszolgáltató lehetővé teszi több tanúsítvány hello tanúsítványlánc, toomerge hello tanúsítványok sorrendben kell. 

toodo minden nyitott Ez a tanúsítvány is egy szövegszerkesztőben. 

Hello egyesített tanúsítvány nevű fájl létrehozása _mergedcertificate.crt_. Egy szövegszerkesztőben másolja az egyes tanúsítványok hello tartalmat a fájlba. a tanúsítványok hello sorrendjét a következő sablon hello kell hasonlítania:

```
-----BEGIN CERTIFICATE-----
<your Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-toopfx"></a>Tanúsítvány tooPFX exportálása

Exportálja az egyesített SSL-tanúsítvány hello titkos kulccsal, a tanúsítvány kérelmezéséhez során létrejött.

Ha a tanúsítvány kérelmezéséhez OpenSSL használatával jön létre, majd hozott létre a titkos kulcs fájlja. tooexport a tanúsítvány tooPFX, futtassa a következő parancs hello. Cserélje le a helyőrzőket hello  _&lt;titkos kulcsfájl >_ és  _&lt;egyesített-tanúsítványfájl >_.

```
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

Amikor a rendszer kéri, adja meg az exportálási jelszó. Ezt a jelszót fog használni, az SSL tanúsítvány tooApp szolgáltatás később feltöltésekor.

Ha az IIS vagy _Certreq.exe_ toogenerate a tanúsítványkérelmet, a telepítés hello tanúsítvány tooyour helyi számítógép, majd [hello tanúsítvány tooPFX exportálása](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>Az SSL-tanúsítvány feltöltése

tooupload az SSL-tanúsítvány, kattintson a **SSL-tanúsítványok** a bal oldali navigációs webalkalmazás hello.

Kattintson a **-tanúsítvány feltöltése**.

A **PFX-tanúsítványfájlt**, válassza ki a PFX-fájlt. A **tanúsítványjelszavas**, típus hello hello PFX-fájl exportálása során létrehozott jelszót.

Kattintson a **Feltöltés** gombra.

![Tanúsítvány feltöltése](./media/app-service-web-tutorial-custom-ssl/upload-certificate.png)

Megjelenik az App Service befejezése után a tanúsítvány feltöltése hello **SSL-tanúsítványok** lap.

![A tanúsítvány feltöltése](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>Az SSL-tanúsítvány kötése

A hello **SSL-kötések** kattintson **kötés hozzáadása**.

A hello **hozzá SSL-kötés** lapján hello legördülő listák megnyílásának tooselect hello tartomány neve toosecure, valamint hello tanúsítvány toouse.

> [!NOTE]
> Ha a tanúsítvány feltöltött, de nem látja a hello hello tartomány neve **állomásnév** legördülő menüből, próbálja meg frissíteni hello böngésző lapot.
>
>

A **SSL típus**, jelölje be e toouse  **[kiszolgálónév jelzése (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  vagy IP-alapú SSL.

- **Az SNI-alapú SSL** -több SNI-alapú SSL-kötések adhatók hozzá. Ez a beállítás lehetővé teszi több SSL-tanúsítványok toosecure hello a több tartományt ugyanazon IP-címet. (Beleértve az Internet Explorer, Chrome, Firefox és Opera) a legtöbb modern böngésző támogatja az SNI (átfogóbb böngésző információt, [kiszolgálónév jelzése](http://wikipedia.org/wiki/Server_Name_Indication)).
- **IP-alapú SSL** -csak egy IP-alapú SSL-kötés adhatók hozzá. Ez a beállítás lehetővé teszi, hogy csak egy SSL-tanúsítvány toosecure egy dedikált nyilvános IP-címet. toosecure több tartomány, biztosítania kell őket minden használatával hello egy SSL-tanúsítvány. Ez a lehetőség hello hagyományos SSL-kötés.

Kattintson a **kötés hozzáadása**.

![SSL-tanúsítvány kötésének létrehozása](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

Megjelenik az App Service befejezése után a tanúsítvány feltöltése hello **SSL-kötések** szakaszok.

![A kötött tanúsítvány tooweb alkalmazás](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>Adja meg újból egy rekordot az IP-SSL

Ha a webalkalmazáson belüli IP-alapú SSL nem adja meg, hagyja ki túl[teszt a HTTPS-t az egyéni tartomány](#test).

Alapértelmezés szerint a webalkalmazás egy megosztott nyilvános IP-címet használja. IP-alapú SSL tanúsítvány kötése, az App Service létrehoz egy új, dedikált IP-címet a webes alkalmazást.

Ha egy A rekord tooyour webalkalmazás leképezett, a tartomány rendszerleíró adatbázis frissítése az új, dedikált IP-címmel.

A webalkalmazás **egyéni tartomány** lap tartalmát hello új, dedikált IP-címmel. [Az IP-cím másolása](app-service-web-tutorial-custom-domain.md#info), majd [újramegfeleltetése hello rekord](app-service-web-tutorial-custom-domain.md#map-an-a-record) toothis új IP-címet.

<a name="test"></a>

## <a name="test-https"></a>Teszt HTTPS

Most már elhagyta toodo csak a toomake meg arról, hogy HTTPS működik-e az egyéni tartományhoz. A különböző böngészők, tallózással keresse meg túl`https://<your.custom.domain>` toosee, amely kész a webalkalmazás működik.

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Ha a webes alkalmazást ad meg a tanúsítvány érvényesítési hibákat, valószínűleg egy önaláírt tanúsítványt használ.
>
> Ha ez nem hello eset, akkor előfordulhat, hogy kimaradt köztes tanúsítványa a toohello PFX-tanúsítványfájlt exportálásakor.

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>HTTPS kényszerítése

Alkalmazás szolgáltatásnak nincs *nem* kényszerítése a HTTPS-t, így bárki, aki továbbra is elérheti a HTTP-n keresztül webalkalmazás. a webalkalmazás HTTPS tooenforce átdolgozás szabály megadása a hello _web.config_ fájl a webalkalmazás. App Service ezt a fájlt, függetlenül a webalkalmazás hello nyelvi keretrendszert használja.

> [!NOTE]
> Kérelmek átirányítása nyelvspecifikus van. ASP.NET MVC használható hello [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) helyett hello módosítsa úgy a szabály a szűrő _web.config_.

Ha a .NET-fejlesztők, ezzel a fájllal viszonylag tisztában kell lennie. A megoldás hello gyökérkönyvtárában van.

Azt is megteheti Ha a PHP, Node.js, Python vagy Java fejleszt, esély van jelenleg ezt a fájlt az Ön nevében létrehozott App Service-ben.

Csatlakozás tooyour web app FTP végpont: hello utasításokat követve [telepítheti az alkalmazást tooAzure használatával az FTP/S App Service](app-service-deploy-ftp.md).

Ez a fájl megtalálható _/home/site/wwwroot_. Ha nem, hozzon létre egy _web.config_ fájl a mappában, amelynek a következő XML hello:

```xml   
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <!-- BEGIN rule ELEMENT FOR HTTPS REDIRECT -->
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
        <!-- END rule ELEMENT FOR HTTPS REDIRECT -->
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

Egy létező _web.config_ fájlt, másolja a hello teljes `<rule>` az elem a _web.config_tartozó `configuration/system.webServer/rewrite/rules` elemet. Ha nincsenek más `<rule>` elemeinek a _web.config_, másolt sor hello `<rule>` elem előtt más hello `<rule>` elemek.

Ez a szabály olyan HTTP 301 (Állandó átirányítás) toohello HTTPS protokollt adja vissza, amikor hello felhasználó esetén egy HTTP-kérelem tooyour webalkalmazás. Például az átirányítja a `http://contoso.com` túl`https://contoso.com`.

Hello IIS URL-újraíró modult további információkért lásd: hello [URL-újraíró](http://www.iis.net/downloads/microsoft/url-rewrite) dokumentációját.

## <a name="enforce-https-for-web-apps-on-linux"></a>A Web Apps Linux HTTPS kényszerítése

Linux alkalmazás-szolgáltatásnak nincs *nem* kényszerítése a HTTPS-t, így bárki, aki továbbra is elérheti a HTTP-n keresztül webalkalmazás. a webalkalmazás HTTPS tooenforce átdolgozás szabály megadása a hello _.htaccess_ fájl a webalkalmazás. 

Csatlakozás tooyour web app FTP végpont: hello utasításokat követve [telepítheti az alkalmazást tooAzure használatával az FTP/S App Service](app-service-deploy-ftp.md).

A _/home/site/wwwroot_, hozzon létre egy _.htaccess_ hello kód a következő fájlt:

```
RewriteEngine On
RewriteCond %{HTTP:X-ARR-SSL} ^$
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

Ez a szabály olyan HTTP 301 (Állandó átirányítás) toohello HTTPS protokollt adja vissza, amikor hello felhasználó esetén egy HTTP-kérelem tooyour webalkalmazás. Például az átirányítja a `http://contoso.com` túl`https://contoso.com`.

## <a name="automate-with-scripts"></a>Parancsfájlok automatizálásához

Automatizálható SSL-kötések parancsfájlok, a webalkalmazás hello segítségével [Azure CLI](/cli/azure/install-azure-cli) vagy [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

a következő parancs hello exportált PFX-fájlt feltölti, valamint lekéri a hello ujjlenyomata.

```bash
thumbprint=$(az appservice web config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

hello alábbi parancs hozzáadja az SNI-alapú SSL-kötést, hello ujjlenyomata hello előző parancs segítségével.

```bash
az appservice web config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a>Azure PowerShell

hello következő parancsot az exportált PFX-fájlt feltölti és hozzáadja az SNI-alapú SSL-kötést.

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```

## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Frissítse az alkalmazás tarifacsomag kiválasztása
> * Az egyéni SSL-tanúsítvány tooApp szolgáltatás kötése
> * Az alkalmazás HTTPS kényszerítése
> * SSL-tanúsítvány kötés parancsfájlokkal automatizálhatja

Hogyan előzetes toohello következő útmutató toolearn toouse Azure Content Delivery Network.

> [!div class="nextstepaction"]
> [Adja hozzá a Content Delivery Network (CDN) tooan Azure App Service](app-service-web-tutorial-content-delivery-network.md)
