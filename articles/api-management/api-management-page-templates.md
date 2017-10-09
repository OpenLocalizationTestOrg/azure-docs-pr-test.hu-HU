---
title: az Azure API Management aaaPage sablonok |} Microsoft Docs
description: "Ismerje meg, hogyan toocustomize hello fejlesztői sablonok használatával az Azure API Management portál lapjai tartalmát."
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
ms.openlocfilehash: 84bd971ad4bcacfdd36c2ebbe05b16063f2a547b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="page-templates-in-azure-api-management"></a><span data-ttu-id="4b56b-103">Az Azure API Management sablonokat</span><span class="sxs-lookup"><span data-stu-id="4b56b-103">Page templates in Azure API Management</span></span>
<span data-ttu-id="4b56b-104">Az Azure API Management biztosít, akkor hello képességét toocustomize hello fejlesztői portál lapok használatával konfigurálhatja a tartalom-sablonok tartalmának.</span><span class="sxs-lookup"><span data-stu-id="4b56b-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="4b56b-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és hello szerkesztő az Ön által választott, például a [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [ A betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), rugalmas lehetőségeket biztosítanak tooconfigure hello hello lapok tartalmát rendelkezik, ezeket a sablonokat igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="4b56b-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="4b56b-106">hello sablonok ebben a szakaszban segítségével toocustomize hello tartalma hello bejelentkezés, jelentkezzen be, és a lap nem található a lapok hello fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="4b56b-106">hello templates in this section allow you toocustomize hello content of hello sign in, sign up, and page not found pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="4b56b-107">bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4b56b-107">Sign in</span></span>](#SignIn)  
  
-   [<span data-ttu-id="4b56b-108">feliratkozni</span><span class="sxs-lookup"><span data-stu-id="4b56b-108">Sign up</span></span>](#SignUp)  
  
-   [<span data-ttu-id="4b56b-109">A lap nem található</span><span class="sxs-lookup"><span data-stu-id="4b56b-109">Page not found</span></span>](#PageNotFound)  
  
> [!NOTE]
>  <span data-ttu-id="4b56b-110">Minta alapértelmezett sablonok a következő dokumentáció hello szerepelnek, de tulajdonos toochange toocontinuous fejlesztései miatt.</span><span class="sxs-lookup"><span data-stu-id="4b56b-110">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="4b56b-111">Navigáljon a szükséges toohello egyéni sablonok hello élő alapértelmezett sablonok a hello fejlesztői portálján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="4b56b-111">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="4b56b-112">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="4b56b-112">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="4b56b-113"><a name="SignIn"></a>bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4b56b-113"><a name="SignIn"></a> Sign in</span></span>  
 <span data-ttu-id="4b56b-114">Hello **bejelentkezés** sablon teszi toocustomize hello bejelentkezési oldal hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="4b56b-114">hello **sign in** template allows you toocustomize hello sign in page in hello developer portal.</span></span>  
  
 <span data-ttu-id="4b56b-115">![Jelentkezzen be az oldal](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM bejelentkezési oldal fejlesztői portál sablonok")</span><span class="sxs-lookup"><span data-stu-id="4b56b-115">![Sign In Page](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM Sign In Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="4b56b-116">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="4b56b-116">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="4b56b-117">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="4b56b-117">Controls</span></span>  
 <span data-ttu-id="4b56b-118">Ez a sablon lehet, hogy használja a hello következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="4b56b-118">This template may  use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="4b56b-119">Basic-bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4b56b-119">basic-signin</span></span>](api-management-page-controls.md#basic-signin)  
  
-   [<span data-ttu-id="4b56b-120">szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="4b56b-120">providers</span></span>](api-management-page-controls.md#providers)  
  
### <a name="data-model"></a><span data-ttu-id="4b56b-121">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="4b56b-121">Data model</span></span>  
 <span data-ttu-id="4b56b-122">[A felhasználói bejelentkezési](api-management-template-data-model-reference.md#UseSignIn) entitás.</span><span class="sxs-lookup"><span data-stu-id="4b56b-122">[User sign in](api-management-template-data-model-reference.md#UseSignIn) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="4b56b-123">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="4b56b-123">Sample template data</span></span>  
  
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
  
##  <span data-ttu-id="4b56b-124"><a name="SignUp"></a>feliratkozni</span><span class="sxs-lookup"><span data-stu-id="4b56b-124"><a name="SignUp"></a> Sign up</span></span>  
 <span data-ttu-id="4b56b-125">Hello **regisztráljon** sablon teszi toocustomize hello bejelentkezési oldal hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="4b56b-125">hello **sign up** template allows you toocustomize hello sign up page in hello developer portal.</span></span>  
  
 <span data-ttu-id="4b56b-126">![Regisztrációs oldalra](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM bejelentkezési oldal Developer-portál sablonok")</span><span class="sxs-lookup"><span data-stu-id="4b56b-126">![Sign Up Page](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM Sign Up Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="4b56b-127">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="4b56b-127">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="4b56b-128">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="4b56b-128">Controls</span></span>  
 <span data-ttu-id="4b56b-129">Ez a sablon lehet, hogy használja a hello következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="4b56b-129">This template may  use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="4b56b-130">-előfizetés</span><span class="sxs-lookup"><span data-stu-id="4b56b-130">sign-up</span></span>](api-management-page-controls.md#sign-up)  
  
### <a name="data-model"></a><span data-ttu-id="4b56b-131">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="4b56b-131">Data model</span></span>  
 <span data-ttu-id="4b56b-132">[Felhasználói bejelentkezési](api-management-template-data-model-reference.md#UserSignUp) entitás.</span><span class="sxs-lookup"><span data-stu-id="4b56b-132">[User sign up](api-management-template-data-model-reference.md#UserSignUp) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="4b56b-133">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="4b56b-133">Sample template data</span></span>  
  
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
  
##  <span data-ttu-id="4b56b-134"><a name="PageNotFound"></a>A lap nem található</span><span class="sxs-lookup"><span data-stu-id="4b56b-134"><a name="PageNotFound"></a> Page not found</span></span>  
 <span data-ttu-id="4b56b-135">Hello **oldal nem található** sablon lehetővé teszi, hogy Ön toocustomize hello oldal nem található oldal hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="4b56b-135">hello **page not found** template allows you toocustomize hello page not found page in hello developer portal.</span></span>  
  
 <span data-ttu-id="4b56b-136">![Nem található az oldal](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM nem található oldal fejlesztői portál sablonok")</span><span class="sxs-lookup"><span data-stu-id="4b56b-136">![Not Found Page](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM Not Found Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="4b56b-137">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="4b56b-137">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="4b56b-138">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="4b56b-138">Controls</span></span>  
 <span data-ttu-id="4b56b-139">Ez a sablon nem használhatja bármelyik [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="4b56b-139">This template may  not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="4b56b-140">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="4b56b-140">Data model</span></span>  
  
|<span data-ttu-id="4b56b-141">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4b56b-141">Property</span></span>|<span data-ttu-id="4b56b-142">Típus</span><span class="sxs-lookup"><span data-stu-id="4b56b-142">Type</span></span>|<span data-ttu-id="4b56b-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="4b56b-143">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="4b56b-144">referenceCode</span><span class="sxs-lookup"><span data-stu-id="4b56b-144">referenceCode</span></span>|<span data-ttu-id="4b56b-145">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4b56b-145">string</span></span>|<span data-ttu-id="4b56b-146">A kód jön létre, ha egy belső hiba eredménye hello meg ezen a lapon.</span><span class="sxs-lookup"><span data-stu-id="4b56b-146">Code generated if this page was displayed as hello result of an internal error.</span></span>|  
|<span data-ttu-id="4b56b-147">Hibakód</span><span class="sxs-lookup"><span data-stu-id="4b56b-147">errorCode</span></span>|<span data-ttu-id="4b56b-148">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4b56b-148">string</span></span>|<span data-ttu-id="4b56b-149">A kód jön létre, ha egy belső hiba eredménye hello meg ezen a lapon.</span><span class="sxs-lookup"><span data-stu-id="4b56b-149">Code generated if this page was displayed as hello result of an internal error.</span></span>|  
|<span data-ttu-id="4b56b-150">emailBody</span><span class="sxs-lookup"><span data-stu-id="4b56b-150">emailBody</span></span>|<span data-ttu-id="4b56b-151">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4b56b-151">string</span></span>|<span data-ttu-id="4b56b-152">Az e-mail törzsének jön létre, ha egy belső hiba eredménye hello meg ezen a lapon.</span><span class="sxs-lookup"><span data-stu-id="4b56b-152">Email body generated if this page was displayed as hello result of an internal error.</span></span>|  
|<span data-ttu-id="4b56b-153">requestedUrl</span><span class="sxs-lookup"><span data-stu-id="4b56b-153">requestedUrl</span></span>|<span data-ttu-id="4b56b-154">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4b56b-154">string</span></span>|<span data-ttu-id="4b56b-155">hello kért URL-cím amikor hello oldal nem található.</span><span class="sxs-lookup"><span data-stu-id="4b56b-155">hello URL requested when hello page was not found.</span></span>|  
|<span data-ttu-id="4b56b-156">referrerUrl</span><span class="sxs-lookup"><span data-stu-id="4b56b-156">referrerUrl</span></span>|<span data-ttu-id="4b56b-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4b56b-157">string</span></span>|<span data-ttu-id="4b56b-158">hello hivatkozó URL-cím toohello kért URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="4b56b-158">hello referrer URL toohello requested URL.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="4b56b-159">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="4b56b-159">Sample template data</span></span>  
  
```json  
{  
    "referenceCode": null,  
    "errorCode": null,  
    "emailBody": null,  
    "requestedUrl": "https://contoso5.portal.azure-api.net:443/NotFoundPage?startEditTemplate=NotFoundPage",  
    "referrerUrl": "https://contoso5.portal.azure-api.net/signup?startEditTemplate=SignUpTemplate"  
}  
```

## <a name="next-steps"></a><span data-ttu-id="4b56b-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4b56b-160">Next steps</span></span>
<span data-ttu-id="4b56b-161">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="4b56b-161">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
