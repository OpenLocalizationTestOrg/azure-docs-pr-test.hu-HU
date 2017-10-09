---
title: "Az Azure Active Directoryban a vállalati Állapothordozás beállítások hibaelhárítása |} Microsoft Docs"
description: "Itt válaszok toosome kérdéseket a rendszergazdák beállításait, valamint az alkalmazás adatszinkronizálás rendelkezhet."
services: active-directory
keywords: "Vállalati állapot barangolási beállításokat, a windows-felhő, gyakran ismételt kérdések a vállalati állapothordozás"
documentationcenter: 
author: tanning
manager: swadhwa
editor: 
ms.assetid: f45d0515-99f7-42ad-94d8-307bc0d07be5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 4636d016983426e30d696459f54167b8ad2babcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#<a name="troubleshooting-enterprise-state-roaming-settings-in-azure-active-directory"></a>A vállalati Állapothordozás hibaelhárítási beállítások, az Azure Active Directoryban

Ez a témakör bemutatja, hogy miként tootroubleshoot és diagnosztizálhatja a vállalati Állapothordozás problémáit, és ismert problémák listáját tartalmazza.

## <a name="preliminary-steps-for-troubleshooting"></a>Első lépések a hibaelhárításhoz 
Hibaelhárítás megkezdése előtt győződjön meg arról, hogy hello felhasználókkal és eszközökkel rendelkeznek megfelelően konfigurálva, és hogy hello követelményeinek a vállalati Állapothordozás hello eszköz- és hello teljesíti. 

1. Windows 10-es hello legújabb frissítéseit, és egy minimális 1511-es verziója (operációsrendszer-verzióval 10586 vagy újabb) hello eszköz telepítve van. 
2. hello eszköz csatlakoztatva, vagy tartományhoz és az Azure ad-vel regisztrált az Azure AD.
3. Hello Azure Active Directory portálon **a vállalati Állapothordozás** engedélyezve van a hello könyvtárat **konfigurálása** > **eszközök**  >  **Felhasználók előfordulhat, hogy a beállítások és a vállalati alkalmazások adatainak szinkronizálása**. Minden felhasználó van kiválasztva, vagy hello felhasználó szinkronizálásra keresztül hello kiválasztott beállítás engedélyezve van, és hello biztonsági csoportban található.
4. hello felhasználó rendelkezik-e az Azure Active Directory Premium előfizetéssel toothem rendelve.  
5. hello eszköz újraindítása után, és hello felhasználó van bejelentkezve a vállalati Állapothordozás engedélyezése után.

## <a name="information-tooinclude-when-you-need-help"></a>Ha segítségre van szüksége információk tooinclude
Ha nem tudja megoldani a problémát az alábbi hello útmutatással, forduljon a a támogatási szakemberek számára. Amikor kapcsolatba lép a őket, ajánlott tooinclude hello a következő információkat:

- **Általános leírása hello hiba** – vannak-e hibaüzenetek hello felhasználó által látható? Ha nincs hibaüzenet, ismertetik hello első fellépése, részletes váratlan viselkedést. Mely szolgáltatások engedélyezve vannak a szinkronizálási szolgáltatás, és mi az toosync várt hello felhasználói? Több funkció nem szinkronizálás folyamatban vagy elkülönített informatikai tooone?
- **Az érintett felhasználók** – szinkronizálás működő vagy sikertelen egy felhasználó vagy több felhasználó van? Eszközök számáról játszó felhasználónként? Az összes nem szinkronizálása vagy némelyikük szinkronizálását, és néhány nem szinkronizálása?
- **Hello felhasználó adatai** – milyen identitás hello felhasználói toolog toohello eszköz használatával? Hogyan hello felhasználó bejelentkezik-toohello eszköz? Vannak toosync engedélyezett kijelölt biztonsági csoport egy részének? 
- **Hello eszközére vonatkozó információt** – az Azure AD csatlakoztatott eszköz, vagy a tartományhoz? Milyen build a hello eszközről van szó? Mik a legújabb frissítéseket hello?
- **Dátum és idő / időzóna típusú** – hello pontos dátum és idő látott hello hiba volt (beleértve a hello időzóna)?
- Például ez az információ segít nekünk minél gyorsabban tudja megoldani a problémát.

