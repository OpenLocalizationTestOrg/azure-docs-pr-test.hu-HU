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
# <a name="validate-and-transform-xml-encode-and-decode-flat-files-and-enrich-messages-features-in-logic-apps"></a>Ellenőrizze és átalakítható XML, kódolása és egybesimított fájlok és funkciógazdagabbá teheti a logic apps üzenetek szolgáltatásai

A logic apps segítségével, akkor hello képességét tooprocess XML-üzenetek küldött és fogadott. Ez a szolgáltatás része hello vállalati integrációs csomagot. Azoknak a felhasználóknak a BizTalk Serveren háttérrel hello vállalati integrációs csomag hasonló képességek tootransform lehetővé teszi és üzenetek ellenőrzése, egybesimított fájlok, és még XPath tooenrich használja vagy tulajdonságokat kinyerése egy üzenetet. 

Azoknak a felhasználóknak, akik új toothis terület ezek a funkciók bontsa ki, hogyan üzenetek feldolgozásához a munkafolyamaton belül. Például ha a vállalatok forgatókönyvek, és együttműködik a megadott XML-sémák, hello vállalati integrációs csomag tooenhance használhatja hogyan vállalata dolgozza fel ezeket az üzeneteket. 

hello vállalati integrációs csomag tartalmazza: 

* [XML-érvényesítés](logic-apps-enterprise-integration-xml-validation.md "további információ a XML-üzenet érvényesítés") -ellenőrzése egy bejövő vagy kimenő XML üzenet egy adott sémának.
* [XML-átalakító](../logic-apps/logic-apps-enterprise-integration-transform.md "XML üzenet átalakítások és a maps ismertetése") - alakítható, vagy testre szabhatja a követelményeknek, vagy egy partner hello követelményei alapján XML-üzenetben.
* [Fájl kódolási és dekódolási egybesimított fájl lapos](logic-apps-enterprise-integration-flatfile.md "további információ a egybesimított fájl kódolás/dekódolás") - kódolni vagy egybesimított fájl dekódolására. Például SAP fogad el, és egybesimított fájl formátumban megadva IDOC-fájlok küld. Integráció számos platformon XML-üzenetek, beleértve a Logic Apps létrehozása. Tehát, hogy használ hello egybesimított fájl kódoló túl "átalakítani" XML-fájlok tooflat fájlok logikai alkalmazás is létrehozhat. 
* [XPath](https://msdn.microsoft.com/library/mt643789.aspx) - üzenet kiegészítése, és konkrét tulajdonságok kinyerése üdvözlőüzenetére. Ezután a használja hello kibontott tulajdonságok tooa tooroute hello üzenetcél vagy köztes végpontot.

## <a name="try-it-out"></a>Próbálja ki!
[Egy teljesen működőképes logikai alkalmazás telepítése ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub minta) az Azure Logic Apps hello XML-szolgáltatások használatával.

## <a name="learn-more"></a>Részletek
[További tudnivalók a vállalati integrációs csomag hello](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")
