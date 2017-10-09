---
title: "aaaSettings és adatok hordozása – gyakori kérdések |} Microsoft Docs"
description: "Itt válaszok toosome kérdéseket a rendszergazdák beállításait, valamint az alkalmazás adatszinkronizálás rendelkezhet."
services: active-directory
keywords: "Vállalati állapot barangolási beállításokat, a windows-felhő, gyakran ismételt kérdések a vállalati állapothordozás"
documentationcenter: 
author: tanning
manager: swadhwa
editor: curtand
ms.assetid: c0824f5c-129b-4240-969f-921f6a64eae7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 4d6d6619b3a5fbd1d274603808d89b73ed942cd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="settings-and-data-roaming-faq"></a>Beállítások és adatroaming GYIK
Ez a témakör néhány rendszergazdák rendelkezhet beállításait, valamint az alkalmazás adatszinkronizálás kérdésekre ad választ.

## <a name="what-data-roams"></a>Milyen adatok barangol?
**Windows-beállítások**: hello hello Windows operációs rendszer beépített beállításai. Általában ezek a beállítások a számítógép testreszabása, és a következő kategóriába sorolhatók hello tartoznak:

* *Téma*, mely funkciók, például asztali téma és a tálca beállításait tartalmazza.
* *Az Internet Explorer beállításainak*, beleértve a legutóbb megnyitott lapokat és a Kedvencekhez.
* *A szegély böngészőbeállítások*, például a Kedvencek és olvasási listáját.
* *Jelszavak*, beleértve a Internet jelszavakat, a Wi-Fi profilok és mások számára.
* *Nyelvi beállítások*, amely tartalmazza a billentyűzetkiosztások, rendszer nyelve, dátum és idő, és további beállításait.
* *Könnyű hozzáférés funkcióját*, például Nagyító, kontrasztos téma vagy Narrátor.
* *Egyéb Windows-beállítások*, például a parancssor beállításai és alkalmazásainak listáját.

