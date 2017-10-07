---
title: "aaaUse meglévő NPS kiszolgálók tooprovide Azure MFA képességei |} Microsoft Docs"
description: "hello Azure multi-factor Authentication a hálózati házirend-kiszolgáló bővítmény egy egyszerű megoldást tooadd felhőalapú kétlépéses vericiation képességek tooyour meglévő hitelesítési infrastruktúráját."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: e9fc495b06873d45f06233f1aa618db9d94f8bd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a>A meglévő hálózati házirend-kiszolgáló infrastruktúra integrálása az Azure multi-factor Authentication

az Azure MFA számára a hálózati házirend-kiszolgáló (NPS) bővítmény hello ad felhőalapú MFA képességek tooyour hitelesítési infrastruktúráját a meglévő kiszolgálók használata. Hálózati házirend-kiszolgáló bővítmény hello hozzáadhat a telefonhívás, szöveges üzenetet vagy telefonos alkalmazás ellenőrzési tooyour meglévő hitelesítési folyamat tooinstall, konfigurálásához és karbantartásához új kiszolgálók nélkül. 

A bővítmény hello Azure MFA kiszolgáló telepítése nélkül tooprotect VPN-kapcsolatok kívánó szervezetek számára készült. hello hálózati házirend-kiszolgáló bővítmény úgy működik, mint egy adaptert, RADIUS és a felhőalapú Azure multi-factor Authentication tooprovide között egy második hitelesítési tényezőt szinkronizált vagy összevont felhasználók hitelesítéséhez.

Hello hálózati házirend-kiszolgáló bővítmény használata az Azure MFA számára, a hello hitelesítési folyamat a következő összetevők hello tartalmazza: 

1. **NAS/VPN-kiszolgáló** kéréseket fogad a VPN-ügyfelek és távoli RADIUS-kérelmeket tooNPS kiszolgálók alakítja azokat. 
2. **Hálózati házirend-kiszolgáló** tooActive Directory tooperform hello elsődleges hitelesítéseként történő hello RADIUS-kérelmek és sikeres, akkor továbbítja a hello tooany telepített kiegészítő mezők csatlakozik.  
3. **Hálózati házirend-kiszolgáló bővítmény** elindítja a kérelem tooAzure MFA hello másodlagos hitelesítés. Hello bővítmény hello válasz fogadása után, és ha sikeres hello MFA-kérdést, azzal, hogy biztosít hello hálózati házirend-kiszolgáló biztonsági jogkivonatokat, amelyek közé tartozik egy MFA Azure STS által kiállított jogcímet befejezése hello hitelesítési kérelmet.  
4. **Az Azure MFA** kommunikál az Azure Active Directory tooretrieve hello felhasználó adatait, és egy ellenőrző konfigurált módszert toohello felhasználó hello másodlagos hitelesítést hajt végre.

hello a következő ábra szemlélteti a magas szintű hitelesítési kérelem folyamata: 

