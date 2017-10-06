---
title: "Alkalmazásproxy aaaTroubleshoot |} Microsoft Docs"
description: "Magában foglalja az hogyan tootroubleshoot hibák az Azure AD alkalmazásproxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 970caafb-40b8-483c-bb46-c8b032a4fb74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: d426708a2ee32da6674987b5794602ed984b06b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-application-proxy-problems-and-error-messages"></a>Alkalmazásproxy problémák és hibaüzenetek hibaelhárítása
Ha hiba történik, a közzétett alkalmazás eléréséhez, vagy az alkalmazás-közzététel, ellenőrizze, ha a Microsoft Azure AD-alkalmazásproxy megfelelően működik-e a következő beállítások toosee hello:

* Nyissa meg hello központi Windows-szolgáltatások konzolon, és győződjön meg arról, hogy hello **Microsoft AAD alkalmazásproxy-összekötő** szolgáltatás engedélyezve van és fut. Is érdemes lehet toolook hello alkalmazásproxy szolgáltatás tulajdonságai lapon látható hello kép a következő módon:  
  ![Microsoft AAD Application Proxy Connector tulajdonságai ablakban képernyőképe](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)
* Nyissa meg az eseménynaplót, és keresse meg az Application Proxy connector-események **alkalmazási és Szolgáltatásnaplójában** > **Microsoft** > **AadApplicationProxy**  >  **Összekötő** > **Admin**.
* Szükség esetén további részletes érhetők el naplók által [bekapcsolásával hello Application Proxy connector munkamenet naplók](application-proxy-understand-connectors.md#under-the-hood).

Az Azure AD hello hibaelhárítás eszközzel kapcsolatos további információkért lásd: [eszköz toovalidate összekötő hálózati Előfeltételek hibaelhárítási](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/03/troubleshooting-tool-to-validate-connector-networking-prerequisites).

## <a name="hello-page-is-not-rendered-correctly"></a>hello lap nem megfelelően megjelenítése
Előfordulhat, hogy az alkalmazás megjelenítése, vagy nem megfelelően működik-e anélkül vonatkozó hibaüzeneteket. Ez akkor fordulhat elő, ha közzétett hello elérési útja, de az alkalmazáshoz hello kívül adott elérési úton található tartalom.

Például ha hello elérési https://yourapp/app közzéteszi, de hello alkalmazás képek meghívja a https://yourapp/media, akkor nem jeleníthető meg. Ellenőrizze, hogy közzé hello alkalmazás hello legmagasabb szintű elérési út használatával tooinclude minden kapcsolódó tartalom van szüksége. Ebben a példában http://yourapp/ lenne.

Ha az elérési út tooinclude hivatkozott tartalom módosítását, de továbbra is a felhasználók tooland mélyebb hivatkozásra a hello elérési útja kell, lásd: hello blogbejegyzésben [beállítás hello jobb oldali hivatkozást alkalmazásproxy-alkalmazások hello Azure AD hozzáférési panel és az Office 365-alkalmazás indítója](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="connector-errors"></a>Összekötő hibák

Használjon hello [az Azure AD Application Proxy Connector portok teszt eszközét](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify, hogy az összekötő érni hello alkalmazásproxy-szolgáltatás. Minimális győződjön meg arról, hogy hello központi US régió és hello régió legközelebbi tooyou összes zöld jelölők. Túl további zöld jelölők azt jelenti, hogy nagyobb rugalmasság. 

Regisztrációs hello összekötő varázsló telepítés során nem sikerül, ha két módon hello hiba tooview hello okát. Vagy keressen a hello eseménynaplóban **alkalmazások és szolgáltatások Logs\Microsoft\AadApplicationProxy\Connector\Admin**, vagy a következő Windows PowerShell-parancs futtatása hello:

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

Amikor hello összekötő hiba hello eseménynaplójából megkereséséhez használja az ebben a táblában a gyakori hibák tooresolve hello probléma:

| Hiba | Javasolt lépések |
| ----- | ----------------- |
| Összekötő regisztrálása nem sikerült: Ellenőrizze, hogy engedélyezte-proxyt hello Azure felügyeleti portálon, és meg helyesen az Active Directory-felhasználónevet és jelszót. Hiba: "egy vagy több hiba történt." | Ha korábban bezárta hello ablakban tooAzure AD bejelentkezés nélkül, futtassa újra a hello összekötő varázslóban, és hello összekötő regisztrálása. <br><br> Hello regisztrációs ablak nyílik meg, és azonnal bezárása után, hogy lehetővé teszi a toolog nélkül, ha a hiba valószínűleg jelenik meg. Ez a hiba akkor fordul elő, amikor egy hálózati hiba van a rendszeren. Győződjön meg arról, hogy a böngésző tooa nyilvános webhelyén lehetséges tooconnect és, hogy hello portjai nyitva a-e [proxyval Előfeltételek](active-directory-application-proxy-enable.md). |
| Törölje a jelet hiba hello regisztrációs ablakban jelenik meg. Nem lehet folytatni | Ha ezt a hibaüzenetet látja, és ezután hello időszak véget ér, a megadott hello rossz felhasználónévvel vagy jelszóval. próbáld újra. |
| Összekötő regisztrálása nem sikerült: Ellenőrizze, hogy engedélyezte-proxyt hello Azure felügyeleti portálon, és meg helyesen az Active Directory-felhasználónevet és jelszót. Hiba: "AADSTS50059: bérlői azonosító adatokat található bármelyik hello kérelemben vagy hallgatólagos bármelyik megadott hitelesítő adatok és a keresési szolgáltatás által egyszerű URI sikertelen volt. | Toosign próbált egy Microsoft Account és a nem tartományhoz, amely hello Szervezetazonosító hello könyvtár része a kívánt tooaccess. Győződjön meg arról, hogy Üdvözöljük a rendszergazdákat hello része tartomány neve megegyezik a hello bérlői tartományhoz, például ha hello Azure AD-tartomány pedig contoso.com, Üdvözöljük a rendszergazdákat legyen admin@contoso.com. |
| Nem sikerült tooretrieve hello aktuális futtatására vonatkozó végrehajtási házirend PowerShell-parancsfájlokat. | Hello Connector telepítése nem sikerül, ha ellenőrizze toomake meg arról, hogy a PowerShell végrehajtási házirend nincs letiltva. <br><br>1. Nyissa meg a Helyicsoportházirend-szerkesztő hello.<br>2. Nyissa meg túl**számítógép konfigurációja** > **felügyeleti sablonok** > **Windows-összetevők**  >   **A Windows PowerShell** duplán **kapcsolja be a parancsfájl végrehajtása**.<br>3. hello végrehajtási házirendet állíthat be tooeither **nincs konfigurálva** vagy **engedélyezve**. Ha túl beállítása**engedélyezve**, győződjön meg arról, hogy a beállítások, hello végrehajtási házirend értéke tooeither **engedélyezése a helyi és távoli aláírt parancsfájlok** vagy túl**összes parancsfájlok**. |
| Összekötő toodownload hello konfigurálása nem sikerült. | hello összekötő ügyféltanúsítványt, a hitelesítéshez használt, lejárt. Ez akkor is előfordulhat, ha hello összekötő telepítve, a rendszer proxy mögött van. Ebben az esetben hello összekötő hello Internet nem érhető el, és nem lesz képes tooprovide alkalmazások tooremote felhasználók. Megbízhatósági hello segítségével manuálisan megújítani `Register-AppProxyConnector` a Windows PowerShell parancsmag. Ha az összekötő a rendszer proxy mögött van, a rendszer szükséges toogrant Internet access toohello összekötő fiókok "hálózati szolgáltatás" és "helyi rendszer". Ehhez megadásával toohello Proxy azok eléréséhez, vagy úgy, hogy azok toobypass hello proxy. |
| Összekötő regisztrálása nem sikerült: Győződjön meg arról, hogy az Active Directory tooregister hello összekötő a globális rendszergazdai jogosultságokkal. Hiba: "hello regisztrációs kérelem visszautasítva." | hello alias be toolog próbált nem található a tartomány rendszergazdája. Az összekötő hello directory hello felhasználói tartomány birtokló mindig telepítve van. Győződjön meg arról, hogy hello rendszergazdai fiók be toosign toohello az Azure AD-bérlő globális engedélyek. |

## <a name="kerberos-errors"></a>Kerberos-hibák

Ez a táblázat ismerteti az hello gyakori hibákat, és konfigurálja a Kerberos származik, és a megoldási javaslatokat is.

| Hiba | Javasolt lépések |
| ----- | ----------------- |
| Nem sikerült tooretrieve hello aktuális futtatására vonatkozó végrehajtási házirend PowerShell-parancsfájlokat. | Hello Connector telepítése nem sikerül, ha ellenőrizze toomake meg arról, hogy a PowerShell végrehajtási házirend nincs letiltva.<br><br>1. Nyissa meg a Helyicsoportházirend-szerkesztő hello.<br>2. Nyissa meg túl**számítógép konfigurációja** > **felügyeleti sablonok** > **Windows-összetevők**  >   **A Windows PowerShell** duplán **kapcsolja be a parancsfájl végrehajtása**.<br>3. hello végrehajtási házirendet állíthat be tooeither **nincs konfigurálva** vagy **engedélyezve**. Ha túl beállítása**engedélyezve**, győződjön meg arról, hogy a beállítások, hello végrehajtási házirend értéke tooeither **engedélyezése a helyi és távoli aláírt parancsfájlok** vagy túl**összes parancsfájlok**. |
| 12008 - azure AD túllépte hello maximális engedélyezett Kerberos hitelesítési kísérletek száma toohello háttérkiszolgálóra. | Ez a hiba előfordulhat, hogy az Azure AD közötti helytelen konfiguráció jelző és háttér hello alkalmazáskiszolgáló, vagy mindkét gépen dátumával és időpontjával konfigurációs problémát. hello háttérkiszolgáló elutasította az Azure AD által létrehozott hello Kerberos-jegy. Győződjön meg arról, hogy az Azure AD és hello háttér-alkalmazáskiszolgáló megfelelően van konfigurálva. Győződjön meg arról, hogy hello dátumával és időpontjával konfigurációs hello Azure AD és a háttér-alkalmazáskiszolgáló hello szinkronizálva van. |
| 13016 – az azure AD nem tudja beolvasni a Kerberos jegy hello felhasználó nevében, mert nincs nincs UPN hello biztonsági jogkivonat vagy hello hozzáférés cookie-ban. | Hello STS konfigurációjával kapcsolatban probléma merül fel. Hárítsa el a configuration UPN jogcím hello hello STS. |
| 13019 – az azure AD hello következő általános API-hiba miatt nem olvashatók be a Kerberos jegy hello felhasználó nevében. | Ez az esemény jelezheti, hogy az Azure AD közötti helytelen konfiguráció és hello tartományvezérlő kiszolgálót vagy mindkét gépen dátumával és időpontjával konfigurációs problémát. hello tartományvezérlő elutasította az Azure AD által létrehozott hello Kerberos-jegy. Győződjön meg arról, hogy az Azure AD és hello háttér-alkalmazáskiszolgáló megfelelően vannak konfigurálva, különösen az SPN hello konfigurációs. Győződjön meg arról, az Azure AD hello tartományhoz csatlakoztatott toohello ugyanabban a tartományban, a tartomány a tartományvezérlő tooensure, hogy a tartományvezérlő hello hello megbízhatósági kapcsolatot hoz létre az Azure ad-val. Ellenőrizze, hogy hello dátumával és időpontjával konfigurációs hello Azure AD és a tartományvezérlő hello szinkronizálva. |
| 13020 – az azure AD nem olvashatók be a következőt, mert hello háttérkiszolgáló SPN neve nincs megadva a Kerberos jegy hello felhasználó nevében. | Ez az esemény jelezheti, hogy az Azure AD közötti helytelen konfiguráció és hello tartományvezérlő kiszolgálót vagy mindkét gépen dátumával és időpontjával konfigurációs problémát. hello tartományvezérlő elutasította az Azure AD által létrehozott hello Kerberos-jegy. Győződjön meg arról, hogy az Azure AD és hello háttér-alkalmazáskiszolgáló megfelelően vannak konfigurálva, különösen az SPN hello konfigurációs. Győződjön meg arról, az Azure AD hello tartományhoz csatlakoztatott toohello ugyanabban a tartományban, a tartomány a tartományvezérlő tooensure, hogy a tartományvezérlő hello hello megbízhatósági kapcsolatot hoz létre az Azure ad-val. Ellenőrizze, hogy hello dátumával és időpontjával konfigurációs hello Azure AD és a tartományvezérlő hello szinkronizálva. |
| 13022 – az azure AD nem tudja hitelesíteni hello felhasználót, mert hello háttérkiszolgáló válaszol tooKerberos hitelesítési kísérlet egy HTTP 401-es hiba miatt. | Ez az esemény esetleg jelzi az Azure AD közötti helytelen konfiguráció, és a háttér hello alkalmazáskiszolgáló, vagy mindkét gépen dátumával és időpontjával konfigurációs problémát. hello háttérkiszolgáló elutasította az Azure AD által létrehozott hello Kerberos-jegy. Győződjön meg arról, hogy az Azure AD és hello háttér-alkalmazáskiszolgáló megfelelően van konfigurálva. Győződjön meg arról, hogy hello dátumával és időpontjával konfigurációs hello Azure AD és a háttér-alkalmazáskiszolgáló hello szinkronizálva van. |

## <a name="end-user-errors"></a>A végfelhasználói hibák

Ez a lista azokat a hibákat, a végfelhasználók előforduló próbálja tooaccess hello alkalmazást, és sikertelen lesz. 

| Hiba | Javasolt lépések |
| ----- | ----------------- |
| hello webhely hello lap nem jeleníthető meg. | A felhasználó előfordulhat, hogy ez a hibaüzenet tooaccess hello alkalmazást, ha az alkalmazás hello integrált Windows-Hitelesítést alkalmazás közzététele közben. Ez az alkalmazás esetleg helytelen hello meghatározott egyszerű Szolgáltatásnevet. Integrált Windows-Hitelesítést alkalmazások esetén győződjön meg róla, hogy az alkalmazáshoz beállított SPN hello helyes-e. |
| hello webhely hello lap nem jeleníthető meg. | A felhasználó előfordulhat, hogy ez a hibaüzenet tooaccess hello alkalmazást, ha az alkalmazás hello OWA alkalmazás közzététele közben. Ennek oka lehet a következő hello:<br><li>Ez az alkalmazás nem megfelelő hello meghatározott egyszerű Szolgáltatásnevet. Győződjön meg róla, hogy az alkalmazáshoz beállított SPN hello helyes-e.</li><li>hello felhasználó próbálta meg tooaccess hello alkalmazás van a Microsoft-fiókkal, hanem hello a megfelelő vállalati fiók toosign vagy hello felhasználó a Vendég felhasználó. Győződjön meg arról, hogy hello felhasználói használatával jelentkezik be, hogy megfelel hello hello tartományának a vállalati fiók közzétett alkalmazást. Microsoft Account felhasználók és a vendég nem hozzáférhessenek az integrált Windows-Hitelesítést alkalmazásokhoz.</li><li>hello felhasználó próbálta meg tooaccess hello alkalmazás megfelelően nincs definiálva ehhez az alkalmazáshoz hello helyszíni oldalán. Győződjön meg arról, hogy a felhasználó jogosult hello megfelelő meghatározottak szerint a háttéralkalmazás hello helyszíni gépen. |
| A vállalati alkalmazás nem érhető el. Ön nem jogosult tooaccess vannak ezt az alkalmazást. Nem sikerült engedélyezése. Győződjön meg arról, hogy tooassign hello felhasználói hozzáférés toothis alkalmazással. | A felhasználók előfordulhat, hogy ez a hibaüzenet tooaccess hello alkalmazást, ha a Microsoft-fiókok használata helyett a munkahelyi fiók toosign a közzétett tett kísérlet során. Vendégfelhasználók is kaphat ezt a hibát. Microsoft Account felhasználók és a vendégek nem hozzáférhessenek az integrált Windows-Hitelesítést alkalmazásokhoz. Győződjön meg arról, hogy hello felhasználói használatával jelentkezik be, hogy megfelel hello hello tartományának a vállalati fiók közzétett alkalmazást.<br><br>Előfordulhat, hogy nem rendelte hello felhasználói ehhez az alkalmazáshoz. Nyissa meg toohello **alkalmazás** fülre, és a **felhasználók és csoportok**, rendelje hozzá a felhasználó vagy felhasználói csoport toothis alkalmazás. |
| A vállalati alkalmazás most nem lehet hozzáférni. Próbálkozzon újra később... hello összekötő túllépte az időkorlátot. | A felhasználók előfordulhat, hogy ez a hibaüzenet tooaccess hello alkalmazást, ha nem megfelelően határozza hello helyszíni oldalon az alkalmazás közzététele közben. Győződjön meg arról, hogy a felhasználók rendelkeznek-e a hello megfelelő engedélyekkel, meghatározottak szerint a háttéralkalmazás hello helyszíni gépen. |
| A vállalati alkalmazás nem érhető el. Ön nem jogosult tooaccess vannak ezt az alkalmazást. Nem sikerült engedélyezése. Győződjön meg arról, hogy hello felhasználói licencet az Azure Active Directory prémium vagy alapszintű kiadásra. | A felhasználók előfordulhat, hogy ez a hibaüzenet tooaccess hello alkalmazást, ha azok nem explicit módon hozzárendelt prémium vagy alapszintű licenccel rendelkező hello előfizető rendszergazda által közzétett tett kísérlet során. Nyissa meg az Active Directory toohello előfizető **licencek** lapra, és győződjön meg arról, hogy a felhasználó vagy felhasználói csoport van hozzárendelve a prémium vagy alapszintű kiadásra licenc. |

## <a name="my-error-wasnt-listed-here"></a>A hiba nem az itt felsorolt

Ha egy hiba vagy probléma az Azure AD-alkalmazásproxy, amely nem szerepel az hibaelhárítási útmutató, toohear kapcsolatos tapasztalatairól. Küldjön egy e-mailek tooour [visszajelzés team](mailto:aadapfeedback@microsoft.com) hello hello hiba részleteit.

## <a name="see-also"></a>Lásd még:
* [Az Azure Active Directory alkalmazásproxy engedélyezése](active-directory-application-proxy-enable.md)
* [Alkalmazások közzététele az alkalmazásproxy](active-directory-application-proxy-publish.md)
* [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
* [Feltételes hozzáférés engedélyezése](active-directory-application-proxy-conditional-access.md)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
