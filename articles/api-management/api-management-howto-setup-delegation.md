---
title: "aaaHow toodelegate felhasználói regisztráció és a termék előfizetés"
description: "Ismerje meg, hogyan toodelegate felhasználói regisztráció és a termék előfizetés tooa harmadik fél az Azure API Management."
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 8b7ad5ee-a873-4966-a400-7e508bbbe158
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 406648db2d2f37c4dcda466294726d331cc0551b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodelegate-user-registration-and-product-subscription"></a><span data-ttu-id="9f6db-103">Hogyan toodelegate felhasználói regisztráció és a termék-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="9f6db-103">How toodelegate user registration and product subscription</span></span>
<span data-ttu-id="9f6db-104">Delegálás toouse lehetővé teszi a meglévő webhely, bejelentkezési-a/regisztrációs és az előfizetés tooproducts fejlesztői kezelése ellenezte toousing hello beépített funkcióval hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="9f6db-104">Delegation allows you toouse your existing website for handling developer sign-in/sign-up and subscription tooproducts as opposed toousing hello built-in functionality in hello developer portal.</span></span> <span data-ttu-id="9f6db-105">Ez lehetővé teszi, hogy a webhely tooown hello felhasználói adatokat, és ezeket a lépéseket hello ellenőrzés végrehajtása egy egyedi módon.</span><span class="sxs-lookup"><span data-stu-id="9f6db-105">This enables your website tooown hello user data and perform hello validation of these steps in a custom way.</span></span>

## <span data-ttu-id="9f6db-106"><a name="delegate-signin-up"></a>Delegálása fejlesztői bejelentkezési és regisztrációs</span><span class="sxs-lookup"><span data-stu-id="9f6db-106"><a name="delegate-signin-up"> </a>Delegating developer sign-in and sign-up</span></span>
<span data-ttu-id="9f6db-107">toodelegate fejlesztői bejelentkezési és regisztrációs tooyour létező webhely toocreate egy különös delegálás végpont szüksége lesz a webhelyen, eleget kell hello hello API Management developer portálról kezdeményezett kérelem belépési pontját.</span><span class="sxs-lookup"><span data-stu-id="9f6db-107">toodelegate developer sign-in and sign-up tooyour existing website you will need toocreate a special delegation endpoint on your site that acts as hello entry-point for any such request initiated from hello API Management developer portal.</span></span>

<span data-ttu-id="9f6db-108">hello végső munkafolyamat a következők:</span><span class="sxs-lookup"><span data-stu-id="9f6db-108">hello final workflow will be as follows:</span></span>

1. <span data-ttu-id="9f6db-109">A hello hello bejelentkezés vagy regisztráció hivatkozására kattint fejlesztői API Management fejlesztői portálján</span><span class="sxs-lookup"><span data-stu-id="9f6db-109">Developer clicks on hello sign-in or sign-up link at hello API Management developer portal</span></span>
2. <span data-ttu-id="9f6db-110">Böngésző átirányított toohello delegálás végpont</span><span class="sxs-lookup"><span data-stu-id="9f6db-110">Browser is redirected toohello delegation endpoint</span></span>
3. <span data-ttu-id="9f6db-111">Delegálás végpont ismét tooor megadja felhasználói felület azzal a kérdéssel felhasználói toosign a vagy előfizetési átirányítja a felhasználókat</span><span class="sxs-lookup"><span data-stu-id="9f6db-111">Delegation endpoint in return redirects tooor presents UI asking user toosign-in or sign-up</span></span>
4. <span data-ttu-id="9f6db-112">Siker hello felhasználói átirányított hátsó toohello API Management developer portálon a elkezdett</span><span class="sxs-lookup"><span data-stu-id="9f6db-112">On success, hello user is redirected back toohello API Management developer portal page they started from</span></span>

<span data-ttu-id="9f6db-113">toobegin, most első beállításról API Management tooroute kérelmek delegálás végpontot keresztül.</span><span class="sxs-lookup"><span data-stu-id="9f6db-113">toobegin, let's first set-up API Management tooroute requests via your delegation endpoint.</span></span> <span data-ttu-id="9f6db-114">Hello API Management publisher portál, kattintson a **biztonsági** majd hello **delegálás** fülre. Kattintson a hello jelölőnégyzet tooenable "Delegált & regisztráció, bejelentkezés".</span><span class="sxs-lookup"><span data-stu-id="9f6db-114">In hello API Management publisher portal, click on **Security** and then click hello **Delegation** tab. Click hello checkbox tooenable 'Delegate sign-in & sign-up'.</span></span>

