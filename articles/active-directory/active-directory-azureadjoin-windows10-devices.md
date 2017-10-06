---
title: "munkahelyi aaaUsing Windows 10-eszközökhöz |} Microsoft Docs"
description: "Pillanatkép képességeket biztosít a felhasználók és informatikai, kontrasztos hello különböző módokon eszköz üzembe helyezve, és a Windows 10 vállalati használt."
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
ms.openlocfilehash: 8767e1649ced8737d20875f425c24198dcaa7f6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-windows-10-devices-in-your-workplace"></a>Windows 10-eszközök használatát a munkahelyi
A következőkre vonatkozik: Windows 10 rendszerű számítógépek

Windows 10 olyan szervezeteknek, amelyek lehetővé teszik a felhasználók tooaccess munkahelyi erőforrásokhoz a biztonságos és kényelmes módot három modellt kínál.

* **Az Azure Active Directory csatlakozási** (az Azure AD Join), az alkalmazottak számára például az Office 365 elsősorban hello felhőben lévő erőforrások eléréséhez. Az Azure AD Join élményt nyújt a Windows 10-es új létesítési önkiszolgáló munkahelyi.
* **Tartományhoz való csatlakozást**, a helyszíni alkalmazások és erőforrások rendelkező szervezeteknek. Tartományhoz való csatlakozást a Windows 10-es csatlakozáskor fejlett élményt nyújt tooAzure AD.
* **Egy új egyszerűsített BYOD élmény**, a felhasználók számára, akik tooadd munkahelyi fiókot, vagy iskolai tooWindows, és egyszerűen hozzáférni az erőforrásokhoz a személyes eszközökön.

hello alábbi táblázat mutatja be képességet biztosít a felhasználók és rendszergazdák, különböző módokon hello eszköz üzembe helyezve, és a Windows 10 vállalati használt kontrasztos pillanatképet:

|  | Csatlakozás tartományhoz | Az Azure AD Join | Személyes eszköz |
| --- | --- | --- | --- |
| A Windows eszköz jelentkezzen be munkahelyi vagy iskolai fiókok. |Igen |Igen |Nem |
| Felhasználó egyszeri bejelentkezéses (SSO) tooOffice 365 és Azure AD alkalmazásaiban. Egyszeri bejelentkezés hello képességét toosign csak egyszer tooaccess szervezeti erőforrások. |Igen |Igen |Igen |
| Felhasználói SSO tooKerberos/NTLM alkalmazásokat. |Igen |Korlátozott |Igen, a VPN-en keresztül |
| Erős hitelesítési és kényelmes jelentkezzen be munkahelyi vagy iskolai fiókok és a Microsoft Passport Windows Hello. |Igen |Igen |Igen |
| Munkahelyi vagy iskolai fiókkal (nem Microsoft-fiókkal) tooenterprise Windows áruház eléréséhez. |Igen |Igen |Igen |
| Vállalati-kompatibilis központi felhasználói beállítások az eszközön a munkahelyi vagy iskolai fiókokkal. |Igen |Igen |Igen |
| hello képességét toorestrict hozzáférés tooorganizational alkalmazások toodevices, amelyek megfelelnek a szervezeti eszközökre vonatkozó házirendeket. |Igen |Igen |Igen |
| A felhasználó önkiszolgáló a kiépítés az eszközök munkára"bárhonnan." |Nem |Igen |Igen |
| Képes toomanage eszközök. |Igen, a csoportházirend/SCCM keresztül |Igen |Igen |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Használja a munkahelyi által birtokolt eszközök az Azure AD Join és a tartományhoz csatlakozott Windows 10
Windows 10 kétféleképpen munkahelyi eszközök tooaccess munkahelyi erőforrásokhoz:

* Az Azure AD Join
* Csatlakozás tartományhoz
  
  Érvényes beállítások attól függően, hogy a szervezetek igényét is lehet. Bizonyos esetekben a szervezetek előnye származhat mindkét módszer központi telepítésének engedélyezése.

## <a name="when-toouse-azure-active-directory-join"></a>Ha toouse az Azure Active Directory csatlakozási
Az Azure AD Join élmény a Windows 10-kiépítés új önkiszolgáló munkahelyi.  Munkahelyi erőforrásokhoz, például az Office 365 elsősorban hello felhőben elérő munkavállalók irányul. Egy egyszerűsített módon tooconfigure számítógépeket, táblagépeket, és telefonok hello vállalati. Eszközök különböző Windows platformokon konzisztens vezérlők használatával kezelt mobileszköz-kezelés keresztül.

