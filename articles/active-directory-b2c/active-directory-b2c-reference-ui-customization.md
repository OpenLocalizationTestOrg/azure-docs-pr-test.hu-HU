---
title: "Felhasználói felület (UI) testreszabása – az Azure AD B2C |} Microsoft Docs"
description: "A felhasználói felület (UI) testreszabási szolgáltatások az Azure Active Directory B2C témakör:"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 99f5a391-5328-471d-a15c-a2fafafe233d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 122fa997ea11b369aae3c59edf0043ab19d21aea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a><span data-ttu-id="92301-103">Az Azure Active Directory B2C: Testreszabása az Azure AD B2C felhasználói felület (UI)</span><span class="sxs-lookup"><span data-stu-id="92301-103">Azure Active Directory B2C: Customize the Azure AD B2C user interface (UI)</span></span>

<span data-ttu-id="92301-104">Felhasználói élmény az alkalmazás elérhető az ügyfél kiemelkedő.</span><span class="sxs-lookup"><span data-stu-id="92301-104">User experience is paramount in a customer facing application.</span></span>  <span data-ttu-id="92301-105">Az ügyfél alap nő a márka megjelenésének és arculatának felhasználói élmény létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="92301-105">Grow your customer base by crafting user experiences with the look and feel of your brand.</span></span> <span data-ttu-id="92301-106">Az Azure Active Directory B2C (az Azure AD B2C) lehetővé teszi, hogy testre szabhatja a regisztráció, bejelentkezés, profil szerkesztése, és a jelszó-átállítási tartalmazó képpontos tökéletes vezérlő.</span><span class="sxs-lookup"><span data-stu-id="92301-106">Azure Active Directory B2C (Azure AD B2C) lets you customize sign-up, sign-in, profile editing, and password reset pages with pixel-perfect control.</span></span>

