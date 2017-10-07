---
title: "Azure MFA kiszolgáló aaaUser portál |} Microsoft Docs"
description: "Ez a hello Azure többtényezős hitelesítés lap, amely leírja, hogyan tooget el az Azure MFA és hello felhasználói portálon."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 06b419fa-3507-4980-96a4-d2e3960e1772
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 0e36644c3d780249fb98d5da654e9d267c63561a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="user-portal-for-hello-azure-multi-factor-authentication-server"></a>Hello Azure multi-factor Authentication kiszolgáló felhasználói portál

hello felhasználói portál, amely lehetővé teszi, hogy a felhasználók tooenroll Azure multi-factor Authentication (MFA) és a fiókok karbantartásához IIS-webhelyet. Egy felhasználó lehet, hogy módosíthatják telefonszámukat, módosíthatják PIN-kódjukat, vagy válasszon toobypass kétlépéses ellenőrzés a következő bejelentkezés során.

Felhasználók toohello felhasználói portálra normál felhasználónevükkel és jelszó bejelentkezés, majd vagy a kétlépéses ellenőrzés telefonhívásának vagy biztonsági kérdéseket toocomplete válaszolja meg a hitelesítés. Milyen felhasználói regisztrációk engedélyezettek, ha felhasználók konfigurálása telefonszámukat és PIN-kód hello első bejelentkezése toohello felhasználói portálon.

Felhasználói portál rendszergazdái beállítása és kap engedélyt tooadd új felhasználók és a meglévő felhasználók frissítése történhet.

A használt környezettől függően célszerű lehet toodeploy hello felhasználói portált a hello Azure multi-factor Authentication kiszolgáló vagy egy másik kiszolgálón, az internet felé néző ugyanarra a kiszolgálóra.

![MFA felhasználói portál](./media/multi-factor-authentication-get-started-portal/portal.png)

> [!NOTE]
> hello felhasználói portálra csak érhető el a multi-factor Authentication kiszolgálóval. Ha használ multi-factor Authentication hello felhő, tekintse meg a felhasználók toohello [beállításról a fiók a kétlépéses ellenőrzéshez](./end-user/multi-factor-authentication-end-user-first-time.md) vagy [kezelheti a kétlépéses ellenőrzés beállításait](./end-user/multi-factor-authentication-end-user-manage-settings.md).

## <a name="install-hello-web-service-sdk"></a>Hello webszolgáltatási SDK telepítése

Mindkét esetben ha hello Azure multi-factor Authentication webszolgáltatási SDK, **nem** hello Azure multi-factor Authentication (MFA) kiszolgáló telepítve, teljes hello lépések olvashat.

1. Nyissa meg a hello multi-factor Authentication kiszolgáló konzol.
2. Nyissa meg toohello **webszolgáltatási SDK** válassza **webszolgáltatási SDK telepítése**.
3. Hello alapértelmezett beállításaival, kivéve, ha szüksége toochange teljes hello telepítés bármilyen okból őket.
4. Az IIS-ben az SSL-tanúsítvány toohello hely kötése.

