---
title: "aaaConvert XML-adatok átalakító - Azure Logic Apps |} Microsoft Docs"
description: "Átalakítások vagy mapps tooconvert XML-adatok között szereplő hello vállalati integrációs SDK használatával a logic apps-formátumok létrehozása"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: b56ec1072c5058d3aefc7f88ac9b2748ebe56456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-integration-with-xml-transforms"></a><span data-ttu-id="51284-103">XML-átalakítók vállalati integrációja</span><span class="sxs-lookup"><span data-stu-id="51284-103">Enterprise integration with XML transforms</span></span>
## <a name="overview"></a><span data-ttu-id="51284-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="51284-104">Overview</span></span>
<span data-ttu-id="51284-105">hello vállalati integrációs átalakító összekötő formátum tooanother formátumba konvertálja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="51284-105">hello Enterprise integration Transform connector converts data from one format tooanother format.</span></span> <span data-ttu-id="51284-106">Például előfordulhat, hogy egy bejövő üzenet, amely tartalmazza a hello aktuális hello YearMonthDay formátumú dátumot.</span><span class="sxs-lookup"><span data-stu-id="51284-106">For example, you may have an incoming message that contains hello current date in hello YearMonthDay format.</span></span> <span data-ttu-id="51284-107">Használhat egy átalakítási tooreformat hello dátum toobe hello MonthDayYear formátumban.</span><span class="sxs-lookup"><span data-stu-id="51284-107">You can use a transform tooreformat hello date toobe in hello MonthDayYear format.</span></span>

## <a name="what-does-a-transform-do"></a><span data-ttu-id="51284-108">Mire átalakító?</span><span class="sxs-lookup"><span data-stu-id="51284-108">What does a transform do?</span></span>
<span data-ttu-id="51284-109">Átalakító, amely más néven a térkép, a forrás XML-séma (hello bemeneti) és a célként megadott XML-séma (hello kimeneti) áll.</span><span class="sxs-lookup"><span data-stu-id="51284-109">A Transform, which is also known as a map, consists of a Source XML schema (hello input) and a Target XML schema (hello output).</span></span> <span data-ttu-id="51284-110">Különféle beépített funkciók toohelp módosítására, vagy szabályozó hello, használhatja a karakterlánc-feldolgozás, feltételes hozzárendelés, aritmetikai kifejezésekben, dátum idő is beleértve, és még ismétlési szerkezeteket.</span><span class="sxs-lookup"><span data-stu-id="51284-110">You can use different built-in functions toohelp manipulate or control hello data, including string manipulations, conditional assignments, arithmetic expressions, date time formatters, and even looping constructs.</span></span>

