---
title: Adja ki a sablonok az Azure API Management |} Microsoft Docs
description: "Ismerje meg, hogyan szabhatja testre a fejlesztői portálra az Azure API Management a probléma lapok tartalmát."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 47da4bb2-426e-4e53-8fa7-214ee2e3ab37
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: e13344df198bca4f73c75fa58221436b94e2f258
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="issue-templates-in-azure-api-management"></a><span data-ttu-id="6381a-103">A probléma sablonok az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="6381a-103">Issue templates in Azure API Management</span></span>
<span data-ttu-id="6381a-104">Az Azure API Management lehetővé teszi a tartalom developer portálon lapok használatával konfigurálhatja a tartalom-sablonok testreszabása.</span><span class="sxs-lookup"><span data-stu-id="6381a-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="6381a-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és az Ön által választott szerkesztőben, mint [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), konfigurálja a tartalmat, a lapok, ahogyan szeretné ezeket a sablonokat használ nagy rugalmasságot biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="6381a-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="6381a-106">Ebben a szakaszban a sablonok lehetővé teszi a tartalom a fejlesztői portálra probléma oldalak testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="6381a-106">The templates in this section allow you to customize the content of the Issue pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="6381a-107">Problémák listája</span><span class="sxs-lookup"><span data-stu-id="6381a-107">Issue list</span></span>](#IssueList)  
  
> [!NOTE]
>  <span data-ttu-id="6381a-108">Minta alapértelmezett sablonok az alábbi dokumentáció szerepelnek, de folyamatos fejlesztéseket miatt változhat.</span><span class="sxs-lookup"><span data-stu-id="6381a-108">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="6381a-109">Megtekintheti az élő alapértelmezett sablonok a fejlesztői portálra nyissa meg a kívánt egyéni sablonokat.</span><span class="sxs-lookup"><span data-stu-id="6381a-109">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="6381a-110">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6381a-110">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>  
  
##  <span data-ttu-id="6381a-111"><a name="IssueList"></a>Problémák listája</span><span class="sxs-lookup"><span data-stu-id="6381a-111"><a name="IssueList"></a> Issue list</span></span>  
 <span data-ttu-id="6381a-112">A **problémák listája** sablon lehetővé teszi a fejlesztői portálra, a probléma lista lap törzsében testreszabását.</span><span class="sxs-lookup"><span data-stu-id="6381a-112">The **Issue list** template allows you to customize the body of the issue list page in the developer portal.</span></span>  
  
 <span data-ttu-id="6381a-113">![Adja ki a lista fejlesztői portálján](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM probléma lista fejlesztői portálján")</span><span class="sxs-lookup"><span data-stu-id="6381a-113">![Issue List Developer Portal](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM Issue List Developer Portal")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="6381a-114">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="6381a-114">Default template</span></span>  
  
