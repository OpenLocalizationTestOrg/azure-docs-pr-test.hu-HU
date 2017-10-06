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
# <a name="issue-templates-in-azure-api-management"></a>A probléma sablonok az Azure API Management
Az Azure API Management biztosít, akkor hello képességét toocustomize hello fejlesztői portál lapok használatával konfigurálhatja a tartalom-sablonok tartalmának. Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és hello szerkesztő az Ön által választott, például a [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [ A betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), rugalmas lehetőségeket biztosítanak tooconfigure hello hello lapok tartalmát rendelkezik, ezeket a sablonokat igényei szerint.  
  
 Ebben a szakaszban hello sablonok lehetővé teszik hello probléma lapok tartalmát toocustomize hello hello developer portálon.  
  
-   [Problémák listája](#IssueList)  
  
> [!NOTE]
>  Minta alapértelmezett sablonok a következő dokumentáció hello szerepelnek, de tulajdonos toochange toocontinuous fejlesztései miatt. Navigáljon a szükséges toohello egyéni sablonok hello élő alapértelmezett sablonok a hello fejlesztői portálján tekintheti meg. A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).  
  
##  <a name="IssueList"></a>Problémák listája  
 Hello **problémák listája** sablon teszi hello probléma lista lap toocustomize hello törzsében hello developer portálon.  
  
 ![Adja ki a lista fejlesztői portálján](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM probléma lista fejlesztői portálján")  
  
### <a name="default-template"></a>Alapértelmezett sablon  
  
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
  
### <a name="controls"></a>Vezérlők  
 Hello `Issue list` sablon használhat hello következő [vezérlők lapon](api-management-page-controls.md).  
  
-   [lapozófájl-vezérlő](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a>Adatmodell  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Hibák|A gyűjtemény [probléma](api-management-template-data-model-reference.md#Issue) entitásokat.|hello problémák látható toohello aktuális felhasználó.|  
|Lapozás|[Lapozás](api-management-template-data-model-reference.md#Paging) entitás.|hello lapozás információ hello alkalmazások gyűjtemény.|  
|IsAuthenticated|Logikai érték|Hello aktuális felhasználónak-e bejelentkezve toohello fejlesztői portálján.|  
|CanReportIssues|Logikai érték|Hello aktuális felhasználó rendelkezik-e engedélyekkel toofile kapcsolatos problémát.|  
|Keresés|Karakterlánc|Ez a tulajdonság elavult, és nem használható.|  
  
### <a name="sample-template-data"></a>Mintaadatokat sablon  
  
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

## <a name="next-steps"></a>Következő lépések
A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).
