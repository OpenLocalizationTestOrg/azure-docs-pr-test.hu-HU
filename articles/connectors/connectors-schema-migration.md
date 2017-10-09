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
# <a name="how-toomigrate-logic-apps-tooschema-version-2015-08-01-preview"></a>Hogyan toomigrate logic apps tooschema verzió 2015-08-01. dátumú előnézeti
toomove a meglévő logic apps toohello új sémát, a következő hello:  

1. Nyissa meg a Logic Apps alkalmazást hello Azure-portálon  
2. Kattintson az Update Schema (Séma frissítése) elemre:
   
   ![API-ikon][step1]   
   hello frissítés séma lap megjeleníti, amelyben és részletekkel szolgálnak hello fejlesztései hello új sémában található hivatkozás tooa dokumentumot: ![API-ikon][step2]

> [!NOTE]
> Ha bejelöli **frissítés séma**, azt automatikusan hello áttelepítési lépések futtatásával, és adja meg a hello kódkimenetet. Akkor használja ezt a tooupdate a definíciót, azonban, mindenképpen kövesse az ajánlott eljárásit, például hello **ajánlott eljárások** az alábbi szakasz.
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-toohello-latest-schema-version"></a>Ajánlott eljárások a Logic apps toohello legújabb sémaverzióra áttelepítésekor:
* Másolás hello át parancsfájl tooa új logikai alkalmazás – ne írja felül hello régi egyik fejezze be a tesztelés és erősítette hello áttelepített alkalmazás megfelelően működik-e.
* Tesztelje a Logic Apps alkalmazást, **mielőtt** éles környezetben használná.
* Az áttelepítés befejezése után indítsa el a Logic apps toouse hello frissítése [felügyelt API-k](apis-list.md) ahol csak lehetséges. Megkezdheti például a Dropbox 2-es verziójának használatát azon alkalmazások esetén, amelyek a DropBox 1-es verzióját használják.

## <a name="whats-next"></a>A következő lépések
* [Ismerje meg, hogyan toomanually át a Logic Apps alkalmazások](../logic-apps/logic-apps-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






