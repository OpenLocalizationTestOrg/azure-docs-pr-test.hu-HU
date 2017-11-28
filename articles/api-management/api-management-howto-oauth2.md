---
title: "az OAuth 2.0 verziót használja az Azure API Management aaaAuthorize fejlesztői fiókok |} Microsoft Docs"
description: "Megtudhatja, hogyan tooauthorize felhasználók OAuth 2.0 API Management használata."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 78c48247-64f0-4708-b2d0-98b61a821283
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 934901dd6df399470a3257bf7a3a9b9fb5f40d5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-using-oauth-20-in-azure-api-management"></a><span data-ttu-id="69fb2-103">Hogyan tooauthorize fejlesztői fiókok OAuth 2.0, az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="69fb2-103">How tooauthorize developer accounts using OAuth 2.0 in Azure API Management</span></span>
<span data-ttu-id="69fb2-104">Számos API támogatja [OAuth 2.0](http://oauth.net/2/) toosecure hello API, és győződjön meg arról, hogy csak érvényes felhasználók hozzáférhetnek, és csak eléréséhez erőforrások toowhich azok még jogosult.</span><span class="sxs-lookup"><span data-stu-id="69fb2-104">Many APIs support [OAuth 2.0](http://oauth.net/2/) toosecure hello API and ensure that only valid users have access, and they can only access resources toowhich they're entitled.</span></span> <span data-ttu-id="69fb2-105">A sorrend toouse Azure API Management meg interaktív fejlesztői konzolján ilyen API-khoz, hello szolgáltatás lehetővé teszi tooconfigure a szolgáltatás példány toowork az OAuth 2.0 API engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="69fb2-105">In order toouse Azure API Management's interactive Developer Console with such APIs, hello service allows you tooconfigure your service instance toowork with your OAuth 2.0 enabled API.</span></span>

## <span data-ttu-id="69fb2-106"><a name="prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="69fb2-106"><a name="prerequisites"> </a>Prerequisites</span></span>
<span data-ttu-id="69fb2-107">Ez az útmutató bemutatja, miként tooconfigure az API Management szolgáltatás példány toouse OAuth 2.0-engedélyezés fejlesztői fiókok számára, azonban nem mutatja be tooconfigure OAuth 2.0-s szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="69fb2-107">This guide shows you how tooconfigure your API Management service instance toouse OAuth 2.0 authorization for developer accounts, but does not show you how tooconfigure an OAuth 2.0 provider.</span></span> <span data-ttu-id="69fb2-108">Minden szolgáltató eltér, bár hello lépések hasonlóak, és az API Management szolgáltatáspéldány OAuth 2.0 konfigurálásához használt szükséges hello adatra OAuth 2.0 hello konfigurációs hello azonos.</span><span class="sxs-lookup"><span data-stu-id="69fb2-108">hello configuration for each OAuth 2.0 provider is different, although hello steps are similar, and hello required pieces of information used in configuring OAuth 2.0 in your API Management service instance are hello same.</span></span> <span data-ttu-id="69fb2-109">Ez a témakör bemutatja az Azure Active Directoryt használja az OAuth 2.0-s szolgáltató példák.</span><span class="sxs-lookup"><span data-stu-id="69fb2-109">This topic shows examples using Azure Active Directory as an OAuth 2.0 provider.</span></span>

> [!NOTE]
> <span data-ttu-id="69fb2-110">Az Azure Active Directoryval OAuth 2.0 konfigurálásáról további információkért lásd: hello [WebApp-GraphAPI-DotNet] [ WebApp-GraphAPI-DotNet] minta.</span><span class="sxs-lookup"><span data-stu-id="69fb2-110">For more information on configuring OAuth 2.0 using Azure Active Directory, see hello [WebApp-GraphAPI-DotNet][WebApp-GraphAPI-DotNet] sample.</span></span>
> 
> 

## <span data-ttu-id="69fb2-111"><a name="step1"></a>OAuth 2.0 hitelesítési kiszolgáló beállítása az API Management</span><span class="sxs-lookup"><span data-stu-id="69fb2-111"><a name="step1"> </a>Configure an OAuth 2.0 authorization server in API Management</span></span>
<span data-ttu-id="69fb2-112">tooget elindítani, kattintson a **Publisher portal** a hello Azure portál, az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="69fb2-112">tooget started, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Közzétevő portál][api-management-management-console]