![Delegálási lapra][api-management-delegation-signin-up]

* <span data-ttu-id="9f6db-116">Döntse el, mi a különleges delegálás végpont URL-címe hello fog kell, és írja be hello **delegálás végponti URL-cím** mező.</span><span class="sxs-lookup"><span data-stu-id="9f6db-116">Decide what hello URL of your special delegation endpoint will be and enter it in hello **Delegation endpoint URL** field.</span></span> 
* <span data-ttu-id="9f6db-117">Hello belül **delegálás hitelesítési kulcs** mezőbe írja be a titkos kulcs, amely egy megadott aláírás tooyou az ellenőrzési tooensure, amely hello kérelem valóban érkezik Azure API Management használt toocompute lesz.</span><span class="sxs-lookup"><span data-stu-id="9f6db-117">Within hello **Delegation authentication key** field enter a secret that will be used toocompute a signature provided tooyou for verification tooensure that hello request is indeed coming from Azure API Management.</span></span> <span data-ttu-id="9f6db-118">Kattinthat a hello **készítése** gomb toohave API felügyeleti véletlenszerű előállításához a kulcsot meg.</span><span class="sxs-lookup"><span data-stu-id="9f6db-118">You can click hello **generate** button toohave API Managemnet randomly generate a key for you.</span></span>

<span data-ttu-id="9f6db-119">Most toocreate hello **delegálás végpont**.</span><span class="sxs-lookup"><span data-stu-id="9f6db-119">Now you need toocreate hello **delegation endpoint**.</span></span> <span data-ttu-id="9f6db-120">Többféle lépéssel rendelkezik tooperform:</span><span class="sxs-lookup"><span data-stu-id="9f6db-120">It has tooperform a number of actions:</span></span>

1. <span data-ttu-id="9f6db-121">Egy kérést kap, a következő képernyő hello:</span><span class="sxs-lookup"><span data-stu-id="9f6db-121">Receive a request in hello following form:</span></span>
   
   > <span data-ttu-id="9f6db-122">*{lap URL-címe forrás} http://www.yourwebsite.com/apimdelegation?Operation=SignIn&returnUrl= & védőérték = {karakterlánc} & sig = {karakterlánc}*</span><span class="sxs-lookup"><span data-stu-id="9f6db-122">*http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl={URL of source page}&salt={string}&sig={string}*</span></span>
   > 
   > 
   
    <span data-ttu-id="9f6db-123">Lekérdezési paraméterek hello / regisztrációs, bejelentkezési esethez:</span><span class="sxs-lookup"><span data-stu-id="9f6db-123">Query parameters for hello sign-in / sign-up case:</span></span>
   
   * <span data-ttu-id="9f6db-124">**a művelet**: delegálás kérés – csak azok milyen típusú azonosítja **SignIn** ebben az esetben</span><span class="sxs-lookup"><span data-stu-id="9f6db-124">**operation**: identifies what type of delegation request it is - it can only be **SignIn** in this case</span></span>
   * <span data-ttu-id="9f6db-125">**returnUrl**: hello ahol hello felhasználói bejelentkezés vagy regisztráció hivatkozásra kattintott hello lap URL-címe</span><span class="sxs-lookup"><span data-stu-id="9f6db-125">**returnUrl**: hello URL of hello page where hello user clicked on a sign-in or sign-up link</span></span>
   * <span data-ttu-id="9f6db-126">**védőérték**: biztonsági kivonatát kiszámításához használt különleges védőérték karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9f6db-126">**salt**: a special salt string used for computing a security hash</span></span>
   * <span data-ttu-id="9f6db-127">**SIG**: egy számított biztonsági kivonatoló toobe összehasonlító tooyour saját használt számított kivonata</span><span class="sxs-lookup"><span data-stu-id="9f6db-127">**sig**: a computed security hash toobe used for comparison tooyour own computed hash</span></span>
