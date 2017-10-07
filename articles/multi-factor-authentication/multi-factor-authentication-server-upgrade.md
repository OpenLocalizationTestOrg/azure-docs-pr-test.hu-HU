---
title: "aaaAzure MFA kiszolgáló frissítése |} Microsoft Docs"
description: "Lépéseket és útmutatást tooupgrade hello Azure multi-factor Authentication kiszolgáló tooa újabb verzióra."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 50bb8ac3-5559-4d8b-a96a-799a74978b14
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: aaa8d400e0e5f1c6be3a6d22cde6dd893ef4d546
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-toohello-latest-azure-multi-factor-authentication-server"></a>Frissítse a toohello legújabb Azure multi-factor Authentication kiszolgáló

Ez a cikk bemutatja, hogyan hello a frissítés az Azure multi-factor Authentication (MFA) kiszolgáló v6.0 folyamata vagy újabb verzióját. Ha tooupgrade hello PhoneFactor ügynökről régi verziója van szüksége, tekintse meg a túl[frissítési hello PhoneFactor ügynökről tooAzure multi-factor Authentication kiszolgáló](multi-factor-authentication-get-started-server-upgrade.md).

Ha frissíti a 6.x vagy régebbi toov7.x vagy újabb, az összes összetevő .NET 2.0 too.NET 4.5 módosítsa. Minden összetevő is Microsoft Visual C++ 2015 Redistributable Update 1 vagy újabb szükséges. hello MFA kiszolgáló telepítő telepíti a hello x86 és x64 verzióját is ezeket az összetevőket, ha még nincsenek telepítve. Ha a különálló kiszolgálókon futó hello felhasználói portál és a mobilalkalmazás webszolgáltatás kell tooinstall azokat a csomagokat az összetevőket a frissítés előtt. Hello a Microsoft Visual C++ 2015 Redistributable frissítés legújabb hello kereshet [Microsoft Download Center](https://www.microsoft.com/en-us/download/). 

## <a name="install-hello-latest-version-of-azure-mfa-server"></a>Azure MFA kiszolgáló hello legújabb verziójának telepítése

1. Hello utasításokat a [letöltési hello Azure multi-factor Authentication kiszolgáló](multi-factor-authentication-get-started-server.md#download-the-azure-multi-factor-authentication-server) tooget hello legújabb verziójára hello Azure MFA kiszolgáló.
2. Hello multi-factor Authentication kiszolgáló adatfájljából helye: C:\Program Files\Multi-Factor Authentication Server\Data\PhoneFactor.pfdata (feltételezve hello alapértelmezett telepítési helyére telepíti) a fő multi-factor Authentication kiszolgáló biztonsági másolatot készíteni.
3. Ha több kiszolgálót a magas rendelkezésre állású, módosítsa a hello ügyfélrendszer toohello MFA kiszolgáló tudják hitelesíteni magukat, így azok leállítása toohello kiszolgálók frissíteni olyan adatforgalmat küldenie. Ha terheléselosztót használ, multi-factor Authentication kiszolgáló eltávolítása a terheléselosztó hello, hello frissítése, és adja hello server hello farm programba.
4. Hello új telepítőt futtassa minden egyes multi-factor Authentication kiszolgálón. Először frissítenie alárendelt kiszolgálók, mert hello régi adatfájl hello főkiszolgálója replikált elolvasása. 

  Toouninstall az aktuális MFA kiszolgáló futó hello telepítő előtt nem kell. hello telepítő helybeni frissítést hajt végre. hello telepítési útvonal van felvételre hello beállításjegyzékből hello korábbi telepítés, így telepíti a hello azonos kiszolgálóra (például, C:\Program Files\Multi-Factor Authentication). 
  
5. Ha a Microsoft Visual C++ felszólító tooinstall 2015 újraterjeszthető csomag, hello kérés elfogadása. Mindkét hello x86 és x64 hello csomag verziója van telepítve.
5. Ha a webszolgáltatási SDK hello használatához a rendszer kéri tooinstall hello új webszolgáltatási SDK. Amikor telepít új webszolgáltatási SDK hello, győződjön meg arról, hogy hello virtuális könyvtár neve megegyezik a hello korábban telepített virtuális könyvtárat (például MultiFactorAuthWebServiceSdk).
6. Hello ismételje meg az összes alárendelt kiszolgálók. Hello beosztottak toobe hello új master, majd a frissítési hello régi fő server egyikét léptesse. 

## <a name="upgrade-hello-user-portal"></a>Hello felhasználói portál frissítése

1. Biztonsági másolat hello web.config fájl, amely a felhasználói portál telepítési helye (például C:\inetpub\wwwroot\MultiFactorAuth) hello hello virtuális könyvtára. Ha a módosítások toohello alapértelmezett téma, hello App_Themes\Default mappáját is biztonsági másolatot készíteni. Jobb toocreate hello alapértelmezett mappa olyan példányából, és hozzon létre egy új témát toochange hello alapértelmezett téma-nál.
2. Ha ugyanazon a kiszolgálón, a többi MFA kiszolgáló összetevő hello hello hello hello felhasználói portál fut az MFA kiszolgáló telepítési kéri tooupdate hello felhasználói portál. Hello kérés elfogadása és hello felhasználói portál frissítés telepítéséhez. Ellenőrizze, hogy hello virtuális könyvtár neve megegyezik a hello korábban telepített virtuális könyvtárat (például a MultiFactorAuth).
3. Ha hello felhasználói portál egy saját kiszolgálót, hello hello a MultiFactorAuthenticationUserPortalSetup64.msi fájl másolása hello multi-factor Authentication kiszolgálók közül az egyik helyen, és hello felhasználói portál webes kiszolgálóra. Hello telepítő futtatásához. 

  Ha a hiba akkor fordul elő, azzal, "A Microsoft Visual C++ 2015 Redistributable Update 1 vagy nagyobb szükség," letölthető és telepíthető hello legfrissebb csomagot hello [Microsoft Download Center](https://www.microsoft.com/download/). Mindkét hello x86 és x64 verzió telepítéséhez.

4. Hello szoftver telepítve van a felhasználói portál frissítése után összehasonlíthatja a hello web.config – biztonsági másolatának meg az 1. lépésben hello új web.config fájlhoz. Ha nem új attribútum hello új web.config található, másolja át a biztonsági mentési web.config hello virtuális könyvtár toooverwrite hello újat. Egy másik lehetőség toocopy és illessze be hello appSettings értékek pedig hello webszolgáltatási SDK URL-címe biztonságimásolat-fájlból hello hello új web.config be.

Ha több kiszolgálón felhasználói portál hello, ismételje meg a hello telepítési ezek. 


## <a name="upgrade-hello-mobile-app-web-service"></a>Frissítési hello mobilalkalmazás webszolgáltatása

1. Biztonsági másolat hello web.config fájl, amely virtuális könyvtára hello hello a mobilalkalmazás webszolgáltatás telepítési hely (például C:\inetpub\wwwroot\app vagy C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService).
2. Hello hello a multifactorauthenticationmobileappwebservicesetup64.msi programot fájl másolása telepítése hello MFA kiszolgáló helyét, és elhelyezi hello mobilalkalmazás regisztrációs webes kiszolgálóra.
3. Hello telepítő futtatásához. 

  Ha hiba lép fel, arról, hogy a Microsoft Visual C++ 2015 terjeszthető frissítésének 1-es vagy újabb rendszer szükséges, töltse le és telepítse hello legújabb frissítési csomagot a hello [Microsoft Download Center](https://www.microsoft.com/download/). Mindkét hello x86 és x64 verzió telepítéséhez.

4. Frissített hello mobilalkalmazás webszolgáltatásának szoftver telepítése után összehasonlíthatja a hello web.config fájl, amely az 1. lépésben hello új web.config fájl biztonsági mentése. Ha nem új attribútum hello új web.config található, másolja vissza a mentett web.config hello virtuális könyvtárba, és hello újat felülírja. Egy másik lehetőség toocopy és illessze be hello appSettings értékek pedig hello webszolgáltatási SDK URL-címe biztonságimásolat-fájlból hello hello új web.config be.

Ha több kiszolgálón mobilalkalmazás webszolgáltatásának hello, ismételje meg a hello telepítési ezek. 

## <a name="upgrade-hello-ad-fs-adapters"></a>AD FS-adapter frissítési hello


### <a name="if-mfa-runs-on-different-servers-than-ad-fs"></a>Ha MFA fut, mint az AD FS különböző kiszolgálókon

Ezek az utasítások csak akkor érvényesek, ha a multi-factor Authentication kiszolgáló külön futtatja a parancsot az AD FS-kiszolgálók. Ha mindkét szolgáltatás futtatnia hello ugyanazokat a kiszolgálókat, hagyja ki ezt a szakaszt, és nyissa meg toohello telepítési lépéseket. 

1. Hello MultiFactorAuthenticationAdfsAdapter.config fájlt az AD FS-ben regisztrált másolatának mentése, vagy használja a következő PowerShell-paranccsal hello hello konfigurációjának exportálása: `Export-AdfsAuthenticationProviderConfigurationData -Name [adapter name] -FilePath [path tooconfig file]`. hello adapternév értéke "WindowsAzureMultiFactorAuthentication" vagy "AzureMfaServerAuthentication" korábban telepített hello verziójától függően.
2. Másolja a következő fájlok hello MFA kiszolgáló telepítési helyét toohello AD FS kiszolgálók hello:

  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register-MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config

3. Szerkessze a hello Register-MultiFactorAuthenticationAdfsAdapter.ps1 parancsfájlt hozzáadásával `-ConfigurationFilePath [path]` hello toohello végét `Register-AdfsAuthenticationProvider` parancsot. Cserélje le *[elérési_út]* a hello teljes elérési útja toohello MultiFactorAuthenticationAdfsAdapter.config fájlt vagy hello konfigurációs fájl exportált hello előző lépésben. 

  Egyezés hello régi konfigurációs fájl hello új MultiFactorAuthenticationAdfsAdapter.config toosee hello attribútumok keresése Ha bármely attribútum hozzáadott vagy eltávolított hello új verziójában, másolja hello attribútumértékek hello régi konfigurációs fájl toohello újat vagy hello régi konfigurációs fájl toomatch módosíthatja.

### <a name="install-new-ad-fs-adapters"></a>Új AD FS-adapter telepítése

> [!IMPORTANT] 
> A felhasználók csak akkor szükséges tooperform kétlépéses ellenőrzés 3 – 8. lépések során a jelen szakasz. Ha az AD FS konfigurált több fürtök, eltávolíthatja, frissítés, és önállóan farm egyes fürtök hello visszaállítási hello más fürtök tooavoid állásidő.

1. Néhány AD FS-kiszolgáló eltávolítása hello farm. Ezeken a kiszolgálókon, miközben továbbra is futnak, mások hello frissítése.
2. Hello új AD FS-adapter telepítése hello AD FS-farm eltávolítja minden kiszolgálón. Ha hello MFA kiszolgáló telepítve van az AD FS-kiszolgálókon, frissítheti, hello MFA kiszolgáló rendszergazdája UX keresztül Ellenkező esetben frissítése MultiFactorAuthenticationAdfsAdapterSetup64.msi futtatásával. 

  Ha a hiba akkor fordul elő, azzal, "A Microsoft Visual C++ 2015 Redistributable Update 1 vagy nagyobb szükség," letölthető és telepíthető hello legfrissebb csomagot hello [Microsoft Download Center](https://www.microsoft.com/download/). Mindkét hello x86 és x64 verzió telepítéséhez.

3. Nyissa meg túl**AD FS** > **hitelesítési házirendek** > **általános többtényezős hitelesítési házirend szerkesztése**. Törölje a jelet **WindowsAzureMultiFactorAuthentication** vagy **AzureMFAServerAuthentication** (attól függően, hogy hello legfrissebb verzió van telepítve). 

  Ez a lépés végrehajtása után multi-factor Authentication kiszolgálón keresztül a kétlépéses ellenőrzést nem érhető el az AD FS fürt 8. lépés végrehajtásáig.

4. Unregister hello régebbi verziója hello AD FS-adapter hello Unregister-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell parancsfájl futtatásával. Győződjön meg arról, hogy hello *-név* paraméter ("WindowsAzureMultiFactorAuthentication" vagy "AzureMFAServerAuthentication") megegyezik a 3. lépésben megjelenő hello neve. Ez a hello ugyanazt az AD FS fürt kiszolgálóinak tooall érvényes, mivel nincs központi konfiguráció.
5. A regisztrálást hello új AD FS-adapter hello Register-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell-parancsfájl futtatásával. Ez a hello ugyanazt az AD FS fürt kiszolgálóinak tooall érvényes, mivel nincs központi konfiguráció.
6. Indítsa újra a hello AD FS szolgáltatás eltávolítja a hello AD FS-farm minden kiszolgálón.
7. Vegye fel a frissített hello kiszolgálók biztonsági toohello AD FS-farm, és távolítsa el más kiszolgálók hello farmból hello.
8. Nyissa meg túl**AD FS** > **hitelesítési házirendek** > **általános többtényezős hitelesítési házirend szerkesztése**. Ellenőrizze **AzureMfaServerAuthentication**.
9. Ismételje meg a 2. lépés tooupdate hello kiszolgálók hello AD FS-farm most eltávolítja, majd indítsa újra a hello AD FS szolgáltatás ezeken a kiszolgálókon.
10. Ezek a kiszolgálók hozzáadása vissza hello AD FS-farm.

## <a name="next-steps"></a>Következő lépések

- Példák az beszerzése [speciálisabb forgatókönyvek esetén az Azure multi-factor Authentication és a külső VPN-EK](multi-factor-authentication-advanced-vpn-configurations.md)

- [A Windows Server Active Directory MFA kiszolgáló szinkronizálása](multi-factor-authentication-get-started-server-dirint.md)

- [Windows-hitelesítés konfigurálása](multi-factor-authentication-get-started-server-windows.md) az alkalmazások számára
