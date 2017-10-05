---
title: "Létrehozása felhasználói felületének meghatározása az Azure által felügyelt alkalmazások megismerése |} Microsoft Docs"
description: "Ismerteti, hogyan lehet az Azure által felügyelt alkalmazások felhasználói felületi-meghatározások létrehozása"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 176b891538f85c5638a2321561c3d8bd377d245b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-createuidefinition"></a><span data-ttu-id="cd485-103">Ismerkedés a CreateUiDefinition</span><span class="sxs-lookup"><span data-stu-id="cd485-103">Getting started with CreateUiDefinition</span></span>
<span data-ttu-id="cd485-104">Ez a dokumentum bemutatja a core CreateUiDefinition, amely az Azure-portálon kezelt alkalmazás létrehozásához a felhasználói felület létrehozásához használja.</span><span class="sxs-lookup"><span data-stu-id="cd485-104">This document introduces the core concepts of a CreateUiDefinition, which is used by the Azure portal to generate the user interface for creating a managed application.</span></span>

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
   "handler": "Microsoft.Compute.MultiVm",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { }
   }
}
```

<span data-ttu-id="cd485-105">Egy CreateUiDefinition mindig három tulajdonságot tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="cd485-105">A CreateUiDefinition always contains three properties:</span></span> 

* <span data-ttu-id="cd485-106">eseménykezelő</span><span class="sxs-lookup"><span data-stu-id="cd485-106">handler</span></span>
* <span data-ttu-id="cd485-107">Verzió</span><span class="sxs-lookup"><span data-stu-id="cd485-107">version</span></span>
* <span data-ttu-id="cd485-108">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="cd485-108">parameters</span></span>

<span data-ttu-id="cd485-109">Kezelt alkalmazások, mindig kell kezelő `Microsoft.Compute.MultiVm`, és a legfrissebb támogatott verziót `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="cd485-109">For managed applications, handler should always be `Microsoft.Compute.MultiVm`, and the latest supported version is `0.1.2-preview`.</span></span>

<span data-ttu-id="cd485-110">A paraméterek tulajdonság sémájának attól függ, hogy a megadott kezelő és verziót.</span><span class="sxs-lookup"><span data-stu-id="cd485-110">The schema of the parameters property depends on the combination of the specified handler and version.</span></span> <span data-ttu-id="cd485-111">Kezelt alkalmazások, a támogatott tulajdonságok: `basics`, `steps`, és `outputs`.</span><span class="sxs-lookup"><span data-stu-id="cd485-111">For managed applications, the supported properties are `basics`, `steps`, and `outputs`.</span></span> <span data-ttu-id="cd485-112">A alapjait és lépéseket tulajdonságok tartalmaznak a _elemek_ - például a szövegmezőből és a legördülő lista - az Azure portálon megjelenítendő.</span><span class="sxs-lookup"><span data-stu-id="cd485-112">The basics and steps properties contain the _elements_ - like textboxes and dropdowns - to be displayed in the Azure portal.</span></span> <span data-ttu-id="cd485-113">A kimenetek tulajdonság a megadott elemek kimeneti értékeit hozzárendelése az Azure Resource Manager központi telepítési sablon paramétereinek szolgál.</span><span class="sxs-lookup"><span data-stu-id="cd485-113">The outputs property is used to map the output values of the specified elements to the parameters of the Azure Resource Manager deployment template.</span></span>

<span data-ttu-id="cd485-114">Beleértve `$schema` javasolt, de nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="cd485-114">Including `$schema` is recommended, but optional.</span></span> <span data-ttu-id="cd485-115">Ha meg van adva, a következő `version` meg kell egyeznie a verzió belül a `$schema` URI.</span><span class="sxs-lookup"><span data-stu-id="cd485-115">If specified, the value for `version` must match the version within the `$schema` URI.</span></span>

## <a name="basics"></a><span data-ttu-id="cd485-116">Alapvető beállítások</span><span class="sxs-lookup"><span data-stu-id="cd485-116">Basics</span></span>
<span data-ttu-id="cd485-117">Az alapok lépés mindig az első lépés a varázsló jönnek létre, ha az Azure-portálon elemzi a CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="cd485-117">The Basics step is always the first step of the wizard generated when the Azure portal parses a CreateUiDefinition.</span></span> <span data-ttu-id="cd485-118">A megadott elemek megjelenítése mellett `basics`, a portálon a felhasználók számára előfizetés, erőforráscsoport és a telepítési hely kiválasztása elemek esetében.</span><span class="sxs-lookup"><span data-stu-id="cd485-118">In addition to displaying the elements specified in `basics`, the portal injects elements for users to choose the subscription, resource group, and location for the deployment.</span></span> <span data-ttu-id="cd485-119">Általában lekérdezése a központi telepítés kiterjedő paraméterek, például egy fürt vagy a rendszergazdai hitelesítő adatokat, nevét elemeket kell nyissa meg az ebben a lépésben.</span><span class="sxs-lookup"><span data-stu-id="cd485-119">Generally, elements that query for deployment-wide parameters, like the name of a cluster or administrator credentials, should go in this step.</span></span>

