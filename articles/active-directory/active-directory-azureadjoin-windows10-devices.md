---
title: "Windows 10-eszközök használatát a munkahelyi |} Microsoft Docs"
description: "Pillanatkép képességeket biztosít a felhasználók és informatikai, kontrasztos eszköz üzembe helyezve, és a Windows 10 vállalati használt különböző módszereit."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: 94ccc8fd-b17b-4fda-8d56-9d87aa37a9f9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 451842f764898af65dd7300e8b48442d256cea7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-windows-10-devices-in-your-workplace"></a>Windows 10-eszközök használatát a munkahelyi
A következőkre vonatkozik: Windows 10 rendszerű számítógépek

Windows 10 olyan szervezeteknek, amelyek lehetővé teszik a felhasználók férhetnek hozzá a munkahelyi erőforrásokhoz a biztonságos és kényelmes módot három modellt kínál.

* **Az Azure Active Directory csatlakozási** (az Azure AD Join), az alkalmazottak számára például az Office 365 elsősorban a felhőben lévő erőforrások eléréséhez. Az Azure AD Join élményt nyújt a Windows 10-es új létesítési önkiszolgáló munkahelyi.
* **Tartományhoz való csatlakozást**, a helyszíni alkalmazások és erőforrások rendelkező szervezeteknek. Tartományhoz való csatlakozást az Azure AD-csatlakozás a Windows 10 fejlett élményt nyújt.
* **Egy új egyszerűsített BYOD élmény**, a felhasználók számára szeretne adni a munkahelyi vagy iskolai Windows és egyszerűen hozzáférni az erőforrásokhoz a személyes eszközökön.

A következő táblázat a képességet biztosít a felhasználók és rendszergazdák, a különböző módokon eszköz üzembe helyezve, és a Windows 10 vállalati használt kontrasztos pillanatképe mutatja be:

|  | Csatlakozás tartományhoz | Az Azure AD Join | Személyes eszköz |
| --- | --- | --- | --- |
| A Windows eszköz jelentkezzen be munkahelyi vagy iskolai fiókok. |Igen |Igen |Nem |
| A felhasználó egyszeri bejelentkezéses (SSO) az Office 365 és az Azure AD-alkalmazásokat. Egyszeri bejelentkezés azt a képességet csak egyszer jelentkezzen be szervezeti erőforrások eléréséhez. |Igen |Igen |Igen |
| Kerberos vagy NTLM alkalmazások egyszeri Bejelentkezéses felhasználó. |Igen |Korlátozott |Igen, a VPN-en keresztül |
| Erős hitelesítési és kényelmes jelentkezzen be munkahelyi vagy iskolai fiókok és a Microsoft Passport Windows Hello. |Igen |Igen |Igen |
| A hozzáférést a vállalati Windows áruház egy munkahelyi vagy iskolai fiókkal (nem Microsoft-fiókkal). |Igen |Igen |Igen |
| Vállalati-kompatibilis központi felhasználói beállítások az eszközön a munkahelyi vagy iskolai fiókokkal. |Igen |Igen |Igen |
| Azon szervezeti alkalmazások, amelyek a szervezeti eszköz házirendeknek megfelelő eszközökre korlátozza a hozzáférést. |Igen |Igen |Igen |
| A felhasználó önkiszolgáló a kiépítés az eszközök munkára"bárhonnan." |Nem |Igen |Igen |
| Képes kezelni az eszközöket. |Igen, a csoportházirend/SCCM keresztül |Igen |Igen |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Használja a munkahelyi által birtokolt eszközök az Azure AD Join és a tartományhoz csatlakozott Windows 10
Windows 10 kétféleképpen munkahelyi által birtokolt eszközök férhessenek hozzá a munkahelyi erőforrásokhoz:

* Az Azure AD Join
* Csatlakozás tartományhoz
  
  Érvényes beállítások attól függően, hogy a szervezetek igényét is lehet. Bizonyos esetekben a szervezetek előnye származhat mindkét módszer központi telepítésének engedélyezése.

## <a name="when-to-use-azure-active-directory-join"></a>Mikor érdemes használni, az Azure Active Directory Join
Az Azure AD Join élmény a Windows 10-kiépítés új önkiszolgáló munkahelyi.  Munkahelyi erőforrásokhoz, például az Office 365 elsősorban a felhőben elérő munkavállalók irányul. Egyszerűsített úgy konfigurálhatja a számítógépek, táblagépeken és telefonokon a vállalati. Eszközök különböző Windows platformokon konzisztens vezérlők használatával kezelt mobileszköz-kezelés keresztül.