**Használja az Azure AD Join az alábbi okok miatt**:

* Azt szeretné, hogy tooenable hello önkiszolgáló kiépítés az eszközök a dolgozók a hello nyissa meg.
* Munkahelyi mobileszközeiket egyaránt például táblagépekről és mobiltelefonokról felhasználók részére.
* Felhasználók toomanage az Active Directoryban, ahelyett, hogy az Azure AD-ben, például határozza munkavállalók, alvállalkozói és a diákok szeretné.
* Azt szeretné, hogy tooprovide csatlakozó képességek tooworkers távoli fiókirodák korlátozott a helyszíni infrastruktúrával.
* A helyszíni Active Directory nem rendelkeznek.

Egyes szervezetek használja az Azure AD Join hello elsődleges módon toodeploy munkahelyi által birtokolt eszközök, különösen a legtöbb vagy összes az erőforrások toohello felhő áttelepítés után. Az Active Directory és az Azure AD hibrid szervezetek is beállíthatja toodeploy egy módszert, vagy egy másik hello felhasználó vagy a részleg függően.

Iskolai kerületben is kaphatók és felsőoktatási, például előfordulhat, hogy kezelése az Active Directory munkatársak és a diákok Azure AD-ben. Egyes vállalatok az Azure AD-érdemes toomanage távoli irodák vagy az értékesítési részleg. Azure AD Join és a tartományi csatlakozási módszer használható egy hibrid szervezeten belül. Az Azure AD Join lehet a nagy komplemens számnak toodomain csatlakozzon a munkahelyi környezetben eszközök telepítéséhez.

**Ha hello általában tooenterprise erőforrások eléréséhez hello felhő keresztül, ha a szervezet előfordulhat, hogy használata további előnyökkel is jár**:

* A helyszíni identitás-infrastruktúra függőségeket is eltávolíthat.
* Egyszerűbbé teheti az eszközök telepítési modell első másik lapra a lemezkép-készítési megoldások azzal, hogy az önkiszolgáló konfigurációja lehetővé.
* Használható mobileszköz-kezelési toomanage az eszközök különböző különböző platformokon.

További információ az Azure AD Join: [kiterjesztése a felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási](active-directory-azureadjoin-overview.md).

## <a name="when-toouse-domain-join-or-keep-using-it"></a>Ha a toouse tartományhoz való csatlakozás (vagy hogy)
Hello utolsó 15 éves, számos szervezet rendelkezik használni a tartományhoz csatlakoztatás tooconnect munkahelyi eszközöket. Ez lehetővé teszi, hogy a felhasználók toosign tootheir az Active Directory-eszközök a munkahelyi vagy iskolai fiókok. Tartományhoz való csatlakozást is lehetővé teszi, hogy informatikai toocentrally, és ezek az eszközök teljes körű felügyeletéhez. A szervezetek általában támaszkodhat imaging módszerek tooprovision eszközök, és gyakran használják a System Center Configuration Manager (SCCM) vagy a csoport házirend (GP) toomanage őket.

**A vállalat kell használnia tartományhoz csatlakoztatás (vagy tartani az) az alábbi okok miatt**:

* Win32 alkalmazásokat telepített toothese használó eszközök NTLM/Kerberos rendelkezik.
* A csoportházirend vagy SCCM/DCM toomanage eszközökre van szüksége.
* Az alkalmazottak toocontinue toouse lemezkép-készítési megoldások tooconfigure eszközök használni szeretne.

**Tartományhoz való csatlakozást, a Windows 10-es is lehetővé teszi az hello következő előnyökkel jár, amikor csatlakozik tooAzure AD**:

* Eszköz adathoz kötött erős hitelesítés és kényelmes jelentkezzen be munkahelyi vagy iskolai fiókok és a Microsoft Passport Windows Hello.
* Hozzáférési toohello vállalati Windows áruház olyan eszközök esetében, használja a munkahelyi vagy iskolai fiókok (nincs Microsoft-fiók szükséges).
* Vállalati-kompatibilis központi felhasználói beállítások munkahelyi vagy iskolai fiókját (nincs Microsoft-fiók szükséges) használó eszközön.
* hello képességét toorestrict hozzáférés tooorganizational alkalmazások toodevices, amelyek megfelelnek a szervezeti eszközökre vonatkozó házirendeket.

