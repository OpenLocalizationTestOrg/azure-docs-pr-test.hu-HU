---
title: "Felhasználói profil sablonok az Azure API Management |} Microsoft Docs"
description: "Ismerje meg, hogyan szabhatja testre a fejlesztői portálra az Azure API Management a felhasználói profil lapok tartalmát."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2e3b73ef-d223-44fe-9280-c3af3fd4a030
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 9a11bd5800068a5725ab2f099043993bff0b28d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="user-profile-templates-in-azure-api-management"></a><span data-ttu-id="eb250-103">Felhasználói profil sablonok az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="eb250-103">User profile templates in Azure API Management</span></span>
<span data-ttu-id="eb250-104">Az Azure API Management lehetővé teszi a tartalom developer portálon lapok használatával konfigurálhatja a tartalom-sablonok testreszabása.</span><span class="sxs-lookup"><span data-stu-id="eb250-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="eb250-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és az Ön által választott szerkesztőben, mint [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), konfigurálja a tartalmat, a lapok, ahogyan szeretné ezeket a sablonokat használ nagy rugalmasságot biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="eb250-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="eb250-106">Ebben a szakaszban a sablonok engedélyezi, hogy testre szabhatja a fejlesztői portálra a felhasználói profil lapok tartalmát.</span><span class="sxs-lookup"><span data-stu-id="eb250-106">The templates in this section allow you to customize the content of the User profile pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="eb250-107">Profil</span><span class="sxs-lookup"><span data-stu-id="eb250-107">Profile</span></span>](#Profile)  
  
