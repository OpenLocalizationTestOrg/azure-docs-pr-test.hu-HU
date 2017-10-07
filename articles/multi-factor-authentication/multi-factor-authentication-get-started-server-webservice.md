---
title: "multi-factor Authentication kiszolgáló mobilalkalmazás webszolgáltatásának aaaAzure |} Microsoft Docs"
description: "hello Microsoft Authenticator alkalmazást egy sávon kívüli további hitelesítési lehetőséget kínál.  Lehetővé teszi a multi-factor Authentication kiszolgáló toouse leküldéses értesítések toousers hello."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 6c8d6fcc-70f4-4da4-9610-c76d66635b8b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 4175b68fcbf85ec3fd53d8edf4e07306c75a4c71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>A mobilalkalmazásos hitelesítés engedélyezése az Azure Multi-Factor Authentication-kiszolgálóval

hello Microsoft Authenticator alkalmazást egy sávon kívüli további ellenőrzési lehetőséget kínál. Helyett egy automatizált telefonhívást vagy SMS toohello felhasználói bejelentkezés során, Azure multi-factor Authentication egy értesítési toohello Microsoft Authenticator alkalmazást a hello felhasználó okostelefonján vagy táblagépén leküldéses értesítések. hello felhasználó egyszerűen koppintással **ellenőrizze** (vagy a PIN-kód kerül, és "Hitelesítés" gombra koppint) a hello app toocomplete a bejelentkezés.

Ha a telefonon a vétel nem megbízható, kétlépéses ellenőrzést biztosító mobilalkalmazás használata ajánlott. Ha az OATH token generátor hello alkalmazást használja, a hálózat és internet kapcsolat nem igényel.

A használt környezettől függően érdemes lehet toodeploy hello mobilalkalmazás webszolgáltatás a hello Azure multi-factor Authentication kiszolgáló vagy egy másik kiszolgálón, az internet felé néző ugyanarra a kiszolgálóra.

## <a name="requirements"></a>Követelmények

toouse hello Microsoft Authenticator alkalmazást, hello következő szükségesek, hogy hello alkalmazás képes sikeresen kommunikálni mobilalkalmazás webszolgáltatás:

