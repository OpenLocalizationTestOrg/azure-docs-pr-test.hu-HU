---
title: "Hogyan kell delegálni a felhasználói regisztráció és a termék-előfizetéshez"
description: "Tudnivalók a felhasználói regisztráció és a termék előfizetés harmadik félnek az Azure API Management delegálása."
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
ms.openlocfilehash: 2637ab6405f2d4ea1da84981295a144874dfa4f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-delegate-user-registration-and-product-subscription"></a><span data-ttu-id="7678e-103">Hogyan kell delegálni a felhasználói regisztráció és a termék-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="7678e-103">How to delegate user registration and product subscription</span></span>
<span data-ttu-id="7678e-104">Delegálás lehetővé teszi a meglévő webhely használatát fejlesztői bejelentkezési-a/regisztrációs és termékek a fejlesztői portálra beépített funkcióval szemben-előfizetés kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="7678e-104">Delegation allows you to use your existing website for handling developer sign-in/sign-up and subscription to products as opposed to using the built-in functionality in the developer portal.</span></span> <span data-ttu-id="7678e-105">Ez lehetővé teszi, hogy a webhely tulajdonosai a felhasználó adatait, és hajtsa végre az alábbi lépéseket az érvényesítés egy egyedi módon.</span><span class="sxs-lookup"><span data-stu-id="7678e-105">This enables your website to own the user data and perform the validation of these steps in a custom way.</span></span>

## <span data-ttu-id="7678e-106"><a name="delegate-signin-up"></a>Delegálása fejlesztői bejelentkezési és regisztrációs</span><span class="sxs-lookup"><span data-stu-id="7678e-106"><a name="delegate-signin-up"> </a>Delegating developer sign-in and sign-up</span></span>
<span data-ttu-id="7678e-107">Fejlesztői delegálása bejelentkezési és regisztrációs meglévő webhelyéhez szüksége lesz különleges delegálás-végpont létrehozása a webhely, amely különbséglemezként funkcionál az API Management developer portálról kezdeményezett kérelem belépési pontját.</span><span class="sxs-lookup"><span data-stu-id="7678e-107">To delegate developer sign-in and sign-up to your existing website you will need to create a special delegation endpoint on your site that acts as the entry-point for any such request initiated from the API Management developer portal.</span></span>

<span data-ttu-id="7678e-108">A végső munkafolyamat a következők:</span><span class="sxs-lookup"><span data-stu-id="7678e-108">The final workflow will be as follows:</span></span>

1. <span data-ttu-id="7678e-109">Fejlesztői gombra kell kattintania a bejelentkezési és regisztrációs hivatkozásra, az API Management fejlesztői portálján</span><span class="sxs-lookup"><span data-stu-id="7678e-109">Developer clicks on the sign-in or sign-up link at the API Management developer portal</span></span>
2. <span data-ttu-id="7678e-110">Böngésző átirányítja a delegálás végpont</span><span class="sxs-lookup"><span data-stu-id="7678e-110">Browser is redirected to the delegation endpoint</span></span>
3. <span data-ttu-id="7678e-111">Delegálás végpont ismét átirányítja a felhasználókat a, vagy megadja a felhasználói felületen kéri a felhasználói bejelentkezés vagy regisztráció</span><span class="sxs-lookup"><span data-stu-id="7678e-111">Delegation endpoint in return redirects to or presents UI asking user to sign-in or sign-up</span></span>
4. <span data-ttu-id="7678e-112">A sikeres, a felhasználót a rendszer átirányítja az API Management developer portálon oldalra azok indította el</span><span class="sxs-lookup"><span data-stu-id="7678e-112">On success, the user is redirected back to the API Management developer portal page they started from</span></span>

<span data-ttu-id="7678e-113">Első lépésként most irányíthatja az első telepítés API Management kérelmek delegálás végpontot keresztül.</span><span class="sxs-lookup"><span data-stu-id="7678e-113">To begin, let's first set-up API Management to route requests via your delegation endpoint.</span></span> <span data-ttu-id="7678e-114">Az API Management publisher portálon, kattintson a **biztonsági** , majd a **delegálás** fülre.</span><span class="sxs-lookup"><span data-stu-id="7678e-114">In the API Management publisher portal, click on **Security** and then click the **Delegation** tab.</span></span> <span data-ttu-id="7678e-115">Kattintson a "Delegált bejelentkezési és regisztrációs" engedélyezése jelölőnégyzet.</span><span class="sxs-lookup"><span data-stu-id="7678e-115">Click the checkbox to enable 'Delegate sign-in & sign-up'.</span></span>

