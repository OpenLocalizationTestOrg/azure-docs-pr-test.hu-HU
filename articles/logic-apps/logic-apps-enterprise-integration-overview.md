---
title: "Vállalati B2B - Azure Logic Apps-integráció |} Microsoft Docs"
description: "B2B munkafolyamatok építhetők fel, és vállalati integrációs feladatokhoz a logic apps a vállalati integrációs csomaggal támogatás"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: dd517c4d-1701-4247-b83c-183c4d8d8aae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 9462707db03ecfcc3d5186ce7ded8655ad3bdcc9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="overview-b2b-scenarios-and-communication-with-the-enterprise-integration-pack"></a><span data-ttu-id="7ebd2-103">Áttekintés: B2B forgatókönyvek és a vállalati integrációs csomag kommunikáció</span><span class="sxs-lookup"><span data-stu-id="7ebd2-103">Overview: B2B scenarios and communication with the Enterprise Integration Pack</span></span>

<span data-ttu-id="7ebd2-104">Az üzleti vállalatközi (B2B) munkafolyamatok és az Azure Logic Apps zökkenőmentes kommunikációt engedélyezheti a vállalati integrációs feladatokhoz a Microsoft felhőalapú megoldással, a vállalati integrációs csomagot.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-104">For business-to-business (B2B) workflows and seamless communication with Azure Logic Apps, you can enable enterprise integration scenarios with Microsoft's cloud-based solution, the Enterprise Integration Pack.</span></span> <span data-ttu-id="7ebd2-105">A szervezetek továbbíthatja az üzenetek elektronikus úton, még akkor is, ha azok használnak a különböző protokollok és formázza az adathordozót.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-105">Organizations can exchange messages electronically, even if they use different protocols and formats.</span></span> <span data-ttu-id="7ebd2-106">A csomag különböző formátumokban átalakítja az olyan formátumra, amely a szervezet rendszerek értelmezi, és feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-106">The pack transforms different formats into a format that organizations' systems can interpret and process.</span></span> <span data-ttu-id="7ebd2-107">A szervezetek továbbíthatja az üzeneteket keresztül ipari szabványnak számító protokollokat, beleértve a [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), és [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md).</span><span class="sxs-lookup"><span data-stu-id="7ebd2-107">Organizations can exchange messages through industry-standard protocols, including [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), and [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md).</span></span> <span data-ttu-id="7ebd2-108">Titkosítás és a digitális aláírás-üzenetek is biztosíthassa.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-108">You can also secure messages with both encryption and digital signatures.</span></span>

<span data-ttu-id="7ebd2-109">Ha ismeri a BizTalk Server vagy a Microsoft Azure BizTalk szolgáltatások, nincsenek a vállalati integrációs szolgáltatások, könnyen használható, mert a legtöbb fogalmak hasonlóak.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-109">If you are familiar with BizTalk Server or Microsoft Azure BizTalk Services, the Enterprise Integration features are easy to use because most concepts are similar.</span></span> <span data-ttu-id="7ebd2-110">Egy fő különbség az, hogy a vállalati integrációs integrációs fiókok használja a tárolása és kezelése az összetevők B2B kommunikációban használt leegyszerűsítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-110">One major difference is that Enterprise Integration uses integration accounts to simplify the storage and management of artifacts used in B2B communications.</span></span> 

<span data-ttu-id="7ebd2-111">A vállalati integrációs csomag architektúráját, "integrációs fiókok" alapul.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-111">Architecturally, the Enterprise Integration Pack is based on "integration accounts".</span></span> <span data-ttu-id="7ebd2-112">Ezek a fiókok olyan felhőalapú tárolók tároló összes a összetevők, például a sémák, partnerek, tanúsítványok, a maps és megállapodások.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-112">These accounts are cloud-based containers that store all your artifacts, like schemas, partners, certificates, maps, and agreements.</span></span> <span data-ttu-id="7ebd2-113">Ezen összetevők is használhatja, tervezése, telepítése és karbantartása a B2B alkalmazások és a logic apps B2B munkafolyamatok építhetők fel.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-113">You can use these artifacts to design, deploy, and maintain your B2B apps and also to build B2B workflows for logic apps.</span></span> <span data-ttu-id="7ebd2-114">Azonban ezen összetevők használata előtt meg kell először az integráció fiók összekötése a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-114">But before you can use these artifacts, you must first link your integration account to your logic app.</span></span> <span data-ttu-id="7ebd2-115">Ezt követően a Logic Apps alkalmazást integrációs fiókja összetevők férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-115">After that, your logic app can access your integration account's artifacts.</span></span>

