---
title: "Az Azure operational biztonsági ellenőrzőlista |} Microsoft Docs"
description: "Ez a cikk számos ellenőrzőlista Azure működési biztonság."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: de225fde09665f25b326f4012ff0452ab6cef83b
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/08/2017
---
# <a name="azure-operational-security-checklist"></a>Az Azure operational biztonsági ellenőrzőlista
Gyors, egyszerű és költséghatékony Azure-alkalmazás központi telepítése. Felhő alkalmazás hasznos lehet egy ellenőrzőlista éles környezetben az alkalmazást egy listát az alapvető és ajánlott a működési biztonság műveletek figyelembe kell venni kiértékelésekor segít üzembe helyezése előtt.

## <a name="introduction"></a>Bevezetés

Azure infrastruktúra-szolgáltatásokat, amelyek segítségével az alkalmazások központi telepítése együttesét nyújtja. Az Azure Operational biztonsági hivatkozik a szolgáltatások, a vezérlők és a felhasználók számára elérhető szolgáltatások védelmére az adatok, alkalmazások és egyéb eszközök a Microsoft Azure-ban.

-   Ahhoz, hogy a lehető legjobban a felhőalapú platform kívül, azt javasoljuk, hogy kihasználja az Azure-szolgáltatásokat, és hajtsa végre az ellenőrzőlista.
-   Szervezeteknek, amelyek fektetnek időt és erőforrásokat, az alkalmazások indítás előtt a működőképesség felmérése elégedettségének sokkal nagyobb aránya, mint azok, akik nem rendelkeznek. A munkaelem végrehajtásakor ellenőrzőlisták lehet egy olyan hasznos információt mechanizmust győződjön meg arról, hogy következetesen és holistically kiértékelése alkalmazások.
-   Értékelési működési szintjét a szervezete cloud érettségi szintjétől és az alkalmazást fejleszt, rendelkezésre állási igényeinek és érzékenységi követelmények fog függ.

## <a name="checklist"></a>Feladatlista

Az alábbi ellenőrzőlista célja a vállalatok gondolja, hogy a különböző működési biztonsági szempontjait, azok Azure kifinomult vállalati alkalmazások telepítése. Is használható a szervezet biztonságos felhőalapú áttelepítési és művelet stratégia létrehozásához.

