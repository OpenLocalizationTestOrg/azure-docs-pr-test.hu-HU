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
# <a name="validate-xml-for-enterprise-integration"></a><span data-ttu-id="1b974-103">Vállalati integrációs XML érvényesítése</span><span class="sxs-lookup"><span data-stu-id="1b974-103">Validate XML for enterprise integration</span></span>

<span data-ttu-id="1b974-104">Gyakran B2B forgatókönyvekben, szerződés hello partnerek kell ellenőrizze, hogy azok az exchange köszönőüzenetei érvényes adatok feldolgozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="1b974-104">Often in B2B scenarios, hello partners in an agreement must make sure that hello messages they exchange are valid before data processing can start.</span></span> <span data-ttu-id="1b974-105">Dokumentumok egy előre definiált sémának hello vállalati integrációs csomag hello használata hello XML-érvényesítés összekötő segítségével ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="1b974-105">You can validate documents against a predefined schema by using hello use hello XML Validation connector in hello Enterprise Integration Pack.</span></span>

## <a name="validate-a-document-with-hello-xml-validation-connector"></a><span data-ttu-id="1b974-106">A dokumentum hello XML-érvényesítés Connector ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="1b974-106">Validate a document with hello XML Validation connector</span></span>

1. <span data-ttu-id="1b974-107">A logikai alkalmazás létrehozása és [hello app toohello integrációs fiókját](../logic-apps/logic-apps-enterprise-integration-accounts.md "ismerje meg, az integráció fiók tooa logikai alkalmazás toolink") , amely rendelkezik hello sémát, toouse az XML-adatok érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="1b974-107">Create a logic app, and [link hello app toohello integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md "Learn toolink an integration account tooa Logic app") that has hello schema you want toouse for validating XML data.</span></span>

2. <span data-ttu-id="1b974-108">Adja hozzá a **kérelem - amikor egy HTTP-kérelem érkezik** eseményindító tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1b974-108">Add a **Request - When an HTTP request is received** trigger tooyour logic app.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-1.png)

3. <span data-ttu-id="1b974-109">tooadd hello **XML-érvényesítés** művelet, válassza a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="1b974-109">tooadd hello **XML Validation** action, choose **Add an action**.</span></span>

4. <span data-ttu-id="1b974-110">összes hello egy használni kívánt műveletek toohello toofilter meg *xml* hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="1b974-110">toofilter all hello actions toohello one that you want, enter *xml* in hello search box.</span></span> <span data-ttu-id="1b974-111">Válasszon **XML-érvényesítés**.</span><span class="sxs-lookup"><span data-stu-id="1b974-111">Choose **XML Validation**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-2.png)

5. <span data-ttu-id="1b974-112">Válassza ki a megjeleníteni kívánt toovalidate, XML-tartalom toospecify hello **tartalom**.</span><span class="sxs-lookup"><span data-stu-id="1b974-112">toospecify hello XML content that you want toovalidate, select **CONTENT**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-1-5.png)

6. <span data-ttu-id="1b974-113">Jelöljön ki hello törzs címke, a tartalom, amelyet az toovalidate hello.</span><span class="sxs-lookup"><span data-stu-id="1b974-113">Select hello body tag as hello content that you want toovalidate.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-3.png)

7. <span data-ttu-id="1b974-114">toospecify hello sémát, toouse ellenőrzéséhez a korábbi hello *tartalom* adjon meg, válassza a **SÉMANÉV**.</span><span class="sxs-lookup"><span data-stu-id="1b974-114">toospecify hello schema you want toouse for validating hello previous *content* input, choose **SCHEMA NAME**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-4.png)

8. <span data-ttu-id="1b974-115">Mentse a munkáját</span><span class="sxs-lookup"><span data-stu-id="1b974-115">Save your work</span></span>  

    ![](./media/logic-apps-enterprise-integration-xml/xml-5.png)

<span data-ttu-id="1b974-116">Most befejezése a érvényesítési connector beállítása.</span><span class="sxs-lookup"><span data-stu-id="1b974-116">You are now done with setting up your validation connector.</span></span> <span data-ttu-id="1b974-117">Egy valós alkalmazás érdemes toostore érvényesítve hello adatok, például a SalesForce-üzletági (LOB) alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1b974-117">In a real world application, you might want toostore hello validated data in a line-of-business (LOB) app like SalesForce.</span></span> <span data-ttu-id="1b974-118">toosend hello érvényesített kimeneti tooSalesforce, új művelet.</span><span class="sxs-lookup"><span data-stu-id="1b974-118">toosend hello validated output tooSalesforce, add an action.</span></span>

<span data-ttu-id="1b974-119">tootest az érvényesítési műveletet, ellenőrizze a kérelem toohello HTTP végpont.</span><span class="sxs-lookup"><span data-stu-id="1b974-119">tootest your validation action, make a request toohello HTTP endpoint.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b974-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b974-120">Next steps</span></span>
[<span data-ttu-id="1b974-121">További tudnivalók a vállalati integrációs csomag hello</span><span class="sxs-lookup"><span data-stu-id="1b974-121">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")   

