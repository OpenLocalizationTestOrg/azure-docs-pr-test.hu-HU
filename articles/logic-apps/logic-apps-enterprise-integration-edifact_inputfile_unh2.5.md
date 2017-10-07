---
title: "Alkalmazások B2B edifact dekódolása aaaLogic megoldásához UNH2.5 - Azure Logic Apps |} Microsoft Docs"
description: "Az Azure Logic Apps B2B edifact dekódolása UNH2.5 feloldása"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 6d85242d0f828fa52cdc9689938f3ba1e51b1183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toohandle-edifact-documents-having-unh25-segment"></a>Hogyan toohandle EDIFACT dokumentumok UNH2.5 rendelkező szegmens
Ha UNH2.5 hello EDIFACT dokumentum szerepel, akkor használatos séma keresési. 

Példa: hello UNH mezője **EAN008** hello EDIFACT üzenetben  
UNH + SSDD1 + RENDELÉSEK: D: 03B: UN:**EAN008**"  

Lépéseket toofollow toohandle köszönőüzenetei 
1. Hello séma frissítése
2. Hello megállapodás beállításainak ellenőrzése  

## <a name="update-hello-schema"></a>Hello séma frissítése
tooprocess üdvözlőüzenetére, toodeploy hello UNH2.5 gyökércsomópontjának neve rendelkező sémát kell.  A megadott példa, hello séma legfelső szintű név lesz **EFACT_D03B_ORDERS_EAN008**  

Az egyes D03B_ORDERS az egy másik UNH2.5 szegmens toodeploy az egyéni séma kellene lennie.  

## <a name="add-schema-toohello-edifact-agreement"></a>Adja hozzá a séma toohello EDIFACT-megállapodás
### <a name="edifact-decode"></a>EDIFACT dekódolása
tooDecode hello bejövő üzenet, hello séma konfigurálása hello EDIFACT megállapodás kap beállításai
1. Hello séma toohello integrációs fiók hozzáadása    
2. Hello séma konfigurálása a hello EDIFACT megállapodás megkapja a beállításokat. 
3. Jelöljön ki EDIFACT szerződést, majd **módosítsa a JSON**.  Hello Receive megállapodás UNH2.5 előnyei **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)

### <a name="edifact-encode"></a>EDIFACT kódolása
tooEncode hello bejövő üzenet, hello séma hello EDIFACT megállapodás küldési beállítások konfigurálása
1. Hello séma toohello integrációs fiók hozzáadása    
2. Hello séma hello EDIFACT megállapodás küldési beállításainak konfigurálása. 
3. Jelöljön ki EDIFACT szerződést, majd **módosítsa a JSON**.  Hello küldése megállapodás UNH2.5 előnyei **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)

## <a name="next-steps"></a>Következő lépések
* [További információ az integrációs fiók megállapodások](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  