|Feladatlista kategória| Leírás|
| ------------ | -------- |
| [<br>Biztonsági szerepkörök és hozzáférés-vezérlést](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)|<ul><li>Használjon [szerepköralapú hozzáférés-vezérlést (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) arra, hogy felhasználó által megadott, amellyel az engedélyek hozzárendelése a felhasználók, csoportok és alkalmazások egy adott hatókörben.</li></ul> |
| [<br>Adatgyűjtés & tárolás](https://docs.microsoft.com/azure/storage/storage-security-guide)|<ul><li>A Storage-fiók használatával biztonságos felügyeleti Vezérlősík biztonsági segítségével [szerepköralapú hozzáférés-vezérlést (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).</li><li>Adatbiztonság Vezérlősík hozzáférés biztonságossá tétele az adatok használatával [megosztott hozzáférési aláírásokkal (SAS)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) és a hozzáférési házirendek tárolja.</li><li>Átviteli szintű titkosítást – HTTPS és a által használt titkosítási [SMB (Server message block protokollok) 3.0](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) a [Azure fájlmegosztásokat](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-files).</li><li>Használjon [ügyféloldali titkosítás](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) tárfiókok Ha titkosítási kulcsok egyetlen irányítását küldött adatok védelmét. </li><li>Használjon [Storage Service Encryption (SSE)](https://docs.microsoft.com/azure/storage/storage-service-encryption) titkosítani az adatokat az Azure Storage és [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) virtuális gépek lemezfájljainak a lemezeket, az operációs rendszer és az adatok titkosításához.</li><li>Használja az Azure [tárolási analitika](https://docs.microsoft.com/rest/api/storageservices/storage-analytics) engedélyezési figyelése típust; például a Blob-tároló, láthatja, ha a felhasználók egy közös hozzáférésű Jogosultságkód vagy a tárfiókkulcsokat használt.</li><li>Használjon [Cross-Origin Resource Sharing (CORS)](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) más tartományokból tárolási erőforrások eléréséhez.</li></ul> |
|[<br>Biztonsági szabályzatok és javaslatok](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)|<ul><li>Használjon [az Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-install-endpoint-protection) végpont megoldások telepítéséhez.</li><li>Adja hozzá a [webalkalmazási tűzfal (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) biztonságos webes alkalmazásokhoz.</li><li>  Használja a [új generációs tűzfal (NGFW)](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall) egy Microsoft-partner növelje a biztonsági védelmet. </li><li>Alkalmazza a biztonsági kapcsolattartási adatait az Azure-előfizetéshez; Ez a [Microsoft Security Response Center (MSRC)](https://technet.microsoft.com/security/dn528958.aspx) felveszi a kapcsolatot, ha azt észleli, hogy az az ügyféladatok egy törvénybe ütköző vagy jogosulatlan félnek elérhető-e.</li></ul> |
| [<br>Identity & Access Management szolgáltatáshoz](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices)|<ul><li>[A helyszíni címtárral szinkronizálja az Azure AD használatával a felhő címtárral](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).</li><li>Használjon [egyszeri bejelentkezés](https://azure.microsoft.com/resources/videos/overview-of-single-sign-on/) engedélyezése a felhasználók hozzáférhessenek az SaaS-alkalmazásokhoz az Azure ad-ben a saját szervezeti fiókjukba alapján.</li><li>Használja a [jelszó alaphelyzetbe állítása regisztrálási tevékenységét](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights) jelentés figyelheti a felhasználókat, akik próbál regisztrálni.</li><li>Engedélyezése [többtényezős hitelesítés (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) a felhasználók számára.</li><li>Biztonságos identitási képességeibe használandó alkalmazásokhoz, például a fejlesztők [Microsoft biztonságos fejlesztési Életciklussal (SDL)](https://www.microsoft.com/download/details.aspx?id=12379).</li><li>Aktívan figyelje a gyanús tevékenységek Azure AD Premium anomáliabiztonsági jelentéseket és [az Azure AD identity protection funkció](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection).</li></ul> |
|[<br>A biztonság folyamatos ellenőrzése](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)|<ul><li>Kártevő szoftver Assessment megoldással [Naplóelemzési](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) szembeni védelem a infrastruktúrában állapotának jelentést.</li><li>Használja [értékelés frissítése](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management) potenciális biztonsági problémákat, általános kitéve meghatározásához és kell-e, vagy hogyan kritikus ezeket a frissítéseket a környezetnek.</li><li>A [identitások és hozzáférések](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) felhasználó áttekintés biztosítása </li><ul><li>felhasználói azonosító állapot</li><li>Jelentkezzen be, hogy a sikertelen kísérletek száma</li><li>    a felhasználói fiókokat, amelyek ki lettek zárva, próbálkozások során használt fiók</li> <li>fiók megváltozott, vagy a jelszó alaphelyzetbe állítása </li><li>Jelenleg fiókok számát naplózza.</li></ul></ul> |
| [<br>Az Azure Security Center az észlelési képességek](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities)|<ul><li>Használjon [az észlelési képességek](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities), a Microsoft Azure-erőforrások célzó aktív fenyegetések azonosítására.</li><li>Használjon [fenyegetésfelderítési adataival integrált](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/19/get-threat-intelligence-reports-with-azure-security-center/) , amely megkeresi ismert ezeken a Microsoft termékeinek és szolgáltatásainak, globális fenyegetésfelderítési adataival, ami a [Microsoft Digital Crimes Unit (DCU)](https://www.microsoft.com/stories/cyber.aspx), a [ Microsoft Security Response Center (MSRC)](https://docs.microsoft.com/azure/security/azure-security-response-center), és a külső hírcsatornák.</li><li>Használjon [viselkedéselemzés](https://blogs.technet.microsoft.com/enterprisemobility/2016/06/30/ata-behavior-analysis-monitoring/) , amelyek kártékony felderítéséhez ismert mintákat vonatkozik. </li><li>Használjon [anomáliadetektálás](https://msdn.microsoft.com/library/azure/dn913096.aspx) hozhat létre egy kiinduló használó a statisztikai adatainak összegyűjtése.</li></ul> |
| [<br>Fejlesztői műveletek (DevOps)](https://docs.microsoft.com/azure/architecture/checklist/dev-ops)|<ul><li>[Szolgáltatott infrastruktúra (IaC) kód](https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) eljárás, amely lehetővé teszi az automatizálás és létrehozásának érvényesítése és lebontás hálózatok és virtuális gépeket futtató rendszerek biztonságos, stabil alkalmazás továbbítása érdekében.</li><li>[Folyamatos integrációt és telepítést](https://www.visualstudio.com/docs/build/overview) a folyamatban lévő egyesítése és a kódot, ami hibák keresése korai vizsgálatát. </li><li>[Kiadáskezelés](https://msdn.microsoft.com/library/vs/alm/release/overview) kezelése automatikus a folyamat minden lépése környezeteit.</li><li>[Alkalmazás-teljesítményfigyelési](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) futó alkalmazások, beleértve az éles környezetben az alkalmazás állapotának, csakúgy, mint az ügyfél használati súgó szervezetek űrlap egy alapul, és gyorsan ellenőrzése vagy stratégiák disprove.</li><li>Használatával [terhelés tesztelés & automatikus méretezése](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) az alkalmazás központi telepítési minőségének javítására is találtunk teljesítménybeli problémákat, és ellenőrizze, hogy az alkalmazás mindig naprakészek vagy elérhető elemeket, az üzleti kell.</li></ul> |


## <a name="conclusion"></a>Összegzés
Számos szervezet sikeresen telepített és az Azure felhőalapú alkalmazások üzemeltetett. A megadott ellenőrzőlista jelöljön ki több feladatlistákra is, amelyek alapvető, és segítséget nyújtson sikeres központi telepítések és eljárás megzavarását szabad műveletek valószínűségének növelésére. Erősen ajánlott a működési és a stratégiai szempontok a meglévő és új alkalmazások központi telepítése az Azure-on.

## <a name="next-steps"></a>Következő lépések
E dokumentumban az OMS biztonsági és auditálási megoldást mutattuk be. Az OMS által nyújtott védelemmel kapcsolatos további információkat a következő cikkek tartalmaznak:

- [Az Operations Management Suite (OMS) – áttekintés](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).
- [Tervezési és a működési biztonság](https://www.microsoft.com/trustcenter/security/designopsecurity).
- [Azure Security Center tervezéséhez és műveletek](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide).