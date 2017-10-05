---
title: "Ismerkedés az Azure Active Directory azonosító adatok védelmét és a Microsoft Graph |} Microsoft Docs"
description: "Bevezető a lekérdezés Microsoft Graph kockázati eseményekről és a kapcsolódó információk listáját az Azure Active Directoryból."
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
ms.openlocfilehash: 9b01ff86da6a1fd4a439a6ba59ea15ed6480cdad
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a><span data-ttu-id="1abe8-104">Ismerkedés az Azure Active Directory azonosító adatok védelmét és a Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="1abe8-104">Get started with Azure Active Directory Identity Protection and Microsoft Graph</span></span>
<span data-ttu-id="1abe8-105">A Microsoft Graph a Microsoft unified API-végpont és az otthoni [Azure Active Directory Identity Protection](active-directory-identityprotection.md) API-k.</span><span class="sxs-lookup"><span data-stu-id="1abe8-105">Microsoft Graph is the Microsoft unified API endpoint and the home of [Azure Active Directory Identity Protection](active-directory-identityprotection.md) APIs.</span></span> <span data-ttu-id="1abe8-106">Az első API **identityRiskEvents**, lehetővé teszi a Microsoft Graph listájának lekérdezési [kockázati események](active-directory-identityprotection-risk-events-types.md) és a kapcsolódó adatokat.</span><span class="sxs-lookup"><span data-stu-id="1abe8-106">The first API, **identityRiskEvents**, allows you to query Microsoft Graph for a list of [risk events](active-directory-identityprotection-risk-events-types.md) and associated information.</span></span> <span data-ttu-id="1abe8-107">Ez a cikk lekérdezi az API lekérdezése lépésekhez.</span><span class="sxs-lookup"><span data-stu-id="1abe8-107">This article gets you started querying this API.</span></span> <span data-ttu-id="1abe8-108">Egy részletes bemutatása, teljes dokumentációját, és a Graph Explorer elérésére, tekintse meg a [Microsoft Graph hely](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="1abe8-108">For an in-depth introduction, full documentation, and access to the Graph Explorer, see the [Microsoft Graph site](https://graph.microsoft.io/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1abe8-109">A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett.</span><span class="sxs-lookup"><span data-stu-id="1abe8-109">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span>

<span data-ttu-id="1abe8-110">A Microsoft Graph el az azonosító adatok védelmét az adatok három lépésben történik:</span><span class="sxs-lookup"><span data-stu-id="1abe8-110">There are three steps to accessing Identity Protection data through Microsoft Graph:</span></span>

1. <span data-ttu-id="1abe8-111">Adja hozzá az alkalmazás egy ügyfélkulcsot.</span><span class="sxs-lookup"><span data-stu-id="1abe8-111">Add an application with a client secret.</span></span> 
2. <span data-ttu-id="1abe8-112">A Microsoft Graph, amikor egy hitelesítési jogkivonatot kap hitelesítéshez használ a titkos kulcs és néhány egyéb adatot.</span><span class="sxs-lookup"><span data-stu-id="1abe8-112">Use this secret and a few other pieces of information to authenticate to Microsoft Graph, where you receive an authentication token.</span></span> 
3. <span data-ttu-id="1abe8-113">Ez a token használatával kéréseket küld az API-végpontot, és vissza Identity Protection-adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="1abe8-113">Use this token to make requests to the API endpoint and get Identity Protection data back.</span></span>

<span data-ttu-id="1abe8-114">Mielőtt hozzáfogna, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="1abe8-114">Before you get started, you’ll need:</span></span>

* <span data-ttu-id="1abe8-115">Az alkalmazás létrehozása az Azure AD rendszergazdai jogosultságokat</span><span class="sxs-lookup"><span data-stu-id="1abe8-115">Administrator privileges to create the application in Azure AD</span></span>
* <span data-ttu-id="1abe8-116">A bérlői tartomány (például contoso.onmicrosoft.com) neve</span><span class="sxs-lookup"><span data-stu-id="1abe8-116">The name of your tenant's domain (for example, contoso.onmicrosoft.com)</span></span>

## <a name="add-an-application-with-a-client-secret"></a><span data-ttu-id="1abe8-117">Egy ügyfél titokkal alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1abe8-117">Add an application with a client secret</span></span>
1. <span data-ttu-id="1abe8-118">[Jelentkezzen be a](https://manage.windowsazure.com) rendszergazdaként a klasszikus Azure portálra.</span><span class="sxs-lookup"><span data-stu-id="1abe8-118">[Sign in](https://manage.windowsazure.com) to your Azure classic portal as an administrator.</span></span> 
2. <span data-ttu-id="1abe8-119">A bal oldali navigációs panelén kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1abe8-119">On on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. <span data-ttu-id="1abe8-121">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="1abe8-121">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
4. <span data-ttu-id="1abe8-122">Kattintson a felső menüben **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1abe8-122">In the menu on the top, click **Applications**.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. <span data-ttu-id="1abe8-124">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="1abe8-124">Click **Add** at the bottom of the page.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. <span data-ttu-id="1abe8-126">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="1abe8-126">On the **What do you want to do** dialog, click **Add an application my organization is developing**.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. <span data-ttu-id="1abe8-128">Az a **adja meg azt az alkalmazás** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1abe8-128">On the **Tell us about your application** dialog, perform the following steps:</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    <span data-ttu-id="1abe8-130">a.</span><span class="sxs-lookup"><span data-stu-id="1abe8-130">a.</span></span> <span data-ttu-id="1abe8-131">Az a **neve** szövegmező, írja be az alkalmazás nevét (pl.: AADIP kockázat esemény API-alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="1abe8-131">In the **Name** textbox, type a name for your application (e.g.: AADIP Risk Event API Application).</span></span>
   
    <span data-ttu-id="1abe8-132">b.</span><span class="sxs-lookup"><span data-stu-id="1abe8-132">b.</span></span> <span data-ttu-id="1abe8-133">Mint **típus**, jelölje be **webalkalmazás és / vagy webes API**.</span><span class="sxs-lookup"><span data-stu-id="1abe8-133">As **Type**, select **Web Application And / Or Web API**.</span></span>
   
    <span data-ttu-id="1abe8-134">c.</span><span class="sxs-lookup"><span data-stu-id="1abe8-134">c.</span></span> <span data-ttu-id="1abe8-135">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="1abe8-135">Click **Next**.</span></span>
8. <span data-ttu-id="1abe8-136">Az a **alkalmazás tulajdonságainak** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1abe8-136">On the **App properties** dialog, perform the following steps:</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    <span data-ttu-id="1abe8-138">a.</span><span class="sxs-lookup"><span data-stu-id="1abe8-138">a.</span></span> <span data-ttu-id="1abe8-139">Az a **bejelentkezési URL-cím** szövegmezőhöz típus `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="1abe8-139">In the **Sign-On URL** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="1abe8-140">b.</span><span class="sxs-lookup"><span data-stu-id="1abe8-140">b.</span></span> <span data-ttu-id="1abe8-141">Az a **App ID URI** szövegmezőhöz típus `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="1abe8-141">In the **App ID URI** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="1abe8-142">c.</span><span class="sxs-lookup"><span data-stu-id="1abe8-142">c.</span></span> <span data-ttu-id="1abe8-143">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="1abe8-143">Click **Complete**.</span></span>

<span data-ttu-id="1abe8-144">Ön most már konfigurálhatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1abe8-144">Your can now configure your application.</span></span>

![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-to-use-the-api"></a><span data-ttu-id="1abe8-146">Adja meg az alkalmazás számára az API-val</span><span class="sxs-lookup"><span data-stu-id="1abe8-146">Grant your application permission to use the API</span></span>
1. <span data-ttu-id="1abe8-147">Az alkalmazás lapján, a felső menüben kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="1abe8-147">On your application's page, in the menu on the top, click **Configure**.</span></span> 
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. <span data-ttu-id="1abe8-149">Az a **egyéb alkalmazások engedélyei** kattintson **alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="1abe8-149">In the **permissions to other applications** section, click **Add application**.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. <span data-ttu-id="1abe8-151">Az a **egyéb alkalmazások engedélyei** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1abe8-151">On the **permissions to other applications** dialog, perform the following steps:</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    <span data-ttu-id="1abe8-153">a.</span><span class="sxs-lookup"><span data-stu-id="1abe8-153">a.</span></span> <span data-ttu-id="1abe8-154">Válassza ki **Microsoft Graph**.</span><span class="sxs-lookup"><span data-stu-id="1abe8-154">Select **Microsoft Graph**.</span></span>
   
    <span data-ttu-id="1abe8-155">b.</span><span class="sxs-lookup"><span data-stu-id="1abe8-155">b.</span></span> <span data-ttu-id="1abe8-156">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="1abe8-156">Click **Complete**.</span></span>
4. <span data-ttu-id="1abe8-157">Kattintson a **Alkalmazásengedélyek: 0**, majd válassza ki **olvassa el az összes identity kockázati események adatait**.</span><span class="sxs-lookup"><span data-stu-id="1abe8-157">Click **Application Permissions: 0**, and then select **Read all identity risk event information**.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. <span data-ttu-id="1abe8-159">Kattintson a lap alján található **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="1abe8-159">Click **Save** at the bottom of the page.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a><span data-ttu-id="1abe8-161">Hívóbetű lekérése</span><span class="sxs-lookup"><span data-stu-id="1abe8-161">Get an access key</span></span>
1. <span data-ttu-id="1abe8-162">Az alkalmazás lapon a a **kulcsok** területen válassza ki a 1 év, időtartam.</span><span class="sxs-lookup"><span data-stu-id="1abe8-162">On your application's page, in the **keys** section, select 1 year as duration.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. <span data-ttu-id="1abe8-164">Kattintson a lap alján található **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="1abe8-164">Click **Save** at the bottom of the page.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. <span data-ttu-id="1abe8-166">a kulcsok szakaszában másolja az újonnan létrehozott kulcs értékét, és illessze be egy biztonságos helyre.</span><span class="sxs-lookup"><span data-stu-id="1abe8-166">in the keys section, copy the value of your newly created key, and then paste it into a safe location.</span></span>
   
    ![Az alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > <span data-ttu-id="1abe8-168">Ha elveszti a kulcsot, akkor ez a szakasz való visszatéréshez, és hozzon létre egy új kulcsot.</span><span class="sxs-lookup"><span data-stu-id="1abe8-168">If you lose this key, you will have to return to this section and create a new key.</span></span> <span data-ttu-id="1abe8-169">Tartsa a kulcs titkos kulcs: bárki, aki hozzáférhet az adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="1abe8-169">Keep this key a secret: anyone who has it can access your data.</span></span>
   > 
   > 
4. <span data-ttu-id="1abe8-170">Az a **tulajdonságok** szakaszban, másolja a **ügyfél-azonosító**, majd illessze be egy biztonságos helyre.</span><span class="sxs-lookup"><span data-stu-id="1abe8-170">In the **properties** section, copy the **Client ID**, and then paste it into a safe location.</span></span> 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a><span data-ttu-id="1abe8-171">Microsoft Graph hitelesítést és az identitás kockázati események API lekérdezése</span><span class="sxs-lookup"><span data-stu-id="1abe8-171">Authenticate to Microsoft Graph and query the Identity Risk Events API</span></span>
<span data-ttu-id="1abe8-172">Ezen a ponton rendelkeznie kell:</span><span class="sxs-lookup"><span data-stu-id="1abe8-172">At this point, you should have:</span></span>

* <span data-ttu-id="1abe8-173">A fenti másolt ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="1abe8-173">The client ID you copied above</span></span>
* <span data-ttu-id="1abe8-174">A fenti másolt kulcsot</span><span class="sxs-lookup"><span data-stu-id="1abe8-174">The key you copied above</span></span>
* <span data-ttu-id="1abe8-175">A bérlői tartomány neve</span><span class="sxs-lookup"><span data-stu-id="1abe8-175">The name of your tenant's domain</span></span>

<span data-ttu-id="1abe8-176">A hitelesítéshez a feladás egy vagy több vonatkozó kérés küldése `https://login.microsoft.com` törzsében a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="1abe8-176">To authenticate, send a post request to `https://login.microsoft.com` with the following parameters in the body:</span></span>

* <span data-ttu-id="1abe8-177">grant_type: "**client_credentials**"</span><span class="sxs-lookup"><span data-stu-id="1abe8-177">grant_type: “**client_credentials**”</span></span>
* <span data-ttu-id="1abe8-178">erőforrás: "**https://graph.microsoft.com**"</span><span class="sxs-lookup"><span data-stu-id="1abe8-178">resource: “**https://graph.microsoft.com**”</span></span>
* <span data-ttu-id="1abe8-179">client_id:<your client ID></span><span class="sxs-lookup"><span data-stu-id="1abe8-179">client_id: <your client ID></span></span>
* <span data-ttu-id="1abe8-180">client_secret:<your key></span><span class="sxs-lookup"><span data-stu-id="1abe8-180">client_secret: <your key></span></span>

> [!NOTE]
> <span data-ttu-id="1abe8-181">Meg kell adni az értékeket a **client_id** és a **client_secret** paraméter.</span><span class="sxs-lookup"><span data-stu-id="1abe8-181">You need to provide values for the **client_id** and the **client_secret** parameter.</span></span>
> 
> 

<span data-ttu-id="1abe8-182">Ha sikeres, ez egy hitelesítési jogkivonatot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1abe8-182">If successful, this returns an authentication token.</span></span>  
<span data-ttu-id="1abe8-183">Hívja az API-t, hogy hozzon létre egy fejléc a következő paraméter:</span><span class="sxs-lookup"><span data-stu-id="1abe8-183">To call the API, create a header with the following parameter:</span></span>

    `Authorization`=”<token_type> <access_token>"


<span data-ttu-id="1abe8-184">Hitelesítésekor található a jogkivonat típusa és a hozzáférési jogkivonat a visszaküldött jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="1abe8-184">When authenticating, you can find the token type and access token in the returned token.</span></span>

<span data-ttu-id="1abe8-185">Ezt a fejlécet a kérés küldése a következő API URL-címe:`https://graph.microsoft.com/beta/identityRiskEvents`</span><span class="sxs-lookup"><span data-stu-id="1abe8-185">Send this header as a request to the following API URL: `https://graph.microsoft.com/beta/identityRiskEvents`</span></span>

<span data-ttu-id="1abe8-186">A válasz, ha sikeres, gyűjteménye identitás kockázati eseményekről és kapcsolódó adatait, amely elemzése és kezelése, ahogyan OData JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="1abe8-186">The response, if successful, is a collection of identity risk events and associated data in the OData JSON format, which can be parsed and handled as see fit.</span></span>

<span data-ttu-id="1abe8-187">Itt látható mintakód a hitelesítés és a Powershell használatával API felület meghívásakor.</span><span class="sxs-lookup"><span data-stu-id="1abe8-187">Here’s sample code for authenticating and calling the API using Powershell.</span></span>  
<span data-ttu-id="1abe8-188">Az ügyfél-azonosító csak hozzáadása kulcs, és a bérlői tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="1abe8-188">Just add your client ID, key, and tenant domain.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="1abe8-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1abe8-189">Next steps</span></span>
<span data-ttu-id="1abe8-190">Gratulálunk, a Microsoft Graph első hívás most végzett!</span><span class="sxs-lookup"><span data-stu-id="1abe8-190">Congratulations, you just made your first call to Microsoft Graph!</span></span>  
<span data-ttu-id="1abe8-191">Most identitás kockázati események és igényei azonban az adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="1abe8-191">Now you can query identity risk events and use the data however you see fit.</span></span>

<span data-ttu-id="1abe8-192">A Microsoft Graph és hogyan hozhat létre alkalmazásokat a Graph API-val kapcsolatos további tudnivalókért tekintse meg a [dokumentáció](https://graph.microsoft.io/docs) és sok más, a a [Microsoft Graph hely](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="1abe8-192">To learn more about Microsoft Graph and how to build applications using the Graph API, check out the [documentation](https://graph.microsoft.io/docs) and much more on the [Microsoft Graph site](https://graph.microsoft.io/).</span></span> <span data-ttu-id="1abe8-193">Bizonyosodjon meg arról, hogy lássa el könyvjelzővel a [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) oldal, amely felsorolja a identitás védelmi API-k gráfban.</span><span class="sxs-lookup"><span data-stu-id="1abe8-193">Also, make sure to bookmark the [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) page that lists all of the Identity Protection APIs available in Graph.</span></span> <span data-ttu-id="1abe8-194">Azt hozzáadja az új módon történő együttműködésre Identity Protection API-n keresztül, adott oldalon láthatja azokat.</span><span class="sxs-lookup"><span data-stu-id="1abe8-194">As we add new ways to work with Identity Protection via API, you’ll see them on that page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1abe8-195">További források</span><span class="sxs-lookup"><span data-stu-id="1abe8-195">Additional resources</span></span>
* [<span data-ttu-id="1abe8-196">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="1abe8-196">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
* [<span data-ttu-id="1abe8-197">Azure Active Directory Identity Protection által észlelt kockázati események típusai</span><span class="sxs-lookup"><span data-stu-id="1abe8-197">Types of risk events detected by Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-risk-events-types.md)
* [<span data-ttu-id="1abe8-198">Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="1abe8-198">Microsoft Graph</span></span>](https://graph.microsoft.io/)
* [<span data-ttu-id="1abe8-199">A Microsoft Graph áttekintése</span><span class="sxs-lookup"><span data-stu-id="1abe8-199">Overview of Microsoft Graph</span></span>](https://graph.microsoft.io/docs)
* [<span data-ttu-id="1abe8-200">Az Azure AD Identity Protection gyökerén</span><span class="sxs-lookup"><span data-stu-id="1abe8-200">Azure AD Identity Protection Service Root</span></span>](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

