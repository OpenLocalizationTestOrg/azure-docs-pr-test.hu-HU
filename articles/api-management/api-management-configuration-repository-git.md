---
title: "aaaConfigure a Git - Azure használatával API Management szolgáltatáshoz |} Microsoft Docs"
description: "Megtudhatja, hogyan toosave és a Git használatával az API Management-konfigurációjának beállítása."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: mattfarm
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: ef7d4c18f2ea3f5c9b86403349a83aef240f979b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosave-and-configure-your-api-management-service-configuration-using-git"></a>Hogyan toosave és a Git használatával az API Management-konfigurációjának beállítása
> 
> 

Minden API Management szolgáltatáspéldány hello konfigurációs és a metaadatok hello szolgáltatáspéldány kapcsolatos információkat tartalmazó konfigurációs adatbázis tárolja. Módosítások elvégzéséhez toohello szolgáltatáspéldány módosításával hello publisher portált a megfelelő értéket, PowerShell-parancsmag használatával, vagy a REST API-hívással. Ezenkívül toothese módszerek is kezelhet, a példány konfigurációjának Git használatával, a szolgáltatás felügyeleti lehetőségeket, mint engedélyezése:

* Konfigurációs rendszerverzió - töltse le, és tárolja a szolgáltatás konfigurációs különböző verziói
* Konfigurációs módosítások tömeges - módosítsa a szolgáltatás konfigurációs toomultiple részei a helyi tárház és hello módosítások hátsó toohello server integrálható egy műveletet
* Jól ismert Git toolchain és munkafolyamat - hello Git tooling és, amelyek már ismeri a munkafolyamatok használni

a következő diagram hello az API Management szolgáltatáspéldány hello különböző módokon tooconfigure áttekintését mutatja.

![Git konfigurálása][api-management-git-configure]

Ha módosít tooyour szolgáltatás hello publisher portál, a PowerShell-parancsmagok vagy a hello REST API használatával, a szolgáltatás konfigurációs adatbázist hello kezelt `https://{name}.management.azure-api.net` végpont, hello diagram hello jobb oldalán látható módon. hello bal oldalán található hello diagram bemutatja, hogyan kezelheti a Git használatával konfigurációjának és a szolgáltatás Git-tárházban található `https://{name}.scm.azure-api.net`.

a lépéseket követve hello nyújt áttekintést a Git használatával az API Management szolgáltatáspéldány kezelése.

1. A szolgáltatás a Git-konfiguráció
2. A szolgáltatás konfigurációs adatbázis tooyour Git-tárház mentése
3. Hello Git tárház tooyour helyi gép klónozása
4. Hello legújabb tárház tooyour helyi számítógép le, és a véglegesítési és leküldéses módosítások hátsó tooyour tárház lekéréses
5. A tárházból hello módosítások üzembe helyezés a szolgáltatás konfigurációs adatbázis

Ez a cikk ismerteti, hogyan tooenable Git toomanage használ a szolgáltatás konfigurációját, és az nyújt a hello fájlok és mappák hello Git-tárházban.

## <a name="access-git-configuration-in-your-service"></a>A szolgáltatás a Git-konfiguráció
Gyorsan megtekintheti a Git-konfiguráció állapota hello hello publisher portál jobb felső sarkában hello hello Git ikon megtekintésével. Ebben a példában hello állapotüzenet azt jelzi, hogy vannak-e a nem mentett módosítások toohello tárház. Ennek az az oka hello API-kezelés szolgáltatás konfigurációs adatbázis van még nincsenek mentve toohello tárházba.

![Git állapota][api-management-git-icon-enable]

tooview és a Git konfigurációs beállításait, hello Git ikonra, vagy kattintson a hello **biztonsági** menüt, és keresse meg a toohello **konfigurációs tárház** fülre.

![GIT engedélyezése][api-management-enable-git]

