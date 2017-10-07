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
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a><span data-ttu-id="fb09e-104">Ismerkedés az Azure Active Directory azonosító adatok védelmét és a Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="fb09e-104">Get started with Azure Active Directory Identity Protection and Microsoft Graph</span></span>
<span data-ttu-id="fb09e-105">A Microsoft Graph hello Microsoft unified API-végpont és az otthoni hello [Azure Active Directory Identity Protection](active-directory-identityprotection.md) API-k.</span><span class="sxs-lookup"><span data-stu-id="fb09e-105">Microsoft Graph is hello Microsoft unified API endpoint and hello home of [Azure Active Directory Identity Protection](active-directory-identityprotection.md) APIs.</span></span> <span data-ttu-id="fb09e-106">hello első API, **identityRiskEvents**, lehetővé teszi a Microsoft Graph tooquery listáját a [kockázati események](active-directory-identityprotection-risk-events-types.md) és a kapcsolódó adatokat.</span><span class="sxs-lookup"><span data-stu-id="fb09e-106">hello first API, **identityRiskEvents**, allows you tooquery Microsoft Graph for a list of [risk events](active-directory-identityprotection-risk-events-types.md) and associated information.</span></span> <span data-ttu-id="fb09e-107">Ez a cikk lekérdezi az API lekérdezése lépésekhez.</span><span class="sxs-lookup"><span data-stu-id="fb09e-107">This article gets you started querying this API.</span></span> <span data-ttu-id="fb09e-108">Egy részletes bemutatása, teljes dokumentációját, és hozzáférést toohello Graph Explorer: hello [Microsoft Graph hely](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="fb09e-108">For an in-depth introduction, full documentation, and access toohello Graph Explorer, see hello [Microsoft Graph site](https://graph.microsoft.io/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb09e-109">A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="fb09e-109">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span>

<span data-ttu-id="fb09e-110">Nincsenek Microsoft Graph három lépést tooaccessing Identity Protection adatokat:</span><span class="sxs-lookup"><span data-stu-id="fb09e-110">There are three steps tooaccessing Identity Protection data through Microsoft Graph:</span></span>

1. <span data-ttu-id="fb09e-111">Adja hozzá az alkalmazás egy ügyfélkulcsot.</span><span class="sxs-lookup"><span data-stu-id="fb09e-111">Add an application with a client secret.</span></span> 
2. <span data-ttu-id="fb09e-112">Használja ezt a titkos kulcsot és egyéb információkat tooauthenticate tooMicrosoft diagramot, amikor egy hitelesítési jogkivonatot kap néhány adatra.</span><span class="sxs-lookup"><span data-stu-id="fb09e-112">Use this secret and a few other pieces of information tooauthenticate tooMicrosoft Graph, where you receive an authentication token.</span></span> 
3. <span data-ttu-id="fb09e-113">A token toomake kérelmek toohello API-végpont használja, és vissza Identity Protection-adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="fb09e-113">Use this token toomake requests toohello API endpoint and get Identity Protection data back.</span></span>

<span data-ttu-id="fb09e-114">Mielőtt hozzáfogna, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="fb09e-114">Before you get started, you’ll need:</span></span>

* <span data-ttu-id="fb09e-115">Rendszergazdai jogosultságok toocreate hello alkalmazás az Azure ad-ben</span><span class="sxs-lookup"><span data-stu-id="fb09e-115">Administrator privileges toocreate hello application in Azure AD</span></span>
* <span data-ttu-id="fb09e-116">a bérlői tartomány (például contoso.onmicrosoft.com) hello neve</span><span class="sxs-lookup"><span data-stu-id="fb09e-116">hello name of your tenant's domain (for example, contoso.onmicrosoft.com)</span></span>

## <a name="add-an-application-with-a-client-secret"></a><span data-ttu-id="fb09e-117">Egy ügyfél titokkal alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fb09e-117">Add an application with a client secret</span></span>
1. <span data-ttu-id="fb09e-118">[Jelentkezzen be a](https://manage.windowsazure.com) tooyour klasszikus Azure portálra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="fb09e-118">[Sign in](https://manage.windowsazure.com) tooyour Azure classic portal as an administrator.</span></span> 
2. <span data-ttu-id="fb09e-119">A hello bal oldali navigációs panelén kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="fb09e-119">On on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. <span data-ttu-id="fb09e-121">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="fb09e-121">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
4. <span data-ttu-id="fb09e-122">Hello hello felső menüben kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fb09e-122">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. <span data-ttu-id="fb09e-124">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="fb09e-124">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. <span data-ttu-id="fb09e-126">A hello **miről szeretne toodo** párbeszédpanel, kattintson **a szerveztem által fejlesztett alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="fb09e-126">On hello **What do you want toodo** dialog, click **Add an application my organization is developing**.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. <span data-ttu-id="fb09e-128">A hello **adja meg azt az alkalmazás** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fb09e-128">On hello **Tell us about your application** dialog, perform hello following steps:</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    <span data-ttu-id="fb09e-130">a.</span><span class="sxs-lookup"><span data-stu-id="fb09e-130">a.</span></span> <span data-ttu-id="fb09e-131">A hello **neve** szövegmező, írja be az alkalmazás nevét (pl.: AADIP kockázat esemény API-alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="fb09e-131">In hello **Name** textbox, type a name for your application (e.g.: AADIP Risk Event API Application).</span></span>
   
    <span data-ttu-id="fb09e-132">b.</span><span class="sxs-lookup"><span data-stu-id="fb09e-132">b.</span></span> <span data-ttu-id="fb09e-133">Mint **típus**, jelölje be **webalkalmazás és / vagy webes API**.</span><span class="sxs-lookup"><span data-stu-id="fb09e-133">As **Type**, select **Web Application And / Or Web API**.</span></span>
   
    <span data-ttu-id="fb09e-134">c.</span><span class="sxs-lookup"><span data-stu-id="fb09e-134">c.</span></span> <span data-ttu-id="fb09e-135">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="fb09e-135">Click **Next**.</span></span>
8. <span data-ttu-id="fb09e-136">A hello **alkalmazás tulajdonságainak** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fb09e-136">On hello **App properties** dialog, perform hello following steps:</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    <span data-ttu-id="fb09e-138">a.</span><span class="sxs-lookup"><span data-stu-id="fb09e-138">a.</span></span> <span data-ttu-id="fb09e-139">A hello **bejelentkezési URL-cím** szövegmezőhöz típus `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="fb09e-139">In hello **Sign-On URL** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="fb09e-140">b.</span><span class="sxs-lookup"><span data-stu-id="fb09e-140">b.</span></span> <span data-ttu-id="fb09e-141">A hello **App ID URI** szövegmezőhöz típus `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="fb09e-141">In hello **App ID URI** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="fb09e-142">c.</span><span class="sxs-lookup"><span data-stu-id="fb09e-142">c.</span></span> <span data-ttu-id="fb09e-143">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="fb09e-143">Click **Complete**.</span></span>

<span data-ttu-id="fb09e-144">Ön most már konfigurálhatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fb09e-144">Your can now configure your application.</span></span>

![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-toouse-hello-api"></a><span data-ttu-id="fb09e-146">Adja meg az alkalmazás engedélyt toouse hello API</span><span class="sxs-lookup"><span data-stu-id="fb09e-146">Grant your application permission toouse hello API</span></span>
1. <span data-ttu-id="fb09e-147">Kattintson az alkalmazás lap hello menüben hello felső részén található **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="fb09e-147">On your application's page, in hello menu on hello top, click **Configure**.</span></span> 
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. <span data-ttu-id="fb09e-149">A hello **engedélyek tooother alkalmazások** kattintson **alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="fb09e-149">In hello **permissions tooother applications** section, click **Add application**.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. <span data-ttu-id="fb09e-151">A hello **engedélyek tooother alkalmazások** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fb09e-151">On hello **permissions tooother applications** dialog, perform hello following steps:</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    <span data-ttu-id="fb09e-153">a.</span><span class="sxs-lookup"><span data-stu-id="fb09e-153">a.</span></span> <span data-ttu-id="fb09e-154">Válassza ki **Microsoft Graph**.</span><span class="sxs-lookup"><span data-stu-id="fb09e-154">Select **Microsoft Graph**.</span></span>
   
    <span data-ttu-id="fb09e-155">b.</span><span class="sxs-lookup"><span data-stu-id="fb09e-155">b.</span></span> <span data-ttu-id="fb09e-156">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="fb09e-156">Click **Complete**.</span></span>
4. <span data-ttu-id="fb09e-157">Kattintson a **Alkalmazásengedélyek: 0**, majd válassza ki **olvassa el az összes identity kockázati események adatait**.</span><span class="sxs-lookup"><span data-stu-id="fb09e-157">Click **Application Permissions: 0**, and then select **Read all identity risk event information**.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. <span data-ttu-id="fb09e-159">Kattintson a **mentése** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="fb09e-159">Click **Save** at hello bottom of hello page.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a><span data-ttu-id="fb09e-161">Hívóbetű lekérése</span><span class="sxs-lookup"><span data-stu-id="fb09e-161">Get an access key</span></span>
1. <span data-ttu-id="fb09e-162">Az alkalmazás lap hello **kulcsok** területen válassza ki a 1 év, időtartam.</span><span class="sxs-lookup"><span data-stu-id="fb09e-162">On your application's page, in hello **keys** section, select 1 year as duration.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. <span data-ttu-id="fb09e-164">Kattintson a **mentése** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="fb09e-164">Click **Save** at hello bottom of hello page.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. <span data-ttu-id="fb09e-166">hello kulcsai című szakaszban másolja az újonnan létrehozott kulcs hello értékét, és illessze be egy biztonságos helyre.</span><span class="sxs-lookup"><span data-stu-id="fb09e-166">in hello keys section, copy hello value of your newly created key, and then paste it into a safe location.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > <span data-ttu-id="fb09e-168">Ha elveszti a kulcs, tooreturn toothis szakasz rendelkezik, és hozzon létre egy új kulcsot.</span><span class="sxs-lookup"><span data-stu-id="fb09e-168">If you lose this key, you will have tooreturn toothis section and create a new key.</span></span> <span data-ttu-id="fb09e-169">Tartsa a kulcs titkos kulcs: bárki, aki hozzáférhet az adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="fb09e-169">Keep this key a secret: anyone who has it can access your data.</span></span>
   > 
   > 
4. <span data-ttu-id="fb09e-170">A hello **tulajdonságok** szakaszban, a Másolás hello **ügyfél-azonosító**, majd illessze be egy biztonságos helyre.</span><span class="sxs-lookup"><span data-stu-id="fb09e-170">In hello **properties** section, copy hello **Client ID**, and then paste it into a safe location.</span></span> 

## <a name="authenticate-toomicrosoft-graph-and-query-hello-identity-risk-events-api"></a><span data-ttu-id="fb09e-171">TooMicrosoft Graph és lekérdezés hello identitás kockázati események API hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="fb09e-171">Authenticate tooMicrosoft Graph and query hello Identity Risk Events API</span></span>
<span data-ttu-id="fb09e-172">Ezen a ponton rendelkeznie kell:</span><span class="sxs-lookup"><span data-stu-id="fb09e-172">At this point, you should have:</span></span>

* <span data-ttu-id="fb09e-173">fent másolt hello ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="fb09e-173">hello client ID you copied above</span></span>
* <span data-ttu-id="fb09e-174">fent másolt hello kulcs</span><span class="sxs-lookup"><span data-stu-id="fb09e-174">hello key you copied above</span></span>
* <span data-ttu-id="fb09e-175">a bérlő tartománya hello neve</span><span class="sxs-lookup"><span data-stu-id="fb09e-175">hello name of your tenant's domain</span></span>

<span data-ttu-id="fb09e-176">tooauthenticate, küldése post kérelem túl`https://login.microsoft.com` a következő paraméterek hello törzsében hello:</span><span class="sxs-lookup"><span data-stu-id="fb09e-176">tooauthenticate, send a post request too`https://login.microsoft.com` with hello following parameters in hello body:</span></span>

* <span data-ttu-id="fb09e-177">grant_type: "**client_credentials**"</span><span class="sxs-lookup"><span data-stu-id="fb09e-177">grant_type: “**client_credentials**”</span></span>
* <span data-ttu-id="fb09e-178">erőforrás: "**https://graph.microsoft.com**"</span><span class="sxs-lookup"><span data-stu-id="fb09e-178">resource: “**https://graph.microsoft.com**”</span></span>
* <span data-ttu-id="fb09e-179">client_id:<your client ID></span><span class="sxs-lookup"><span data-stu-id="fb09e-179">client_id: <your client ID></span></span>
* <span data-ttu-id="fb09e-180">client_secret:<your key></span><span class="sxs-lookup"><span data-stu-id="fb09e-180">client_secret: <your key></span></span>

> [!NOTE]
> <span data-ttu-id="fb09e-181">Hello szüksége tooprovide értékek **client_id** és hello **client_secret** paraméter.</span><span class="sxs-lookup"><span data-stu-id="fb09e-181">You need tooprovide values for hello **client_id** and hello **client_secret** parameter.</span></span>
> 
> 

<span data-ttu-id="fb09e-182">Ha sikeres, ez egy hitelesítési jogkivonatot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="fb09e-182">If successful, this returns an authentication token.</span></span>  
<span data-ttu-id="fb09e-183">toocall hello API-t létrehozni hello paraméter a következő fejléc:</span><span class="sxs-lookup"><span data-stu-id="fb09e-183">toocall hello API, create a header with hello following parameter:</span></span>

    `Authorization`=”<token_type> <access_token>"


<span data-ttu-id="fb09e-184">Hitelesítésekor található hello a jogkivonat típusa és a hozzáférési jogkivonatot adott vissza hello.</span><span class="sxs-lookup"><span data-stu-id="fb09e-184">When authenticating, you can find hello token type and access token in hello returned token.</span></span>

<span data-ttu-id="fb09e-185">Ezt a fejlécet küldeni egy kérelem toohello API URL-CÍMÉT a következő:`https://graph.microsoft.com/beta/identityRiskEvents`</span><span class="sxs-lookup"><span data-stu-id="fb09e-185">Send this header as a request toohello following API URL: `https://graph.microsoft.com/beta/identityRiskEvents`</span></span>

<span data-ttu-id="fb09e-186">hello választ, ha sikeres, gyűjteménye identitás kockázati eseményekről és hello OData JSON formátum, ami elemzése és kezelése, ahogyan az adatok.</span><span class="sxs-lookup"><span data-stu-id="fb09e-186">hello response, if successful, is a collection of identity risk events and associated data in hello OData JSON format, which can be parsed and handled as see fit.</span></span>

<span data-ttu-id="fb09e-187">Itt látható mintakód a hitelesítés és Powershell használatával hello API felület meghívásakor.</span><span class="sxs-lookup"><span data-stu-id="fb09e-187">Here’s sample code for authenticating and calling hello API using Powershell.</span></span>  
<span data-ttu-id="fb09e-188">Az ügyfél-azonosító csak hozzáadása kulcs, és a bérlői tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="fb09e-188">Just add your client ID, key, and tenant domain.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="fb09e-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fb09e-189">Next steps</span></span>
<span data-ttu-id="fb09e-190">Gratulálunk, az első hívás tooMicrosoft Graph most végzett!</span><span class="sxs-lookup"><span data-stu-id="fb09e-190">Congratulations, you just made your first call tooMicrosoft Graph!</span></span>  
<span data-ttu-id="fb09e-191">Most identitás kockázati események és hello adatokat használni, azonban tetszés szerint.</span><span class="sxs-lookup"><span data-stu-id="fb09e-191">Now you can query identity risk events and use hello data however you see fit.</span></span>

<span data-ttu-id="fb09e-192">bővebben a Microsoft Graph toolearn, és hogyan toobuild használó alkalmazások hello Graph API-val, tekintse meg hello [dokumentáció](https://graph.microsoft.io/docs) és sok más, a hello [Microsoft Graph hely](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="fb09e-192">toolearn more about Microsoft Graph and how toobuild applications using hello Graph API, check out hello [documentation](https://graph.microsoft.io/docs) and much more on hello [Microsoft Graph site](https://graph.microsoft.io/).</span></span> <span data-ttu-id="fb09e-193">Továbbá győződjön meg arról, hogy toobookmark hello [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) oldal, amely tartalmazza az összes Identity Protection API-kat a Graph hello.</span><span class="sxs-lookup"><span data-stu-id="fb09e-193">Also, make sure toobookmark hello [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) page that lists all of hello Identity Protection APIs available in Graph.</span></span> <span data-ttu-id="fb09e-194">Azt adja hozzá az új módon toowork az Identity Protection API-n keresztül, mint adott oldalon láthatja azokat.</span><span class="sxs-lookup"><span data-stu-id="fb09e-194">As we add new ways toowork with Identity Protection via API, you’ll see them on that page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fb09e-195">További források</span><span class="sxs-lookup"><span data-stu-id="fb09e-195">Additional resources</span></span>
* [<span data-ttu-id="fb09e-196">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="fb09e-196">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
* [<span data-ttu-id="fb09e-197">Azure Active Directory Identity Protection által észlelt kockázati események típusai</span><span class="sxs-lookup"><span data-stu-id="fb09e-197">Types of risk events detected by Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-risk-events-types.md)
* [<span data-ttu-id="fb09e-198">Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="fb09e-198">Microsoft Graph</span></span>](https://graph.microsoft.io/)
* [<span data-ttu-id="fb09e-199">A Microsoft Graph áttekintése</span><span class="sxs-lookup"><span data-stu-id="fb09e-199">Overview of Microsoft Graph</span></span>](https://graph.microsoft.io/docs)
* [<span data-ttu-id="fb09e-200">Az Azure AD Identity Protection gyökerén</span><span class="sxs-lookup"><span data-stu-id="fb09e-200">Azure AD Identity Protection Service Root</span></span>](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

