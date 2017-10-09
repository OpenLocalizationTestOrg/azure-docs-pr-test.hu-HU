---
title: "aaaUpgrade PhoneFactor tooAzure MFA kiszolgáló |} Microsoft Docs"
description: "Ismerkedés az Azure MFA kiszolgáló hello régebbi phonefactor ügynökről történő frissítéskor."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 42838ff7-bdf2-4d06-bacc-b3839a00cd76
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: kgremban
ms.openlocfilehash: 15b7b8517929c30f66e6a39cd44c69d12d25c6d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-hello-phonefactor-agent-tooazure-multi-factor-authentication-server"></a>Hello PhoneFactor ügynökről tooAzure multi-factor Authentication kiszolgáló frissítése
tooupgrade hello PhoneFactor ügynökről 5.x vagy régebbi tooAzure multi-factor Authentication kiszolgáló, távolítsa el a PhoneFactor-ügynököket hello és a kapcsolt összetevők először. Majd hello a multi-factor Authentication kiszolgálón, és a kapcsolt összetevői telepíthetők.

## <a name="uninstall-hello-phonefactor-agent"></a>Hello PhoneFactor-ügynök eltávolítása

1. Először készítsen biztonsági másolatot hello PhoneFactor-adatfájlt. hello alapértelmezett telepítési hely: C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.

2. Ha telepítve van a felhasználói portál hello:
  1. Keresse meg a telepítési mappa toohello és hello web.config fájlról biztonsági másolatot készíteni. hello alapértelmezett telepítési hely a következő C:\inetpub\wwwroot\PhoneFactor.

  2. Egyéni témák toohello portal hozzáadott, készítsen biztonsági másolatot az egyéni mappa hello C:\inetpub\wwwroot\PhoneFactor\App_Themes könyvtár alatt.

  3. Hello felhasználói portálon keresztül hello PhoneFactor-ügynök eltávolítása (csak akkor használható, ha hello telepíthető ugyanarra a kiszolgálóra hello PhoneFactor ügynökről) vagy a Windows-programok és szolgáltatások segítségével történik.

3. Ha a mobilalkalmazás webszolgáltatása hello telepítve van:

  1. Nyissa meg toohello telepítési mappát, és hello web.config fájlról biztonsági másolatot készíteni. hello alapértelmezett telepítési hely a következő C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.

  2. Távolítsa el a hello mobilalkalmazás webszolgáltatásának Windows-programok és szolgáltatások segítségével történik.

4. Ha a webszolgáltatási SDK hello telepítve van, távolítsa el azt hello PhoneFactor-ügynök vagy a Windows-programok és szolgáltatások keresztül.

5. Távolítsa el a hello PhoneFactor-ügynököket Windows-programok és szolgáltatások segítségével történik.

## <a name="install-hello-multi-factor-authentication-server"></a>Hello multi-factor Authentication kiszolgáló telepítése

hello telepítési útvonal van felvételre hello korábbi PhoneFactor ügynökről telepítés hello beállításjegyzékből, akkor telepítse a hello azonos helyét (például, C:\Program Files\PhoneFactor). Az új telepítések eltérő alapértelmezett telepítési útvonallal rendelkeznek (pl. C:\Program Files\Multi-Factor Authentication Server). hello hello előző PhoneFactor-ügynök telepítése során kell frissíteni, a felhasználók és a beállítások továbbra is kell nincs telepítése után új multi-factor Authentication kiszolgáló hello által hátrahagyott adatfájlt.

1. Ha a rendszer kéri, hello multi-factor Authentication kiszolgáló aktiválása, és győződjön meg arról, toohello megfelelő replikációs csoporthoz van hozzárendelve.

2. Ha a webszolgáltatási SDK korábban telepítve lett, hello telepítése hello új webszolgáltatási SDK keresztül hello multi-factor Authentication kiszolgáló felhasználói felületét.

  az alapértelmezett virtuális könyvtár neve most hello **MultiFactorAuthWebServiceSdk** helyett **PhoneFactorWebServiceSdk**. Ha azt szeretné, hogy toouse hello korábbi név, módosítania kell hello virtuális könyvtár nevét hello telepítése során. Ellenkező esetben hello telepítési toouse hello új alapértelmezett neve engedélyezése esetén vannak toochange hello URL-cím olyan alkalmazást, a hivatkozás hello (például a felhasználói portál hello és a mobilalkalmazás webszolgáltatása) webszolgáltatási SDK toopoint hello megfelelő helyen.