> [!NOTE]
> <span data-ttu-id="69fb2-114">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="69fb2-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="69fb2-115">Kattintson a **biztonsági** a hello **API Management** menü hello balra, kattintson a **OAuth 2.0**, és kattintson a **engedélyezési kiszolgáló hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="69fb2-115">Click **Security** from hello **API Management** menu on hello left, click **OAuth 2.0**, and then click **Add authorization server**.</span></span>

![OAuth 2.0][api-management-oauth2]

<span data-ttu-id="69fb2-117">Miután rákattintott **engedélyezési kiszolgáló hozzáadása**, hello új engedélyezési server képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="69fb2-117">After clicking **Add authorization server**, hello new authorization server form is displayed.</span></span>

![Új kiszolgáló][api-management-oauth2-server-1]

<span data-ttu-id="69fb2-119">Adjon meg egy nevet és leírást nem kötelező hello **neve** és **leírás** mezőket.</span><span class="sxs-lookup"><span data-stu-id="69fb2-119">Enter a name and an optional description in hello **Name** and **Description** fields.</span></span> 

> [!NOTE]
> <span data-ttu-id="69fb2-120">A mezők kitöltése használt tooidentify hello OAuth 2.0 hitelesítési kiszolgáló hello aktuális API Management szolgáltatáspéldány belül, és azok értékeit nem hello OAuth 2.0 kiszolgálóról származnak.</span><span class="sxs-lookup"><span data-stu-id="69fb2-120">These fields are used tooidentify hello OAuth 2.0 authorization server within hello current API Management service instance and their values do not come from hello OAuth 2.0 server.</span></span>
> 
> 

<span data-ttu-id="69fb2-121">Adja meg a hello **ügyfél regisztrációs URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="69fb2-121">Enter hello **Client registration page URL**.</span></span> <span data-ttu-id="69fb2-122">Ez a lap, ahol a felhasználók létrehozása és azok a fiókok kezelése és a használt hello OAuth 2.0-s szolgáltató függ.</span><span class="sxs-lookup"><span data-stu-id="69fb2-122">This page is where users can create and manage their accounts, and varies depending on hello OAuth 2.0 provider used.</span></span> <span data-ttu-id="69fb2-123">Hello **ügyfél regisztrációs URL-címe** toohello lapon, hogy a felhasználók pontok toocreate használhatja és a saját felhasználói konfigurálása a felhasználói fiókok kezelését támogató OAuth 2.0-s szolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="69fb2-123">hello **Client registration page URL** points toohello page that users can use toocreate and configure their own accounts for OAuth 2.0 providers that support user management of accounts.</span></span> <span data-ttu-id="69fb2-124">Egyes szervezetek konfigurálásához, vagy nem használja ezt a funkciót, még akkor is, ha hello OAuth 2.0-s szolgáltató támogatja.</span><span class="sxs-lookup"><span data-stu-id="69fb2-124">Some organizations do not configure or use this functionality even if hello OAuth 2.0 provider supports it.</span></span> <span data-ttu-id="69fb2-125">Az OAuth 2.0-s szolgáltató nem rendelkezik konfigurált fiókok felhasználói kezelését, ha meg egy helyőrzőt URL-címet itt hello vállalata, és egy URL-címe például például `https://placeholder.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="69fb2-125">If your OAuth 2.0 provider does not have user management of accounts configured, enter a placeholder URL here such as hello URL of your company, or a URL such as `https://placeholder.contoso.com`.</span></span>

<span data-ttu-id="69fb2-126">hello hello űrlap következő szakaszában található hello **engedélyezési kódot adjon típusok**, **engedélyezési végpont URL-címet**, és **engedélyezési metódus** beállításait.</span><span class="sxs-lookup"><span data-stu-id="69fb2-126">hello next section of hello form contains hello **Authorization code grant types**, **Authorization endpoint URL**, and **Authorization request method** settings.</span></span>

