---
title: "aaaHow toomigrate logic apps tooschema verzió 2015-08-01. dátumú előnézeti |} Microsoft Docs"
description: "A logic apps toohello legújabb sémaverzióra egyszerűen áttelepítheti. Csak kövesse az alábbi lépéseket."
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
ms.openlocfilehash: c7b42aaec547eddd28b0c649a3c0625047f9f805
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-logic-apps-tooschema-version-2015-08-01-preview"></a><span data-ttu-id="ec04e-104">Hogyan toomigrate logic apps tooschema verzió 2015-08-01. dátumú előnézeti</span><span class="sxs-lookup"><span data-stu-id="ec04e-104">How toomigrate logic apps tooschema version 2015-08-01-preview</span></span>
<span data-ttu-id="ec04e-105">toomove a meglévő logic apps toohello új sémát, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ec04e-105">toomove your existing logic apps toohello new schema, do hello following:</span></span>  

1. <span data-ttu-id="ec04e-106">Nyissa meg a Logic Apps alkalmazást hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ec04e-106">Open your logic app in hello Azure portal</span></span>  
2. <span data-ttu-id="ec04e-107">Kattintson az Update Schema (Séma frissítése) elemre:</span><span class="sxs-lookup"><span data-stu-id="ec04e-107">Click Update Schema:</span></span>
   
   <span data-ttu-id="ec04e-108">![API-ikon][step1] </span><span class="sxs-lookup"><span data-stu-id="ec04e-108">![API Icon][step1] </span></span>  
   <span data-ttu-id="ec04e-109">hello frissítés séma lap megjeleníti, amelyben és részletekkel szolgálnak hello fejlesztései hello új sémában található hivatkozás tooa dokumentumot: ![API-ikon][step2]</span><span class="sxs-lookup"><span data-stu-id="ec04e-109">hello Update Schema page displays and provides a link tooa document that provide details on hello improvements in hello new schema: ![API Icon][step2]</span></span>

> [!NOTE]
> <span data-ttu-id="ec04e-110">Ha bejelöli **frissítés séma**, azt automatikusan hello áttelepítési lépések futtatásával, és adja meg a hello kódkimenetet.</span><span class="sxs-lookup"><span data-stu-id="ec04e-110">When you select **Update Schema**, we automatically run hello migration steps and provide hello code output for you.</span></span> <span data-ttu-id="ec04e-111">Akkor használja ezt a tooupdate a definíciót, azonban, mindenképpen kövesse az ajánlott eljárásit, például hello **ajánlott eljárások** az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="ec04e-111">You can use this tooupdate your definition, however, ensure you follow good coding practices such as those outlined in hello **Best practices** section below.</span></span>
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-toohello-latest-schema-version"></a><span data-ttu-id="ec04e-112">Ajánlott eljárások a Logic apps toohello legújabb sémaverzióra áttelepítésekor:</span><span class="sxs-lookup"><span data-stu-id="ec04e-112">Best practices when migrating your Logic apps toohello latest schema version:</span></span>
* <span data-ttu-id="ec04e-113">Másolás hello át parancsfájl tooa új logikai alkalmazás – ne írja felül hello régi egyik fejezze be a tesztelés és erősítette hello áttelepített alkalmazás megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="ec04e-113">Copy hello migrated script tooa new Logic App - don't overwrite hello old one until you've completed your testing and confirmed hello migrated app works as expected.</span></span>
* <span data-ttu-id="ec04e-114">Tesztelje a Logic Apps alkalmazást, **mielőtt** éles környezetben használná.</span><span class="sxs-lookup"><span data-stu-id="ec04e-114">Test your Logic app **before** putting in production</span></span>
* <span data-ttu-id="ec04e-115">Az áttelepítés befejezése után indítsa el a Logic apps toouse hello frissítése [felügyelt API-k](apis-list.md) ahol csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="ec04e-115">After migration completes, start updating your Logic apps toouse hello [managed APIs](apis-list.md) where possible.</span></span> <span data-ttu-id="ec04e-116">Megkezdheti például a Dropbox 2-es verziójának használatát azon alkalmazások esetén, amelyek a DropBox 1-es verzióját használják.</span><span class="sxs-lookup"><span data-stu-id="ec04e-116">For example, you can start using Dropbox v2, whereever you are using DropBox v1.</span></span>

## <a name="whats-next"></a><span data-ttu-id="ec04e-117">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="ec04e-117">What's next</span></span>
* [<span data-ttu-id="ec04e-118">Ismerje meg, hogyan toomanually át a Logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ec04e-118">Learn how toomanually migrate your Logic apps</span></span>](../logic-apps/logic-apps-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