**Alkalmazásadatok**: univerzális Windows-alkalmazások beállításainak adatok tooa központi mappa lehet írni, és bármely toothis mappa írt adatok automatikusan lesznek szinkronizálva. Toohello az egyes alkalmazásokra fejlesztői toodesign ezt a képességet app tootake előnyös működik. További információt a hogyan toodevelop egy univerzális Windows-alkalmazást használó központi, lásd: hello [appdata tárolási API](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) és hello [Windows 8 appdata fejlesztői blogja központi](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Milyen fiókot szolgál szinkronizálási beállítások?
A Windows 8 és Windows 8.1 a szinkronizálási beállítások mindig használt végfelhasználói Microsoft-fiókok. Vállalati felhasználók kellett hello képességét tooconnect egy Microsoft-fiók tootheir Active Directory tartományi fiók toogain hozzáférés toosettings szinkronizálása. A Windows 10 Ez egy elsődleges és másodlagos fiók keretrendszer helyére, a funkciók Microsoft-fiók csatlakoztatva.

hello elsődleges fiók használatához hello fiók toosign tooWindows van definiálva. Ez lehet egy Microsoft-fiókkal, egy Azure Active Directory (Azure AD-) fiók, a helyszíni Active Directory-fiókkal vagy egy helyi fiókot. Windows 10-felhasználók hozzáadása toohello elsődleges fiók, adhat hozzá egy vagy több másodlagos felhő fiókok tootheir eszköz. Egy másodlagos számítógépfiókja általában egy Microsoft-fiókkal, az Azure AD-fiókot vagy néhány más fiók, például a Gmailes vagy a Facebook-on. Ezek a másodlagos fiókok hozzáférést biztosítanak tooadditional szolgáltatásokat, például egyszeri bejelentkezés és a Windows áruház hello, de azok nem képes a szinkronizálási beállítások működtetéséhez.

Windows 10-ben csak hello elsődleges fiók hello eszközhöz használható beállításokat szinkronizáláshoz (lásd: [hogyan frissítse a Microsoft fiók beállítások szinkronizálása a Windows 8 tooAzure AD szinkronizálása a Windows 10-es?](active-directory-windows-enterprise-state-roaming-faqs.md#how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10)).

Adatok soha nem vegyes hello különböző felhasználói fiókok hello eszköz között. Beállítások szinkronizálása két szabályok vonatkoznak:

* A Windows rendszer mindig hordozhatók hello elsődleges fiókkal.
* Az alkalmazásadatok hello használt fiók tooacquire hello alkalmazással címkével fog rendelkezni. Hello elsődleges fiókkal címkével csak alkalmazásokat szinkronizálja. Alkalmazás tulajdonjoga címkézés hello Windows áruház vagy a mobileszköz-kezelés (MDM) keresztül közvetlenül telepített alkalmazások esetén határozza meg.

Ha az alkalmazás tulajdonosa nem azonosítható, azt fogja barangolás hello elsődleges fiókkal. Ha egy eszközt a Windows 8 vagy Windows 8.1 tooWindows 10 frissítve van, minden hello alkalmazás címkével fog rendelkezni, hello Microsoft-fiók használatával. Ennek az oka, hogy a legtöbb felhasználó programon keresztül hello Windows Store apps, és nincs Windows Store támogatása az Azure AD-fiókok előzetes tooWindows 10 volt. Ha egy alkalmazás telepítve van egy kapcsolat nélküli licenc keresztül, hello app címkével fog rendelkezni hello eszközön hello elsődleges fiók használatával.

> [!NOTE]
> Windows 10-eszközökre, amelyek a vállalati által birtokolt és a csatlakoztatott tooAzure AD már nem kapcsolódhatnak a Microsoft-fiókok tooa tartományi fiókot. hello képességét tooconnect Microsoft fiók tooa tartományi fiókot, és rendelkezik az összes hello felhasználói adatok szinkronizálása toohello Microsoft-fiókkal (azaz hello Microsoft-fiók keresztül csatlakozó hello Microsoft-fiók és Active Directory működési központi) a rendszer eltávolítja a Windows 10-eszközökre, amelyek összeillesztett tooa csatlakoztatva az Active Directory vagy az Azure AD-környezetben.
>
>

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-tooazure-ad-settings-sync-in-windows-10"></a>Hogyan lehet frissíteni a Microsoft fiók beállítások szinkronizálása a Windows 8 tooAzure AD szinkronizálása a Windows 10-es?
Ha a Windows 8 vagy Windows 8.1 fut összekapcsolt Microsoft-fiókkal rendelkező illesztett toohello Active Directory-tartomány, a Microsoft-fiókján keresztül fogja szinkronizálni az beállítások. TooWindows 10 a frissítés után, továbbra is toosync felhasználói beállítások Microsoft-fiók segítségével, amíg a tartományhoz csatlakozó felhasználók és hello Active Directory-tartomány nem csatlakozik az Azure ad-val.

Ha hello a helyszíni Active Directory-tartomány és az Azure AD connect, az eszköz toosync a beállításokat a csatlakoztatott hello Azure AD-fiókot kísérli meg. Ha hello Azure AD-rendszergazda nem engedélyezi a vállalati Állapothordozás, a csatlakoztatott Azure AD-fiókot a beállítások szinkronizálása leáll. Ha a Windows 10-felhasználók és jelentkezik be egy Azure AD identity, hozzákezdhet szinkronizálása a windows-beállítások, amint a rendszergazda engedélyezi a beállítások szinkronizálása az Azure AD használatával.

Ha a vállalati eszköz személyes adatokat tárolja, vegye figyelembe, hogy a Windows operációs rendszer és az alkalmazásadatok megkezdődik a szinkronizálás tooAzure AD kell lennie. Annak a következő megvalósítását hello:

* A személyes Microsoft-fiók beállításainak eltolódás hello beállítások mellett a a munkahelyi vagy iskolai fiókok Azure AD lesz. Ennek az az oka hello Microsoft-fiókkal és az Azure AD-beállításokat a szinkronizálás most már külön fiókot használ.
* Személyes adatok, például Wi-Fi jelszavakat, a webes hitelesítő adatok és az Internet Explorer Kedvencek az előzőleg szinkronizált keresztül összekapcsolt Microsoft-fiókkal az Azure AD keresztül lesznek szinkronizálva.

## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Honnan, Microsoft-fiókkal és az Azure AD vállalati állapot központi együttműködés?
A 2015. November hello vagy későbbi kiadásai a Windows 10 a vállalati Állapothordozás csak az támogatott ugyanazt a fiókot egy időben. Ha bejelentkezik a tooWindows a munkahelyi vagy iskolai fiókkal az Azure AD használatával, minden adatot az Azure AD útján szinkronizálódnak. Ha személyes Microsoft-fiókkal bejelentkezés tooWindows, minden adatot hello Microsoft-fiók útján szinkronizálódnak. Univerzális appdata barangolás-e a fiókkal csak hello elsődleges bejelentkezés hello eszközön, és ez csak akkor, ha hello alkalmazás licencének hello elsődleges fiók tulajdonosa lesz barangolás. Univerzális appdata bármely másodlagos fiókokat tulajdonában hello alkalmazások nem lesznek szinkronizálva.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Azure AD-fiókok több bérlő beállítások is szinkronizálja?
Ha több Azure AD különböző fiókok Azure AD-bérlő szerepelnek hello ugyanarra az eszközre, frissítenie kell hello eszköz beállításjegyzék toocommunicate az Azure Rights Management (Azure RMS) minden Azure AD-bérlő számára.  

1. Minden Azure AD-bérlő hello GUID keresése. Nyissa meg a hello a klasszikus Azure portálon, és válassza ki az Azure AD-bérlő. hello GUID hello bérlő számára a böngésző címmezőjébe hello hello URL-CÍMRE van. Például:`https://manage.windowsazure.com/YourAccount.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/Tenant GUID/directoryQuickStart`
2. Miután hello GUID, szüksége lesz a tooadd hello beállításkulcs **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<bérlői azonosító GUID >**.
   A hello **bérlői azonosító GUID** kulcsot, hozzon létre egy új karakterláncsoros értéket (REG-MULTI-SZ) **AllowedRMSServerUrls**. Az adatait adja meg a terjesztési pont URL-címei hello eszközhozzáférések hello Azure bérlőktől licencelési hello.
3. Terjesztési pont URL-címek licencelési hello futtatásával hello található **Get-AadrmConfiguration** parancsmag. Ha hello értékei hello **LicensingIntranetDistributionPointUrl** és **LicensingExtranetDistributionPointUrl** eltérőek, mindkét értéket adni. Értékek: hello hello azonos, ha csak egyszer adja meg hello értéket.

## <a name="what-are-hello-roaming-settings-options-for-existing-windows-desktop-applications"></a>Mik azok a meglévő Windows asztali alkalmazások hello központi beállítások lehetőségei?
Csak az univerzális Windows-alkalmazások központi működik. Nincsenek elérhető egy meglévő Windows asztali alkalmazások a roaming engedélyezésének két lehetőség közül választhat:

* Hello [asztali híd](http://aka.ms/desktopbridge) segítséget nyújt a meglévő Windows asztali alkalmazások toohello univerzális Windows Platform kapcsolja. Itt módosításait minimális kód tootake előnyeit, az Azure AD alkalmazásadat-barangolás szükséges. hello asztali híd biztosít az alkalmazások app identitást, amely szükséges a tooenable alkalmazásadat-barangolás meglévő asztali alkalmazások esetén.
* [Felhasználói élmény Virtualization (UE-V)](https://technet.microsoft.com/library/dn458947.aspx) segítségével hozzon létre egy meglévő Windows asztali alkalmazások egyéni beállítások sablont, és engedélyezte a Win32-alkalmazások központi. Ezzel a beállítással kell hello fejlesztői toochange alkalmazáskódot hello alkalmazás. UE-V korlátozott tooon helyi Active Directory-felhasználók, akik vásárolt Microsoft Desktop Optimization Pack hello központi.

A rendszergazdák beállíthatják UE-V tooroam Windows asztali alkalmazások adatainak a Windows operációs rendszer beállításainak és univerzális alkalmazások adatainak keresztül központi módosításával [UE-V csoportházirendek](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2), többek között a következőket:

* A Windows beállításait csoportházirend barangolás
* Ne szinkronizáljon a Windows csoportházirend
* Az Internet Explorer hello alkalmazások szakaszban központi

Jövőbeli hello Microsoft vizsgálata módon toomake UE-V mélyen integrált a Windows rendszerbe, és UE-V tooroam beállítások keresztül hello Azure AD cloud kiterjesztése.

## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Tárolhatok szinkronizált beállításokat és adatokat a helyszínen?
A vállalati Állapothordozás összes szinkronizált adatot tárol hello Azure felhőben. UE-V biztosít egy helyszíni megoldás központi.

## <a name="who-owns-hello-data-thats-being-roamed"></a>Ki, hogy a rendszer éppen forrásul hello adatok tulajdonosa?
a vállalatok számára saját hello adatok forrásul keresztül a vállalati Állapothordozás hello. Egy Azure-adatközpontban adatokat tárolja. Összes felhasználói adat titkosítva van, mind az átvitel során, és az Azure RMS hello felhő aktívan. Ez az egy képest fokozása tooMicrosoft fiók-alapú beállítások szinkronizálás, amely csak bizonyos érzékeny adatok, például felhasználói hitelesítő adatok titkosítja, mielőtt hello eszköz kikerül.

A Microsoft ügyféladatok véglegesített toosafeguarding. Egy vállalati felhasználói beállítások automatikusan adattitkosítás az Azure RMS által még a Windows 10-eszközön, hogy más felhasználó nem tudja olvasni az adatokat. Ha a szervezete a szolgáltatás fizetős Azure RMS-hez, is használjon más Azure RMS-szolgáltatások, például nyomon követése és visszavonatni a dokumentumokat, automatikusan bizalmas információkat tartalmazó e-mailek védelmét, és a saját kulcsok kezelése (hello "állapotba hozása a saját kulcs" megoldás is néven BYOK). Ezek a szolgáltatások és az Azure RMS működésével kapcsolatos további információkért lásd: [Mi az az Azure Rights Management](https://technet.microsoft.com/jj585026.aspx).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Kezelheti egy adott alkalmazás vagy beállítás-szinkronizálás?
A Windows 10 nem az egyes alkalmazások a roaming MDM vagy csoportházirend beállítás toodisable van. A bérlői rendszergazdák letilthatják appdata szinkronizálási az összes olyan alkalmazáshoz egy felügyelt eszközön, de nincs egyeztetését vezérlő alkalmazásonkénti vagy alkalmazáson belül szinten.

## <a name="how-can-i-enable-or-disable-roaming"></a>Hogyan is engedélyezheti vagy letilthatja a központi?
A hello **beállítások** alkalmazást, és nyissa meg túl**fiókok** > **beállítások szinkronizálása**. Ezen a lapon láthatja, melyik fiók használható tooroam beállítások folyamatban van, és akkor engedélyezheti vagy letilthatja a beállítások toobe forrásul egyedi csoportját.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Mi az a Microsoft által ajánlott engedélyezése a Windows 10 központi?
A Microsoftnál érhető el, beleértve a központi felhasználói profilok, a UE-V és a vállalati állapot központi megoldások központi néhány beállítást.  Microsoft véglegesített toomaking a vállalati Állapothordozás későbbi Windows verziók befektetést. Ha a szervezet nem áll készen vagy kényelmes a mozgóátlag adatok toohello felhőalapú, majd ajánlott UE-V, az elsődleges központi technológia használatát. Ha a szervezet megköveteli a meglévő Windows asztali alkalmazások támogatása központi, de különösen toomove toohello felhő, vállalati állapot központi és UE-V használatát javasoljuk. Annak ellenére, hogy UE-V és a vállalati Állapothordozás nagyon hasonló technológiákat, azok nincsenek kölcsönösen kizárják egymást. Kiegészíti toohelp győződjön meg arról, hogy a szervezet által a szolgáltatásokat, amelyeket a felhasználók központi hello egymással.  

A vállalati Állapothordozás és UE-V használata esetén alkalmazza a következő szabályok hello:

* A vállalati Állapothordozás hello elsődleges központi ügynök hello eszközön. UE-V alatt használt toosupplement hello "Win32 térközét."
* UE-V központi Windows-beállítások és modern UWP-alkalmazások adatainak amikor hello UE-V csoport szabályzatok le kell tiltani. Ezekre már vonatkozik a vállalati Állapothordozás.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Hogyan a vállalati Állapothordozás támogatja a virtuális asztali infrastruktúra (VDI)?
A vállalati Állapothordozás termékváltozatok Windows 10-ügyfélen, de nem található a kiszolgálón termékváltozatok esetén támogatott. Ha egy ügyfél VM hipervizor gépen birtokolt, és távolról bejelentkezik toohello virtuális gépet, az adatok fog vándorol. Ha több felhasználó megosztva hello ugyanazt az operációs rendszer és a felhasználók távolról jelentkeznek be tooa kiszolgáló teljes asztali élmény, központi előfordulhat, hogy nem működik. hello utóbbi munkamenet-alapú forgatókönyv hivatalosan nem támogatott.

## <a name="what-happens-when-my-organization-purchases-azure-rms-after-using-roaming"></a>Mi történik, ha a szervezet Azure RMS vásárol barangolás felhasználását követően?
Ha a munkahelyén már működik a Windows 10 központi hello Azure RMS korlátozott használatú ingyenes előfizetésre, az Azure RMS-előfizetés megvásárlását nem semmilyen hatással lesz a hello központi funkció hello működését, és konfigurációs módosítások nélküli lesz az informatikai rendszergazda által kötelezően előírt.

## <a name="known-issues"></a>Ismert problémák
Lásd: hello hello dokumentáció [hibaelhárítási](active-directory-windows-enterprise-state-roaming-troubleshooting.md) fellépő ismert problémák listáját szakasz. 

## <a name="related-topics"></a>Kapcsolódó témakörök
* [Vállalati állapotának központi áttekintése](active-directory-windows-enterprise-state-roaming-overview.md)
* [Az Azure Active Directoryban a vállalati állapothordozás engedélyezése](active-directory-windows-enterprise-state-roaming-enable.md)
* [Csoport házirend és a mobileszköz-kezelési beállítások beállítások szinkronizálási szolgáltatás](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 központi beállításainak ismertetése](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [hibaelhárítással](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
