---
title: "aaaAzure Active Directory forgatókönyv építőelemei a koncepció igazolása |} Microsoft Docs"
description: "Vizsgálatát, és gyorsan végrehajthatja az identitás- és kezelési helyzetek"
services: active-directory
keywords: "az Azure active directory, a forgatókönyv, a koncepció igazolása, PoC"
documentationcenter: 
author: dstefanMSFT
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: dstefan
ms.openlocfilehash: e54148330a123baf27d7e0f73469ff2a24c0efcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-building-blocks"></a>Az Azure Active Directory alkalmazástervezési a koncepció igazolása: építőelemek

## <a name="catalog-of-roles"></a>A szerepkörök katalógus

| Szerepkör | Leírás | Megvalósíthatósági felelősségi igazolása |
| --- | --- | --- |
| **Identitás-architektúra / fejlesztői csapat** | Ez a csoport általában hello egyik, hello megoldás tervez, amely prototípusok, jóváhagyások meghajtók, és végül átadja az toooperations | Adja meg a hello környezetekben, és ők hello értékelésekor hello különböző forgatókönyv hello kezelhetőség szempontjából |
| **A helyszíni identitás műveleti csapata** | Kezeli a hello identitások források helyszíni: Active Directory-erdők, LDAP-könyvtárak, HR rendszerek és összevonási identitás-szolgáltatóktól. | Adja meg a hello koncepció forgatókönyvek tooon helyszíni erőforrások eléréséhez szükséges.<br/>Akkor be kell vonni lehető|
| **Műszaki Alkalmazástulajdonosok** | Hello különböző felhőalapú alkalmazásokat és szolgáltatásokat, az Azure ad-vel integrálni műszaki tulajdonosainak | Adja meg az adatokat a SaaS-alkalmazások (esetleg tesztelési példány) |
| **Az Azure AD globális rendszergazda** | Az Azure AD hello konfigurációjának kezelése | Adja meg a hitelesítő adatokat tooconfigure hello szinkronizálási szolgáltatást. Általában identitás architektúra, azonos team hello koncepció során, de külön hello műveletek fázis során|
| **Adatbázis-csoport** | Hello adatbázis infrastruktúra tulajdonosainak | Adja meg a hozzáférés tooSQL környezet (AD FS vagy az Azure AD Connect) az adott forgatókönyv előkészített.<br/>Akkor be kell vonni lehető |
| **Hálózati adapterek** | Hello hálózati infrastruktúra tulajdonosainak | Hozzáférést szükséges hello hálózati szintű hello szinkronizálási kiszolgálók tooproperly hozzáférés hello adatforrásokhoz, és a felhőalapú szolgáltatásokhoz (tűzfalszabályok, megnyitott portok, IPSec-szabályok stb.) |
| **Biztonsági csapat** | Hello biztonsági stratégia meghatározása, elemzi a különböző forrásokból származó biztonsági jelentések és megállapítások következik. | Adja meg a célként megadott biztonsági értékelési forgatókönyvek |

## <a name="common-prerequisites-for-all-building-blocks"></a>Minden egyéb építőelemekből közös előfeltételei

Az alábbiakban néhány szükséges előfeltételek bármely Koncepció az Azure AD Premium szükséges.

