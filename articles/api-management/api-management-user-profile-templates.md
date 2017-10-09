---
title: "AAA \"felhasználói profil sablonok az Azure API Management |} Microsoft dokumentumok\""
description: "Ismerje meg, hogyan toocustomize hello tartalma hello felhasználói profil lapok: az Azure API Management hello developer portálon."
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
ms.openlocfilehash: c8f153b310221164809acf58e4af236928ceb41d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="user-profile-templates-in-azure-api-management"></a><span data-ttu-id="47bfa-103">Felhasználói profil sablonok az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="47bfa-103">User profile templates in Azure API Management</span></span>
<span data-ttu-id="47bfa-104">Az Azure API Management biztosít, akkor hello képességét toocustomize hello fejlesztői portál lapok használatával konfigurálhatja a tartalom-sablonok tartalmának.</span><span class="sxs-lookup"><span data-stu-id="47bfa-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="47bfa-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és hello szerkesztő az Ön által választott, például a [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [ A betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), rugalmas lehetőségeket biztosítanak tooconfigure hello hello lapok tartalmát rendelkezik, ezeket a sablonokat igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="47bfa-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="47bfa-106">Ebben a szakaszban hello sablonok lehetővé teszik hello felhasználói profil lapok tartalmát toocustomize hello hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="47bfa-106">hello templates in this section allow you toocustomize hello content of hello User profile pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="47bfa-107">Profil</span><span class="sxs-lookup"><span data-stu-id="47bfa-107">Profile</span></span>](#Profile)  
  
-   [<span data-ttu-id="47bfa-108">Előfizetések</span><span class="sxs-lookup"><span data-stu-id="47bfa-108">Subscriptions</span></span>](#Subscriptions)  
  
-   [<span data-ttu-id="47bfa-109">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="47bfa-109">Applications</span></span>](#Applications)  
  
-   [<span data-ttu-id="47bfa-110">Fiók adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="47bfa-110">Update account info</span></span>](#UpdateAccountInfo)  
  
> [!NOTE]
>  <span data-ttu-id="47bfa-111">Minta alapértelmezett sablonok a következő dokumentáció hello szerepelnek, de tulajdonos toochange toocontinuous fejlesztései miatt.</span><span class="sxs-lookup"><span data-stu-id="47bfa-111">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="47bfa-112">Navigáljon a szükséges toohello egyéni sablonok hello élő alapértelmezett sablonok a hello fejlesztői portálján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="47bfa-112">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="47bfa-113">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="47bfa-113">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="47bfa-114"><a name="Profile"></a>Profil</span><span class="sxs-lookup"><span data-stu-id="47bfa-114"><a name="Profile"></a> Profile</span></span>  
 <span data-ttu-id="47bfa-115">Hello **profil** sablon lehetővé teszi a toocustomize hello felhasználói profil szakasz hello felhasználói profil lap hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="47bfa-115">hello **profile** template allows you toocustomize hello user profile section of hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="47bfa-116">![Felhasználói profil lap](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM felhasználói profil lap")</span><span class="sxs-lookup"><span data-stu-id="47bfa-116">![User Profile Page](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM User Profile Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="47bfa-117">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="47bfa-117">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="47bfa-118">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="47bfa-118">Controls</span></span>  
 <span data-ttu-id="47bfa-119">Ez a sablon nem használhatja bármelyik [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="47bfa-119">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="47bfa-120">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="47bfa-120">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="47bfa-121">Hello [profil](#Profile), [alkalmazások](#Applications), és [előfizetések](#Subscriptions) sablonok megosztása hello ugyanazokat az adatokat a modell, és fogadni hello sablon ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="47bfa-121">hello [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share hello same data model and receive hello same template data.</span></span>  
  
|<span data-ttu-id="47bfa-122">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="47bfa-122">Property</span></span>|<span data-ttu-id="47bfa-123">Típus</span><span class="sxs-lookup"><span data-stu-id="47bfa-123">Type</span></span>|<span data-ttu-id="47bfa-124">Leírás</span><span class="sxs-lookup"><span data-stu-id="47bfa-124">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="47bfa-125">Utónév</span><span class="sxs-lookup"><span data-stu-id="47bfa-125">firstName</span></span>|<span data-ttu-id="47bfa-126">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-126">string</span></span>|<span data-ttu-id="47bfa-127">Utónév hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-127">First name of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-128">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="47bfa-128">lastName</span></span>|<span data-ttu-id="47bfa-129">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-129">string</span></span>|<span data-ttu-id="47bfa-130">Hello aktuális felhasználó vezetékneve.</span><span class="sxs-lookup"><span data-stu-id="47bfa-130">Last name of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-131">Cégnév</span><span class="sxs-lookup"><span data-stu-id="47bfa-131">companyName</span></span>|<span data-ttu-id="47bfa-132">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-132">string</span></span>|<span data-ttu-id="47bfa-133">a vállalatnév hello hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-133">hello company name of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-134">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="47bfa-134">addresserEmail</span></span>|<span data-ttu-id="47bfa-135">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-135">string</span></span>|<span data-ttu-id="47bfa-136">Hello aktuális felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="47bfa-136">Email address of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-137">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="47bfa-137">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="47bfa-138">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-138">string</span></span>|<span data-ttu-id="47bfa-139">Relatív URL-cím tooview analytics hello aktuális felhasználó esetén.</span><span class="sxs-lookup"><span data-stu-id="47bfa-139">Relative URL tooview analytics for hello current user.</span></span>|  
|<span data-ttu-id="47bfa-140">előfizetések</span><span class="sxs-lookup"><span data-stu-id="47bfa-140">subscriptions</span></span>|<span data-ttu-id="47bfa-141">A gyűjtemény [előfizetés](api-management-template-data-model-reference.md#Subscription) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="47bfa-141">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="47bfa-142">hello előfizetések hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-142">hello subscriptions for hello current user.</span></span>|  
|<span data-ttu-id="47bfa-143">alkalmazások</span><span class="sxs-lookup"><span data-stu-id="47bfa-143">applications</span></span>|<span data-ttu-id="47bfa-144">A gyűjtemény [alkalmazás](api-management-template-data-model-reference.md#Application) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="47bfa-144">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="47bfa-145">hello alkalmazások hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-145">hello applications of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-146">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="47bfa-146">changePasswordUrl</span></span>|<span data-ttu-id="47bfa-147">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-147">string</span></span>|<span data-ttu-id="47bfa-148">hello relatív URL-cím toochange hello aktuális felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="47bfa-148">hello relative URL toochange hello current user's password.</span></span>|  
|<span data-ttu-id="47bfa-149">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="47bfa-149">changeNameOrEmailUrl</span></span>|<span data-ttu-id="47bfa-150">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-150">string</span></span>|<span data-ttu-id="47bfa-151">hello relatív URL-cím toochange hello név és e-mail hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-151">hello relative URL toochange hello name and email for hello current user.</span></span>|  
|<span data-ttu-id="47bfa-152">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="47bfa-152">canChangePassword</span></span>|<span data-ttu-id="47bfa-153">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="47bfa-153">boolean</span></span>|<span data-ttu-id="47bfa-154">E hello aktuális felhasználó megváltoztathatja a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="47bfa-154">Whether hello current user can change their password.</span></span>|  
|<span data-ttu-id="47bfa-155">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="47bfa-155">isSystemUser</span></span>|<span data-ttu-id="47bfa-156">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="47bfa-156">boolean</span></span>|<span data-ttu-id="47bfa-157">Hello aktuális felhasználónak-e egyik hello beépített [csoportok](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="47bfa-157">Whether hello current user is a member of one of hello built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="47bfa-158">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="47bfa-158">Sample template data</span></span>  
  
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
            "ProductDescription": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
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
            "ProductDescription": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
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
  
##  <span data-ttu-id="47bfa-159"><a name="Subscriptions"></a>Előfizetések</span><span class="sxs-lookup"><span data-stu-id="47bfa-159"><a name="Subscriptions"></a> Subscriptions</span></span>  
 <span data-ttu-id="47bfa-160">Hello **előfizetések** sablon lehetővé teszi a toocustomize hello előfizetések szakasza hello felhasználói profil oldal hello fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="47bfa-160">hello **Subscriptions** template allows you toocustomize hello subscriptions section of hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="47bfa-161">![A felhasználó előfizetéshez lap](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "APIM felhasználói előfizetési lap")</span><span class="sxs-lookup"><span data-stu-id="47bfa-161">![User Subscription Page](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "APIM User Subscription Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="47bfa-162">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="47bfa-162">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="47bfa-163">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="47bfa-163">Controls</span></span>  
 <span data-ttu-id="47bfa-164">Ez a sablon lehet, hogy használja a hello következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="47bfa-164">This template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="47bfa-165">előfizetés-Mégse</span><span class="sxs-lookup"><span data-stu-id="47bfa-165">subscription-cancel</span></span>](api-management-page-controls.md#subscription-cancel)  
  
### <a name="data-model"></a><span data-ttu-id="47bfa-166">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="47bfa-166">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="47bfa-167">Hello [profil](#Profile), [alkalmazások](#Applications), és [előfizetések](#Subscriptions) sablonok megosztása hello ugyanazokat az adatokat a modell, és fogadni hello sablon ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="47bfa-167">hello [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share hello same data model and receive hello same template data.</span></span>  
  
|<span data-ttu-id="47bfa-168">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="47bfa-168">Property</span></span>|<span data-ttu-id="47bfa-169">Típus</span><span class="sxs-lookup"><span data-stu-id="47bfa-169">Type</span></span>|<span data-ttu-id="47bfa-170">Leírás</span><span class="sxs-lookup"><span data-stu-id="47bfa-170">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="47bfa-171">Utónév</span><span class="sxs-lookup"><span data-stu-id="47bfa-171">firstName</span></span>|<span data-ttu-id="47bfa-172">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-172">string</span></span>|<span data-ttu-id="47bfa-173">Utónév hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-173">First name of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-174">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="47bfa-174">lastName</span></span>|<span data-ttu-id="47bfa-175">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-175">string</span></span>|<span data-ttu-id="47bfa-176">Hello aktuális felhasználó vezetékneve.</span><span class="sxs-lookup"><span data-stu-id="47bfa-176">Last name of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-177">Cégnév</span><span class="sxs-lookup"><span data-stu-id="47bfa-177">companyName</span></span>|<span data-ttu-id="47bfa-178">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-178">string</span></span>|<span data-ttu-id="47bfa-179">a vállalatnév hello hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-179">hello company name of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-180">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="47bfa-180">addresserEmail</span></span>|<span data-ttu-id="47bfa-181">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-181">string</span></span>|<span data-ttu-id="47bfa-182">Hello aktuális felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="47bfa-182">Email address of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-183">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="47bfa-183">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="47bfa-184">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-184">string</span></span>|<span data-ttu-id="47bfa-185">Relatív URL-cím tooview analytics hello aktuális felhasználó esetén.</span><span class="sxs-lookup"><span data-stu-id="47bfa-185">Relative URL tooview analytics for hello current user.</span></span>|  
|<span data-ttu-id="47bfa-186">előfizetések</span><span class="sxs-lookup"><span data-stu-id="47bfa-186">subscriptions</span></span>|<span data-ttu-id="47bfa-187">A gyűjtemény [előfizetés](api-management-template-data-model-reference.md#Subscription) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="47bfa-187">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="47bfa-188">hello előfizetések hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-188">hello subscriptions for hello current user.</span></span>|  
|<span data-ttu-id="47bfa-189">alkalmazások</span><span class="sxs-lookup"><span data-stu-id="47bfa-189">applications</span></span>|<span data-ttu-id="47bfa-190">A gyűjtemény [alkalmazás](api-management-template-data-model-reference.md#Application) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="47bfa-190">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="47bfa-191">hello alkalmazások hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-191">hello applications of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-192">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="47bfa-192">changePasswordUrl</span></span>|<span data-ttu-id="47bfa-193">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-193">string</span></span>|<span data-ttu-id="47bfa-194">hello relatív URL-cím toochange hello aktuális felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="47bfa-194">hello relative URL toochange hello current user's password.</span></span>|  
|<span data-ttu-id="47bfa-195">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="47bfa-195">changeNameOrEmailUrl</span></span>|<span data-ttu-id="47bfa-196">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-196">string</span></span>|<span data-ttu-id="47bfa-197">hello relatív URL-cím toochange hello név és e-mail hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-197">hello relative URL toochange hello name and email for hello current user.</span></span>|  
|<span data-ttu-id="47bfa-198">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="47bfa-198">canChangePassword</span></span>|<span data-ttu-id="47bfa-199">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="47bfa-199">boolean</span></span>|<span data-ttu-id="47bfa-200">E hello aktuális felhasználó megváltoztathatja a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="47bfa-200">Whether hello current user can change their password.</span></span>|  
|<span data-ttu-id="47bfa-201">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="47bfa-201">isSystemUser</span></span>|<span data-ttu-id="47bfa-202">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="47bfa-202">boolean</span></span>|<span data-ttu-id="47bfa-203">Hello aktuális felhasználónak-e egyik hello beépített [csoportok](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="47bfa-203">Whether hello current user is a member of one of hello built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="47bfa-204">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="47bfa-204">Sample template data</span></span>  
  
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
            "ProductDescription": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
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
            "ProductDescription": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
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
  
##  <span data-ttu-id="47bfa-205"><a name="Applications"></a>Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="47bfa-205"><a name="Applications"></a> Applications</span></span>  
 <span data-ttu-id="47bfa-206">Hello **alkalmazások** sablon lehetővé teszi a toocustomize hello előfizetések szakasza hello felhasználói profil oldal hello fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="47bfa-206">hello **Applications** template allows you toocustomize hello subscriptions section of hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="47bfa-207">![Felhasználói fiók alkalmazások lap](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM felhasználóifiók-alkalmazások lap")</span><span class="sxs-lookup"><span data-stu-id="47bfa-207">![User Account Applications Page](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM User Account Applications Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="47bfa-208">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="47bfa-208">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="47bfa-209">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="47bfa-209">Controls</span></span>  
 <span data-ttu-id="47bfa-210">Ez a sablon lehet, hogy használja a hello következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="47bfa-210">This template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="47bfa-211">alkalmazás-műveletek</span><span class="sxs-lookup"><span data-stu-id="47bfa-211">app-actions</span></span>](api-management-page-controls.md#app-actions)  
  
### <a name="data-model"></a><span data-ttu-id="47bfa-212">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="47bfa-212">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="47bfa-213">Hello [profil](#Profile), [alkalmazások](#Applications), és [előfizetések](#Subscriptions) sablonok megosztása hello ugyanazokat az adatokat a modell, és fogadni hello sablon ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="47bfa-213">hello [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share hello same data model and receive hello same template data.</span></span>  
  
|<span data-ttu-id="47bfa-214">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="47bfa-214">Property</span></span>|<span data-ttu-id="47bfa-215">Típus</span><span class="sxs-lookup"><span data-stu-id="47bfa-215">Type</span></span>|<span data-ttu-id="47bfa-216">Leírás</span><span class="sxs-lookup"><span data-stu-id="47bfa-216">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="47bfa-217">Utónév</span><span class="sxs-lookup"><span data-stu-id="47bfa-217">firstName</span></span>|<span data-ttu-id="47bfa-218">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-218">string</span></span>|<span data-ttu-id="47bfa-219">Utónév hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-219">First name of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-220">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="47bfa-220">lastName</span></span>|<span data-ttu-id="47bfa-221">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-221">string</span></span>|<span data-ttu-id="47bfa-222">Hello aktuális felhasználó vezetékneve.</span><span class="sxs-lookup"><span data-stu-id="47bfa-222">Last name of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-223">Cégnév</span><span class="sxs-lookup"><span data-stu-id="47bfa-223">companyName</span></span>|<span data-ttu-id="47bfa-224">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-224">string</span></span>|<span data-ttu-id="47bfa-225">a vállalatnév hello hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-225">hello company name of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-226">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="47bfa-226">addresserEmail</span></span>|<span data-ttu-id="47bfa-227">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-227">string</span></span>|<span data-ttu-id="47bfa-228">Hello aktuális felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="47bfa-228">Email address of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-229">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="47bfa-229">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="47bfa-230">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-230">string</span></span>|<span data-ttu-id="47bfa-231">Relatív URL-cím tooview analytics hello aktuális felhasználó esetén.</span><span class="sxs-lookup"><span data-stu-id="47bfa-231">Relative URL tooview analytics for hello current user.</span></span>|  
|<span data-ttu-id="47bfa-232">előfizetések</span><span class="sxs-lookup"><span data-stu-id="47bfa-232">subscriptions</span></span>|<span data-ttu-id="47bfa-233">A gyűjtemény [előfizetés](api-management-template-data-model-reference.md#Subscription) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="47bfa-233">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="47bfa-234">hello előfizetések hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-234">hello subscriptions for hello current user.</span></span>|  
|<span data-ttu-id="47bfa-235">alkalmazások</span><span class="sxs-lookup"><span data-stu-id="47bfa-235">applications</span></span>|<span data-ttu-id="47bfa-236">A gyűjtemény [alkalmazás](api-management-template-data-model-reference.md#Application) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="47bfa-236">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="47bfa-237">hello alkalmazások hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-237">hello applications of hello current user.</span></span>|  
|<span data-ttu-id="47bfa-238">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="47bfa-238">changePasswordUrl</span></span>|<span data-ttu-id="47bfa-239">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-239">string</span></span>|<span data-ttu-id="47bfa-240">hello relatív URL-cím toochange hello aktuális felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="47bfa-240">hello relative URL toochange hello current user's password.</span></span>|  
|<span data-ttu-id="47bfa-241">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="47bfa-241">changeNameOrEmailUrl</span></span>|<span data-ttu-id="47bfa-242">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47bfa-242">string</span></span>|<span data-ttu-id="47bfa-243">hello relatív URL-cím toochange hello név és e-mail hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="47bfa-243">hello relative URL toochange hello name and email for hello current user.</span></span>|  
|<span data-ttu-id="47bfa-244">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="47bfa-244">canChangePassword</span></span>|<span data-ttu-id="47bfa-245">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="47bfa-245">boolean</span></span>|<span data-ttu-id="47bfa-246">E hello aktuális felhasználó megváltoztathatja a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="47bfa-246">Whether hello current user can change their password.</span></span>|  
|<span data-ttu-id="47bfa-247">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="47bfa-247">isSystemUser</span></span>|<span data-ttu-id="47bfa-248">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="47bfa-248">boolean</span></span>|<span data-ttu-id="47bfa-249">Hello aktuális felhasználónak-e egyik hello beépített [csoportok](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="47bfa-249">Whether hello current user is a member of one of hello built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="47bfa-250">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="47bfa-250">Sample template data</span></span>  
  
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
            "ProductDescription": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
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
            "ProductDescription": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
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
  
##  <span data-ttu-id="47bfa-251"><a name="UpdateAccountInfo"></a>Fiók adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="47bfa-251"><a name="UpdateAccountInfo"></a> Update account info</span></span>  
 <span data-ttu-id="47bfa-252">Hello **Uodate fiók adatainak** sablon lehetővé teszi a toocustomize hello **fiókadatok frissítése** oldal hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="47bfa-252">hello **Uodate account info** template allows you toocustomize hello **Update account information** page in hello developer portal.</span></span>  
  
 <span data-ttu-id="47bfa-253">![Felhasználói fiók adatai lap fejlesztői portál sablonok](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM felhasználói fiók adatai lap fejlesztői portál sablonok")</span><span class="sxs-lookup"><span data-stu-id="47bfa-253">![User Account Info Page Developer Portal Templates](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM User Account Info Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="47bfa-254">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="47bfa-254">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="47bfa-255">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="47bfa-255">Controls</span></span>  
 <span data-ttu-id="47bfa-256">Ez a sablon nem használhatja bármelyik [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="47bfa-256">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="47bfa-257">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="47bfa-257">Data model</span></span>  
 <span data-ttu-id="47bfa-258">[Felhasználói fiók adatainak](api-management-template-data-model-reference.md#UserAccountInfo) entitás.</span><span class="sxs-lookup"><span data-stu-id="47bfa-258">[User account info](api-management-template-data-model-reference.md#UserAccountInfo) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="47bfa-259">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="47bfa-259">Sample template data</span></span>  
  
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

## <a name="next-steps"></a><span data-ttu-id="47bfa-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47bfa-260">Next steps</span></span>
<span data-ttu-id="47bfa-261">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="47bfa-261">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
