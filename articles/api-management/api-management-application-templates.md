---
title: az Azure API Management aaaApplication sablonok |} Microsoft Docs
description: "Ismerje meg, hogyan toocustomize hello hello lapjain hello developer portálon az Azure API Management tartalmát."
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
ms.openlocfilehash: f4dc078be7163b047ca2e640a9065ba9e5ba709e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-templates-in-azure-api-management"></a><span data-ttu-id="ae968-103">Alkalmazássablonok az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="ae968-103">Application templates in Azure API Management</span></span>
<span data-ttu-id="ae968-104">Az Azure API Management biztosít, akkor hello képességét toocustomize hello fejlesztői portál lapok használatával konfigurálhatja a tartalom-sablonok tartalmának.</span><span class="sxs-lookup"><span data-stu-id="ae968-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="ae968-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és hello szerkesztő az Ön által választott, például a [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [ A betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), rugalmas lehetőségeket biztosítanak tooconfigure hello hello lapok tartalmát rendelkezik, ezeket a sablonokat igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="ae968-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="ae968-106">Ebben a szakaszban hello sablonok lehetővé teszik toocustomize hello tartalma hello lapjain hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="ae968-106">hello templates in this section allow you toocustomize hello content of hello Application pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="ae968-107">– Alkalmazáslista</span><span class="sxs-lookup"><span data-stu-id="ae968-107">Application list</span></span>](#ProductList)  
  
-   [<span data-ttu-id="ae968-108">Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="ae968-108">Application</span></span>](#Application)  
  
> [!NOTE]
>  <span data-ttu-id="ae968-109">Minta alapértelmezett sablonok a következő dokumentáció hello szerepelnek, de tulajdonos toochange toocontinuous fejlesztései miatt.</span><span class="sxs-lookup"><span data-stu-id="ae968-109">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="ae968-110">Navigáljon a szükséges toohello egyéni sablonok hello élő alapértelmezett sablonok a hello fejlesztői portálján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="ae968-110">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="ae968-111">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="ae968-111">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="ae968-112"><a name="ProductList"></a>– Alkalmazáslista</span><span class="sxs-lookup"><span data-stu-id="ae968-112"><a name="ProductList"></a> Application list</span></span>  
 <span data-ttu-id="ae968-113">Hello **Alkalmazáslista** sablon teszi hello alkalmazás lista lap toocustomize hello törzsében hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="ae968-113">hello **Application list** template allows you toocustomize hello body of hello application list page in hello developer portal.</span></span>  
  
 <span data-ttu-id="ae968-114">![Lista lap Developer portálon alkalmazássablonok](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM lista lap Developer portálon alkalmazássablonok")</span><span class="sxs-lookup"><span data-stu-id="ae968-114">![Application List Page Developer Portal Templates](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM Application List Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="ae968-115">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="ae968-115">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="ae968-116">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="ae968-116">Controls</span></span>  
 <span data-ttu-id="ae968-117">Hello `Product list` sablon használhat hello következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="ae968-117">hello `Product list` template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="ae968-118">lapozófájl-vezérlő</span><span class="sxs-lookup"><span data-stu-id="ae968-118">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a><span data-ttu-id="ae968-119">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="ae968-119">Data model</span></span>  
  
|<span data-ttu-id="ae968-120">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ae968-120">Property</span></span>|<span data-ttu-id="ae968-121">Típus</span><span class="sxs-lookup"><span data-stu-id="ae968-121">Type</span></span>|<span data-ttu-id="ae968-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="ae968-122">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="ae968-123">Lapozás</span><span class="sxs-lookup"><span data-stu-id="ae968-123">Paging</span></span>|<span data-ttu-id="ae968-124">[Lapozás](api-management-template-data-model-reference.md#Paging) entitás.</span><span class="sxs-lookup"><span data-stu-id="ae968-124">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="ae968-125">hello lapozás információ hello alkalmazások gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="ae968-125">hello paging information for hello applications collection.</span></span>|  
|<span data-ttu-id="ae968-126">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ae968-126">Applications</span></span>|<span data-ttu-id="ae968-127">A gyűjtemény [alkalmazás](api-management-template-data-model-reference.md#Application) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="ae968-127">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="ae968-128">hello alkalmazások látható toohello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="ae968-128">hello applications visible toohello current user.</span></span>|  
|<span data-ttu-id="ae968-129">CategoryName</span><span class="sxs-lookup"><span data-stu-id="ae968-129">CategoryName</span></span>|<span data-ttu-id="ae968-130">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ae968-130">string</span></span>|<span data-ttu-id="ae968-131">hello kategória az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ae968-131">hello category of application.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="ae968-132">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="ae968-132">Sample template data</span></span>  
  
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
  
##  <span data-ttu-id="ae968-133"><a name="Application"></a>Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="ae968-133"><a name="Application"></a> Application</span></span>  
 <span data-ttu-id="ae968-134">Hello **alkalmazás** sablon teszi hello alkalmazás lap toocustomize hello törzsében hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="ae968-134">hello **Application** template allows you toocustomize hello body of hello application page in hello developer portal.</span></span>  
  
 <span data-ttu-id="ae968-135">![Alkalmazássablonok lap Developer portálon](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM lap Developer portálon alkalmazássablonok")</span><span class="sxs-lookup"><span data-stu-id="ae968-135">![Application Page Developer Portal Templates](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM Application Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="ae968-136">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="ae968-136">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="ae968-137">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="ae968-137">Controls</span></span>  
 <span data-ttu-id="ae968-138">Hello `Application` sablon nem teszi lehetővé a hello használata [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="ae968-138">hello `Application` template does not allow hello use of any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="ae968-139">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="ae968-139">Data model</span></span>  
 <span data-ttu-id="ae968-140">[Alkalmazás](api-management-template-data-model-reference.md#Application) entitás.</span><span class="sxs-lookup"><span data-stu-id="ae968-140">[Application](api-management-template-data-model-reference.md#Application) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="ae968-141">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="ae968-141">Sample template data</span></span>  
  
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

## <a name="next-steps"></a><span data-ttu-id="ae968-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae968-142">Next steps</span></span>
<span data-ttu-id="ae968-143">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="ae968-143">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