## <a name="troubleshooting-and-diagnosing-issues"></a>Hibaelhárítás és a problémák diagnosztizálása
Ez a szakasz javaslatokat nyújt hogyan tootroubleshoot és diagnosztizálhatja a problémákat kapcsolódó tooEnterprise Állapothordozás.

## <a name="verify-sync-and-hello-sync-your-settings-settings-page"></a>Ellenőrizze a szinkronizálási, és a beállítások lapon "Beállítások szinkronizálása" hello 

1. A Windows 10 rendszerű Számítógépeken tooa csatlakoztatás után tartomány a vállalati Állapothordozás, a munkahelyi fiókhoz bejelentkezési tooallow konfigurálva. Nyissa meg túl**beállítások** > **fiókok** > **a szinkronizálási beállítások** , és győződjön meg arról, hogy szinkronizálási és hello egyedi beállítása on, és, hogy hello felső részén hello beállítások azt jelzi, hogy a munkahelyi fiókjával szinkronizálását végzi. Győződjön meg arról is ugyanazt a fiókot használja a bejelentkezési fiókként hello **beállítások** > **fiókok** > **a információ**. 
2. Ellenőrizze, hogy szinkronizálás működik-e több számítógépen azáltal, hogy egyes módosítások gépen hello eredeti hello tálca toohello jobb oldali vagy felső oldalán üdvözlő képernyőt áthelyezésével. Tekintse meg a hello változás propagálása toohello második gép öt percen belül. 
 - Zárolásának és feloldásának üdvözlő képernyőt (Win + L) segítségével, a szinkronizálási események indítása.
 - Használjon hello ugyanazt a felhasználói fiókot a szinkronizálási toowork – mindkét számítógépeken, mivel a vállalati Állapothordozás kapcsolt toohello felhasználói fiókkal és hello számítógépfiókot.

**Lehetséges probléma**: hello-beállítások lap rendelkezik hello váltógombok szürkén jelenik meg, és nem egy fiókot, láthatja hello szöveg "egyes Windows-szolgáltatások csak érhető el, ha egy Microsoft-fiók vagy a munkahelyi fiók használata". Ez a probléma akkor merülhet fel, eszközöket, amelyek toobe be van állítva a tartományhoz, és tooAzure AD regisztrálva, de a hello eszköz hitelesítése nem sikerült tooAzure AD. Egy lehetséges oka, hogy hello eszköz szabályzatot kell alkalmazni, de ez az alkalmazás aszinkron módon történik, és néhány órával késhet. probléma hello lépéseit kövesse ellenőrizze tooverify hello eszköz regisztrációs állapot toocheck, ha ez helyzet hello.

### <a name="verify-hello-device-registration-status"></a>Hello eszköz regisztrációs állapotának ellenőrzése
A vállalati Állapothordozás hello eszköz toobe regisztrálva az Azure AD szükséges. Bár Állapothordozás hello az alábbi utasítások a következő nem meghatározott tooEnterprise segítségével, győződjön meg arról, hogy regisztrálva van-e a Windows 10-ügyfeleknek hello, majd erősítse meg az ujjlenyomatot, az Azure AD beállításai URL-CÍMÉT, NGC állapotát, és más információkat.

1.  Nyissa meg hello parancssor megvont jogosultságú. toodo ezt a Windows, nyissa meg a hello Futtatás indító (Win + K), és írja be a "cmd" tooopen.
2.  Ha meg nyitva a parancssor hello, írja be a "*dsregcmd.exe/status*".
3.  Várt kimenet hello **AzureAdJoined** mező értéknek kell lennie a "YES", **WamDefaultSet** mező értékét "YES" kell, és hello **WamDefaultGUID** mező értéknek kell lennie egy Hello végén a "(AzureAd)" GUID.

