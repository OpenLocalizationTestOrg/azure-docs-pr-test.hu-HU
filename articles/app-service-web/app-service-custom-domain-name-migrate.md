---
title: "egy aktív DNS aaaMigrate tooAzure App Service name |} Microsoft Docs"
description: "Ismerje meg, hogyan toomigrate egy egyéni DNS-tartománynév, amely már hozzá van rendelve a tooa élő hely tooAzure App Service állásidő nélkül."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: cephalin
ms.openlocfilehash: fbc4cc30dcb87efb2e867cb65c5404b667661e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-an-active-dns-name-tooazure-app-service"></a>Telepítse át egy aktív DNS-név tooAzure App Service

Ez a cikk bemutatja, hogyan toomigrate egy aktív DNS neve túl[Azure App Service](../app-service/app-service-value-prop-what-is.md) állásidő nélkül.

Egy élő hely és a DNS tartomány neve tooApp szolgáltatás áttelepítésekor, hogy a DNS-név már kiszolgál élő forgalmat. DNS-feloldás az állásidő elkerülhetők hello az áttelepítés során megelőző jelleggel kötés hello aktív DNS nevét tooyour App Service-alkalmazást.

Ha nem aggódik DNS-feloldás az állásidő, lásd: [leképezése egy meglévő egyéni DNS nevét tooAzure webalkalmazások](app-service-web-tutorial-custom-domain.md).

## <a name="prerequisites"></a>Előfeltételek

Ez az útmutató toocomplete:

- [Győződjön meg arról, hogy az App Service alkalmazás nem ingyenes szint](app-service-web-tutorial-custom-domain.md#checkpricing).

## <a name="bind-hello-domain-name-preemptively"></a>Hello tartománynév megelőző jelleggel kötése

Az egyéni tartománynév megelőző jelleggel kötését, elérheti mindkét hello alábbiakat, mielőtt a DNS-rekordok módosítása:

- Ellenőrizze a tartomány tulajdonosa
- Az alkalmazás hello tartomány nevét engedélyezése

Végül a való áttelepítéskor a egyéni DNS-nevével hello régi hely toohello App Service-alkalmazást, lesz állásidő nélkül a DNS-feloldás eredményének.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a>Tartomány ellenőrzési rekord létrehozása

Jegyezze fel tooverify tartomány tulajdonosa, hozzáadása egy TXT. a maps hello TXT-rekord _awverify.&lt; altartomány >_ too_&lt;alkalmazásnév >. azurewebsites.net_. 

hello kell TXT-rekord hello toomigrate kívánt DNS-rekord függ. Tekintse meg a következő táblázat hello (`@` általában jelöli hello gyökértartomány):  

| DNS-rekord példa | TXT-állomás | TXT érték |
| - | - | - |
| @ (root) | _awverify_ | _&lt;alkalmazásnév >. azurewebsites.net_ |
| a webszolgáltatáshoz (rész) | _awverify.www_ | _&lt;alkalmazásnév >. azurewebsites.net_ |
| \*(helyettesítő) | _awverify.\*_ | _&lt;alkalmazásnév >. azurewebsites.net_ |

A DNS-rekordok lapon jegyezze fel hello rekord típusú hello toomigrate kívánt DNS-nevét. App Service leképezéseket a CNAME és A rekordok támogatja.

### <a name="enable-hello-domain-for-your-app"></a>Az alkalmazás hello tartomány engedélyezése

A hello [Azure-portálon](https://portal.azure.com), a bal oldali navigációs hello app lap hello, válassza ki **egyéni tartományok**. 

![Az egyéni tartomány menü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

A hello **egyéni tartományok** lapra, jelölje be hello  **+**  ikon következő túl**állomásnév hozzáadása**.

![Adja hozzá a gazdagép neve](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Típus hello teljesen minősített tartománynevét hello TXT rekord, például a hozzáadott `www.contoso.com`. Egy helyettesítő tartomány (például \*. contoso.com), minden DNS-neve, amely megfelel a hello helyettesítő tartomány is használhatja. 

Válassza ki **érvényesítése**.

Hello **állomásnév hozzáadása** gomb aktiválásakor. 

Győződjön meg arról, hogy **állomásnév rekordtípus** toohello DNS-rekord típusát toomigrate kívánt van beállítva.

Válassza ki **állomásnév hozzáadása**.

![DNS-név toohello alkalmazás hozzáadása](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Hello hello app kerülnek új állomásnév toobe némi időbe telhet **egyéni tartományok** lap. Próbálja meg frissíteni a hello böngésző tooupdate hello adatokat.

![CNAME rekord hozzáadva](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Az egyéni DNS-neve most már engedélyezve van az Azure alkalmazásban. 

## <a name="remap-hello-active-dns-name"></a>Adja meg újból hello aktív DNS-név

hello csak dolog bal oldali toodo van ismételt leképezését, az aktív DNS-rekord toopoint tooApp szolgáltatás. Jobb most, még mindig mutat tooyour régi helyről.

<a name="info"></a>

### <a name="copy-hello-apps-ip-address-a-record-only"></a>Másolja a hello app IP-cím (csak egy a rekordot)

Ha egy CNAME rekordot a rendszer újramegfeleltetése, hagyja ki az ebben a szakaszban. 

tooremap egy A rekordot kell hello App Service alkalmazás külső IP-címet, amely megjelenik-e a hello **egyéni tartományok** lap.

Bezárás hello **állomásnév hozzáadása** lap választásával **X** hello jobb felső sarokban. 

A hello **egyéni tartományok** lapján hello app IP-cím másolásához.

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-hello-dns-record"></a>Hello DNS-bejegyzésének frissítése

Hello DNS rekordok lap a tartomány szolgáltató válassza hello DNS-rekord tooremap.

A hello `contoso.com` : Példa a gyökérkönyvtár, megfeleltetheti hello A vagy CNAME rekord a következő táblázat hello hello példák mint: 

| FQDN-példa | Bejegyzéstípus | Gazdagép | Érték |
| - | - | - | - |
| a contoso.com (root) | A | `@` | IP-cím a [másolási hello app IP-cím](#info) |
| a www.contoso.com (rész) | CNAME | `www` | _&lt;alkalmazásnév >. azurewebsites.net_ |
| \*. contoso.com (helyettesítő) | CNAME | _\*_ | _&lt;alkalmazásnév >. azurewebsites.net_ |

A beállítások mentéséhez.

DNS-lekérdezések feloldása tooyour App Service-alkalmazást, akkor fordul elő, DNS-propagálás után azonnal kell kezdődnie.

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan toobind egyéni SSL tanúsítvány tooApp szolgáltatás.

> [!div class="nextstepaction"]
> [Egy meglévő egyéni SSL tanúsítvány tooAzure webalkalmazások kötése](app-service-web-tutorial-custom-ssl.md)
