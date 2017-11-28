---
title: az Azure API Management aaaIssue sablonok |} Microsoft Docs
description: "Ismerje meg, hogyan toocustomize hello hello developer portálon az Azure API Management hello probléma lapok tartalmát."
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
ms.openlocfilehash: e12902e52c164f73902a97f15ea550790dfecf1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="issue-templates-in-azure-api-management"></a><span data-ttu-id="9ebd1-103">A probléma sablonok az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="9ebd1-103">Issue templates in Azure API Management</span></span>
<span data-ttu-id="9ebd1-104">Az Azure API Management biztosít, akkor hello képességét toocustomize hello fejlesztői portál lapok használatával konfigurálhatja a tartalom-sablonok tartalmának.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="9ebd1-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és hello szerkesztő az Ön által választott, például a [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [ A betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), rugalmas lehetőségeket biztosítanak tooconfigure hello hello lapok tartalmát rendelkezik, ezeket a sablonokat igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="9ebd1-106">Ebben a szakaszban hello sablonok lehetővé teszik hello probléma lapok tartalmát toocustomize hello hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-106">hello templates in this section allow you toocustomize hello content of hello Issue pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="9ebd1-107">Problémák listája</span><span class="sxs-lookup"><span data-stu-id="9ebd1-107">Issue list</span></span>](#IssueList)  
  
> [!NOTE]
>  <span data-ttu-id="9ebd1-108">Minta alapértelmezett sablonok a következő dokumentáció hello szerepelnek, de tulajdonos toochange toocontinuous fejlesztései miatt.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-108">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="9ebd1-109">Navigáljon a szükséges toohello egyéni sablonok hello élő alapértelmezett sablonok a hello fejlesztői portálján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-109">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="9ebd1-110">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9ebd1-110">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>  
  
##  <span data-ttu-id="9ebd1-111"><a name="IssueList"></a>Problémák listája</span><span class="sxs-lookup"><span data-stu-id="9ebd1-111"><a name="IssueList"></a> Issue list</span></span>  
 <span data-ttu-id="9ebd1-112">Hello **problémák listája** sablon teszi hello probléma lista lap toocustomize hello törzsében hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-112">hello **Issue list** template allows you toocustomize hello body of hello issue list page in hello developer portal.</span></span>  
  
 <span data-ttu-id="9ebd1-113">![Adja ki a lista fejlesztői portálján](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM probléma lista fejlesztői portálján")</span><span class="sxs-lookup"><span data-stu-id="9ebd1-113">![Issue List Developer Portal](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM Issue List Developer Portal")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="9ebd1-114">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="9ebd1-114">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="9ebd1-115">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="9ebd1-115">Controls</span></span>  
 <span data-ttu-id="9ebd1-116">Hello `Issue list` sablon használhat hello következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="9ebd1-116">hello `Issue list` template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="9ebd1-117">lapozófájl-vezérlő</span><span class="sxs-lookup"><span data-stu-id="9ebd1-117">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a><span data-ttu-id="9ebd1-118">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="9ebd1-118">Data model</span></span>  
  
|<span data-ttu-id="9ebd1-119">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9ebd1-119">Property</span></span>|<span data-ttu-id="9ebd1-120">Típus</span><span class="sxs-lookup"><span data-stu-id="9ebd1-120">Type</span></span>|<span data-ttu-id="9ebd1-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="9ebd1-121">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="9ebd1-122">Hibák</span><span class="sxs-lookup"><span data-stu-id="9ebd1-122">Issues</span></span>|<span data-ttu-id="9ebd1-123">A gyűjtemény [probléma](api-management-template-data-model-reference.md#Issue) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-123">Collection of [Issue](api-management-template-data-model-reference.md#Issue) entities.</span></span>|<span data-ttu-id="9ebd1-124">hello problémák látható toohello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-124">hello issues visible toohello current user.</span></span>|  
|<span data-ttu-id="9ebd1-125">Lapozás</span><span class="sxs-lookup"><span data-stu-id="9ebd1-125">Paging</span></span>|<span data-ttu-id="9ebd1-126">[Lapozás](api-management-template-data-model-reference.md#Paging) entitás.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-126">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="9ebd1-127">hello lapozás információ hello alkalmazások gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-127">hello paging information for hello applications collection.</span></span>|  
|<span data-ttu-id="9ebd1-128">IsAuthenticated</span><span class="sxs-lookup"><span data-stu-id="9ebd1-128">IsAuthenticated</span></span>|<span data-ttu-id="9ebd1-129">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="9ebd1-129">boolean</span></span>|<span data-ttu-id="9ebd1-130">Hello aktuális felhasználónak-e bejelentkezve toohello fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-130">Whether hello current user is signed-in toohello developer portal.</span></span>|  
|<span data-ttu-id="9ebd1-131">CanReportIssues</span><span class="sxs-lookup"><span data-stu-id="9ebd1-131">CanReportIssues</span></span>|<span data-ttu-id="9ebd1-132">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="9ebd1-132">boolean</span></span>|<span data-ttu-id="9ebd1-133">Hello aktuális felhasználó rendelkezik-e engedélyekkel toofile kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-133">Whether hello current user has permissions toofile an issue.</span></span>|  
|<span data-ttu-id="9ebd1-134">Keresés</span><span class="sxs-lookup"><span data-stu-id="9ebd1-134">Search</span></span>|<span data-ttu-id="9ebd1-135">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9ebd1-135">string</span></span>|<span data-ttu-id="9ebd1-136">Ez a tulajdonság elavult, és nem használható.</span><span class="sxs-lookup"><span data-stu-id="9ebd1-136">This property is deprecated and should not be used.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="9ebd1-137">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="9ebd1-137">Sample template data</span></span>  
  
```json  
{  
    "Issues": [  
        {  
            "Id": "5702b68bb16653124c8f9ba7",  
            "ApiId": "570275f1b16653124c8f9ba3",  
            "Title": "I couldn't figure out how tooconnect my application toohello API",  
            "Description": "I'm having trouble connecting my application toohello backend API.",  
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

## <a name="next-steps"></a><span data-ttu-id="9ebd1-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ebd1-138">Next steps</span></span>
<span data-ttu-id="9ebd1-139">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9ebd1-139">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
