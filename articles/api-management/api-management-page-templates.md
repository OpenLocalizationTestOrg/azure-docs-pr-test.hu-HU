---
title: Az Azure API Management sablonok lapon |} Microsoft Docs
description: "Ismerje meg, hogyan szabhatja testre a sablonok használatával az Azure API Management portál lapjai fejlesztői tartalmát."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: e57df269-1019-4b74-b74d-53155b809d59
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 7f9ef37a694bce786b6acaa428df83f0cb23c2dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="page-templates-in-azure-api-management"></a><span data-ttu-id="f4c1e-103">Az Azure API Management sablonokat</span><span class="sxs-lookup"><span data-stu-id="f4c1e-103">Page templates in Azure API Management</span></span>
<span data-ttu-id="f4c1e-104">Az Azure API Management lehetővé teszi a tartalom developer portálon lapok használatával konfigurálhatja a tartalom-sablonok testreszabása.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="f4c1e-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és az Ön által választott szerkesztőben, mint [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), konfigurálja a tartalmat, a lapok, ahogyan szeretné ezeket a sablonokat használ nagy rugalmasságot biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="f4c1e-106">Ebben a szakaszban a sablonok lehetővé teszik testre szabhatja a bejelentkezést, bejelentkezési tartalma fel, és a lap nem található a lapok a fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-106">The templates in this section allow you to customize the content of the sign in, sign up, and page not found pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="f4c1e-107">bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f4c1e-107">Sign in</span></span>](#SignIn)  
  
