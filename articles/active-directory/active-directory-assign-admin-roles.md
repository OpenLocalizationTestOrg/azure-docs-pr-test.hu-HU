---
title: "Rendszergazdai szerepkörök hozzárendelése az Azure Active Directory |} Microsoft Docs"
description: "Egy rendszergazda szerepkör létrehozása vagy szerkesztése a felhasználók, rendszergazdai szerepkörök hozzárendelése, felhasználók új jelszavainak létrehozására, felhasználói licencek kezelése vagy tartományok kezelése használható. A rendszergazda szerepkörrel felruházott felhasználó ugyanazokkal az engedélyekkel rendelkezik, amelyhez a szervezet előfizetett összes felhőszolgáltatás."
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
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 042e2f4117a35e80694a1643dd95fa54d508f1f7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Rendszergazdai jogosultságok kiosztása az Azure Active Directoryban
> [!div class="op_single_selector"]
> * [Azure Portal]()
> * [klasszikus Azure portál](active-directory-assign-admin-roles.md)
>
>

Azure Active Directory (Azure AD) segítségével különböző célokból külön rendszergazdáinak. A rendszergazdák férhetnek hozzá az Azure-portálon vagy a klasszikus Azure portálon, és attól függően, hogy a szerepkör a kijelölt szolgáltatások, fog tudni létrehozni vagy felhasználók szerkesztése, rendszergazdai szerepkörök hozzárendelése mások, felhasználók új jelszavainak létrehozására, felhasználói licencek kezelése és kezelhetik a tartományokat, többek között. A rendszergazda szerepkörrel felruházott felhasználó ugyanazokkal az engedélyekkel rendelkeznek az összes, amelyhez a szervezet kér le, függetlenül attól, hogy rendelje hozzá a szerepkört az Office 365 portál és a klasszikus Azure portálon vagy az Azure AD-modul segítségével felhőszolgáltatások e Microsoft PowerShell.

> [!IMPORTANT]
> A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett. Arról, hogyan rendelhet hozzá rendszergazdai szerepköröket az Azure AD felügyeleti központban című [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md).


A következő rendszergazdai szerepkörök állnak rendelkezésre:

* **Számlázási rendszergazda**: lebonyolítja a vásárlásokat, kezeli az előfizetéseket, támogatási jegyeket, és figyeli a szolgáltatás állapotát.

* **Megfelelőségi rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók jogosult felügyeleti belül a Office 365 biztonsági és megfelelőségi központ és az Exchange felügyeleti központban. További információ a "[Office 365 rendszergazdai szerepkörök](https://support.office.com/en-us/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)."

* **Feltételes hozzáférés rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók képesek az Azure Active Directory feltételes hozzáférési beállításainak kezelése.

* **Szolgáltatás-rendszergazda CRM**: Ezzel a szerepkörrel rendelkező felhasználók rendelkeznek-e Microsoft CRM Online-hoz, ha jelen a szolgáltatás globális engedéllyel, valamint a támogatási jegyek kezelése és figyelése a szolgáltatás állapotát. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Eszközrendszergazdák**: Ezzel a szerepkörrel rendelkező felhasználók válnak a helyi számítógép rendszergazdák az összes Azure Active Directory tartományhoz csatlakoztatott Windows 10-eszközökön. Nem rendelkeznek képes kezelni az eszközöket objektumok Azure Active Directoryban.

* **Directory olvasók**: egy örökölt szerepkör, amely hozzá kell rendelni, amelyek nem támogatják az alkalmazások a [hozzájárulás keretrendszer](active-directory-integrating-applications.md). Azt nem rendelhető hozzá azokat a felhasználókat.

* **Címtár-szinkronizálás fiókok**: ne használja. Ez a szerepkör lesz automatikusan hozzárendelve az Azure AD Connect szolgáltatást, és nem szánt vagy bármely más használata támogatott.

* **Directory írók**: egy örökölt szerepkör, amely hozzá kell rendelni, amelyek nem támogatják az alkalmazások a [hozzájárulás keretrendszer](active-directory-integrating-applications.md). Azt nem rendelhető hozzá azokat a felhasználókat.