![Delegálási lapra][api-management-delegation-signin-up]

* <span data-ttu-id="7678e-117">Döntse el, mi a különleges delegálás végpont URL-CÍMÉT a rendszer, majd adja meg a legyen a **delegálás végponti URL-cím** mező.</span><span class="sxs-lookup"><span data-stu-id="7678e-117">Decide what the URL of your special delegation endpoint will be and enter it in the **Delegation endpoint URL** field.</span></span> 
* <span data-ttu-id="7678e-118">Belül a **delegálás hitelesítési kulcs** mezőbe írja be a aláírás-ellenőrzés győződjön meg arról, hogy a kérelem valóban származik Azure API Management az Ön számára biztosított kiszámításához használt titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="7678e-118">Within the **Delegation authentication key** field enter a secret that will be used to compute a signature provided to you for verification to ensure that the request is indeed coming from Azure API Management.</span></span> <span data-ttu-id="7678e-119">Kattintson a **készítése** véletlenszerű előállításához a kulcs akkor API felügyeleti gombra.</span><span class="sxs-lookup"><span data-stu-id="7678e-119">You can click the **generate** button to have API Managemnet randomly generate a key for you.</span></span>

<span data-ttu-id="7678e-120">Most kell létrehoznia a **delegálás végpont**.</span><span class="sxs-lookup"><span data-stu-id="7678e-120">Now you need to create the **delegation endpoint**.</span></span> <span data-ttu-id="7678e-121">Van több műveletet végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="7678e-121">It has to perform a number of actions:</span></span>

1. <span data-ttu-id="7678e-122">A kérés fogadásához a következő formában:</span><span class="sxs-lookup"><span data-stu-id="7678e-122">Receive a request in the following form:</span></span>
   
   > <span data-ttu-id="7678e-123">*{lap URL-címe forrás} http://www.yourwebsite.com/apimdelegation?Operation=SignIn&returnUrl= & védőérték = {karakterlánc} & sig = {karakterlánc}*</span><span class="sxs-lookup"><span data-stu-id="7678e-123">*http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl={URL of source page}&salt={string}&sig={string}*</span></span>
   > 
   > 
   
    <span data-ttu-id="7678e-124">Lekérdezési paraméterek a bejelentkezési / előfizetési esethez:</span><span class="sxs-lookup"><span data-stu-id="7678e-124">Query parameters for the sign-in / sign-up case:</span></span>
   
   * <span data-ttu-id="7678e-125">**a művelet**: delegálás kérés – csak azok milyen típusú azonosítja **SignIn** ebben az esetben</span><span class="sxs-lookup"><span data-stu-id="7678e-125">**operation**: identifies what type of delegation request it is - it can only be **SignIn** in this case</span></span>
   * <span data-ttu-id="7678e-126">**returnUrl**: a lap, ahol a felhasználó egy bejelentkezési vagy előfizetési hivatkozásra kattint az URL-cím</span><span class="sxs-lookup"><span data-stu-id="7678e-126">**returnUrl**: the URL of the page where the user clicked on a sign-in or sign-up link</span></span>
   * <span data-ttu-id="7678e-127">**védőérték**: biztonsági kivonatát kiszámításához használt különleges védőérték karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7678e-127">**salt**: a special salt string used for computing a security hash</span></span>
   * <span data-ttu-id="7678e-128">**SIG**: összehasonlítása a saját használandó számított biztonsági kivonatát számított kivonata</span><span class="sxs-lookup"><span data-stu-id="7678e-128">**sig**: a computed security hash to be used for comparison to your own computed hash</span></span>