> [!IMPORTANT]
> Bármely titkos kulcsok nem definiált tulajdonságok hello tárházban fogja tárolni, és akkor is marad, amíg az előzmények tiltsa le, és engedélyezze újra a Git-hozzáférés. Tulajdonságok adjon meg egy biztonságos hely toomanage állandó karakterlánc-értékek titkos adatait, beleértve a összes API konfigurálása és házirendek, így nem kell toostore közvetlenül a házirend-utasításoknál őket. További információkért lásd: [hogyan toouse tulajdonságokat az Azure API-felügyeleti házirendek](api-management-howto-properties.md).
> 
> 

Az engedélyezés vagy letiltás hello REST API-t használó Git hozzáférés információkért lásd: [engedélyezheti vagy tilthatja le a REST API hello Git-hozzáférés](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="toosave-hello-service-configuration-toohello-git-repository"></a>toosave hello szolgáltatás konfigurációs toohello Git-tárház
hello első lépéseket, mielőtt hello tárház klónozása toosave hello aktuális állapot hello szolgáltatás konfigurációs toohello tárház. Kattintson a **konfigurációs toorepository mentése**.

![Konfiguráció mentése][api-management-save-configuration]

Adja meg a kívánt módosításokat a hello visszaigazoló képernyő, és kattintson a **Ok** toosave.

![Konfiguráció mentése][api-management-save-configuration-confirm]

Néhány másodpercen belül a program menti a hello konfigurációt, és hello konfiguráció állapota hello tárház megjelennek, például a hello dátum és idő hello utolsó konfigurációváltozás és a legutóbbi szinkronizálás hello hello szolgáltatás konfigurációja és hello között tárház.

![Konfiguráció állapota][api-management-configuration-status]

Miután hello konfigurációs toohello tárház menti, akkor klónozható.

Információ a hello REST API használatával a művelet végrehajtása: [véglegesítési konfiguráció pillanatkép-hello REST API használatával](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="tooclone-hello-repository-tooyour-local-machine"></a>tooclone hello tárház tooyour helyi számítógép
a tárház tooclone, kell hello URL-cím tooyour tárház, a felhasználónevet és jelszót. hello felhasználói nevét és URL-cím megjelenített hello hello tetején **konfigurációs tárház** fülre.

![Git-klón][api-management-configuration-git-clone]

hello jelszó jön létre hello hello alján **konfigurációs tárház** fülre.

![Jelszó létrehozása][api-management-generate-password]

toogenerate jelszavát, győződjön meg arról, hogy hello **lejárati** toohello szükséges a lejárati dátum és idő beállítása, és kattintson a **azonosító létrehozása**.

![Jelszó][api-management-password]

> [!IMPORTANT]
> Jegyezze fel ezt a jelszót. Ha hagyja a lap hello jelszó nem jelenik meg újra.
> 
> 

az eszköz a következő példák használja a Git bash eszközt hello hello [Git for Windows](http://www.git-scm.com/downloads) , de bármely, a ismeri a Git-eszközt is használhatja.

Nyissa meg a Git eszköz hello kívánt mappa, és futtassa a következő parancs tooclone hello git tárház tooyour helyi számítógép, hello publisher portál által szolgáltatott hello paranccsal hello.

```
git clone https://bugbashdev4.scm.azure-api.net/
```

Hello felhasználónevet és jelszót adjon meg.

Ha bármilyen hiba jelentkezik, próbálja meg módosítani a `git clone` parancs tooinclude hello felhasználónevét és jelszavát, ahogy az alábbi példa hello.

```
git clone https://username:password@bugbashdev4.scm.azure-api.net/
```

Ez biztosítja, hogy a hiba, ha próbálja hello parancs része hello jelszó kódolás URL-CÍMÉT. Egy gyorsan toodo ez tooopen Visual Studio, és probléma hello következő parancsot a hello **parancsablakban**. tooopen hello **parancsablakban**, nyissa meg a bármely megoldás vagy a projektet a Visual Studio (vagy hozzon létre egy új üres konzolalkalmazást), és válassza a **Windows**, **Immediate** a Hello **Debug** menü.

```
?System.NetWebUtility.UrlEncode("password from publisher portal")
```

A felhasználó nevét és a tárház hely tooconstruct hello git parancs együtt kódolású hello jelszó használata.

```
git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/
```

Miután hello tárház klónozták tekintheti meg és a helyi rendszer használható. További információkért lásd: [fájl- és helyi Git-tárház hivatkozás szerkezeti](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="tooupdate-your-local-repository-with-hello-most-current-service-instance-configuration"></a>tooupdate a helyi-tárház hello legfrissebb példányának konfigurációja
Ha módosítások tooyour API Management service-példány hello publisher portálon vagy a hello REST API használatával, a helyi tárház hello legújabb módosításokkal frissítése előtt mentenie kell a módosítások toohello tárház. toodo, kattintson **konfigurációs toorepository mentése** a hello **konfigurációs tárház** hello publisher portál lapot, és hogyan adhat ki a következő parancs a helyi tárházban hello.

```
git pull
```

Futtatása előtt `git pull` érdekében, hogy hello mappában található a helyi tárház. Ha befejezte a hello `git clone` parancsot, majd meg kell változtatnia hello directory tooyour tárház hello következő parancs futtatásával.

```
cd bugbashdev4.scm.azure-api.net/
```

## <a name="toopush-changes-from-your-local-repo-toohello-server-repo"></a>a helyi tárház toohello server tárházból toopush módosítások
toopush módosítja a helyi tárház toohello server tárházban lévő, a változtatások véglegesítése a határidő és majd küldje le őket toohello server tárház kell. toocommit a módosításokat, a Git parancs eszköz, a helyi tárház, és a következő parancsok probléma hello kapcsoló toohello könyvtár megnyitásához.

```
git add --all
git commit -m "Description of your changes"
```

összes hello véglegesíti toohello kiszolgálón, futtassa toopush hello a következő parancsot.

```
git push
```

## <a name="toodeploy-any-service-configuration-changes-toohello-api-management-service-instance"></a>toodeploy bármilyen konfigurációs módosításokat toohello API-kezelés szolgáltatás szolgáltatáspéldány
Ha a helyi módosítások véglegesítése és megnyomott toohello server tárház, telepíthetők tooyour API Management service-példány.

![Üzembe helyezés][api-management-configuration-deploy]

Információ a hello REST API használatával a művelet végrehajtása: [telepítése Git változik hello REST API használatával tooconfiguration adatbázis](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Fájl- és helyi Git-tárház szerkezete-hivatkozás
hello fájlok és mappák hello helyi git-tárházban hello konfigurációs információkat tartalmaznak hello szolgáltatáspéldány.

| Elem | Leírás |
| --- | --- |
| Api-felügyeleti gyökérmappa |Hello szolgáltatáspéldány legfelső szintű konfigurációját tartalmazza |
| API-k mappa |A következő szolgáltatáspéldányba hello hello API-k hello konfigurációját tartalmazza |
| csoportok mappa |A következő szolgáltatáspéldányba hello hello csoportok hello konfigurációját tartalmazza |
| házirend mappa |Tartalmazza a hello házirendek hello szolgáltatáspéldány |
| portalStyles mappa |A szolgáltatáspéldány hello hello developer portálon testreszabások hello konfigurációját tartalmazza |
| termékek mappa |A következő szolgáltatáspéldányba hello hello termékek hello konfigurációját tartalmazza |
| sablonok mappa |A következő szolgáltatáspéldányba hello hello e-mail sablonok hello konfigurációját tartalmazza |

Minden mappa tartalmazhat egy vagy több fájlt, és bizonyos esetekben egy vagy több mappát, például egy mappát az egyes API-t, a termék vagy a csoport. hello minden mappában található fájlok adott hello mappa neve szerint hello entitástípushoz.

| Fájltípus | Cél |
| --- | --- |
| JSON-ban |Hello megfelelő entitás konfigurációs adatait |
| HTML |Hello entitás, gyakran hello developer portálon megjelenő leírást |
| xml |Házirend-utasítások |
| CSS |A fejlesztői portálon testreszabáshoz stíluslapok |

Ezeket a fájlokat létrehozása, törlése, szerkeszthető, és a helyi fájlrendszerben felügyelt, és hello módosítások telepített hátsó toohello az API Management szolgáltatáspéldány.

> [!NOTE]
> hello következő entitások nem hello Git-tárházban szereplő és nem lehet konfigurálni a Git használatával.
> 
> * Felhasználók
> * Előfizetések
> * Tulajdonságok
> * Fejlesztői portálon entitások eltérő stílusok
> 
> 

### <a name="root-api-management-folder"></a>Api-felügyeleti gyökérmappa
hello legfelső szintű `api-management` a mappa tartalmaz egy `configuration.json` hello szolgáltatáspéldány formátuma a következő hello a legfelső szintű információkat tartalmazó fájlt.

```json
{
  "settings": {
    "RegistrationEnabled": "True",
    "UserRegistrationTerms": null,
    "UserRegistrationTermsEnabled": "False",
    "UserRegistrationTermsConsentRequired": "False",
    "DelegationEnabled": "False",
    "DelegationUrl": "",
    "DelegatedSubscriptionEnabled": "False",
    "DelegationValidationKey": ""
  },
  "$ref-policy": "api-management/policies/global.xml"
}
```

első négy beállítások hello (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, és `UserRegistrationTermsConsentRequired`) hozzárendelését követően beállítások hello toohello **identitások** hello lapján **biztonsági** szakasz.

| Azonosító beállítása | A Maps túl|
| --- | --- |
| RegistrationEnabled |**A névtelen felhasználók toosign oldal átirányítási** jelölőnégyzet |
| UserRegistrationTerms |**Használati feltételek a felhasználói regisztráció** szövegmező |
| UserRegistrationTermsEnabled |**Használati feltételek megjelenítése a bejelentkezési lapon** jelölőnégyzet |
| UserRegistrationTermsConsentRequired |**Hozzájárulás szükséges** jelölőnégyzet |

![Identitás][api-management-identity-settings]

hello a következő négy beállítások (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, és `DelegationValidationKey`) hozzárendelését követően beállítások hello toohello **delegálás** hello lapján **biztonsági** szakasz.

| Delegálási beállítás | A Maps túl|
| --- | --- |
| DelegationEnabled |**Delegált bejelentkezési és regisztrációs** jelölőnégyzet |
| DelegationUrl |**Delegálás végponti URL-cím** szövegmező |
| DelegatedSubscriptionEnabled |**Termék-előfizetéshez delegálása** jelölőnégyzet |
| DelegationValidationKey |**Érvényesítési kulcs delegálása** szövegmező |

![A delegálási beállításokat][api-management-delegation-settings]

végső beállítás hello `$ref-policy`, toohello globális utasítások házirendfájl hello szolgáltatáspéldány rendeli.

### <a name="apis-folder"></a>API-k mappa
Hello `apis` mappa tartalmaz egy mappát az egyes API hello szolgáltatást a példányon, amely tartalmazza a következő elemek hello.

* `apis\<api name>\configuration.json`-Ez hello API hello konfigurációját és hello háttérkiszolgáló URL-címe és hello műveletek tartalmaz információkat. Ez a hello ugyanazokat az információkat, amely akkor tér vissza, ha toocall [egy meghatározott API beszerzésének](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) rendelkező `export=true` a `application/json` formátumban.
* `apis\<api name>\api.description.html`-Ez hello hello API leírása és felel meg a toohello `description` hello tulajdonságának [API entitás](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
* `apis\<api name>\operations\`– Ez a mappa tartalmaz `<operation name>.description.html` fájlok, amelyek kapcsolódnak hello API toohello műveletei. Minden fájl tartalmazza a hello API, amely a beléptetést toohello egyetlen műveletben hello leírása `description` hello tulajdonságának [művelet entitás](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) a hello REST API-t.

### <a name="groups-folder"></a>csoportok mappa
Hello `groups` mappa tartalmaz egy mappát az egyes hello szolgáltatáspéldány definiálva.

* `groups\<group name>\configuration.json`-hello csoport hello beállítani. Ez a hello ugyanazokat az információkat, amely akkor tér vissza, ha toocall hello [egy adott csoport lekérése](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) műveletet.
* `groups\<group name>\description.html`-Ez hello csoport hello leírása és felel meg a toohello `description` hello tulajdonságának [entitás csoport](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>házirend mappa
Hello `policies` mappa hello házirend-utasításoknál a szolgáltatáspéldány tartalmazza.

* `policies\global.xml`-a szolgáltatáspéldány globális hatókörben meghatározott házirendek szerint tartalmazza.
* `policies\apis\<api name>\`-Ha bármely API hatókörből meghatározott házirendek szerint, akkor ebben a mappában találhatók.
* `policies\apis\<api name>\<operation name>\`mappa - Ha minden házirendet a hatókör művelet van megadva, akkor az a mappában található `<operation name>.xml` fájlok, amelyek kapcsolódnak a házirend-utasításoknál toohello minden művelethez.
* `policies\products\`-Ha minden házirendet a termék hatókörben van megadva, ezt tartalmazza ezt a mappát, amelybe `<product name>.xml` fájlok, amelyek az egyes termékek toohello házirend-utasításoknál kapcsolódnak.

### <a name="portalstyles-folder"></a>portalStyles mappa
Hello `portalStyles` mappa tartalmazza a konfigurációs és stílus lapok developer portálon testreszabni hello szolgáltatáspéldány.

* `portalStyles\configuration.json`-hello fejlesztői portál által használt hello stíluslapok hello nevét tartalmazza
* `portalStyles\<style name>.css`-minden `<style name>.css` fájl tartalmazza a stílusokat hello fejlesztői portálján (`Preview.css` és `Production.css` alapértelmezés szerint).

### <a name="products-folder"></a>termékek mappa
Hello `products` mappa tartalmaz egy mappát az egyes termékek hello szolgáltatáspéldány definiálva.

* `products\<product name>\configuration.json`-hello termék hello beállítani. Ez a hello ugyanazokat az információkat, amely akkor tér vissza, ha toocall hello [beolvasni a termék](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) műveletet.
* `products\<product name>\product.description.html`-Ez hello termék hello leírása és toohello megfelel `description` hello tulajdonságának [termék entitás](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) a hello REST API-t.

### <a name="templates"></a>sablonok
Hello `templates` mappa hello konfigurációját tartalmazza [e-mail sablonok](api-management-howto-configure-notifications.md) hello szolgáltatás példányának.

* `<template name>\configuration.json`-hello e-mail sablon hello beállítani.
* `<template name>\body.html`-Ez az hello hello e-mail sablon szövegtörzsét.

## <a name="next-steps"></a>Következő lépések
Más módokon toomanage olvashat a szolgáltatáspéldány, lásd:

* A szolgáltatáspéldány hello a következő PowerShell-parancsmagok használatával kezelése
  * [Szolgáltatások üzembe helyezése – PowerShell-parancsmagok leírása](https://msdn.microsoft.com/library/azure/mt619282.aspx)
  * [Szolgáltatás-felügyelet PowerShell parancsmag-referencia](https://msdn.microsoft.com/library/azure/mt613507.aspx)
* A szolgáltatáspéldány hello publisher portálon kezelése
  * [Az első API kezelése](api-management-get-started.md)
* A szolgáltatáspéldány hello REST API használatával kezelése
  * [API Management REST API-referencia](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Áttekintő videó megtekintése
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Configuration-over-Git/player]
> 
> 

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




