---
title: "aaaEncode vagy egybesimított fájlok az Azure logic apps |} Microsoft Docs"
description: "Hogyan toouse hello fájl fájl kódoló és dekóder a logic Apps alkalmazásokat a vállalati integrációs csomag hello"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 82152dab-c7ad-43df-b721-596559703be8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 2c295586625fd84366ec7cbafdcebf0489ba234d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-enterprise-integration-with-flat-files"></a><span data-ttu-id="040f7-103">Vállalati integrációs egybesimított fájlok áttekintése</span><span class="sxs-lookup"><span data-stu-id="040f7-103">Overview of enterprise integration with flat files</span></span>

<span data-ttu-id="040f7-104">Érdemes lehet tooencode XML tartalom elküldés előtt tooa üzleti partner üzleti vállalatközi (B2B) esetén.</span><span class="sxs-lookup"><span data-stu-id="040f7-104">You may want tooencode XML content before you send it tooa business partner in a business-to-business (B2B) scenario.</span></span> <span data-ttu-id="040f7-105">A logikai alkalmazás hello strukturálatlan fájlkódolás összekötő toodo ez is használhatja.</span><span class="sxs-lookup"><span data-stu-id="040f7-105">In a logic app, you can use hello flat file encoding connector toodo this.</span></span> <span data-ttu-id="040f7-106">hello az Ön által létrehozott logikai alkalmazás lekérheti az XML tartalom móddal, többek között egy HTTP-kérelem eseményindítóval, egy másik alkalmazás, vagy akár egy hello számos különböző [összekötők](../connectors/apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="040f7-106">hello logic app that you create can get its XML content from a variety of sources, including from an HTTP request trigger, from another application, or even from one of hello many [connectors](../connectors/apis-list.md).</span></span> <span data-ttu-id="040f7-107">A logic apps kapcsolatos további információkért lásd: hello [logic apps dokumentációs](logic-apps-what-are-logic-apps.md "további információk a Logic apps").</span><span class="sxs-lookup"><span data-stu-id="040f7-107">For more information about logic apps, see hello [logic apps documentation](logic-apps-what-are-logic-apps.md "Learn more about Logic apps").</span></span>  

## <a name="create-hello-flat-file-encoding-connector"></a><span data-ttu-id="040f7-108">Hello egybesimított fájl kódolási összekötő létrehozása</span><span class="sxs-lookup"><span data-stu-id="040f7-108">Create hello flat file encoding connector</span></span>
<span data-ttu-id="040f7-109">Kövesse ezeket a lépéseket tooadd egy egyszerű fájlkódolás összekötő tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="040f7-109">Follow these steps tooadd a flat file encoding connector tooyour logic app.</span></span>

1. <span data-ttu-id="040f7-110">Logikai alkalmazás létrehozása és [tooyour integrációs fiók hivatkozás](logic-apps-enterprise-integration-accounts.md "ismerje meg, az integráció fiók tooa logikai alkalmazás toolink").</span><span class="sxs-lookup"><span data-stu-id="040f7-110">Create a logic app and [link it tooyour integration account](logic-apps-enterprise-integration-accounts.md "Learn toolink an integration account tooa Logic app").</span></span> <span data-ttu-id="040f7-111">Ezt a fiókot fogja használni tooencode hello XML-adatok hello sémát tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="040f7-111">This account contains hello schema you will use tooencode hello XML data.</span></span>  
2. <span data-ttu-id="040f7-112">Adja hozzá a **kérelem - amikor egy HTTP-kérelem érkezik** eseményindító tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="040f7-112">Add a **Request - When an HTTP request is received** trigger tooyour logic app.</span></span>  
   <span data-ttu-id="040f7-113">![Képernyőkép a eseményindító tooselect](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span><span class="sxs-lookup"><span data-stu-id="040f7-113">![Screenshot of trigger tooselect](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span></span>    
3. <span data-ttu-id="040f7-114">Adja hozzá a hello strukturálatlan fájlkódolás művelet, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="040f7-114">Add hello flat file encoding action, as follows:</span></span>
   
    <span data-ttu-id="040f7-115">a.</span><span class="sxs-lookup"><span data-stu-id="040f7-115">a.</span></span> <span data-ttu-id="040f7-116">Jelölje be hello **plus** bejelentkezési.</span><span class="sxs-lookup"><span data-stu-id="040f7-116">Select hello **plus** sign.</span></span>
   
    <span data-ttu-id="040f7-117">b.</span><span class="sxs-lookup"><span data-stu-id="040f7-117">b.</span></span> <span data-ttu-id="040f7-118">Jelölje be hello **művelet hozzáadása** (jelenik meg, miután kiválasztotta a hello plusz jel) hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="040f7-118">Select hello **Add an action** link (appears after you have selected hello plus sign).</span></span>
   
    <span data-ttu-id="040f7-119">c.</span><span class="sxs-lookup"><span data-stu-id="040f7-119">c.</span></span> <span data-ttu-id="040f7-120">Hello keresési mezőbe, írja be a *egyszerű* toofilter összes hello műveletek toohello egy megjeleníteni kívánt toouse.</span><span class="sxs-lookup"><span data-stu-id="040f7-120">In hello search box, enter *Flat* toofilter all hello actions toohello one that you want toouse.</span></span>
   
    <span data-ttu-id="040f7-121">d.</span><span class="sxs-lookup"><span data-stu-id="040f7-121">d.</span></span> <span data-ttu-id="040f7-122">Jelölje be hello **strukturálatlan fájlkódolás** elemet hello listában.</span><span class="sxs-lookup"><span data-stu-id="040f7-122">Select hello **Flat File Encoding** option from hello list.</span></span>   
   <span data-ttu-id="040f7-123">![Képernyőfelvétel a strukturálatlan fájlkódolás beállítás](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span><span class="sxs-lookup"><span data-stu-id="040f7-123">![Screenshot of Flat File Encoding option](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span></span>   
4. <span data-ttu-id="040f7-124">A hello **strukturálatlan fájlkódolás** párbeszédpanel megnyitásához, jelölje be hello **tartalom** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="040f7-124">On hello **Flat File Encoding** dialog box, select hello **Content** text box.</span></span>  
   <span data-ttu-id="040f7-125">![Képernyőfelvétel a tartalom szövegmező](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)</span><span class="sxs-lookup"><span data-stu-id="040f7-125">![Screenshot of Content text box](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)</span></span>  
5. <span data-ttu-id="040f7-126">Jelöljön ki hello törzs címke, a tartalom, amelyet az tooencode hello.</span><span class="sxs-lookup"><span data-stu-id="040f7-126">Select hello body tag as hello content that you want tooencode.</span></span> <span data-ttu-id="040f7-127">hello törzs címke feltölti hello tartalom mezőben.</span><span class="sxs-lookup"><span data-stu-id="040f7-127">hello body tag will populate hello content field.</span></span>     
   ![Képernyőkép a szervezet címke](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  
6. <span data-ttu-id="040f7-129">Jelölje be hello **sémanév** listán, és válassza ki a kívánt toouse tooencode hello hello séma bemeneti tartalom.</span><span class="sxs-lookup"><span data-stu-id="040f7-129">Select hello **Schema Name** list box, and choose hello schema you want toouse tooencode hello input content.</span></span>    
   <span data-ttu-id="040f7-130">![Képernyőfelvétel a séma neve listamező](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)</span><span class="sxs-lookup"><span data-stu-id="040f7-130">![Screenshot of Schema Name list box](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)</span></span>  
7. <span data-ttu-id="040f7-131">Mentse a munkáját.</span><span class="sxs-lookup"><span data-stu-id="040f7-131">Save your work.</span></span>   
   ![Képernyőfelvétel a mentés ikon](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

<span data-ttu-id="040f7-133">Ezen a ponton befejezte a fájlszintű kódolási connector beállítása.</span><span class="sxs-lookup"><span data-stu-id="040f7-133">At this point, you are finished setting up your flat file encoding connector.</span></span> <span data-ttu-id="040f7-134">Egy valós alkalmazás érdemes lehet egy sor üzleti alkalmazás, például a Salesforce toostore hello kódolású adatokat.</span><span class="sxs-lookup"><span data-stu-id="040f7-134">In a real world application, you may want toostore hello encoded data in a line-of-business application, such as Salesforce.</span></span> <span data-ttu-id="040f7-135">Vagy a kódolt adatok tooa kereskedelmi partner küldhet.</span><span class="sxs-lookup"><span data-stu-id="040f7-135">Or you can send that encoded data tooa trading partner.</span></span> <span data-ttu-id="040f7-136">Egy művelet toosend hello kimeneti hello kódolási művelet tooSalesforce vagy tooyour kereskedelmi partner, a hello segítségével a többi összekötőt, a megadott egyszerű hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="040f7-136">You can easily add an action toosend hello output of hello encoding action tooSalesforce, or tooyour trading partner, by using any one of hello other connectors provided.</span></span>

<span data-ttu-id="040f7-137">Az összekötő egy kérelem toohello HTTP-végpont, és a hello kérelem törzse hello is beleértve hello XML-tartalom most tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="040f7-137">You can now test your connector by making a request toohello HTTP endpoint, and including hello XML content in hello body of hello request.</span></span>  

## <a name="create-hello-flat-file-decoding-connector"></a><span data-ttu-id="040f7-138">Hello egybesimított fájl dekódolás összekötő létrehozása</span><span class="sxs-lookup"><span data-stu-id="040f7-138">Create hello flat file decoding connector</span></span>

> [!NOTE]
> <span data-ttu-id="040f7-139">toocomplete ezeket a lépéseket, a következő sémafájl már feltöltött az integráció fiókjába toohave kell.</span><span class="sxs-lookup"><span data-stu-id="040f7-139">toocomplete these steps, you need toohave a schema file already uploaded into you integration account.</span></span>

1. <span data-ttu-id="040f7-140">Adja hozzá a **kérelem - amikor egy HTTP-kérelem érkezik** eseményindító tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="040f7-140">Add a **Request - When an HTTP request is received** trigger tooyour logic app.</span></span>  
   <span data-ttu-id="040f7-141">![Képernyőkép a eseményindító tooselect](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span><span class="sxs-lookup"><span data-stu-id="040f7-141">![Screenshot of trigger tooselect](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span></span>    
2. <span data-ttu-id="040f7-142">Hello művelet, az alábbiak szerint dekódolás egybesimított fájl hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="040f7-142">Add hello flat file decoding action, as follows:</span></span>
   
    <span data-ttu-id="040f7-143">a.</span><span class="sxs-lookup"><span data-stu-id="040f7-143">a.</span></span> <span data-ttu-id="040f7-144">Jelölje be hello **plus** bejelentkezési.</span><span class="sxs-lookup"><span data-stu-id="040f7-144">Select hello **plus** sign.</span></span>
   
    <span data-ttu-id="040f7-145">b.</span><span class="sxs-lookup"><span data-stu-id="040f7-145">b.</span></span> <span data-ttu-id="040f7-146">Jelölje be hello **művelet hozzáadása** (jelenik meg, miután kiválasztotta a hello plusz jel) hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="040f7-146">Select hello **Add an action** link (appears after you have selected hello plus sign).</span></span>
   
    <span data-ttu-id="040f7-147">c.</span><span class="sxs-lookup"><span data-stu-id="040f7-147">c.</span></span> <span data-ttu-id="040f7-148">Hello keresési mezőbe, írja be a *egyszerű* toofilter összes hello műveletek toohello egy megjeleníteni kívánt toouse.</span><span class="sxs-lookup"><span data-stu-id="040f7-148">In hello search box, enter *Flat* toofilter all hello actions toohello one that you want toouse.</span></span>
   
    <span data-ttu-id="040f7-149">d.</span><span class="sxs-lookup"><span data-stu-id="040f7-149">d.</span></span> <span data-ttu-id="040f7-150">Jelölje be hello **Egybesimított fájl dekódolás** elemet hello listában.</span><span class="sxs-lookup"><span data-stu-id="040f7-150">Select hello **Flat File Decoding** option from hello list.</span></span>   
   <span data-ttu-id="040f7-151">![Képernyőfelvétel a Egybesimított fájl dekódolás beállítás](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span><span class="sxs-lookup"><span data-stu-id="040f7-151">![Screenshot of Flat File Decoding option](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span></span>   
3. <span data-ttu-id="040f7-152">Jelölje be hello **tartalom** vezérlő.</span><span class="sxs-lookup"><span data-stu-id="040f7-152">Select hello **Content** control.</span></span> <span data-ttu-id="040f7-153">Ezzel létrehozza a korábbi lépések, amely akkor használható hello tartalom toodecode hello tartalmat listáját.</span><span class="sxs-lookup"><span data-stu-id="040f7-153">This produces a list of hello content from earlier steps that you can use as hello content toodecode.</span></span> <span data-ttu-id="040f7-154">Figyelje meg, hogy hello *törzs* hello bejövő HTTP-kérelem nem érhető el tartalom toodecode hello használt toobe.</span><span class="sxs-lookup"><span data-stu-id="040f7-154">Notice that hello *Body* from hello incoming HTTP request is available toobe used as hello content toodecode.</span></span> <span data-ttu-id="040f7-155">Közvetlenül a hello is megadhat hello tartalom toodecode **tartalom** vezérlő.</span><span class="sxs-lookup"><span data-stu-id="040f7-155">You can also enter hello content toodecode directly into hello **Content** control.</span></span>     
4. <span data-ttu-id="040f7-156">Jelölje be hello *törzs* címke.</span><span class="sxs-lookup"><span data-stu-id="040f7-156">Select hello *Body* tag.</span></span> <span data-ttu-id="040f7-157">Értesítés hello törzs címke már hello **tartalom** vezérlő.</span><span class="sxs-lookup"><span data-stu-id="040f7-157">Notice hello body tag is now in hello **Content** control.</span></span>
5. <span data-ttu-id="040f7-158">Válassza ki a megjeleníteni kívánt toouse toodecode hello tartalom hello séma hello nevét.</span><span class="sxs-lookup"><span data-stu-id="040f7-158">Select hello name of hello schema that you want toouse toodecode hello content.</span></span> <span data-ttu-id="040f7-159">hello alábbi képernyőfelvételen látható, amely *OrderFile* hello kijelölt séma neve.</span><span class="sxs-lookup"><span data-stu-id="040f7-159">hello following screenshot shows that *OrderFile* is hello selected schema name.</span></span> <span data-ttu-id="040f7-160">A séma neve kellett feltöltve hello integrációs figyelembe korábban.</span><span class="sxs-lookup"><span data-stu-id="040f7-160">This schema name had been uploaded into hello integration account previously.</span></span>
   
   ![Képernyőfelvétel a Egybesimított fájl dekódolás párbeszédpanel](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png)    
6. <span data-ttu-id="040f7-162">Mentse a munkáját.</span><span class="sxs-lookup"><span data-stu-id="040f7-162">Save your work.</span></span>  
   ![Képernyőfelvétel a mentés ikon](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

<span data-ttu-id="040f7-164">Ezen a ponton befejezte a egybesimított fájl dekódolás összekötő beállítása.</span><span class="sxs-lookup"><span data-stu-id="040f7-164">At this point, you are finished setting up your flat file decoding connector.</span></span> <span data-ttu-id="040f7-165">Egy valós alkalmazás érdemes lehet toostore hello dekódolni, adatok, például a Salesforce-üzletági alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="040f7-165">In a real world application, you may want toostore hello decoded data in a line-of-business application such as Salesforce.</span></span> <span data-ttu-id="040f7-166">Egyszerűen hozzáadhatja művelet tooSalesforce dekódolás hello művelet toosend hello kimenete.</span><span class="sxs-lookup"><span data-stu-id="040f7-166">You can easily add an action toosend hello output of hello decoding action tooSalesforce.</span></span>

<span data-ttu-id="040f7-167">Az összekötő azáltal, hogy egy kérelem toohello HTTP-végpont most tesztelheti, és hello XML-tartalom toodecode hello kérelem törzse hello a kívánt.</span><span class="sxs-lookup"><span data-stu-id="040f7-167">You can now test your connector by making a request toohello HTTP endpoint and including hello XML content you want toodecode in hello body of hello request.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="040f7-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="040f7-168">Next steps</span></span>
* <span data-ttu-id="040f7-169">[További tudnivalók a vállalati integrációs csomag hello](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag").</span><span class="sxs-lookup"><span data-stu-id="040f7-169">[Learn more about hello Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about Enterprise Integration Pack").</span></span>  