![Új kiszolgáló][api-management-oauth2-server-2]

<span data-ttu-id="69fb2-128">Adja meg a hello **engedélyezési kódot adjon típusok** szükséges hello típusok ellenőrzésével.</span><span class="sxs-lookup"><span data-stu-id="69fb2-128">Specify hello **Authorization code grant types** by checking hello desired types.</span></span> <span data-ttu-id="69fb2-129">**Engedélyezési kód** alapértelmezés szerint van megadva.</span><span class="sxs-lookup"><span data-stu-id="69fb2-129">**Authorization code** is specified by default.</span></span>

<span data-ttu-id="69fb2-130">Adja meg a hello **engedélyezési végpont URL-címet**.</span><span class="sxs-lookup"><span data-stu-id="69fb2-130">Enter hello **Authorization endpoint URL**.</span></span> <span data-ttu-id="69fb2-131">Az Azure Active Directory, az URL-cím lesz hasonló toohello a következő URL-cím, ahol `<client_id>` hello ügyfél-azonosító, amely azonosítja az alkalmazás OAuth 2.0 toohello server helyére.</span><span class="sxs-lookup"><span data-stu-id="69fb2-131">For Azure Active Directory, this URL will be similar toohello following URL, where `<client_id>` is replaced with hello client id that identifies your application toohello OAuth 2.0 server.</span></span>

`https://login.microsoftonline.com/<client_id>/oauth2/authorize`

<span data-ttu-id="69fb2-132">Hello **engedélyezési metódus** határozza meg, hogyan hello engedélyezési kérelmet küldött toohello OAuth 2.0-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="69fb2-132">hello **Authorization request method** specifies how hello authorization request is sent toohello OAuth 2.0 server.</span></span> <span data-ttu-id="69fb2-133">Alapértelmezés szerint **beolvasása** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="69fb2-133">By default **GET** is selected.</span></span>

<span data-ttu-id="69fb2-134">hello a következő szakaszban van, ahol hello **végponti URL-cím Token**, **ügyfél-hitelesítési módszerek**, **küldése metódus hozzáférési jogkivonat**, és **hatóköralapértelmezett** vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="69fb2-134">hello next section is where hello **Token endpoint URL**, **Client authentication methods**, **Access token sending method**, and **Default scope** are specified.</span></span>

![Új kiszolgáló][api-management-oauth2-server-3]

<span data-ttu-id="69fb2-136">Hello Azure Active Directory OAuth 2.0-kiszolgáló esetén **végponti URL-cím a Token** lesz hello a következő formátumban, ahol `<APPID>` hello formátuma a `yourapp.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="69fb2-136">For an Azure Active Directory OAuth 2.0 server, hello **Token endpoint URL** will have hello following format, where `<APPID>`  has hello format of `yourapp.onmicrosoft.com`.</span></span>

`https://login.microsoftonline.com/<APPID>/oauth2/token`

<span data-ttu-id="69fb2-137">Alapértelmezés szerint hello **ügyfél-hitelesítési módszerek** van **alapvető**, és **küldése metódus hozzáférési jogkivonat** van **Authorization fejlécet**.</span><span class="sxs-lookup"><span data-stu-id="69fb2-137">hello default setting for **Client authentication methods** is **Basic**, and  **Access token sending method** is **Authorization header**.</span></span> <span data-ttu-id="69fb2-138">Ezek az értékek vannak konfigurálva a ebben a szakaszban hello képernyő, valamint hello **hatókör alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="69fb2-138">These values are configured on this section of hello form, along with hello **Default scope**.</span></span>