2. <span data-ttu-id="7678e-129">Győződjön meg arról, hogy a kérelem érkezik Azure API Management (nem kötelező, de erősen ajánlott a biztonság)</span><span class="sxs-lookup"><span data-stu-id="7678e-129">Verify that the request is coming from Azure API Management (optional, but highly recommended for security)</span></span>
   
   * <span data-ttu-id="7678e-130">Egy olyan karakterlánc alapján HMAC-SHA512 kivonatának számítási a **returnUrl** és **védőérték** lekérdezési paramétert ([alábbi példakód]):</span><span class="sxs-lookup"><span data-stu-id="7678e-130">Compute an HMAC-SHA512 hash of a string based on the **returnUrl** and **salt** query parameters ([example code provided below]):</span></span>
     
     > <span data-ttu-id="7678e-131">HMAC (**védőérték** + "\n" + **returnUrl**)</span><span class="sxs-lookup"><span data-stu-id="7678e-131">HMAC(**salt** + '\n' + **returnUrl**)</span></span>
     > 
     > 
   * <span data-ttu-id="7678e-132">A fenti számított kivonat értékének összehasonlítása a **sig** lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="7678e-132">Compare the above-computed hash to the value of the **sig** query parameter.</span></span> <span data-ttu-id="7678e-133">Ha a két kivonatok megegyeznek, lépjen tovább a következő lépés, egyébként visszautasítja a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="7678e-133">If the two hashes match, move on to the next step, otherwise deny the request.</span></span>
3. <span data-ttu-id="7678e-134">Győződjön meg arról, hogy fordulnak elő a bejelentkezési/sign up kérelmet: a **művelet** úgy lesz beállítva, lekérdezési paraméter "**SignIn**".</span><span class="sxs-lookup"><span data-stu-id="7678e-134">Verify that you are receiving a request for sign-in/sign-up: the **operation** query parameter will be set to "**SignIn**".</span></span>
4. <span data-ttu-id="7678e-135">A felhasználók bemutatásához bejelentkezés vagy regisztráció a felhasználói felületen</span><span class="sxs-lookup"><span data-stu-id="7678e-135">Present the user with UI to sign-in or sign-up</span></span>
5. <span data-ttu-id="7678e-136">Ha a felhasználó regisztrált kell létrehoznia a megfelelő fiókkal számukra az API Management.</span><span class="sxs-lookup"><span data-stu-id="7678e-136">If the user is signing-up you have to create a corresponding account for them in API Management.</span></span> <span data-ttu-id="7678e-137">[Hozzon létre egy felhasználót] API Management REST API-val.</span><span class="sxs-lookup"><span data-stu-id="7678e-137">[Create a user] with the API Management REST API.</span></span> <span data-ttu-id="7678e-138">Annak során, győződjön meg arról, hogy állítsa a felhasználói Azonosítóját, ugyanaz a felhasználókhoz tartozó tárolóban van, vagy egy azonosító, akkor is nyomon követésére szolgáló.</span><span class="sxs-lookup"><span data-stu-id="7678e-138">When doing so, ensure that you set the user ID to the same it is in your user store or to an ID that you can keep track of.</span></span>
6. <span data-ttu-id="7678e-139">Amikor a rendszer sikeresen hitelesíteni a felhasználót:</span><span class="sxs-lookup"><span data-stu-id="7678e-139">When the user is successfully authenticated:</span></span>
   
   * <span data-ttu-id="7678e-140">[egyszeri bejelentkezéses (SSO) jogkivonatot kérni] az API Management REST API-n keresztül</span><span class="sxs-lookup"><span data-stu-id="7678e-140">[request a single-sign-on (SSO) token] via the API Management REST API</span></span>
   * <span data-ttu-id="7678e-141">egy returnUrl lekérdezési paraméter hozzáfűzése fent API-hívás kapott egyszeri bejelentkezési URL-címe:</span><span class="sxs-lookup"><span data-stu-id="7678e-141">append a returnUrl query parameter to the SSO URL you have received from the API call above:</span></span>
     
     > <span data-ttu-id="7678e-142">pl. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url</span><span class="sxs-lookup"><span data-stu-id="7678e-142">e.g. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url</span></span> 
     > 
     > 
   * <span data-ttu-id="7678e-143">a fenti létrehozott URL-címet átirányítja a felhasználót</span><span class="sxs-lookup"><span data-stu-id="7678e-143">redirect the user to the above produced URL</span></span>

<span data-ttu-id="7678e-144">Kívül a **SignIn** művelet is végrehajthat felhasználóifiók-kezelés az előző lépések és a következő műveletek egyikével.</span><span class="sxs-lookup"><span data-stu-id="7678e-144">In addition to the **SignIn** operation, you can also perform account management by following the previous steps and using one of the following operations.</span></span>

