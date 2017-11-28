---
title: "az Operations Management Suite (OMS) megoldások aaaViews |} Microsoft Docs"
description: "Az Operations Management Suite (OMS) megoldások általában egy vagy több nézetek toovisualize adatait tartalmazzák.  Ez a cikk ismerteti, hogyan tooexport nézet által létrehozott hello adatforrásnézet-tervezőből, és adja hozzá a megoldásra. "
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 570b278c-2d47-4e5a-9828-7f01f31ddf8c
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 303861465014a27289f831332b3d95925c0ae66d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a><span data-ttu-id="0492f-104">Nézetek az Operations Management Suite (OMS) megoldások (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="0492f-104">Views in Operations Management Suite (OMS) management solutions (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="0492f-105">Ez az előzetes dokumentum megoldások létrehozásához az OMS Szolgáltatáshoz, amely jelenleg előzetes verziójúak.</span><span class="sxs-lookup"><span data-stu-id="0492f-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="0492f-106">Az alábbiakban semmilyen sémát tulajdonos toochange.</span><span class="sxs-lookup"><span data-stu-id="0492f-106">Any schema described below is subject toochange.</span></span>    
>
>

<span data-ttu-id="0492f-107">[Az Operations Management Suite (OMS) megoldások](operations-management-suite-solutions.md) általában egy vagy több nézetek toovisualize adatait tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="0492f-107">[Management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions.md) will typically include one or more views toovisualize data.</span></span>  <span data-ttu-id="0492f-108">Ez a cikk ismerteti, hogyan tooexport nézet által létrehozott hello [adatforrásnézet-tervezőből](../log-analytics/log-analytics-view-designer.md) adja hozzá a megoldásra.</span><span class="sxs-lookup"><span data-stu-id="0492f-108">This article describes how tooexport a view created by hello [View Designer](../log-analytics/log-analytics-view-designer.md) and include it in a management solution.</span></span>  

> [!NOTE]
> <span data-ttu-id="0492f-109">hello minták ebben a cikkben használható paraméterek és változók vagy kötelező vagy közös toomanagement megoldások és ismertetett [létrehozása kezelési megoldásai Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="0492f-109">hello samples in this article use parameters and variables that are either required or common toomanagement solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="0492f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0492f-110">Prerequisites</span></span>
<span data-ttu-id="0492f-111">Ez a cikk feltételezi, hogy most már tudja, hogyan túl[felügyeleti megoldás létrehozása](operations-management-suite-solutions-creating.md) és a megoldás fájl hello struktúrája.</span><span class="sxs-lookup"><span data-stu-id="0492f-111">This article assumes that you're already familiar with how too[create a management solution](operations-management-suite-solutions-creating.md) and hello structure of a solution file.</span></span>

## <a name="overview"></a><span data-ttu-id="0492f-112">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0492f-112">Overview</span></span>
<span data-ttu-id="0492f-113">tooinclude megoldásra nézet, hozzon létre egy **erőforrás** ki azt a hello [megoldásfájlt](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="0492f-113">tooinclude a view in a management solution, you create a **resource** for it in hello [solution file](operations-management-suite-solutions-creating.md).</span></span>  <span data-ttu-id="0492f-114">hello hello nézet részletes konfigurációs leíró JSON pedig általában összetett, ha valami nem, hogy egy tipikus megoldás Szerző manuálisan lenne képes toocreate.</span><span class="sxs-lookup"><span data-stu-id="0492f-114">hello JSON that describes hello view's detailed configuration is typically complex though and not something that a typical solution author would be able toocreate manually.</span></span>  <span data-ttu-id="0492f-115">hello leggyakoribb módja toocreate hello nézet használatával hello [adatforrásnézet-tervezőből](../log-analytics/log-analytics-view-designer.md), exportálni, és adja meg a részletes konfigurációs toohello megoldás.</span><span class="sxs-lookup"><span data-stu-id="0492f-115">hello most common method is toocreate hello view using hello [View Designer](../log-analytics/log-analytics-view-designer.md), export it, and then add its detailed configuration toohello solution.</span></span>

<span data-ttu-id="0492f-116">hello lépéseken tooadd nézet tooa megoldás a következők:</span><span class="sxs-lookup"><span data-stu-id="0492f-116">hello basic steps tooadd a view tooa solution are as follows.</span></span>  <span data-ttu-id="0492f-117">Minden lépés az alábbi hello szakaszokban részletesen ismertetett.</span><span class="sxs-lookup"><span data-stu-id="0492f-117">Each step is described in further detail in hello sections below.</span></span>

1. <span data-ttu-id="0492f-118">Hello nézet tooa fájl exportálása.</span><span class="sxs-lookup"><span data-stu-id="0492f-118">Export hello view tooa file.</span></span>
2. <span data-ttu-id="0492f-119">Hozzon létre hello nézet erőforrás hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="0492f-119">Create hello view resource in hello solution.</span></span>
3. <span data-ttu-id="0492f-120">Adja hozzá a hello részleteinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="0492f-120">Add hello view details.</span></span>

## <a name="export-hello-view-tooa-file"></a><span data-ttu-id="0492f-121">Hello nézet tooa fájl exportálása</span><span class="sxs-lookup"><span data-stu-id="0492f-121">Export hello view tooa file</span></span>
<span data-ttu-id="0492f-122">Hajtsa végre a hello található utasítások segítségével: [napló Analytics adatforrásnézet-tervezőből](../log-analytics/log-analytics-view-designer.md) tooexport nézet tooa fájl.</span><span class="sxs-lookup"><span data-stu-id="0492f-122">Follow hello instructions at [Log Analytics View Designer](../log-analytics/log-analytics-view-designer.md) tooexport a view tooa file.</span></span>  <span data-ttu-id="0492f-123">hello exportált fájlt fog kell JSON formátumban hello azonos [hello megoldásfájlt az elemek](operations-management-suite-solutions-solution-file.md).</span><span class="sxs-lookup"><span data-stu-id="0492f-123">hello exported file will be in JSON format with hello same [elements as hello solution file](operations-management-suite-solutions-solution-file.md).</span></span>  

<span data-ttu-id="0492f-124">Hello **erőforrások** hello nézet fájl eleme lesz típussal rendelkező erőforrás **Microsoft.OperationalInsights/workspaces** , hogy jelöli hello OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="0492f-124">hello **resources** element of hello view file will have a resource with a type of **Microsoft.OperationalInsights/workspaces** that represents hello OMS workspace.</span></span>  <span data-ttu-id="0492f-125">Ez az elem lesz a subelement típussal rendelkező **nézetek** , amely hello nézet jelöli, és a részletes kapcsolatos konfigurációját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0492f-125">This element will have a subelement with a type of **views** that represents hello view and contains its detailed configuration.</span></span>  <span data-ttu-id="0492f-126">Másolja az ehhez az elemhez hello részleteit lesz, és átmásolja a megoldás.</span><span class="sxs-lookup"><span data-stu-id="0492f-126">You will copy hello details of this element and then copy it into your solution.</span></span>

## <a name="create-hello-view-resource-in-hello-solution"></a><span data-ttu-id="0492f-127">Hello megoldásban hello nézet erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0492f-127">Create hello view resource in hello solution</span></span>
<span data-ttu-id="0492f-128">Adja hozzá a következő nézet erőforrás toohello hello **erőforrások** a megoldásfájlt eleme.</span><span class="sxs-lookup"><span data-stu-id="0492f-128">Add hello following view resource toohello **resources** element of your solution file.</span></span>  <span data-ttu-id="0492f-129">Ez a változókat, amelyek az alábbiakban található, hogy hozzá kell adnia is használja.</span><span class="sxs-lookup"><span data-stu-id="0492f-129">This uses variables that are described below that you must also add.</span></span>  <span data-ttu-id="0492f-130">Vegye figyelembe, hogy hello **irányítópult** és **OverviewTile** tulajdonságok jelölik hello vonatkozó tulajdonságok hello exportált nézet fájlból felülírja.</span><span class="sxs-lookup"><span data-stu-id="0492f-130">Note that hello **Dashboard** and **OverviewTile** properties are placeholders that you will overwrite with hello corresponding properties from hello exported view file.</span></span>

    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        "dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile":
        }
    }

<span data-ttu-id="0492f-131">Adja hozzá a következő változók toohello változók elem hello megoldásfájl hello, és cserélje le a hello értékek toothose megoldást.</span><span class="sxs-lookup"><span data-stu-id="0492f-131">Add hello following variables toohello variables element of hello solution file and replace hello values toothose for your solution.</span></span>

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of hello view."
    "ViewName": "Provide a name for hello view here."


<span data-ttu-id="0492f-132">Fontos megjegyezni, hogy hello teljes nézet erőforrás sikerült másolni az exportált nézet fájl kellene módosítások hozzá a következő toomake hello toowork a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="0492f-132">Note that you could copy hello entire view resource from your exported view file, but you would need toomake hello following changes for it toowork in your solution.</span></span>  

* <span data-ttu-id="0492f-133">Hello **típus** hello nézet erőforrás kell változása toobe **nézetek** túl**Microsoft.OperationalInsights/workspaces**.</span><span class="sxs-lookup"><span data-stu-id="0492f-133">hello **type** for hello view resource needs toobe changed from **views** too**Microsoft.OperationalInsights/workspaces**.</span></span>
* <span data-ttu-id="0492f-134">Hello **neve** hello nézet erőforráshoz tartozó kell módosítani toobe tooinclude hello munkaterület nevét.</span><span class="sxs-lookup"><span data-stu-id="0492f-134">hello **name** property for hello view resource needs toobe changed tooinclude hello workspace name.</span></span>
* <span data-ttu-id="0492f-135">hello munkaterület hello függőség kell eltávolítani, mert hello munkaterület erőforrás nincs definiálva a hello megoldás toobe.</span><span class="sxs-lookup"><span data-stu-id="0492f-135">hello dependency on hello workspace needs toobe removed since hello workspace resource isn't defined in hello solution.</span></span>
* <span data-ttu-id="0492f-136">**DisplayName** tulajdonság igények toobe hozzáadott toohello nézet.</span><span class="sxs-lookup"><span data-stu-id="0492f-136">**DisplayName** property needs toobe added toohello view.</span></span>  <span data-ttu-id="0492f-137">Hello **azonosító**, **neve**, és **DisplayName** összes egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="0492f-137">hello **Id**, **Name**, and **DisplayName** must all match.</span></span>
* <span data-ttu-id="0492f-138">Meg kell változtatni a paraméterneveknek toomatch hello szükséges beállítása olyan paraméterek összessége.</span><span class="sxs-lookup"><span data-stu-id="0492f-138">Parameter names must be changed toomatch hello required set of parameters.</span></span>
* <span data-ttu-id="0492f-139">Változók hello megoldás definiált legyen, és megfelelő tulajdonságok hello szerepel.</span><span class="sxs-lookup"><span data-stu-id="0492f-139">Variables should be defined in hello solution and used in hello appropriate properties.</span></span>

## <a name="add-hello-view-details"></a><span data-ttu-id="0492f-140">Adja hozzá a hello részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="0492f-140">Add hello view details</span></span>
<span data-ttu-id="0492f-141">hello nézet erőforrás hello az exportált fájlt fogja tartalmazni hello két elemét nézet **tulajdonságok** nevű elem **irányítópult** és **OverviewTile** hello tartalmazó részletes konfigurációs hello nézet.</span><span class="sxs-lookup"><span data-stu-id="0492f-141">hello view resource in hello exported view file will contain two elements in hello **properties** element named **Dashboard** and **OverviewTile** which contain hello detailed configuration of hello view.</span></span>  <span data-ttu-id="0492f-142">Ez a két elem és a tartalom másolása hello **tulajdonságok** elem hello nézet erőforrás a megoldásfájlt.</span><span class="sxs-lookup"><span data-stu-id="0492f-142">Copy these two elements and their contents into hello **properties** element of hello view resource in your solution file.</span></span>

## <a name="example"></a><span data-ttu-id="0492f-143">Példa</span><span class="sxs-lookup"><span data-stu-id="0492f-143">Example</span></span>
<span data-ttu-id="0492f-144">A következő minta hello például egy egyszerű megoldást fájl nézetet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="0492f-144">For example, hello following sample shows a simple solution file with a view.</span></span>  <span data-ttu-id="0492f-145">Folytatást jelző pontokra (...) jelennek meg a hello **irányítópult** és **OverviewTile** terület okokból tartalmát.</span><span class="sxs-lookup"><span data-stu-id="0492f-145">Ellipses (...) are shown for hello **Dashboard** and **OverviewTile** contents for space reasons.</span></span>

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
          ]
    }




## <a name="next-steps"></a><span data-ttu-id="0492f-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0492f-146">Next steps</span></span>
* <span data-ttu-id="0492f-147">További részleteket létrehozásának [megoldások](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="0492f-147">Learn complete details of creating [management solutions](operations-management-suite-solutions-creating.md).</span></span>
* <span data-ttu-id="0492f-148">Tartalmaznak [a felügyeleti megoldás az automatizálási runbookok](operations-management-suite-solutions-resources-automation.md).</span><span class="sxs-lookup"><span data-stu-id="0492f-148">Include [Automation runbooks in your management solution](operations-management-suite-solutions-resources-automation.md).</span></span>
