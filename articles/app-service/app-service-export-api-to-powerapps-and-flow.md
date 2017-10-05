---
title: "Az Azure által üzemeltetett API-k exportálása a PowerApps és Microsoft Flow |} Microsoft Docs"
description: "Hogyan teszi közzé a PowerApps és Microsoft Flow App Service-ben üzemeltetett API"
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
ms.openlocfilehash: 0d166a2e5b60c3b9f911f9fc3ad49ad7f252abb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="exporting-an-azure-hosted-api-to-powerapps-and-microsoft-flow"></a>Az Azure által üzemeltetett API-k PowerApps és Microsoft Flow exportálása

## <a name="creating-custom-connectors-for-powerapps-and-microsoft-flow"></a>Hozzon létre egy egyéni összekötők PowerApps és Microsoft Flow

[PowerApps](https://powerapps.com) kialakításához, és kapcsolódhat adataihoz, és a platformok közötti használathoz egyéni üzleti alkalmazások szolgáltatás. [Microsoft Flow](https://flow.microsoft.com) megkönnyíti a munkafolyamatok és a kedvenc alkalmazások és szolgáltatások közötti üzleti folyamatok automatizálásához. PowerApps és Microsoft Flow is rendelkeznek beépített összekötők különböző adatforrásokhoz, mint az Office 365, Dynamics 365, Salesforce. Azonban felhasználók is kell adatforrások és a szervezet által létrehozott API-k képesek.

Hasonló módon teszi közzé az API-szélesebb körben a szervezeten belül a fejlesztők is elérhetővé kíván tenni az API-PowerApps és Microsoft Flow felhasználók számára. Ez a témakör bemutatja, hogyan teszi közzé az API-k Azure App Service szolgáltatásban vagy az Azure Functions PowerApps és Microsoft Flow-val készült. [Az Azure App Service](https://azure.microsoft.com/services/app-service/) van egy platform,--szolgáltatás ajánlat, amely lehetővé teszi a fejlesztők gyorsan és egyszerűen vállalati szintű webes, mobil és API-alkalmazások. [Az Azure Functions](https://azure.microsoft.com/services/functions/) egy kiszolgáló nélküli számítási esemény-alapú megoldás, amely lehetővé teszi, hogy gyorsan reagálhasson más részekkel a rendszer és a skála igény szerint Szerző kódot.

A szolgáltatásokkal kapcsolatos további tudnivalókért lásd:
- [PowerApps interaktív tanulási](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) 
- [Microsoft adatfolyam interaktív tanulási](https://flow.microsoft.com/guided-learning/learning-introducing-flow/)
- [Mi az App Service?](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- [Mi az Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview)

## <a name="sharing-an-api-definition"></a>Az API-definíció megosztása

API-k gyakran ismerteti a használatával egy [OpenAPI dokumentum](https://www.openapis.org/) (más néven "Swagger" dokumentumot). Milyen műveletek legyenek elérhetők, és hogyan a adatszerkezet kapcsolatos adatokat tartalmazza. PowerApps és Microsoft Flow OpenAPI 2.0 dokumentumtípus egyéni összekötőket hozhat létre. Egyéni összekötő létrehozása után is használható pontosan ugyanúgy, mint a beépített összekötők egyike, és gyorsan integrálható egy alkalmazásba.

Az Azure App Service és az Azure Functions [beépített támogatást](https://docs.microsoft.com/azure/app-service-api/app-service-api-metadata) létrehozására, üzemeltető, és egy OpenAPI dokumentum kezelésére. Egyéni webes, mobil-összekötő, API vagy függvény alkalmazás létrehozása, PowerApps és Flow kell adni a definícióját.

> [!NOTE]
> Az API-definíció másolatának használatban van, mert PowerApps és Microsoft Flow azonnal tudta frissítések vagy jelentős változásokat az alkalmazáshoz. Az API-t egy új verziója legyen elérhető, ha ezeket a lépéseket az új verzióhoz tartozó meg kell ismételni. 

Az alkalmazás üzemeltetett API-definícióval PowerApps és Microsoft Flow megadásához kövesse az alábbi lépéseket:

1. Nyissa meg a [Azure Portal](https://portal.azure.com) , és keresse meg az App Service vagy az Azure Functions alkalmazás.

    Ha használja az Azure App Service, válassza ki a **API-definíció** beállítások listájáról. 
    
    Ha az Azure Functions használatával, jelölje ki a függvény alkalmazást, és válassza **Platform funkciói**, majd **API-definíció**. Sikerült meg nyissa meg a **API definition (előzetes verzió)** lapon helyette.

2. Az API-definíció adtak meg, ha megjelenik egy **exportálja a PowerApps és Microsoft Flow** gombra. Kattintson erre a gombra kattintva az exportálási folyamat megkezdéséhez.

3. Válassza ki a **exportálási mód**. Ez határozza meg a szüksége lesz egy összekötő létrehozásához kövesse a lépéseket. App Service biztosítható a PowerApps és Microsoft Flow az API-definíció két lehetőséget kínálja:

    **Express** készíthet az egyéni összekötő az Azure portálon. Ehhez szükséges, hogy a jelenlegi bejelentkezett felhasználó jogosult-célkörnyezetben összekötők létrehozásához. Ez a javasolt, hogy úgy érheti el. Ha ezt a módot használ, kövesse a [exportálási Express](#express) az alábbi utasításokat.

    **Manuális** lehetővé kell exportálnia az API definiton a PowerApps és Microsoft Flow portálokon használatával importálható egy példányát. Ez az az ajánlott módszer, ha az Azure felhasználói és összekötők létrehozásához engedéllyel rendelkező felhasználók más személyek vagy az összekötő kell létrehozni egy másik-bérlőben. Ha ezt a módot használ, kövesse a [manuális exportálási és importálási](#manual) az alábbi utasításokat.

<a name="express"></a>
## <a name="express-export"></a>Exportálás Express

Ebben a szakaszban egy új egyéni összekötő az Azure portálon hoz létre. Az exportálni kívánt, és a célkörnyezet egyéni összekötő létrehozásához engedéllyel kell rendelkeznie a bérlő kell bejelentkeznie.

1. Válassza ki a környezetben, ahol az összekötőt a létrehozás szeretné. Az egyéni összekötő nevét adja meg.

2. Ha az API-definíció bármilyen biztonsági definíciók tartalmaz, ezek neve a #2. Szükség esetén adja meg az API-hoz való hozzáférés engedélyezése a felhasználók számára szükséges biztonsági konfigurációs adatait. További információkért lásd: [hitelesítési](#auth) alatt. 

3. Kattintson a **OK** az egyéni összekötő létrehozásához.


<a name="manual"></a>
## <a name="manual-export-and-import"></a>Manuális az exportálás és importálás

Egy egyéni összekötő webes, mobil, API vagy függvény alkalmazás létrehozásához szükséges két lépésből áll:

1. [App Service vagy az Azure Functions az API-definíció beolvasása](#export)
2. [Az API-definíció importálása a PowerApps és Microsoft Flow](#import)

Akkor lehet, hogy ezt a két lépést kell külön egyéni felhasználók számára a szervezeten belül végzi egy adott felhasználó lehet, hogy rendelkezik engedéllyel mindkét művelet elvégzéséhez. Ebben az esetben egy fejlesztő, akinek közreműködői hozzáférni az App Service vagy az Azure Functions alkalmazáshoz kell kérnie az API-definíció (egyetlen JSON-fájl) vagy egy hivatkozásra. Hogy majd meg kell, hogy definíció biztosítása a powerapps segítségével vagy a Microsoft Flow tulajdonosa. A tulajdonos a metaadatok használatával hozhat létre egyéni összekötőt.

<a name="export"></a>
### <a name="retrieving-the-api-definition-from-app-service-or-azure-functions"></a>App Service vagy az Azure Functions az API-definíció beolvasása

Ebben a szakaszban pedig exportálja majd az App Service API-, később a powerapps segítségével vagy a Microsoft Flow portálon használandó API-definíció.

1. Dönthet úgy, hogy **az API-definíció letöltése** vagy **szerezzen be egy hivatkozást**. Attól választja, az eredmény biztosítja a következő szakaszban. Válassza ki a következő beállítások egyikét, és kövesse az utasításokat.
 
2. Ha az API-definíció bármilyen biztonsági definíciók tartalmaz, ezek neve a #2. Importálás során PowerApps és Microsoft Flow ezek észleli, és felszólítja a biztonsági információk. Gyűjtse össze a hitelesítő adatokat használja a következő szakasz mindegyik definíciója kapcsolódik. További információkért lásd: [hitelesítési](#auth) alatt. 

<a name="import"></a>
### <a name="importing-the-api-definition-into-powerapps-and-microsoft-flow"></a>Az API-definíció importálása a PowerApps és Microsoft Flow

Ebben a szakaszban egy egyéni összekötő PowerApps és a Microsoft Flow korábban beszerzett API-definíció használatával hoz létre. Egyéni összekötők a két szolgáltatás, így csak egyszer a-definíciójának importálásához között vannak megosztva. Egyéni összekötők további információkért lásd: [regisztrálja, és egyéni összekötők a PowerApps] és [regisztrálja, és a Microsoft Flow egyéni összekötők].

1. Nyissa meg a [Powerapps webes portál](https://web.powerapps.com) vagy a [Microsoft Flow webes portál](https://flow.microsoft.com/), és jelentkezzen be. 

2. Kattintson a **beállítások** (fogaskerék ikonra) gombra a képernyő jobb felső sarkában a lapot, és válasszon **egyéni összekötők**. 

3. Kattintson a **hozzon létre egyéni összekötő**.

4. Az a **általános** lapon adjon meg egy nevet az API-, és majd töltse fel a OpenAPI definition vagy illessze be a metaadatainak URL-CÍMÉT. Kattintson a **továbbra is**.

4. Az a **biztonsági** lapon, ha a hitelesítési részletesen, adja meg az értékeket az előző szakaszban beszerzett kéri. Ha nem, akkor folytassa a következő lépéssel.

5. Az a **definíciók** lapon, a OpenAPI fájlban meghatározott összes műveletek esetében, automatikus feltöltve. Ha a szükséges műveletek vannak meghatározva, megnyithatja a következő lépéssel. Ha nem, akkor adja hozzá, és itt műveletek módosítása.

6. Kattintson a **-összekötő létrehozása**. Ha szeretné tesztelni az API-hívásokban, nyissa meg a következő lépéssel.

7. Az a **tesztelése** lapon kapcsolatot, válasszon ki egy műveletet tesztelése, és adja meg a művelethez szükséges adatokat.

8. Kattintson a **művelet tesztelése**.


<a name="auth"></a>
## <a name="authentication"></a>Authentication

PowerApps és Microsoft Flow natív módon támogatja a felhasználók az egyéni összekötője bejelentkezéshez használható identitás-szolgáltatóktól gyűjteménye. Ha az API-hitelesítés szükséges, győződjön meg arról, mint a rögzítés egy _biztonsági definíciójának_ a OpenAPI dokumentumban. Exportálás során meg kell adnia a konfigurációs értékek, amelyek lehetővé teszik a powerapps segítségével egy Microsoft Flow bejelentkezési műveletek elvégzéséhez.

Ez a fejezet az express folyamat által támogatott hitelesítési típusok: API key, az Azure Active Directory és a általános OAuth 2.0-s. Szolgáltatók és a hitelesítő adatokat igényel teljes listáját lásd: [regisztrálja, és egyéni összekötők a PowerApps] és [regisztrálja, és a Microsoft Flow egyéni összekötők].

### <a name="api-key"></a>Az API-kulcs
A biztonsági rendszer használata esetén az összekötő a felhasználók felszólítást kapnak a kulcs megadására, amikor azok a kapcsolat létrehozása. Megadhatja az API-kulcsnév felhasználóinál tudja, melyik kulcs szükséges. Az Azure Functions Ez általában lesz az állomások kulcsait, a függvény alkalmazásban több funkciók kiterjedő egyikét.

### <a name="azure-active-directory"></a>Azure Active Directory
Egy egyéni összekötő AAD bejelentkezési igénylő konfigurálásakor két AAD alkalmazás regisztrációk szükség: egy, a háttér-API modellek és egy modellezésére a PowerApps és Flow-összekötőjét.

Az API-t konfigurálni kell, hogy az első regisztráció dolgozni, és ez lesz már kell venni megvagyunk Ha követte a [App Service hitelesítési/engedélyezési](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication) szolgáltatás.

Kell létrehoznia az összekötőt, a második regisztrálása ismertetett lépéseket követve [hozzáadása egy AAD-alkalmazást](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application). Delegált az API-t és a válasz URL-hozzáféréssel kell rendelkeznie kell a regisztrációt `https://msmanaged-na.consent.azure-apim.net/redirect`. Ellenőrizze a [ebben a példában](
https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) részletesebben, és az API-t az Azure Resource Manager.

A következő konfigurációs értékeket szükségesek:
- **Ügyfél-azonosító** – az ügyfél-Azonosítót az AAD-regisztrációs összekötője
- **Titkos ügyfélkulcsot** – a titkos ügyfélkulcs az AAD-regisztrációs összekötője
- **Bejelentkezési URL-cím** -aad alap URL-címet. A nyilvános Azure-ban ez általában lesz `https://login.windows.net`.
- **A bérlői azonosító** – a bérlő által használt bejelentkezési azonosító. Ez legyen "általános" vagy az azonosító a bérlő, amelyben az összekötő létrehozása folyamatban van.
- **Forrás URL-cím** -a erőforrás URL-címet a API háttéralkalmazás AAD regisztrációt

> [!IMPORTANT]
> Ha egy másik személlyel importálja az API-definíció PowerApps és Microsoft Flow a manuális folyamat részeként, szüksége lesz szeretne biztosítani nekik az ügyfél-azonosító és a titkos ügyfélkódot **az összekötő-regisztrációk**, valamint a az API-erőforrás URL-címe Győződjön meg arról, hogy ezeknek a kulcsoknak biztonságosan kezeli-e. **Az API-t a biztonsági hitelesítő adatokat nem osztják meg.**

### <a name="generic-oauth-20"></a>Általános OAuth 2.0
Az általános OAuth 2.0 támogatása lehetővé teszi bármely OAuth 2.0-s szolgáltató integrálható. Ez lehetővé teszi, hogy kapcsolja a bármely egyéni szolgáltató, amely natív módon nem támogatott.

A következő konfigurációs értékeket szükségesek:
- **Ügyfél-azonosító** -az OAuth 2.0 ügyfél-azonosító
- **Titkos ügyfélkulcsot** -az OAuth 2.0 ügyfél titkos kulcs
- **Engedélyezési URL-címet** -az OAuth 2.0-engedélyezési URL-cím
- **A token URL-cím** -OAuth 2.0 token URL-címe
- **Frissítse az URL-cím** -az OAuth 2.0-s frissítési URL-cím



[regisztrálja, és egyéni összekötők a PowerApps]: https://powerapps.microsoft.com/tutorials/register-custom-api/
[regisztrálja, és a Microsoft Flow egyéni összekötők]: https://flow.microsoft.com/documentation/register-custom-api/