2. <span data-ttu-id="9f6db-128">Győződjön meg arról, hogy hello kérelem érkezik Azure API Management (nem kötelező, de erősen ajánlott a biztonság)</span><span class="sxs-lookup"><span data-stu-id="9f6db-128">Verify that hello request is coming from Azure API Management (optional, but highly recommended for security)</span></span>
   
   * <span data-ttu-id="9f6db-129">Egy karakterlánc alapján hello HMAC-SHA512 kivonatának számítási **returnUrl** és **védőérték** lekérdezési paramétert ([alábbi példakód]):</span><span class="sxs-lookup"><span data-stu-id="9f6db-129">Compute an HMAC-SHA512 hash of a string based on hello **returnUrl** and **salt** query parameters ([example code provided below]):</span></span>
     
     > <span data-ttu-id="9f6db-130">HMAC (**védőérték** + "\n" + **returnUrl**)</span><span class="sxs-lookup"><span data-stu-id="9f6db-130">HMAC(**salt** + '\n' + **returnUrl**)</span></span>
     > 
     > 
   * <span data-ttu-id="9f6db-131">Hasonlítsa össze a hello fent számított toohello kivonatértéke hello **sig** lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="9f6db-131">Compare hello above-computed hash toohello value of hello **sig** query parameter.</span></span> <span data-ttu-id="9f6db-132">Hello két kivonatok megegyeznek, ha áthelyezni a következő lépés toohello, ellenkező esetben a hello kérelem elutasítása.</span><span class="sxs-lookup"><span data-stu-id="9f6db-132">If hello two hashes match, move on toohello next step, otherwise deny hello request.</span></span>
3. <span data-ttu-id="9f6db-133">Győződjön meg arról, hogy fordulnak elő a bejelentkezési/sign up kérelmet: hello **művelet** lekérdezésparaméter lesz beállítva, túl "**SignIn**".</span><span class="sxs-lookup"><span data-stu-id="9f6db-133">Verify that you are receiving a request for sign-in/sign-up: hello **operation** query parameter will be set too"**SignIn**".</span></span>
4. <span data-ttu-id="9f6db-134">Jelen hello felhasználói vagy előfizetési toosign a felhasználói felületen</span><span class="sxs-lookup"><span data-stu-id="9f6db-134">Present hello user with UI toosign-in or sign-up</span></span>
5. <span data-ttu-id="9f6db-135">Ha hello felhasználó regisztrált toocreate megfelelő fiókkal rendelkező számukra az API Management.</span><span class="sxs-lookup"><span data-stu-id="9f6db-135">If hello user is signing-up you have toocreate a corresponding account for them in API Management.</span></span> <span data-ttu-id="9f6db-136">[Hozzon létre egy felhasználót] a hello API Management REST API-t.</span><span class="sxs-lookup"><span data-stu-id="9f6db-136">[Create a user] with hello API Management REST API.</span></span> <span data-ttu-id="9f6db-137">Ha így arról, hogy ugyanaz a felhasználókhoz tartozó tárolóban van, vagy akkor is nyomon követésére szolgáló tooan azonosító beállítva hello felhasználói azonosító toohello.</span><span class="sxs-lookup"><span data-stu-id="9f6db-137">When doing so, ensure that you set hello user ID toohello same it is in your user store or tooan ID that you can keep track of.</span></span>
6. <span data-ttu-id="9f6db-138">Ha a hello felhasználó sikeresen hitelesített:</span><span class="sxs-lookup"><span data-stu-id="9f6db-138">When hello user is successfully authenticated:</span></span>
   
   * <span data-ttu-id="9f6db-139">[egyszeri bejelentkezéses (SSO) jogkivonatot kérni] hello API Management REST API-n keresztül</span><span class="sxs-lookup"><span data-stu-id="9f6db-139">[request a single-sign-on (SSO) token] via hello API Management REST API</span></span>
   * <span data-ttu-id="9f6db-140">hozzáfűzése egy returnUrl lekérdezési paraméter toohello egyszeri bejelentkezési URL-címet, a fenti hello API-hívás érkezett:</span><span class="sxs-lookup"><span data-stu-id="9f6db-140">append a returnUrl query parameter toohello SSO URL you have received from hello API call above:</span></span>
     
     > <span data-ttu-id="9f6db-141">pl. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url</span><span class="sxs-lookup"><span data-stu-id="9f6db-141">e.g. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url</span></span> 
     > 
     > 
   * <span data-ttu-id="9f6db-142">az átirányítási hello felhasználói toohello fenti előállított URL-címe</span><span class="sxs-lookup"><span data-stu-id="9f6db-142">redirect hello user toohello above produced URL</span></span>

