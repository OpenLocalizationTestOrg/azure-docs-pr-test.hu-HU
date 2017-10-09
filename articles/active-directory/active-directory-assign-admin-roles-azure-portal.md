---
title: "az Azure Active Directoryban aaaAssigning rendszergazdai szerepkörök |} Microsoft Docs"
description: "Egy rendszergazdai szerepkört is létre vagy felhasználók szerkesztése, tooothers rendszergazdai szerepkörök hozzárendelése, felhasználók új jelszavainak létrehozására, felhasználói licencek kezelése vagy tartományok kezelése. A rendszergazda szerepkörrel felruházott felhasználó rendelkezik hello ugyanazokkal az engedélyekkel minden felhőalapú szolgáltatások toowhich feliratkozott a szervezet között."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7fc27e8e-b55f-4194-9b8f-2e95705fb731
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.reviewer: Vince.Smith
ms.custom: it-pro;
ms.openlocfilehash: 41cddbf311767d9995c99ee386e6d276745dad18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Rendszergazdai jogosultságok kiosztása az Azure Active Directoryban
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-assign-admin-roles-azure-portal.md)
> * [klasszikus Azure portál](active-directory-assign-admin-roles.md)
>
>

Azure Active Directory (Azure AD) segítségével meghatározhat külön rendszergazdák tooserve különböző funkcióihoz. A rendszergazdák hozzáférési toovarious funkciókat hello Azure-portálon vagy a klasszikus Azure portálon kell és, attól függően, a szerepkör lesz készül képes toocreate vagy felhasználók szerkesztése, tooothers rendszergazdai szerepkörök hozzárendelése, felhasználói jelszavak átállítása felhasználói licencek kezelése és tartományok, többek között kezelése. A rendszergazda szerepkörrel felruházott felhasználó hello ugyanazokkal az engedélyekkel hello felhőszolgáltatások, amelyek a szervezet fizet elő, függetlenül attól, hogy hozzárendelt összes szerepkör az Office 365 portál hello hello, vagy a klasszikus Azure portálon hello, vagy hello fog rendelkezni. Az Azure AD-modullal a Windows PowerShell környezethez

a következő rendszergazdai szerepkörök hello érhetők el:

* **Számlázási rendszergazda**: lebonyolítja a vásárlásokat, kezeli az előfizetéseket, támogatási jegyeket, és figyeli a szolgáltatás állapotát.

* **Megfelelőségi rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók jogosult felügyeleti belül hello Office 365 biztonsági és megfelelőségi központ és az Exchange felügyeleti központban. További információ a "[Office 365 rendszergazdai szerepkörök](https://support.office.com/en-us/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)."

* **Szolgáltatás-rendszergazda CRM**: Ezzel a szerepkörrel rendelkező felhasználók globális engedélye belül Microsoft CRM Online, hello szolgáltatás jelen, valamint hello képességét toomanage támogatási jegy megjelenítése és szolgáltatás állapotának figyeléséhez. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Eszközrendszergazdák**: Ezzel a szerepkörrel rendelkező felhasználók a helyi számítógép rendszergazdák összes Windows 10 rendszerű eszközökön, amelyek összeillesztett tooAzure Active Directory válnak. Az Azure Active Directoryban nem rendelkeznek az hello képességét toomanage eszközök objektumok.

* **Directory olvasók**: egy örökölt szerepkör, amely hozzárendelt toobe tooapplications, amelyek nem támogatják az hello [hozzájárulás keretrendszer](active-directory-integrating-applications.md). Azt nem rendelhetők tooany felhasználók.

* **Címtár-szinkronizálás fiókok**: ne használja. Ez a szerepkör toohello az Azure AD Connect szolgáltatást, automatikusan és nem készült vagy bármely más használata támogatott.

* **Directory írók**: egy örökölt szerepkör, amely hozzárendelt toobe tooapplications, amelyek nem támogatják az hello [hozzájárulás keretrendszer](active-directory-integrating-applications.md). Azt nem rendelhetők tooany felhasználók.