* Az Azure Multi-Factor Authentication-kiszolgáló 6.0-s vagy újabb verziója
* A Mobile App Web Service telepítése Microsoft® [Internet Information Services (IIS) 7.x vagy újabb](http://www.iis.net/) verziót futtató internetes webkiszolgálóra
* Az ASP.NET 4.0.30319 van regisztrált, és állítsa be a tooAllowed
* A szükséges szerepkör-szolgáltatások közé tartozik az ASP.NET és az IIS 6 metabázisával való kompatibilitás.
* A Mobile App Web Service elérhető egy nyilvános URL-címről
* A Mobile App Web Service védelmét egy SSL-tanúsítvány biztosítja.
* Hello Azure multi-factor Authentication webszolgáltatási SDK telepítése az IIS 7.x vagy nagyobb hello **hello Azure multi-factor Authentication kiszolgálót ugyanarra a kiszolgálóra**
* hello Azure multi-factor Authentication webszolgáltatási SDK egy SSL-tanúsítvánnyal védett.
* Mobilalkalmazás webszolgáltatás SSL-en keresztül kapcsolódhatnak toohello Azure multi-factor Authentication webszolgáltatási SDK
* Mobilalkalmazás webszolgáltatásának hitelesítheti toohello Azure multi-factor Authentication webszolgáltatási SDK egy szolgáltatás-fiók, amely hello "PhoneFactor Admins" biztonsági csoport tagja hello hitelesítő adataival. A szolgáltatásfiók és csoportfelügyeletű található az Active Directoryban, ha hello Azure multi-factor Authentication kiszolgáló egy tartományhoz csatlakozó kiszolgálón. A szolgáltatásfiók és csoportfelügyeletű található helyileg a hello Azure multi-factor Authentication kiszolgáló Ha nincs csatlakoztatott tooa tartományhoz.

## <a name="install-hello-mobile-app-web-service"></a>Hello mobilalkalmazás webszolgáltatásának telepítése

Hello mobilalkalmazás webszolgáltatás a telepítés előtt vegye figyelembe a következő adatok hello:

* Olyan szolgáltatásfiók szükséges, amely a „PhoneFactor-adminisztrátorok” csoport tagja. Ez a fiók lehet hello megegyezik egy hello használt hello felhasználói portál telepítése.
* Hasznos tooopen egy webes böngésző hello Internet felé néző webes kiszolgálón, és keresse meg a webszolgáltatási SDK hello web.config fájlban megadott hello toohello URL-CÍMÉT. Ha hello böngésző sikeresen kaphat toohello webszolgáltatás, azt kell kérni a hitelesítő adatokat. Adja meg a hello felhasználónévvel és jelszóval, pontosan úgy hello fájl megjelenik hello web.config fájlba vannak megadva. Győződjön meg arról, hogy nem látható tanúsítvánnyal kapcsolatos figyelmeztetés vagy hiba.
* Ha egy fordított proxykiszolgálón vagy tűzfalon elé hello mobilalkalmazás webszolgáltatásának webkiszolgáló óta várakozik, és SSL végrehajtása, mert, szerkesztheti hello mobilalkalmazás webszolgáltatás web.config fájl, így hello mobilalkalmazás webszolgáltatásának használhatja http helyett https. A mobilalkalmazás toohello tűzfal/fordított proxy hello továbbra is szükség az SSL. Adja hozzá a következő kulcs toohello hello \<appSettings\> szakasz:

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-hello-web-service-sdk"></a>Hello webszolgáltatási SDK telepítése

Mindkét esetben ha hello Azure multi-factor Authentication webszolgáltatási SDK, **nem** hello Azure multi-factor Authentication (MFA) kiszolgáló telepítve, teljes hello lépések olvashat.

1. Nyissa meg a hello multi-factor Authentication kiszolgáló konzol.
2. Nyissa meg toohello **webszolgáltatási SDK** válassza **webszolgáltatási SDK telepítése**.
3. Hello alapértelmezett beállításaival, kivéve, ha szüksége toochange teljes hello telepítés bármilyen okból őket.
4. Az IIS-ben az SSL-tanúsítvány toohello hely kötése.

Ha kérdései SSL-tanúsítvány konfigurálása az IIS-kiszolgálón, olvassa el hello [hogyan tooSet be SSL IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

Webszolgáltatási SDK hello az SSL-tanúsítványt kell biztosítani. Erre a célra megfelel egy önaláírt tanúsítvány. Importálja hello hello "Megbízható legfelső szintű hitelesítésszolgáltatók" tárolóban hello helyi számítógépfiók hello felhasználói portál webkiszolgálón úgy, hogy a tanúsítvány bízik hello SSL-kapcsolat kezdeményezésekor.

![MFA-kiszolgáló konfigurálása – Web Service SDK](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)

### <a name="install-hello-service"></a>Hello szolgáltatás telepítése

1. **A multi-factor Authentication kiszolgáló hello**, keresse meg a toohello telepítési útvonalat.
2. Keresse meg a toohello mappát, ahol hello Azure MFA kiszolgáló telepített hello alapértelmezett van **C:\Program Files\Azure multi-factor Authentication**.
3. Keresse meg a telepítőfájlt hello **MultiFactorAuthenticationMobileAppWebServiceSetup64**. Ha hello kiszolgáló **nem** internetre, a Másolás hello telepítési toohello internetre fájlkiszolgáló.
4. Ha hello MFA-kiszolgáló nem **nem** internetre kapcsoló toohello **internetre server**.
5. Futtassa a hello **MultiFactorAuthenticationMobileAppWebServiceSetup64** fájl telepítéséhez rendszergazdaként, hello hely módosítása, ha szükséges, és módosítsa a hello virtuális könyvtár tooa rövid nevét, ha azt szeretné.
6. Befejezési hello telepítése után Tallózás túl**C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService** (vagy hello virtuális könyvtár neve alapján a megfelelő könyvtár) és hello Web.Config fájl szerkesztésével.

   * Hello kulcs található **"WEB_SERVICE_SDK_AUTHENTICATION_USERNAME"** , és módosítsa **érték = ""** túl**érték = "Tartomány\felhasználónév"** tartomány\felhasználó esetén, amely része a szolgáltatásfiók "PhoneFactor Admins" csoporthoz.
   * Hello kulcs található **"Web_service_sdk_authentication_password értékét"** , és módosítsa **érték = ""** túl**érték = a "Password"** hello jelszó hello szolgáltatást a jelszó esetén Hello előző sorban megadott fiók.
   * Hello található **pfMobile alkalmazás webes Service_pfwssdk_PfWsSdk** beállítást és módosítsa hello értéket **http://localhost:4898/PfWsSdk.asmx** toohello webszolgáltatási SDK URL-címe (Példa: https://mfa.contoso.com/ MultiFactorAuthWebServiceSdk/PfWsSdk.asmx).
   * Hello Web.Config fájlt mentse és zárja be a Jegyzettömböt.

   > [!NOTE]
   > Mivel az SSL használata a kapcsolat esetén kell hivatkoznia hello webszolgáltatási SDK által **teljesen minősített tartománynevét (FQDN)** és **nem IP-cím**. hello SSL-tanúsítvány volna kiadott hello teljes Tartománynevét, és hello használt URL-címet meg kell egyeznie hello tanúsítvány hello neve.

7. Hello webhelyet, ahol a mobilalkalmazás webszolgáltatást telepítették alatt nem már kötve lett egy nyilvános aláírású tanúsítványt, ha hello tanúsítványt telepíteni hello kiszolgálóra, nyissa meg az IIS-kezelőt, és hello tanúsítvány toohello webhely kötése.
8. Nyisson meg egy webböngészőt bármely olyan számítógépről, és keresse meg a toohello URL-címe, ahová a mobilalkalmazás webszolgáltatás telepítve van-e (Példa: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService). Győződjön meg arról, hogy nem látható tanúsítvánnyal kapcsolatos figyelmeztetés vagy hiba.

## <a name="configure-hello-mobile-app-settings-in-hello-azure-multi-factor-authentication-server"></a>Az Azure multi-factor Authentication kiszolgáló hello hello mobilalkalmazás beállításainak konfigurálása

Most, hogy hello mobilalkalmazás webszolgáltatás telepítve van, szüksége tooconfigure hello Azure multi-factor Authentication kiszolgáló toowork hello portállal.

1. Hello multi-factor Authentication kiszolgáló konzolon kattintson a hello felhasználói portál ikonra. Ha a felhasználók számára engedélyezett toocontrol a hitelesítési módszerek ellenőrzése **mobilalkalmazás** hello beállítások lapon, a **engedélyezése a felhasználók tooselect metódus**. Anélkül, hogy ez a szolgáltatás engedélyezve van, a végfelhasználók számára szükséges toocontact a támogatási szolgálat toocomplete aktiválási hello mobilalkalmazás számára.
2. Ellenőrizze a hello **engedélyezése a felhasználók tooactivate mobilalkalmazás** mezőbe.
3. Ellenőrizze a hello **felhasználó regisztrálásának engedélyezése** mezőbe.
4. Kattintson a hello **mobilalkalmazás** ikonra.
5. Virtuális könyvtár hello MultiFactorAuthenticationMobileAppWebServiceSetup64 telepítésekor létrehozott használt hello URL-címét (például: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService/) hello mezőben  **Mobilalkalmazás webszolgáltatásának URL-címe:**.
6. Hello feltöltése **fióknév** mezőt a hello vállalat vagy szervezet nevét toodisplay a hello mobilalkalmazás ehhez a fiókhoz.
   ![MFA-kiszolgáló konfigurálása – A Mobile App beállításai](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)

## <a name="next-steps"></a>Következő lépések

- [Speciális, az Azure Multi-Factor Authenticationre és külső VPN-ekre vonatkozó forgatókönyvek](multi-factor-authentication-advanced-vpn-configurations.md).