<span data-ttu-id="9f6db-143">A hozzáadása toohello **SignIn** művelet is végrehajthat fiókkezelés hello előző lépések és hello a következő műveletek egyikével.</span><span class="sxs-lookup"><span data-stu-id="9f6db-143">In addition toohello **SignIn** operation, you can also perform account management by following hello previous steps and using one of hello following operations.</span></span>

* <span data-ttu-id="9f6db-144">**A ChangePassword**</span><span class="sxs-lookup"><span data-stu-id="9f6db-144">**ChangePassword**</span></span>
* <span data-ttu-id="9f6db-145">**ChangeProfile**</span><span class="sxs-lookup"><span data-stu-id="9f6db-145">**ChangeProfile**</span></span>
* <span data-ttu-id="9f6db-146">**CloseAccount**</span><span class="sxs-lookup"><span data-stu-id="9f6db-146">**CloseAccount**</span></span>

<span data-ttu-id="9f6db-147">A következő lekérdezési paraméterek a fiókkezelési műveletekhez hello kell átadni.</span><span class="sxs-lookup"><span data-stu-id="9f6db-147">You must pass hello following query parameters for account management operations.</span></span>

* <span data-ttu-id="9f6db-148">**a művelet**: azonosítja (a ChangePassword, ChangeProfile vagy CloseAccount) delegálás kérelem típusa</span><span class="sxs-lookup"><span data-stu-id="9f6db-148">**operation**: identifies what type of delegation request it is (ChangePassword, ChangeProfile, or CloseAccount)</span></span>
* <span data-ttu-id="9f6db-149">**userId**: hello fiók toomanage hello felhasználó azonosítója</span><span class="sxs-lookup"><span data-stu-id="9f6db-149">**userId**: hello user id of hello account toomanage</span></span>
* <span data-ttu-id="9f6db-150">**védőérték**: biztonsági kivonatát kiszámításához használt különleges védőérték karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9f6db-150">**salt**: a special salt string used for computing a security hash</span></span>
* <span data-ttu-id="9f6db-151">**SIG**: egy számított biztonsági kivonatoló toobe összehasonlító tooyour saját használt számított kivonata</span><span class="sxs-lookup"><span data-stu-id="9f6db-151">**sig**: a computed security hash toobe used for comparison tooyour own computed hash</span></span>

## <span data-ttu-id="9f6db-152"><a name="delegate-product-subscription"></a>Termék-előfizetéshez delegálása</span><span class="sxs-lookup"><span data-stu-id="9f6db-152"><a name="delegate-product-subscription"> </a>Delegating product subscription</span></span>
<span data-ttu-id="9f6db-153">Termék-előfizetéshez delegálása működik hasonlóképpen toodelegating felhasználói bejelentkezés/felfelé.</span><span class="sxs-lookup"><span data-stu-id="9f6db-153">Delegating product subscription works similarly toodelegating user sign-in/-up.</span></span> <span data-ttu-id="9f6db-154">hello végső munkafolyamata a következőképpen nézne ki:</span><span class="sxs-lookup"><span data-stu-id="9f6db-154">hello final workflow would be as follows:</span></span>

1. <span data-ttu-id="9f6db-155">Fejlesztői termék kiválasztása hello API Management developer portálon, és a hello előfizetés gombra kattint</span><span class="sxs-lookup"><span data-stu-id="9f6db-155">Developer selects a product in hello API Management developer portal and clicks on hello Subscribe button</span></span>
2. <span data-ttu-id="9f6db-156">Böngésző átirányított toohello delegálás végpont</span><span class="sxs-lookup"><span data-stu-id="9f6db-156">Browser is redirected toohello delegation endpoint</span></span>
3. <span data-ttu-id="9f6db-157">Delegálás endpoint szükséges termék előfizetés lépéseket végzi el – ez tooyou működik, és átirányítása tooanother lap toorequest számlázási adatokat, további kérdések vagy hello információk tárolásának és felhasználói beavatkozást nem igénylő járhat</span><span class="sxs-lookup"><span data-stu-id="9f6db-157">Delegation endpoint performs required product subscription steps - this is up tooyou and may entail redirecting tooanother page toorequest billing information, asking additional questions, or simply storing hello information and not requiring any user action</span></span>

