---
title: "A munkafolyamatok - Azure Logic Apps XML-üzenetek használata |} Microsoft Docs"
description: "Feldolgozni, ellenőrizze, átalakítás és funkciógazdagabbá teheti a logic Apps alkalmazásokat és üzleti XML-üzenetek-forgatókönyvekre, a vállalati integrációs csomag segítségével"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 47672dc4-1caa-44e5-b8cb-68ec3a76b7dc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 02/27/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 3fec4935f5317be4bf8c9e05f1c24a7c05381b1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="validate-and-transform-xml-encode-and-decode-flat-files-and-enrich-messages-features-in-logic-apps"></a><span data-ttu-id="25255-103">Ellenőrizze és átalakítható XML, kódolása és egybesimított fájlok és funkciógazdagabbá teheti a logic apps üzenetek szolgáltatásai</span><span class="sxs-lookup"><span data-stu-id="25255-103">Validate and transform XML, encode and decode flat files, and enrich messages features in logic apps</span></span>

<span data-ttu-id="25255-104">A logic apps segítségével, lehetősége nyílik XML üzenetek küldésére és fogadására.</span><span class="sxs-lookup"><span data-stu-id="25255-104">Using logic apps, you have the ability to process XML messages that you send and receive.</span></span> <span data-ttu-id="25255-105">Ez a szolgáltatás része a vállalati integrációs csomagot.</span><span class="sxs-lookup"><span data-stu-id="25255-105">This feature is included with the Enterprise Integration Pack.</span></span> <span data-ttu-id="25255-106">Azoknak a felhasználóknak a BizTalk Serveren háttérrel a vállalati integrációs csomag lehetővé teszi az átalakítási és üzenetek ellenőrzése, egyszerű fájlok, és még használatához XPath kiegészítése, illetve konkrét tulajdonságok üzenetből hasonló képességek.</span><span class="sxs-lookup"><span data-stu-id="25255-106">For those users with a BizTalk Server background, the Enterprise Integration Pack gives you similar abilities to transform and validate messages, work with flat files, and even use XPath to enrich or extract specific properties from a message.</span></span> 

<span data-ttu-id="25255-107">Azoknak a felhasználóknak, akik még nem használta a hely a ezek a funkciók bontsa ki, hogyan üzenetek feldolgozásához a munkafolyamaton belül.</span><span class="sxs-lookup"><span data-stu-id="25255-107">For those users who are new to this space, these features expand how you process messages within your workflow.</span></span> <span data-ttu-id="25255-108">Például ha a vállalatok forgatókönyvek, és együttműködik a megadott XML-sémák, használhatja a vállalati integrációs csomag javítása érdekében hogyan vállalata dolgozza fel ezeket az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="25255-108">For example, if you are in a business-to-business scenario, and work with specific XML schemas, then you can use the Enterprise Integration Pack to enhance how your company processes these messages.</span></span> 

<span data-ttu-id="25255-109">A vállalati integrációs csomag tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="25255-109">The Enterprise Integration Pack includes:</span></span> 

* <span data-ttu-id="25255-110">[XML-érvényesítés](logic-apps-enterprise-integration-xml-validation.md "további információ a XML-üzenet érvényesítés") -ellenőrzése egy bejövő vagy kimenő XML üzenet egy adott sémának.</span><span class="sxs-lookup"><span data-stu-id="25255-110">[XML validation](logic-apps-enterprise-integration-xml-validation.md "Learn about XML message validation") - Validate an incoming or outgoing XML message against a specific schema.</span></span>
* <span data-ttu-id="25255-111">[XML-átalakító](../logic-apps/logic-apps-enterprise-integration-transform.md "XML üzenet átalakítások és a maps ismertetése") - alakítható, vagy testre szabhatja a követelmények, vagy egy partner követelményei alapján XML-üzenetben.</span><span class="sxs-lookup"><span data-stu-id="25255-111">[XML transform](../logic-apps/logic-apps-enterprise-integration-transform.md "Learn about XML message transformations and maps") - Convert or customize an XML message based on your requirements, or the requirements of a partner.</span></span>
* <span data-ttu-id="25255-112">[Fájl kódolási és dekódolási egybesimított fájl lapos](logic-apps-enterprise-integration-flatfile.md "további információ a egybesimított fájl kódolás/dekódolás") - kódolni vagy egybesimított fájl dekódolására.</span><span class="sxs-lookup"><span data-stu-id="25255-112">[Flat file encoding and flat file decoding](logic-apps-enterprise-integration-flatfile.md "Learn about flat file encoding/decoding") - Encode or decode a flat file.</span></span> <span data-ttu-id="25255-113">Például SAP fogad el, és egybesimított fájl formátumban megadva IDOC-fájlok küld.</span><span class="sxs-lookup"><span data-stu-id="25255-113">For example, SAP accepts and sends IDOC files in flat file format.</span></span> <span data-ttu-id="25255-114">Integráció számos platformon XML-üzenetek, beleértve a Logic Apps létrehozása.</span><span class="sxs-lookup"><span data-stu-id="25255-114">Many integration platforms create XML messages, including Logic Apps.</span></span> <span data-ttu-id="25255-115">Igen a egybesimított fájl kódoló "átalakítani" XML-fájlok egybesimított fájlokba használó logikai alkalmazás is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="25255-115">So, you can create a logic app that uses the flat file encoder to "convert" XML files to flat files.</span></span> 
* <span data-ttu-id="25255-116">[XPath](https://msdn.microsoft.com/library/mt643789.aspx) - funkciógazdagabbá teheti az üzenetet, és konkrét tulajdonságok kinyerése az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="25255-116">[XPath](https://msdn.microsoft.com/library/mt643789.aspx) - Enrich a message and extract specific properties from the message.</span></span> <span data-ttu-id="25255-117">A kibontott tulajdonságok segítségével majd átirányíthatja az üzenet egy cél, vagy köztes végpont.</span><span class="sxs-lookup"><span data-stu-id="25255-117">You can then use the extracted properties to route the message to a destination, or an intermediary endpoint.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="25255-118">Próbálja ki!</span><span class="sxs-lookup"><span data-stu-id="25255-118">Try it out</span></span>
<span data-ttu-id="25255-119">[Egy teljesen működőképes logikai alkalmazás telepítése ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub minta) az XML-szolgáltatások az Azure Logic Apps segítségével.</span><span class="sxs-lookup"><span data-stu-id="25255-119">[Deploy a fully operational logic app ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub sample) by using the XML features in Azure Logic Apps.</span></span>

## <a name="learn-more"></a><span data-ttu-id="25255-120">Részletek</span><span class="sxs-lookup"><span data-stu-id="25255-120">Learn more</span></span>
[<span data-ttu-id="25255-121">További tudnivalók a vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="25255-121">Learn more about the Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")