## <a name="how-toocreate-a-transform"></a><span data-ttu-id="51284-111">Hogyan toocreate átalakító?</span><span class="sxs-lookup"><span data-stu-id="51284-111">How toocreate a transform?</span></span>
<span data-ttu-id="51284-112">Átalakítás/térképre hello Visual Studio használatával hozhat létre [vállalati integrációs SDK](https://aka.ms/vsmapsandschemas).</span><span class="sxs-lookup"><span data-stu-id="51284-112">You can create a transform/map by using hello Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas).</span></span> <span data-ttu-id="51284-113">Amikor végzett létrehozása és tesztelése hello átalakító, az integráció figyelembe feltöltött hello átalakító.</span><span class="sxs-lookup"><span data-stu-id="51284-113">When you are finished creating and testing hello transform, you upload hello transform into your integration account.</span></span> 

## <a name="how-toouse-a-transform"></a><span data-ttu-id="51284-114">Hogyan toouse átalakítás</span><span class="sxs-lookup"><span data-stu-id="51284-114">How toouse a transform</span></span>
<span data-ttu-id="51284-115">Az integráció figyelembe hello átalakító/térkép feltöltésekor használata toocreate logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="51284-115">After you upload hello transform/map into your integration account, you can use it toocreate a Logic app.</span></span> <span data-ttu-id="51284-116">hello logikai alkalmazás a átalakítások fut, amikor hello logikai alkalmazás elindul (vagy át legyenek-e toobe igénylő beviteli tartalom).</span><span class="sxs-lookup"><span data-stu-id="51284-116">hello Logic app runs your transformations whenever hello Logic app is triggered (and there is input content that needs toobe transformed).</span></span>

<span data-ttu-id="51284-117">**Az alábbiakban hello lépéseket toouse átalakító**:</span><span class="sxs-lookup"><span data-stu-id="51284-117">**Here are hello steps toouse a transform**:</span></span>

### <a name="prerequisites"></a><span data-ttu-id="51284-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="51284-118">Prerequisites</span></span>

* <span data-ttu-id="51284-119">Integráció-fiók létrehozása és egy térképen tooit hozzáadása</span><span class="sxs-lookup"><span data-stu-id="51284-119">Create an integration account and add a map tooit</span></span>  

<span data-ttu-id="51284-120">Most, hogy a hello Előfeltételek korábban hozott ítélt, a rendszer idő toocreate a Logic Apps alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="51284-120">Now that you've taken care of hello prerequisites, it's time toocreate your Logic app:</span></span>  

1. <span data-ttu-id="51284-121">Logikai alkalmazás létrehozása és [tooyour integrációs fiók hivatkozás](../logic-apps/logic-apps-enterprise-integration-accounts.md "ismerje meg, az integráció fiók tooa logikai alkalmazás toolink") hello térkép tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="51284-121">Create a Logic app and [link it tooyour integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md "Learn toolink an integration account tooa Logic app") that contains hello map.</span></span>
2. <span data-ttu-id="51284-122">Adja hozzá a **kérelem** eseményindító tooyour logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="51284-122">Add a **Request** trigger tooyour Logic app</span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. <span data-ttu-id="51284-123">Hozzáadás hello **átalakítása XML** első kiválasztásával művelet **művelet hozzáadása** </span><span class="sxs-lookup"><span data-stu-id="51284-123">Add hello **Transform XML** action by first selecting **Add an action** </span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. <span data-ttu-id="51284-124">Adja meg a hello word *átalakítási* hello Keresés mezőbe toofilter összes műveletek toohello egy megjeleníteni kívánt toouse hello</span><span class="sxs-lookup"><span data-stu-id="51284-124">Enter hello word *transform* in hello search box toofilter all hello actions toohello one that you want toouse</span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. <span data-ttu-id="51284-125">Jelölje be hello **átalakítása XML** művelet</span><span class="sxs-lookup"><span data-stu-id="51284-125">Select hello **Transform XML** action</span></span>   
6. <span data-ttu-id="51284-126">Adja hozzá az hello XML **tartalom** , amely alakít át.</span><span class="sxs-lookup"><span data-stu-id="51284-126">Add hello XML **CONTENT** that you transform.</span></span> <span data-ttu-id="51284-127">Is használhatja, mint hello hello HTTP-kérelem érkezik XML-adatokat **tartalom**.</span><span class="sxs-lookup"><span data-stu-id="51284-127">You can use any XML data you receive in hello HTTP request as hello **CONTENT**.</span></span> <span data-ttu-id="51284-128">Ebben a példában válassza ki a hello hello logikai alkalmazás kiváltó hello HTTP-kérelem törzsét.</span><span class="sxs-lookup"><span data-stu-id="51284-128">In this example, select hello body of hello HTTP request that triggered hello Logic app.</span></span>
7. <span data-ttu-id="51284-129">Jelölje be hello neve hello **térkép** , amelyet az toouse tooperform hello átalakítása.</span><span class="sxs-lookup"><span data-stu-id="51284-129">Select hello name of hello **MAP** that you want toouse tooperform hello transformation.</span></span> <span data-ttu-id="51284-130">hello térkép szerepelniük kell a integrációs fiókját.</span><span class="sxs-lookup"><span data-stu-id="51284-130">hello map must already be in your integration account.</span></span> <span data-ttu-id="51284-131">A korábbi lépésben a Logic app hozzáférési tooyour integrációs fiók, amely tartalmazza a térképre már megadott.</span><span class="sxs-lookup"><span data-stu-id="51284-131">In an earlier step, you already gave your Logic app access tooyour integration account that contains your map.</span></span>      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. <span data-ttu-id="51284-132">Mentse a munkáját</span><span class="sxs-lookup"><span data-stu-id="51284-132">Save your work</span></span>  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

<span data-ttu-id="51284-133">Ezen a ponton befejezte a térképre beállítása.</span><span class="sxs-lookup"><span data-stu-id="51284-133">At this point, you are finished setting up your map.</span></span> <span data-ttu-id="51284-134">Egy valós alkalmazás érdemes lehet egy LOB-alkalmazás, például SalesForce toostore hello át legyenek-e adatokat.</span><span class="sxs-lookup"><span data-stu-id="51284-134">In a real world application, you may want toostore hello transformed data in an LOB application such as SalesForce.</span></span> <span data-ttu-id="51284-135">Hello művelet toosend hello kimeneteként könnyen tooSalesforce alakíthatja át.</span><span class="sxs-lookup"><span data-stu-id="51284-135">You can easily as an action toosend hello output of hello transform tooSalesforce.</span></span> 

<span data-ttu-id="51284-136">Az átalakítási azáltal, hogy egy kérelem toohello HTTP-végpont most tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="51284-136">You can now test your transform by making a request toohello HTTP endpoint.</span></span>  

## <a name="features-and-use-cases"></a><span data-ttu-id="51284-137">Szolgáltatások és a használati esetek</span><span class="sxs-lookup"><span data-stu-id="51284-137">Features and use cases</span></span>
* <span data-ttu-id="51284-138">lehet, hogy a térképen létrehozott hello átalakítása egyszerű, például egy dokumentum tooanother másolása nevét és címét.</span><span class="sxs-lookup"><span data-stu-id="51284-138">hello transformation created in a map can be simple, such as copying a name and address from one document tooanother.</span></span> <span data-ttu-id="51284-139">Vagy összetettebb átalakítások hello out-of-az-box térkép műveletekkel is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="51284-139">Or, you can create more complex transformations using hello out-of-the-box map operations.</span></span>  
* <span data-ttu-id="51284-140">Több leképezés műveletek vagy funkciók azonnal elérhetők, beleértve a karakterláncok, dátum időpont függvényei, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="51284-140">Multiple map operations or functions are readily available, including strings, date time functions, and so on.</span></span>  
* <span data-ttu-id="51284-141">Hello sémák közötti közvetlen adatmásolás teheti meg.</span><span class="sxs-lookup"><span data-stu-id="51284-141">You can do a direct data copy between hello schemas.</span></span> <span data-ttu-id="51284-142">Hello leképező hello SDK szerepel ez az más dolga, mint egy vonal hello elemek hello forrás sémában kapcsoló, mint a hello cél sémában.</span><span class="sxs-lookup"><span data-stu-id="51284-142">In hello Mapper included in hello SDK, this is as simple as drawing a line that connects hello elements in hello source schema with their counterparts in hello destination schema.</span></span>  
* <span data-ttu-id="51284-143">Olyan térképet létrehozásakor megtekintheti hello leképezés, megjelenítheti az összes hello kapcsolat és hivatkozásokat hoz létre grafikus ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="51284-143">When creating a map, you view a graphical representation of hello map, which shows all hello relationships and links you create.</span></span>
* <span data-ttu-id="51284-144">Hello teszt leképezés funkció tooadd XML mintaüzenet használja.</span><span class="sxs-lookup"><span data-stu-id="51284-144">Use hello Test Map feature tooadd a sample XML message.</span></span> <span data-ttu-id="51284-145">Egy egyszerű kattintással hello térkép hozott létre, és tekintse meg a létrehozott hello kimeneti tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="51284-145">With a simple click, you can test hello map you created, and see hello generated output.</span></span>  
* <span data-ttu-id="51284-146">Töltse fel a meglévő leképezéseket</span><span class="sxs-lookup"><span data-stu-id="51284-146">Upload existing maps</span></span>  
* <span data-ttu-id="51284-147">Hello XML-formátuma támogatását is magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="51284-147">Includes support for hello XML format.</span></span>

## <a name="learn-more"></a><span data-ttu-id="51284-148">Részletek</span><span class="sxs-lookup"><span data-stu-id="51284-148">Learn more</span></span>
* [<span data-ttu-id="51284-149">További tudnivalók a vállalati integrációs csomag hello</span><span class="sxs-lookup"><span data-stu-id="51284-149">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")  
* [<span data-ttu-id="51284-150">További információ a maps</span><span class="sxs-lookup"><span data-stu-id="51284-150">Learn more about maps</span></span>](../logic-apps/logic-apps-enterprise-integration-maps.md "vállalati integrációs maps ismertetése")  