<span data-ttu-id="9f6db-158">tooenable hello funkciói hello **delegálás** kattintson **termék-előfizetéshez delegálása**.</span><span class="sxs-lookup"><span data-stu-id="9f6db-158">tooenable hello functionality, on hello **Delegation** page click **Delegate product subscription**.</span></span>

<span data-ttu-id="9f6db-159">Gondoskodnia kell hello delegálás végpont hello a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="9f6db-159">Then ensure hello delegation endpoint performs hello following actions:</span></span>

1. <span data-ttu-id="9f6db-160">Egy kérést kap, a következő képernyő hello:</span><span class="sxs-lookup"><span data-stu-id="9f6db-160">Receive a request in hello following form:</span></span>
   
   > <span data-ttu-id="9f6db-161">*{művelet} http://www.yourwebsite.com/apimdelegation?Operation= & productId = {termék toosubscribe való} & userId = {a felhasználó kérést} & védőérték = {karakterlánc} & sig = {karakterlánc}*</span><span class="sxs-lookup"><span data-stu-id="9f6db-161">*http://www.yourwebsite.com/apimdelegation?operation={operation}&productId={product toosubscribe to}&userId={user making request}&salt={string}&sig={string}*</span></span>
   > 
   > 
   
    <span data-ttu-id="9f6db-162">Lekérdezési paraméterek hello termék előfizetés esethez:</span><span class="sxs-lookup"><span data-stu-id="9f6db-162">Query parameters for hello product subscription case:</span></span>
   
   * <span data-ttu-id="9f6db-163">**a művelet**: azonosítja a delegálás kérelem milyen típusú legyen.</span><span class="sxs-lookup"><span data-stu-id="9f6db-163">**operation**: identifies what type of delegation request it is.</span></span> <span data-ttu-id="9f6db-164">A termék-előfizetéshez tartozó kérelmek hello érvényes lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="9f6db-164">For product subscription requests hello valid options are:</span></span>
     * <span data-ttu-id="9f6db-165">"Előfizetés": a kérés toosubscribe hello felhasználói tooa megadott termék megadott azonosító (lásd alább)</span><span class="sxs-lookup"><span data-stu-id="9f6db-165">"Subscribe": a request toosubscribe hello user tooa given product with provided ID (see below)</span></span>
     * <span data-ttu-id="9f6db-166">"Leiratkozhat": a kérés toounsubscribe a terméket felhasználó</span><span class="sxs-lookup"><span data-stu-id="9f6db-166">"Unsubscribe": a request toounsubscribe a user from a product</span></span>
     * <span data-ttu-id="9f6db-167">"Megújítása": egy requst toorenew előfizetést (pl. is lejár)</span><span class="sxs-lookup"><span data-stu-id="9f6db-167">"Renew": a requst toorenew a subscription (e.g. that may be expiring)</span></span>
   * <span data-ttu-id="9f6db-168">**productId**: hello hello termék hello felhasználói Azonosítóját a toosubscribe kért</span><span class="sxs-lookup"><span data-stu-id="9f6db-168">**productId**: hello ID of hello product hello user requested toosubscribe to</span></span>
   * <span data-ttu-id="9f6db-169">**userId**: hello hello felhasználó, akinek hello kérelem azonosítója</span><span class="sxs-lookup"><span data-stu-id="9f6db-169">**userId**: hello ID of hello user for whom hello request is made</span></span>
   * <span data-ttu-id="9f6db-170">**védőérték**: biztonsági kivonatát kiszámításához használt különleges védőérték karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9f6db-170">**salt**: a special salt string used for computing a security hash</span></span>
   * <span data-ttu-id="9f6db-171">**SIG**: egy számított biztonsági kivonatoló toobe összehasonlító tooyour saját használt számított kivonata</span><span class="sxs-lookup"><span data-stu-id="9f6db-171">**sig**: a computed security hash toobe used for comparison tooyour own computed hash</span></span>