## <a name="why-should-you-use-enterprise-integration"></a><span data-ttu-id="7ebd2-116">Miért kell használni a vállalati integrációs?</span><span class="sxs-lookup"><span data-stu-id="7ebd2-116">Why should you use enterprise integration?</span></span>

* <span data-ttu-id="7ebd2-117">A vállalati integrációs tárolhatja az összetevők egy helyen--integrációs fiókját.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-117">With enterprise integration, you can store all your artifacts in one place -- your integration account.</span></span>
* <span data-ttu-id="7ebd2-118">B2B munkafolyamatok építhetők fel, és a külső szolgáltatásként szoftver (SaaS) alkalmazások, a helyszíni alkalmazásokhoz és az egyéni alkalmazások integrálása az Azure Logic Apps és az összekötők használatával.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-118">You can build B2B workflows and integrate with third-party software-as-service (SaaS) apps, on-premises apps, and custom apps by using the Azure Logic Apps engine and all its connectors.</span></span>
* <span data-ttu-id="7ebd2-119">A logic Apps alkalmazások az Azure functions létrehozhat egyéni kódot.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-119">You can create custom code for your logic apps with Azure functions.</span></span>

## <a name="how-to-get-started-with-enterprise-integration"></a><span data-ttu-id="7ebd2-120">Ismerkedés a vállalati integrációs hogyan?</span><span class="sxs-lookup"><span data-stu-id="7ebd2-120">How to get started with enterprise integration?</span></span>

<span data-ttu-id="7ebd2-121">Létrehozhatja és a vállalati integrációs csomag révén a Logic App Designer B2B alkalmazások kezelése a **Azure-portálon**.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-121">You can build and manage B2B apps with the Enterprise Integration Pack through the Logic App Designer in the **Azure portal**.</span></span> <span data-ttu-id="7ebd2-122">Emellett kezelhetők a logic apps a [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "a Logic apps PowerShell témaköreiben talál").</span><span class="sxs-lookup"><span data-stu-id="7ebd2-122">You can also manage your logic apps with [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logic apps PowerShell topics").</span></span>

<span data-ttu-id="7ebd2-123">Alkalmazások létrehozása az Azure portálon előtt végre kell hajtania magas szintű lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="7ebd2-123">Here are the high-level steps you must take before you can create apps in the Azure portal:</span></span>