> [!NOTE]
> <span data-ttu-id="92301-107">A jelen cikkben ismertetett lap felhasználói felületi testreszabási szolgáltatás nem vonatkozik a bejelentkezési egyetlen házirend, a hozzá tartozó jelszó alaphelyzetbe állítása lap és ellenőrzési e-maileket.</span><span class="sxs-lookup"><span data-stu-id="92301-107">The page UI customization feature described in this article does not apply to the sign-in only policy, its accompanying password reset page, and verification emails.</span></span>  <span data-ttu-id="92301-108">Ezek a funkciók használata a [vállalati arculat megjelenítése a szolgáltatás](../active-directory/active-directory-add-company-branding.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="92301-108">These features use the [company branding feature](../active-directory/active-directory-add-company-branding.md) instead.</span></span>
>

<span data-ttu-id="92301-109">Ez a cikk ismerteti a következő témaköröket:</span><span class="sxs-lookup"><span data-stu-id="92301-109">This article covers the following topics:</span></span>

* <span data-ttu-id="92301-110">A lap felhasználói felületi testreszabási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="92301-110">The page UI customization feature.</span></span>
* <span data-ttu-id="92301-111">Egy eszköz a HTML-tartalmakat feltöltése az Azure Blob Storage a lap felhasználói felületének testreszabása szolgáltatással való használathoz.</span><span class="sxs-lookup"><span data-stu-id="92301-111">A tool for uploading HTML content to Azure Blob Storage for use with the page UI customization feature.</span></span>
* <span data-ttu-id="92301-112">A testre szabható egymásra épülő stílus lapok (CSS) használata az Azure AD B2C által használt Kezelőfelület-elemek.</span><span class="sxs-lookup"><span data-stu-id="92301-112">The UI elements used by Azure AD B2C that you can customize using Cascading Style Sheets (CSS).</span></span>
* <span data-ttu-id="92301-113">Gyakorlati tanácsok gyakorló ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="92301-113">Best practices when exercising this feature.</span></span>

## <a name="the-page-ui-customization-feature"></a><span data-ttu-id="92301-114">A lap felhasználói felületi testreszabási szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="92301-114">The page UI customization feature</span></span>

<span data-ttu-id="92301-115">Testre szabhatja a megjelenését a felhasználói regisztráció, bejelentkezés, a jelszó alaphelyzetbe állítása és profil szerkesztése lapok (konfigurálásával [házirendek](active-directory-b2c-reference-policies.md)).</span><span class="sxs-lookup"><span data-stu-id="92301-115">You can customize the look and feel of customer sign-up, sign-in, password reset, and profile-editing pages (by configuring [policies](active-directory-b2c-reference-policies.md)).</span></span> <span data-ttu-id="92301-116">Az ügyfelek beolvasása zökkenőmentes élményt, amikor máshová az alkalmazás és az Azure AD B2C által kiszolgált lapok között.</span><span class="sxs-lookup"><span data-stu-id="92301-116">Your customers get a seamless experience when navigating between your application and pages served by Azure AD B2C.</span></span>

<span data-ttu-id="92301-117">Más szolgáltatás, amelyben felhasználói felület lehetőségei, az Azure AD B2C használ-e egy egyszerű és modern felhasználói felület testreszabásával megközelítése szemben.</span><span class="sxs-lookup"><span data-stu-id="92301-117">Unlike other services where UI options, Azure AD B2C uses a simple and modern approach to UI customization.</span></span>

<span data-ttu-id="92301-118">Hogyan működik ez: Azure AD B2C az ügyfél böngészőjében kód fut, és a modern megközelítés nevű [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="92301-118">Here's how it works: Azure AD B2C runs code in your customer's browser and uses a modern approach called [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span></span>  <span data-ttu-id="92301-119">Tartalom futásidőben, be van töltve a házirendben megadott URL-címről.</span><span class="sxs-lookup"><span data-stu-id="92301-119">At run-time, content is loaded from a URL that you specify in a policy.</span></span> <span data-ttu-id="92301-120">Másik URL-címéből különböző oldalain adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="92301-120">You can specify different URLs for different pages.</span></span> <span data-ttu-id="92301-121">Tartalom betöltődnek az URL-cím egy HTML-részlet szúrja be az Azure AD B2C egyesített, miután az ügyfél az oldal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="92301-121">After content loaded from your URL is merged with an HTML fragment inserted from Azure AD B2C, the page is displayed to your customer.</span></span> <span data-ttu-id="92301-122">Ehhez van szüksége:</span><span class="sxs-lookup"><span data-stu-id="92301-122">All you need to do is:</span></span>

1. <span data-ttu-id="92301-123">Hozzon létre egy üres tartalom szabályos HTML5 `<div id="api"></div>` elem található valahol az `<body>`.</span><span class="sxs-lookup"><span data-stu-id="92301-123">Create well-formed HTML5 content with an empty `<div id="api"></div>` element located somewhere in the `<body>`.</span></span> <span data-ttu-id="92301-124">A elem jelöli, ahol az Azure AD B2C tartalom egészül ki.</span><span class="sxs-lookup"><span data-stu-id="92301-124">This element marks where the Azure AD B2C content is inserted.</span></span>
1. <span data-ttu-id="92301-125">A tartalmat a HTTPS-végpontnak állomás (a CORS engedélyezett).</span><span class="sxs-lookup"><span data-stu-id="92301-125">Host your content on an HTTPS endpoint (with CORS allowed).</span></span> <span data-ttu-id="92301-126">Vegye figyelembe, mind az beszerzése és beállítások kérelem módszerek engedélyezni kell a CORS konfigurálása során.</span><span class="sxs-lookup"><span data-stu-id="92301-126">Note both GET and OPTIONS request methods must be enabled when configuring CORS.</span></span>
1. <span data-ttu-id="92301-127">CSS használatával az Azure AD B2C beszúrása felhasználói felületi elemeket stílus.</span><span class="sxs-lookup"><span data-stu-id="92301-127">Use CSS to style the UI elements that Azure AD B2C inserts.</span></span>

### <a name="a-basic-example-of-customized-html"></a><span data-ttu-id="92301-128">Egy alapszintű testreszabott HTML – példa</span><span class="sxs-lookup"><span data-stu-id="92301-128">A basic example of customized HTML</span></span>

<span data-ttu-id="92301-129">A következő példa egy a legalapvetőbb HTML-tartalmakat, amelyek segítségével tesztelheti ezt a képességet.</span><span class="sxs-lookup"><span data-stu-id="92301-129">The following example is the most basic HTML content that you can use to test this capability.</span></span> <span data-ttu-id="92301-130">Használja a [segédeszköze](active-directory-b2c-reference-ui-customization-helper-tool.md) feltöltése és konfigurálása az Azure Blob Storage tárolóban ezt a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="92301-130">Use the [helper tool](active-directory-b2c-reference-ui-customization-helper-tool.md) to upload and configure this content on your Azure Blob storage.</span></span> <span data-ttu-id="92301-131">Ezután ellenőrizheti, hogy az egyszerű, nem stilizált gombok & űrlapmezők minden oldalon megjelenített és megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="92301-131">You can then verify that the basic, non-stylized buttons & form fields on each page are displayed and functional.</span></span>

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
    </body>
</html>
```

## <a name="test-out-the-ui-customization-feature"></a><span data-ttu-id="92301-132">Tesztelje a felhasználói felület testreszabása szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="92301-132">Test out the UI customization feature</span></span>

<span data-ttu-id="92301-133">Szeretné, hogy a minta HTML és CSS tartalom használatával a felhasználói felület testreszabása a szolgáltatás kipróbálásához?</span><span class="sxs-lookup"><span data-stu-id="92301-133">Want to try out the UI customization feature by using our sample HTML and CSS content?</span></span>  <span data-ttu-id="92301-134">Ön mellékelt [egy segédeszköze](active-directory-b2c-reference-ui-customization-helper-tool.md) , amely feltölti és minta tartalom konfigurálja az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="92301-134">We've provided you [a helper tool](active-directory-b2c-reference-ui-customization-helper-tool.md) that uploads and configures sample content on your Azure Blob storage.</span></span>

> [!NOTE]
> <span data-ttu-id="92301-135">A felhasználói felület tartalom bárhol tárolhatja: a webkiszolgálók, a tartalomtovábbító, AWS S3, megosztási fájlrendszerek stb. Mindaddig, amíg a CORS engedélyezése mellett a tartalmak tárolása egy nyilvánosan elérhető HTTPS-végponton, akkor áll.</span><span class="sxs-lookup"><span data-stu-id="92301-135">You can host your UI content anywhere: on web servers, CDNs, AWS S3, file sharing systems, etc. As long as the content is hosted on a publicly available HTTPS endpoint with CORS enabled, you are good to go.</span></span> <span data-ttu-id="92301-136">Azure Blob Storage tárolóban csak szemléltető célokra használjuk.</span><span class="sxs-lookup"><span data-stu-id="92301-136">We are using Azure Blob storage for illustrative purposes only.</span></span>
>

## <a name="the-ui-fragments-embedded-by-azure-ad-b2c"></a><span data-ttu-id="92301-137">A felhasználói felület töredék Azure AD B2C beágyazott</span><span class="sxs-lookup"><span data-stu-id="92301-137">The UI fragments embedded by Azure AD B2C</span></span>

<span data-ttu-id="92301-138">Az alábbi szakaszok tartalmazzák a HTML5-töredék, amely az Azure AD B2C egyesíti a `<div id="api"></div>` elem található a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="92301-138">The following sections list the HTML5 fragments that Azure AD B2C merges into the `<div id="api"></div>` element located in your content.</span></span> <span data-ttu-id="92301-139">**Ne jelölje be ezeket a töredékeket a HTML 5 tartalom.**</span><span class="sxs-lookup"><span data-stu-id="92301-139">**Do not insert these fragments in your HTML 5 content.**</span></span> <span data-ttu-id="92301-140">Az Azure AD B2C szolgáltatás futásidőben beszúrja azokat.</span><span class="sxs-lookup"><span data-stu-id="92301-140">The Azure AD B2C service inserts them in at run-time.</span></span> <span data-ttu-id="92301-141">Használja referenciának ezeket a töredékeket, a saját egymásra épülő stílus lapok (CSS) tervezésekor.</span><span class="sxs-lookup"><span data-stu-id="92301-141">Use these fragments as a reference when designing your own Cascading Style Sheets (CSS).</span></span>

### <a name="fragment-inserted-into-the-identity-provider-selection-page"></a><span data-ttu-id="92301-142">A "identitás szolgáltató kiválasztása lapon" beszúrt töredék</span><span class="sxs-lookup"><span data-stu-id="92301-142">Fragment inserted into the "Identity provider selection page"</span></span>

<span data-ttu-id="92301-143">Ezen a lapon a regisztráció vagy bejelentkezés során a felhasználó választhat az identitás-szolgáltatóktól listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="92301-143">This page contains a list of identity providers that the user can choose from during sign-up or sign-in.</span></span> <span data-ttu-id="92301-144">Az ilyen gombokat közé tartozik a közösségi Identitásszolgáltatók, például a Facebook-on és Google + és helyi fiókok (e-mail cím vagy a felhasználó neve alapján).</span><span class="sxs-lookup"><span data-stu-id="92301-144">These buttons include social identity providers such as Facebook and Google+, or local accounts (based on email address or user name).</span></span>

```HTML
<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-local-account-sign-up-page"></a><span data-ttu-id="92301-145">A "helyi fiók előfizetési page" beszúrt töredék</span><span class="sxs-lookup"><span data-stu-id="92301-145">Fragment inserted into the "Local account sign-up page"</span></span>

<span data-ttu-id="92301-146">Ez a lap tartalmaz egy e-mail címet vagy egy felhasználónevet alapuló helyi fiók regisztrációs űrlap.</span><span class="sxs-lookup"><span data-stu-id="92301-146">This page contains a form for local account sign-up based on an email address or a user name.</span></span> <span data-ttu-id="92301-147">Az űrlap szöveg beviteli mezőt, a jelszó beviteli mezője, a választógomb, a egyetlen legördülő listák és a többszörös kiválasztási jelölőnégyzetet például különböző bemeneti vezérlőket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="92301-147">The form can contain different input controls such as text input box, password entry box, radio button, single-select drop-down boxes, and multi-select check boxes.</span></span>

```HTML
<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-social-account-sign-up-page"></a><span data-ttu-id="92301-148">A "" közösségi fiók bejelentkezési oldalán"beszúrt töredék</span><span class="sxs-lookup"><span data-stu-id="92301-148">Fragment inserted into the ""Social account sign-up page"</span></span>

<span data-ttu-id="92301-149">Ezen a lapon jelenhet meg, ha regisztrál a Facebook-on vagy a Google + például közösségi identitásszolgáltató egy meglévő fiókkal.</span><span class="sxs-lookup"><span data-stu-id="92301-149">This page may appear when signing up using an existing account from a social identity provider such as Facebook or Google+.</span></span>  <span data-ttu-id="92301-150">Ha további információkat kell gyűjteni a regisztrációs űrlap használatával a végfelhasználó használható.</span><span class="sxs-lookup"><span data-stu-id="92301-150">It is used when additional information must be collected from the end user using a sign-up form.</span></span> <span data-ttu-id="92301-151">Ezen a lapon hasonlít a helyi fiók regisztrációs oldalon (az előző szakaszban látható) a jelszó számbeviteli mezők kivételével.</span><span class="sxs-lookup"><span data-stu-id="92301-151">This page is similar to the local account sign-up page (shown in the previous section) with the exception of the password entry fields.</span></span>

### <a name="fragment-inserted-into-the-unified-sign-up-or-sign-in-page"></a><span data-ttu-id="92301-152">A "egyesített regisztráció vagy bejelentkezés page" beszúrt töredék</span><span class="sxs-lookup"><span data-stu-id="92301-152">Fragment inserted into the "Unified sign-up or sign-in page"</span></span>

<span data-ttu-id="92301-153">Ezen a lapon a regisztrációs és bejelentkezés ügyfelek, akik használhatják a közösségi Identitásszolgáltatók, például a Facebook-on vagy a Google + és helyi fiókok kezeli.</span><span class="sxs-lookup"><span data-stu-id="92301-153">This page handles both sign-up & sign-in of customers, who can use social identity providers such as Facebook or Google+, or local accounts.</span></span>

```HTML
<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>
```

### <a name="fragment-inserted-into-the-multi-factor-authentication-page"></a><span data-ttu-id="92301-154">A "multi-factor authentication-page" beszúrt töredék</span><span class="sxs-lookup"><span data-stu-id="92301-154">Fragment inserted into the "Multi-factor authentication page"</span></span>

<span data-ttu-id="92301-155">Ezen a lapon felhasználók regisztráció vagy bejelentkezés során ellenőrizheti a telefonszámok (szöveges vagy hangos használatával).</span><span class="sxs-lookup"><span data-stu-id="92301-155">On this page, users can verify their phone numbers (using text or voice) during sign-up or sign-in.</span></span>

```HTML
<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-error-page"></a><span data-ttu-id="92301-156">A "" hibalap"beszúrt töredék</span><span class="sxs-lookup"><span data-stu-id="92301-156">Fragment inserted into the ""Error page"</span></span>

```HTML
<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>
```

## <a name="localizing-your-html-content"></a><span data-ttu-id="92301-157">A HTML-tartalmakat azaz</span><span class="sxs-lookup"><span data-stu-id="92301-157">Localizing your HTML content</span></span>

<span data-ttu-id="92301-158">A HTML-tartalmakat is honosítás bekapcsolásával ["Nyelvi testreszabási"](active-directory-b2c-reference-language-customization.md).</span><span class="sxs-lookup"><span data-stu-id="92301-158">You can localize your HTML content by turning on ['Language customization'](active-directory-b2c-reference-language-customization.md).</span></span>  <span data-ttu-id="92301-159">Ez a funkció lehetővé teszi, hogy az Azure AD B2C az Open ID Connect paraméter továbbítására `ui-locales`, a végpontnak.</span><span class="sxs-lookup"><span data-stu-id="92301-159">Enabling this feature allows Azure AD B2C to forward the Open ID Connect parameter, `ui-locales`, to your endpoint.</span></span>  <span data-ttu-id="92301-160">A tartalomkiszolgáló Ez a paraméter használatával adja meg a testreszabott HTML-lapok nyelvspecifikus.</span><span class="sxs-lookup"><span data-stu-id="92301-160">Your content server can use this parameter to provide customized HTML pages that are language-specific.</span></span>

## <a name="things-to-remember-when-building-your-own-content"></a><span data-ttu-id="92301-161">Megjegyzendő tudnivalók a saját tartalom felépítése közben</span><span class="sxs-lookup"><span data-stu-id="92301-161">Things to remember when building your own content</span></span>

<span data-ttu-id="92301-162">Ha azt tervezi, a lap felhasználói felületének testreszabása szolgáltatás használatához, tekintse át az alábbi gyakorlati tanácsokat:</span><span class="sxs-lookup"><span data-stu-id="92301-162">If you are planning to use the page UI customization feature, review the following best practices:</span></span>

* <span data-ttu-id="92301-163">Nem az Azure AD B2C alapértelmezett tartalmát, és próbálja meg módosítani.</span><span class="sxs-lookup"><span data-stu-id="92301-163">Don't copy the Azure AD B2C's default content and attempt to modify it.</span></span> <span data-ttu-id="92301-164">Érdemes a HTML5 tartalom teljesen új összeállításához és alapértelmezett tartalom használni.</span><span class="sxs-lookup"><span data-stu-id="92301-164">It is best to build your HTML5 content from scratch and to use default content as reference.</span></span>
* <span data-ttu-id="92301-165">Biztonsági okok miatt jelenleg nem engedélyezi, hogy bármely JavaScript szerepeljenek a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="92301-165">For security reasons, we don't allow you to include any JavaScript in your content.</span></span> <span data-ttu-id="92301-166">A legtöbb mire van szüksége a kezdő verzióról elérhetőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="92301-166">Most of what you need should be available out of the box.</span></span> <span data-ttu-id="92301-167">Ha nem, [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) kérjen új funkciókat.</span><span class="sxs-lookup"><span data-stu-id="92301-167">If not, use [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) to request new functionality.</span></span>
* <span data-ttu-id="92301-168">A támogatott böngészőkben:</span><span class="sxs-lookup"><span data-stu-id="92301-168">Supported browser versions:</span></span>
  * <span data-ttu-id="92301-169">Az Internet Explorer 11, 10, a peremhálózati</span><span class="sxs-lookup"><span data-stu-id="92301-169">Internet Explorer 11, 10, Edge</span></span>
  * <span data-ttu-id="92301-170">Az Internet Explorer 9 böngészőben 8 korlátozott támogatása</span><span class="sxs-lookup"><span data-stu-id="92301-170">Limited support for Internet Explorer 9, 8</span></span>
  * <span data-ttu-id="92301-171">Google Chrome 42.0 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="92301-171">Google Chrome 42.0 and above</span></span>
  * <span data-ttu-id="92301-172">Mozilla Firefox 38.0 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="92301-172">Mozilla Firefox 38.0 and above</span></span>