* <span data-ttu-id="7678e-145">**A ChangePassword**</span><span class="sxs-lookup"><span data-stu-id="7678e-145">**ChangePassword**</span></span>
* <span data-ttu-id="7678e-146">**ChangeProfile**</span><span class="sxs-lookup"><span data-stu-id="7678e-146">**ChangeProfile**</span></span>
* <span data-ttu-id="7678e-147">**CloseAccount**</span><span class="sxs-lookup"><span data-stu-id="7678e-147">**CloseAccount**</span></span>

<span data-ttu-id="7678e-148">Át kell adnia a következő lekérdezés-paraméterek a fiókkezelési műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="7678e-148">You must pass the following query parameters for account management operations.</span></span>

* <span data-ttu-id="7678e-149">**a művelet**: azonosítja (a ChangePassword, ChangeProfile vagy CloseAccount) delegálás kérelem típusa</span><span class="sxs-lookup"><span data-stu-id="7678e-149">**operation**: identifies what type of delegation request it is (ChangePassword, ChangeProfile, or CloseAccount)</span></span>
* <span data-ttu-id="7678e-150">**userId**: a fiók kezeléséhez a felhasználói azonosító</span><span class="sxs-lookup"><span data-stu-id="7678e-150">**userId**: the user id of the account to manage</span></span>
* <span data-ttu-id="7678e-151">**védőérték**: biztonsági kivonatát kiszámításához használt különleges védőérték karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7678e-151">**salt**: a special salt string used for computing a security hash</span></span>
* <span data-ttu-id="7678e-152">**SIG**: összehasonlítása a saját használandó számított biztonsági kivonatát számított kivonata</span><span class="sxs-lookup"><span data-stu-id="7678e-152">**sig**: a computed security hash to be used for comparison to your own computed hash</span></span>

## <span data-ttu-id="7678e-153"><a name="delegate-product-subscription"></a>Termék-előfizetéshez delegálása</span><span class="sxs-lookup"><span data-stu-id="7678e-153"><a name="delegate-product-subscription"> </a>Delegating product subscription</span></span>
<span data-ttu-id="7678e-154">Termék-előfizetéshez delegálása működik, hasonlóképpen delegálása a felhasználói bejelentkezés/felfelé.</span><span class="sxs-lookup"><span data-stu-id="7678e-154">Delegating product subscription works similarly to delegating user sign-in/-up.</span></span> <span data-ttu-id="7678e-155">A végső munkafolyamata a következőképpen nézne ki:</span><span class="sxs-lookup"><span data-stu-id="7678e-155">The final workflow would be as follows:</span></span>

1. <span data-ttu-id="7678e-156">Fejlesztői termék kiválasztása az API Management developer portálon, és az előfizetés gombra kattint</span><span class="sxs-lookup"><span data-stu-id="7678e-156">Developer selects a product in the API Management developer portal and clicks on the Subscribe button</span></span>
2. <span data-ttu-id="7678e-157">Böngésző átirányítja a delegálás végpont</span><span class="sxs-lookup"><span data-stu-id="7678e-157">Browser is redirected to the delegation endpoint</span></span>
3. <span data-ttu-id="7678e-158">Delegálás endpoint szükséges termék előfizetés lépéseket végzi el – ez Öntől, és kérje a számlázási adatokat, további kérdések vagy az információk tárolásának és felhasználói beavatkozást nem igénylő másik lapra irányít át járhat</span><span class="sxs-lookup"><span data-stu-id="7678e-158">Delegation endpoint performs required product subscription steps - this is up to you and may entail redirecting to another page to request billing information, asking additional questions, or simply storing the information and not requiring any user action</span></span>

<span data-ttu-id="7678e-159">A funkció engedélyezéséhez kattintson a **delegálás** kattintson **delegálja a termék-előfizetéshez**.</span><span class="sxs-lookup"><span data-stu-id="7678e-159">To enable the functionality, on the **Delegation** page click **Delegate product subscription**.</span></span>

<span data-ttu-id="7678e-160">Gondoskodnia kell a delegálás végpont a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="7678e-160">Then ensure the delegation endpoint performs the following actions:</span></span>