| Előfeltétel | Erőforrások |
| --- | --- |
| Az Azure AD-bérlő definiált egy érvényes Azure-előfizetés | [Hogyan tooget egy Azure Active Directory-bérlőben](active-directory-howto-tenant.md)<br/>**Megjegyzés:** Ha már rendelkezik Azure AD Premium licenccel rendelkező környezetek, nulla cap előfizetés beszerezheti toohttps://aka.ms/accessaad navigálva <br/>További tudnivalókért olvassa el: https://blogs.technet.microsoft.com/enterprisemobility/2016/02/26/azure-ad-mailbag-azure-subscriptions-and-azure-ad-2/ és https://technet.microsoft.com/library/dn832618.aspx |
| Meghatározott és ellenőrzött tartományok | [Egy egyéni tartomány nevét az Active Directory tooAzure hozzáadása](active-directory-domains-add-azure-portal.md)<br/>**Megjegyzés:** bizonyos munkaterhelések esetén, mint például a Power BI sikerült kiépítése az azure AD-bérlő alatt hello ismerteti. toocheck Ha egy adott tartományban társított tooa bérlői, keresse meg a toohttps://login.microsoftonline.com/ {domain}/v2.0/.well-known/openid-configuration. Ha tarozik sikeres, akkor hello tartomány tooa bérlői már hozzá van rendelve, és vegye át lehet szükség. Ha igen, lépjen kapcsolatba a Microsofttal kapcsolatos további iránymutatásért. További tudnivalók hello felvásárlási beállításokat érintő: [Mi az az Azure önkiszolgáló regisztráció?](active-directory-self-service-signup.md) |
| Prémium szintű Azure AD- vagy EMS próba engedélyezve | [Az Azure Active Directory prémium ingyenes egy hónapos](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Prémium szintű Azure AD- vagy EMS-licenceket rendelt tooPoC felhasználók | [Licenc saját maga és a felhasználók az Azure Active Directoryban](active-directory-licensing-get-started-azure-portal.md) |
| Az Azure AD globális rendszergazda hitelesítő adataival | [Rendszergazdai szerepkörök hozzárendelése az Azure Active Directoryban](active-directory-assign-admin-roles-azure-portal.md) |
| Nem kötelező, de erősen ajánlott: tartalékként párhuzamos laborkörnyezetben | [Az Azure AD Connect előfeltételei](./connect/active-directory-aadconnect-prerequisites.md) |

## <a name="directory-synchronization---password-hash-sync-phs---new-installation"></a>Directory szinkronizálási - Jelszókivonat-szinkronizálás (PHS-ben) – új telepítése

Hozzávetőleges időt tooComplete: egy órával kevesebb, mint 1000 PoC-felhasználók

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| Az Azure AD Connect kiszolgáló tooRun | [Az Azure AD Connect előfeltételei](./connect/active-directory-aadconnect-prerequisites.md) |
| Célfelhasználók Koncepció, a hello azonos tartományban és egy biztonsági csoportot, és a szervezeti része | [Az Azure AD Connect testreszabott telepítése](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) |
| Az Azure AD Connect-funkciókat Koncepció azonosított hello szükséges | [Az Azure Active Directoryval az Active Directory Connect - szinkronizálás konfigurálása szolgáltatások](./connect/active-directory-aadconnect.md#configure-sync-features) |
| A helyszíni hitelesítő adatok kellett, és a felhőalapú környezetben  | [Azure AD Connect: fiókok és engedélyek](./connect/active-directory-aadconnect-accounts-permissions.md) |

### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Hello Azure AD Connect legújabb verziójának letöltése | [A Microsoft Azure Active Directory Connect letöltése](https://www.microsoft.com/download/details.aspx?id=47594) |
| Azure AD Connect telepítése hello legegyszerűbb elérési úttal: Express <br/>1. Toohello OU toominimize hello szinkronizálási ciklusban célidőnél szűrése<br/>2. Válasszon cél felhasználók hello a helyi csoport.<br/>3. Ha szükséges, a más Koncepció témák hello hello szolgáltatásokat | [Az Azure AD Connect: Testreszabott telepítés: tartomány és szervezeti egységek szűrése](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) <br/>[Az Azure AD Connect: Testreszabott telepítés: csoport alapú szűrése](./connect/active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups)<br/>[Az Azure AD Connect: A helyszíni identitások integrálása az Azure Active Directoryval: a szinkronizálási funkciók konfigurálása](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Az Azure AD Connect felhasználói felület hello megnyitását és megtekintését a hello futtató profilok befejezett (importálása szinkronizálási és exportálás) | [Az Azure AD Connect szinkronizálása: ütemező](./connect/active-directory-aadconnectsync-feature-scheduler.md) |
| Nyissa meg hello [Azure AD felügyeleti portál](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/), nyissa meg toohello "Minden felhasználó" panelen, "hatóság" forrásoszlop és hello felhasználók jelennek meg, lásd "A Windows Server AD" érkező megfelelően jelölésű hozzáadása | [Az Azure AD felügyeleti portál](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) |

### <a name="considerations"></a>Megfontolandó szempontok

1. Nézze meg hello biztonsági szempontok a Jelszókivonat-szinkronizálás [Itt](./connect/active-directory-aadconnectsync-implement-password-synchronization.md).  Ha kísérleti felhasználóknak Jelszókivonat-szinkronizálás nincs véglegesen lehetőség, fontolja meg hello alternatíva a következő:
   * Tesztfelhasználók létrehozása hello éles tartományban. Ellenőrizze, hogy bármely más fiók nincs szinkronizálás
   * Helyezze át a tooan UAT környezet
2.  Ha azt szeretné, hogy toopursue összevonási, a rendszer megfelelőbb toounderstand hello költségek összevont megoldás társított helyszíni identitásszolgáltató hello Koncepció túli és mérték elleni hello előnyére keres:
    * Az egy hello kritikus elérési útja, így a magas rendelkezésre állású toodesign kell
    * Egy toocapacity terv van szüksége a helyi szolgáltatás
    * Egy szükséges toomonitor/karbantartása/javítást a helyi szolgáltatás

További: [az Office 365 identitás- és Azure Active Directory - összevont identitás](https://support.office.com/article/Understanding-Office-365-identity-and-Azure-Active-Directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9#bk_federated)

## <a name="branding"></a>Védjegyek

Hozzávetőleges időt tooComplete: 15 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| Eszközök (képek, emblémák, stb.); Ajánlott a képi megjelenítéshez tartozó győződjön meg arról, hogy hello eszközök méretek ajánlott hello rendelkezik. | [Vállalati arculat megjelenítése a tooyour hello Azure Active Directory bejelentkezési lap](active-directory-branding-custom-signon-azure-portal.md) |
| Nem kötelező: Ha hello környezetben egy AD FS-kiszolgáló, hozzáférési toohello server toocustomize Webhelytéma | [AD FS felhasználói bejelentkezési testreszabása](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-user-sign-in-customization) |
| Ügyfél számítógép tooperform végfelhasználói bejelentkezési élmény |  |
| Választható lehetőség: Mobileszközök toovalidate élmény |  |

### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Nyissa meg tooAzure AD felügyeleti portál | [Azure AD felügyeleti portál - vállalati arculat megjelenítése](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/LoginTenantBranding) |
| Töltse fel a hello eszközök hello bejelentkezési oldal (kiemelt embléma kis embléma, címkék, stb.). Ha szükséges, ha az AD FS igazítása ugyanazokat az eszközöket az AD FS bejelentkezési oldalak hello | [Vállalati arculat megjelenítése a tooyour bejelentkezési és hozzáférési Panel oldalakon: testreszabható elemek](active-directory-add-company-branding.md) |
| Várjon néhány percig, amíg hello módosítása toofully életbe lépéséhez |  |
| Jelentkezzen be a hello Koncepció felhasználói hitelesítő adatok toohttps://myapps.microsoft.com |  |
| Erősítse meg a hello megjelenését és működését a böngészőben | [Vállalati arculat megjelenítése a tooyour bejelentkezési és hozzáférési Panel oldalakon](active-directory-add-company-branding.md) |
| Szükség esetén ellenőrizze a hello Megjelenés és működés az egyéb eszközök |  |

### <a name="considerations"></a>Megfontolandó szempontok

Ha hello régi megjelenését és működését maradó hello testreszabási hello böngésző ügyfél gyorsítótárának ürítése, és újra hello műveletet.

## <a name="group-based-licensing"></a>Csoport alapú licencelés

Hozzávetőleges időt tooComplete: 10 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| Senki Koncepció részei egy biztonsági csoportot (helyszíni vagy felhőalapú) | [Hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban](active-directory-groups-create-azure-portal.md) |

### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Nyissa meg az Azure AD felügyeleti portál toolicenses panel | [Azure AD felügyeleti portál: Licencelés](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) |
| Rendelje hozzá a hello licencek toohello biztonsági csoport Koncepció felhasználókkal. | [Rendelje hozzá a licencek tooa csoport számára az Azure Active Directoryban](active-directory-licensing-group-assignment-azure-portal.md) |

### <a name="considerations"></a>Megfontolandó szempontok

Ha probléma merül fel, nyissa meg túl[forgatókönyvek, korlátozások és az Azure Active Directory licencelése toomanage csoportok használatával kapcsolatos ismert problémák](active-directory-licensing-group-advanced.md)

## <a name="saas-federated-sso-configuration"></a>SaaS összevont egyszeri bejelentkezés konfigurálása

Hozzávetőleges időt tooComplete: 60 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| SaaS-alkalmazáshoz elérhető hello tesztkörnyezetben. Ez az útmutató a ServiceNow használjuk példaként.<br/>Erősen ajánlott toouse egy teszt példány toominimize súrlódás a Navigálás a meglévő adatok minőségét és a leképezéseket. | Nyissa meg toohttps://developer.servicenow.com/app.do#! / home toostart hello folyamatának a teszt példánya |
| Felügyeleti hozzáférés toohello ServiceNow felügyeleti konzol | [Oktatóanyag: Azure Active Directoryval integrált ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Célja a felhasználók tooassign hello alkalmazás állítja be. Hello koncepció felhasználókat tartalmazó biztonsági csoport használata ajánlott. <br/>Ha létrehozása hello csoport esetén nem valósítható meg, majd hello felhasználók hozzárendelése hello koncepció toodirectly toohello alkalmazás | [Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások az Azure Active Directoryban](active-directory-coreapps-assign-user-azure-portal.md) |

### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Hello oktatóanyag tooall szereplője Microsoft Documentation megosztása  | [Oktatóanyag: Azure Active Directoryval integrált ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Egy működő értekezlet beállítása és lépések hello oktatóanyag minden résztvevővel. | [Oktatóanyag: Azure Active Directoryval integrált ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Rendelje hozzá a hello app toohello csoporttal, a hello előfeltételek. Ha hello Koncepció feltételes hozzáférés hello hatókörében, le újra, hogy később, és adja hozzá a többtényezős Hitelesítést, és hasonló. <br/>Megjegyzés: Ez fogja indítsa a hello létesítésének folyamatát kell használnia (Ha be van állítva) |  [Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások az Azure Active Directoryban](active-directory-coreapps-assign-user-azure-portal.md) <br/>[Hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban](active-directory-groups-create-azure-portal.md) |
| Az Azure AD felügyeleti portál tooadd ServiceNow alkalmazás használja a gyűjteményből| [Azure AD felügyeleti portál: vállalati alkalmazások](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/Overview) <br/>[Vállalati alkalmazások kezelése az Azure Active Directory újdonságai](active-directory-enterprise-apps-whats-new-azure-portal.md) |
| "Egyszeri bejelentkezés" panelen ServiceNow alkalmazás SAML-alapú bejelentkezés"engedélyezése" |  |
| Töltse ki a "Bejelentkezési URL-cím" és "Azonosítója" mezőket a ServiceNow URL-címmel<br/>Hello jelölőnégyzetet túl "aktívvá új tanúsítvány"<br/>és a beállítások mentése |  |
| Nyissa meg "Konfigurálása ServiceNow" paneljét hello alsó részén hello panel tooview testre szabott útmutatást tartalmaz tooconfigure ServiceNow |  |
| Kövesse az utasításokat tooconfigure ServiceNow |  |
| A ServiceNow alkalmazás "Kiépítés" panelen "Automatikus" kiépítés engedélyezése | [Felhasználói fiók kiépítése hello új Azure-portálon a vállalati alkalmazások kezelése](active-directory-enterprise-apps-manage-provisioning.md) |
| Várjon néhány percig, amíg kiépítése befejeződött.  Az hello addig is megtekintheti a jelentések kiépítés hello |  |
| Jelentkezzen be toohttps://myapps.microsoft.com/, amely hozzáféréssel rendelkezik a tesztfelhasználó számára | [Mi az a hozzáférési Panel hello?](active-directory-saas-access-panel-introduction.md) |
| Kattintson az újonnan létrehozott hello alkalmazás hello csempe. Hozzáférés megerősítése |  |
| Másik lehetőségként meggyőződhet arról is hello alkalmazás használati jelentések. Vegye figyelembe azt némi késés, így Önnek kell toowait bizonyos idő toosee hello forgalom hello jelentésekben. | [Bejelentkezési tevékenység jelentések hello Azure Active Directory-portálon: felügyelt alkalmazások használata](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Az Azure Active Directory jelentésmegőrzési házirendje](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Megfontolandó szempontok

1. Fent [oktatóanyag](active-directory-saas-servicenow-tutorial.md) tooold az Azure AD felügyeleti élmény hivatkozik. A koncepció alapul, de [gyors üzembe helyezési](active-directory-enterprise-apps-whats-new-azure-portal.md#quick-start-get-going-with-your-new-application-right-away) tapasztalhat.
2. Ha hello célalkalmazás nincs hello gyűjteményben, majd használhatja "Állapotba hozása a saját alkalmazás". További: [What's new in Azure Active Directory vállalati Alkalmazáskezelés: egyetlen helyről egyéni alkalmazás hozzáadása](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

## <a name="saas-password-sso-configuration"></a>SaaS-jelszó egyszeri bejelentkezés konfigurálása

Hozzávetőleges időt tooComplete: 15 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| A tesztkörnyezet az SaaS-alkalmazásokhoz. Egy Egyszeri jelszót példája HipChat és a Twitter. Egyetlen más alkalmazáshoz hello hello oldal HTML-bejelentkezési űrlap a pontos URL-kell. | [A Microsoft Azure piactérről Twitter](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[A Microsoft Azure piactérről HipChat](https://azuremarketplace.microsoft.com/marketplace/apps/aad.hipchat) |
| Fiókok hello alkalmazások tesztelése. | [Feliratkozás a Twitteren](https://twitter.com/signup?lang=en)<br/>[Regisztráljon az ingyenes: HipChat](https://www.hipchat.com/sign_up) |
| Célja a felhasználók tooassign hello alkalmazás állítja be. Ajánlott egy biztonsági csoport tartalmazott hello felhasználóinak. | [Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások az Azure Active Directoryban](active-directory-coreapps-assign-user-azure-portal.md) |
| Helyi rendszergazdai hozzáférés tooa számítógép toodeploy hello hozzáférési Panel bővítményét az Internet Explorer, a Chrome vagy a Firefox | [Az Internet Explorer hozzáférési Panel bővítmény](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Hozzáférési Panel bővítményével Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Hozzáférési Panel bővítményével Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Hello bővítmény telepítése | [Az Internet Explorer hozzáférési Panel bővítmény](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Hozzáférési Panel bővítményével Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Hozzáférési Panel bővítményével Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Állítsa be alkalmazását a gyűjteményből | [What's new in Azure Active Directory vállalati Alkalmazáskezelés: hello új és továbbfejlesztett alkalmazáskatalógusában](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Egyszeri jelszó konfigurálása | [Egyszeri bejelentkezés hello új Azure-portálon a vállalati alkalmazások kezelése: jelszó alapú bejelentkezés](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Rendelje hozzá a hello app toohello csoporttal, a hello Előfeltételek | [Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások az Azure Active Directoryban](active-directory-coreapps-assign-user-azure-portal.md) |
| Jelentkezzen be toohttps://myapps.microsoft.com/, amely hozzáféréssel rendelkezik a tesztfelhasználó számára |  |
| Kattintson az újonnan létrehozott hello alkalmazás hello csempe. | [Mi az hozzáférési Panel hello?: jelszó alapú egyszeri bejelentkezési identitás kiépítés nélkül](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Adja meg a hello alkalmazás hitelesítő adatai | [Mi az hozzáférési Panel hello?: jelszó alapú egyszeri bejelentkezési identitás kiépítés nélkül](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Zárja be a böngésző hello és ismétlési hello bejelentkezési. Ez alkalommal körülbelül hello felhasználói megjelennie zökkenőmentes hozzáférést toohello alkalmazás. |  |
| Másik lehetőségként meggyőződhet arról is hello alkalmazás használati jelentések. Vegye figyelembe azt némi késés, így Önnek kell toowait bizonyos idő toosee hello forgalom hello jelentésekben. | [Bejelentkezési tevékenység jelentések hello Azure Active Directory-portálon: felügyelt alkalmazások használata](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Az Azure Active Directory jelentésmegőrzési házirendje](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Megfontolandó szempontok

Ha hello célalkalmazás nincs hello gyűjteményben, majd használhatja "Állapotba hozása a saját alkalmazás". További: [What's new in Azure Active Directory vállalati Alkalmazáskezelés: egyetlen helyről egyéni alkalmazás hozzáadása](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Tartsa szem előtt tartva hello követelményeknek:
   * Alkalmazás egy ismert bejelentkezési URL-címet kell rendelkeznie.
   * hello bejelentkezési oldal tartalmaznia kell egy HTML-űrlaphoz mezőkkel egy további szöveg hello böngészőbővítményeket automatikus-töltheti fel. A minimális hello felhasználónevet és jelszót kell tartalmaznia.

## <a name="saas-shared-accounts-configuration"></a>Megosztott SaaS konfigurációja

Hozzávetőleges időt tooComplete: 30 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| hello cél-alkalmazások listájában, és pontos bejelentkezési URL-CÍMEK időben hello. Tegyük fel a Twitter is használhatja. | [A Microsoft Azure piactérről Twitter](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[Feliratkozás a Twitteren](https://twitter.com/signup?lang=en) |
| Megosztott az SaaS-alkalmazáshoz tartozó hitelesítő adatok. | [Az Azure AD fiókok megosztása](active-directory-sharing-accounts.md)<br/>[Az azure AD automatikus jelszó során a Facebook, a Twitter és a LinkedIn előzetes verzióként elérhető! -Nagyvállalati mobilitással és biztonsággal foglalkozó blogban] (https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Legalább két csoport tagjai, akik hozzáférhetnek a hitelesítő adatok hello ugyanazt a fiókot. Azok a biztonsági csoport részének kell lennie. | [Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások az Azure Active Directoryban](active-directory-coreapps-assign-user-azure-portal.md) |
| Helyi rendszergazdai hozzáférés tooa számítógép toodeploy hello hozzáférési Panel bővítményét az Internet Explorer, a Chrome vagy a Firefox | [Az Internet Explorer hozzáférési Panel bővítmény](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Hozzáférési Panel bővítményével Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Hozzáférési Panel bővítményével Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Hello bővítmény telepítése | [Az Internet Explorer hozzáférési Panel bővítmény](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Hozzáférési Panel bővítményével Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Hozzáférési Panel bővítményével Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Állítsa be alkalmazását a gyűjteményből | [What's new in Azure Active Directory vállalati Alkalmazáskezelés: hello új és továbbfejlesztett alkalmazáskatalógusában](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Egyszeri jelszó konfigurálása | [Egyszeri bejelentkezés hello új Azure-portálon a vállalati alkalmazások kezelése: jelszó alapú bejelentkezés](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Rendelje hozzá a hello app toohello csoporttal hello Előfeltételek a hitelesítő adatok hozzárendelése közben | [Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások az Azure Active Directoryban](active-directory-coreapps-assign-user-azure-portal.md) |
| Jelentkezzen be másik felhasználóként, hogy hozzáférési alkalmazást hello **ugyanazt a közös fiókot.**  |  |
| Másik lehetőségként meggyőződhet arról is hello alkalmazás használati jelentések. Vegye figyelembe azt némi késés, így Önnek kell toowait bizonyos idő toosee hello forgalom hello jelentésekben. | [Bejelentkezési tevékenység jelentések hello Azure Active Directory-portálon: felügyelt alkalmazások használata](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Az Azure Active Directory jelentésmegőrzési házirendje](active-directory-reporting-retention.md) |


### <a name="considerations"></a>Megfontolandó szempontok

Ha hello célalkalmazás nincs hello gyűjteményben, majd használhatja "Állapotba hozása a saját alkalmazás". További: [What's new in Azure Active Directory vállalati Alkalmazáskezelés: egyetlen helyről egyéni alkalmazás hozzáadása](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Tartsa szem előtt tartva hello követelményeknek:
   * Alkalmazás egy ismert bejelentkezési URL-címet kell rendelkeznie.
   * hello bejelentkezési oldal tartalmaznia kell egy HTML-űrlaphoz mezőkkel egy további szöveg hello böngészőbővítményeket automatikus-töltheti fel. A minimális hello felhasználónevet és jelszót kell tartalmaznia.

## <a name="app-proxy-configuration"></a>Alkalmazás proxykonfigurációt

Hozzávetőleges időt tooComplete: 20 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| A Microsoft Azure AD alapszintű vagy premium előfizetéssel és, amelynek egy globális rendszergazda Azure AD-címtár | [Az Azure Active Directory-kiadások](active-directory-editions.md) |
| A webes alkalmazás üzemeltetett helyszíni, hogy szeretné-e a távoli hozzáféréshez tooconfigure |  |
| A Windows Server 2012 R2 vagy Windows 8.1 vagy újabb rendszert futtató kiszolgáló, amely telepíthető hello alkalmazásproxy-összekötő | [Az Azure AD-alkalmazásproxy összekötők ismertetése](application-proxy-understand-connectors.md) |
| Ha van tűzfal a hello elérési útja, győződjön meg arról, hogy meg nyitva, az összekötő el tudja küldeni HTTPS (TCP), hogy hello kérelmek toohello proxyval | [Alkalmazásproxy engedélyezése az Azure-portálon hello: alkalmazásproxy Előfeltételek](active-directory-application-proxy-enable.md#application-proxy-prerequisites) |
| Ha a szervezet használja a proxy kiszolgálók tooconnect toohello internet, hajtsa végre a megfelelő hello blog egy pillantást működő utáni a meglévő helyszíni proxykiszolgálókkal a további tudnivalókat tooconfigure őket | [A meglévő helyszíni proxykiszolgálókkal működik](application-proxy-working-with-proxy-servers.md) |


### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Hello kiszolgálón egy összekötő telepítése | [Alkalmazásproxy engedélyezése az Azure-portálon hello: hello összekötő regisztrálása és telepítése](active-directory-application-proxy-enable.md#install-and-register-a-connector) |
| Az Azure AD alkalmazásproxy alkalmazásként hello helyszíni alkalmazás közzététele | [Az Azure AD-alkalmazásproxy használó alkalmazások közzététele](application-proxy-publish-azure-portal.md) |
| Teszt felhasználók hozzárendelése | [Alkalmazások közzététele az Azure AD-alkalmazásproxy használatával: teszt felhasználó hozzáadása](application-proxy-publish-azure-portal.md#add-a-test-user) |
| Nem kötelező lépésként beállíthatja egy egyszeri bejelentkezéses felhasználói élmény | [Adja meg az egyszeri bejelentkezés az Azure AD-alkalmazásproxyval](application-proxy-sso-azure-portal.md) |
| Teszt app tooMyApps portált, mint a bejelentkezéssel hozzárendelt felhasználó | https://myapps.microsoft.com |

### <a name="considerations"></a>Megfontolandó szempontok

1. Javasoljuk, hogy a vállalati hálózat hello összekötő üzembe, amíg vannak esetek, amikor látni fogja a jobb teljesítmény érdekében helyezés hello felhő. További: [Azure Active Directory Application Proxy használata esetén a hálózati topológia szempontjai](application-proxy-network-topology-considerations.md)
2. További biztonsági részletek és hogyan Ez a különösen biztonságos távoli hozzáférést biztosít a megoldás csak a kimenő kapcsolatok fölött:: [alkalmazásokhoz távolról az Azure AD-alkalmazásproxy használatával fér hozzá biztonsági szempontjai](application-proxy-security-considerations.md)

## <a name="generic-ldap-connector-configuration"></a>Általános LDAP-összekötő konfigurálása

Hozzávetőleges időt tooComplete: 60 perc

> [!IMPORTANT]
> Ez a beállítás egy speciális igénylő FIM vagy MIM bizonyos fokú ismeretét. Ha használja termelési, javasoljuk, hogy ez a konfiguráció kapcsolatos kérdéseket halad át [Premier támogatási](https://support.microsoft.com/premier).

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| Az Azure AD Connect telepítése és konfigurálása | Építőelem: [a címtár-szinkronizálás - Jelszókivonat-szinkronizálás](#directory-synchronization--password-hash-sync-phs--new-installation) |
| ADLDS példány értekezlet követelmények | [Általános LDAP-összekötő műszaki útmutatója: hello általános LDAP-összekötő áttekintése](./connect/active-directory-aadconnectsync-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| Felhasználók által használt, munkaterhelések és az ilyen terhelések tartozó attribútumok listája | [Azure AD Connect szinkronizálása: attribútumok szinkronizálva tooAzure Active Directory](./connect/active-directory-aadconnectsync-attributes-synchronized.md) |


### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Általános LDAP-összekötő hozzáadása | [Általános LDAP-összekötő műszaki útmutatója: hozzon létre egy új összekötőt](./connect/active-directory-aadconnectsync-connector-genericldap.md#create-a-new-connector) |
| (A teljes importálás, különbözeti importálás, teljes szinkronizálást, a különbözeti szinkronizálás, exportálás) létrehozott összekötőhöz futtatási profilok létrehozása | [A felügyeleti ügynök futtatási profil létrehozásához](https://technet.microsoft.com/library/jj590219(v=ws.10).aspx)<br/> [Összekötők használata az Azure AD Connect szinkronizálási szolgáltatás Manager hello](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md)|
| Teljes importálás profil futtatni, és ellenőrizze, hogy nincsenek-e objektumokat a kapcsolódási térbe | [Egy összekötő terület objektum keresése](https://technet.microsoft.com/library/jj590287(v=ws.10).aspx)<br/>[Összekötők használata az Azure AD Connect szinkronizálási szolgáltatás Manager hello: Összekötőtér keresése](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md#search-connector-space) |
| Szinkronizálási szabályok létrehozása, hogy a Metaverzumban található objektumok munkaterhelések szükséges attribútumokkal rendelkeznek | [Azure AD Connect szinkronizálása: ajánlott eljárások a hello alapértelmezett konfiguráció megváltoztatását: tooSynchronization szabályok módosítása](./connect/active-directory-aadconnectsync-best-practices-changing-default-configuration.md#changes-to-synchronization-rules)<br/>[Azure AD Connect szinkronizálása: Understanding deklaratív kiépítés](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning.md)<br/>[Azure AD Connect szinkronizálása: deklaratív kiépítés kifejezések ismertetése](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| Indítsa el a teljes szinkronizálási ciklust | [Azure AD Connect szinkronizálása: a Feladatütemező: hello a Feladatütemező indítása](./connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler) |
| Problémák esetén hajtsa végre a hibaelhárítási | [Olyan objektum, amely nem szinkronizál tooAzure AD hibaelhárítása](./connect/active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) |
| Győződjön meg arról, hogy LDAP felhasználó bejelentkezési és hello alkalmazás elérése | https://myapps.microsoft.com |

### <a name="considerations"></a>Megfontolandó szempontok

> [!IMPORTANT]
> Ez a beállítás egy speciális igénylő FIM vagy MIM bizonyos fokú ismeretét. Ha használja termelési, javasoljuk, hogy ez a konfiguráció kapcsolatos kérdéseket halad át [Premier támogatási](https://support.microsoft.com/premier).

## <a name="groups---delegated-ownership"></a>Csoportok - delegált tulajdonosa

Hozzávetőleges időt tooComplete: 10 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| SaaS-alkalmazáshoz (összevont egyszeri Bejelentkezéses vagy Egyszeri jelszó) már konfigurálva | Építőelem: [SaaS összevont egyszeri bejelentkezés konfigurálása](#saas-federated-sso-configuration) |
| Access toohello alkalmazás # 1 felhő csoportnak, amelynél | Építőelem: [SaaS összevont egyszeri bejelentkezés konfigurálása](#saas-federated-sso-configuration) <br/>[Hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban](active-directory-groups-create-azure-portal.md) |
| Hello csoport tulajdonos hitelesítő adatok | [Hozzáférés tooresources kezelése az Azure Active Directoryval](active-directory-manage-groups.md) |
| Hitelesítő adatok hello adatokat feldolgozó elérése során hello alkalmazások azonosítása | [Mi az a hozzáférési Panel hello?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Hello csoport, amely rendelkezik hozzáférési toohello alkalmazás azonosításához, és konfigurálja az adott hello tulajdonosának csoport| [Az Azure Active Directory csoport hello beállításainak kezelése](active-directory-groups-settings-azure-portal.md) |
| Jelentkezzen be a csoport tulajdonosa hello, lásd: a hozzáférési panel csoportok lapján hello csoporttagság | [Az Azure Active Directory-csoportok kezelése lap](https://account.activedirectory.windowsazure.com/r/#/groups) |
| Adja hozzá a kívánt tootest hello Infomunkás |  |
| Jelentkezzen be hello információkkal dolgozó szakember, ellenőrizze a hello csempe érhető el | [Mi az a hozzáférési Panel hello?](active-directory-saas-access-panel-introduction.md) |

### <a name="considerations"></a>Megfontolandó szempontok

Ha hello alkalmazás kiépítés engedélyezve van, előfordulhat, hogy szüksége toowait néhány perc múlva hello toocomplete kiépítés hello információkkal dolgozó szakember hello alkalmazás elérése előtt.

## <a name="saas-and-identity-lifecycle"></a>SaaS és identitás életciklusa

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| SaaS-alkalmazáshoz (összevont egyszeri Bejelentkezéses vagy Egyszeri jelszó) már konfigurálva | Építőelem: [SaaS összevont egyszeri bejelentkezés konfigurálása](#saas-federated-sso-configuration) |
| Access toohello alkalmazás # 1 felhő csoportnak, amelynél | Építőelem: [SaaS összevont egyszeri bejelentkezés konfigurálása](#saas-federated-sso-configuration) <br/>[Hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban](active-directory-groups-create-azure-portal.md) |
| Hitelesítő adatok hello adatokat feldolgozó elérése során hello alkalmazások azonosítása | [Mi az a hozzáférési Panel hello?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Távolítsa el hello felhasználói hello csoport hello alkalmazásból túl van hozzárendelve| [Az Azure Active Directory-bérlő felhasználók csoport tagságának kezelésére](active-directory-groups-members-azure-portal.md) |
| Várjon néhány percig, megszüntetést | [Az Azure AD SaaS-alkalmazás a felhasználók átadása az automatikus: hogyan működik az automatikus létesítési munkahelyi?](active-directory-saas-app-provisioning.md#how-does-automated-provisioning-work) |
| A különálló böngésző-munkamenet a hello adatokat feldolgozó toomy alkalmazások portál be, és győződjön meg arról, hogy a csempe nem található | http://myapps.microsoft.com |


### <a name="considerations"></a>Megfontolandó szempontok

Extrapolálják hello koncepció forgatókönyv tooleavers és/vagy hagyja az hiányában forgatókönyvek. Ha hello felhasználói lekérdezi az le van tiltva a helyszíni AD, vagy eltávolítani, van már nem egy módon toolog toohello SaaS-alkalmazáshoz.

## <a name="self-service-access-tooapplication-management"></a>Saját felügyeleti hozzáférés tooApplication

Hozzávetőleges időt tooComplete: 10 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| Toohello alkalmazásokat, fog igényelni hello biztonsági csoport részeként Koncepció felhasználók azonosítása | Építőelem: [SaaS összevont egyszeri bejelentkezés konfigurálása](#saas-federated-sso-configuration) |
| Cél alkalmazása telepítve van. | Építőelem: [SaaS összevont egyszeri bejelentkezés konfigurálása](#saas-federated-sso-configuration) |

### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Nyissa meg tooEnterprise alkalmazások panel az Azure AD felügyeleti portál | [Az Azure AD felügyeleti portál: Vállalati alkalmazások](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) |
| Alkalmazás eltávolítása a szükséges Előfeltételek konfigurálása az önkiszolgáló szolgáltatással | [What's new in Azure Active Directory vállalati Alkalmazáskezelés: önkiszolgáló alkalmazás-hozzáférés konfigurálása](active-directory-enterprise-apps-whats-new-azure-portal.md#configure-self-service-application-access) |
| Jelentkezzen be hello adatokat feldolgozó toomy alkalmazások portál | http://myapps.microsoft.com |
| Figyelje meg, "+ alkalmazás hozzáadása a" hello lap op gombjára. Az tooget hozzáférés toohello alkalmazás használata |  |

### <a name="considerations"></a>Megfontolandó szempontok

a kiválasztott hello alkalmazások esetében kiépítés követelményeinek, így azonnal is toohello alkalmazás bizonyos hibákat okozhat. Ha a kiválasztott hello alkalmazás támogatja a létesítést és az azure ad, és van-e konfigurálva, ezzel folyamatként egy lehetőség tooshow hello teljes end tooend működik. Lásd: hello építőeleme [SaaS összevont egyszeri Bejelentkezéses konfigurációs](#saas-federated-sso-configuration) további ajánlások

## <a name="self-service-password-reset"></a>Önkiszolgáló jelszóváltoztatás

Hozzávetőleges időt tooComplete: 15 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| A bérlői önkiszolgáló jelszó-kezelésen. | [Az Azure Active Directory-jelszó alaphelyzetbe állítása a informatikai rendszergazdák](active-directory-passwords.md) |
| A helyszíni jelszavak késleltetve visszaírt toomanage engedélyezése. Megjegyzés: ehhez az adott Azure AD Connect verziók | [Jelszóvisszaírás előfeltételei](active-directory-passwords-writeback.md) |
| Ez a funkció, és győződjön meg arról, hogy egy biztonsági csoport tagjai hello koncepció felhasználók azonosítása. hello felhasználó nem rendszergazda toofully showcase hello funkció kell lennie. | [Testreszabása: Az Azure AD a jelszókezelés: hozzáférés korlátozása toopassword alaphelyzetbe állítása](active-directory-passwords-writeback.md) |


### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Keresse meg a tooAzure AD felügyeleti portál: jelszó alaphelyzetbe állítása | [Azure AD felügyeleti portál: Jelszó alaphelyzetbe állítása](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) |
| Határozza meg, hello jelszó-visszaállítási házirend. Koncepció célokra használhatja a telefonhívás és a Q & a területen. Ajánlott tooenable regisztrációs toobe tooaccess panel bejelentkezéshez szükséges. |  |
| Jelentkezzen ki, majd jelentkezzen be az Infomunkás |  |
| Adja meg a hello önkiszolgáló jelszó-változtatási adatok konfigurált / 2. lépés | http://aka.MS/ssprsetup |
| Bezárás hello böngésző |  |
| A 4. lépésben használt hello Infomunkás hello bejelentkezési folyamat elindítása |  |
| Hello jelszó alaphelyzetbe állítása | [Frissítse a saját jelszavát: a jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md) |
| Próbálja meg jelentkezhetnek be az új jelszó tooAzure AD, valamint a tooon helyszíni erőforrások |  |

### <a name="considerations"></a>Megfontolandó szempontok

1. Ha az Azure AD Connect frissítése hello toocause súrlódás lesz, majd esetleg felhő fiókok ellen, vagy tegye egy bemutató egy külön környezet ellen
2. hello a rendszergazdák egy másik házirendet és hello rendszergazdai fiók tooreset hello jelszó előfordulhat, hogy hello koncepció vágófelülettel és zavart okozhat. Ellenőrizze, hogy a normál felhasználói fiók tootest hello visszaállítási műveletek


## <a name="azure-multi-factor-authentication-with-phone-calls"></a>Az Azure multi-factor Authentication telefonhívást

Hozzávetőleges időt tooComplete: 10 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| Többtényezős Hitelesítést használó Koncepció felhasználók azonosítása  |  |
| A jó fogadása az MFA-kérdést Phone  | [Mi az az Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) |

### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Keresse meg a túl "Felhasználók és csoportok" panel az Azure AD felügyeleti portál | [Az Azure AD felügyeleti portál: Felhasználók és csoportok](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Overview/menuId/) |
| Válassza ki a "Minden felhasználó" panel |  |
| A hello felső menüsoron válasszon "Többtényezős hitelesítés" gombra | Közvetlen URL-CÍMÉT az Azure MFA-portálon: https://aka.ms/mfaportal |
| Hello "User" beállításaiban hello koncepció felhasználók válassza ki, és lehetővé teszi a multi-factor Authentication | [Azure Multi-Factor Authentication – felhasználói állapotok](../multi-factor-authentication/multi-factor-authentication-get-started-user-states.md) |
| Jelentkezzen be a hello koncepció felhasználói és a lépésein végighaladva hello igazolása létrehozása folyamatban  |  |

### <a name="considerations"></a>Megfontolandó szempontok

1. Ez az explicit módon beállítása MFA egy felhasználó összes bejelentkezések építőelem hello koncepció szükséges lépések. Például a feltételes hozzáférés és Identity Protection egyéb eszközöket, amelyek több többtényezős Hitelesítést végezhetnek megcélzott forgatókönyvek. Erre akkor van valami tooconsider Koncepció tooproduction áthelyezésekor.
2. hello koncepció lépéseit a építőelem explicit módon használ telefonhívásokat hello expedience többtényezős hitelesítési módszert. Hogy térjen át a Koncepció tooproduction, azt javasoljuk, alkalmazásokhoz, mint a hello [Microsoft Authenticator](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) a második tényezőként amikor csak lehetséges.
További: [VÁZLAT NIST különleges közlemény 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="mfa-conditional-access-for-saas-applications"></a>Többtényezős hitelesítés feltételes hozzáférés a Szolgáltatottszoftver-alkalmazáshoz

Hozzávetőleges időt tooComplete: 10 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| Koncepció felhasználók tootarget hello házirend azonosítására. Ezek a felhasználók biztonsági csoport tooscope hello feltételes hozzáférési házirendet kell lennie. | [SaaS összevont egyszeri bejelentkezés konfigurálása](#saas-federated-sso-configuration) |
| SaaS-alkalmazás már konfigurálva |  |
| Koncepció felhasználók toohello alkalmazás már hozzá vannak rendelve |  |
| Hitelesítő adatok toohello Koncepció felhasználói érhetők el |  |
| Ez a felhasználó a multi-factor Authentication regisztrálva van. A telefon használata jó fogadása | http://aka.MS/ssprsetup |
| Hello belső hálózati eszköz. Belső hello tartománya konfigurált IP-cím | Az IP-címének: https://www.bing.com/search?q=what%27s+my+ip |
| A külső hálózati hello (egy phone mobil hello vivőjel-hálózat használatával lehet) eszköz |  |

### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Nyissa meg tooAzure AD felügyeleti portál: feltételes hozzáférési panel | [Az Azure AD felügyeleti portál: Feltételes hozzáférés](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) |
| Feltételes hozzáférési szabályzat létrehozása:<br/>-A cél koncepció felhasználók "Felhasználók és csoportok"<br/>-Koncepció célalkalmazás "Felhőalapú alkalmazások" alatt<br/>-Az összes hely cél kivételével megbízható ők "Feltételek" a "Helyek" -> **Megjegyzés:** megbízható IP-címek vannak konfigurálva a [MFA-portálon](https://account.activedirectory.windowsazure.com/UserManagement/MfaSettings.aspx)<br/>-"Grant" a többtényezős hitelesítés megkövetelése | [Feltételes hozzáférés az Azure Active Directoryban az első lépései: a házirend-konfigurációs lépések](active-directory-conditional-access-azure-portal-get-started.md#policy-configuration-steps) |
| Hozzáférés a vállalati hálózaton belüli alkalmazás | [Ismerkedés a feltételes hozzáférés az Azure Active Directoryban: hello házirend tesztelése](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |
| Nyilvános hálózat hozzáférés alkalmazás | [Ismerkedés a feltételes hozzáférés az Azure Active Directoryban: hello házirend tesztelése](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |

### <a name="considerations"></a>Megfontolandó szempontok

Összevonási használatakor használható hello helyszíni identitásszolgáltató (IdP) toocommunicate hello belső/külső JOGCÍMEKKEL rendelkező és a vállalati hálózat állapota. Ezzel a technikával anélkül, hogy az IP-címek, amely előfordulhat, hogy bonyolult tooassess kell, és kezelhetik a nagy méretű szervezeteknek toomanage hello listáját is használhatja. A telepítő által kell hello "központi hálózat" forgatókönyv (a felhasználó naplózás hello belső hálózatról, és közben a kapcsolók helyeken, például egy kávézóban) fiókot, és győződjön meg arról, hogy hello vonatkozó következmények ismertetése. További: [Securing felhőalapú erőforrásokat az Azure többtényezős hitelesítés és az AD FS: megbízható IP-címeinek összevont felhasználók](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md#trusted-ips-for-federated-users)

## <a name="privileged-identity-management-pim"></a>Privileged Identity Management (PIM)

Hozzávetőleges időt tooComplete: 15 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| Hello globális rendszergazda hello Koncepció a PIM szolgáltatásra része lesz azonosítása | [Azure AD Privileged Identity Management használatának megkezdése](active-directory-privileged-identity-management-getting-started.md) |
| Hello globális rendszergazdája lesz a biztonsági rendszergazda hello azonosítása | [Azure AD Privileged Identity Management használatának megkezdése](active-directory-privileged-identity-management-getting-started.md)<br/> [Az Azure Active Directory PIM különböző rendszergazdai szerepkörei](active-directory-privileged-identity-management-roles.md) |
| Választható lehetőség: Győződjön meg róla, ha hello globális rendszergazdák e-mailt kell-e hozzáférni a PIM tooexercise e-mail értesítések | [Mi az Azure AD Privileged Identity Management?: hello szerepkör aktiválási beállításainak konfigurálása](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)


### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Bejelentkezési toohttps://portal.azure.com globális rendszergazda (GA) és a rendszerindítási hello PIM panelen. Globális rendszergazda hajtja végre ezt a lépést hello van rendezés hello biztonsági rendszergazdaként.  Az aktor GA1 most hívása | [Az Azure AD Privileged Identity Management hello biztonsági varázsló segítségével](active-directory-privileged-identity-management-security-wizard.md) |
| Üdvözöljük a globális rendszergazdákat határozza meg és állandó tooeligible át azokat. Egy különálló rendszergazdai egy 1. lépésben a jobb érthetőség kedvéért bizonyos hello a legyen. Az aktor GA2 most hívása | [Az Azure AD Privileged Identity Management: Hogyan tooadd vagy egy felhasználói szerepkör eltávolítása](active-directory-privileged-identity-management-how-to-add-role-to-user.md)<br/>[Mi az Azure AD Privileged Identity Management?: hello szerepkör aktiválási beállításainak konfigurálása](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)  |
| Most jelentkezzen be GA2 toohttps://portal.azure.com, és próbálja meg módosítani a "Felhasználói beállítások". Figyelje meg, több lehetősége is használhatók. | |
| Új lapon hello ugyanabban a munkamenetben, a 3. lépés: most toohttps://portal.azure.com keresse meg és hello PIM panel toohello irányítópult hozzáadása. | [Hogyan tooactivate és szerepkörök az Azure AD Privileged Identity Management inaktiválása: hello Privileged Identity Management alkalmazás felvétele](active-directory-privileged-identity-management-how-to-activate-role.md#add-the-privileged-identity-management-application) |
| Kérelem aktiválási toohello globális rendszergazdai szerepkörrel | [Hogyan tooactivate és szerepkörök az Azure AD Privileged Identity Management inaktiválása: a szerepkör aktiválása](active-directory-privileged-identity-management-how-to-activate-role.md#activate-a-role) |
| Vegye figyelembe, hogy ha GA2 soha nem iratkozott fel az MFA szolgáltatásra, az Azure MFA regisztrációs szükség lesz |  |
| Lépjen vissza a 3. lépés toohello eredeti lapján, majd kattintson a hello hello böngésző Frissítés gombjára. Vegye figyelembe, hogy most már rendelkezik hozzáférési toochange "Felhasználói beállítások" | |
| Ha szükséges Ha a globális rendszergazdák e-mailek engedélyezve van, ellenőrizheti GA1 és GA2 a Beérkezett üzenetek és hello értesítés hello szerepkör aktiválása folyamatban |  |
| 8 hello naplózási előzmények ellenőrizze, és figyelje a hello jelentés tooconfirm hello jogok GA2 jelenik meg. | [Mi az Azure AD Privileged Identity Management?: tekintse át a szerepkör-tevékenység](active-directory-privileged-identity-management-configure.md#review-role-activity) |

### <a name="considerations"></a>Megfontolandó szempontok

Ez a funkció az Azure AD Premium P2 és/vagy az EMS E5 része

## <a name="discovering-risk-events"></a>Kockázati események felderítéséhez

Hozzávetőleges időt tooComplete: 20 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| Tor böngészővel rendelkező eszköz letöltése és telepítése | [Töltse le a Tor-böngésző](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Access tooPOC felhasználói toodo hello bejelentkezés | [Az Azure Active Directory Identity Protection-forgatókönyv](active-directory-identityprotection-playbook.md) |

### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Nyissa meg a tor-böngésző | [Töltse le a Tor-böngésző](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Jelentkezzen be a Koncepció felhasználói fiók hello toohttps://myapps.microsoft.com | [Az Azure Active Directory Identity Protection-forgatókönyv: szimulálva kockázati események](active-directory-identityprotection-playbook.md#simulating-risk-events) |
| Várjon 5-7 percet |  |
| Jelentkezzen be egy globális rendszergazdai toohttps://portal.azure.com, és nyissa meg a hello Identity Protection panelen | https://aka.MS/aadipgetstarted |
| Nyissa meg hello kockázati események panelen. Egy "Bejelentkezéseket névtelen IP-címekről" bejegyzést kell megjelennie  | [Az Azure Active Directory Identity Protection-forgatókönyv: szimulálva kockázati események](active-directory-identityprotection-playbook.md#simulating-risk-events) |

### <a name="considerations"></a>Megfontolandó szempontok

Ez a funkció az Azure AD Premium P2 és/vagy az EMS E5 része

## <a name="deploying-sign-in-risk-policies"></a>Bejelentkezés a kockázat házirendek

Hozzávetőleges időt tooComplete: 10 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| Tor böngészővel rendelkező eszköz letöltése és telepítése | [Töltse le a Tor-böngésző](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| A Koncepció felhasználói toodo hello bejelentkezés tesztelése hozzáférést |  |
| Ez a felhasználó többtényezős hitelesítéssel regisztrálva van. Győződjön meg arról, hogy toouse jó fogadása telefon | Építőelem: [telefonhívásokat az Azure többtényezős hitelesítés](#azure-multi-factor-authentication-with-phone-calls) |


### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| Jelentkezzen be egy globális rendszergazdai toohttps://portal.azure.com és hello Identity Protection paneljének megnyitásához | https://aka.MS/aadipgetstarted |
| A kockázat bejelentkezési házirend engedélyezése az alábbiak szerint:<br/>-Rendelt: Koncepció felhasználó<br/>– Feltételek: Bejelentkezési kockázat közepes vagy újabb rendszer (bejelentkezés névtelen helyről akkor számít elértnek, egy közepes méretű kockázati szint)<br/>-Vezérlők: Többtényezős hitelesítés megkövetelése | [Az Azure Active Directory Identity Protection-forgatókönyv: bejelentkezési kockázata](active-directory-identityprotection-playbook.md#sign-in-risk) |
| Nyissa meg a tor-böngésző | [Töltse le a Tor-böngésző](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Jelentkezzen be a koncepció felhasználói fiók hello toohttps://myapps.microsoft.com |  |
| Értesítés hello MFA-kérdést | [Bejelentkezés során lép az Azure AD Identity Protection: kockázatos bejelentkezési helyreállítási](active-directory-identityprotection-flows.md#risky-sign-in-recovery)

### <a name="considerations"></a>Megfontolandó szempontok

Ez a funkció az Azure AD Premium P2 és/vagy az EMS E5 részét képezi. kockázati események olvashat toolearn: [Azure Active Directory kockázati események](active-directory-reporting-risk-events.md)

## <a name="configuring-certificate-based-authentication"></a>A Tanúsítványalapú hitelesítés konfigurálása

Hozzávetőleges időt toocomplete: 20 perc

### <a name="pre-requisites"></a>Előfeltételek

| Előfeltétel | Erőforrások |
| --- | --- |
| Eszköz felhasználói tanúsítvánnyal kiépítve (Windows, iOS vagy Android) a vállalati nyilvános kulcsú infrastruktúra | [Felhasználói tanúsítványok telepítése](https://msdn.microsoft.com/library/cc770857.aspx) |
| Az AD FS összevont Azure AD-tartomány | [Azure AD Connect és összevonás](./connect/active-directory-aadconnectfed-whatis.md)<br/>[Active Directory tanúsítványszolgáltatások áttekintése](https://technet.microsoft.com/library/hh831740.aspx)|
| Az iOS-eszközök telepítve a Microsoft Authenticator alkalmazás | [Ismerkedés a hello Microsoft Authenticator alkalmazásról](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) |

### <a name="steps"></a>Lépések

| Lépés | Erőforrások |
| --- | --- |
| "Tanúsítvány-hitelesítés" az AD FS engedélyezése | [Hitelesítési házirendek konfigurálása: tooconfigure elsődleges hitelesítési globálisan a Windows Server 2012 R2](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-authentication-policies#to-configure-primary-authentication-globally-in-windows-server-2012-r2) |
| Választható: Engedélyezze a Tanúsítványalapú hitelesítés az Azure AD-ban az Exchange ActiveSync-ügyfelek | [Ismerkedés az Azure Active Directory-alapú hitelesítés](active-directory-certificate-based-authentication-get-started.md) |
| TooAccess panelen keresse meg és a hitelesítés a felhasználói tanúsítvány használatával | https://myapps.microsoft.com |

### <a name="considerations"></a>Megfontolandó szempontok

a központi telepítés figyelmeztetések olvashat toolearn: [az AD FS: tanúsítványhitelesítés az Azure ad-val & Office 365](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)



> [!NOTE]
> Felhasználói tanúsítvány birtokában kell védeni. Vagy eszközök kezelése esetén az intelligens kártyák PIN-kóddal.



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
