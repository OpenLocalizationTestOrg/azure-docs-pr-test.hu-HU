---
title: "egy meglévő egyéni DNS aaaMap tooAzure webalkalmazások name |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd egy meglévő egyéni DNS-tartomány (kreatív tartomány) tooa webalkalmazás, mobil-háttéralkalmazás vagy az Azure App Service API-alkalmazás neve."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2c4eea3c56c758ca11355554321ffa52dd2c6b9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="map-an-existing-custom-dns-name-tooazure-web-apps"></a>Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése

Az [Azure Web Apps](app-service-web-overview.md) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás. Az oktatóanyag bemutatja, hogyan toomap egy meglévő egyéni DNS name tooAzure webalkalmazások.

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Altartomány leképezése (például `www.contoso.com`) egy olyan CNAME rekordot használatával
> * Egy legfelső szintű tartomány leképezése (például `contoso.com`) az A rekord használatával
> * Rendelni egy helyettesítő tartományt (például `*.contoso.com`) egy olyan CNAME rekordot használatával
> * Tartomány leképezése parancsfájlokkal automatizálhatja

Választhat egy **CNAME rekord** vagy egy **rekord** toomap egy egyéni DNS tooApp szolgáltatás neve. 

> [!NOTE]
> Azt javasoljuk, hogy használ egy olyan CNAME REKORDOT a legfelső szintű tartomány kivételével az összes egyéni DNS-nevek (például `contoso.com`).

toomigrate élő webhelyet, és a DNS tartomány neve tooApp szolgáltatás, lásd: [áttelepítése egy aktív DNS-név tooAzure App Service](app-service-custom-domain-name-migrate.md).

## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

* [App Service-alkalmazás létrehozása](/azure/app-service/), vagy egy másik az oktatóanyaghoz létrehozott alkalmazást használni.
* Vásároljon egy tartomány nevét, és ellenőrizze, hogy hozzáférési toohello DNS beállításjegyzék a tartomány-szolgáltató (például a GoDaddy).

  Például tooadd DNS-bejegyzéseket `contoso.com` és `www.contoso.com`, képes tooconfigure hello DNS-beállításainak hello kell `contoso.com` gyökértartomány.

  > [!NOTE]
  > Ha még nem rendelkezik egy meglévő tartomány nevét, érdemes lehet [megvásárlásáról a tartomány használatával hello Azure-portálon](custom-dns-web-site-buydomains-web-app.md). 

## <a name="prepare-hello-app"></a>Hello alkalmazás előkészítése

