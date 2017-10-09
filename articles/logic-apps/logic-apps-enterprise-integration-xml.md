---
title: "a munkafolyamatok - Azure Logic Apps állapotüzenetek XML aaaWorking |} Microsoft Docs"
description: "Feldolgozni, ellenőrizze, átalakítás és funkciógazdagabbá teheti az XML logic Apps alkalmazásokat és üzleti-tooscenarios használatával üzenet hello vállalati integrációs csomag"
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
ms.openlocfilehash: f90ae89fef0a4bd17286adbce398e573940bb790
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="validate-and-transform-xml-encode-and-decode-flat-files-and-enrich-messages-features-in-logic-apps"></a><span data-ttu-id="a3657-103">Ellenőrizze és átalakítható XML, kódolása és egybesimított fájlok és funkciógazdagabbá teheti a logic apps üzenetek szolgáltatásai</span><span class="sxs-lookup"><span data-stu-id="a3657-103">Validate and transform XML, encode and decode flat files, and enrich messages features in logic apps</span></span>

<span data-ttu-id="a3657-104">A logic apps segítségével, akkor hello képességét tooprocess XML-üzenetek küldött és fogadott.</span><span class="sxs-lookup"><span data-stu-id="a3657-104">Using logic apps, you have hello ability tooprocess XML messages that you send and receive.</span></span> <span data-ttu-id="a3657-105">Ez a szolgáltatás része hello vállalati integrációs csomagot.</span><span class="sxs-lookup"><span data-stu-id="a3657-105">This feature is included with hello Enterprise Integration Pack.</span></span> <span data-ttu-id="a3657-106">Azoknak a felhasználóknak a BizTalk Serveren háttérrel hello vállalati integrációs csomag hasonló képességek tootransform lehetővé teszi és üzenetek ellenőrzése, egybesimított fájlok, és még XPath tooenrich használja vagy tulajdonságokat kinyerése egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="a3657-106">For those users with a BizTalk Server background, hello Enterprise Integration Pack gives you similar abilities tootransform and validate messages, work with flat files, and even use XPath tooenrich or extract specific properties from a message.</span></span> 

<span data-ttu-id="a3657-107">Azoknak a felhasználóknak, akik új toothis terület ezek a funkciók bontsa ki, hogyan üzenetek feldolgozásához a munkafolyamaton belül.</span><span class="sxs-lookup"><span data-stu-id="a3657-107">For those users who are new toothis space, these features expand how you process messages within your workflow.</span></span> <span data-ttu-id="a3657-108">Például ha a vállalatok forgatókönyvek, és együttműködik a megadott XML-sémák, hello vállalati integrációs csomag tooenhance használhatja hogyan vállalata dolgozza fel ezeket az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="a3657-108">For example, if you are in a business-to-business scenario, and work with specific XML schemas, then you can use hello Enterprise Integration Pack tooenhance how your company processes these messages.</span></span> 

<span data-ttu-id="a3657-109">hello vállalati integrációs csomag tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="a3657-109">hello Enterprise Integration Pack includes:</span></span> 

* <span data-ttu-id="a3657-110">[XML-érvényesítés](logic-apps-enterprise-integration-xml-validation.md "további információ a XML-üzenet érvényesítés") -ellenőrzése egy bejövő vagy kimenő XML üzenet egy adott sémának.</span><span class="sxs-lookup"><span data-stu-id="a3657-110">[XML validation](logic-apps-enterprise-integration-xml-validation.md "Learn about XML message validation") - Validate an incoming or outgoing XML message against a specific schema.</span></span>
* <span data-ttu-id="a3657-111">[XML-átalakító](../logic-apps/logic-apps-enterprise-integration-transform.md "XML üzenet átalakítások és a maps ismertetése") - alakítható, vagy testre szabhatja a követelményeknek, vagy egy partner hello követelményei alapján XML-üzenetben.</span><span class="sxs-lookup"><span data-stu-id="a3657-111">[XML transform](../logic-apps/logic-apps-enterprise-integration-transform.md "Learn about XML message transformations and maps") - Convert or customize an XML message based on your requirements, or hello requirements of a partner.</span></span>
* <span data-ttu-id="a3657-112">[Fájl kódolási és dekódolási egybesimított fájl lapos](logic-apps-enterprise-integration-flatfile.md "további információ a egybesimított fájl kódolás/dekódolás") - kódolni vagy egybesimított fájl dekódolására.</span><span class="sxs-lookup"><span data-stu-id="a3657-112">[Flat file encoding and flat file decoding](logic-apps-enterprise-integration-flatfile.md "Learn about flat file encoding/decoding") - Encode or decode a flat file.</span></span> <span data-ttu-id="a3657-113">Például SAP fogad el, és egybesimított fájl formátumban megadva IDOC-fájlok küld.</span><span class="sxs-lookup"><span data-stu-id="a3657-113">For example, SAP accepts and sends IDOC files in flat file format.</span></span> <span data-ttu-id="a3657-114">Integráció számos platformon XML-üzenetek, beleértve a Logic Apps létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a3657-114">Many integration platforms create XML messages, including Logic Apps.</span></span> <span data-ttu-id="a3657-115">Tehát, hogy használ hello egybesimított fájl kódoló túl "átalakítani" XML-fájlok tooflat fájlok logikai alkalmazás is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="a3657-115">So, you can create a logic app that uses hello flat file encoder too"convert" XML files tooflat files.</span></span> 
* <span data-ttu-id="a3657-116">[XPath](https://msdn.microsoft.com/library/mt643789.aspx) - üzenet kiegészítése, és konkrét tulajdonságok kinyerése üdvözlőüzenetére.</span><span class="sxs-lookup"><span data-stu-id="a3657-116">[XPath](https://msdn.microsoft.com/library/mt643789.aspx) - Enrich a message and extract specific properties from hello message.</span></span> <span data-ttu-id="a3657-117">Ezután a használja hello kibontott tulajdonságok tooa tooroute hello üzenetcél vagy köztes végpontot.</span><span class="sxs-lookup"><span data-stu-id="a3657-117">You can then use hello extracted properties tooroute hello message tooa destination, or an intermediary endpoint.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="a3657-118">Próbálja ki!</span><span class="sxs-lookup"><span data-stu-id="a3657-118">Try it out</span></span>
<span data-ttu-id="a3657-119">[Egy teljesen működőképes logikai alkalmazás telepítése ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub minta) az Azure Logic Apps hello XML-szolgáltatások használatával.</span><span class="sxs-lookup"><span data-stu-id="a3657-119">[Deploy a fully operational logic app ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub sample) by using hello XML features in Azure Logic Apps.</span></span>

## <a name="learn-more"></a><span data-ttu-id="a3657-120">Részletek</span><span class="sxs-lookup"><span data-stu-id="a3657-120">Learn more</span></span>
[<span data-ttu-id="a3657-121">További tudnivalók a vállalati integrációs csomag hello</span><span class="sxs-lookup"><span data-stu-id="a3657-121">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")
