---
title: "aaaExporting egy Azure által üzemeltetett API tooPowerApps és a Microsoft Flow |} Microsoft Docs"
description: "Hogyan tooexpose API App Service tooPowerApps és tárolt Microsoft Flow áttekintése"
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/20/2017
ms.author: mahender
ms.openlocfilehash: 285b6efa3af5b0feac1ee2f617c0dc56dc3fd198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exporting-an-azure-hosted-api-toopowerapps-and-microsoft-flow"></a>Az Azure által üzemeltetett API tooPowerApps és a Microsoft Flow exportálása

## <a name="creating-custom-connectors-for-powerapps-and-microsoft-flow"></a>Hozzon létre egy egyéni összekötők PowerApps és Microsoft Flow

[PowerApps](https://powerapps.com) kialakításához, és csatlakozzon a tooyour adatokat, és a platformok közötti használathoz egyéni üzleti alkalmazások szolgáltatás. [Microsoft Flow](https://flow.microsoft.com) könnyen tooautomate munkafolyamatok és az üzleti folyamatokat a kedvenc alkalmazások és szolgáltatások között. Számos beépített összekötők toodata móddal, mint az Office 365, Dynamics 365, Salesforce, PowerApps és Microsoft Flow is rendelkeznek. A felhasználóknak is kell toobe képes tooleverage adatforrások és a szervezet által létrehozott API-k.

Hasonlóképpen a fejlesztők tooexpose az API-további körben belül hello szervezet azt szeretné, hogy toomake a rendelkezésre álló tooPowerApps API-k és a Microsoft Flow felhasználók. Ez a témakör bemutatja, hogyan tooexpose egy API-t az Azure App Service szolgáltatásban vagy az Azure Functions tooPowerApps és a Microsoft Flow beépített. [Az Azure App Service](https://azure.microsoft.com/services/app-service/) van egy platform,--szolgáltatás ajánlat, amely lehetővé teszi a fejlesztők tooquickly és könnyen build vállalati szintű webes, mobil, és API-alkalmazások. [Az Azure Functions](https://azure.microsoft.com/services/functions/) egy kiszolgáló nélküli számítási esemény-alapú megoldás, amely lehetővé teszi, hogy a rendszer és a skála igény szerint tooother részei reagálhasson tooquickly Szerző kódot.

További információk a szolgáltatásokról, toolearn lásd:
- [PowerApps interaktív tanulási](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) 
- [Microsoft adatfolyam interaktív tanulási](https://flow.microsoft.com/guided-learning/learning-introducing-flow/)
- [Mi az App Service?](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- [Mi az Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview)

## <a name="sharing-an-api-definition"></a>Az API-definíció megosztása

API-k gyakran ismerteti a használatával egy [OpenAPI dokumentum](https://www.openapis.org/) (hívják tooas "Swagger" dokumentumot). Az összes milyen műveletek legyenek elérhetők, és hogyan hello adatszerkezet hello információt tartalmazza. PowerApps és Microsoft Flow OpenAPI 2.0 dokumentumtípus egyéni összekötőket hozhat létre. Egyéni összekötő létrehozása után használható a hello pontosan ugyanúgy, mint egy beépített összekötők hello és gyorsan integrálható egy alkalmazásba.

Az Azure App Service és az Azure Functions [beépített támogatást](https://docs.microsoft.com/azure/app-service-api/app-service-api-metadata) létrehozására, üzemeltető, és egy OpenAPI dokumentum kezelésére. Rendelés toocreate egyéni összekötő webes, mobil, API vagy függvény app PowerApps és Flow hello definícióját megadott toobe kell.

> [!NOTE]
> Mert hello API-definíció másolatának használatban van, PowerApps és Microsoft Flow azonnal tudta frissítések és a legfrissebb változtatásokat toohello alkalmazás. Hello API új verziója legyen elérhető, ha ezeket a lépéseket hello új verziójának meg kell ismételni. 

tooprovide PowerApps és a Microsoft Flow hello üzemeltetett API definition az alkalmazás, kövesse az alábbi lépéseket:

1. Nyissa meg hello [Azure Portal](https://portal.azure.com) , és keresse meg az App Service-vagy Azure Functions tooyour.

    Ha használja az Azure App Service, válassza ki a **API-definíció** hello beállítások listából. 
    
    Ha az Azure Functions használatával, jelölje ki a függvény alkalmazást, és válassza **Platform funkciói**, majd **API-definíció**. Sikerült meg tooopen hello **API definition (előzetes verzió)** lapon helyette.

2. Az API-definíció adtak meg, ha megjelenik egy **tooPowerApps + Microsoft Flow exportálása** gombra. Kattintson a gombra toobegin hello exportálás során.

3. Jelölje be hello **exportálási mód**. Meghatározza, hogy a hello lépések toofollow toocreate összekötőt. App Service biztosítható a PowerApps és Microsoft Flow az API-definíció két lehetőséget kínálja:

    **Express** készíthet hello egyéni összekötő belül hello Azure-portálon. Hello jelenlegi bejelentkezett rendelkezik engedéllyel toocreate összekötők hello célkörnyezet igényel. Ez az ajánlott módszert, ha érheti el, hogy ez a követelmény hello. Ha ezt a módot használ, kövesse a hello [exportálási Express](#express) az alábbi utasításokat.

    **Manuális** hello API definiton hello PowerApps és Microsoft Flow portálokon használatával importálható másolatát exportálás lehetővé. Ez az ajánlott megközelítést alkalmazva, ha hello Azure felhasználói és hello felhasználói engedélyt toocreate összekötőkkel más személyek vagy hello összekötő szüksége van egy másik bérlői létrehozott toobe hello. Ha ezt a módot használ, kövesse a hello [manuális exportálási és importálási](#manual) az alábbi utasításokat.

<a name="express"></a>
## <a name="express-export"></a>Exportálás Express

Ebben a szakaszban egy új egyéni összekötő hello Azure-portálon belül hoz létre. Be kell jelentkeznie a hello bérlői toowhich tooexport kívánja, és rendelkeznie kell engedéllyel toocreate egyéni összekötő hello cél környezetben.

1. Válassza ki, amelyben szeretné toocreate hello összekötő hello környezetben. Az egyéni összekötő nevét adja meg.

2. Ha az API-definíció bármilyen biztonsági definíciók tartalmaz, ezek neve a #2. Ha szükséges, részletesen hello biztonsági konfiguráció szükséges toogrant felhasználók hozzáférési tooyour API. További információkért lásd: [hitelesítési](#auth) alatt. 

3. Kattintson a **OK** toocreate az egyéni összekötő.


<a name="manual"></a>
## <a name="manual-export-and-import"></a>Manuális az exportálás és importálás

Rendelés toocreate egyéni összekötő webes, mobil, API vagy függvény app két lépésben lesz szükség:

1. [App Service vagy az Azure Functions hello API-definíció beolvasása](#export)
2. [PowerApps és Microsoft Flow hello API-definíció importálása](#import)

Akkor lehet, hogy ezeket a lépéseket két szükség van egy külön egyéni felhasználók számára a szervezeten belüli által végzett toobe egy adott felhasználó nem lehet engedélyt tooperform mindkét művelet. Ebben az esetben egy fejlesztő, akinek közreműködői hozzáférés toohello App Service-vagy Azure Functions tooobtain hello API definition (egyetlen JSON-fájl) vagy egy hivatkozás tooit szükséges. Hogy majd meg kell tooprovide adott definition tooa PowerApps vagy a Microsoft Flow tulajdonosa. A tulajdonos hello metaadatok toocreate hello egyéni összekötőt is használhatja.

<a name="export"></a>
### <a name="retrieving-hello-api-definition-from-app-service-or-azure-functions"></a>App Service vagy az Azure Functions hello API-definíció beolvasása

Ebben a szakaszban pedig exportálja majd hello API-definíció az App Service API toobe hello PowerApps vagy a Microsoft Flow portal szerepel.

1. Kiválaszthatja a tooeither **hello API-definíció letöltése** vagy **szerezzen be egy hivatkozást**. Attól úgy dönt, hello eredmény nyújtanak hello a következő szakaszban. Válassza ki a következő beállítások egyikét, és hello utasítások.
 
2. Ha az API-definíció bármilyen biztonsági definíciók tartalmaz, ezek neve a #2. Importálás során PowerApps és Microsoft Flow ezek észleli, és felszólítja a biztonsági információk. Gyűjtse össze a hello hitelesítő adatok kapcsolódó tooeach definition hello a következő szakaszban használható. További információkért lásd: [hitelesítési](#auth) alatt. 

<a name="import"></a>
### <a name="importing-hello-api-definition-into-powerapps-and-microsoft-flow"></a>PowerApps és Microsoft Flow hello API-definíció importálása

Ebben a szakaszban a PowerApps és a hello API definition korábban beszerzett Microsoft Flow egyéni összekötő hoz létre. Egyéni összekötők hello két szolgáltatást, így csak egyszer tooimport hello definition között vannak megosztva. Egyéni összekötők további információkért lásd: [regisztrálja, és egyéni összekötők a PowerApps] és [regisztrálja, és a Microsoft Flow egyéni összekötők].

1. Nyissa meg hello [Powerapps webes portál](https://web.powerapps.com) vagy hello [Microsoft Flow webes portál](https://flow.microsoft.com/), és jelentkezzen be. 

2. Hello kattintson **beállítások** hello lap, és válassza ki a hello felső sarokban (hello fogaskerék ikonra) gomb **egyéni összekötők**. 

3. Kattintson a **hozzon létre egyéni összekötő**.

4. A hello **általános** lapon adjon meg egy nevet az API-, és majd feltölteni hello OpenAPI definition vagy illessze be a hello metaadatainak URL-CÍMÉT. Kattintson a **továbbra is**.

4. A hello **biztonsági** lapon, ha felszólító tooprovide hitelesítési részletek hello előző szakaszban beszerzett hello értékeket. Ha nem, akkor folytassa a következő lépésre toohello.

5. A hello **definíciók** lapon, a OpenAPI fájlban meghatározott összes hello műveletek esetében, automatikus feltöltve. Ha a szükséges műveletek vannak meghatározva, lépjen toohello következő lépésre. Ha nem, akkor adja hozzá, és itt műveletek módosítása.

6. Kattintson a **-összekötő létrehozása**. Ha azt szeretné, hogy tootest API-hívások, lépjen a toohello következő lépésre.

7. A hello **teszt** lapon kapcsolatot, válasszon egy művelet tootest, és írjon be hello a művelethez szükséges adatokat.

8. Kattintson a **művelet tesztelése**.


<a name="auth"></a>
## <a name="authentication"></a>Authentication

PowerApps és Microsoft Flow natív módon támogatja az identitás-szolgáltatóktól, amely lehet a felhasználók az egyéni összekötője használt toolog gyűjteménye. Ha az API-hitelesítés szükséges, győződjön meg arról, mint a rögzítés egy _biztonsági definíciójának_ a OpenAPI dokumentumban. Exportálás során szüksége lesz a tooprovide konfigurációs értékeket, amelyek lehetővé teszik a powerapps segítségével egy Microsoft Flow tooperform bejelentkezési műveletek.

Ez a fejezet hello expressz folyamata által támogatott hello hitelesítési típusok: API key, az Azure Active Directory és a általános OAuth 2.0-s. Szolgáltatók és hello hitelesítő adatokat igényel teljes listáját lásd: [regisztrálja, és egyéni összekötők a PowerApps] és [regisztrálja, és a Microsoft Flow egyéni összekötők].

### <a name="api-key"></a>Az API-kulcs
A biztonsági rendszer használata esetén az összekötő hello felhasználók felszólító tooprovide hello kulcs lesz, ha hoznak létre a kapcsolat. Megadhat egy API-kulcsnév toohelp, azokat tudja, melyik kulcs szükséges. Az Azure Functions Ez általában lesz hello állomások kulcsait, kiterjedő hello függvény alkalmazáson belül több funkciók egyikét.

### <a name="azure-active-directory"></a>Azure Active Directory
Egy egyéni összekötő AAD bejelentkezési igénylő konfigurálásakor két AAD alkalmazás regisztrációk szükség: egy toomodel hello háttér-API és a PowerApps és Flow egy toomodel hello összekötőt.

Az API-t kell konfigurált toowork hello első regisztrációját, és ez lesz már kell venni megvagyunk hello használatakor [App Service hitelesítési/engedélyezési](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication) szolgáltatás.

Hogy hello második regisztráció hello összekötő létrehozása toomanually ismertetett hello lépésekkel [hozzáadása egy AAD-alkalmazást](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application). hello regisztrációs kell toohave meghatalmazott hozzáférést tooyour API és a válasz URL- `https://msmanaged-na.consent.azure-apim.net/redirect`. Ellenőrizze a [ebben a példában](
https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) részletesebben, és az API-t a hello Azure Resource Manager.

a következő konfigurációs értékeket hello szükség:
- **Ügyfél-azonosító** -ügyfél-azonosító az AAD-regisztrációs összekötője hello
- **Titkos ügyfélkulcsot** -hello titkos ügyfélkulcs az AAD-regisztrációs összekötője
- **Bejelentkezési URL-cím** – hello alap URL az aad-ben. A nyilvános Azure-ban ez általában lesz `https://login.windows.net`.
- **A bérlői azonosító** -hello hello bérlői toobe használt hello bejelentkezési azonosítója. Ez legyen "általános" vagy hello azonosító hello bérlő mely hello az összekötő létrehozása folyamatban van.
- **Forrás URL-cím** -erőforrás URL-címet a API háttéralkalmazás AAD regisztrációt hello

> [!IMPORTANT]
> Ha egy másik személlyel importálja a hello API definition PowerApps és Microsoft Flow hello manuális folyamat részeként, szüksége lesz a tooprovide őket a hello azonosító és az ügyfél ügyfélkulcs **hello összekötő regisztráció**, illetve hello erőforrás URL-címet a API. Győződjön meg arról, hogy ezeknek a kulcsoknak biztonságosan kezeli-e. **Hello maga API hello biztonsági hitelesítő adatokat nem osztják meg.**

### <a name="generic-oauth-20"></a>Általános OAuth 2.0
hello általános OAuth 2.0 támogatása lehetővé teszi bármely OAuth 2.0-s szolgáltatóval toointegrate. Ez lehetővé teszi toobring a bármely egyéni szolgáltató, amely natív módon nem támogatott.

a következő konfigurációs értékeket hello szükség:
- **Ügyfél-azonosító** -hello OAuth 2.0 ügyfél-azonosító
- **Titkos ügyfélkulcsot** -hello OAuth 2.0 ügyfél titkos kulcs
- **Engedélyezési URL-címet** -hello OAuth 2.0-engedélyezési URL-címe
- **A token URL-cím** -hello OAuth 2.0 token URL-címe
- **Frissítse az URL-cím** -hello OAuth 2.0-s frissítési URL-címe



[regisztrálja, és egyéni összekötők a PowerApps]: https://powerapps.microsoft.com/tutorials/register-custom-api/
[regisztrálja, és a Microsoft Flow egyéni összekötők]: https://flow.microsoft.com/documentation/register-custom-api/