1. <span data-ttu-id="7678e-161">A kérés fogadásához a következő formában:</span><span class="sxs-lookup"><span data-stu-id="7678e-161">Receive a request in the following form:</span></span>
   
   > <span data-ttu-id="7678e-162">*{művelet} http://www.yourwebsite.com/apimdelegation?Operation= & productId = {termék előfizetni} & userId = {a felhasználó kérést} & védőérték = {karakterlánc} & sig = {karakterlánc}*</span><span class="sxs-lookup"><span data-stu-id="7678e-162">*http://www.yourwebsite.com/apimdelegation?operation={operation}&productId={product to subscribe to}&userId={user making request}&salt={string}&sig={string}*</span></span>
   > 
   > 
   
    <span data-ttu-id="7678e-163">Lekérdezés-paraméterek a termék-előfizetéshez eset:</span><span class="sxs-lookup"><span data-stu-id="7678e-163">Query parameters for the product subscription case:</span></span>
   
   * <span data-ttu-id="7678e-164">**a művelet**: azonosítja a delegálás kérelem milyen típusú legyen.</span><span class="sxs-lookup"><span data-stu-id="7678e-164">**operation**: identifies what type of delegation request it is.</span></span> <span data-ttu-id="7678e-165">A termék-előfizetéshez tartozó kérelmek érvényes lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="7678e-165">For product subscription requests the valid options are:</span></span>
     * <span data-ttu-id="7678e-166">"Előfizetés": egy kérelem a felhasználó számára az adott termék előfizetni a megadott azonosító (lásd alább)</span><span class="sxs-lookup"><span data-stu-id="7678e-166">"Subscribe": a request to subscribe the user to a given product with provided ID (see below)</span></span>
     * <span data-ttu-id="7678e-167">"Leiratkozhat": egy kérelem egy felhasználó egy terméket mondhatja</span><span class="sxs-lookup"><span data-stu-id="7678e-167">"Unsubscribe": a request to unsubscribe a user from a product</span></span>
     * <span data-ttu-id="7678e-168">"Megújítása": egy requst (pl., előfordulhat, hogy lejáró) előfizetés megújításához</span><span class="sxs-lookup"><span data-stu-id="7678e-168">"Renew": a requst to renew a subscription (e.g. that may be expiring)</span></span>
   * <span data-ttu-id="7678e-169">**productId**: a felhasználó azt kérte előfizetni a termék Azonosítóját</span><span class="sxs-lookup"><span data-stu-id="7678e-169">**productId**: the ID of the product the user requested to subscribe to</span></span>
   * <span data-ttu-id="7678e-170">**userId**: a felhasználó, akinek a kérelem azonosítója</span><span class="sxs-lookup"><span data-stu-id="7678e-170">**userId**: the ID of the user for whom the request is made</span></span>
   * <span data-ttu-id="7678e-171">**védőérték**: biztonsági kivonatát kiszámításához használt különleges védőérték karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7678e-171">**salt**: a special salt string used for computing a security hash</span></span>
   * <span data-ttu-id="7678e-172">**SIG**: összehasonlítása a saját használandó számított biztonsági kivonatát számított kivonata</span><span class="sxs-lookup"><span data-stu-id="7678e-172">**sig**: a computed security hash to be used for comparison to your own computed hash</span></span>
2. <span data-ttu-id="7678e-173">Győződjön meg arról, hogy a kérelem érkezik Azure API Management (nem kötelező, de erősen ajánlott a biztonság)</span><span class="sxs-lookup"><span data-stu-id="7678e-173">Verify that the request is coming from Azure API Management (optional, but highly recommended for security)</span></span>
   
   * <span data-ttu-id="7678e-174">Egy olyan karakterlánc alapján HMAC-SHA512 számítási a **productId**, **userId** és **védőérték** lekérdezési paramétert:</span><span class="sxs-lookup"><span data-stu-id="7678e-174">Compute an HMAC-SHA512 of a string based on the **productId**, **userId** and **salt** query parameters:</span></span>
     
     > <span data-ttu-id="7678e-175">HMAC (**védőérték** + "\n" + **productId** + "\n" + **userId**)</span><span class="sxs-lookup"><span data-stu-id="7678e-175">HMAC(**salt** + '\n' + **productId** + '\n' + **userId**)</span></span>
     > 
     > 
   * <span data-ttu-id="7678e-176">A fenti számított kivonat értékének összehasonlítása a **sig** lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="7678e-176">Compare the above-computed hash to the value of the **sig** query parameter.</span></span> <span data-ttu-id="7678e-177">Ha a két kivonatok megegyeznek, lépjen tovább a következő lépés, egyébként visszautasítja a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="7678e-177">If the two hashes match, move on to the next step, otherwise deny the request.</span></span>