* **Exchange szolgáltatás-rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók engedélye globális belül a Microsoft Exchange online-hoz, ha jelen hello szolgáltatást. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Globális rendszergazda / vállalati rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók hozzáférési tooall felügyeleti funkciókat rendelkezik az Azure Active Directoryban, valamint, hogy federate tooAzure Exchange Online, SharePoint online-hoz, például az Active Directory-szolgáltatások és Skype vállalati Online verzióhoz. hello személy hello Azure Active Directory-bérlő globális rendszergazdája lesz. Csak a globális rendszergazdák egyéb rendszergazdai szerepköröket rendelhet. A vállalat a egynél több globális rendszergazda is lehet. Globális rendszergazda alaphelyzetbe állíthatja a hello jelszavát, minden olyan felhasználó, és más rendszergazdák számára.

  > [!NOTE]
  > A Microsoft Graph API-val, a Azure AD Graph API és az Azure AD PowerShell a szerepkör azonosítja "Vállalati rendszergazda". "Globális rendszergazda" hello [Azure-portálon](https://portal.azure.com).
  >
  >

* **Vendég meghívó**: a szerepet betöltő felhasználók kezelhetik az Azure Active Directory B2B Vendég felhasználó meghívókat, amikor hello "Tagok kérhetnek" felhasználói beállítása tooNo. További információ a következő B2B együttműködés [kapcsolatos hello Azure AD B2B együttműködés előzetes verziója](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Nem tartalmazza azokat az engedélyeket.

* **Intune szolgáltatás-rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók engedélye globális belül a Microsoft Intune Online, ha jelen hello szolgáltatást. Ez a szerepkör emellett hello képességét toomanage felhasználók és eszközök a rendelés tooassociate házirend tartalmazza, valamint hozzon létre, és csoportok kezelése.

* **Postaláda-rendszergazda**: ezt a szerepkört csak használja az Exchange Online e-mailes támogatást részeként RIM Blackberry eszközöket. Ha a szervezete nem használja az Exchange Online e-mailek RIM Blackberry eszközön, ne használja ezt a szerepkört.

* **1 támogatási réteg partneri**: ne használja. Ez a szerepkör elavult és törlődni fognak a jövőbeli hello Azure AD. Ez a szerepkör van néhány továbbértékesítése Microsoft-partnerek számára készült, és nem általános használatra szánt.

* **Támogatási réteg 2 partnereinek**: ne használja. Ez a szerepkör elavult és törlődni fognak a jövőbeli hello Azure AD. Ez a szerepkör van néhány továbbértékesítése Microsoft-partnerek számára készült, és nem általános használatra szánt.

* **Jelszókezelő / segélyszolgálat rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók jelszavak alaphelyzetbe állítása, szolgáltatáskérések kezelése és figyelése a szolgáltatás állapotát. Jelszókezelők jelszavát állíthatják alaphelyzetbe állíthatja a jelszavakat, csak a felhasználók és más jelszókezelők jelszavát állíthatják.

  > [!NOTE]
  > A Microsoft Graph API-val, a Azure AD Graph API és az Azure AD PowerShell a szerepkör azonosítja "Segélyszolgálat rendszergazda". "Jelszó-rendszergazda" hello [Azure-portálon](https://portal.azure.com/).
  >
  >
  
* **A Power BI szolgáltatás-rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók kell globális engedéllyel a Microsoft Power BI, hello szolgáltatás esetén hello képességét toomanage továbbá a támogatási jegy megjelenítése, és a szolgáltatás állapotának figyeléséhez. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/en-us/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-001&ad=US).

* **Emelt szintű rendszergazdai szerepkör**: Ezzel a szerepkörrel rendelkező felhasználók kezelhetik a szerepkör-hozzárendelések az Azure Active Directoryban, valamint az Azure AD Privileged Identity Management belül. Emellett ez a szerepkör lehetővé teszi a Privileged Identity Management minden szempontját kezelését.

* **Biztonsági rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók rendelkeznek az összes hello olvasási engedélyek hello biztonsági olvasó szerepkört, plusz hello képességét toomanage konfigurációs biztonsággal kapcsolatos szolgáltatások: Azure Active Directory azonosító adatok védelmét, Privileged Identity Management és az Office 365 biztonsági és megfelelőségi központ. Office 365 engedélyekkel kapcsolatos további információt [hello Office 365 biztonsági és megfelelőségi központ engedélyek](https://support.office.com/en-us/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Biztonsági olvasó**: Ezzel a szerepkörrel rendelkező felhasználók többek között az összes adatot az Azure Active Directory, a azonosító adatok védelmét, a Privileged Identity Management globális csak olvasási hozzáféréssel rendelkezik, valamint hello képességét tooread Azure Active Directory-bejelentkezés a jelentések és a naplókat. hello szerepkör is biztosít az Office 365 biztonsági és megfelelőségi központ olvasási engedéllyel. Office 365 engedélyekkel kapcsolatos további információt [hello Office 365 biztonsági és megfelelőségi központ engedélyek](https://support.office.com/en-us/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Szolgáltatás-rendszergazda támogatja**: Ezzel a szerepkörrel rendelkező felhasználók megnyithatják a támogatási kérelmek a Microsoft Azure és az Office 365 szolgáltatáshoz, és nézetek hello hello Azure-portálra szolgáltatás-irányítópult és az üzenet központ és az Office 365 felügyeleti portálon. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **A SharePoint szolgáltatás-rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók globális engedélye belül a Microsoft SharePoint Online, hello szolgáltatás jelen, valamint hello képességét toomanage támogatási jegy megjelenítése és szolgáltatás állapotának figyeléséhez. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **A Skype vállalati verzió / Lync szolgáltatás-rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók engedélye globális belül Microsoft Skype vállalati, ha jelen hello szolgáltatást, valamint kezelheti a Skype-specifikus felhasználói attribútumok az Azure Active Directoryban. Emellett a szerepkör biztosít hello képességét toomanage támogatási jegyek és monitor szolgáltatás állapotát. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  > [!NOTE]
  > A Microsoft Graph API-val, a Azure AD Graph API és az Azure AD PowerShell a szerepkör azonosítja "Lync szolgáltatás-rendszergazda". "Skype az üzleti szolgáltatás-rendszergazda" hello [Azure-portálon](https://portal.azure.com/).
  >
  >

* **Felhasználói fiók rendszergazdai**: Ezzel a szerepkörrel rendelkező felhasználók hozhat létre és kezelheti a felhasználók és csoportok összes tulajdonságát. Emellett a szerepkör magában foglalja a hello képességét toomanage támogatási jegyeket, és figyeli a szolgáltatás állapotát. Bizonyos korlátozások vonatkoznak. Például a szerepkör nem teszi lehetővé a globális rendszergazda törlése, és lehetővé teszi a nem rendszergazda jelszavak módosítása, amíg nem teszi lehetővé a globális rendszergazdák jelszavakat és egyéb jogosultsággal rendelkező rendszergazdák módosítása.

## <a name="administrator-permissions"></a>Rendszergazdai engedélyek

### <a name="billing-administrator"></a>Számlázási rendszergazda

| Teheti meg | Nem hajtható végre |
| --- | --- |
|<p>Vállalati és felhasználói adatok megtekintése</p><p>Office támogatási jegyek kezelése</p><p>Office-termékek számlázási és beszerzési műveletek végrehajtása</p> |<p>Felhasználói jelszavak átállítása</p><p>Hozzon létre és kezelheti a felhasználói nézetek</p><p>Létrehozása, szerkesztése, törlése a felhasználók és csoportok és felhasználói licencek kezelése</p><p>Tartományok kezelése</p><p>Vállalati adatok kezelése</p><p>Rendszergazdai szerepkörök tooothers delegálása</p><p>Címtár-szinkronizálás</p><p>Auditnaplók megtekintése</p>|

### <a name="global-administrator"></a>Globális rendszergazda
| Teheti meg | Nem hajtható végre |
| --- | --- |
| <p>Vállalati és felhasználói adatok megtekintése</p><p>Office támogatási jegyek kezelése</p><p>Office-termékek számlázási és beszerzési műveletek végrehajtása</p><p>Felhasználói jelszavak átállítása</p>
<p>Állítsa vissza a többi rendszergazda jelszavát</p> <p>Hozzon létre és kezelheti a felhasználói nézetek</p><p>Létrehozása, szerkesztése, törlése a felhasználók és csoportok és felhasználói licencek kezelése</p><p>Tartományok kezelése</p><p>Vállalati adatok kezelése</p><p>Rendszergazdai szerepkörök tooothers delegálása</p><p>Címtár-szinkronizálás</p><p>Engedélyezheti vagy tilthatja le a többtényezős hitelesítés</p><p>Auditnaplók megtekintése</p> |N/A |

### <a name="password-administrator"></a>Jelszókezelő
| Teheti meg | Nem hajtható végre |
| --- | --- |
| <p>Vállalati és felhasználói adatok megtekintése</p><p>Office támogatási jegyek kezelése</p><p>Felhasználói jelszavak átállítása</p> <p>Állítsa vissza a többi rendszergazda jelszavát</p>|<p>Office-termékek számlázási és beszerzési műveletek végrehajtása</p><p>Hozzon létre és kezelheti a felhasználói nézetek</p><p>Létrehozása, szerkesztése, törlése a felhasználók és csoportok és felhasználói licencek kezelése</p><p>Tartományok kezelése</p><p>Vállalati adatok kezelése</p><p>Rendszergazdai szerepkörök tooothers delegálása</p><p>Címtár-szinkronizálás</p><p>Jelentések megtekintése</p>|

### <a name="service-administrator"></a>Szolgáltatás-rendszergazda
| Teheti meg | Nem hajtható végre |
| --- | --- |
| <p>Vállalati és felhasználói adatok megtekintése</p><p>Office támogatási jegyek kezelése</p> |<p>Felhasználói jelszavak átállítása</p><p>Office-termékek számlázási és beszerzési műveletek végrehajtása</p><p>Hozzon létre és kezelheti a felhasználói nézetek</p><p>Létrehozása, szerkesztése, törlése a felhasználók és csoportok és felhasználói licencek kezelése</p><p>Tartományok kezelése</p><p>Vállalati adatok kezelése</p><p>Rendszergazdai szerepkörök tooothers delegálása</p><p>Címtár-szinkronizálás</p><p>Auditnaplók megtekintése</p> |

### <a name="user-administrator"></a>Felhasználó rendszergazda
| Teheti meg | Nem hajtható végre |
| --- | --- |
| <p>Vállalati és felhasználói adatok megtekintése</p><p>Office támogatási jegyek kezelése</p><p>Felhasználók új jelszavainak létrehozására, korlátozásokkal.</p><p>Állítsa vissza a többi rendszergazda jelszavát</p><p>Más felhasználók jelszavát alaphelyzetbe állítása</p><p>Hozzon létre és kezelheti a felhasználói nézetek</p><p>Létrehozása, szerkesztése, és a felhasználók és csoportok törlésére és korlátozásokkal a felhasználói licencek kezelése. Őt nem törölhet globális rendszergazdát, és hozhat létre más rendszergazdákat.</p> |<p>Office-termékek számlázási és beszerzési műveletek végrehajtása</p><p>Tartományok kezelése</p><p>Vállalati adatok kezelése</p><p>Rendszergazdai szerepkörök tooothers delegálása</p><p>Címtár-szinkronizálás</p><p>Engedélyezheti vagy tilthatja le a többtényezős hitelesítés</p><p>Auditnaplók megtekintése</p> |

### <a name="security-reader"></a>Biztonsági olvasó
| A | Teheti meg |
| --- | --- |
| Identitás-kezelési központ |Olvassa el az összes biztonsági jelentések és beállítási információk biztonsági szolgáltatások<ul><li>Levélszemét<li>Titkosítás<li>Adatveszteség-megelőzési<li>Kártevőirtó<li>Az Advanced threat protection<li>Adathalászat elleni<li>Mailflow szabályok |
| Privileged Identity Management |<p>Az Azure AD PIM illesztett csak olvasási hozzáféréssel tooall információt tartalmaz: házirendeket és az Azure AD szerepkör-hozzárendelések jelentéseinek, ellenőrzi, hogy biztonsági és hello a jövőben olvasási hozzáférés toopolicy adatokkal és jelentésekkel forgatókönyvek mellett az Azure AD szerepkör-hozzárendelés.<p>**Nem lehet** előfizetés az Azure AD PIM, vagy ellenőrizze a módosítások tooit. A PIM a portálon, vagy a PowerShell segítségével (például a globális rendszergazda vagy a kiemelt szerepkör felügyeleti), a további szerepkörök valaki a szerepkör is aktiválnak, ha hello felhasználói jelöltségét ellenőrző őket. |
| <p>A figyelő az Office 365 szolgáltatás állapota</p><p>Az Office 365 biztonsági és megfelelőségi központ</p> |<ul><li>Olvassa el és kezelheti a riasztásokat<li>Olvassa el a biztonsági házirendek<li>Olvassa el a fenyegetésfelderítési adataival, a Cloud App Discovery és a Keresés és a vizsgálat karantén<li>Olvassa el az összes jelentés |

### <a name="security-administrator"></a>Biztonsági rendszergazda
| A | Teheti meg |
| --- | --- |
| Identitás-kezelési központ |<ul><li>Hello biztonsági olvasó szerepkört az összes engedélyt.<li>Emellett hello képességét tooperform jelszavak alaphelyzetbe állítását kivételével minden IPC műveletet. |
| Privileged Identity Management |<ul><li>Hello biztonsági olvasó szerepkört az összes engedélyt.<li>**Nem lehet** kezelése az Azure AD szerepkörtagságok vagy beállítások. |
| <p>A figyelő az Office 365 szolgáltatás állapota</p><p>Az Office 365 biztonsági és megfelelőségi központ |<ul><li>Hello biztonsági olvasó szerepkört az összes engedélyt.<li>Minden beállításait is konfigurálhatja a hello Advanced Threat Protection szolgáltatás (kártevők & vírus védelmet, rosszindulatú URL-cím config, URL nyomkövetésének, stb.). |

## <a name="details-about-hello-global-administrator-role"></a>Hello globális rendszergazdai szerepkörrel részleteit
hello globális rendszergazdai hozzáférési tooall felügyeleti funkciókat tartalmaz. Alapértelmezés szerint hello személy, aki az Azure-előfizetésre regisztrál van hello globális rendszergazdai szerepkörrel hello könyvtár. Csak a globális rendszergazdák egyéb rendszergazdai szerepköröket rendelhet.

### <a name="tooadd-a-colleague-as-a-global-administrator"></a>a globális rendszergazdai munkatárs tooadd

1. Jelentkezzen be toohello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) egy olyan fiókkal, amely hello bérlő címtár globális rendszergazdája.

   ![Az azure AD felügyeleti központ megnyitása](./media/active-directory-assign-admin-roles-azure-portal/active-directory-admin-center.png)

2. Válassza ki **felhasználók és csoportok &gt; minden felhasználó**

3. Keresse meg azt szeretné, hogy toodesignate globális rendszergazdaként, és nyissa meg az adott felhasználó hello panelt hello felhasználót.

4. Hello felhasználói paneljén válassza **Directory szerepkör**.
 
5. A hello directory szerepkör panelen válassza ki a hello **globális rendszergazda** szerepkör, és mentse.

## <a name="assign-or-remove-administrator-roles"></a>Rendelje hozzá, vagy távolítsa el a rendszergazdai szerepkörökről
Hogyan tooassign rendszergazdai szerepkörei tooa felhasználói az Azure Active Directoryban: toolearn [rendelje hozzá a felhasználó tooadministrator szerepkörök az Azure Active Directory előzetes](active-directory-users-assign-role-azure-portal.md).

## <a name="deprecated-roles"></a>Elavult szerepkörök

a következő szerepkörök hello nem használható. Ezek már elavult, és az Azure AD-t jövőbeli hello törlődni fog.

* Ad hoc licenc rendszergazda
* E-mailek ellenőrzése létrehozó felhasználó
* Csatlakozás bármilyen eszközről
* Eszköz-kezelők
* Eszközök felhasználói
* Munkahelyi csatlakozás bármilyen eszközről

## <a name="next-steps"></a>Következő lépések

* További részletek toolearn, hogyan toochange rendszergazdák az Azure-előfizetéssel, lásd: [hogyan tooadd, vagy módosítsa az Azure rendszergazdai szerepkörök](../billing-add-change-azure-subscription-administrator.md)
* toolearn hogyan szabályozott erőforrások elérése a Microsoft Azure-ban kapcsolatos további információkért lásd: [az az Azure-erőforrások hozzáférésének megismerése](active-directory-understanding-resource-access.md)
* Hogyan kapcsolódik az Azure Active Directory a tooyour Azure-előfizetés további információkért lásd: [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directoryval](active-directory-how-subscriptions-associated-directory.md)
* [Felhasználók kezelése](active-directory-create-users.md)
* [Jelszavak kezelése](active-directory-manage-passwords.md)
* [Csoportok kezelése](active-directory-manage-groups.md)