-   [<span data-ttu-id="f4c1e-108">feliratkozni</span><span class="sxs-lookup"><span data-stu-id="f4c1e-108">Sign up</span></span>](#SignUp)  
  
-   [<span data-ttu-id="f4c1e-109">A lap nem található</span><span class="sxs-lookup"><span data-stu-id="f4c1e-109">Page not found</span></span>](#PageNotFound)  
  
> [!NOTE]
>  <span data-ttu-id="f4c1e-110">Minta alapértelmezett sablonok az alábbi dokumentáció szerepelnek, de folyamatos fejlesztéseket miatt változhat.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-110">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="f4c1e-111">Megtekintheti az élő alapértelmezett sablonok a fejlesztői portálra nyissa meg a kívánt egyéni sablonokat.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-111">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="f4c1e-112">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="f4c1e-112">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="f4c1e-113"><a name="SignIn"></a>bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f4c1e-113"><a name="SignIn"></a> Sign in</span></span>  
 <span data-ttu-id="f4c1e-114">A **bejelentkezés** sablon lehetővé teszi a bejelentkezési oldal a fejlesztői portálra testreszabását.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-114">The **sign in** template allows you to customize the sign in page in the developer portal.</span></span>  
  
 <span data-ttu-id="f4c1e-115">![Jelentkezzen be az oldal](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM bejelentkezési oldal fejlesztői portál sablonok")</span><span class="sxs-lookup"><span data-stu-id="f4c1e-115">![Sign In Page](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM Sign In Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="f4c1e-116">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="f4c1e-116">Default template</span></span>  
  
```xml  
<h2 class="text-center">{% localized "SigninStrings|WebAuthenticationSigninTitle" %}</h2>  
{% if registrationEnabled == true %}  
<p class="text-center">{% localized "SigninStrings|WebAuthenticationNotAMember" %}</p>  
{% endif %}  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    {% if registrationEnabled == true %}  
        <p>{% localized "SigninStrings|WebAuthenticationSigininWithPassword" %}</p>  
    <basic-SignIn></basic-SignIn>  
    {% endif %}  
  </div>  
  
    {% if registrationEnabled != true and providers.size == 0 %}  
        {% localized "ProviderInfoStrings|TextboxExternalIdentitiesDisabled" %}  
  {% else %}  
        {% if providers.size > 0 %}  
      <div class="col-md-6">  
            <div class="providers-list">  
                <p class="text-left">  
                {% if registrationEnabled == true %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitation" %}  
                {% else %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitationPrimary" %}  
                {% endif %}  
                </p>  
        <providers></providers>  
            </div>  
    </div>  
        {% endif %}  
    {% endif %}  
  
  {% if userRegistrationTermsEnabled == true %}  
    <div class="col-md-6">  
        <div id="terms" class="modal" role="dialog" tabindex="-1">  
            <div class="modal-dialog">  
                <div class="modal-content">  
                    <div class="modal-header">  
                        <h4 class="modal-title">{% localized "SigninResources|DialogHeadingTermsOfUse" %}</h4>  
                    </div>  
                    <div class="modal-body break-all">{{userRegistrationTerms}}</div>  
                    <div class="modal-footer">  
                        <button type="button" class="btn btn-default" data-dismiss="modal">{% localized "CommonStrings|ButtonLabelClose" %}</button>  
                    </div>  
                </div>  
            </div>  
        </div>  
        <p>{% localized "SigninResources|TextblockUserRegistrationTermsProvided" %}</p>  
    </div>  
    {% endif %}  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="f4c1e-117">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="f4c1e-117">Controls</span></span>  
 <span data-ttu-id="f4c1e-118">Ez a sablon lehet, hogy használja a következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="f4c1e-118">This template may  use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="f4c1e-119">Basic-bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f4c1e-119">basic-signin</span></span>](api-management-page-controls.md#basic-signin)  
  
-   [<span data-ttu-id="f4c1e-120">szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="f4c1e-120">providers</span></span>](api-management-page-controls.md#providers)  
  
### <a name="data-model"></a><span data-ttu-id="f4c1e-121">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="f4c1e-121">Data model</span></span>  
 <span data-ttu-id="f4c1e-122">[A felhasználói bejelentkezési](api-management-template-data-model-reference.md#UseSignIn) entitás.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-122">[User sign in](api-management-template-data-model-reference.md#UseSignIn) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="f4c1e-123">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="f4c1e-123">Sample template data</span></span>  
  
```json  
{  
    "Email": null,  
    "Password": null,  
    "ReturnUrl": null,  
    "RememberMe": false,  
    "RegistrationEnabled": true,  
    "DelegationEnabled": false,  
    "DelegationUrl": null,  
    "SsoSignUpUrl": null,  
    "AuxServiceUrl": "https://manage.windowsazure.com/#Workspaces/ApiManagementExtension/service/contoso5/dashboard",  
    "Providers": [  
        {  
            "Properties": {  
                "AuthenticationType": "Aad",  
                "Caption": "Azure Active Directory"  
            },  
            "AuthenticationType": "Aad",  
            "Caption": "Azure Active Directory"  
        }  
    ],  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsEnabled": false  
}  
```  
  
##  <span data-ttu-id="f4c1e-124"><a name="SignUp"></a>feliratkozni</span><span class="sxs-lookup"><span data-stu-id="f4c1e-124"><a name="SignUp"></a> Sign up</span></span>  
 <span data-ttu-id="f4c1e-125">A **regisztráljon** sablon lehetővé teszi a bejelentkezési oldal a fejlesztői portálra testreszabását.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-125">The **sign up** template allows you to customize the sign up page in the developer portal.</span></span>  
  
 <span data-ttu-id="f4c1e-126">![Regisztrációs oldalra](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM bejelentkezési oldal Developer-portál sablonok")</span><span class="sxs-lookup"><span data-stu-id="f4c1e-126">![Sign Up Page](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM Sign Up Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="f4c1e-127">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="f4c1e-127">Default template</span></span>  
  
```xml  
<h2 class="text-center">{% localized "SignupStrings|PageTitleSignup" %}</h2>  
<p class="text-center">  
  {% localized "SignupStrings|WebAuthenticationAlreadyAMember" %} <a href="/signin">{% localized "SignupStrings|WebAuthenticationSigninNow" %}</a>  
</p>  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    <p>{% localized "SignupStrings|WebAuthenticationCreateNewAccount" %}</p>  
    <sign-up></sign-up>  
  </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="f4c1e-128">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="f4c1e-128">Controls</span></span>  
 <span data-ttu-id="f4c1e-129">Ez a sablon lehet, hogy használja a következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="f4c1e-129">This template may  use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="f4c1e-130">-előfizetés</span><span class="sxs-lookup"><span data-stu-id="f4c1e-130">sign-up</span></span>](api-management-page-controls.md#sign-up)  
  
### <a name="data-model"></a><span data-ttu-id="f4c1e-131">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="f4c1e-131">Data model</span></span>  
 <span data-ttu-id="f4c1e-132">[Felhasználói bejelentkezési](api-management-template-data-model-reference.md#UserSignUp) entitás.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-132">[User sign up](api-management-template-data-model-reference.md#UserSignUp) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="f4c1e-133">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="f4c1e-133">Sample template data</span></span>  
  
```json  
{  
    "PasswordConfirm": null,  
    "Password": null,  
    "PasswordVerdictLevel": 0,  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsOptions": 0,  
    "ConsentAccepted": false,  
    "Email": null,  
    "FirstName": null,  
    "LastName": null,  
    "UserData": null,  
    "NameIdentifier": null,  
    "ProviderName": null  
}  
```  
  
##  <span data-ttu-id="f4c1e-134"><a name="PageNotFound"></a>A lap nem található</span><span class="sxs-lookup"><span data-stu-id="f4c1e-134"><a name="PageNotFound"></a> Page not found</span></span>  
 <span data-ttu-id="f4c1e-135">A **oldal nem található** sablon teszi lehetővé testre szabhatja a lapot oldal nem található a fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-135">The **page not found** template allows you to customize the page not found page in the developer portal.</span></span>  
  
 <span data-ttu-id="f4c1e-136">![Nem található az oldal](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM nem található oldal fejlesztői portál sablonok")</span><span class="sxs-lookup"><span data-stu-id="f4c1e-136">![Not Found Page](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM Not Found Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="f4c1e-137">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="f4c1e-137">Default template</span></span>  
  
```xml  
<h2>{% localized "NotFoundStrings|PageTitleNotFound" %}</h2>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialCause" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseOldLink" %}</li>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseMisspelledUrl" %}</li>  
</ul>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialSolution" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialSolutionRetype" %}</li>  
  <li>  
    {% capture textPotentialSolutionStartOver %}{% localized "NotFoundStrings|TextblockPotentialSolutionStartOver" %}{% endcapture %}  
    {% capture homeLink %}<a href="/">{% localized "NotFoundStrings|LinkLabelHomePage" %}</a>{% endcapture %}  
    {% assign replaceString = '{0}' %}  
  
    {{ textPotentialSolutionStartOver | replace : replaceString, homeLink }}  
  </li>  
</ul>  
  
<p>  
  {% capture textReportProblem %}{% localized "NotFoundStrings|TextReportProblem" %}{% endcapture %}  
  {% capture emailLink %}<a href="mailto:apimgmt@microsoft.com" target="_self" title="API Management Support">{% localized "NotFoundStrings|LinkLabelSendUsEmail" %}</a>{% endcapture %}  
  {% assign replaceString = '{0}' %}  
  
  {{ textReportProblem | replace : replaceString, emailLink }}  
</p>  
```  
  
### <a name="controls"></a><span data-ttu-id="f4c1e-138">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="f4c1e-138">Controls</span></span>  
 <span data-ttu-id="f4c1e-139">Ez a sablon nem használhatja bármelyik [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="f4c1e-139">This template may  not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="f4c1e-140">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="f4c1e-140">Data model</span></span>  
  
|<span data-ttu-id="f4c1e-141">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f4c1e-141">Property</span></span>|<span data-ttu-id="f4c1e-142">Típus</span><span class="sxs-lookup"><span data-stu-id="f4c1e-142">Type</span></span>|<span data-ttu-id="f4c1e-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="f4c1e-143">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="f4c1e-144">referenceCode</span><span class="sxs-lookup"><span data-stu-id="f4c1e-144">referenceCode</span></span>|<span data-ttu-id="f4c1e-145">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f4c1e-145">string</span></span>|<span data-ttu-id="f4c1e-146">A kód jön létre, ha ezen a lapon jelent meg, egy belső hiba miatt.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-146">Code generated if this page was displayed as the result of an internal error.</span></span>|  
|<span data-ttu-id="f4c1e-147">Hibakód</span><span class="sxs-lookup"><span data-stu-id="f4c1e-147">errorCode</span></span>|<span data-ttu-id="f4c1e-148">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f4c1e-148">string</span></span>|<span data-ttu-id="f4c1e-149">A kód jön létre, ha ezen a lapon jelent meg, egy belső hiba miatt.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-149">Code generated if this page was displayed as the result of an internal error.</span></span>|  
|<span data-ttu-id="f4c1e-150">emailBody</span><span class="sxs-lookup"><span data-stu-id="f4c1e-150">emailBody</span></span>|<span data-ttu-id="f4c1e-151">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f4c1e-151">string</span></span>|<span data-ttu-id="f4c1e-152">Az e-mail törzsének jön létre, ha ezen a lapon jelent meg, egy belső hiba miatt.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-152">Email body generated if this page was displayed as the result of an internal error.</span></span>|  
|<span data-ttu-id="f4c1e-153">requestedUrl</span><span class="sxs-lookup"><span data-stu-id="f4c1e-153">requestedUrl</span></span>|<span data-ttu-id="f4c1e-154">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f4c1e-154">string</span></span>|<span data-ttu-id="f4c1e-155">Ha a lap nem található a kért URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-155">The URL requested when the page was not found.</span></span>|  
|<span data-ttu-id="f4c1e-156">referrerUrl</span><span class="sxs-lookup"><span data-stu-id="f4c1e-156">referrerUrl</span></span>|<span data-ttu-id="f4c1e-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f4c1e-157">string</span></span>|<span data-ttu-id="f4c1e-158">A kért URL-címre hivatkozó URL.</span><span class="sxs-lookup"><span data-stu-id="f4c1e-158">The referrer URL to the requested URL.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="f4c1e-159">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="f4c1e-159">Sample template data</span></span>  
  
```json  
{  
    "referenceCode": null,  
    "errorCode": null,  
    "emailBody": null,  
    "requestedUrl": "https://contoso5.portal.azure-api.net:443/NotFoundPage?startEditTemplate=NotFoundPage",  
    "referrerUrl": "https://contoso5.portal.azure-api.net/signup?startEditTemplate=SignUpTemplate"  
}  
```

## <a name="next-steps"></a><span data-ttu-id="f4c1e-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f4c1e-160">Next steps</span></span>
<span data-ttu-id="f4c1e-161">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f4c1e-161">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>