3. <span data-ttu-id="7678e-178">Hajtsa végre a kért művelet típusa alapján bármely termék feldolgozási **művelet** – pl. számlázást, a további kérdésekre, stb.</span><span class="sxs-lookup"><span data-stu-id="7678e-178">Perform any product subscription processing based on the type of operation requested in **operation** - e.g. billing, further questions, etc.</span></span>
4. <span data-ttu-id="7678e-179">Sikeresen előfizetés a felhasználót, hogy a termék a oldalon, az előfizetés a felhasználó által az API Management termékre [termék-előfizetéshez tartozó REST API hívása].</span><span class="sxs-lookup"><span data-stu-id="7678e-179">On successfully subscribing the user to the product on your side, subscribe the user to the API Management product by [calling the REST API for product subscription].</span></span>

## <span data-ttu-id="7678e-180"><a name="delegate-example-code"></a> Példakód</span><span class="sxs-lookup"><span data-stu-id="7678e-180"><a name="delegate-example-code"> </a> Example Code</span></span>
<span data-ttu-id="7678e-181">Ezek mintakódok bemutatják, hogyan érvénybe a *delegálás érvényesítési kulcs*, melynek értéke a közzétevő portál delegálás képernyőjén hozzon létre egy HMAC ellenőrzi az aláírást, majd szolgáló igazolására a átadott Kérelemátirányítás érvényességét.</span><span class="sxs-lookup"><span data-stu-id="7678e-181">These code samples show how to take the *delegation validation key*, which is set in the Delegation screen of the publisher portal, to create a HMAC which is then used to validate the signature, proving the validity of the passed returnUrl.</span></span> <span data-ttu-id="7678e-182">A termékkód és a felhasználói azonosítóját, enyhe módosítással működik ugyanazt a kódot.</span><span class="sxs-lookup"><span data-stu-id="7678e-182">The same code works for the productId and userId with slight modification.</span></span>

<span data-ttu-id="7678e-183">**C#-kódban returnUrl kivonatának létrehozásához**</span><span class="sxs-lookup"><span data-stu-id="7678e-183">**C# code to generate hash of returnUrl**</span></span>

```c#
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
}
```

<span data-ttu-id="7678e-184">**NodeJS kód returnUrl kivonatának létrehozásához**</span><span class="sxs-lookup"><span data-stu-id="7678e-184">**NodeJS code to generate hash of returnUrl**</span></span>

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature to sig query parameter

var signature = digest.toString('base64');
```

## <a name="next-steps"></a><span data-ttu-id="7678e-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7678e-185">Next steps</span></span>
<span data-ttu-id="7678e-186">Delegálás további információkért tekintse meg a következő videó.</span><span class="sxs-lookup"><span data-stu-id="7678e-186">For more information on delegation, see the following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
<span data-ttu-id="7678e-187">[egyszeri bejelentkezéses (SSO) jogkivonatot kérni]: http://go.microsoft.com/fwlink/?LinkId=507409</span><span class="sxs-lookup"><span data-stu-id="7678e-187">[request a single-sign-on (SSO) token]: http://go.microsoft.com/fwlink/?LinkId=507409</span></span>
<span data-ttu-id="7678e-188">[felhasználó létrehozása]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser</span><span class="sxs-lookup"><span data-stu-id="7678e-188">[create a user]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser</span></span>
<span data-ttu-id="7678e-189">[termék-előfizetéshez tartozó REST API hívása]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO</span><span class="sxs-lookup"><span data-stu-id="7678e-189">[calling the REST API for product subscription]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO</span></span>
[Next steps]: #next-steps
<span data-ttu-id="7678e-190">[alábbi példakód]: #delegate-example-code</span><span class="sxs-lookup"><span data-stu-id="7678e-190">[example code provided below]: #delegate-example-code</span></span>

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
