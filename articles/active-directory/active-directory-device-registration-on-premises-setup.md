---
title: "a helyszíni feltételes hozzáférés használata az Azure Active Directory eszközregisztrációs aaaSetting |} Microsoft Docs"
description: "Egy részletes útmutató tooenabling feltételes hozzáférés tooon helyszíni alkalmazások a Windows Server 2012 R2 Active Directory összevonási szolgáltatások (AD FS) használatával."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6ae9df8b-31fe-4d72-9181-cf50cfebbf05
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 808e3b96873102aebae667153f588dcdb205e884
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-on-premises-conditional-access-by-using-azure-active-directory-device-registration"></a>A helyszíni feltételes hozzáférés beállítása az Azure Active Directory eszközregisztrációs használatával
Ha tooworkplace illesztési felhasználók a személyes eszközök toohello eszközregisztrációs szolgáltatás Azure Active Directory (Azure AD), az eszközeiket is ismert tooyour szervezet jelölésű. Az alábbiakban az részletesen ismerteti a feltételes hozzáférés tooon helyszíni alkalmazások engedélyezéséhez a Windows Server 2012 R2 Active Directory összevonási szolgáltatások (AD FS) használatával.

> [!NOTE]
> Az Office 365 licenc- vagy Azure AD Premium licenccel kell adni, ha az Azure Active Directory eszköz regisztrációs szolgáltatás feltételes hozzáférési házirendek a regisztrált eszközök használatával. Ezek közé tartozik a házirendekben, amelyek az AD FS-ben a helyszíni erőforrások érvényesíti.
> 
> A helyszíni erőforrások hello feltételes hozzáférési forgatókönyvek kapcsolatos további információkért lásd: [csatlakozás tooworkplace bármilyen eszközről egyszeri Bejelentkezéssel és zavartalan kéttényezős hitelesítése különböző vállalati alkalmazások](https://technet.microsoft.com/library/dn280945.aspx).

Ezen lehetőségek állnak rendelkezésre toocustomers, akik az Azure Active Directory Premium-licenc vásárlása.

## <a name="supported-devices"></a>Támogatott eszközök
* Windows 7-tartományhoz csatlakozó eszközök
* Windows 8.1 személyes és a tartományhoz csatlakozó eszközök
* iOS 6 és újabb verziók hello Safari böngésző
* Android 4.0-s vagy újabb verzió, Samsung GS3 vagy későbbi telefonok, Samsung Galaxy megjegyzés 2 vagy újabb rendszerű táblagépek

## <a name="scenario-prerequisites"></a>A forgatókönyv előfeltételei
* Előfizetés tooOffice 365 vagy Azure Active Directory Premium
* Az Azure Active Directory-bérlő
* Windows Server Active Directory (Windows Server 2008 vagy újabb)
* A Windows Server 2012 R2 frissített séma
* Az Azure Active Directory Premium licenc
* Windows Server 2012 R2 összevonási szolgáltatások, egyszeri Bejelentkezéses tooAzure AD beállítása
* Windows Server 2012 R2 webalkalmazás-Proxy 
* A Microsoft Azure Active Directory Connect (Azure AD Connect) [(Azure AD Connect letöltése)](http://www.microsoft.com/en-us/download/details.aspx?id=47594)
* Ellenőrzött tartomány

## <a name="known-issues-in-this-release"></a>Ebben a kiadásban ismert problémák
* Eszközalapú feltételes hozzáférési házirendek az eszköz objektum visszaírási tooActive könyvtárat az Azure Active Directory szükséges. Eszköz objektumok toobe visszaírását tooActive Directory toothree órát is eltarthat.
* iOS 7-es eszközöket a ügyféltanúsítvány-alapú hitelesítés során mindig megkérdezi a felhasználót hello felhasználói tooselect egy tanúsítványt.
* IOS 8, iOS 8.3 nem működnek, mielőtt az egyes verziói.

## <a name="scenario-assumptions"></a>A forgatókönyv Előfeltételek
Ez a forgatókönyv feltételezi, hogy rendelkezik-e az Azure AD-bérlő és a helyszíni Active Directory hibrid környezetben. Ezek a bérlők kell csatlakoztatni az Azure AD Connect egy ellenőrzött tartomány és az AD FS az egyszeri bejelentkezés. A következő ellenőrzőlista toohelp a környezet toohello követelmények szerint konfigurálja hello használata.

## <a name="checklist-prerequisites-for-conditional-access-scenario"></a>Ellenőrzőlista: A feltételes hozzáférési forgatókönyv előfeltételei
Az Azure AD-bérlő kapcsolódnak a helyszíni Active Directory-példányban.

## <a name="configure-azure-active-directory-device-registration-service"></a>Azure Active Directory eszközregisztrációs szolgáltatás konfigurálása
Ez az útmutató toodeploy használja, és konfigurálja a hello Azure Active Directory eszközregisztrációs szolgáltatás a szervezet számára.

Ez az útmutató feltételezi, hogy a Windows Server Active Directory konfigurálása és előfizetett tooMicrosoft Azure Active Directoryban. Tekintse meg a korábban ismertetett hello előfeltételek.

Bérlői toodeploy hello Azure Active Directory eszközregisztrációs szolgáltatás az Azure Active Directory, a következő ellenőrzőlista sorrendben hello teljes hello feladatokat. Ha egy hivatkozás viszi tooa elméleti témához, térjen vissza toothis ellenőrzőlista a későbbiekben úgy, hogy folytassa a hátralévő feladatok hello. Egyes feladatok közé tartozik a forgatókönyv ellenőrzési lépés, amelyek segítségével győződjön meg arról, hogy hello lépés sikeresen befejeződött.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>1. lépés: Engedélyezze Azure Active Directory eszközregisztráció
Hello ellenőrzőlista tooenable hello kövesse és hello Azure Active Directory eszközregisztrációs szolgáltatást konfigurálja.

| Tevékenység | Referencia | 
| --- | --- |
| Engedélyezze az Azure Active Directory bérlői tooallow eszközök toojoin hello munkahelyi eszközök regisztrációja. Alapértelmezés szerint a Azure multi-factor Authentication hello szolgáltatás nincs engedélyezve. Azt javasoljuk azonban, hogy többtényezős hitelesítés használata, amikor regisztrál egy eszközt. Ahhoz, hogy a többtényezős hitelesítés az Active Directory eszközregisztrációs szolgáltatás, ügyeljen arra, hogy az AD FS többtényezős hitelesítési szolgáltató. |[Azure Active Directory eszközregisztrációs engedélyezése](active-directory-device-registration-get-started.md)| 
|Eszközök az Azure Active Directory eszközregisztrációs szolgáltatás felderítéséhez a jól ismert DNS-rekordok megkeresésével. A vállalat DNS-ÉT, hogy az eszköz képes felderíteni az Azure Active Directory eszközregisztrációs szolgáltatás konfigurálása |[Azure Active Directory-eszköz regisztrációs felderítés konfigurálása](active-directory-device-registration-get-started.md)| 


## <a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>2. lépés: Telepítse és konfigurálja a Windows Server 2012 R2 Active Directory összevonási szolgáltatások, és állítsa be az Azure AD összevonási kapcsolat

| Tevékenység | Referencia |
| --- | --- |
| Active Directory tartományi szolgáltatások telepítése a Windows Server 2012 R2 sémakiterjesztések hello. Nem kell tooupgrade a tartomány tartományvezérlői tooWindows Server 2012 R2 bármelyikét. hello séma frissítése hello egyetlen követelménye. |[Az Active Directory tartományi szolgáltatások séma frissítése](#upgrade-your-active-directory-domain-services-schema) |
| Eszközök az Azure Active Directory eszközregisztrációs szolgáltatás felderítéséhez a jól ismert DNS-rekordok megkeresésével. A vállalat DNS-ÉT, hogy az eszköz képes felderíteni az Azure Active Directory eszközregisztrációs szolgáltatás konfigurálása |[Az Active Directory támogatási eszközök előkészítése](#prepare-your-active-directory-to-support-devices) |

## <a name="part-3-enable-device-writeback-in-azure-ad"></a>3. lépés: Engedélyezze az Azure AD eszközvisszaíró
| Tevékenység | Referencia |
| --- | --- |
| Fejezze be a második rész a "Eszközvisszaírás engedélyezése az Azure AD Connectben." Ha befejezte, akkor a visszatérési toothis útmutató. |[Eszközvisszaírás engedélyezése az Azure AD Connectben](#upgrade-your-active-directory-domain-services-schema) |

## <a name="optional-part-4-enable-multi-factor-authentication"></a>[Választható] 4. lépés: Az többtényezős hitelesítés engedélyezése
Erősen ajánlott konfigurált hello egyik multi-factor Authentication számos lehetőség közül választhat. Ha azt szeretné, hogy a multi-factor Authentication toorequire, [hello multi-factor Authentication biztonsági megoldás kiválasztása az Ön](../multi-factor-authentication/multi-factor-authentication-get-started.md). Az egyes megoldások leírását tartalmazza, és konfigurálja az Ön által választott hello megoldás toohelp hivatkozásokat tartalmaz.

## <a name="part-5-verification"></a>5. rész: ellenőrzése
hello telepítés befejeződött, és bizonyos esetekben tekinthetők meg. Hello következő hello szolgáltatásban tooexperiment csatolja, és ismerkedjen meg a szolgáltatások használatára.

| Tevékenység | Referencia |
| --- | --- |
| Csatlakoztassa az egyes eszközök tooyour munkahelyi Azure Active Directory eszközregisztrációs szolgáltatás használatával. IOS, Windows és Android-eszköz csatlakozhat. |[Azure Active Directory eszközregisztrációs szolgáltatással eszközök tooyour munkahelyi csatlakozás](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Megtekintheti és engedélyezése vagy letiltása a regisztrált eszközök hello rendszergazdai portál használatával. Ebben a feladatban az egyes regisztrált eszközök hello rendszergazdai portál használatával megtekintése. |[Az Azure Active Directory eszközregisztrációs szolgáltatás áttekintése](active-directory-device-registration-get-started.md) |
| Győződjön meg arról, hogy eszközobjektumot az Azure Active Directory tooWindows Server Active Directory biztonsági írt. |[Ellenőrizze, hogy a regisztrált eszközök vissza tooActive Directory írt](#verify-registered-devices-are-written-back-to-active-directory) |
| Most, hogy a felhasználók regisztrálhatják az eszközeiket, létrehozhat az alkalmazás hozzáférési szabályzatok az AD FS, amelyek csak regisztrált eszközök engedélyezésére. Ez a feladat létrehozza egy alkalmazás-hozzáférési szabályt és egy egyéni hozzáférés-megtagadási üzenetet. |[Alkalmazás-hozzáférési házirend és egyéni hozzáférés-megtagadási üzenet létrehozása](#create-an-application-access-policy-and-custom-access-denied-message) |

## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Az Azure Active Directory integrálása a helyszíni Active Directoryban
Ez a lépés segítséget nyújt az Azure AD-bérlő integrálása a helyszíni Active Directoryból az Azure AD Connect használatával. Annak ellenére, hogy hello lépéseket a klasszikus Azure portálon hello érhető el, jegyezze fel a jelen szakaszban felsorolt különleges utasítások.

1. Jelentkezzen be toohello klasszikus Azure portál egy olyan fiókkal, amely globális rendszergazda Azure AD-ben.
2. Hello bal oldali ablaktáblában jelölje ki **Active Directory**.
3. A hello **Directory** lapra, válassza ki azt a címtárat.
4. Jelölje be hello **címtár-integráció** fülre.
5. A hello **telepítéséhez és kezeléséhez** területén végezze el az 1 – 3 toointegrate Azure Active Directory a helyszíni címtárral.
   
   1. Tartományok hozzáadása.
   2. Telepítse és futtassa az Azure AD Connect hello található utasítások segítségével: [az Azure AD Connect egyéni telepítési](connect/active-directory-aadconnect-get-started-custom.md).
   3. Győződjön meg arról, és a címtár-Szinkronizáló kezelése. Egyszeri bejelentkezés utasításokat ebben a lépésben belül érhetők el.
   
   Összevonás emellett konfigurálása az AD FS a [az Azure AD Connect egyéni telepítési](connect/active-directory-aadconnect-get-started-custom.md).

## <a name="upgrade-your-active-directory-domain-services-schema"></a>Az Active Directory tartományi szolgáltatások séma frissítése
> [!NOTE]
> Miután frissítette az Active Directory-séma, hello folyamat nem állítható vissza korábbi állapotba. Azt javasoljuk, hogy először frissíti hello tesztkörnyezetben.
> 

1. Jelentkezzen be egy olyan fiókkal, amely a vállalati rendszergazdák és a séma-rendszergazdai jogosultságokkal rendelkezik tooyour tartományvezérlő.
2. Másolás hello **[adathordozó] \support\adprep** könyvtárban és annak alkönyvtáraiban levő tooone Active Directory tartományvezérlő (ahol **[adathordozó]** hello elérési toohello Windows Server 2012 R2 telepítési adathordozó ).
4. A parancssorból, lépjen a toohello **adprep** könyvtárhoz, és futtassa **adprep.exe/forestprep**. Hajtsa végre a hello képernyőn megjelenő utasításokat toocomplete hello séma frissítését.

## <a name="prepare-your-active-directory-toosupport-devices"></a>Az Active Directory toosupport eszközök előkészítése
> [!NOTE]
> Ez akkor kell futtatnia a tooprepare az Active Directory-erdő toosupport eszközök egyszeri műveletet. toocomplete ezzel az eljárással be kell jelentkeznie be vállalati rendszergazdai jogosultságokkal, és az Active Directory-erdőben kell hello Windows Server 2012 R2 séma.
> 


### <a name="prepare-your-active-directory-forest"></a>Az Active Directory-erdő előkészítése
1. Az összevonási kiszolgálón nyisson meg egy Windows PowerShell parancsablakot, és írja be **Initialize-ADDeviceRegistration**. 
2. Ha a rendszer kéri **ServiceAccountName**, adja meg az AD FS hello szolgáltatásfiókként kiválasztott hello szolgáltatásfiók hello nevét. Ha csoportosan felügyelt szolgáltatásfiókok, adjon meg hello fiókot hello **domain\accountname$** formátumban. Egy olyan tartományi fiók használata a hello formátum **domain\accountname**.

### <a name="enable-device-authentication-in-ad-fs"></a>Az AD FS eszközhitelesítés engedélyezése
1. Az összevonási kiszolgálón nyissa meg a hello AD FS felügyeleti konzolt, és folytassa túl**AD FS** > **hitelesítési házirendek**.
2. A hello **műveletek** ablaktáblán válassza előbb **általános elsődleges hitelesítés szerkesztése**.
3. Ellenőrizze **eszközhitelesítés engedélyezése**, majd válassza ki **OK**.
4. Alapértelmezés szerint az AD FS rendszeres időközönként eltávolítja nem használt eszközöket az Active Directoryból. Tiltsa le ezt a feladatot, amikor Azure Active Directory eszközregisztrációs szolgáltatás használja, hogy az eszközöket kezelheti az Azure-ban.

### <a name="disable-unused-device-cleanup"></a>Tiltsa le a nem használt eszköz karbantartása
Az összevonási kiszolgálón nyisson meg egy Windows PowerShell parancsablakot, és írja be **Set-AdfsDeviceRegistration - MaximumInactiveDays 0**.

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Az Azure AD Connect Felkészülés az eszközök visszaírásához.
Fejezze be az 1. lépés: az Azure AD Connect előkészítése.

## <a name="join-devices-tooyour-workplace-by-using-azure-active-directory-device-registration-service"></a>Eszközök tooyour munkahelyi csatlakozás Azure Active Directory eszközregisztrációs szolgáltatás segítségével

### <a name="join-an-ios-device-by-using-azure-active-directory-device-registration"></a>Csatlakozás az iOS-eszközök Azure Active Directory eszközregisztrációs segítségével
Azure Active Directory eszközregisztráció az iOS-eszközök hello sugárzási profil regisztrációs folyamatot használ. Ez a folyamat akkor kezdődik, amikor hello felhasználói Safari csatlakoznak az toohello profil regisztrációs URL-CÍMÉT. hello URL-cím formátuma a következő:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Ebben az esetben `yourdomainname` az Azure Active Directoryval konfigurált tartománynév hello. Például ha a tartománynév contoso.com, hello URL-cím a következőképpen történik:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Számos különböző módon toocommunicate az URL-cím tooyour felhasználók vannak. Például egy ajánlott módszer közzéteszi az URL-cím az AD FS egyéni hozzáférés-megtagadási üzenet található. Ez az információ hello következő szakasz tárgyalja [hozzon létre egy alkalmazás-hozzáférési házirend és egyéni hozzáférés-megtagadási üzenet](#create-an-application-access-policy-and-custom-access-denied-message).

### <a name="join-a-windows-81-device-by-using-azure-active-directory-device-registration"></a>Azure Active Directory eszközregisztrációs segítségével össze a Windows 8.1 eszköz
1. Válassza ki a Windows 8.1-eszközön **gépház** > **hálózati** > **munkahelyi**.
2. Adja meg a felhasználónevét UPN formátumú; például  **dan@contoso.com** .
3. Válassza ki **csatlakozás**.
4. Amikor a rendszer kéri, jelentkezzen be a fiókjával. hello eszköz már csatlakoztatva van.

### <a name="join-a-windows-7-device-by-using-azure-active-directory-device-registration"></a>Csatlakozás egy Windows 7 eszköz segítségével az Azure Active Directory eszközregisztrációs
tooregister Windows 7-tartományhoz csatlakozó eszközök toodeploy hello eszközregisztrációs szoftvercsomagot kell. hello szoftvercsomag neve munkahelyi csatlakoztatás Windows 7 rendszerhez készült, és az elérhető letölthető a következő hello [Microsoft Connect webhelyen](https://connect.microsoft.com/site1164). 

Leírja, hogyan toouse hello csomag találhatók [hogyan tooconfigure az automatikus regisztráció, a Windows-tartományhoz az Azure Active Directoryval eszközök](active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="verify-that-registered-devices-are-written-back-tooactive-directory"></a>Győződjön meg arról, hogy a regisztrált eszközök írt vissza tooActive könyvtár
Megtekintheti, és győződjön meg arról, hogy az eszköz objektumok készült vissza tooyour Active Directory által az LDP.exe eszközzel vagy az ADSI-szerkesztő. Mindkettő hello Active Directory rendszergazdai eszközök érhető el.

Alapértelmezés szerint az Azure Active Directory biztonsági írt eszközobjektumok kerülnek hello azonos tartományban találhatók, mint az AD FS-farmban.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Alkalmazás-hozzáférési házirend és egyéni hozzáférés-megtagadási üzenet létrehozása
Vegye figyelembe a következő forgatókönyv hello: Függőentitás-megbízhatóság az AD FS alkalmazás létrehozása és konfigurálása egy kiadási engedélyezési szabályt, amely lehetővé teszi, hogy csak a regisztrált készülékek. Most már csak a regisztrált eszközök tooaccess hello alkalmazás használata engedélyezett. 

toomake megkönnyítik a felhasználók toogain access toohello alkalmazást, konfigurálása egyéni hozzáférés-megtagadási üzenetet, amely útmutatást tartalmaz toojoin az eszközt. Mostantól a felhasználók rendelkeznek egy zavartalanul tooregister az eszközeiket, az alkalmazás eléréséhez.

hello következő lépések bemutatják, hogyan tooimplement ebben a forgatókönyvben.

> [!NOTE]
> Jelen szakaszban feltételezzük, hogy már konfigurálta a függő entitás megbízhatóságának az AD FS-ben az alkalmazáshoz.
> 

1. Nyissa meg az AD FS MMC eszköz hello, és válassza **AD FS** > **megbízhatósági kapcsolatok** > **függő entitás Megbízhatóságai**.
2. Keresse meg a hello alkalmazás toowhich az új hozzáférési szabály vonatkozik. Kattintson a jobb gombbal a hello alkalmazást, majd válassza ki **Jogcímszabályok szerkesztése**.
3. Jelölje be hello **Kiállításengedélyezési szabályok** lapra, majd válassza ki **szabály hozzáadása**.
4. A hello **jogcímszabály** sablon legördülő listában válassza **engedélyezése vagy tiltása felhasználók egy bejövő jogcím alapján**. Válassza ki **következő**.
5. A hello **Jogcímszabály nevének** mezőbe írja be **hozzáférés engedélyezése a regisztrált eszközökről**.
6. A hello **bejövő jogcím típusa** legördülő listában válassza **regisztrált felhasználó van**.
7. A hello **bejövő jogcím értéke** mezőbe írja be **igaz**.
8. Jelölje be hello **engedélyezési hozzáférési toousers ilyen bejövő jogcímmel rendelkező** választógombot.
9. Válassza ki **Befejezés**, majd válassza ki **alkalmaz**.
10. Távolítsa el az összes szabály, amely megengedőbb, mint a létrehozott hello szabály. Például a hello alapértelmezett szabály eltávolítása **felhasználók hozzáférésének engedélyezése tooall**.

Az alkalmazás már csak konfigurált tooallow érhető el, csak akkor, amikor hello felhasználó az, hogy azok regisztrálva, és toohello munkahelyhez csatlakoztatott eszközről érkezik. Speciális hozzáférési szabályzatok, lásd: [kockázatkezelés további többtényezős hitelesítéssel érzékeny alkalmazások](https://technet.microsoft.com/library/dn280949.aspx).

Ezután konfigurálja a egyéni hibaüzenet az alkalmazáshoz. hello hibaüzenet tájékoztatja a felhasználókat arról, hogy azok csatlakoztatnia kell az eszköz toohello munkahelyi hello alkalmazás eléréséhez. Egy egyéni alkalmazás-hozzáférés-megtagadási üzenetet is létrehozhat egyéni HTML- és PowerShell használatával.

Az összevonási kiszolgálón nyisson meg egy PowerShell-parancsablakot, és írja be a következő parancs hello. Cserélje le hello parancs részeit, amelyek adott tooyour rendszer elemek:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Ez az alkalmazás elérése előtt regisztrálnia kell az eszközt.

**Ha iOS-eszközt használ, jelölje be a hivatkozás toojoin az eszköz**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Az iOS eszköz tooyour munkahelyi csatlakoztatás.

Ha a Windows 8.1-eszközt használ, az eszköz csatlakozhat kiválasztásával **gépház**> **hálózati** > **munkahelyi**.

A parancsok, megelőző hello **függő fél megbízhatósági neve** hello neve, az alkalmazás megbízható függő entitás megbízhatóságának objektumot az AD FS-ben.
És **tartomány.com címről** hello tartománynév már konfigurálta az Azure Active Directoryban (például contoso.com).
Lehet, hogy bármely sortörések (ha van ilyen) a hello HTML tartalom át kell adni toohello tooremove **Set-AdfsRelyingPartyWebContent** parancsmag.

Most felhasználók általi elérésekor az alkalmazás egy eszközről, amely nincs regisztrálva az Azure Active Directory eszközregisztrációs szolgáltatás hello, azok lap jelenik meg, a következőhöz hasonló toohello alábbi képernyőfelvételen.

![Képernyőkép a hibaüzenet, amikor a felhasználók az eszköz még nem regisztrált az Azure ad szolgáltatással](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)


