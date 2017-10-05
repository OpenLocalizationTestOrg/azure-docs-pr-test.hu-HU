---
title: "Azure Data Factory entitások elnevezési szabályai |} Microsoft Docs"
description: "Adat-előállító entitások elnevezési szabályainak ismerteti."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: d447bbceb4ab344e011311eaf143b20f0a0400d3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-factory---naming-rules"></a><span data-ttu-id="b8b2c-103">Az Azure Data Factory - elnevezési szabályok</span><span class="sxs-lookup"><span data-stu-id="b8b2c-103">Azure Data Factory - naming rules</span></span>
<span data-ttu-id="b8b2c-104">A következő táblázat elnevezési szabályoknak az adat-előállító összetevők.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-104">The following table provides naming rules for Data Factory artifacts.</span></span>

| <span data-ttu-id="b8b2c-105">Név</span><span class="sxs-lookup"><span data-stu-id="b8b2c-105">Name</span></span> | <span data-ttu-id="b8b2c-106">Név egyedisége</span><span class="sxs-lookup"><span data-stu-id="b8b2c-106">Name Uniqueness</span></span> | <span data-ttu-id="b8b2c-107">Érvényességi ellenőrzéseket</span><span class="sxs-lookup"><span data-stu-id="b8b2c-107">Validation Checks</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b8b2c-108">Data Factory</span><span class="sxs-lookup"><span data-stu-id="b8b2c-108">Data Factory</span></span> |<span data-ttu-id="b8b2c-109">Egyedi Microsoft Azure között.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-109">Unique across Microsoft Azure.</span></span> <span data-ttu-id="b8b2c-110">Nevek nem különböztetik meg, ez azt jelenti, hogy `MyDF` és `mydf` adat-előállító hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-110">Names are case-insensitive, that is, `MyDF` and `mydf` refer to the same data factory.</span></span> |<ul><li><span data-ttu-id="b8b2c-111">Minden adat-előállító pontosan egy Azure-előfizetés van kötve.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-111">Each data factory is tied to exactly one Azure subscription.</span></span></li><li><span data-ttu-id="b8b2c-112">Objektumnevek betűvel vagy számmal kell kezdődnie, és csak betűket, számokat és a kötőjel (-) karaktert tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-112">Object names must start with a letter or a number, and can contain only letters, numbers, and the dash (-) character.</span></span></li><li><span data-ttu-id="b8b2c-113">Minden kötőjel (-) karaktert legyen azonnal előtt, és betűvel vagy számmal követ.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-113">Every dash (-) character must be immediately preceded and followed by a letter or a number.</span></span> <span data-ttu-id="b8b2c-114">A tároló neve nem szerepelhetnek egymást követő kötőjeleket.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-114">Consecutive dashes are not permitted in container names.</span></span></li><li><span data-ttu-id="b8b2c-115">Neve 3 – 63 karakter hosszú lehet.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-115">Name can be 3-63 characters long.</span></span></li></ul> |
| <span data-ttu-id="b8b2c-116">Szolgáltatások/táblák/folyamatok csatolt</span><span class="sxs-lookup"><span data-stu-id="b8b2c-116">Linked Services/Tables/Pipelines</span></span> |<span data-ttu-id="b8b2c-117">Egyedi az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-117">Unique with in a data factory.</span></span> <span data-ttu-id="b8b2c-118">Nevek nem különböztetik meg.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-118">Names are case-insensitive.</span></span> |<ul><li><span data-ttu-id="b8b2c-119">A táblanév maximális karakterszámot: 260.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-119">Maximum number of characters in a table name: 260.</span></span></li><li><span data-ttu-id="b8b2c-120">Objektumnevek betű, szám vagy aláhúzásjel (_) kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-120">Object names must start with a letter, number, or an underscore (_).</span></span></li><li><span data-ttu-id="b8b2c-121">Következő karakterek nem engedélyezettek: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</span><span class="sxs-lookup"><span data-stu-id="b8b2c-121">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”, ”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |
| <span data-ttu-id="b8b2c-122">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="b8b2c-122">Resource Group</span></span> |<span data-ttu-id="b8b2c-123">Egyedi Microsoft Azure között.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-123">Unique across Microsoft Azure.</span></span> <span data-ttu-id="b8b2c-124">Nevek nem különböztetik meg.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-124">Names are case-insensitive.</span></span> |<ul><li><span data-ttu-id="b8b2c-125">Karakterek maximális száma: 1000.</span><span class="sxs-lookup"><span data-stu-id="b8b2c-125">Maximum number of characters: 1000.</span></span></li><li><span data-ttu-id="b8b2c-126">Név tartalmazhat betűket, számjegyeket és a következő karaktereket: "-", "_",","és"."</span><span class="sxs-lookup"><span data-stu-id="b8b2c-126">Name can contain letters, digits, and the following characters: “-”, “_”, “,” and “.”</span></span></li></ul> |