* **Exchange szolgáltatás-rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók engedélye globális belül a Microsoft Exchange online-hoz, ha a szolgáltatás jelen. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Globális rendszergazda / vállalati rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók férhetnek hozzá az Azure Active Directoryval, valamint a szolgáltatásokról, amelyek összevonni az Azure Active Directory Exchange Online, SharePoint online-hoz, például az összes felügyeleti funkcióhoz és Skype vállalati Online verzióhoz. A személy, aki az Azure Active Directory-bérlő előfizet egy globális rendszergazdája lesz. Csak a globális rendszergazdák egyéb rendszergazdai szerepköröket rendelhet. A vállalat a egynél több globális rendszergazda is lehet. Globális rendszergazda alaphelyzetbe állíthatja a jelszavát, minden olyan felhasználó, és más rendszergazdák számára.

  > [!NOTE]
  > A Microsoft Graph API-val, a Azure AD Graph API és az Azure AD PowerShell a szerepkör azonosítja "Vállalati rendszergazda". Az "Globális rendszergazda" a [Azure-portálon](https://portal.azure.com).
  >
  >

* **Vendég meghívó**: a szerepet betöltő felhasználók kezelhetik az Azure Active Directory B2B Vendég felhasználó meghívókat, amikor a felhasználó "Tagok kérhetnek" beállítás értéke nem. További információ a következő B2B együttműködés [kapcsolatban az Azure AD B2B együttműködés előzetes verziója](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Nem tartalmazza azokat az engedélyeket.

* **Intune szolgáltatás-rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók engedélye globális belül a Microsoft Intune Online, ha a szolgáltatás jelen. Ez a szerepkör emellett képes kezelni a felhasználók és eszközök számára ahhoz, hogy társítja a házirendet, valamint a csoportok létrehozásához és kezeléséhez tartalmazza.

* **Postaláda-rendszergazda**: ezt a szerepkört csak használja az Exchange Online e-mailes támogatást részeként RIM Blackberry eszközöket. Ha a szervezete nem használja az Exchange Online e-mailek RIM Blackberry eszközön, ne használja ezt a szerepkört.

* **1 támogatási réteg partneri**: ne használja. Ez a szerepkör elavult, és eltávolítja az Azure AD a jövőben. Ez a szerepkör van néhány továbbértékesítése Microsoft-partnerek számára készült, és nem általános használatra szánt.

* **Támogatási réteg 2 partnereinek**: ne használja. Ez a szerepkör elavult, és eltávolítja az Azure AD a jövőben. Ez a szerepkör van néhány továbbértékesítése Microsoft-partnerek számára készült, és nem általános használatra szánt.

* **Jelszókezelő / segélyszolgálat rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók jelszavak alaphelyzetbe állítása, szolgáltatáskérések kezelése és figyelése a szolgáltatás állapotát. Jelszókezelők jelszavát állíthatják alaphelyzetbe állíthatja a jelszavakat, csak a felhasználók és más jelszókezelők jelszavát állíthatják.

  > [!NOTE]
  > A Microsoft Graph API-val, a Azure AD Graph API és az Azure AD PowerShell a szerepkör azonosítja "Segélyszolgálat rendszergazda". A "Jelszó-rendszergazda" a [Azure-portálon](https://portal.azure.com/).
  >
  >
  
* **A Power BI szolgáltatás-rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók rendelkeznek-e a Microsoft Power bi-ban, ha a szolgáltatás jelenlegi globális engedéllyel, valamint a támogatási jegyek kezelése és figyelése a szolgáltatás állapotát. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/en-us/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-001&ad=US).

* **Emelt szintű rendszergazdai szerepkör**: Ezzel a szerepkörrel rendelkező felhasználók kezelhetik a szerepkör-hozzárendelések az Azure Active Directoryban, valamint az Azure AD Privileged Identity Management belül. Emellett ez a szerepkör lehetővé teszi a Privileged Identity Management minden szempontját kezelését.

* **Biztonsági rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók rendelkeznek a csak olvasási engedéllyel a biztonsági olvasó szerepkört, és képes kezelni a biztonsággal kapcsolatos szolgáltatások konfigurációs: Azure Active Directory azonosító adatok védelmét és az Azure- Adatvédelem, Privileged Identity Management és az Office 365 biztonsági és megfelelőségi központ. Office 365 engedélyekkel kapcsolatos további információt [engedélyek az Office 365 biztonsági és megfelelőségi központ](https://support.office.com/en-us/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Biztonsági olvasó**: Ezzel a szerepkörrel rendelkező felhasználók globális csak olvasási hozzáféréssel rendelkezik, többek között az összes adatot az Azure Active Directory, a azonosító adatok védelmét, a Privileged Identity Management, valamint Azure Active Directory bejelentkezési jelentések Olvasás és a naplók. A szerepkör is biztosít az Office 365 biztonsági és megfelelőségi központ olvasási engedéllyel. Office 365 engedélyekkel kapcsolatos további információt [engedélyek az Office 365 biztonsági és megfelelőségi központ](https://support.office.com/en-us/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Szolgáltatás-rendszergazda támogatja**: Ezzel a szerepkörrel rendelkező felhasználók megnyitható támogatási kérelmek Microsoft Azure és az Office 365-szolgáltatások és a szolgáltatás irányítópultját és a üzenet center az Azure portál és az Office 365 felügyeleti portálon található nézetek. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **A SharePoint szolgáltatás-rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók rendelkeznek-e a Microsoft SharePoint online-hoz, ha a szolgáltatás jelenlegi globális engedéllyel, valamint a támogatási jegyek kezelése és figyelése a szolgáltatás állapotát. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **A Skype vállalati verzió / Lync szolgáltatás-rendszergazda**: Ezzel a szerepkörrel rendelkező felhasználók engedélye globális belül Microsoft Skype vállalati, ha a szolgáltatás jelen, valamint kezelheti a Skype-specifikus felhasználói attribútumok az Azure Active Directoryban. Emellett a szerepkörök a támogatási jegyek kezelése és figyelése a szolgáltatás állapotát. További információ: [Office 365 rendszergazdai szerepkörök](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  > [!NOTE]
  > A Microsoft Graph API-val, a Azure AD Graph API és az Azure AD PowerShell a szerepkör azonosítja "Lync szolgáltatás-rendszergazda". Az "Skype az üzleti szolgáltatás-rendszergazda" a [Azure-portálon](https://portal.azure.com/).
  >
  >

* **Felhasználói fiók rendszergazdai**: Ezzel a szerepkörrel rendelkező felhasználók hozhat létre és kezelheti a felhasználók és csoportok összes tulajdonságát. Emellett ez a szerepkör lehetővé teszi a támogatási jegyek kezelése, és figyeli a szolgáltatás állapotát. Bizonyos korlátozások vonatkoznak. Például a szerepkör nem teszi lehetővé a globális rendszergazda törlése, és lehetővé teszi a nem rendszergazda jelszavak módosítása, amíg nem teszi lehetővé a globális rendszergazdák jelszavakat és egyéb jogosultsággal rendelkező rendszergazdák módosítása.

## <a name="administrator-permissions"></a>Rendszergazdai engedélyek

### <a name="billing-administrator"></a>Számlázási rendszergazda

| Teheti meg | Nem hajtható végre |
| --- | --- |
|<p>Vállalati és felhasználói adatok megtekintése</p><p>Office támogatási jegyek kezelése</p><p>Office-termékek számlázási és beszerzési műveletek végrehajtása</p> |<p>Felhasználói jelszavak átállítása</p><p>Hozzon létre és kezelheti a felhasználói nézetek</p><p>Létrehozása, szerkesztése, törlése a felhasználók és csoportok és felhasználói licencek kezelése</p><p>Tartományok kezelése</p><p>Vállalati adatok kezelése</p><p>Rendszergazdai szerepkörök delegálása</p><p>Címtár-szinkronizálás</p><p>Auditnaplók megtekintése</p>|

### <a name="conditional-access-administrator"></a>Feltételes hozzáférés rendszergazda
| Teheti meg | Nem hajtható végre |
| --- | --- |
|<p>Vállalati és felhasználói adatok megtekintése</p><p>Feltételes hozzáférési beállításainak kezelése</p> |<p>Felhasználói jelszavak átállítása</p><p>Hozzon létre és kezelheti a felhasználói nézetek</p><p>Létrehozása, szerkesztése, törlése a felhasználók és csoportok és felhasználói licencek kezelése</p><p>Tartományok kezelése</p><p>Vállalati adatok kezelése</p><p>Rendszergazdai szerepkörök delegálása</p><p>Címtár-szinkronizálás</p><p>Auditnaplók megtekintése</p>|

### <a name="global-administrator"></a>Globális rendszergazda
| Teheti meg | Nem hajtható végre |
| --- | --- |
|<p>Vállalati és felhasználói adatok megtekintése</p><p>Office támogatási jegyek kezelése</p><p>Office-termékek számlázási és beszerzési műveletek végrehajtása</p><p>Felhasználói jelszavak átállítása</p><p>Állítsa vissza a többi rendszergazda jelszavát</p><p>Hozzon létre és kezelheti a felhasználói nézetek</p><p>Létrehozása, szerkesztése, törlése a felhasználók és csoportok és felhasználói licencek kezelése</p><p>Tartományok kezelése</p><p>Vállalati adatok kezelése</p><p>Rendszergazdai szerepkörök delegálása</p><p>Címtár-szinkronizálás</p><p>Engedélyezheti vagy tilthatja le a többtényezős hitelesítés</p><p>Auditnaplók megtekintése</p> |<p>N/A</p>|

### <a name="password-administrator"></a>Jelszókezelő
| Teheti meg | Nem hajtható végre |
| --- | --- |
| <p>Vállalati és felhasználói adatok megtekintése</p><p>Office támogatási jegyek kezelése</p><p>Felhasználói jelszavak átállítása</p> <p>Állítsa vissza a többi rendszergazda jelszavát</p>|<p>Office-termékek számlázási és beszerzési műveletek végrehajtása</p><p>Hozzon létre és kezelheti a felhasználói nézetek</p><p>Létrehozása, szerkesztése, törlése a felhasználók és csoportok és felhasználói licencek kezelése</p><p>Tartományok kezelése</p><p>Vállalati adatok kezelése</p><p>Rendszergazdai szerepkörök delegálása</p><p>Címtár-szinkronizálás</p><p>Jelentések megtekintése</p>|

### <a name="service-administrator"></a>Szolgáltatás-rendszergazda
| Teheti meg | Nem hajtható végre |
| --- | --- |
| <p>Vállalati és felhasználói adatok megtekintése</p><p>Office támogatási jegyek kezelése</p> |<p>Felhasználói jelszavak átállítása</p><p>Office-termékek számlázási és beszerzési műveletek végrehajtása</p><p>Hozzon létre és kezelheti a felhasználói nézetek</p><p>Létrehozása, szerkesztése, törlése a felhasználók és csoportok és felhasználói licencek kezelése</p><p>Tartományok kezelése</p><p>Vállalati adatok kezelése</p><p>Rendszergazdai szerepkörök delegálása</p><p>Címtár-szinkronizálás</p><p>Auditnaplók megtekintése</p> |

### <a name="user-administrator"></a>Felhasználó rendszergazda
| Teheti meg | Nem hajtható végre |
| --- | --- |
| <p>Vállalati és felhasználói adatok megtekintése</p><p>Office támogatási jegyek kezelése</p><p>Felhasználók új jelszavainak létrehozására, korlátozásokkal.</p><p>Állítsa vissza a többi rendszergazda jelszavát</p><p>Más felhasználók jelszavát alaphelyzetbe állítása</p><p>Hozzon létre és kezelheti a felhasználói nézetek</p><p>Létrehozása, szerkesztése, és a felhasználók és csoportok törlésére és korlátozásokkal a felhasználói licencek kezelése. Őt nem törölhet globális rendszergazdát, és hozhat létre más rendszergazdákat.</p> |<p>Office-termékek számlázási és beszerzési műveletek végrehajtása</p><p>Tartományok kezelése</p><p>Vállalati adatok kezelése</p><p>Rendszergazdai szerepkörök delegálása</p><p>Címtár-szinkronizálás</p><p>Engedélyezheti vagy tilthatja le a többtényezős hitelesítés</p><p>Auditnaplók megtekintése</p> |

### <a name="security-reader"></a>Biztonsági olvasó
| A | Teheti meg |
| --- | --- |
| Identitás-kezelési központ |Olvassa el az összes biztonsági jelentések és beállítási információk biztonsági szolgáltatások<ul><li>Levélszemét<li>Titkosítás<li>Adatveszteség-megelőzési<li>Kártevőirtó<li>Az Advanced threat protection<li>Adathalászat elleni<li>Mailflow szabályok |
| Privileged Identity Management |<p>Csak olvasási hozzáféréssel el az összes információ illesztett rendelkezik Azure AD PIM: házirendeket és az Azure AD szerepkör-hozzárendelések jelentéseinek, biztonsági ellenőrzi, és a jövőben olvasási hozzáféréssel házirend adatokkal és jelentésekkel forgatókönyvek mellett az Azure AD szerepkör-hozzárendelés.<p>**Nem lehet** előfizetés az Azure AD PIM, vagy végezze el a módosításokat. PIM a portálon, vagy a PowerShell segítségével valaki a szerepkör további szerepkörök (például a globális rendszergazda vagy a kiemelt szerepkör rendszergazda), ha a felhasználó őket jelöltségét ellenőrző aktiválhatja. |
| <p>A figyelő az Office 365 szolgáltatás állapota</p><p>Az Office 365 biztonsági és megfelelőségi központ</p> |<ul><li>Olvassa el és kezelheti a riasztásokat<li>Olvassa el a biztonsági házirendek<li>Olvassa el a fenyegetésfelderítési adataival, a Cloud App Discovery és a Keresés és a vizsgálat karantén<li>Olvassa el az összes jelentés |

### <a name="security-administrator"></a>Biztonsági rendszergazda
| A | Teheti meg |
| --- | --- |
| Identitás-kezelési központ |<ul><li>Az olvasó biztonsági szerepkörhöz tartozó jogosultságok.<li>Emellett lehetővé teszi jelszó alaphelyzetbe állítása kivételével minden IPC műveletek végrehajtásához. |
| Privileged Identity Management |<ul><li>Az olvasó biztonsági szerepkörhöz tartozó jogosultságok.<li>**Nem lehet** kezelése az Azure AD szerepkörtagságok vagy beállítások. |
| <p>A figyelő az Office 365 szolgáltatás állapota</p><p>Az Office 365 biztonsági és megfelelőségi központ |<ul><li>Az olvasó biztonsági szerepkörhöz tartozó jogosultságok.<li>Az Advanced Threat Protection szolgáltatás (kártevők & vírus védelmet, rosszindulatú URL-cím config, URL nyomkövetésének, stb.) az összes beállításait is konfigurálhatja. |

## <a name="details-about-the-global-administrator-role"></a>A globális rendszergazdai szerepkörrel részleteit
A globális rendszergazda az összes felügyeleti funkcióhoz hozzáférése van. Alapértelmezés szerint a személy, aki az Azure-előfizetésre regisztrál a címtár globális rendszergazdai szerepkörrel rendelkeznek. Csak a globális rendszergazdák egyéb rendszergazdai szerepköröket rendelhet.

### <a name="to-add-a-colleague-as-a-global-administrator"></a>A globális rendszergazdai munkatárs hozzáadása

1. Jelentkezzen be a [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) egy olyan fiókkal, amely a bérlő címtár globális rendszergazdája.

   ![Az azure AD felügyeleti központ megnyitása](./media/active-directory-assign-admin-roles/active-directory-admin-center.png)

2. Válassza ki **felhasználók és csoportok &gt; minden felhasználó**

3. Keresse meg azt a felhasználót kijelöl egy globális rendszergazda, és nyissa meg az adott felhasználó a panelt.

4. A felhasználó panelen válassza ki a **Directory szerepkör**.
 
5. A könyvtár szerepkör panelen válassza ki a **globális rendszergazda** szerepkör, és mentse.

## <a name="assign-or-remove-administrator-roles"></a>Rendelje hozzá, vagy távolítsa el a rendszergazdai szerepkörökről
Rendszergazdai szerepkörök hozzárendelése az Azure Active Directoryban a felhasználó további tudnivalókért lásd: [felhasználó hozzárendelése az Azure Active Directory rendszergazdai szerepkörök](active-directory-users-assign-role-azure-portal.md).

## <a name="deprecated-roles"></a>Elavult szerepkörök

A következő szerepkörök nem használható. Ezek már elavult, és az Azure AD a jövőben törlődni fog.

* Ad hoc licenc rendszergazda
* E-mailek ellenőrzése létrehozó felhasználó
* Csatlakozás bármilyen eszközről
* Eszköz-kezelők
* Eszközök felhasználói
* Munkahelyi csatlakozás bármilyen eszközről

## <a name="next-steps"></a>Következő lépések
* Az Azure-előfizetések rendszergazdáinak módosításáról további információ: [Azure-rendszergazdai szerepkörök felvétele vagy módosítása](../billing-add-change-azure-subscription-administrator.md)
* Az erőforrások hozzáférésének Microsoft Azure-ban történő kezeléséről további információért lásd: [Az erőforrások hozzáférésének megismerése az Azure-ban](active-directory-understanding-resource-access.md)
* Hogyan Azure Active Directory vonatkozik-e az Azure-előfizetéshez további információkért lásd: [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directoryval](active-directory-how-subscriptions-associated-directory.md)
* [Felhasználók kezelése](active-directory-create-users.md)
* [Jelszavak kezelése](active-directory-manage-passwords.md)
* [Csoportok kezelése](active-directory-manage-groups.md)