**Használja az Azure AD Join az alábbi okok miatt**:

* Az önkiszolgáló kiépítés engedélyezése az eszközök útközben dolgozó szeretné.
* Munkahelyi mobileszközeiket egyaránt például táblagépekről és mobiltelefonokról felhasználók részére.
* Felhasználók kezelése az Active Directoryban, ahelyett, hogy az Azure AD-ben például a határozza munkavállalók, alvállalkozói és a diákok szeretné.
* Szeretne távoli fiókirodák munkavállalók csatlakozó képességek biztosítása a korlátozott a helyszíni infrastruktúrával.
* A helyszíni Active Directory nem rendelkeznek.

Egyes szervezetek használja az Azure AD Join munkahelyi tulajdonban lévő eszközök, az elsődleges módot, különösen a legtöbb vagy összes erőforrásaik át a felhőbe. Az Active Directory és az Azure AD hibrid szervezetek is választhatja, hogy egy módszert, vagy egy másik telepítése attól függően, hogy a felhasználó vagy a részleg.

Iskolai kerületben is kaphatók és felsőoktatási, például előfordulhat, hogy kezelése az Active Directory munkatársak és a diákok Azure AD-ben. Egyes vállalatok távoli irodák vagy értékesítési szervezeti egységek kezelése az Azure AD-érdemes. Azure AD Join és a tartományi csatlakozási módszer használható egy hibrid szervezeten belül. Az Azure AD Join tartományhoz való csatlakozást eszközök munkahelyi környezetben üzembe helyezéséhez kiváló kiegészítéseként is lehet.

**Ha a vállalati erőforrások eléréséhez a általában a felhővel, ha a szervezet előfordulhat, hogy használata további előnyökkel is jár**:

* A helyszíni identitás-infrastruktúra függőségeket is eltávolíthat.
* Egyszerűbbé teheti az eszközök telepítési modell első másik lapra a lemezkép-készítési megoldások azzal, hogy az önkiszolgáló konfigurációja lehetővé.
* Mobileszköz-kezelés segítségével különböző platformon, az eszközök kezelése.

További információ az Azure AD Join: [Extending felhőalapú képességek a Windows 10-eszközökre az Azure Active Directory Join keresztül](active-directory-azureadjoin-overview.md).

## <a name="when-to-use-domain-join-or-keep-using-it"></a>Mikor érdemes használni a tartományhoz csatlakoztatás (vagy tartani az)
Az elmúlt 15 éves, a számos szervezet munkahelyi eszközök csatlakoztatásához használt tartományhoz való csatlakozást. Segítségével a felhasználók jelentkezzen be az eszközét a következővel a Active Directory munkahelyi vagy iskolai fiókok. Tartományhoz való csatlakozást is lehetővé teszi, hogy informatikai központilag és teljes mértékben kezelheti ezeket az eszközöket. A szervezetek általában támaszkodhat a lemezkép-készítési módszerek eszközök beállításához, és gyakran használnak a System Center Configuration Manager (SCCM) vagy a csoport házirend (GP) kezelhetők.

**A vállalat kell használnia tartományhoz csatlakoztatás (vagy tartani az) az alábbi okok miatt**:

* Win32 alkalmazásokat értünk ezekre az eszközökre, amelyek NTLM/Kerberos telepítve van.
* A csoportházirend vagy az SCCM/DCM Eszközkezelés van szüksége.
* Biztosan folytatja a lemezkép-készítési megoldások segítségével konfigurálhatja az eszközöket az alkalmazottak számára.

**Tartományhoz való csatlakozást, a Windows 10-es is biztosít az Azure AD-csatlakozás a következő előnyöket**:

* Eszköz adathoz kötött erős hitelesítés és kényelmes jelentkezzen be munkahelyi vagy iskolai fiókok és a Microsoft Passport Windows Hello.
* A vállalati Windows áruház elérését az eszközök munkahelyi vagy iskolai fiókok (nincs Microsoft-fiók szükséges).
* Vállalati-kompatibilis központi felhasználói beállítások munkahelyi vagy iskolai fiókját (nincs Microsoft-fiók szükséges) használó eszközön.
* Azon szervezeti alkalmazások, amelyek a szervezeti eszköz házirendeknek megfelelő eszközökre korlátozza a hozzáférést.