2. <span data-ttu-id="9f6db-172">Győződjön meg arról, hogy hello kérelem érkezik Azure API Management (nem kötelező, de erősen ajánlott a biztonság)</span><span class="sxs-lookup"><span data-stu-id="9f6db-172">Verify that hello request is coming from Azure API Management (optional, but highly recommended for security)</span></span>
   
   * <span data-ttu-id="9f6db-173">Egy karakterlánc alapján hello HMAC-SHA512 számítási **productId**, **userId** és **védőérték** lekérdezési paramétert:</span><span class="sxs-lookup"><span data-stu-id="9f6db-173">Compute an HMAC-SHA512 of a string based on hello **productId**, **userId** and **salt** query parameters:</span></span>
     
     > <span data-ttu-id="9f6db-174">HMAC (**védőérték** + "\n" + **productId** + "\n" + **userId**)</span><span class="sxs-lookup"><span data-stu-id="9f6db-174">HMAC(**salt** + '\n' + **productId** + '\n' + **userId**)</span></span>
     > 
     > 
   * <span data-ttu-id="9f6db-175">Hasonlítsa össze a hello fent számított toohello kivonatértéke hello **sig** lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="9f6db-175">Compare hello above-computed hash toohello value of hello **sig** query parameter.</span></span> <span data-ttu-id="9f6db-176">Hello két kivonatok megegyeznek, ha áthelyezni a következő lépés toohello, ellenkező esetben a hello kérelem elutasítása.</span><span class="sxs-lookup"><span data-stu-id="9f6db-176">If hello two hashes match, move on toohello next step, otherwise deny hello request.</span></span>
3. <span data-ttu-id="9f6db-177">A termék feldolgozási hello alapján a kért művelet végrehajtásához **művelet** – pl. számlázást, a további kérdésekre, stb.</span><span class="sxs-lookup"><span data-stu-id="9f6db-177">Perform any product subscription processing based on hello type of operation requested in **operation** - e.g. billing, further questions, etc.</span></span>
4. <span data-ttu-id="9f6db-178">Sikeresen előfizetés hello felhasználói toohello termék a oldalon, az előfizetés által hello felhasználói toohello API-felügyeleti termék [termék-előfizetéshez tartozó REST API-t hívó hello].</span><span class="sxs-lookup"><span data-stu-id="9f6db-178">On successfully subscribing hello user toohello product on your side, subscribe hello user toohello API Management product by [calling hello REST API for product subscription].</span></span>

## <span data-ttu-id="9f6db-179"><a name="delegate-example-code"></a> Példakód</span><span class="sxs-lookup"><span data-stu-id="9f6db-179"><a name="delegate-example-code"> </a> Example Code</span></span>
<span data-ttu-id="9f6db-180">Ezek az minták megjelenítése hogyan code tootake hello *delegálás érvényesítési kulcs*, melynek értéke hello publisher portál hello delegálás képernyőjén, egy HMAC, amely majd toocreate használt toovalidate hello aláírás igazolására hello hello érvényességét átadott returnUrl.</span><span class="sxs-lookup"><span data-stu-id="9f6db-180">These code samples show how tootake hello *delegation validation key*, which is set in hello Delegation screen of hello publisher portal, toocreate a HMAC which is then used toovalidate hello signature, proving hello validity of hello passed returnUrl.</span></span> <span data-ttu-id="9f6db-181">hello ugyanazt a kódot működik hello productId és kis mértékben módosított rendelkező felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="9f6db-181">hello same code works for hello productId and userId with slight modification.</span></span>

<span data-ttu-id="9f6db-182">**C# kóddal toogenerate kivonatának returnUrl**</span><span class="sxs-lookup"><span data-stu-id="9f6db-182">**C# code toogenerate hash of returnUrl**</span></span>

```c#
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature toosig query parameter
}
```

<span data-ttu-id="9f6db-183">**NodeJS code returnUrl toogenerate kivonata**</span><span class="sxs-lookup"><span data-stu-id="9f6db-183">**NodeJS code toogenerate hash of returnUrl**</span></span>

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature toosig query parameter

var signature = digest.toString('base64');
```

## <a name="next-steps"></a><span data-ttu-id="9f6db-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9f6db-184">Next steps</span></span>
<span data-ttu-id="9f6db-185">Delegálás további információkért tekintse meg a következő videó hello.</span><span class="sxs-lookup"><span data-stu-id="9f6db-185">For more information on delegation, see hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[egyszeri bejelentkezéses (SSO) jogkivonatot kérni]: http://go.microsoft.com/fwlink/?LinkId=507409
[felhasználó létrehozása]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[termék-előfizetéshez tartozó REST API-t hívó hello]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[alábbi példakód]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
