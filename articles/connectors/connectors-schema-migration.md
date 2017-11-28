---
title: "Logic Apps-alkalmazások áttelepítése a 2015. 08. 01. dátumú előzetes sémaverzióra | Microsoft Docs"
description: "Logic Apps alkalmazásait egyszerűen áttelepítheti a legújabb sémaverzióra. Csak kövesse az alábbi lépéseket."
services: logic-apps
documentationcenter: 
author: MSFTMAN
manager: erikre
editor: 
tags: connectors
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: deonhe
ms.openlocfilehash: a5a73a9f124e5339b61dbc49021444a208a471f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a><span data-ttu-id="a12a7-104">Logic Apps alkalmazások áttelepítése a 2015. 08. 01. dátumú előzetes sémaverzióra</span><span class="sxs-lookup"><span data-stu-id="a12a7-104">How to migrate logic apps to schema version 2015-08-01-preview</span></span>
<span data-ttu-id="a12a7-105">A meglévő Logic Apps alkalmazásoknak az új sémára való áttelepítéséhez tegye az alábbiakat:</span><span class="sxs-lookup"><span data-stu-id="a12a7-105">To move your existing logic apps to the new schema, do the following:</span></span>  

1. <span data-ttu-id="a12a7-106">Nyissa meg a Logic Apps alkalmazást az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="a12a7-106">Open your logic app in the Azure portal</span></span>  
2. <span data-ttu-id="a12a7-107">Kattintson az Update Schema (Séma frissítése) elemre:</span><span class="sxs-lookup"><span data-stu-id="a12a7-107">Click Update Schema:</span></span>
   
   <span data-ttu-id="a12a7-108">![API-ikon][step1] </span><span class="sxs-lookup"><span data-stu-id="a12a7-108">![API Icon][step1] </span></span>  
   <span data-ttu-id="a12a7-109">Megjelenik az Update Schema (Séma frissítése) lap az új sémában található fejlesztések részleteit tartalmazó dokumentumra mutató hivatkozással: ![API-ikon][step2]</span><span class="sxs-lookup"><span data-stu-id="a12a7-109">The Update Schema page displays and provides a link to a document that provide details on the improvements in the new schema: ![API Icon][step2]</span></span>

> [!NOTE]
> <span data-ttu-id="a12a7-110">Az **Update Schema** (Séma frissítése) választásakor automatikusan futtatjuk az áttelepítési lépéseket, és átadjuk Önnek a kódkimenetet.</span><span class="sxs-lookup"><span data-stu-id="a12a7-110">When you select **Update Schema**, we automatically run the migration steps and provide the code output for you.</span></span> <span data-ttu-id="a12a7-111">Ennek használatával frissítheti a definíciót, azonban mindenképpen kövesse a kódolás ajánlott eljárásit, például az alábbi **Ajánlott eljárások** című szakaszban foglaltakat.</span><span class="sxs-lookup"><span data-stu-id="a12a7-111">You can use this to update your definition, however, ensure you follow good coding practices such as those outlined in the **Best practices** section below.</span></span>
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a><span data-ttu-id="a12a7-112">Ajánlott eljárások a Logic Apps alkalmazások áttelepítéséhez a legújabb sémaverzióra:</span><span class="sxs-lookup"><span data-stu-id="a12a7-112">Best practices when migrating your Logic apps to the latest schema version:</span></span>
* <span data-ttu-id="a12a7-113">Az áttelepített parancsfájlt új Logic Apps alkalmazásba másolja – ne írja felül a régit, amíg el nem végezte a tesztelést, és meg nem győződött róla, hogy az áttelepített alkalmazás a vártnak megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="a12a7-113">Copy the migrated script to a new Logic App - don't overwrite the old one until you've completed your testing and confirmed the migrated app works as expected.</span></span>
* <span data-ttu-id="a12a7-114">Tesztelje a Logic Apps alkalmazást, **mielőtt** éles környezetben használná.</span><span class="sxs-lookup"><span data-stu-id="a12a7-114">Test your Logic app **before** putting in production</span></span>
* <span data-ttu-id="a12a7-115">Az áttelepítés befejezését követően kezdje el frissíteni a Logic Apps alkalmazásokat a [felügyelt API-k](apis-list.md) használatára, ha lehetséges.</span><span class="sxs-lookup"><span data-stu-id="a12a7-115">After migration completes, start updating your Logic apps to use the [managed APIs](apis-list.md) where possible.</span></span> <span data-ttu-id="a12a7-116">Megkezdheti például a Dropbox 2-es verziójának használatát azon alkalmazások esetén, amelyek a DropBox 1-es verzióját használják.</span><span class="sxs-lookup"><span data-stu-id="a12a7-116">For example, you can start using Dropbox v2, whereever you are using DropBox v1.</span></span>

## <a name="whats-next"></a><span data-ttu-id="a12a7-117">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="a12a7-117">What's next</span></span>
* [<span data-ttu-id="a12a7-118">Információk a Logic Apps alkalmazások manuális áttelepítésével kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="a12a7-118">Learn how to manually migrate your Logic apps</span></span>](../logic-apps/logic-apps-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






