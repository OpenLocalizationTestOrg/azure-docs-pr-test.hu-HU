---
title: "az Azure által felügyelt alkalmazások felhasználói felületi definíciójának létrehozása aaaUnderstand |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate felhasználói felület definíciókat az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: d53ddf438c24d5a6cb8dd53ca0b4694ab0462515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-createuidefinition"></a><span data-ttu-id="27fa8-103">Ismerkedés a CreateUiDefinition</span><span class="sxs-lookup"><span data-stu-id="27fa8-103">Getting started with CreateUiDefinition</span></span>
<span data-ttu-id="27fa8-104">Ez a dokumentum egy CreateUiDefinition, hello Azure portál toogenerate hello felhasználói felület által kezelt alkalmazás létrehozásához használt alapvető fogalmait hello vezet be.</span><span class="sxs-lookup"><span data-stu-id="27fa8-104">This document introduces hello core concepts of a CreateUiDefinition, which is used by hello Azure portal toogenerate hello user interface for creating a managed application.</span></span>

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

<span data-ttu-id="27fa8-105">Egy CreateUiDefinition mindig három tulajdonságot tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="27fa8-105">A CreateUiDefinition always contains three properties:</span></span> 

* <span data-ttu-id="27fa8-106">eseménykezelő</span><span class="sxs-lookup"><span data-stu-id="27fa8-106">handler</span></span>
* <span data-ttu-id="27fa8-107">Verzió</span><span class="sxs-lookup"><span data-stu-id="27fa8-107">version</span></span>
* <span data-ttu-id="27fa8-108">paraméterek</span><span class="sxs-lookup"><span data-stu-id="27fa8-108">parameters</span></span>