**Lehetséges probléma**: **WamDefaultSet** és **AzureAdJoined** egyaránt hello mező értéke "NO" lehet, hello eszköz tartományhoz és az Azure AD-ben regisztrált, és nem szinkronizálja a hello eszköz. Ez látható, ha hello eszköz toowait szükséges alkalmazott házirend toobe vagy hello hello eszköz tooAzure AD kapcsolódáskor sikertelen hitelesítés. hello felhasználó rendelkezhet toowait hello házirend toobe alkalmazott néhány órát. További hibaelhárítási lépéseket is tartalmazhat, automatikus regisztráció újrapróbálkozás a Feladatütemező aláíró, és vissza vagy indító hello feladat által. Néhány esetben fut "*dsregcmd.exe /leave*" egy rendszergazda jogú parancssori ablakban újraindul, majd próbálkozzon újra a regisztrációs segíthet a probléma megoldásához.


**Lehetséges probléma**: hello mezője **AzureAdSettingsUrl** üres, és hello eszköz nem létezik szinkronizálási. hello felhasználó előfordulhat, hogy rendelkezik utoljára bejelentkezett toohello eszközt a vállalati Állapothordozás engedélyezése az Azure Active hello előtt Directory portálon. Indítsa újra a hello eszközt, és hello felhasználói bejelentkezési adatokkal. Szükség esetén a hello portal, próbálja ki a rendelkező hello rendszergazda tiltsa le, majd engedélyezze újra a felhasználók is szinkronizálási beállítások és a vállalati alkalmazásadatok. Amennyiben újra engedélyezik, újraindítás hello eszközt, és hello felhasználói bejelentkezési adatokkal. Ha ez nem oldja meg a probléma hello **AzureAdSettingsUrl** rossz eszköz tanúsítványt hello esetben üres is lehet. Ebben az esetben fut "*dsregcmd.exe /leave*" egy rendszergazda jogú parancssori ablakban újraindul, majd próbálkozzon újra a regisztrációs segíthet a probléma megoldásához.