Ha kérdései SSL-tanúsítvány konfigurálása az IIS-kiszolgálón, olvassa el hello [hogyan tooSet be SSL IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

Webszolgáltatási SDK hello az SSL-tanúsítványt kell biztosítani. Erre a célra megfelel egy önaláírt tanúsítvány. Importálja hello hello "Megbízható legfelső szintű hitelesítésszolgáltatók" tárolóban hello helyi számítógépfiók hello felhasználói portál webkiszolgálón úgy, hogy a tanúsítvány bízik hello SSL-kapcsolat kezdeményezésekor.

![MFA-kiszolgáló konfigurálása – Web Service SDK](./media/multi-factor-authentication-get-started-portal/sdk.png)

## <a name="deploy-hello-user-portal-on-hello-same-server-as-hello-azure-multi-factor-authentication-server"></a>Hello felhasználói portált a hello telepítheti ugyanarra a kiszolgálóra hello Azure multi-factor Authentication kiszolgáló

hello Előfeltételek szükséges tooinstall hello felhasználói portált a hello **ugyanarra a kiszolgálóra** , az Azure multi-factor Authentication kiszolgáló hello:

* IIS, beleértve az ASP.NET-et és az IIS 6 metabázisával való kompatibilitási funkciót (IIS 7 vagy újabb verzió esetén)
* Egy fiók rendszergazdai jogosultságokkal a hello számítógép és a tartományhoz, ha van ilyen. hello fiókot kell engedélyek toocreate Active Directory biztonsági csoportokat.
* Felhasználói portál biztonságos hello az SSL-tanúsítványt.
* Biztonságos hello Azure multi-factor Authentication webszolgáltatási SDK egy SSL-tanúsítvánnyal.

toodeploy hello felhasználói portál, kövesse ezeket a lépéseket:

1. Nyissa meg a hello Azure multi-factor Authentication kiszolgáló konzol, kattintson a hello **felhasználói portál** hello ikonra bal oldali menüben, majd kattintson a **felhasználói portál telepítése**.
2. Hello alapértelmezett beállításaival, kivéve, ha szüksége toochange teljes hello telepítés bármilyen okból őket.
3. Az IIS-ben az SSL-tanúsítvány toohello hely kötése

   > [!NOTE]
   > Ez az SSL-tanúsítvány általában egy nyilvánosan aláírt SSL-tanúsítvány.

4. Nyisson meg egy webböngészőt bármely olyan számítógépről, és keresse meg a toohello URL-címe, ahová a hello felhasználói portál telepítve van (például: https://mfa.contoso.com/MultiFactorAuth). Győződjön meg arról, hogy nem látható tanúsítvánnyal kapcsolatos figyelmeztetés vagy hiba.

![MFA-kiszolgáló felhasználói portáljának telepítése](./media/multi-factor-authentication-get-started-portal/install.png)

Ha kérdései SSL-tanúsítvány konfigurálása az IIS-kiszolgálón, olvassa el hello [hogyan tooSet be SSL IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

## <a name="deploy-hello-user-portal-on-a-separate-server"></a>Egy különálló kiszolgálón hello felhasználói portál telepítése

Ha hello Azure multi-factor Authentication kiszolgálót futtató kiszolgáló internetre irányuló nincs, telepítse hello felhasználói portál egy **külön, internetre irányuló kiszolgáló**.

Ha a szervezet által használt Microsoft Authenticator alkalmazás hello hello hitelesítési módszerek egyikét, és toodeploy hello felhasználói portált a saját kiszolgáló, végezze el a következő követelmények hello:

* V6.0 használata vagy a hello Azure multi-factor Authentication kiszolgáló újabb verzióját.
* Hello felhasználói portál telepítése az internetre irányuló webkiszolgálón fut a Microsoft internet Information Services (IIS) 6.x vagy újabb verzióját.
* Ha az IIS-sel 6.x, győződjön meg arról, az ASP.NET v2.0.50727 van regisztrált, és állítsa be a túl**engedélyezett**.
* Az IIS 7.x vagy újabb verzió használatakor IIS, beleértve az alapszintű hitelesítést, az ASP.NET-et és az IIS 6 metabázisával való kompatibilitási funkciót.
* Felhasználói portál biztonságos hello az SSL-tanúsítványt.
* Biztonságos hello Azure multi-factor Authentication webszolgáltatási SDK egy SSL-tanúsítvánnyal.
* Győződjön meg arról, hogy hello felhasználói portál SSL-en keresztül kapcsolódhatnak toohello Azure multi-factor Authentication webszolgáltatási SDK.
* Győződjön meg arról, hogy hello felhasználói portál hitelesítheti toohello Azure multi-factor Authentication webszolgáltatási SDK hello szolgáltatásfiók hitelesítő adatainak használatával hello "PhoneFactor Admins" biztonsági csoporthoz. A szolgáltatásfiók és csoportfelügyeletű léteznie kell az Active Directory hello Azure multi-factor Authentication kiszolgáló egy tartományhoz csatlakoztatott kiszolgálón fut. Ha. A szolgáltatásfiók és csoportfelügyeletű található helyileg a hello Azure multi-factor Authentication kiszolgáló Ha nincs csatlakoztatott tooa tartományhoz.

Azon hello Azure multi-factor Authentication kiszolgálót telepíteni hello felhasználói portál szükséges lépések hello:

1. **Az MFA kiszolgáló hello**, keresse meg a telepítési útvonalat toohello (Példa: C:\Program Files\Multi-Factor Authentication kiszolgáló), és hello fájl másolása **MultiFactorAuthenticationUserPortalSetup64** tooa helye elérhető toohello internetre kiszolgáló fogja telepíteni.
2. **Hello internetre webkiszolgálón**, hello MultiFactorAuthenticationUserPortalSetup64 telepítési fájl futtatása rendszergazdaként, hello hely módosítása, ha szükséges, és módosítsa a hello virtuális könyvtár tooa rövid nevét, ha azt szeretné.
3. Az IIS-ben az SSL-tanúsítvány toohello hely kötése.

   > [!NOTE]
   > Ez az SSL-tanúsítvány általában egy nyilvánosan aláírt SSL-tanúsítvány.

4. Keresse meg a túl**C:\inetpub\wwwroot\MultiFactorAuth**
5. Hello Web.Config fájlt a Jegyzettömb szerkesztése

    * Hello kulcs található **"USE_WEB_SERVICE_SDK"** , és módosítsa **érték = "false"** túl**érték = "true"**
    * Hello kulcs található **"WEB_SERVICE_SDK_AUTHENTICATION_USERNAME"** , és módosítsa **érték = ""** túl**érték = "Tartomány\felhasználónév"** tartomány\felhasználó esetén, amely része a szolgáltatásfiók "PhoneFactor Admins" csoporthoz.
    * Hello kulcs található **"Web_service_sdk_authentication_password értékét"** , és módosítsa **érték = ""** túl**érték = a "Password"** hello jelszó hello szolgáltatást a jelszó esetén Hello előző sorban megadott fiók.
    * Hello értékének **https://www.contoso.com/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx** , és módosítsa a helyőrző URL-cím toohello webszolgáltatási SDK URL-címe telepítettük a 2. lépésben.
    * Hello Web.Config fájlt mentse és zárja be a Jegyzettömböt.

6. Nyisson meg egy webböngészőt bármely olyan számítógépről, és keresse meg a toohello URL-címe, ahová a hello felhasználói portál telepítve van (például: https://mfa.contoso.com/MultiFactorAuth). Győződjön meg arról, hogy nem látható tanúsítvánnyal kapcsolatos figyelmeztetés vagy hiba.

Ha kérdései SSL-tanúsítvány konfigurálása az IIS-kiszolgálón, olvassa el hello [hogyan tooSet be SSL IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

## <a name="configure-user-portal-settings-in-hello-azure-multi-factor-authentication-server"></a>Hello Azure multi-factor Authentication kiszolgáló a felhasználói portál beállításainak konfigurálása

Most, hogy hello felhasználói portál telepítése tooconfigure hello Azure multi-factor Authentication kiszolgáló toowork hello portálon kell.

1. Hello Azure multi-factor Authentication kiszolgáló konzolon kattintson a hello **felhasználói portál** ikonra. Hello beállítások lapon adja meg hello URL-cím toohello felhasználói portál hello **felhasználói portál URL-CÍMÉT** szövegmező. Ha e-mailek funkció engedélyezve van, az URL-cím szerepel hello e-mailek toousers küldött hello Azure multi-factor Authentication kiszolgáló történő importálásakor.
2. Hello beállításokat, hogy szeretné-e a felhasználói portál hello toouse. Például, ha a felhasználók számára engedélyezett toochoose a hitelesítési módszereket, győződjön meg arról, hogy **engedélyezése a felhasználók tooselect metódus** be van jelölve, valamint hello módszerek közül választhatnak.
3. Adja meg, akik kell rendszergazdák hello **rendszergazdák** fülre. Hello checkboxes és a legördülő lista használatával hello hozzáadása/szerkesztése mezőkbe részletes felügyeleti engedélyeket is létrehozhat.

Választható konfiguráció:
- **Biztonsági kérdések** -megadása jóváhagyott biztonsági kérdéseket a környezet és hello nyelven jelennek meg.
- **Elvégzett munkamenetek** – Konfigurálhatja a felhasználói portál integrációját egy űrlapalapú webhellyel az MFA használatával.
- **Megbízható IP-címek** -felhasználók tooskip többtényezős hitelesítés engedélyezése, ha a listájából hitelesítése megbízható IP-címeket vagy címtartományokat.

![MFA-kiszolgáló felhasználói portáljának konfigurálása](./media/multi-factor-authentication-get-started-portal/config.png)

Az Azure multi-factor Authentication kiszolgáló felhasználói portál hello számos lehetőséget biztosít. hello következő táblázat felsorolja ezeket a beállításokat és a használatuk leírását.

| Felhasználói portál beállításai | Leírás |
|:--- |:--- |
| Felhasználói portál URL-címe | Adja meg a hello URL-cím, ahol hello portal tárolása. |
| Elsődleges hitelesítés | Adja meg a hitelesítési toouse hello típusú toohello portal bejelentkezéskor. Ez Windows-, Radius- vagy LDAP-hitelesítés lehet. |
| A felhasználók toolog engedélyezése | Lehetővé teszi felhasználók tooenter felhasználónév és jelszó hello hello felhasználói portál bejelentkezési oldalán. Ha ez a beállítás nincs bejelölve, a hello mezőkbe szürkén jelennek meg. |
| Felhasználó beléptetésének engedélyezése | Egy felhasználó tooenroll engedélyezése a multi-factor Authentication révén azokat tooa telepítő megjelenő megerősítést kér a további információkat, például telefonszáma. Rákérdezés a tartalék telefonszám lehetővé teszi, hogy felhasználók toospecify egy másodlagos telefonszámot. Rákérdezés a harmadik féltől származó OATH-tokennel lehetővé teszi a felhasználók toospecify egy harmadik féltől származó OATH jogkivonattal. |
| Felhasználók tooinitiate egyszeri megkerülés engedélyezése | Az egyszeri Mellőzés lehetővé teszi a felhasználók tooinitiate. Ha a felhasználó beállítja ezt a beállítást, eltarthat hatás hello legközelebb hello felhasználó jelentkezik be. Rákérdezés a Mellőzés másodpercben hello felhasználói dobozban biztosít hello alapértelmezett 300 másodperc módosíthassák. Ellenkező esetben az egyszeri Mellőzés hello tulajdonság csak jó 300 másodpercig. |
| Lehetővé teszi a felhasználók tooselect módszer | Lehetővé teszi a felhasználók toospecify az elsődleges kapcsolattartási módszerként. Ez lehet telefonhívás, szöveges üzenet, mobilalkalmazás vagy OATH token. |
| Lehetővé teszi a felhasználók tooselect nyelv | Engedélyezi a felhasználók toochange hello nyelvi hello telefonhívás, szöveges üzenet, mobilalkalmazás vagy OATH token használt. |
| Lehetővé teszi a felhasználók tooactivate mobilalkalmazás | Lehetővé teszi a felhasználók toogenerate egy aktiválási kódot toocomplete hello mobilalkalmazás aktiválási folyamat hello server használt.  Azt is beállíthatja aktiválásához eszközök hello száma, 1 és 10 közötti hello alkalmazást. |
| Biztonsági kérdések használata tartalék megoldásként | Engedélyezi a biztonsági kérdések használatát, ha a kétlépéses ellenőrzés sikertelennek bizonyul. Megadhatja a biztonsági kérdéseket kell sikeresen hello száma. |
| Lehetővé teszi a felhasználók tooassociate harmadik féltől származó OATH jogkivonat | Lehetővé teszi a felhasználók toospecify egy harmadik féltől származó OATH jogkivonattal. |
| OATH token használata tartalékként | Lehetővé teszik a OATH-tokenek hello használata, ha kétlépéses ellenőrzést nem sikeres. Hello munkamenet időkorlátja percben is megadható. |
| Naplózás engedélyezése | Hello felhasználói portál naplózásának engedélyezése. hello naplófájlok helye: C:\Program Files\Multi-Factor Authentication Server\Logs. |

Ezek a beállítások látható toohello felhasználói hello portálon vált, amennyiben engedélyezve vannak, és a felhasználói portál toohello bejelentkezés.

![Felhasználói portál beállításai](./media/multi-factor-authentication-get-started-portal/portalsettings.png)

### <a name="self-service-user-enrollment"></a>Önkiszolgáló felhasználói regisztráció

Ha szeretné, hogy a felhasználók toosign, és regisztrálta, ki kell választania hello **engedélyezése a felhasználók toolog** és **felhasználó regisztrálásának engedélyezése** beállítások hello beállítások lapon. Ne feledje, hogy a hello beállításoktól hello felhasználói bejelentkezési élmény hatással.

Például amikor egy felhasználó első alkalommal jelentkezik be toohello felhasználói portálon azon hello számára, azok majd készít toohello Azure multi-factor Authentication felhasználó beállítása lap. Attól függően, hogy az Azure multi-factor Authentication hogyan lettek konfigurálva, hello felhasználói is képes tooselect kell a hitelesítési módszert.

Ha azok hello hang hívás ellenőrzési módszer kiválasztása vagy előre beállított toouse Ez a módszer, hello lap megadását kéri az hello felhasználói tooenter az elsődleges telefonszám és a bővítmény, ha van ilyen. Az is lehetséges engedélyezett tooenter a tartalék telefonszám mellékét.

![Elsődleges és másodlagos telefonszámok regisztrálása](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Ha hello felhasználó PIN-kód szükséges toouse hitelesítéshez, a hello lap megerősítést kér toocreate PIN-kódot. A telefon száma és (ha van ilyen) PIN-kód bevitele, után hello kattintva hello **hitelesítő hívást kérek tooAuthenticate** gombra. Az Azure multi-factor Authentication hajt végre a telefonhívás ellenőrzési toohello felhasználó elsődleges telefonszámot. hello felhasználói hello telefonhívás és írja be a PIN-KÓDJUKBAN (ha van ilyen) és nyomja le a # toomove toohello hello önálló regisztrációs folyamat következő lépése.

Ha hello felhasználói hello szöveges üzenet ellenőrzési módszert választja, vagy ez a módszer már előre konfigurált toouse, hello oldal kéri hello a mobiltelefon számával. Ha hello felhasználó PIN-kód szükséges toouse hitelesítéshez, hello lap is megerősítést kér tooenter PIN-kódot.  Bevitele telefonszámukat és PIN-kódot (ha van ilyen), után hello kattintva hello **szöveges üzenetet kérek tooAuthenticate** gombra. Az Azure multi-factor Authentication egy SMS ellenőrzési toohello felhasználó mobiltelefon hajt végre. hello felhasználó megkapja hello szöveges üzenetet egy-alapú egyszeri-jelszót (OTP), majd a válaszok toohello üzenet, hogy egyszeri jelszavas hitelesítés és a PIN-KÓDJUKBAN (ha van ilyen).

![Felhasználói portál SMS](./media/multi-factor-authentication-get-started-portal/text.png)

Hello felhasználói hello mobilalkalmazás ellenőrzési módszert választja ki, ha hello lap kérni fogja a hello felhasználói tooinstall hello Microsoft Authenticator alkalmazást az eszközükön, és új aktiváló kód előállításához. Hello alkalmazás telepítése után a hello felhasználó hello aktiváló kód generálása gombra kattint.

> [!NOTE]
> toouse hello Microsoft Authenticator alkalmazást, hello felhasználói engedélyeznie kell a leküldéses értesítések az eszközön.

hello lap majd jeleníti meg az aktiváló kódot és URL-címet és egy vonalkódképet. Ha hello felhasználó PIN-kód szükséges toouse hitelesítéshez, hello lap továbbá megerősítést kér tooenter PIN-kódot. hello felhasználói hello aktiváló kódot és URL-cím köt hello Microsoft Authenticator alkalmazást, vagy hello vonalkód képolvasó tooscan hello vonalkódképet használ, és hello aktiválás gombra kattint.

Hello az aktiválás után hello kattintva hello **hitelesítés most** gombra. Az Azure multi-factor Authentication ellenőrző toohello felhasználó mobilalkalmazás hajt végre. hello felhasználói kell adja meg a PIN-KÓDJUKBAN (ha van ilyen), és kattintson az hello hitelesítés gombot az a mobilalkalmazás toomove toohello hello önálló regisztrációs folyamat következő lépése.

Ha hello rendszergazdák konfigurált hello Azure multi-factor Authentication kiszolgáló toocollect biztonsági kérdéseit és válaszait, hello felhasználói majd szükséges toohello biztonsági kérdések lap. hello felhasználói négy biztonsági kérdést válasszon, majd lerövidíti a kiválasztott tootheir kérdéseket.

![A felhasználói portál biztonsági kérdései](./media/multi-factor-authentication-get-started-portal/secq.png)

hello felhasználói önregisztráció befejeződött, és hello van bejelentkezett felhasználó toohello felhasználói portálon. Felhasználók bejelentkezhet vissza toohello felhasználói portál hello jövőbeli toochange bármikor a telefonszámokat, a PIN-kód, a hitelesítési módszerek és a biztonsági kérdések módosítása a módszerek a rendszergazdák által engedélyezett.

## <a name="next-steps"></a>Következő lépések

- [Hello Azure multi-factor Authentication kiszolgáló mobilalkalmazás webszolgáltatásának telepítése](multi-factor-authentication-get-started-server-webservice.md)