![Kép – áttekintés](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a><span data-ttu-id="7ebd2-125">Mik azok a olyan gyakori forgatókönyveket tartalmaz?</span><span class="sxs-lookup"><span data-stu-id="7ebd2-125">What are some common scenarios?</span></span>

<span data-ttu-id="7ebd2-126">Vállalati integrációs ezeket az ipari szabványok támogatja:</span><span class="sxs-lookup"><span data-stu-id="7ebd2-126">Enterprise Integration supports these industry standards:</span></span>

* <span data-ttu-id="7ebd2-127">EDI - elektronikus adatcsere</span><span class="sxs-lookup"><span data-stu-id="7ebd2-127">EDI - Electronic Data Interchange</span></span>
* <span data-ttu-id="7ebd2-128">EAI - vállalati alkalmazások integrálása</span><span class="sxs-lookup"><span data-stu-id="7ebd2-128">EAI - Enterprise Application Integration</span></span>

## <a name="heres-what-you-need-to-get-started"></a><span data-ttu-id="7ebd2-129">Ez szükséges a kezdéshez</span><span class="sxs-lookup"><span data-stu-id="7ebd2-129">Here's what you need to get started</span></span>

* <span data-ttu-id="7ebd2-130">Azure-előfizetés integrációs fiókkal</span><span class="sxs-lookup"><span data-stu-id="7ebd2-130">An Azure subscription with an integration account</span></span>
* <span data-ttu-id="7ebd2-131">A Visual Studio 2015-leképezések és sémák létrehozása</span><span class="sxs-lookup"><span data-stu-id="7ebd2-131">Visual Studio 2015 to create maps and schemas</span></span>
* [<span data-ttu-id="7ebd2-132">A Microsoft Azure Logic Apps Enterprise-integrációs eszközök a Visual Studio 2015 2.0</span><span class="sxs-lookup"><span data-stu-id="7ebd2-132">Microsoft Azure Logic Apps Enterprise Integration Tools for Visual Studio 2015 2.0</span></span>](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a><span data-ttu-id="7ebd2-133">Kipróbálás</span><span class="sxs-lookup"><span data-stu-id="7ebd2-133">Try it now</span></span>

<span data-ttu-id="7ebd2-134">[Egy teljesen működőképes minta AS2 küldési telepítése, és a logikai alkalmazás fogadni](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) , amely a B2B funkciókat használ az Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-134">[Deploy a fully operational sample AS2 send & receive logic app](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) that uses the B2B features for Azure Logic Apps.</span></span>

## <a name="learn-more"></a><span data-ttu-id="7ebd2-135">Részletek</span><span class="sxs-lookup"><span data-stu-id="7ebd2-135">Learn more</span></span>
* [<span data-ttu-id="7ebd2-136">Szerződések</span><span class="sxs-lookup"><span data-stu-id="7ebd2-136">Agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")
* [<span data-ttu-id="7ebd2-137">A vállalatok (B2B) környezetek</span><span class="sxs-lookup"><span data-stu-id="7ebd2-137">Business to Business (B2B) scenarios</span></span>](../logic-apps/logic-apps-enterprise-integration-b2b.md "megtudhatja, hogyan hozzon létre a Logic apps B2B szolgáltatások")  
* [<span data-ttu-id="7ebd2-138">Tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="7ebd2-138">Certificates</span></span>](logic-apps-enterprise-integration-certificates.md "vállalati integrációs tanúsítványokkal kapcsolatos további információ")
* [<span data-ttu-id="7ebd2-139">Fájl kódolás/dekódolás lapos</span><span class="sxs-lookup"><span data-stu-id="7ebd2-139">Flat file encoding/decoding</span></span>](logic-apps-enterprise-integration-flatfile.md "útmutató kódolni és dekódolási egybesimított fájl tartalma")  
* [<span data-ttu-id="7ebd2-140">Integrációs fiókok</span><span class="sxs-lookup"><span data-stu-id="7ebd2-140">Integration accounts</span></span>](../logic-apps/logic-apps-enterprise-integration-accounts.md "integrációs fiókok ismertetése")
* [<span data-ttu-id="7ebd2-141">Leképezhető</span><span class="sxs-lookup"><span data-stu-id="7ebd2-141">Maps</span></span>](../logic-apps/logic-apps-enterprise-integration-maps.md "vállalati integrációs maps ismertetése")
* [<span data-ttu-id="7ebd2-142">Partnerek</span><span class="sxs-lookup"><span data-stu-id="7ebd2-142">Partners</span></span>](logic-apps-enterprise-integration-partners.md "vállalati integrációs partnerek ismertetése")
* [<span data-ttu-id="7ebd2-143">Sémák</span><span class="sxs-lookup"><span data-stu-id="7ebd2-143">Schemas</span></span>](logic-apps-enterprise-integration-schemas.md "további információ a vállalati integrációs sémák")
* [<span data-ttu-id="7ebd2-144">XML-üzenet érvényesítés</span><span class="sxs-lookup"><span data-stu-id="7ebd2-144">XML message validation</span></span>](logic-apps-enterprise-integration-xml.md "megtudhatja, hogyan XML-üzenetek a Logic apps ellenőrzése")
* [<span data-ttu-id="7ebd2-145">XML-átalakító</span><span class="sxs-lookup"><span data-stu-id="7ebd2-145">XML transform</span></span>](logic-apps-enterprise-integration-transform.md "vállalati integrációs maps ismertetése")
* [<span data-ttu-id="7ebd2-146">Vállalati integrációs összekötők</span><span class="sxs-lookup"><span data-stu-id="7ebd2-146">Enterprise Integration Connectors</span></span>](../connectors/apis-list.md "további információ a vállalati integrációs csomag összekötők")
* [<span data-ttu-id="7ebd2-147">Integráció fiók metaadat</span><span class="sxs-lookup"><span data-stu-id="7ebd2-147">Integration Account Metadata</span></span>](../logic-apps/logic-apps-enterprise-integration-metadata.md "integrációs fiók metaadat ismertetése")
* [<span data-ttu-id="7ebd2-148">B2B üzeneteket figyelő</span><span class="sxs-lookup"><span data-stu-id="7ebd2-148">Monitor B2B messages</span></span>](logic-apps-monitor-b2b-message.md "tudhat meg többet a B2B üzenetek figyelése")
* [<span data-ttu-id="7ebd2-149">Az OMS-portálon B2B üzenetek nyomon követése</span><span class="sxs-lookup"><span data-stu-id="7ebd2-149">Tracking B2B messages in OMS portal</span></span>](logic-apps-track-b2b-messages-omsportal.md "tudhat meg többet az OMS-portálon B2B üzenetek nyomon követése")