-   [<span data-ttu-id="eb250-108">Előfizetések</span><span class="sxs-lookup"><span data-stu-id="eb250-108">Subscriptions</span></span>](#Subscriptions)  
  
-   [<span data-ttu-id="eb250-109">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="eb250-109">Applications</span></span>](#Applications)  
  
-   [<span data-ttu-id="eb250-110">Fiók adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="eb250-110">Update account info</span></span>](#UpdateAccountInfo)  
  
> [!NOTE]
>  <span data-ttu-id="eb250-111">Minta alapértelmezett sablonok az alábbi dokumentáció szerepelnek, de folyamatos fejlesztéseket miatt változhat.</span><span class="sxs-lookup"><span data-stu-id="eb250-111">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="eb250-112">Megtekintheti az élő alapértelmezett sablonok a fejlesztői portálra nyissa meg a kívánt egyéni sablonokat.</span><span class="sxs-lookup"><span data-stu-id="eb250-112">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="eb250-113">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="eb250-113">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="eb250-114"><a name="Profile"></a>Profil</span><span class="sxs-lookup"><span data-stu-id="eb250-114"><a name="Profile"></a> Profile</span></span>  
 <span data-ttu-id="eb250-115">A **profil** sablon lehetővé teszi a felhasználói profil szakaszában a felhasználó oldalon a fejlesztői portálra testreszabását.</span><span class="sxs-lookup"><span data-stu-id="eb250-115">The **profile** template allows you to customize the user profile section of the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="eb250-116">![Felhasználói profil lap](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM felhasználói profil lap")</span><span class="sxs-lookup"><span data-stu-id="eb250-116">![User Profile Page](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM User Profile Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="eb250-117">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="eb250-117">Default template</span></span>  
  
```xml  
<div class="pull-right">  
  {% if canChangePassword == true %}  
  <a class="btn btn-default" id="ChangePassword" role="button" href="{{changePasswordUrl}}">{% localized "UserProfile|ButtonLabelChangePassword" %}</a>  
  {% endif %}  
  <a id="changeAccountInfo" href="{{changeNameOrEmailUrl}}" class="btn btn-default">  
    <span class="glyphicon glyphicon-user"></span>  
    <span>{% localized "UserProfile|ButtonLabelChangeAccountInfo" %}</span>  
  </a>  
</div>  
<h2>{% localized "SubscriptionStrings|PageTitleDeveloperProfile" %}</h2>  
<div class="container-fluid">  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="Email">{% localized "UserProfile|TextboxLabelEmail" %}</label>  
    </div>  
    <div class="col-sm-9" id="Email">{{email}}</div>  
  </div>  
  
  {% if isSystemUser != true %}  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="FirstName">{% localized "UserProfile|TextboxLabelEmailFirstName" %}</label>  
    </div>  
    <div class="col-sm-9" id="FirstName">{{FirstName}}</div>  
  </div>  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="LastName">{% localized "UserProfile|TextboxLabelEmailLastName" %}</label>  
    </div>  
    <div class="col-sm-9" id="LastName">{{LastName}}</div>  
  </div>  
  {% else %}  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="CompanyName">{% localized "UserProfile|TextboxLabelOrganizationName" %}</label>  
    </div>  
    <div class="col-sm-9" id="CompanyName">{{CompanyName}}</div>  
  </div>  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="AddresserEmail">{% localized "UserProfile|TextboxLabelNotificationsSenderEmail" %}</label>  
    </div>  
    <div class="col-sm-9" id="AddresserEmail">{{AddresserEmail}}</div>  
  </div>  
  {% endif %}  
  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="eb250-118">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="eb250-118">Controls</span></span>  
 <span data-ttu-id="eb250-119">Ez a sablon nem használhatja bármelyik [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="eb250-119">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="eb250-120">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="eb250-120">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="eb250-121">A [profil](#Profile), [alkalmazások](#Applications), és [előfizetések](#Subscriptions) sablonok megosztani az azonos adatmodellt, és a sablon azonos adatok fogadására.</span><span class="sxs-lookup"><span data-stu-id="eb250-121">The [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share the same data model and receive the same template data.</span></span>  
  
|<span data-ttu-id="eb250-122">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="eb250-122">Property</span></span>|<span data-ttu-id="eb250-123">Típus</span><span class="sxs-lookup"><span data-stu-id="eb250-123">Type</span></span>|<span data-ttu-id="eb250-124">Leírás</span><span class="sxs-lookup"><span data-stu-id="eb250-124">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="eb250-125">Utónév</span><span class="sxs-lookup"><span data-stu-id="eb250-125">firstName</span></span>|<span data-ttu-id="eb250-126">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-126">string</span></span>|<span data-ttu-id="eb250-127">Az aktuális felhasználó utóneve.</span><span class="sxs-lookup"><span data-stu-id="eb250-127">First name of the current user.</span></span>|  
|<span data-ttu-id="eb250-128">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="eb250-128">lastName</span></span>|<span data-ttu-id="eb250-129">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-129">string</span></span>|<span data-ttu-id="eb250-130">Az aktuális felhasználó vezetékneve.</span><span class="sxs-lookup"><span data-stu-id="eb250-130">Last name of the current user.</span></span>|  
|<span data-ttu-id="eb250-131">Cégnév</span><span class="sxs-lookup"><span data-stu-id="eb250-131">companyName</span></span>|<span data-ttu-id="eb250-132">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-132">string</span></span>|<span data-ttu-id="eb250-133">A vállalat nevét az aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="eb250-133">The company name of the current user.</span></span>|  
|<span data-ttu-id="eb250-134">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="eb250-134">addresserEmail</span></span>|<span data-ttu-id="eb250-135">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-135">string</span></span>|<span data-ttu-id="eb250-136">Az aktuális felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="eb250-136">Email address of the current user.</span></span>|  
|<span data-ttu-id="eb250-137">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="eb250-137">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="eb250-138">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-138">string</span></span>|<span data-ttu-id="eb250-139">Relatív URL-címe az aktuális felhasználó analytics megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="eb250-139">Relative URL to view analytics for the current user.</span></span>|  
|<span data-ttu-id="eb250-140">előfizetések</span><span class="sxs-lookup"><span data-stu-id="eb250-140">subscriptions</span></span>|<span data-ttu-id="eb250-141">A gyűjtemény [előfizetés](api-management-template-data-model-reference.md#Subscription) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="eb250-141">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="eb250-142">Az aktuális felhasználó előfizetések.</span><span class="sxs-lookup"><span data-stu-id="eb250-142">The subscriptions for the current user.</span></span>|  
|<span data-ttu-id="eb250-143">alkalmazások</span><span class="sxs-lookup"><span data-stu-id="eb250-143">applications</span></span>|<span data-ttu-id="eb250-144">A gyűjtemény [alkalmazás](api-management-template-data-model-reference.md#Application) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="eb250-144">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="eb250-145">Az alkalmazások, az aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="eb250-145">The applications of the current user.</span></span>|  
|<span data-ttu-id="eb250-146">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="eb250-146">changePasswordUrl</span></span>|<span data-ttu-id="eb250-147">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-147">string</span></span>|<span data-ttu-id="eb250-148">A relatív URL-cím módosítása az aktuális felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="eb250-148">The relative URL to change the current user's password.</span></span>|  
|<span data-ttu-id="eb250-149">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="eb250-149">changeNameOrEmailUrl</span></span>|<span data-ttu-id="eb250-150">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-150">string</span></span>|<span data-ttu-id="eb250-151">A relatív URL-címet nevének és e-mailek, az aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="eb250-151">The relative URL to change the name and email for the current user.</span></span>|  
|<span data-ttu-id="eb250-152">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="eb250-152">canChangePassword</span></span>|<span data-ttu-id="eb250-153">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="eb250-153">boolean</span></span>|<span data-ttu-id="eb250-154">Hogy az aktuális felhasználó is módosíthatják jelszavukat.</span><span class="sxs-lookup"><span data-stu-id="eb250-154">Whether the current user can change their password.</span></span>|  
|<span data-ttu-id="eb250-155">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="eb250-155">isSystemUser</span></span>|<span data-ttu-id="eb250-156">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="eb250-156">boolean</span></span>|<span data-ttu-id="eb250-157">Az aktuális felhasználónak-e a beépített egyik tagja [csoportok](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="eb250-157">Whether the current user is a member of one of the built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="eb250-158">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="eb250-158">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="eb250-159"><a name="Subscriptions"></a>Előfizetések</span><span class="sxs-lookup"><span data-stu-id="eb250-159"><a name="Subscriptions"></a> Subscriptions</span></span>  
 <span data-ttu-id="eb250-160">A **előfizetések** sablon lehetővé teszi a felhasználó oldalon a fejlesztői portálra előfizetések szakasza testreszabását.</span><span class="sxs-lookup"><span data-stu-id="eb250-160">The **Subscriptions** template allows you to customize the subscriptions section of the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="eb250-161">![A felhasználó előfizetéshez lap](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "APIM felhasználói előfizetési lap")</span><span class="sxs-lookup"><span data-stu-id="eb250-161">![User Subscription Page](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "APIM User Subscription Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="eb250-162">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="eb250-162">Default template</span></span>  
  
```xml  
<div class="ap-account-subscriptions">  
  <a href="{{developersUsageStatisticsLink}}" id="UsageStatistics" class="btn btn-default pull-right">  
    <span class="glyphicon glyphicon-stats"></span>  
    <span>{% localized "SubscriptionListStrings|WebDevelopersUsageStatisticsLink" %}</span>  
  </a>  
  
  <h2>{% localized "SubscriptionListStrings|WebDevelopersYourSubscriptions" %}</h2>  
  
  <table class="table">  
    <thead>  
      <tr>  
        <th>Subscription details</th>  
        <th>Product</th>  
        <th>{% localized "SubscriptionListStrings|WebDevelopersSubscriptionTableStateHeader" %}</th>  
        <th>Action</th>  
      </tr>  
    </thead>  
    <tbody>  
      {% if subscriptions.size == 0 %}  
      <tr>  
        <td class="text-center" colspan="4">  
          {% localized "CommonResources|NoItemsToDisplay" %}  
        </td>  
      </tr>  
      {% else %}  
      {% for subscription in subscriptions %}  
      <tr id="{{subscription.id}}" {% if subscription.hasExpired %} class="expired" {% endif %}>  
        <td>  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelName" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.displayName }}  
            </div>  
            <div class="col-lg-2">  
              <a class="btn-link" href="/Subscriptions/{{subscription.id}}/Rename">Rename</a>  
            </div>  
            <div class="clearfix"></div>  
          </div>  
          {% if subscription.isAwaitingApproval %}  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelRequestedDate" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.createdDate | date:"MM/dd/yyyy" }}  
            </div>  
          </div>  
          {% else %}  
          {% if subscription.isRejected == false %}  
          {% if subscription.startDate %}  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelStartedDate" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.startDate | date:"MM/dd/yyyy" }}  
            </div>  
          </div>  
          {% endif %}  
  
          <!-- ko with: Developers.Account.Root.account.key('{{subscription.primaryKey}}', '{{subscription.id}}', true) -->  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|WebDevelopersPrimaryKey" %}</label>  
            <div class="col-lg-6">  
              <code data-bind="text: $data.displayKey()" id="primary_{{subscription.id}}"></code>  
            </div>  
            <div class="col-lg-2">  
              <!-- ko if: !requestInProgress() -->  
              <div class="nowrap">  
                <a href="#" class="btn-link" id="togglePrimary_{{subscription.id}}" data-bind="click: toggleKeyDisplay, text: toggleKeyLabel"></a>  
                |  
                <a href="#" class="btn-link" id="regeneratePrimary_{{subscription.id}}" data-bind="click: regenerateKey, text: regenerateKeyLabel"></a>  
              </div>  
              <!-- /ko -->  
              <!-- ko if: requestInProgress() -->  
              <div class="progress progress-striped active">  
                <div class="progress-bar" role="progressbar" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100" style="width: 100%">  
                  <span class="sr-only"></span>  
                </div>  
              </div>  
              <!-- /ko -->  
            </div>  
            <div class="clearfix"></div>  
          </div>  
          <!-- /ko -->  
          <!-- ko with: Developers.Account.Root.account.key('{{subscription.secondaryKey}}', '{{subscription.id}}', false) -->  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|WebDevelopersSecondaryKey" %}</label>  
            <div class="col-lg-6">  
              <code data-bind="text: $data.displayKey()" id="secondary_{{subscription.id}}"></code>  
            </div>  
            <div class="col-lg-2">  
              <div class="nowrap">  
                <a href="#" class="btn-link" id="toggleSecondary_{{subscription.id}}" data-bind="click: toggleKeyDisplay, text: toggleKeyLabel">{% localized "SubscriptionListStrings|ButtonLabelShowKey" %}</a>  
                |  
                <a href="#" class="btn-link" id="regenerateSecondary_{{subscription.id}}" data-bind="click: regenerateKey, text: regenerateKeyLabel">{% localized "SubscriptionListStrings|WebDevelopersRegenerateLink" %}</a>  
              </div>  
            </div>  
            <div class="clearfix"> </div>  
          </div>  
          <!-- /ko -->  
          {% endif %}  
          {% endif %}  
        </td>  
        <td>  
          <a href="{{subscription.productDetailsUrl}}">{{subscription.productTitle}}</a>  
        </td>  
        <td>  
          <strong>  
            {{subscription.state}}  
          </strong>  
        </td>  
        <td>  
          <div class="nowrap">  
            {% if subscription.canBeCancelled %}  
            <subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }"></subscription-cancel>  
            {% endif %}  
          </div>  
        </td>  
      </tr>  
      {% endfor %}  
      {% endif %}  
    </tbody>  
  </table>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="eb250-163">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="eb250-163">Controls</span></span>  
 <span data-ttu-id="eb250-164">Ez a sablon lehet, hogy használja a következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="eb250-164">This template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="eb250-165">előfizetés-Mégse</span><span class="sxs-lookup"><span data-stu-id="eb250-165">subscription-cancel</span></span>](api-management-page-controls.md#subscription-cancel)  
  
### <a name="data-model"></a><span data-ttu-id="eb250-166">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="eb250-166">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="eb250-167">A [profil](#Profile), [alkalmazások](#Applications), és [előfizetések](#Subscriptions) sablonok megosztani az azonos adatmodellt, és a sablon azonos adatok fogadására.</span><span class="sxs-lookup"><span data-stu-id="eb250-167">The [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share the same data model and receive the same template data.</span></span>  
  
|<span data-ttu-id="eb250-168">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="eb250-168">Property</span></span>|<span data-ttu-id="eb250-169">Típus</span><span class="sxs-lookup"><span data-stu-id="eb250-169">Type</span></span>|<span data-ttu-id="eb250-170">Leírás</span><span class="sxs-lookup"><span data-stu-id="eb250-170">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="eb250-171">Utónév</span><span class="sxs-lookup"><span data-stu-id="eb250-171">firstName</span></span>|<span data-ttu-id="eb250-172">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-172">string</span></span>|<span data-ttu-id="eb250-173">Az aktuális felhasználó utóneve.</span><span class="sxs-lookup"><span data-stu-id="eb250-173">First name of the current user.</span></span>|  
|<span data-ttu-id="eb250-174">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="eb250-174">lastName</span></span>|<span data-ttu-id="eb250-175">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-175">string</span></span>|<span data-ttu-id="eb250-176">Az aktuális felhasználó vezetékneve.</span><span class="sxs-lookup"><span data-stu-id="eb250-176">Last name of the current user.</span></span>|  
|<span data-ttu-id="eb250-177">Cégnév</span><span class="sxs-lookup"><span data-stu-id="eb250-177">companyName</span></span>|<span data-ttu-id="eb250-178">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-178">string</span></span>|<span data-ttu-id="eb250-179">A vállalat nevét az aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="eb250-179">The company name of the current user.</span></span>|  
|<span data-ttu-id="eb250-180">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="eb250-180">addresserEmail</span></span>|<span data-ttu-id="eb250-181">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-181">string</span></span>|<span data-ttu-id="eb250-182">Az aktuális felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="eb250-182">Email address of the current user.</span></span>|  
|<span data-ttu-id="eb250-183">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="eb250-183">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="eb250-184">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-184">string</span></span>|<span data-ttu-id="eb250-185">Relatív URL-címe az aktuális felhasználó analytics megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="eb250-185">Relative URL to view analytics for the current user.</span></span>|  
|<span data-ttu-id="eb250-186">előfizetések</span><span class="sxs-lookup"><span data-stu-id="eb250-186">subscriptions</span></span>|<span data-ttu-id="eb250-187">A gyűjtemény [előfizetés](api-management-template-data-model-reference.md#Subscription) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="eb250-187">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="eb250-188">Az aktuális felhasználó előfizetések.</span><span class="sxs-lookup"><span data-stu-id="eb250-188">The subscriptions for the current user.</span></span>|  
|<span data-ttu-id="eb250-189">alkalmazások</span><span class="sxs-lookup"><span data-stu-id="eb250-189">applications</span></span>|<span data-ttu-id="eb250-190">A gyűjtemény [alkalmazás](api-management-template-data-model-reference.md#Application) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="eb250-190">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="eb250-191">Az alkalmazások, az aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="eb250-191">The applications of the current user.</span></span>|  
|<span data-ttu-id="eb250-192">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="eb250-192">changePasswordUrl</span></span>|<span data-ttu-id="eb250-193">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-193">string</span></span>|<span data-ttu-id="eb250-194">A relatív URL-cím módosítása az aktuális felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="eb250-194">The relative URL to change the current user's password.</span></span>|  
|<span data-ttu-id="eb250-195">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="eb250-195">changeNameOrEmailUrl</span></span>|<span data-ttu-id="eb250-196">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-196">string</span></span>|<span data-ttu-id="eb250-197">A relatív URL-címet nevének és e-mailek, az aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="eb250-197">The relative URL to change the name and email for the current user.</span></span>|  
|<span data-ttu-id="eb250-198">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="eb250-198">canChangePassword</span></span>|<span data-ttu-id="eb250-199">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="eb250-199">boolean</span></span>|<span data-ttu-id="eb250-200">Hogy az aktuális felhasználó is módosíthatják jelszavukat.</span><span class="sxs-lookup"><span data-stu-id="eb250-200">Whether the current user can change their password.</span></span>|  
|<span data-ttu-id="eb250-201">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="eb250-201">isSystemUser</span></span>|<span data-ttu-id="eb250-202">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="eb250-202">boolean</span></span>|<span data-ttu-id="eb250-203">Az aktuális felhasználónak-e a beépített egyik tagja [csoportok](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="eb250-203">Whether the current user is a member of one of the built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="eb250-204">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="eb250-204">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="eb250-205"><a name="Applications"></a>Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="eb250-205"><a name="Applications"></a> Applications</span></span>  
 <span data-ttu-id="eb250-206">A **alkalmazások** sablon lehetővé teszi a felhasználó oldalon a fejlesztői portálra előfizetések szakasza testreszabását.</span><span class="sxs-lookup"><span data-stu-id="eb250-206">The **Applications** template allows you to customize the subscriptions section of the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="eb250-207">![Felhasználói fiók alkalmazások lap](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM felhasználóifiók-alkalmazások lap")</span><span class="sxs-lookup"><span data-stu-id="eb250-207">![User Account Applications Page](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM User Account Applications Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="eb250-208">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="eb250-208">Default template</span></span>  
  
```xml  
<div class="ap-account-applications">  
  <a id="RegisterApplication" href="/Developer/Applications/Register" class="btn btn-success pull-right">  
    <span class="glyphicon glyphicon-plus"></span>  
    <span>{% localized "ApplicationListStrings|WebDevelopersRegisterAppLink" %}</span>  
  </a>  
  <h2>{% localized "ApplicationListStrings|WebDevelopersYourApplicationsHeader" %}</h2>  
  
  <table class="table">  
    <thead>  
      <tr>  
        <th class="col-md-8">{% localized "ApplicationListStrings|WebDevelopersAppTableNameHeader" %}</th>  
        <th class="col-md-2">{% localized "ApplicationListStrings|WebDevelopersAppTableCategoryHeader" %}</th>  
        <th class="col-md-2" colspan="2">{% localized "ApplicationListStrings|WebDevelopersAppTableStateHeader" %}</th>  
      </tr>  
    </thead>  
    <tbody>  
  
      {% if applications.size == 0 %}  
  
      <tr>  
        <td class="col-md-12 text-center" colspan="4">  
          {% localized "CommonResources|NoItemsToDisplay" %}  
        </td>  
      </tr>  
  
      {% else %}  
  
      {% for app in applications %}  
      <tr>  
        <td class="col-md-8">  
          {{app.title}}  
        </td>  
        <td class="col-md-2">  
          {{app.categoryName}}  
        </td>  
        <td class="col-md-2">  
          <strong>  
            {% case app.state %}  
            {% when ApplicationStateModel.Registered %}  
            {% localized "ApplicationListStrings|WebDevelopersAppNotSubminted" %}  
  
            {% when ApplicationStateModel.Unpublished %}  
            {% localized "ApplicationListStrings|WebDevelopersAppNotPublished" %}  
  
            {% else %}  
            {{ app.state }}  
            {% endcase %}  
          </strong>  
        </td>  
        <td class="col-md-1">  
          <div class="nowrap">  
            {% if app.state != ApplicationStateModel.Submitted and app.state != ApplicationStateModel.Published %}  
            <app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
            {% endif %}  
          </div>  
        </td>  
      </tr>  
      {% endfor %}  
  
      {% endif %}  
    </tbody>  
  </table>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="eb250-209">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="eb250-209">Controls</span></span>  
 <span data-ttu-id="eb250-210">Ez a sablon lehet, hogy használja a következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="eb250-210">This template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="eb250-211">alkalmazás-műveletek</span><span class="sxs-lookup"><span data-stu-id="eb250-211">app-actions</span></span>](api-management-page-controls.md#app-actions)  
  
### <a name="data-model"></a><span data-ttu-id="eb250-212">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="eb250-212">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="eb250-213">A [profil](#Profile), [alkalmazások](#Applications), és [előfizetések](#Subscriptions) sablonok megosztani az azonos adatmodellt, és a sablon azonos adatok fogadására.</span><span class="sxs-lookup"><span data-stu-id="eb250-213">The [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share the same data model and receive the same template data.</span></span>  
  
|<span data-ttu-id="eb250-214">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="eb250-214">Property</span></span>|<span data-ttu-id="eb250-215">Típus</span><span class="sxs-lookup"><span data-stu-id="eb250-215">Type</span></span>|<span data-ttu-id="eb250-216">Leírás</span><span class="sxs-lookup"><span data-stu-id="eb250-216">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="eb250-217">Utónév</span><span class="sxs-lookup"><span data-stu-id="eb250-217">firstName</span></span>|<span data-ttu-id="eb250-218">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-218">string</span></span>|<span data-ttu-id="eb250-219">Az aktuális felhasználó utóneve.</span><span class="sxs-lookup"><span data-stu-id="eb250-219">First name of the current user.</span></span>|  
|<span data-ttu-id="eb250-220">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="eb250-220">lastName</span></span>|<span data-ttu-id="eb250-221">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-221">string</span></span>|<span data-ttu-id="eb250-222">Az aktuális felhasználó vezetékneve.</span><span class="sxs-lookup"><span data-stu-id="eb250-222">Last name of the current user.</span></span>|  
|<span data-ttu-id="eb250-223">Cégnév</span><span class="sxs-lookup"><span data-stu-id="eb250-223">companyName</span></span>|<span data-ttu-id="eb250-224">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-224">string</span></span>|<span data-ttu-id="eb250-225">A vállalat nevét az aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="eb250-225">The company name of the current user.</span></span>|  
|<span data-ttu-id="eb250-226">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="eb250-226">addresserEmail</span></span>|<span data-ttu-id="eb250-227">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-227">string</span></span>|<span data-ttu-id="eb250-228">Az aktuális felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="eb250-228">Email address of the current user.</span></span>|  
|<span data-ttu-id="eb250-229">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="eb250-229">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="eb250-230">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-230">string</span></span>|<span data-ttu-id="eb250-231">Relatív URL-címe az aktuális felhasználó analytics megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="eb250-231">Relative URL to view analytics for the current user.</span></span>|  
|<span data-ttu-id="eb250-232">előfizetések</span><span class="sxs-lookup"><span data-stu-id="eb250-232">subscriptions</span></span>|<span data-ttu-id="eb250-233">A gyűjtemény [előfizetés](api-management-template-data-model-reference.md#Subscription) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="eb250-233">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="eb250-234">Az aktuális felhasználó előfizetések.</span><span class="sxs-lookup"><span data-stu-id="eb250-234">The subscriptions for the current user.</span></span>|  
|<span data-ttu-id="eb250-235">alkalmazások</span><span class="sxs-lookup"><span data-stu-id="eb250-235">applications</span></span>|<span data-ttu-id="eb250-236">A gyűjtemény [alkalmazás](api-management-template-data-model-reference.md#Application) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="eb250-236">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="eb250-237">Az alkalmazások, az aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="eb250-237">The applications of the current user.</span></span>|  
|<span data-ttu-id="eb250-238">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="eb250-238">changePasswordUrl</span></span>|<span data-ttu-id="eb250-239">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-239">string</span></span>|<span data-ttu-id="eb250-240">A relatív URL-cím módosítása az aktuális felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="eb250-240">The relative URL to change the current user's password.</span></span>|  
|<span data-ttu-id="eb250-241">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="eb250-241">changeNameOrEmailUrl</span></span>|<span data-ttu-id="eb250-242">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="eb250-242">string</span></span>|<span data-ttu-id="eb250-243">A relatív URL-címet nevének és e-mailek, az aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="eb250-243">The relative URL to change the name and email for the current user.</span></span>|  
|<span data-ttu-id="eb250-244">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="eb250-244">canChangePassword</span></span>|<span data-ttu-id="eb250-245">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="eb250-245">boolean</span></span>|<span data-ttu-id="eb250-246">Hogy az aktuális felhasználó is módosíthatják jelszavukat.</span><span class="sxs-lookup"><span data-stu-id="eb250-246">Whether the current user can change their password.</span></span>|  
|<span data-ttu-id="eb250-247">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="eb250-247">isSystemUser</span></span>|<span data-ttu-id="eb250-248">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="eb250-248">boolean</span></span>|<span data-ttu-id="eb250-249">Az aktuális felhasználónak-e a beépített egyik tagja [csoportok](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="eb250-249">Whether the current user is a member of one of the built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="eb250-250">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="eb250-250">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="eb250-251"><a name="UpdateAccountInfo"></a>Fiók adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="eb250-251"><a name="UpdateAccountInfo"></a> Update account info</span></span>  
 <span data-ttu-id="eb250-252">A **Uodate fiók adatainak** sablon teszi lehetővé testre szabhatja a **fiókadatok frissítése** oldal az developer portálon.</span><span class="sxs-lookup"><span data-stu-id="eb250-252">The **Uodate account info** template allows you to customize the **Update account information** page in the developer portal.</span></span>  
  
 <span data-ttu-id="eb250-253">![Felhasználói fiók adatai lap fejlesztői portál sablonok](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM felhasználói fiók adatai lap fejlesztői portál sablonok")</span><span class="sxs-lookup"><span data-stu-id="eb250-253">![User Account Info Page Developer Portal Templates](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM User Account Info Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="eb250-254">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="eb250-254">Default template</span></span>  
  
```xml  
<div class="row">  
  <div class="col-sm-6 col-md-6">  
    <div class="form-group">  
      <label for="Email">{% localized "SigninResources|TextboxLabelEmail" %}</label>  
      <input autofocus="autofocus" class="form-control" id="Email" name="Email" type="text" value="{{email}}">  
    </div>  
    <div class="form-group">  
      <label for="FirstName">{% localized "SigninResources|TextboxLabelEmailFirstName" %}</label>  
      <input class="form-control" id="FirstName" name="FirstName" type="text" value="{{firstName}}">  
    </div>  
    <div class="form-group">  
      <label for="LastName">{% localized "SigninResources|TextboxLabelEmailLastName" %}</label>  
      <input class="form-control" id="LastName" name="LastName" type="text" value="{{lastName}}">  
    </div>  
    <div class="form-group">  
      <label for="Password">{% localized "SigninResources|WebAuthenticationSigninPasswordLabel" %}</label>  
      <input class="form-control" id="Password" name="Password" type="password">  
    </div>  
  </div>  
</div>  
  
<button type="submit" class="btn btn-primary" id="UpdateProfile">  
  {% localized "UpdateProfileStrings|ButtonLabelUpdateProfile" %}  
</button>  
<a class="btn btn-default" href="/developer" role="button">  
  {% localized "CommonStrings|ButtonLabelCancel" %}  
</a>  
```  
  
### <a name="controls"></a><span data-ttu-id="eb250-255">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="eb250-255">Controls</span></span>  
 <span data-ttu-id="eb250-256">Ez a sablon nem használhatja bármelyik [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="eb250-256">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="eb250-257">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="eb250-257">Data model</span></span>  
 <span data-ttu-id="eb250-258">[Felhasználói fiók adatainak](api-management-template-data-model-reference.md#UserAccountInfo) entitás.</span><span class="sxs-lookup"><span data-stu-id="eb250-258">[User account info](api-management-template-data-model-reference.md#UserAccountInfo) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="eb250-259">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="eb250-259">Sample template data</span></span>  
  
```json  
{  
    "FirstName": "Administrator",  
    "LastName": "",  
    "Email": "admin@live.com",  
    "Password": null,  
    "NameIdentifier": null,  
    "ProviderName": null,  
    "IsBasicAccount": false  
}  
```

## <a name="next-steps"></a><span data-ttu-id="eb250-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eb250-260">Next steps</span></span>
<span data-ttu-id="eb250-261">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="eb250-261">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>