## <a name="enterprise-state-roaming-and-multi-factor-authentication"></a>A vállalati Állapothordozás és a többtényezős hitelesítés 
Bizonyos körülmények között a vállalati Állapothordozás is hibajelzés toosync adatok Azure multi-factor Authentication hitelesítés van beállítva. Ezek a problémák a további részletekért lásd: hello támogatási dokumentum [KB3193683](https://support.microsoft.com/kb/3193683). 

**Lehetséges probléma**: Ha az eszköz konfigurált toorequire multi-factor Authentication hello Azure Active Directory portálon, toosync beállítások meghiúsulhat tooa Windows 10-jelszó eszközt bejelentkezés során. Ez a multi-factor Authentication konfigurációjától típus tervezett tooprotect Azure rendszergazdai fiókkal. Rendszergazda felhasználók továbbra is lehet képes toosync tootheir Windows 10-es eszközökön a Microsoft Passport for Work PIN KÓDJÁNAK aláírása, vagy a multi-factor Authentication végrehajtása Office 365-höz hasonló más Azure-szolgáltatások elérése közben.

**Lehetséges probléma**: szinkronizálása sikertelen lehet, ha Üdvözöljük a rendszergazdákat hello Active Directory összevonási szolgáltatások multi-factor Authentication feltételes hozzáférési szabályzat konfigurálása és hello eszközön hello hozzáférési jogkivonat lejár. Győződjön meg arról, hogy jelentkezzen be, és kijelentkezés hello Microsoft Passport for Work PIN KÓDJÁNAK használatával, vagy végezze el a multi-factor Authentication Office 365-höz hasonló más Azure-szolgáltatások elérése közben.

###<a name="event-viewer"></a>Eseménynapló
Speciális hibaelhárítás, az Eseménynapló használt toofind bizonyos hibákat is lehet. Ezek hello az alábbi táblázat ismerteti. hello eseményeket az Eseménynapló található > alkalmazások és szolgáltatásnaplók > **Microsoft** > **Windows** > **SettingSync** és az identitás-hibák a szinkronizálási **Microsoft** > **Windows** > **az Azure AD**.


## <a name="known-issues"></a>Ismert problémák

### <a name="sync-does-not-work-on-devices-that-have-apps-side-loaded-using-mdm-software"></a>Szinkronizálás nem működik az eszközökre, amelyeken alkalmazások közvetlenül telepített-kezelési szoftver használatával

Operációs rendszert futtató eszközök hatással van a Windows 10 évforduló frissítés (verzió 1607) hello. Az Eseménynapló hello SettingSync-Azure naplók alatt hello Event ID 6013 80070259 hibával gyakran látható.

**Javasolt művelet**  
Ellenőrizze, hogy a Windows 10 hello v1607 ügyfél rendelkezik hello 2016 augusztusától 23 összegző frissítést ([KB3176934](https://support.microsoft.com/kb/3176934) OS Build 14393.82). 

---

### <a name="internet-explorer-favorites-do-not-sync"></a>Internet Explorer Kedvencek szinkronizálja a

Operációs rendszert futtató eszközök érinti hello Windows 10 November frissítés (1511-es verzió).

**Javasolt művelet**  
Ellenőrizze, hogy a Windows 10 hello v1511 ügyfél rendelkezik hello 2016. július összegző frissítést ([KB3172985](https://support.microsoft.com/kb/3172985) OS Build 10586.494).

---

### <a name="theme-is-not-syncing-as-well-as-data-protected-with-windows-information-protection"></a>Téma nem szinkronizálása, valamint az adatok védelme a Windows információvédelem 

a védett adatok tooprevent adatszivárgás [Windows információvédelem](https://technet.microsoft.com/itpro/windows/keep-secure/protect-enterprise-data-using-wip) nem szinkronizál a vállalati Állapothordozás keresztül az eszközök a Windows 10 évforduló frissítés hello.



**Javasolt művelet**  
nincs. A jövőbeli frissítésekre tooWindows megoldhatja a problémát.

---

### <a name="date-time-and-region-settings-do-not-sync-on-domain-joined-device"></a>Dátum, idő és terület beállítások szinkronizálja a tartományhoz csatlakoztatott eszközön 
  
Tartományhoz csatlakozó eszközök nem fog tapasztalni hello beállítása dátum, idő és a terület-szinkronizálás: automatikus idő. Automatikus idő előfordulhat, hogy felülbírálás más dátum, idő és terület beállítások hello és ezek a beállítások nem toosync okozhat. 

**Javasolt művelet**  
nincs. 

---

### <a name="uac-prompts-when-syncing-passwords"></a>A felhasználói fiókok felügyelete kéri, jelszavak szinkronizálásakor

Hello futtató érinti eszközökön Windows 10 November frissítés (1511-es verzió), amely vezeték nélküli hálózati adapter által konfigurált toosync jelszavak.

**Javasolt művelet**  
Ellenőrizze, hogy a Windows 10 hello v1511 ügyfél rendelkezik hello összegző frissítés ([KB3140743](https://support.microsoft.com/kb/3140743) OS Build 10586.494).

---

### <a name="sync-does-not-work-on-devices-that-use-smart-card-for-login"></a>Szinkronizálás nem működik az intelligens kártyás bejelentkezés használó eszközök
Az intelligens kártya vagy virtuális intelligens kártya használatával tooyour Windows-eszköz toosign kísérli meg, ha a szinkronizálási beállítások működése leáll.     

**Javasolt művelet**  
nincs. A jövőbeli frissítésekre tooWindows megoldhatja a problémát.

---

### <a name="domain-joined-device-is-not-syncing-after-leaving-corporate-network"></a>Vállalati hálózat elhagyása után nem szinkronizál-tartományhoz csatlakoztatott eszközön     
Tartományhoz csatlakozó eszközök AD szinkronizálás hibája tapasztalhat, ha hello eszköz telephelytől távoli huzamosabb ideig, és nem tudja végrehajtani a tartományi hitelesítéshez tooAzure regisztrálva.

**Javasolt művelet**  
Csatlakozás hello eszköz tooa vállalati hálózaton, hogy a szinkronizálási folytathatja.

---

 ### <a name="azure-ad-joined-device-is-not-syncing-and-hello-user-has-a-mixed-case-user-principal-name"></a>Az Azure AD csatlakoztatott eszköz nem szinkronizálja, és hello felhasználó rendelkezik-e egy vegyes eset egyszerű felhasználónév.
 Ha hello felhasználó rendelkezik-e egy nagybetű UPN (pl. a felhasználónév helyett felhasználónév) és a hello felhasználó az Azure AD csatlakoztatott eszközön, amely a Windows 10-Build 10586 too14393 frissítve van, a hello felhasználó-eszköz toosync sikertelenek lehetnek. 

**Javasolt művelet**  
hello felhasználói toounjoin kell, és a újracsatlakozhat hello eszköz toohello felhő. toodo hello helyi rendszergazda felhasználóként, a bejelentkezési és hello eszköz elhagyása túl címen**beállítások** > **rendszer** > **kapcsolatos** , és jelölje ki " Kezeléséhez, vagy válassza le a munkahelyi vagy iskolai". Karbantartása az alábbi hello fájlok, és az Azure AD Join hello eszközt újra **beállítások** > **rendszer** > **kapcsolatos** , majd válassza a "Kapcsolódás tooWork vagy az iskolai". Továbbra is toojoin hello eszköz tooAzure Active Directory és a teljes hello folyamata.

Hello tisztítás lépésben következő fájlok karbantartása hello:
- A Settings.dat`C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\Settings\`
- Minden hello fájlok hello mappában`C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\AC\TokenBroker\Account`

---

### <a name="event-id-6065-80070533-this-user-cant-sign-in-because-this-account-is-currently-disabled"></a>Eseményazonosító 6065:80070533 a felhasználók nem jelentkezhetnek be, mert ez a fiók jelenleg le van tiltva.  
Hello SettingSync/hibakeresési naplók az eseménynaplóban Ez a hiba látható, ha hello felhasználói hitelesítő adatok lejártak. Az emellett akkor fordulhat elő, amikor hello bérlői automatikusan nincs kiépítve AzureRMS. 

**Javasolt művelet**  
Hello első esetben hello felhasználói bejelentkezési toohello eszköz, valamint a hitelesítő adatok frissítése hello új hitelesítő adatokkal rendelkezik. toosolve hello AzureRMS problémát, folytassa a hello felsorolt [KB3193791](https://support.microsoft.com/kb/3193791). 

---

### <a name="event-id-1098-error-0xcaa5001c-token-broker-operation-failed"></a>Eseményazonosító 1098: Hiba: 0xCAA5001C Token broker művelet végrehajtása sikertelen volt  
Hello AAD/műveleti naplók az eseménynaplóban, ez a hiba lehetséges, hogy láthatók az esemény 1104: az AAD-felhő AP beépülő modul hívás Get tokent hibát adott vissza: 0xC000005F. Ez a probléma akkor fordul elő, ha nincs engedélyeket, vagy tulajdonjogot attribútumok hiányzik.    

**Javasolt művelet**  
Folytassa a hello felsorolt [KB3196528](https://support.microsoft.com/kb/3196528).  



## <a name="next-steps"></a>Következő lépések

- Használjon hello [User Voice fórum](https://feedback.azure.com/forums/169401-azure-active-directory/category/158658-enterprise-state-roaming) tooprovide visszajelzését és később javaslatok a hogyan tooimprove vállalati állapot központi.

- További információkért lásd: hello [vállalati Állapothordozás áttekintése](active-directory-windows-enterprise-state-roaming-overview.md). 

## <a name="related-topics"></a>Kapcsolódó témakörök
* [Vállalati állapotának központi áttekintése](active-directory-windows-enterprise-state-roaming-overview.md)
* [Az Azure Active Directoryban a vállalati állapothordozás engedélyezése](active-directory-windows-enterprise-state-roaming-enable.md)
* [Beállítások és adatok hordozása – gyakori kérdések](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Csoport házirend és a mobileszköz-kezelési beállítások beállítások szinkronizálási szolgáltatás](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 központi beállításainak ismertetése](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