```xml  
<div class="row">  
  <div class="col-md-9">  
    <h2>{% localized "IssuesStrings|WebIssuesIndexTitle" %}</h2>  
  </div>  
</div>  
<div class="row">  
  <div class="col-md-12">  
    {% if issues.size > 0 %}  
    <ul class="list-unstyled">  
      {% capture reportedBy %}{% localized "IssuesStrings|WebIssuesStatusReportedBy" %}{% endcapture %}  
      {% assign replaceString0 = '{0}' %}  
      {% assign replaceString1 = '{1}' %}  
      {% for issue in issues %}  
      <li>  
        <h3>  
          <a href="/issues/{{issue.id}}">{{issue.title}}</a>  
        </h3>  
        <p>{{issue.description}}</p>  
        <em>  
          {% capture state %}{{issue.issueState}}{% endcapture %}  
          {% capture devName %}{{issue.subscriptionDeveloperName}}{% endcapture %}  
          {% capture str1 %}{{ reportedBy | replace : replaceString0, state }}{% endcapture %}  
          {{ str1 | replace : replaceString1, devName }}  
          <span class="UtcDateElement">{{ issue.reportedOn | date: "r" }}</span>  
        </em>  
      </li>  
      {% endfor %}  
    </ul>  
    <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    {% if canReportIssue %}  
    <a class="btn btn-primary" id="createIssue" href="/Issues/Create">{% localized "IssuesStrings|WebIssuesReportIssueButton" %}</a>  
    {% elsif isAuthenticated %}  
    <hr />  
    <p>{% localized "IssuesStrings|WebIssuesNoActiveSubscriptions" %}</p>  
    {% else %}  
    <hr />  
    <p>  
      {% capture signIntext %}{% localized "IssuesStrings|WebIssuesNotSignin" %}{% endcapture %}  
      {% capture link %}<a href="/signin">{% localized "IssuesStrings|WebIssuesSignIn" %}</a>{% endcapture %}  
      {{ signIntext | replace : replaceString0, link }}  
    </p>  
    {% endif %}  
  </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="6381a-115">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="6381a-115">Controls</span></span>  
 <span data-ttu-id="6381a-116">A `Issue list` sablon előfordulhat, hogy használja a következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="6381a-116">The `Issue list` template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="6381a-117">lapozófájl-vezérlő</span><span class="sxs-lookup"><span data-stu-id="6381a-117">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a><span data-ttu-id="6381a-118">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="6381a-118">Data model</span></span>  
  
|<span data-ttu-id="6381a-119">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6381a-119">Property</span></span>|<span data-ttu-id="6381a-120">Típus</span><span class="sxs-lookup"><span data-stu-id="6381a-120">Type</span></span>|<span data-ttu-id="6381a-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="6381a-121">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="6381a-122">Hibák</span><span class="sxs-lookup"><span data-stu-id="6381a-122">Issues</span></span>|<span data-ttu-id="6381a-123">A gyűjtemény [probléma](api-management-template-data-model-reference.md#Issue) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="6381a-123">Collection of [Issue](api-management-template-data-model-reference.md#Issue) entities.</span></span>|<span data-ttu-id="6381a-124">Az aktuális felhasználó számára látható problémákat.</span><span class="sxs-lookup"><span data-stu-id="6381a-124">The issues visible to the current user.</span></span>|  
|<span data-ttu-id="6381a-125">Lapozás</span><span class="sxs-lookup"><span data-stu-id="6381a-125">Paging</span></span>|<span data-ttu-id="6381a-126">[Lapozás](api-management-template-data-model-reference.md#Paging) entitás.</span><span class="sxs-lookup"><span data-stu-id="6381a-126">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="6381a-127">Az alkalmazások gyűjtemény lapozás adatait.</span><span class="sxs-lookup"><span data-stu-id="6381a-127">The paging information for the applications collection.</span></span>|  
|<span data-ttu-id="6381a-128">IsAuthenticated</span><span class="sxs-lookup"><span data-stu-id="6381a-128">IsAuthenticated</span></span>|<span data-ttu-id="6381a-129">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="6381a-129">boolean</span></span>|<span data-ttu-id="6381a-130">Hogy az aktuális felhasználó van bejelentkezve a fejlesztői portálhoz.</span><span class="sxs-lookup"><span data-stu-id="6381a-130">Whether the current user is signed-in to the developer portal.</span></span>|  
|<span data-ttu-id="6381a-131">CanReportIssues</span><span class="sxs-lookup"><span data-stu-id="6381a-131">CanReportIssues</span></span>|<span data-ttu-id="6381a-132">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="6381a-132">boolean</span></span>|<span data-ttu-id="6381a-133">Az aktuális felhasználónak van-e engedélyekkel a következő fájl kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="6381a-133">Whether the current user has permissions to file an issue.</span></span>|  
|<span data-ttu-id="6381a-134">Keresés</span><span class="sxs-lookup"><span data-stu-id="6381a-134">Search</span></span>|<span data-ttu-id="6381a-135">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="6381a-135">string</span></span>|<span data-ttu-id="6381a-136">Ez a tulajdonság elavult, és nem használható.</span><span class="sxs-lookup"><span data-stu-id="6381a-136">This property is deprecated and should not be used.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="6381a-137">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="6381a-137">Sample template data</span></span>  
  
```json  
{  
    "Issues": [  
        {  
            "Id": "5702b68bb16653124c8f9ba7",  
            "ApiId": "570275f1b16653124c8f9ba3",  
            "Title": "I couldn't figure out how to connect my application to the API",  
            "Description": "I'm having trouble connecting my application to the backend API.",  
            "SubscriptionDeveloperName": "Clayton",  
            "IssueState": "Proposed",  
            "ReportedOn": "2016-04-04T18:46:35.64",  
            "Comments": null,  
            "Attachments": null,  
            "Services": null  
        }  
    ],  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 1,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "IsAuthenticated": true,  
    "CanReportIssue": true,  
    "Search": null  
}  
```

## <a name="next-steps"></a><span data-ttu-id="6381a-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6381a-138">Next steps</span></span>
<span data-ttu-id="6381a-139">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6381a-139">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>