<span data-ttu-id="69fb2-139">Hello **ügyfél hitelesítő adatait** szakasz hello **ügyfél-azonosító** és **ügyfélkulcs**, amely akkor kapja meg az OAuth 2.0 létrehozása és a konfigurációs folyamat hello során a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="69fb2-139">hello **Client credentials** section contains hello **Client ID** and **Client secret**, which are obtained during hello creation and configuration process of your OAuth 2.0 server.</span></span> <span data-ttu-id="69fb2-140">Egyszer hello **ügyfél-azonosító** és **ügyfélkulcs** vannak megadva, hello **redirect_uri** a hello **engedélyezési kód** jön létre.</span><span class="sxs-lookup"><span data-stu-id="69fb2-140">Once hello **Client ID** and **Client secret** are specified, hello **redirect_uri** for hello **authorization code** is generated.</span></span> <span data-ttu-id="69fb2-141">Ezt az URI használt tooconfigure hello válasz URL-CÍMEN az OAuth 2.0-kiszolgálói konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="69fb2-141">This URI is used tooconfigure hello reply URL in your OAuth 2.0 server configuration.</span></span>

![Új kiszolgáló][api-management-oauth2-server-4]

<span data-ttu-id="69fb2-143">Ha **engedélyezési kódot adjon típusok** értéke túl**erőforrás tulajdonosi jelszó**, hello **erőforrás tulajdonosa jelszavas hitelesítő adatokat** szakaszban használt toospecify azokat a hitelesítő adatok; Ellenkező esetben üresen azt.</span><span class="sxs-lookup"><span data-stu-id="69fb2-143">If **Authorization code grant types** is set too**Resource owner password**, hello **Resource owner password credentials** section is used toospecify those credentials; otherwise you can leave it blank.</span></span>

![Új kiszolgáló][api-management-oauth2-server-5]

<span data-ttu-id="69fb2-145">Hello űrlap befejeztével kattintson **mentése** toosave hello API Management OAuth 2.0 hitelesítési kiszolgáló konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="69fb2-145">Once hello form is complete, click **Save** toosave hello API Management OAuth 2.0 authorization server configuration.</span></span> <span data-ttu-id="69fb2-146">Után hello kiszolgálókonfiguráció mentésekor állíthatja be API-k toouse ebben a konfigurációban hello a következő szakaszban látható.</span><span class="sxs-lookup"><span data-stu-id="69fb2-146">Once hello server configuration is saved, you can configure APIs toouse this configuration, as shown in hello next section.</span></span>

## <span data-ttu-id="69fb2-147"><a name="step2"></a>Egy API toouse OAuth 2.0 felhasználói hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="69fb2-147"><a name="step2"> </a>Configure an API toouse OAuth 2.0 user authorization</span></span>
<span data-ttu-id="69fb2-148">Kattintson a **API-k** a hello **API Management** hello menüjének balra kattintson a kívánt hello API hello neve, kattintson **biztonsági**, és a majd hello jelölőnégyzetet **OAuth 2.0**.</span><span class="sxs-lookup"><span data-stu-id="69fb2-148">Click **APIs** from hello **API Management** menu on hello left, click hello name of hello desired API, click **Security**, and then check hello box for **OAuth 2.0**.</span></span>

![Felhasználó engedélyezése][api-management-user-authorization]

<span data-ttu-id="69fb2-150">Jelölje be hello szükséges **engedélyezési server** hello legördülő listából, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="69fb2-150">Select hello desired **Authorization server** from hello drop-down list, and click **Save**.</span></span>

![Felhasználó engedélyezése][api-management-user-authorization-save]

## <span data-ttu-id="69fb2-152"><a name="step3"></a>Hello fejlesztői portálján hello OAuth 2.0 felhasználói hitelesítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="69fb2-152"><a name="step3"> </a>Test hello OAuth 2.0 user authorization in hello Developer Portal</span></span>
<span data-ttu-id="69fb2-153">Miután beállította az OAuth 2.0 hitelesítési kiszolgáló és az API toouse konfigurálva a kiszolgálón, tesztelheti toohello fejlesztői portálján is, és az API felület meghívásakor.</span><span class="sxs-lookup"><span data-stu-id="69fb2-153">Once you have configured your OAuth 2.0 authorization server and configured your API toouse that server, you can test it by going toohello Developer Portal and calling an API.</span></span>  <span data-ttu-id="69fb2-154">Kattintson a **fejlesztői portálján** hello jobb felső menüjében található.</span><span class="sxs-lookup"><span data-stu-id="69fb2-154">Click **Developer portal** in hello top right menu.</span></span>

