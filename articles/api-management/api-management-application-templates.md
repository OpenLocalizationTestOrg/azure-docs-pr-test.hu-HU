---
title: "Az Azure API Management alkalmazássablonok |} Microsoft Docs"
description: "Ismerje meg, hogyan szabhatja testre a fejlesztői portálra az Azure API Management az alkalmazás lapok tartalmát."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: f3122c4d-e10e-4cdf-977b-36e8f4133fc8
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 6d2d44465800219f16866a621d4822614ac9e1dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="application-templates-in-azure-api-management"></a><span data-ttu-id="d80b3-103">Alkalmazássablonok az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="d80b3-103">Application templates in Azure API Management</span></span>
<span data-ttu-id="d80b3-104">Az Azure API Management lehetővé teszi a tartalom developer portálon lapok használatával konfigurálhatja a tartalom-sablonok testreszabása.</span><span class="sxs-lookup"><span data-stu-id="d80b3-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="d80b3-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és az Ön által választott szerkesztőben, mint [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), konfigurálja a tartalmat, a lapok, ahogyan szeretné ezeket a sablonokat használ nagy rugalmasságot biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="d80b3-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="d80b3-106">Ebben a szakaszban a sablonok engedélyezi, hogy testre szabhatja a fejlesztői portálra az alkalmazás lapok tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d80b3-106">The templates in this section allow you to customize the content of the Application pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="d80b3-107">– Alkalmazáslista</span><span class="sxs-lookup"><span data-stu-id="d80b3-107">Application list</span></span>](#ProductList)  
  
-   [<span data-ttu-id="d80b3-108">Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d80b3-108">Application</span></span>](#Application)  
  
> [!NOTE]
>  <span data-ttu-id="d80b3-109">Minta alapértelmezett sablonok az alábbi dokumentáció szerepelnek, de folyamatos fejlesztéseket miatt változhat.</span><span class="sxs-lookup"><span data-stu-id="d80b3-109">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="d80b3-110">Megtekintheti az élő alapértelmezett sablonok a fejlesztői portálra nyissa meg a kívánt egyéni sablonokat.</span><span class="sxs-lookup"><span data-stu-id="d80b3-110">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="d80b3-111">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="d80b3-111">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="d80b3-112"><a name="ProductList"></a>– Alkalmazáslista</span><span class="sxs-lookup"><span data-stu-id="d80b3-112"><a name="ProductList"></a> Application list</span></span>  
 <span data-ttu-id="d80b3-113">A **Alkalmazáslista** sablon lehetővé teszi a fejlesztői portálra az alkalmazás lista lap törzsét testreszabását.</span><span class="sxs-lookup"><span data-stu-id="d80b3-113">The **Application list** template allows you to customize the body of the application list page in the developer portal.</span></span>  
  
 <span data-ttu-id="d80b3-114">![Lista lap Developer portálon alkalmazássablonok](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM lista lap Developer portálon alkalmazássablonok")</span><span class="sxs-lookup"><span data-stu-id="d80b3-114">![Application List Page Developer Portal Templates](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM Application List Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="d80b3-115">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="d80b3-115">Default template</span></span>  
  
```xml  
<div class="row">  
    <div class="col-md-9">  
        <h2>{% localized "AppStrings|WebApplicationsHeader" %}</h2>  
    </div>  
</div>  
<div class="row">  
    <div class="col-md-12">  
    {% if applications.size > 0 %}  
        <ul class="list-unstyled">  
        {% for app in applications %}  
            <li>  
            {% if app.application.icon.url != "" %}  
                <aside>  
                    <a href="/applications/details/{{app.application.id}}"><img src="{{app.application.icon.url}}" alt="App Icon" /></a>  
                </aside>  
            {% endif %}  
                <h3><a href="/applications/details/{{app.application.id}}">{{app.application.title}}</a></h3>  
                {{app.application.description}}  
            </li>     
        {% endfor %}  
        </ul>  
        <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="d80b3-116">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="d80b3-116">Controls</span></span>  
 <span data-ttu-id="d80b3-117">A `Product list` sablon előfordulhat, hogy használja a következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="d80b3-117">The `Product list` template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="d80b3-118">lapozófájl-vezérlő</span><span class="sxs-lookup"><span data-stu-id="d80b3-118">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a><span data-ttu-id="d80b3-119">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="d80b3-119">Data model</span></span>  
  
|<span data-ttu-id="d80b3-120">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d80b3-120">Property</span></span>|<span data-ttu-id="d80b3-121">Típus</span><span class="sxs-lookup"><span data-stu-id="d80b3-121">Type</span></span>|<span data-ttu-id="d80b3-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="d80b3-122">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="d80b3-123">Lapozás</span><span class="sxs-lookup"><span data-stu-id="d80b3-123">Paging</span></span>|<span data-ttu-id="d80b3-124">[Lapozás](api-management-template-data-model-reference.md#Paging) entitás.</span><span class="sxs-lookup"><span data-stu-id="d80b3-124">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="d80b3-125">Az alkalmazások gyűjtemény lapozás adatait.</span><span class="sxs-lookup"><span data-stu-id="d80b3-125">The paging information for the applications collection.</span></span>|  
|<span data-ttu-id="d80b3-126">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="d80b3-126">Applications</span></span>|<span data-ttu-id="d80b3-127">A gyűjtemény [alkalmazás](api-management-template-data-model-reference.md#Application) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="d80b3-127">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="d80b3-128">Az aktuális felhasználó számára látható alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="d80b3-128">The applications visible to the current user.</span></span>|  
|<span data-ttu-id="d80b3-129">CategoryName</span><span class="sxs-lookup"><span data-stu-id="d80b3-129">CategoryName</span></span>|<span data-ttu-id="d80b3-130">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d80b3-130">string</span></span>|<span data-ttu-id="d80b3-131">A kategória az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d80b3-131">The category of application.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="d80b3-132">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="d80b3-132">Sample template data</span></span>  
  
```json  
{  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 1,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "Applications": [  
        {  
            "Application": {  
                "Id": "5702b96fb16653124c8f9ba8",  
                "Title": "Contoso Calculator",  
                "Description": "A simple online calculator.",  
                "Url": null,  
                "Version": null,  
                "Requirements": "Free application with no requirements.",  
                "State": 2,  
                "RegistrationDate": "2016-04-04T18:59:00",  
                "CategoryId": 5,  
                "DeveloperId": "5702b5b0b16653124c8f9ba4",  
                "Attachments": [  
                    {  
                        "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
                        "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
                        "Type": "Icon",  
                        "ContentType": "image/png"  
                    },  
                    {  
                        "UniqueId": "2b4fa5dd-00ff-4a8f-b1b7-51e715849ede",  
                        "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/2b4fa5dd-00ff-4a8f-b1b7-51e715849ede.png",  
                        "Type": "Screenshot",  
                        "ContentType": "image/png"  
                    }  
                ],  
                "Icon": {  
                    "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
                    "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
                    "Type": "Icon",  
                    "ContentType": "image/png"  
                }  
            },  
            "CategoryName": "Finance"  
        }  
    ]  
}  
```  
  
##  <span data-ttu-id="d80b3-133"><a name="Application"></a>Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d80b3-133"><a name="Application"></a> Application</span></span>  
 <span data-ttu-id="d80b3-134">A **alkalmazás** sablon lehetővé teszi a fejlesztői portálra az alkalmazás lap törzsét testreszabását.</span><span class="sxs-lookup"><span data-stu-id="d80b3-134">The **Application** template allows you to customize the body of the application page in the developer portal.</span></span>  
  
 <span data-ttu-id="d80b3-135">![Alkalmazássablonok lap Developer portálon](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM lap Developer portálon alkalmazássablonok")</span><span class="sxs-lookup"><span data-stu-id="d80b3-135">![Application Page Developer Portal Templates](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM Application Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="d80b3-136">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="d80b3-136">Default template</span></span>  
  
```xml  
<h2>{{title}}</h2>  
{% if icon.url != "" %}  
<aside class="applications_aside">  
  <div class="image-placeholder">  
    <img src="{{icon.url}}" alt="Application Icon" />  
  </div>  
</aside>  
{% endif %}  
  
<article>  
  {% if url != "" %}  
    <a target="_blank" href="{{url}}">{{url}}</a>  
  {% endif %}  
  
  <p>{{description}}</p>  
  
  {% if requirements != null %}  
  <h3>{% localized "AppDetailsStrings|WebApplicationsRequirementsHeader" %}</h3>  
  <p>{{requirements}}</p>  
  {% endif %}  
  
  {% if attachments.size > 0 %}  
  <h3>{% localized "AppDetailsStrings|WebApplicationsScreenshotsHeader" %}</h3>  
    {% for screenshot in attachments %}  
      {% if screenshot.type != "Icon" %}  
      <a href="{{screenshot.url}}" data-lightbox="example-set">  
          <img src="/Developer/Applications/Thumbnail?url={{screenshot.url}}" alt='{% localized "AppDetailsStrings|WebApplicationsScreenshotAlt" %}' />  
      </a>  
      {% endif %}  
    {% endfor %}  
  {% endif %}  
</article>  
  
```  
  
### <a name="controls"></a><span data-ttu-id="d80b3-137">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="d80b3-137">Controls</span></span>  
 <span data-ttu-id="d80b3-138">A `Application` sablon nem teszi lehetővé a használata [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="d80b3-138">The `Application` template does not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="d80b3-139">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="d80b3-139">Data model</span></span>  
 <span data-ttu-id="d80b3-140">[Alkalmazás](api-management-template-data-model-reference.md#Application) entitás.</span><span class="sxs-lookup"><span data-stu-id="d80b3-140">[Application](api-management-template-data-model-reference.md#Application) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="d80b3-141">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="d80b3-141">Sample template data</span></span>  
  
```json  
{  
    "Id": "5702b96fb16653124c8f9ba8",  
    "Title": "Contoso Calculator",  
    "Description": "A simple online calculator.",  
    "Url": null,  
    "Version": null,  
    "Requirements": "Free application with no requirements.",  
    "State": 2,  
    "RegistrationDate": "2016-04-04T18:59:00",  
    "CategoryId": 5,  
    "DeveloperId": "5702b5b0b16653124c8f9ba4",  
    "Attachments": [  
        {  
            "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
            "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
            "Type": "Icon",  
            "ContentType": "image/png"  
        },  
        {  
            "UniqueId": "2b4fa5dd-00ff-4a8f-b1b7-51e715849ede",  
            "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/2b4fa5dd-00ff-4a8f-b1b7-51e715849ede.png",  
            "Type": "Screenshot",  
            "ContentType": "image/png"  
        }  
    ],  
    "Icon": {  
        "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
        "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
        "Type": "Icon",  
        "ContentType": "image/png"  
    }  
}  
```

## <a name="next-steps"></a><span data-ttu-id="d80b3-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d80b3-142">Next steps</span></span>
<span data-ttu-id="d80b3-143">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d80b3-143">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>