egy egyéni DNS neve tooa webalkalmazás, webes alkalmazás hello toomap [App Service-csomag](https://azure.microsoft.com/pricing/details/app-service/) fizetős rétegben kell lennie (**megosztott**, **alapvető**, **szabványos**, vagy  **Prémium szintű**). Ebben a lépésben biztosíthatja, hogy az App Service alkalmazás van hello hello támogatott tarifacsomagot.

### <a name="sign-in-tooazure"></a>Jelentkezzen be tooAzure

Nyissa meg hello [Azure-portálon](https://portal.azure.com) és jelentkezzen be az Azure-fiókjával.

### <a name="navigate-toohello-app-in-hello-azure-portal"></a>Keresse meg az alkalmazás toohello hello Azure-portálon

Hello bal oldali menüben válassza ki a **alkalmazásszolgáltatások**, majd válassza ki a hello alkalmazás hello nevét.

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/select-app.png)

Az App Service alkalmazás hello hello felügyeleti oldal akkor jelenik meg.  

<a name="checkpricing"></a>

### <a name="check-hello-pricing-tier"></a>IP-címek hello ellenőrzése

A bal oldali navigációs hello app lap hello, görgessen a toohello **beállítások** válassza ki azt **vertikális felskálázás (App Service-csomag)**.

![Méretezett menü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

hello alkalmazás jelenlegi rétegtől kék szegély ki van jelölve. Ellenőrizze, hogy hello alkalmazáshoz nincs hello toomake **szabad** réteg. Hello nem támogatja az egyéni DNS **szabad** réteg. 

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Ha hello App Service-csomag nincs **szabad**zárja be, hello **válasszon tarifacsomagot** lapon, és hagyja ki túl[képezi le egy olyan CNAME rekordot](#cname).

<a name="scaleup"></a>

### <a name="scale-up-hello-app-service-plan"></a>Vertikális felskálázás hello App Service-csomag

Válassza ki valamelyik hello nem szabad rétegek (**megosztott**, **alapvető**, **szabványos**, vagy **prémium**). 

Kattintson a **Kiválasztás** gombra.

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Amikor megjelenik a következő értesítési hello, hello skálázási művelet be nem fejeződött.

![Skálázási művelet megerősítése](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a>A CNAME rekord leképezése

Hello oktatóanyag példában hello CNAME rekord hozzáadása `www` altartomány (például `www.contoso.com`).

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a>Hello CNAME rekord létrehozása

Adja hozzá egy CNAME rekord toomap egy altartomány toohello alkalmazás alapértelmezett állomásnév (`<app_name>.azurewebsites.net`).

A hello `www.contoso.com` tartomány például adjon hozzá egy CNAME rekordot, amely leképezhető hello neve `www` túl`<app_name>.azurewebsites.net`.

Miután hozzáadta a hello CNAME, hello DNS-rekordok oldalon néz ki a következő példa hello:

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-hello-cname-record-mapping-in-azure"></a>Engedélyezett hello CNAME rekord-hozzárendelést az Azure-ban

Hello hello app lap navigációs maradtak hello Azure-portálon, válassza ki **egyéni tartományok**. 

![Az egyéni tartomány menü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

A hello **egyéni tartományok** lap hello alkalmazás hozzáadása a hello teljesen minősített egyéni DNS-nevét (`www.contoso.com`) toohello listája.

Jelölje be hello  **+**  ikon következő túl**állomásnév hozzáadása**.

![Adja hozzá a gazdagép neve](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Típus hello teljesen minősített tartománynevét egy CNAME rekordot, például a hozzáadott `www.contoso.com`. 

Válassza ki **érvényesítése**.

Hello **állomásnév hozzáadása** gomb aktiválásakor. 

Győződjön meg arról, hogy **állomásnév rekordtípus** értéke túl**CNAME (www.example.com vagy bármely altartomány)**.

Válassza ki **állomásnév hozzáadása**.

![DNS-név toohello alkalmazás hozzáadása](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Hello hello app kerülnek új állomásnév toobe némi időbe telhet **egyéni tartományok** lap. Próbálja meg frissíteni a hello böngésző tooupdate hello adatokat.

![CNAME rekord hozzáadva](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Ha nem talált meg egy lépést, vagy elgépelte valahol korábban, egy ellenőrzési hiba hello hello lap alsó részén látható.

![Ellenőrzési hiba](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a>Az A rekord leképezése

Hello oktatóanyag példában hello legfelső szintű tartomány az A rekord hozzáadása (például `contoso.com`). 

<a name="info"></a>

### <a name="copy-hello-apps-ip-address"></a>Hello app IP-cím másolása

toomap egy A rekordot kell hello app külső IP-címet. Az IP-cím hello alkalmazásban található **egyéni tartományok** hello Azure portálra a lap.

Hello hello app lap navigációs maradtak hello Azure-portálon, válassza ki **egyéni tartományok**. 

![Az egyéni tartomány menü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

A hello **egyéni tartományok** lapján hello app IP-cím másolásához.

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-a-record"></a>Hello A rekord létrehozása

toomap egy A rekord tooan alkalmazás, az App Service csak **két** DNS-rekordokat:

- Egy **A** toomap toohello app IP-cím rögzítése.
- A **TXT** toomap toohello alkalmazás alapértelmezett állomásnév rögzítse `<app_name>.azurewebsites.net`. App Service használja ezt a rekordot csak egyidejűleg konfigurációs tooverify, hogy Ön a tulajdonosa hello egyéni tartományt. Miután az egyéni tartomány érvényesítve, és az App Service-ben konfigurált, törölheti a TXT-rekord. 

A hello `contoso.com` tartomány például hello és TXT-rekordok megfelelően a következő táblázat toohello létrehozása (`@` általában jelöli hello gyökértartomány). 

| Bejegyzéstípus | Gazdagép | Érték |
| - | - | - |
| A | `@` | IP-cím a [másolási hello app IP-cím](#info) |
| TXT | `@` | `<app_name>.azurewebsites.net` |

Hello bejegyzések felvételekor hello DNS-rekordok oldalon néz ki a következő példa hello:

![DNS-rekordok lap](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-hello-a-record-mapping-in-hello-app"></a>Egy rekord leképezése hello alkalmazásban hello engedélyezése

Vissza a hello app **egyéni tartományok** hello Azure-portálon lapján hozzáadása hello teljesen minősített egyéni DNS-nevét (például `contoso.com`) toohello listája.

Jelölje be hello  **+**  ikon következő túl**állomásnév hozzáadása**.

![Adja hozzá a gazdagép neve](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

Típus hello teljesen minősített tartománynevét hello A rekord, például a konfigurált `contoso.com`.

Válassza ki **érvényesítése**.

Hello **állomásnév hozzáadása** gomb aktiválásakor. 

Győződjön meg arról, hogy **állomásnév rekordtípus** értéke túl**egy olyan rekordot (example.com)**.

Válassza ki **állomásnév hozzáadása**.

![DNS-név toohello alkalmazás hozzáadása](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Hello hello app kerülnek új állomásnév toobe némi időbe telhet **egyéni tartományok** lap. Próbálja meg frissíteni a hello böngésző tooupdate hello adatokat.

![Egy hozzáadott bejegyzés](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

Ha nem talált meg egy lépést, vagy elgépelte valahol korábban, egy ellenőrzési hiba hello hello lap alsó részén látható.

![Ellenőrzési hiba](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a>A helyettesítő tartomány leképezése

Hello oktatóanyag példában leképez egy [helyettesítő DNS-név](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (például `*.contoso.com`) toohello App Service alkalmazáshoz egy olyan CNAME rekordot hozzáadásával. 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a>Hello CNAME rekord létrehozása

Adja hozzá egy CNAME rekord toomap egy helyettesítő karakteres nevet toohello alkalmazás alapértelmezett állomásnév (`<app_name>.azurewebsites.net`).

A hello `*.contoso.com` tartomány például hello CNAME rekord leképezése hello neve `*` túl`<app_name>.azurewebsites.net`.

Hello CNAME felvételekor hello DNS-rekordok oldalon néz ki a következő példa hello:

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-hello-cname-record-mapping-in-hello-app"></a>Hello CNAME rekord hozzárendelésének hello alkalmazásban engedélyezése

Mostantól hozzáadhatja azok bármely altartomány megfelelő hello helyettesítő neve toohello app (például `sub1.contoso.com` és `sub2.contoso.com` megfelelő `*.contoso.com`). 

Hello hello app lap navigációs maradtak hello Azure-portálon, válassza ki **egyéni tartományok**. 

![Az egyéni tartomány menü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Jelölje be hello  **+**  ikon következő túl**állomásnév hozzáadása**.

![Adja hozzá a gazdagép neve](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Írjon be egy teljesen minősített tartománynevet, amely megfelel a hello helyettesítő tartomány (például `sub1.contoso.com`), majd válassza ki **ellenőrzése**.

Hello **állomásnév hozzáadása** gomb aktiválásakor. 

Győződjön meg arról, hogy **állomásnév rekordtípus** értéke túl**CNAME rekord (www.example.com vagy bármely altartomány)**.

Válassza ki **állomásnév hozzáadása**.

![DNS-név toohello alkalmazás hozzáadása](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

Hello hello app kerülnek új állomásnév toobe némi időbe telhet **egyéni tartományok** lap. Próbálja meg frissíteni a hello böngésző tooupdate hello adatokat.

Jelölje be hello  **+**  ikon újra tooadd egy másik állomásnév, amely megfelel a hello helyettesítő tartomány. Adja hozzá például `sub2.contoso.com`.

![CNAME rekord hozzáadva](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a>A böngészőben tesztelése

Keresse meg a korábban megadott értékektől toohello DNS-nevek (például `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, és `sub2.contoso.com`).

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a>Parancsfájlok automatizálásához

Automatizálható a parancsfájlok, az egyéni tartományok felügyeleti hello segítségével [Azure CLI](/cli/azure/install-azure-cli) vagy [Azure PowerShell](/powershell/azure/overview). 

### <a name="azure-cli"></a>Azure CLI 

a következő parancs hello ad hozzá egy konfigurált egyéni DNS nevét tooan App Service-alkalmazást. 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

További információkért lásd: [képezi le egy egyéni tartomány tooa webalkalmazás](scripts/app-service-cli-configure-custom-domain.md). 

### <a name="azure-powershell"></a>Azure PowerShell 

a következő parancs hello ad hozzá egy konfigurált egyéni DNS nevét tooan App Service-alkalmazást. 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

További információkért lásd: [hozzárendelése egyéni tartományhoz tooa webalkalmazás](scripts/app-service-powershell-configure-custom-domain.md).

## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * A CNAME rekord használatával altartomány térkép
> * Az A rekord használatával a legfelső szintű tartomány leképezése
> * Térkép egy helyettesítő tartomány CNAME rekord használatával
> * Tartomány leképezése parancsfájlokkal automatizálhatja

Előzetes toohello következő útmutató toolearn hogyan toobind egyéni SSL tanúsítvány tooa webalkalmazás.

> [!div class="nextstepaction"]
> [Egy meglévő egyéni SSL tanúsítvány tooAzure webalkalmazások kötése](app-service-web-tutorial-custom-ssl.md)
