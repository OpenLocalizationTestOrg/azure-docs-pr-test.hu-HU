---
title: "a Mobile Engagement-REST API-k - manuális telepítési módra aaaAuthenticate"
description: "Ismerteti, hogyan toomanually beállítani a Mobile Engagement REST API-k hitelesítés"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3884f94afcd6b9a62bfcf498fb6ee84bb6e837b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Azokkal a Mobile Engagement-REST API-k - manuális beállítása
Ez a egy függelék dokumentáció túl[hitelesítés a Mobile Engagement REST API-k](mobile-engagement-api-authentication.md). Győződjön meg arról, hogy olvassa el, az első tooget hello környezetben. Ez ez a cikk ismerteti egy másik módja toodo hello egyszeri telepítőprogramja a hitelesítés beállítása hello Mobile Engagement REST API-k használatával hello Azure portálon. 

> [!NOTE]
> az alábbi utasítások hello ez alapuló [útmutató az Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md) és testre szabható a Mobile Engagement API-k hitelesítéshez szükséges. Ezért tekintse meg tooit, ha azt szeretné, hogy toounderstand hello alábbi lépéseket részletesen. 
> 
> 

1. Bejelentkezési tooyour Azure-fiók keresztül hello [klasszikus portál](https://manage.windowsazure.com/).
2. Válassza ki **Active Directory** hello bal oldali ablaktáblán.
   
     ![az Active Directory kiválasztása][1]
3. Válassza ki a hello **Active Directory alapértelmezett** az Azure-portálon. 
   
     ![Válassza ki a könyvtár][2]
   
   > [!IMPORTANT]
   > Ez a módszer csak akkor, ha az Active Directory-fiókja hello alapértelmezett dolgozik, és nem fog működni, akkor használatos, ha ez egy Active Directory fiókjában létrehozott működik. 
   > 
   > 
4. tooview hello alkalmazások a könyvtárban kattintson a **alkalmazások**.
   
     ![alkalmazások megtekintése][3]
5. Kattintson a **hozzáadása**. 
   
     ![Alkalmazás hozzáadása][4]
6. Kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**
   
     ![Új alkalmazás][5]
7. Adja meg a hello alkalmazás és az alkalmazás válassza hello típusú nevét **WEB APPLICATION AND/OR WEB API** és hello Tovább gombra.
   
     ![alkalmazás neve][6]
8. Megadhatja, hogy bármilyen típusú URL-címéből **SIGN-ON URL** és **APP ID URI**. A mi esetünkben nem használhatók, és maguk hello URL-címeket a rendszer nem érvényesíti.  
   
     ![Alkalmazás tulajdonságai][7]
9. Ezen hello végén kell hasonló hello korábban megadott hello nevű alkalmazás aad-ben. Ez a **AD\_APP\_neve** és jegyezze fel a azt.  
   
     ![Alkalmazás neve][8]
10. Kattintson a hello alkalmazás nevére, majd kattintson a **konfigurálása**.
    
      ![alkalmazás konfigurálása][9]
11. Jegyezze fel a használandó ügyfél-azonosító hello **ügyfél\_azonosító** az API-hívásokat. 
    
     ![alkalmazás konfigurálása][10]
12. Görgessen lefelé toohello **kulcsok** szakaszt, és adja hozzá a 2 év (lejárati) időtartama lehetőleg kulcsot, és kattintson **mentése**. 
    
     ![alkalmazás konfigurálása][11]
13. Azonnal másolási hello érték látható hello kulcs csak akkor jelenik meg most és nem tárolja, nem fog megjelenni egyre újra. Ha elveszített majd akkor toogenerate egy új kulcsot. Ez lesz a hello **CLIENT_SECRET** az API-hívásokat. 
    
     ![alkalmazás konfigurálása][12]
    
    > [!IMPORTANT]
    > Ezt a kulcsot, hogy a megadott igen győződjön meg arról, hogy toorenew, ha hello idő ellenkező esetben az API-hitelesítés nem fog többé működni hello végén lévő hello időtartama lejár. Töröl, és hozza létre újból ezt a kulcsot, ha úgy gondolja, hogy veszélyben van.
    > 
    > 
14. Kattintson a **VÉGPONTOK megtekintése** gombra kattint, most amely hello megnyílik **App végpontok** párbeszédpanel megnyitásához. 
    
    ![][13]
15. Hello App végpontok párbeszédpanel, másolja a hello **OAUTH 2.0 TOKEN-VÉGPONT**. 
    
    ![][14]
16. Ez a végpont kerül, ahol hello GUID hello URL-címben az űrlap következő hello a **TENANT_ID** , jegyezze fel a azt: 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. Most azt folytatódik tooconfigure hello engedélyeinek ezt az alkalmazást. Ez tooopen hello fel lesz [Azure-portálon](https://portal.azure.com). 
18. Kattintson a **erőforráscsoportok** és hello található **a Mobile Engagement** erőforráscsoportot.  
    
    ![][15]
19. Hello kattintson **a Mobile Engagement** erőforrás csoportban, és keresse meg a toohello **beállítások** itt panelen. 
    
    ![][16]
20. Kattintson a **felhasználók** a hello beállítások panelen, majd kattintson a **Hozzáadás** tooadd a felhasználó. 
    
    ![][17]
21. Kattintson a **szerepkör kiválasztása**
    
    ![][18]
22. Kattintson a **tulajdonosa**
    
    ![][19]
23. Keresse meg az alkalmazás nevére hello **AD\_APP\_neve** hello Keresés mezőbe. Nem látják ezt itt alapértelmezés szerint. Miután megtalálta, válassza ki azt, majd kattintson a **válasszon** hello hello panel alsó részén. 
    
    ![][20]
24. A hello **hozzáférés hozzáadása** panelen megjelenik **1 felhasználó, a 0 csoportok**. Kattintson a **OK** a panel tooconfirm hello változás. 
    
    ![][21]

Ezzel befejezte a szükséges hello AAD-konfigurációt, és minden set toocall hello API-k. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



