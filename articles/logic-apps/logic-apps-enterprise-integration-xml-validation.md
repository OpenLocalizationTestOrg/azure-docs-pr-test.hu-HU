---
title: aaaValidate XML - Azure Logic Apps |} Microsoft Docs
description: "Ellenőrizze az XML a sémák Azure Logic Apps és B2B forgatókönyvek hello vállalati integrációs csomag segítségével"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: d700588f-2d8a-4c92-93eb-e1e6e250e760
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 81f662d0ddf908657b54de8af0a75fff55782ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="validate-xml-for-enterprise-integration"></a>Vállalati integrációs XML érvényesítése

Gyakran B2B forgatókönyvekben, szerződés hello partnerek kell ellenőrizze, hogy azok az exchange köszönőüzenetei érvényes adatok feldolgozásának megkezdése előtt. Dokumentumok egy előre definiált sémának hello vállalati integrációs csomag hello használata hello XML-érvényesítés összekötő segítségével ellenőrizheti.

## <a name="validate-a-document-with-hello-xml-validation-connector"></a>A dokumentum hello XML-érvényesítés Connector ellenőrzése

1. A logikai alkalmazás létrehozása és [hello app toohello integrációs fiókját](../logic-apps/logic-apps-enterprise-integration-accounts.md "ismerje meg, az integráció fiók tooa logikai alkalmazás toolink") , amely rendelkezik hello sémát, toouse az XML-adatok érvényesítése.

2. Adja hozzá a **kérelem - amikor egy HTTP-kérelem érkezik** eseményindító tooyour logikai alkalmazást.

    ![](./media/logic-apps-enterprise-integration-xml/xml-1.png)

3. tooadd hello **XML-érvényesítés** művelet, válassza a **művelet hozzáadása**.

4. összes hello egy használni kívánt műveletek toohello toofilter meg *xml* hello Keresés mezőbe. Válasszon **XML-érvényesítés**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-2.png)

5. Válassza ki a megjeleníteni kívánt toovalidate, XML-tartalom toospecify hello **tartalom**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-1-5.png)

6. Jelöljön ki hello törzs címke, a tartalom, amelyet az toovalidate hello.

    ![](./media/logic-apps-enterprise-integration-xml/xml-3.png)

7. toospecify hello sémát, toouse ellenőrzéséhez a korábbi hello *tartalom* adjon meg, válassza a **SÉMANÉV**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-4.png)

8. Mentse a munkáját  

    ![](./media/logic-apps-enterprise-integration-xml/xml-5.png)

Most befejezése a érvényesítési connector beállítása. Egy valós alkalmazás érdemes toostore érvényesítve hello adatok, például a SalesForce-üzletági (LOB) alkalmazásban. toosend hello érvényesített kimeneti tooSalesforce, új művelet.

tootest az érvényesítési műveletet, ellenőrizze a kérelem toohello HTTP végpont.

## <a name="next-steps"></a>Következő lépések
[További tudnivalók a vállalati integrációs csomag hello](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")   