![Fejlesztői portál][api-management-developer-portal-menu]

<span data-ttu-id="69fb2-156">Kattintson a **API-k** hello felső menüre, majd válassza a **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="69fb2-156">Click **APIs** in hello top menu and select **Echo API**.</span></span>

![Echo API][api-management-apis-echo-api]

> [!NOTE]
> <span data-ttu-id="69fb2-158">Ha csak egy API konfigurálva van, vagy látható tooyour fiókot, majd kattintson az API-k viszi közvetlenül toohello műveletek, hogy az API-hoz.</span><span class="sxs-lookup"><span data-stu-id="69fb2-158">If you have only one API configured or visible tooyour account, then clicking APIs takes you directly toohello operations for that API.</span></span>
> 
> 

<span data-ttu-id="69fb2-159">SELECT hello **erőforrás beolvasása** műveletet, kattintson a **nyissa meg a konzolt**, majd válassza ki **engedélyezési kód** a hello legördülő.</span><span class="sxs-lookup"><span data-stu-id="69fb2-159">Select hello **GET Resource** operation, click **Open Console**, and then select **Authorization code** from hello drop-down.</span></span>

![Konzol megnyitása][api-management-open-console]

<span data-ttu-id="69fb2-161">Ha **engedélyezési kód** van jelölve, egy előugró ablak jelenik meg, amely hello bejelentkezési űrlap hello OAuth 2.0-s szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="69fb2-161">When **Authorization code** is selected, a pop-up window is displayed with hello sign-in form of hello OAuth 2.0 provider.</span></span> <span data-ttu-id="69fb2-162">Ebben a példában hello bejelentkezési űrlap Azure Active Directory által biztosított.</span><span class="sxs-lookup"><span data-stu-id="69fb2-162">In this example hello sign-in form is provided by Azure Active Directory.</span></span>

> [!NOTE]
> <span data-ttu-id="69fb2-163">Ha az előugró ablakok le van tiltva, akkor kéri tooenable hello böngésző őket.</span><span class="sxs-lookup"><span data-stu-id="69fb2-163">If you have pop-ups disabled you will be prompted tooenable them by hello browser.</span></span> <span data-ttu-id="69fb2-164">Miután engedélyezte őket, válassza ki a **engedélyezési kód** újra és hello bejelentkezési képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="69fb2-164">After you enable them, select **Authorization code** again and hello sign-in form will be displayed.</span></span>
> 
> 

![Bejelentkezés][api-management-oauth2-signin]

<span data-ttu-id="69fb2-166">Miután bejelentkezett, hello **kérelem fejlécei** kerülnek egy `Authorization : Bearer` , amely engedélyezi a hello kérelem fejléce.</span><span class="sxs-lookup"><span data-stu-id="69fb2-166">Once you have signed in, hello **Request headers** are populated with an `Authorization : Bearer` header that authorizes hello request.</span></span>

![Kérelem fejléc jogkivonat][api-management-request-header-token]

<span data-ttu-id="69fb2-168">Ezen a ponton konfigurálja a fennmaradó paraméterek hello hello szükséges értékeket, és hello kérelem küldése.</span><span class="sxs-lookup"><span data-stu-id="69fb2-168">At this point you can configure hello desired values for hello remaining parameters, and submit hello request.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="69fb2-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="69fb2-169">Next steps</span></span>
<span data-ttu-id="69fb2-170">OAuth 2.0-s és API-kezelés használatával kapcsolatos további információkért tekintse meg a video- és a hozzá tartozó hello következőt [cikk](api-management-howto-protect-backend-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="69fb2-170">For more information about using OAuth 2.0 and API Management, see hello following video and accompanying [article](api-management-howto-protect-backend-with-aad.md).</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