3. Ha a felhasználói portál korábban telepítették a PhoneFactor-ügynök kiszolgáló, hello hello telepítése hello új multi-factor Authentication felhasználói portálon keresztül hello multi-factor Authentication kiszolgáló felhasználói felületét.

  az alapértelmezett virtuális könyvtár neve most hello **MultiFactorAuth** helyett **PhoneFactor**. Ha azt szeretné, hogy toouse hello korábbi név, módosítania kell hello virtuális könyvtár nevét hello telepítése során. Ellenkező esetben hello telepítési toouse hello új alapértelmezett neve engedélyezése esetén meg kell kattintson hello felhasználói portál hello multi-factor Authentication kiszolgáló és frissítse a felhasználói portál URL-CÍMÉT hello hello-beállítások lapon.

4. Ha korábban telepítették a másik kiszolgáló hello felhasználói portál és/vagy a mobilalkalmazás webszolgáltatás a PhoneFactor-ügynököket hello:

  1. Nyissa meg toohello telepítési helyet (például, C:\Program Files\PhoneFactor), és másolja át egy vagy több telepítők toohello tároló más kiszolgálón. Nincsenek a felhasználói portál hello és a mobilalkalmazás webszolgáltatása 32 bites és 64 bites telepítők. Ezeknek a neve MultiFactorAuthenticationUserPortalSetupXX.msi, illetve MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi.

  2. tooinstall hello felhasználói portál hello webkiszolgálón nyisson meg egy parancssort rendszergazdaként, és futtassa a MultiFactorAuthenticationUserPortalSetupXX.msi.

    az alapértelmezett virtuális könyvtár neve most hello **MultiFactorAuth** helyett **PhoneFactor**. Ha azt szeretné, hogy toouse hello korábbi név, módosítania kell hello virtuális könyvtár nevét hello telepítése során. Ellenkező esetben hello telepítési toouse hello új alapértelmezett neve engedélyezése esetén meg kell kattintson hello felhasználói portál hello multi-factor Authentication kiszolgáló és frissítse a felhasználói portál URL-CÍMÉT hello hello-beállítások lapon. A meglévő felhasználók toobe hello új URL-cím tájékoztatni kell.

  3. Nyissa meg toohello felhasználói portál telepítési helyet (például C:\inetpub\wwwroot\MultiFactorAuth), és szerkessze a hello web.config fájlt. Másolja át az eredeti web.config fájl, amely készült biztonsági másolat hello frissítés előtt hello új web.config fájlba hello appSettings és applicationSettings szakaszokat hello értékek. Ha új alapértelmezett virtuális könyvtár nevét hello tartották hello webszolgáltatási SDK telepítésekor, módosítsa a hello URL-címet, hello applicationSettings szakasz toopoint toohello megfelelő helyen. Hello korábbi web.config fájlban módosított többi alapértelmezett értéket, ha alkalmazza ezeket azonos módosítások toohello új web.config fájlt.

  4. tooinstall hello mobilalkalmazás webszolgáltatásának hello webkiszolgálón nyisson meg egy parancssort rendszergazdaként, és futtassa a hello MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi.

    az alapértelmezett virtuális könyvtár neve most hello **MultiFactorAuthMobileAppWebService** helyett **PhoneFactorPhoneAppWebService**. Ha azt szeretné, hogy toouse hello korábbi név, módosítania kell hello virtuális könyvtár nevét hello telepítése során. Érdemes lehet egy rövidebb nevet toomake toochoose megkönnyítik a végfelhasználók tootype a mobileszközükről. Ellenkező esetben hello telepítési toouse hello új alapértelmezett neve engedélyezése esetén meg kell kattintson hello mobilalkalmazás hello multi-factor Authentication kiszolgáló és hello Mobile App webes szolgáltatás URL-címet frissíteni.

  5. Nyissa meg toohello mobilalkalmazás webszolgáltatás telepítési helyet (például C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService), és szerkessze a hello web.config fájlt. Másolja át az eredeti web.config fájl, amely készült biztonsági másolat hello frissítés előtt hello új web.config fájlba hello appSettings és applicationSettings szakaszokat hello értékek. Ha új alapértelmezett virtuális könyvtár nevét hello tartották hello webszolgáltatási SDK telepítésekor, módosítsa a hello URL-címet, hello applicationSettings szakasz toopoint toohello megfelelő helyen. Hello korábbi web.config fájlban módosított többi alapértelmezett értéket, ha alkalmazza ezeket azonos módosítások toohello új web.config fájlt.

## <a name="next-steps"></a>Következő lépések

- [Hello felhasználói portál telepítése](multi-factor-authentication-get-started-portal.md) hello Azure multi-factor Authentication kiszolgáló számára.

- [Konfigurálja a Windows-hitelesítést](multi-factor-authentication-get-started-server-windows.md) az alkalmazásokhoz. 