További információ az Azure AD Join: [kiterjesztése a felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Csatlakozás munkahelyi vagy iskolai személyes tulajdonú eszközök engedélyezése
BYOD toosupport hello vállalati, a Windows 10 hello felhasználói hello képességét tooadd biztosít, egy munkahelyi vagy iskolai fiók tootheir számítógép, táblagép vagy telefon. Hello után a felhasználó hozzáadja a munkahelyi vagy iskolai fiókját, hello eszköz regisztrálva az Azure AD és opcionálisan regisztrált hello mobileszköz felügyeleti rendszer hello szervezet van-e beállítva. hello directory ezeknek az eszközöknek jelenik meg: "Regisztrált" vs. "Az azure AD csatlakoztatott". A rendszergazdák alkalmazhat különböző házirendeket ezen információk alapján a személyes tulajdonban lévő eszközökre, mint a munkahelyi eszközök világosabb touch biztosít, ha szükséges.

Felhasználók hozzáadása a munkahelyi vagy iskolai nagyon kényelmesen fiók tootheir személyes eszköz. Ehhez a munkahelyi alkalmazás hello első alkalommal való hozzáféréskor, vagy akkor is elvégezhető manuálisan keresztül hello-beállítások menüjében. Ez a fiók SSO tooorganizational erőforrásokat ad meg.

További információ az Azure AD Join: [csatlakozni a tartományhoz csatlakozó eszközök tooAzure AD, a Windows 10 észlel](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Csatlakozás tartományhoz vagy az Azure AD Join engedélyezése
A korábban ismertetett módszerek mindegyikéhez (tartományhoz való csatlakozást, az Azure AD Join és az Add munkahelyi vagy iskolai fiók) rendelkezik a belépési pontok hello Windows 10 felhasználói élményt nyújt. Azonban összes funkciókra van szükség egy informatikai rendszergazda tooenable hello hello infrastruktúrában előtt hello élmény fog működni.

## <a name="requirements-for-deploying-azure-ad-join"></a>Az Azure AD Join telepítésének követelményei
a következő hello kell toodeploy Azure AD Joint a felhasználók meg:

* Az Azure AD-előfizetés.
* Az Azure AD Premium előfizetéssel, például a mobileszközök felügyeleti automatikus igénylés, ha további képességeket van szüksége.
* Mobileszköz-kezelés – például egy Microsoft Intune-előfizetést, az Office 365 mobileszköz-kezelés, vagy bármely hello partner mobil eszköz felügyeleti szállítók, amelyekbe beépül az Azure AD. (Lásd: hello [feltett](#frequently-asked-questions) további információt a jelen cikk hello végéhez).

Ha a létesítmények hibrid, erősen ajánlott telepíteni, az Azure AD Connect tooextend hello a helyszíni címtár tooAzure AD.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Tartományhoz való csatlakozást az Azure AD használatának követelményei
Tartományhoz való csatlakozást toowork mindaddig mindig rendelkezik. Azonban tooget hello Azure AD előnyei szüksége lesz a következő hello:

* Az Azure AD-előfizetés.
* Az Azure AD Connect tooextend hello központi telepítését a helyszíni Active directory tooAzure.
* Ez a szabály lehetővé teszi, hogy a tartományhoz csatlakozó eszközök toohave feltételes hozzáférés tooAzure AD.
* Ez a szabály lehetővé teszi a hozzáférést túl "tartományhoz" Ha azt szeretné, hogy toobe képes toorestrict hozzáférés egyes eszközök esetében.
* A System Center Configuration Manager Technical Preview, az előírásoknak megfelelő eszközök megkövetelő tooenable szabályok 1509-es verziója. (Lásd a TechNet-dokumentációja hello és blogbejegyzés).

További információ a tartományhoz való csatlakozást, a Windows 10: [csatlakozni a tartományhoz csatlakozó eszközök tooAzure AD, a Windows 10 észlel](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>BYOD és "Munkahelyi vagy iskolai fiók hozzáadása" használatának követelményei
tooenable "megteremthetők a saját eszközök" (BYOD) a munkahelyi vagy iskolai fiókokkal hello a következőkre lesz szüksége:

* Az Azure AD-előfizetés.
* Az Azure AD Premium előfizetéssel, például a mobileszközök felügyeleti automatikus igénylés, ha további képességeket van szüksége.

## <a name="requirements-for-using-microsoft-passport"></a>A Microsoft Passport használatának követelményei
a Microsoft Passport tooenable, szüksége lesz a következő hello:

* A nyilvános kulcsokra épülő infrastruktúra (PKI), amely használja a Microsoft Passport-alapú hitelesítés támogatásához.
* Tanúsítványalapú hitelesítés támogatása, amely a Microsoft Passport for Azure AD-csatlakozás és a munkahelyi vagy iskolai fiókját használja az Intune-előfizetést.
* A System Center Configuration Manager Technical Preview 1509-es verziója (lásd a TechNet-dokumentációja hello és blogbejegyzés)-alapú hitelesítés támogatásához a Microsoft Passport által a tartományhoz való csatlakozást.
* A Microsoft Passport engedélyezése hello szervezet házirendet.

Alternatív toousing egy nyilvános kulcsokra épülő infrastruktúra, mint a Microsoft Passport-alapú engedélyezheti hello következő tevékenységek végrehajtásával:

* Telepítse a Windows Server 2016 "Éles Preview 1" tartományvezérlők (nincs szükség tartomány vagy erdő működési szintjét; több tartományvezérlők a redundancia szolgál az Active Directory-helyekhez elegendő).
* Állítsa be a házirend tooenable Microsoft Passport hello szervezetében.

További információ a Microsoft Passport és a Windows Hello Windows 10: < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Gyakori kérdések
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Partner mobil eszköz felügyeleti termékek integrálása az Azure AD?
hello következő szállító termékek integrálása egyesített regisztráció és a feltételes hozzáférés a Windows 10-es Azure ad-val:

* VMware által AirWatch
* Citrix Xenmobile
* Mobileszköz-kezelő Lightspeed
* SOTI a helyszíni mobileszköz-kezelés
* MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Mi a helyzet a munkahelyi csatlakoztatás, a Windows 10 rendszerben?
A munkahelyi csatlakoztatás Windows 8.1 tooenable BYOD használt. Windows 10-ben BYOD engedélyezve van "Adja hozzá a munkahelyi vagy iskolai fiók" via a jelen dokumentum korábbi leírtak szerint. A szervezet számára, hogy a mobileszköz-kezelés nem integrálható az Azure AD felhasználók is hello eszközt felügyeletre való regisztrálására kézzel **beállítások** > **fiókok**  >  **Munkahelyi hozzáférés**.

### <a name="can-users-connect-their-microsoft-account-tootheir-domain-account-in-windows-10"></a>Kapcsolódhatnak a felhasználók a saját Microsoft-tootheir tartományi fiókot a Windows 10-es?
Nem a Windows 10. A Windows 8.1 a tartományhoz csatlakozó eszközök felhasználói számára sikertelen "Csatlakozás" a Microsoft fiókkal (például Hotmail, Live, az Outlook, Xbox, stb.) tootheir tartományi fiók tooenable bizonyos feladatokat, például egyszeri bejelentkezés tooLive szolgáltatások, hello Windows áruház használatát, és a központi felhasználói beállítások az eszközön. A Windows 10-es "Csatlakozás" funkció Microsoft-fiók hello eltávolították. hello felhasználói szerint további fiókokat tooenable SSO tooconsumer szolgáltatásokat, például a Windows Store hello is felvehet egy vagy több Microsoft-fiókot. Ezt **beállítások** > **fiókok** > **fiókja**.

Felhasználók, akik rendszerről a Windows 8.1-tartományhoz csatlakoztatott eszközökre, és ki volt a Microsoft-fiók csatlakoztatva, a rendszer automatikusan rendelkezik saját összekapcsolt Microsoft-fiókjával adja hozzá további fiókokat használnak toohello listája.

## <a name="additional-information"></a>További információ
* [Windows 10 Enterprise hello: módon toouse eszközök munkára](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése](active-directory-azureadjoin-user-upgrade.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