További információ az Azure AD Join: [Extending felhőalapú képességek a Windows 10-eszközökre az Azure Active Directory Join keresztül](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Csatlakozás munkahelyi vagy iskolai személyes tulajdonú eszközök engedélyezése
A vállalat byod-MODELLT támogató, Windows 10 lehetővé teszi a felhasználó munkahelyi vagy iskolai fiók hozzáadása a számítógép, táblagép vagy telefon. Miután a felhasználó hozzáadja a munkahelyi vagy iskolai fiókkal, az eszköz regisztrálva az Azure AD, és opcionálisan a a szervezet van-e beállítva mobileszköz-kezelési rendszerbe beléptetett. A könyvtár ezeknek az eszközöknek jelenik meg: "Regisztrált" vs. "Az azure AD csatlakoztatott". A rendszergazdák alkalmazhat különböző házirendeket ezen információk alapján a személyes tulajdonban lévő eszközökre, mint a munkahelyi eszközök világosabb touch biztosít, ha szükséges.

Felhasználók is vegye fel a munkahelyi vagy iskolai fiók a személyes eszközüket a nagyon kényelmesen. Ehhez először egy munkahelyi alkalmazás eléréséhez, vagy akkor is elvégezhető manuálisan a beállítások menü segítségével. Ennek a fióknak egyszeri Bejelentkezést biztosít a szervezeti erőforrásokhoz.

További információ az Azure AD Join: [tartományhoz csatlakozó eszközök csatlakoztatása az Azure AD, a Windows 10 észlel](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Csatlakozás tartományhoz vagy az Azure AD Join engedélyezése
A korábban ismertetett módszerek mindegyikéhez (tartományhoz való csatlakozást, az Azure AD Join és az Add munkahelyi vagy iskolai fiók) belépési pontok rendelkezik a Windows 10 felhasználói élményt nyújt. Azonban az összes infrastruktúra funkcióinak engedélyezéséhez ahhoz a felhasználói élmény fog működni a rendszergazda szükségesek.

## <a name="requirements-for-deploying-azure-ad-join"></a>Az Azure AD Join telepítésének követelményei
Az Azure AD Join telepítése a felhasználók meg a következőkre lesz szüksége:

* Az Azure AD-előfizetés.
* Az Azure AD Premium előfizetéssel, például a mobileszközök felügyeleti automatikus igénylés, ha további képességeket van szüksége.
* Mobileszköz-kezelés – például egy Microsoft Intune-előfizetést, az Office 365 mobileszköz-kezelés, vagy bármely, a partner mobileszköz-felügyeleti szállítók, amelyekbe beépül az Azure AD. (Lásd a [feltett](#frequently-asked-questions) további információt a jelen cikk vége).

Ha a létesítmények hibrid, erősen ajánlott telepíteni, az Azure AD Connect kibővítheti a helyszíni címtár az Azure AD.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Tartományhoz való csatlakozást az Azure AD használatának követelményei
Tartományhoz való csatlakozást továbbra is működik, mert mindig. Azonban az Azure AD előnyök szüksége lesz a következő:

* Az Azure AD-előfizetés.
* Kibővítheti a helyszíni címtár az Azure AD az Azure AD Connect telepítése.
* Ez a szabály lehetővé teszi, hogy a tartományhoz csatlakozó eszközök feltételes hozzáférést az Azure AD.
* Ez a szabály lehetővé teszi a "tartományhoz" eszközökhöz való hozzáférést, ha szeretné tudni korlátozza a hozzáférést az egyes eszközök esetében.
* A System Center Configuration Manager az előírásoknak megfelelő eszközök megkövetelő szabályok engedélyezéséhez a Technical Preview 1509-es verzió. (Lásd a TechNet-dokumentációja és a következő blogbejegyzésben található).

További információ a tartományhoz való csatlakozást, a Windows 10: [tartományhoz csatlakozó eszközök csatlakoztatása az Azure AD, a Windows 10 során lép fel.](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>BYOD és "Munkahelyi vagy iskolai fiók hozzáadása" használatának követelményei
Ahhoz, hogy "megteremthetők a saját eszközök" (BYOD) a munkahelyi vagy iskolai fiókokkal, a következőkre lesz szüksége:

* Az Azure AD-előfizetés.
* Az Azure AD Premium előfizetéssel, például a mobileszközök felügyeleti automatikus igénylés, ha további képességeket van szüksége.

## <a name="requirements-for-using-microsoft-passport"></a>A Microsoft Passport használatának követelményei
A Microsoft Passport engedélyezéséhez a következőkre lesz szüksége:

* A nyilvános kulcsokra épülő infrastruktúra (PKI), amely használja a Microsoft Passport-alapú hitelesítés támogatásához.
* Tanúsítványalapú hitelesítés támogatása, amely a Microsoft Passport for Azure AD-csatlakozás és a munkahelyi vagy iskolai fiókját használja az Intune-előfizetést.
* A System Center Configuration Manager Technical Preview 1509-es verziója (lásd a TechNet-dokumentációja és a következő blogbejegyzésben található)-alapú hitelesítés támogatásához a Microsoft Passport által a tartományhoz való csatlakozást.
* Egy házirendet, a Microsoft Passport engedélyezése a szervezetében.

A nyilvános kulcsokra épülő infrastruktúra használata helyett az alábbi lépésekkel engedélyezheti a Microsoft Passport-alapú:

* Telepítse a Windows Server 2016 "Éles Preview 1" tartományvezérlők (nincs szükség tartomány vagy erdő működési szintjét; több tartományvezérlők a redundancia szolgál az Active Directory-helyekhez elegendő).
* A Microsoft Passport engedélyezése a szervezet házirendjének beállítása.

További információ a Microsoft Passport és a Windows Hello Windows 10: < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Gyakori kérdések
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Partner mobil eszköz felügyeleti termékek integrálása az Azure AD?
A következő szállító termékek egyesített beléptetéshez és a Windows 10-es feltételes hozzáférés az Azure AD integrálása:

* VMware által AirWatch
* Citrix Xenmobile
* Mobileszköz-kezelő Lightspeed
* SOTI a helyszíni mobileszköz-kezelés
* MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Mi a helyzet a munkahelyi csatlakoztatás, a Windows 10 rendszerben?
A munkahelyi csatlakoztatás Windows 8.1 használt BYOD engedélyezéséhez. Windows 10-ben BYOD engedélyezve van "Adja hozzá a munkahelyi vagy iskolai fiók" via a jelen dokumentum korábbi leírtak szerint. A szervezet számára, hogy a mobileszköz-kezelés nem integrálható az Azure AD felhasználók is regisztrálják az eszközüket a felügyelet kézzel **beállítások** > **fiókok**  >  **Munkahelyi hozzáférés**.

### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Felhasználók kapcsolódhatnak a Windows 10-es tartományi fiókot a Microsoft-fiókkal?
Nem a Windows 10. A Windows 8.1 a tartományhoz csatlakozó eszközök felhasználói számára sikertelen "Csatlakozás" saját Microsoft-fiókjával (például Hotmail, Live, az Outlook, Xbox, stb.) a tartományi fióknak bizonyos élő szolgáltatások, a Windows áruház és a felhasználó központi hasonló egyszeri Bejelentkezéses felhasználói élményt a beállítások az eszközön. A Windows 10 a Microsoft-fiók "Csatlakozás" funkció eltávolították. A felhasználó hozzáadhat egy vagy több Microsoft-fiókok színvonalú szolgáltatásokat, például a Windows áruház egyszeri bejelentkezés lehetővé teszi, hogy további fiókokat. Ezt **beállítások** > **fiókok** > **fiókja**.

Felhasználók, akik rendszerről a Windows 8.1-tartományhoz csatlakoztatott eszközökre, és ki volt a Microsoft-fiók csatlakoztatva, automatikusan lesz az összekapcsolt Microsoft-fiók felvette a további fiókokat használnak.

## <a name="additional-information"></a>További információ
* [Vállalati használatú Windows 10: Az eszközök munkahelyi célú használata](active-directory-azureadjoin-windows10-devices-overview.md)
* [A felhőalapú képességek kiterjesztése a Windows 10-eszközökre az Azure Active Directory Joinon keresztül](active-directory-azureadjoin-user-upgrade.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