![Hitelesítési folyamatábrája](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Az üzembe helyezés megtervezése

hello hálózati házirend-kiszolgáló bővítmény redundancia, automatikusan kezeli, így nincs szükség speciális konfigurációra.

Tetszőleges számú Azure MFA-kompatibilis hálózati házirend-kiszolgáló kiszolgálók szükség szerint hozhat létre. Ha több kiszolgálót telepíti, azokat a különbség az ügyfél-tanúsítványt kell használnia. Kiszolgálónként egy tanúsítvány létrehozása azt jelenti, hogy minden tanúsítvány frissítheti egyenként, és nem kell foglalkoznia állásidő minden kiszolgálóra.

VPN-kiszolgálók útvonal hitelesítési kérelmek, ezért toobe hello új Azure MFA-kompatibilis hálózati házirend-kiszolgáló kiszolgálók tisztában kell.

## <a name="prerequisites"></a>Előfeltételek

hálózati házirend-kiszolgáló bővítmény hello célja, hogy a meglévő infrastruktúra toowork. Győződjön meg arról, hogy a következő előfeltételek előtt hello megkezdéséhez.

### <a name="licenses"></a>Licencek

az Azure MFA számára a hálózati házirend-kiszolgáló bővítmény hello a rendelkezésre álló toocustomers [Azure multi-factor Authentication licenceinek](multi-factor-authentication.md) (az Azure AD prémium, EMS vagy az MFA szolgáltatásra része).

### <a name="software"></a>Szoftver

Windows Server 2008 R2 SP1 vagy újabb.

### <a name="libraries"></a>Szalagtárak

Ezek a kódtárak hello kiterjesztésű automatikusan települnek.
-   [Visual C++ újraterjeszthető csomag a Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Active Directory modul Windows Powershellhez készült Azure 1.1.166.0 verziója](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

### <a name="azure-active-directory"></a>Azure Active Directory

A szinkronizált tooAzure Active Directory mindenki hello hálózati házirend-kiszolgáló kiterjesztéssel kell lennie az Azure AD Connect használatával, és a multi-factor Authentication regisztrálva kell lennie.

Hello bővítmény telepítésekor szüksége hello directory azonosító és a rendszergazdai hitelesítő adatokat az Azure AD-bérlő. A könyvtár-azonosító található a hello [Azure-portálon](https://portal.azure.com). Jelentkezzen be rendszergazdaként, válassza hello **Azure Active Directory** hello bal oldali ikonra, majd válassza ki **tulajdonságok**. Másolás hello GUID a hello **könyvtár-azonosítója** és mentse azt. A GUID hello bérlői azonosító használja, ha hello NPS-bővítményt telepíteni.

![A könyvtár-azonosító az Azure Active Directoryban található](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="prepare-your-environment"></a>A környezet előkészítése

Hello NPS-bővítményének telepítése előtt szeretné tooprepare, környezet toohandle hello hitelesítési forgalom.

### <a name="enable-hello-nps-role-on-a-domain-joined-server"></a>Tartományhoz csatlakozó kiszolgálón hello hálózati házirend-kiszolgáló szerepkör engedélyezése

hello hálózati házirend-kiszolgáló Active Directory tooAzure csatlakozik, és ezzel hitelesíti a hello többtényezős hitelesítési kérelmeket. Válasszon egy kiszolgálót ehhez a szerepkörhöz. Azt javasoljuk, hogy kiválasztása olyan kiszolgálóra, amely nem kezeli az egyéb szolgáltatások érkező kéréseket, mert a hálózati házirend-kiszolgáló bővítmény hello bármilyen kérelmeket, amelyek nem RADIUS által jelzett hibákat jelez.

1. A kiszolgálón nyissa meg a hello **hozzáadása szerepkörök és szolgáltatások varázsló** a Kiszolgálókezelő gyors üzembe helyezés menü hello.
2. Válasszon **szerepköralapú vagy szolgáltatásalapú telepítés** a telepítés típusát.
3. Jelölje be hello **hálózati házirend- és elérési szolgáltatások** kiszolgálói szerepkört. Egy ablak lehet felugró tooinform a szükséges szolgáltatások toorun ezt a szerepkört.
4. Folytatható hello varázsló segítségével, amíg nem hello visszaigazolási lapja. Válassza ki **telepítése**.

Most, hogy a hálózati házirend-kiszolgálójának kijelölt kiszolgáló, is konfigurálnia kell a kiszolgáló toohandle hello VPN-megoldás a bejövő RADIUS-kérelmeket.

### <a name="configure-your-vpn-solution-toocommunicate-with-hello-nps-server"></a>A VPN-megoldás toocommunicate hello hálózati házirend-kiszolgáló konfigurálása

Attól függően, hogy melyik VPN-megoldást használni, a RADIUS hitelesítési házirend eltérő lépéseket tooconfigure hello. A házirend toopoint tooyour RADIUS NPS-kiszolgáló konfigurálása.

### <a name="sync-domain-users-toohello-cloud"></a>Szinkronizálási tartományi felhasználók toohello felhő

Ez a lépés már lehet, hogy a bérlő befejeződött, de jó toodouble-ellenőrizze, hogy az Azure AD Connect szinkronizálása megtörtént-e az adatbázis nemrég.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) rendszergazdaként.
2. Válassza ki **az Azure Active Directory** > **az Azure AD Connect**
3. Győződjön meg arról, hogy a szinkronizálási állapota **engedélyezve** és az utolsó szinkronizálás nem kisebb, mint egy óra telt el.

Ha ki egy új ciklikus szinkronizálási tookick van szüksége, velünk hello utasításait [az Azure AD Connect szinkronizálása: a Feladatütemező](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).

### <a name="determine-which-authentication-methods-your-users-can-use"></a>A felhasználók hitelesítési módok meghatározása

Két tényező befolyásolja, hogy mely hitelesítési módszerek állnak rendelkezésre az NPS-bővítmény központi telepítése történik:

1. hello hello RADIUS-ügyfél között használt jelszó titkosítási algoritmus (VPN-profilok, Netscaler kiszolgáló, vagy egyéb) és hello NPS-kiszolgálókon.
   - **PAP** hello felhőben hello hitelesítési módszerek mindegyikéhez Azure MFA támogatja: telefonhívás, a egyirányú SMS-üzenet, a mobilalkalmazáson keresztüli értesítések és a mobilalkalmazás ellenőrzőkódjának.
   - **CHAPv2** és **EAP** támogatja a telefonhívás, és értesítést a mobilalkalmazásban.
2. hello bemeneti módszereket, amelyek ügyfélalkalmazás hello (VPN-profilok, Netscaler kiszolgáló, vagy más) képes kezelni. Például hello VPN-ügyfél rendelkezik néhány azt jelenti, hogy tooallow hello felhasználói tootype a szöveg- vagy mobilalkalmazás ellenőrzőkódja?

Hello NPS-bővítmény telepítésekor használni ezeket tényezők tooevaluate, mely módszerek állnak rendelkezésre a felhasználók számára. Ha a RADIUS-ügyfél a PAP FUNKCIÓT támogatja, de UX hello ügyfél nem rendelkezik a beviteli mezők az ellenőrző kódot, majd telefonhívás és a mobilalkalmazáson keresztüli értesítések hello két módon támogatott.

Is [tiltsa le a nem támogatott hitelesítési módszerek](multi-factor-authentication-whats-next.md#selectable-verification-methods) az Azure-ban.

### <a name="enable-users-for-mfa"></a>Lehetővé teszi a felhasználók a multi-factor Authentication

Hello teljes hálózati házirend-kiszolgáló bővítményt telepít, meg kell tooenable MFA hello felhasználóknak, hogy szeretné-e tooperform kétlépéses ellenőrzést. Több azonnal tootest hello bővítményt, ha Ön azt, a rendszer a multi-factor Authentication teljesen regisztrálva van legalább egy tesztet-fiók szükséges.

Ezen lépések tooget lépések egy olyan fiókot használja:
1. Jelentkezzen be a túl[https://aka.ms/mfasetup](https://aka.ms/mfasetup) teszt-fiókkal. 
2. Kövesse az ellenőrzési módjának hello kér tooset.
3. Vagy hozzon létre egy feltételes hozzáférési házirend vagy [hello felhasználói állapot módosítása](multi-factor-authentication-get-started-user-states.md) toorequire kétlépéses ellenőrzés hello teszt fiók. 

A felhasználók is kell toofollow ezeket a lépéseket tooenroll ahhoz, azok hello hálózati házirend-kiszolgáló kiterjesztésű hitelesítheti.

## <a name="install-hello-nps-extension"></a>Hello NPS-kiterjesztés telepítése

> [!IMPORTANT]
> Hello NPS-kiterjesztés telepítése hello VPN-csatlakozási pont eltérő kiszolgálóra.

### <a name="download-and-install-hello-nps-extension-for-azure-mfa"></a>Töltse le és telepítse a hálózati házirend-kiszolgáló bővítmény hello az Azure MFA számára

1.  [Töltse le a hálózati házirend-kiszolgáló bővítmény hello](https://aka.ms/npsmfa) a hello Microsoft Download Center.
2.  Hello bináris toohello hálózati házirend-kiszolgáló tooconfigure szeretne másolni.
3.  Futtatás *setup.exe* hello telepítési utasításait kövesse. Ha hibákba ütközik, ellenőrizze, hogy két könyvtárakat hello Előfeltételek szakasz a sikeresen telepített hello.

### <a name="run-hello-powershell-script"></a>Hello PowerShell parancsfájl futtatása

hello telepítő létrehoz egy PowerShell-parancsfájlt ezen a helyen: `C:\Program Files\Microsoft\AzureMfa\Config` (ahol a C:\ az a telepítési meghajtó). A PowerShell parancsfájl hello a következő műveleteket hajtja végre:

-   Hozzon létre egy önaláírt tanúsítványt.
-   Társítsa a nyilvános kulcs hello hello tanúsítvány toohello szolgáltatás egyszerű Azure ad-val.
-   Hello cert hello helyi számítógép tanúsítványtároló tárolja.
-   Adjon hozzáférést toohello tanúsítvány titkos kulcs tooNetwork felhasználó.
-   Indítsa újra a hálózati házirend-kiszolgáló hello.

Csak akkor, ha toouse saját (helyett hello önaláírt tanúsítványokat, amelyek hello PowerShell parancsfájlt hoz létre), a tanúsítványok telepítést hello PowerShell-parancsfájl toocomplete hello. Ha több kiszolgálón hello bővítményének telepítése minden egyes a saját tanúsítványban kell rendelkeznie.

1. A Windows PowerShell futtatása rendszergazdaként.
2. Módosítsa a könyvtárat.

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. PowerShell-parancsprogrammal hello hello telepítő által.

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. PowerShell kérni fogja a bérlő azonosítójával. Hello Directory azonosító GUID hello Azure-portálon hello Előfeltételek szakaszában fájlból másolt használja.
5. Jelentkezzen be rendszergazdaként AD tooAzure.
6. PowerShell a sikerről szóló üzenetet jeleníti meg, amikor befejeződött az hello parancsfájl.  

Ismételje meg ezeket a lépéseket minden további NPS-kiszolgálókon, amelyet az tooset terheléselosztás feliratkozott a.

>[!NOTE]
>Tanúsítványok a PowerShell-parancsfájl hello generálása helyett a saját tanúsítványok használatakor győződjön meg arról, hogy rendezve toohello hálózati házirend-kiszolgáló elnevezési konvenciót. hello tulajdonos nevének kell **CN =\<TenantID\>, OU = a Microsoft hálózati házirend-kiszolgáló bővítmény**. 

## <a name="configure-your-nps-extension"></a>A hálózati házirend-kiszolgáló-kiterjesztés konfigurálása

Ez a szakasz a kialakítási szempontokat és javaslatokat sikeres hálózati házirend-kiszolgáló bővítmény központi telepítéseket tartalmazza.

### <a name="configuration-limitations"></a>Konfiguráció korlátozásai

- hello Azure MFA kiterjesztése hálózati házirend-kiszolgáló nem tartalmaz eszközök toomigrate felhasználók és a multi-factor Authentication kiszolgáló toohello felhő beállításokat. Ezért javasoljuk, hogy az új központi telepítéséhez, nem pedig meglévő központi telepítési hello bővítményének használatával. Hello bővítmény egy meglévő telepítéshez használja, ha a felhasználók rendelkeznek-e tooperform igazolása felfelé újra toopopulate az MFA részletek hello felhőben.  
- hálózati házirend-kiszolgáló bővítmény hello használ hello hello az egyszerű felhasználónév a helyszíni Active directory tooidentify hello felhasználó az Azure MFA hello másodlagos Outlookhoz hello bővítmény végrehajtásához lehet konfigurált toouse egy másik azonosító például a másodlagos bejelentkezési azonosító vagy egyéni Active Directory a mező nem egyszerű felhasználónév. Lásd: [speciális konfigurációs beállítások a hálózati házirend-kiszolgáló bővítmény hello a multi-factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) további információt.
- Nem minden titkosítási protokollokkal támogatja az összes ellenőrzési módszert.
   - **PAP** támogatja a telefonhívás, a egyirányú SMS-üzenet, a mobilalkalmazáson keresztüli értesítések és a mobilalkalmazás ellenőrzőkódjának
   - **CHAPv2** és **EAP** telefonhívás és a mobilalkalmazáson keresztüli értesítések támogatása

### <a name="control-radius-clients-that-require-mfa"></a>Többtényezős Hitelesítést igénylő vezérlő RADIUS-ügyfelek

Egy RADIUS-ügyfél hello hálózati házirend-kiszolgáló bővítmény használatával engedélyezi az MFA Használatát, ha az ügyfél számára minden hitelesítési szükséges tooperform MFA. Ha tooenable MFA nem másokhoz azonban bizonyos RADIUS-ügyfelek, két hálózati házirend-kiszolgálók konfigurálása, és csak az egyik hello bővítmény telepítése. Konfigurálja a RADIUS-ügyfelek toorequire MFA toosend kérelmek toohello hálózati házirend-kiszolgáló hello bővítménnyel konfigurálva, és más RADIUS ügyfelek toohello hálózati házirend-kiszolgáló nincs konfigurálva hello kiterjesztésű kívánt.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>A felhasználók számára, amelyek nincsenek regisztrálva, a multi-factor Authentication előkészítése

Ha a multi-factor Authentication nem regisztrált felhasználók, azt is meghatározhatja, mi történik, ha mégis megpróbálják tooauthenticate. Hello beállításjegyzék-beállítást használja *REQUIRE_USER_MATCH* hello beállításjegyzékbeli elérési út *HKLM\Software\Microsoft\AzureMFA* toocontrol hello szolgáltatás viselkedését. Ezzel a beállítással rendelkezik egyetlen konfigurációs beállítás:

| Kulcs | Érték | Alapértelmezett |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | IGAZ/HAMIS | Nincs beállítva (egyenértékű tooTRUE) |

Ez a beállítás célját hello esetén toodetermine milyen toodo egy felhasználó nincs regisztrálva az MFA szolgáltatásra. Ha hello kulcs nem létezik, nincs beállítva, vagy be van állítva tooTRUE, hello felhasználó nincs regisztrálva, majd hello hivatkozásazonosító nem hello MFA-kérdést. Ha hello kulcs tooFALSE és hello felhasználó nincs regisztrálva, a hitelesítés folytatódik MFA végrehajtása nélkül.

Toocreate válassza ezt a kulcsot, és állítsa be úgy tooFALSE közben a felhasználó bevezetése, és nem lehet regisztrálni az Azure MFA még. Azonban mivel beállítási hello kulcs lehetővé teszi a felhasználóknak, az az MFA toosign be nem léptetett, el kell távolítania ezt a kulcsot, amelyet tooproduction előtt.

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="how-do-i-verify-that-hello-client-cert-is-installed-as-expected"></a>Hogyan ellenőrizhetem adott hello ügyféltanúsítványt a várt módon telepítve van?

Keresse meg a hello hello tanúsítványtároló, és ellenőrizze, hogy a titkos kulcs hello hello telepítő által létrehozott önaláírt tanúsítvány van jogosultságaitól toouser **hálózati szolgáltatás**. hello cert a tulajdonos neve van **CN \<tenantid\>, OU = a Microsoft hálózati házirend-kiszolgáló bővítmény**

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-toomy-tenant-in-azure-active-directory"></a>Hogyan ellenőrizhetem, hogy az ügyfél-tanúsítvány-e a kapcsolódó toomy bérlő az Azure Active Directoryban?

Nyissa meg a PowerShell-parancssort, és futtassa a következő parancsok hello:

```
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

Ezek a parancsok a hálózati házirend-kiszolgáló bővítmény hello példánya a PowerShell-munkamenetben a bérlő társítása minden hello tanúsítvány nyomtassa ki. Az ügyfél cert exportálásával hello titkos kulcs nélküli "Base-64 kódolású X.509(.cer)" fájlként keresse meg a tanúsítványt, és összehasonlíthatja hello listáját a következőtől: PowerShell.

Érvényes-származó, érvényes-amíg időbélyegeket, amelyek emberek számára olvasható formátumban, lehet, nyilvánvaló misfits használt toofilter hello parancs függvény egynél több tanúsítvány.

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Miért van saját kérések hiba miatt sikertelenül működő ADAL token?

Lehet, hogy ez a hiba miatt tooone több oka. Tegye a következőket toohelp hibáinak elhárítása:

1. Indítsa újra a hálózati házirend-kiszolgáló.
2. Győződjön meg arról, hogy adott ügyféltanúsítványt várt módon van-e telepítve.
3. Győződjön meg arról, hogy hello tanúsítvány társítva a bérlője Azure ad-val.
4. Győződjön meg arról, hogy https://login.microsoftonline.com/ hello bővítményt futtató hello kiszolgáló elérhető-e.

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-hello-user-is-not-found"></a>Miért nem sikerül a hitelesítési hiba történt a HTTP-naplókban figyelmezteti a felhasználókat arra, hogy hello a felhasználó nem található?

Győződjön meg arról, hogy az AD Connect fut, és ez hello a felhasználó megtalálható-e a Windows Active Directory és az Azure Active Directory.

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Miért látom HTTP-hibák rögzít minden a hitelesítések sikertelenek lesznek csatlakozás?

Győződjön meg arról, hogy https://adnotifications.windowsazure.com futtató kiszolgálóról, hello hello hálózati házirend-kiszolgáló bővítmény érhető el.


## <a name="next-steps"></a>Következő lépések

- Bejelentkezés másik azonosítók konfigurálása, vagy állítsa be a kivételek listájához a kétlépéses ellenőrzést végző nem szabad IP-címekről [speciális konfigurációs beállítások a hálózati házirend-kiszolgáló bővítmény hello a többtényezős hitelesítés](nps-extension-advanced-configuration.md)

- [Hárítsa el a hálózati házirend-kiszolgáló bővítmény hello hibaüzeneteket az Azure multi-factor Authentication](multi-factor-authentication-nps-errors.md)