<span data-ttu-id="cd485-120">Ha egy elem viselkedés attól függ, a felhasználó előfizetés, erőforráscsoportból vagy helyét, majd, hogy elem nem használható az alapokat.</span><span class="sxs-lookup"><span data-stu-id="cd485-120">If an element's behavior depends on the user's subscription, resource group, or location, then that element can't be used in basics.</span></span> <span data-ttu-id="cd485-121">Például **Microsoft.Compute.SizeSelector** attól függ, a felhasználó előfizetésben és helyen meghatározni az elérhető méretek listáját.</span><span class="sxs-lookup"><span data-stu-id="cd485-121">For example, **Microsoft.Compute.SizeSelector** depends on the user's subscription and location to determine the list of available sizes.</span></span> <span data-ttu-id="cd485-122">Ezért **Microsoft.Compute.SizeSelector** csak akkor használható a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cd485-122">Therefore, **Microsoft.Compute.SizeSelector** can only be used in steps.</span></span> <span data-ttu-id="cd485-123">Általában csak elemeinek a **Microsoft.Common** névtér alapokat is használható.</span><span class="sxs-lookup"><span data-stu-id="cd485-123">Generally, only elements in the **Microsoft.Common** namespace can be used in basics.</span></span> <span data-ttu-id="cd485-124">Bár bizonyos elemek más névtérben (például **Microsoft.Compute.Credentials**), amely nem függ a felhasználói környezet, továbbra is engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="cd485-124">Although some elements in other namespaces (like **Microsoft.Compute.Credentials**) that don't depend on the user's context, are still allowed.</span></span>

## <a name="steps"></a><span data-ttu-id="cd485-125">Lépések</span><span class="sxs-lookup"><span data-stu-id="cd485-125">Steps</span></span>
<span data-ttu-id="cd485-126">A lépések tulajdonság nulla vagy több további lépést is végre megjelenítése után alapjai, egy vagy több elemet tartalmaz, amelyek mindegyike tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="cd485-126">The steps property can contain zero or more additional steps to display after basics, each of which contains one or more elements.</span></span> <span data-ttu-id="cd485-127">Fontolja meg egy szerepkör vagy a réteg az alkalmazás telepítése lépést.</span><span class="sxs-lookup"><span data-stu-id="cd485-127">Consider adding steps per role or tier of the application being deployed.</span></span> <span data-ttu-id="cd485-128">Például hozzáadhat egy bemenetek a fő csomóponthoz, és egy lépést az ezen csomópontokhoz tartozó fürtben.</span><span class="sxs-lookup"><span data-stu-id="cd485-128">For example, add a step for inputs for the master nodes, and a step for the worker nodes in a cluster.</span></span>

## <a name="outputs"></a><span data-ttu-id="cd485-129">kimenetek</span><span class="sxs-lookup"><span data-stu-id="cd485-129">Outputs</span></span>
<span data-ttu-id="cd485-130">Az Azure-portálon használja a `outputs` tulajdonság elemeket a `basics` és `steps` paramétereinek az Azure Resource Manager központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="cd485-130">The Azure portal uses the `outputs` property to map elements from `basics` and `steps` to the parameters of the Azure Resource Manager deployment template.</span></span> <span data-ttu-id="cd485-131">Ehhez a szótárhoz kulcsai a sablon-paraméterek nevei, és a kimeneti objektumokat a hivatkozott elemeket tulajdonságainak értékei.</span><span class="sxs-lookup"><span data-stu-id="cd485-131">The keys of this dictionary are the names of the template parameters, and the values are properties of the output objects from the referenced elements.</span></span>

## <a name="functions"></a><span data-ttu-id="cd485-132">Functions</span><span class="sxs-lookup"><span data-stu-id="cd485-132">Functions</span></span>
<span data-ttu-id="cd485-133">Hasonló [sablonfüggvényei](resource-group-template-functions.md) az Azure Resource Manager (mind a szintaxist és a funkció), CreateUiDefinition elem bemenetekhez és kimenetekhez való munkához funkciókat biztosít, valamint szolgáltatások – pl. conditionals.</span><span class="sxs-lookup"><span data-stu-id="cd485-133">Similar to [template functions](resource-group-template-functions.md) in Azure Resource Manager (both in syntax and functionality), CreateUiDefinition provides functions for working with elements' inputs and outputs, as well as features such as conditionals.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd485-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd485-134">Next steps</span></span>
<span data-ttu-id="cd485-135">CreateUiDefinition önmagában egy egyszerű sémával rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="cd485-135">CreateUiDefinition itself has a simple schema.</span></span> <span data-ttu-id="cd485-136">A valódi mélysége származik támogatott elemeket és függvények, amelyek wondrous részletesen leírja a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="cd485-136">The real depth of it comes from all the supported elements and functions, which the following documents describe in wondrous detail:</span></span>

- [<span data-ttu-id="cd485-137">Elemek</span><span class="sxs-lookup"><span data-stu-id="cd485-137">Elements</span></span>](managed-application-createuidefinition-elements.md)
- [<span data-ttu-id="cd485-138">Functions</span><span class="sxs-lookup"><span data-stu-id="cd485-138">Functions</span></span>](managed-application-createuidefinition-functions.md)

<span data-ttu-id="cd485-139">A jelenlegi JSON-séma CreateUiDefinition érhető el itt: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span><span class="sxs-lookup"><span data-stu-id="cd485-139">A current JSON schema for CreateUiDefinition is available here: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span></span> 

<span data-ttu-id="cd485-140">Ugyanazon a helyen későbbi verzióiban lesz elérhető.</span><span class="sxs-lookup"><span data-stu-id="cd485-140">Later versions will be available at the same location.</span></span> <span data-ttu-id="cd485-141">Cserélje le a `0.1.2-preview` az URL-cím része, és a `version` a használni kívánt azonosító értéket.</span><span class="sxs-lookup"><span data-stu-id="cd485-141">Replace the `0.1.2-preview` portion of the URL and the `version` value with the version identifier you intend to use.</span></span> <span data-ttu-id="cd485-142">A jelenleg támogatott verziójú azonosítók `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, és `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="cd485-142">The currently supported version identifiers are `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, and `0.1.2-preview`.</span></span>