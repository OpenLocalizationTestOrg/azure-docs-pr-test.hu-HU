---
title: "az Azure Active Directory azonosító adatok védelmét és a Microsoft Graph lépései aaaGet |} Microsoft Docs"
description: "Egy bevezető tooquery Microsoft Graph biztosít a kockázati eseményekről és a kapcsolódó információk listáját az Azure Active Directoryból."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a kockázat esemény, a biztonsági rés, a biztonsági házirendet, a Microsoft Graph"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 75b8b7629a0120d8101f6fde0d9163122503d276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Ismerkedés az Azure Active Directory azonosító adatok védelmét és a Microsoft Graph
A Microsoft Graph hello Microsoft unified API-végpont és az otthoni hello [Azure Active Directory Identity Protection](active-directory-identityprotection.md) API-k. hello első API, **identityRiskEvents**, lehetővé teszi a Microsoft Graph tooquery listáját a [kockázati események](active-directory-identityprotection-risk-events-types.md) és a kapcsolódó adatokat. Ez a cikk lekérdezi az API lekérdezése lépésekhez. Egy részletes bemutatása, teljes dokumentációját, és hozzáférést toohello Graph Explorer: hello [Microsoft Graph hely](https://graph.microsoft.io/).

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.

Nincsenek Microsoft Graph három lépést tooaccessing Identity Protection adatokat:

1. Adja hozzá az alkalmazás egy ügyfélkulcsot. 
2. Használja ezt a titkos kulcsot és egyéb információkat tooauthenticate tooMicrosoft diagramot, amikor egy hitelesítési jogkivonatot kap néhány adatra. 
3. A token toomake kérelmek toohello API-végpont használja, és vissza Identity Protection-adatok beolvasása.

Mielőtt hozzáfogna, szüksége lesz:

* Rendszergazdai jogosultságok toocreate hello alkalmazás az Azure ad-ben
* a bérlői tartomány (például contoso.onmicrosoft.com) hello neve

## <a name="add-an-application-with-a-client-secret"></a>Egy ügyfél titokkal alkalmazás hozzáadása
1. [Jelentkezzen be a](https://manage.windowsazure.com) tooyour klasszikus Azure portálra rendszergazdaként. 
2. A hello bal oldali navigációs panelén kattintson **Active Directory**. 
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
4. Hello hello felső menüben kattintson a **alkalmazások**.
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. A hello **miről szeretne toodo** párbeszédpanel, kattintson **a szerveztem által fejlesztett alkalmazás hozzáadása**.
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. A hello **adja meg azt az alkalmazás** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    a. A hello **neve** szövegmező, írja be az alkalmazás nevét (pl.: AADIP kockázat esemény API-alkalmazás).
   
    b. Mint **típus**, jelölje be **webalkalmazás és / vagy webes API**.
   
    c. Kattintson a **Tovább** gombra.
8. A hello **alkalmazás tulajdonságainak** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    a. A hello **bejelentkezési URL-cím** szövegmezőhöz típus `http://localhost`.
   
    b. A hello **App ID URI** szövegmezőhöz típus `http://localhost`.
   
    c. Kattintson a **Befejezés** gombra.

Ön most már konfigurálhatja az alkalmazást.

![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-toouse-hello-api"></a>Adja meg az alkalmazás engedélyt toouse hello API
1. Kattintson az alkalmazás lap hello menüben hello felső részén található **konfigurálása**. 
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. A hello **engedélyek tooother alkalmazások** kattintson **alkalmazás hozzáadása**.
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. A hello **engedélyek tooother alkalmazások** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    a. Válassza ki **Microsoft Graph**.
   
    b. Kattintson a **Befejezés** gombra.
4. Kattintson a **Alkalmazásengedélyek: 0**, majd válassza ki **olvassa el az összes identity kockázati események adatait**.
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. Kattintson a **mentése** hello lap hello alján.
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a>Hívóbetű lekérése
1. Az alkalmazás lap hello **kulcsok** területen válassza ki a 1 év, időtartam.
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. Kattintson a **mentése** hello lap hello alján.
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. hello kulcsai című szakaszban másolja az újonnan létrehozott kulcs hello értékét, és illessze be egy biztonságos helyre.
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > Ha elveszti a kulcs, tooreturn toothis szakasz rendelkezik, és hozzon létre egy új kulcsot. Tartsa a kulcs titkos kulcs: bárki, aki hozzáférhet az adatokhoz.
   > 
   > 
4. A hello **tulajdonságok** szakaszban, a Másolás hello **ügyfél-azonosító**, majd illessze be egy biztonságos helyre. 

## <a name="authenticate-toomicrosoft-graph-and-query-hello-identity-risk-events-api"></a>TooMicrosoft Graph és lekérdezés hello identitás kockázati események API hitelesítéséhez
Ezen a ponton rendelkeznie kell:

* fent másolt hello ügyfél-azonosító
* fent másolt hello kulcs
* a bérlő tartománya hello neve

tooauthenticate, küldése post kérelem túl`https://login.microsoft.com` a következő paraméterek hello törzsében hello:

* grant_type: "**client_credentials**"
* erőforrás: "**https://graph.microsoft.com**"
* client_id:<your client ID>
* client_secret:<your key>

> [!NOTE]
> Hello szüksége tooprovide értékek **client_id** és hello **client_secret** paraméter.
> 
> 

Ha sikeres, ez egy hitelesítési jogkivonatot ad vissza.  
toocall hello API-t létrehozni hello paraméter a következő fejléc:

    `Authorization`=”<token_type> <access_token>"


Hitelesítésekor található hello a jogkivonat típusa és a hozzáférési jogkivonatot adott vissza hello.

Ezt a fejlécet küldeni egy kérelem toohello API URL-CÍMÉT a következő:`https://graph.microsoft.com/beta/identityRiskEvents`

hello választ, ha sikeres, gyűjteménye identitás kockázati eseményekről és hello OData JSON formátum, ami elemzése és kezelése, ahogyan az adatok.

Itt látható mintakód a hitelesítés és Powershell használatával hello API felület meghívásakor.  
Az ügyfél-azonosító csak hozzáadása kulcs, és a bérlői tartományhoz.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Következő lépések
Gratulálunk, az első hívás tooMicrosoft Graph most végzett!  
Most identitás kockázati események és hello adatokat használni, azonban tetszés szerint.

bővebben a Microsoft Graph toolearn, és hogyan toobuild használó alkalmazások hello Graph API-val, tekintse meg hello [dokumentáció](https://graph.microsoft.io/docs) és sok más, a hello [Microsoft Graph hely](https://graph.microsoft.io/). Továbbá győződjön meg arról, hogy toobookmark hello [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) oldal, amely tartalmazza az összes Identity Protection API-kat a Graph hello. Azt adja hozzá az új módon toowork az Identity Protection API-n keresztül, mint adott oldalon láthatja azokat.

## <a name="additional-resources"></a>További források
* [Az Azure Active Directory azonosító adatok védelmét](active-directory-identityprotection.md)
* [Azure Active Directory Identity Protection által észlelt kockázati események típusai](active-directory-identityprotection-risk-events-types.md)
* [Microsoft Graph](https://graph.microsoft.io/)
* [A Microsoft Graph áttekintése](https://graph.microsoft.io/docs)
* [Az Azure AD Identity Protection gyökerén](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