<span data-ttu-id="27fa8-109">Kezelt alkalmazások, mindig kell kezelő `Microsoft.Compute.MultiVm`, és a legújabb támogatott hello verziója `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="27fa8-109">For managed applications, handler should always be `Microsoft.Compute.MultiVm`, and hello latest supported version is `0.1.2-preview`.</span></span>

<span data-ttu-id="27fa8-110">hello paraméterek tulajdonság sémájának hello hello megadott kezelő és a verzió hello kombinációja függ.</span><span class="sxs-lookup"><span data-stu-id="27fa8-110">hello schema of hello parameters property depends on hello combination of hello specified handler and version.</span></span> <span data-ttu-id="27fa8-111">Kezelt alkalmazások hello támogatott tulajdonságok: `basics`, `steps`, és `outputs`.</span><span class="sxs-lookup"><span data-stu-id="27fa8-111">For managed applications, hello supported properties are `basics`, `steps`, and `outputs`.</span></span> <span data-ttu-id="27fa8-112">hello alapjait és lépéseket tulajdonságok tartalmaznak hello _elemek_ – például a szövegmezők és legördülő listák megnyílásának - toobe megjelenő hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="27fa8-112">hello basics and steps properties contain hello _elements_ - like textboxes and dropdowns - toobe displayed in hello Azure portal.</span></span> <span data-ttu-id="27fa8-113">hello kimenetek tulajdonság használt toomap hello kimeneti értékeit hello megadott elemek toohello paraméterei hello Azure Resource Manager központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="27fa8-113">hello outputs property is used toomap hello output values of hello specified elements toohello parameters of hello Azure Resource Manager deployment template.</span></span>

<span data-ttu-id="27fa8-114">Beleértve `$schema` javasolt, de nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="27fa8-114">Including `$schema` is recommended, but optional.</span></span> <span data-ttu-id="27fa8-115">Ha meg van adva, a következő hello `version` meg kell egyeznie a hello verzió belül hello `$schema` URI.</span><span class="sxs-lookup"><span data-stu-id="27fa8-115">If specified, hello value for `version` must match hello version within hello `$schema` URI.</span></span>

## <a name="basics"></a><span data-ttu-id="27fa8-116">Alapvető beállítások</span><span class="sxs-lookup"><span data-stu-id="27fa8-116">Basics</span></span>
<span data-ttu-id="27fa8-117">hello alapjai lépés mindig hello jönnek létre, ha az Azure-portálon hello elemzi a CreateUiDefinition hello varázsló első lépése.</span><span class="sxs-lookup"><span data-stu-id="27fa8-117">hello Basics step is always hello first step of hello wizard generated when hello Azure portal parses a CreateUiDefinition.</span></span> <span data-ttu-id="27fa8-118">Ezenkívül toodisplaying hello elemek megadott `basics`, hello portál esetében elemei felhasználók toochoose hello előfizetés, erőforráscsoport és hello telepítési helyét.</span><span class="sxs-lookup"><span data-stu-id="27fa8-118">In addition toodisplaying hello elements specified in `basics`, hello portal injects elements for users toochoose hello subscription, resource group, and location for hello deployment.</span></span> <span data-ttu-id="27fa8-119">Általában lekérdezése a központi telepítés kiterjedő paraméterek hello nevét egy fürt vagy a rendszergazdai hitelesítő adatokat, például elemeket kell nyissa meg az ebben a lépésben.</span><span class="sxs-lookup"><span data-stu-id="27fa8-119">Generally, elements that query for deployment-wide parameters, like hello name of a cluster or administrator credentials, should go in this step.</span></span>

<span data-ttu-id="27fa8-120">Ha egy elem viselkedés attól függ, hello felhasználói előfizetés, erőforráscsoporthoz vagy helyre, majd, hogy elem nem használható az alapokat.</span><span class="sxs-lookup"><span data-stu-id="27fa8-120">If an element's behavior depends on hello user's subscription, resource group, or location, then that element can't be used in basics.</span></span> <span data-ttu-id="27fa8-121">Például **Microsoft.Compute.SizeSelector** hello felhasználói előfizetésben és helyen toodetermine hello listájában elérhető méretek függ.</span><span class="sxs-lookup"><span data-stu-id="27fa8-121">For example, **Microsoft.Compute.SizeSelector** depends on hello user's subscription and location toodetermine hello list of available sizes.</span></span> <span data-ttu-id="27fa8-122">Ezért **Microsoft.Compute.SizeSelector** csak akkor használható a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="27fa8-122">Therefore, **Microsoft.Compute.SizeSelector** can only be used in steps.</span></span> <span data-ttu-id="27fa8-123">Általában csak elemek hello **Microsoft.Common** névtér alapokat is használható.</span><span class="sxs-lookup"><span data-stu-id="27fa8-123">Generally, only elements in hello **Microsoft.Common** namespace can be used in basics.</span></span> <span data-ttu-id="27fa8-124">Bár bizonyos elemek más névtérben (például **Microsoft.Compute.Credentials**), amely nem függ a hello felhasználói környezet, továbbra is engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="27fa8-124">Although some elements in other namespaces (like **Microsoft.Compute.Credentials**) that don't depend on hello user's context, are still allowed.</span></span>

## <a name="steps"></a><span data-ttu-id="27fa8-125">Lépések</span><span class="sxs-lookup"><span data-stu-id="27fa8-125">Steps</span></span>
<span data-ttu-id="27fa8-126">hello lépéseket tulajdonság nulla vagy több további lépést toodisplay után alapjai, egy vagy több elemet tartalmaz, amelyek mindegyike tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="27fa8-126">hello steps property can contain zero or more additional steps toodisplay after basics, each of which contains one or more elements.</span></span> <span data-ttu-id="27fa8-127">Fontolja meg egy szerepkör vagy a réteg hello alkalmazás telepítése lépést.</span><span class="sxs-lookup"><span data-stu-id="27fa8-127">Consider adding steps per role or tier of hello application being deployed.</span></span> <span data-ttu-id="27fa8-128">Például hozzáadhat egy bemenetek hello fő csomóponthoz, és egy lépést hello ezen csomópontokhoz tartozó fürtben.</span><span class="sxs-lookup"><span data-stu-id="27fa8-128">For example, add a step for inputs for hello master nodes, and a step for hello worker nodes in a cluster.</span></span>

## <a name="outputs"></a><span data-ttu-id="27fa8-129">kimenetek</span><span class="sxs-lookup"><span data-stu-id="27fa8-129">Outputs</span></span>
<span data-ttu-id="27fa8-130">hello Azure-portált használja hello `outputs` tulajdonság toomap elemek `basics` és `steps` toohello paraméterei hello Azure Resource Manager központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="27fa8-130">hello Azure portal uses hello `outputs` property toomap elements from `basics` and `steps` toohello parameters of hello Azure Resource Manager deployment template.</span></span> <span data-ttu-id="27fa8-131">a szótár hello kulcsok hello hello sablon paraméterek nevei, és hello értékei származó hivatkozott hello hello kimeneti objektumok tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="27fa8-131">hello keys of this dictionary are hello names of hello template parameters, and hello values are properties of hello output objects from hello referenced elements.</span></span>

## <a name="functions"></a><span data-ttu-id="27fa8-132">Functions</span><span class="sxs-lookup"><span data-stu-id="27fa8-132">Functions</span></span>
<span data-ttu-id="27fa8-133">Hasonló túl[sablonfüggvényei](resource-group-template-functions.md) az Azure Resource Manager (mind a szintaxist és a funkció), CreateUiDefinition elem bemenetekhez és kimenetekhez való munkához funkciókat biztosít, valamint szolgáltatások – pl. conditionals.</span><span class="sxs-lookup"><span data-stu-id="27fa8-133">Similar too[template functions](resource-group-template-functions.md) in Azure Resource Manager (both in syntax and functionality), CreateUiDefinition provides functions for working with elements' inputs and outputs, as well as features such as conditionals.</span></span>

## <a name="next-steps"></a><span data-ttu-id="27fa8-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="27fa8-134">Next steps</span></span>
<span data-ttu-id="27fa8-135">CreateUiDefinition önmagában egy egyszerű sémával rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="27fa8-135">CreateUiDefinition itself has a simple schema.</span></span> <span data-ttu-id="27fa8-136">hello valós mélysége azt minden hello támogatott elemek és funkciók, mely a következő dokumentumok hello wondrous részletesen leírja származnak:</span><span class="sxs-lookup"><span data-stu-id="27fa8-136">hello real depth of it comes from all hello supported elements and functions, which hello following documents describe in wondrous detail:</span></span>

- [<span data-ttu-id="27fa8-137">Elemek</span><span class="sxs-lookup"><span data-stu-id="27fa8-137">Elements</span></span>](managed-application-createuidefinition-elements.md)
- [<span data-ttu-id="27fa8-138">Functions</span><span class="sxs-lookup"><span data-stu-id="27fa8-138">Functions</span></span>](managed-application-createuidefinition-functions.md)

<span data-ttu-id="27fa8-139">A jelenlegi JSON-séma CreateUiDefinition érhető el itt: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span><span class="sxs-lookup"><span data-stu-id="27fa8-139">A current JSON schema for CreateUiDefinition is available here: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span></span> 

<span data-ttu-id="27fa8-140">Későbbi verzióiban lesz elérhető legyen a következőn hello ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="27fa8-140">Later versions will be available at hello same location.</span></span> <span data-ttu-id="27fa8-141">Cserélje le a hello `0.1.2-preview` hello URL-cím és hello része `version` érték hello verzió azonosítójú toouse szeretné.</span><span class="sxs-lookup"><span data-stu-id="27fa8-141">Replace hello `0.1.2-preview` portion of hello URL and hello `version` value with hello version identifier you intend toouse.</span></span> <span data-ttu-id="27fa8-142">hello támogatott verzió azonosítók `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, és `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="27fa8-142">hello currently supported version identifiers are `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, and `0.1.2-preview`.</span></span>
