---
title: "aaaConnect tooan helyszíni SAP rendszer az Azure Logic Apps |} Microsoft Docs"
description: "A logic app munkafolyamat hello a helyszíni adatok átjárón keresztül kapcsolódó tooan helyszíni SAP rendszer"
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 594ec5fed337398bf931d396684630ee9f907d2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-on-premises-sap-system-from-logic-apps-with-hello-sap-connector"></a><span data-ttu-id="254a7-103">Tooan helyszíni SAP rendszer csatlakoztatja a logic apps hello SAP-összekötőn keresztül</span><span class="sxs-lookup"><span data-stu-id="254a7-103">Connect tooan on-premises SAP system from logic apps with hello SAP connector</span></span> 

<span data-ttu-id="254a7-104">hello helyszíni adatátjáró lehetővé teszi toomanage adatokat, és biztonságosan férhetnek hozzá a helyszíni erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="254a7-104">hello on-premises data gateway enables you toomanage data, and securely access resources that are on-premises.</span></span> <span data-ttu-id="254a7-105">Ez a témakör bemutatja, hogyan csatlakoztathatja a logic apps tooan helyszíni SAP rendszert.</span><span class="sxs-lookup"><span data-stu-id="254a7-105">This topic shows how you can connect logic apps tooan on-premises SAP system.</span></span> <span data-ttu-id="254a7-106">Ebben a példában a Logic Apps alkalmazást az IDOC-kérelmek HTTP Protokollon keresztül, és vissza hello választ küld.</span><span class="sxs-lookup"><span data-stu-id="254a7-106">In this example, your logic app requests an IDOC over HTTP and sends hello response back.</span></span>    

> [!NOTE]
> <span data-ttu-id="254a7-107">Aktuális korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="254a7-107">Current limitations:</span></span> 
> - <span data-ttu-id="254a7-108">A Logic Apps alkalmazást időtúllépés történik, ha hello válasz szükséges lépéseket nem fejeződik be hello [kérelem időkorlátja](./logic-apps-limits-and-config.md).</span><span class="sxs-lookup"><span data-stu-id="254a7-108">Your logic app times out if all steps required for hello response don't finish within hello [request timeout limit](./logic-apps-limits-and-config.md).</span></span> <span data-ttu-id="254a7-109">Ebben a forgatókönyvben kérelmek előfordulhat, hogy blokkolnánk a hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="254a7-109">In this scenario, requests might get blocked.</span></span> 
> - <span data-ttu-id="254a7-110">hello fájlválasztó nem jeleníti meg az összes hello elérhető mezők.</span><span class="sxs-lookup"><span data-stu-id="254a7-110">hello file picker does not display all hello available fields.</span></span> <span data-ttu-id="254a7-111">Ebben a forgatókönyvben kézzel is hozzáadhatja a elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="254a7-111">In this scenario, you can manually add paths.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="254a7-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="254a7-112">Prerequisites</span></span>

- <span data-ttu-id="254a7-113">Telepítse és konfigurálja a hello legújabb [helyszíni adatátjáró](https://www.microsoft.com/download/details.aspx?id=53127) 1.15.6150.1 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="254a7-113">Install and configure hello latest [on-premises data gateway](https://www.microsoft.com/download/details.aspx?id=53127) version 1.15.6150.1 or newer.</span></span> <span data-ttu-id="254a7-114">[Hogyan tooconnect toohello helyszíni adatátjáró a logikai alkalmazás](http://aka.ms/logicapps-gateway) listák hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="254a7-114">[How tooconnect toohello on-premises data gateway in a logic app](http://aka.ms/logicapps-gateway) lists hello steps.</span></span> <span data-ttu-id="254a7-115">a folytatás előtt a helyi gépen telepítenie kell hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="254a7-115">hello gateway must be installed on an on-premises machine before you can proceed.</span></span>

- <span data-ttu-id="254a7-116">A letöltés és telepítés hello legújabb SAP ügyféloldali kódtára a hello azonos számítógépre hello adatátjáró telepítési.</span><span class="sxs-lookup"><span data-stu-id="254a7-116">Download and install hello latest SAP client library on hello same machine where you installed hello data gateway.</span></span> <span data-ttu-id="254a7-117">Használja a következő SAP verziók hello valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="254a7-117">Use any of hello following SAP versions:</span></span> 
    - <span data-ttu-id="254a7-118">SAP-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="254a7-118">SAP Server</span></span>
        - <span data-ttu-id="254a7-119">Egyetlen SAP-kiszolgálóhoz, hogy támogatási hello (Ice) 3.0-s .NET-összekötő</span><span class="sxs-lookup"><span data-stu-id="254a7-119">Any SAP Server that support hello .NET Connector (NCo) 3.0</span></span>
 
    - <span data-ttu-id="254a7-120">SAP-ügyfél</span><span class="sxs-lookup"><span data-stu-id="254a7-120">SAP Client</span></span>
        - <span data-ttu-id="254a7-121">SAP .NET Connector (Ice) 3.0</span><span class="sxs-lookup"><span data-stu-id="254a7-121">SAP .NET Connector (NCo) 3.0</span></span>

## <a name="add-triggers-and-actions-for-connecting-tooyour-sap-system"></a><span data-ttu-id="254a7-122">Eseményindítók és műveletek tooyour SAP rendszer kapcsolódás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="254a7-122">Add triggers and actions for connecting tooyour SAP system</span></span>

<span data-ttu-id="254a7-123">hello SAP összekötő műveleteket, de nem eseményindítók rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="254a7-123">hello SAP connector has actions, but not triggers.</span></span> <span data-ttu-id="254a7-124">Igen van toouse egy másik eseményindító hello munkafolyamat hello elején.</span><span class="sxs-lookup"><span data-stu-id="254a7-124">So, we have toouse another trigger at hello start of hello workflow.</span></span> 

1. <span data-ttu-id="254a7-125">Adja hozzá a hello kérelem/válasz eseményindító, majd válassza ki **új lépés**.</span><span class="sxs-lookup"><span data-stu-id="254a7-125">Add hello Request/Response trigger, and then select **New step**.</span></span>

2. <span data-ttu-id="254a7-126">Válassza ki **művelet hozzáadása**, majd válassza ki a hello SAP összekötő beírásával `SAP` hello keresési mező:</span><span class="sxs-lookup"><span data-stu-id="254a7-126">Select **Add an action**, and then select hello SAP connector by typing `SAP` in hello search field:</span></span>    

     ![SAP-alkalmazáskiszolgáló vagy SAP üzenet kiszolgáló kiválasztása](media/logic-apps-using-sap-connector/sap-action.png)

3. <span data-ttu-id="254a7-128">Válassza ki [ **SAP alkalmazáskiszolgáló** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) vagy [ **SAP üzenet Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm)a SAP-beállításai alapján.</span><span class="sxs-lookup"><span data-stu-id="254a7-128">Select [**SAP Application Server**](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) or [**SAP Message Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), based on your SAP setup.</span></span> <span data-ttu-id="254a7-129">Ha nincs meglévő kapcsolat, egy felszólító toocreate áll.</span><span class="sxs-lookup"><span data-stu-id="254a7-129">If you don't have an existing connection, you are prompted toocreate one.</span></span>

   1. <span data-ttu-id="254a7-130">Válassza ki **keresztül, a helyszíni adatátjáró**, és adja meg a SAP rendszer hello részleteit:</span><span class="sxs-lookup"><span data-stu-id="254a7-130">Select **Connect via on-premises data gateway**, and enter hello details for your SAP system:</span></span>   

       ![Adja hozzá a kapcsolati karakterlánc tooSAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. <span data-ttu-id="254a7-132">A **átjáró**jelöljön ki egy meglévő átjárót, vagy tooinstall egy új átjárót, válassza ki **átjáró telepítése**.</span><span class="sxs-lookup"><span data-stu-id="254a7-132">Under **Gateway**, select an existing gateway, or tooinstall a new gateway, select **Install Gateway**.</span></span>

        ![Telepítsen egy új átjárót](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. <span data-ttu-id="254a7-134">Minden hello részletek megadása után válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="254a7-134">After you enter all hello details, select **Create**.</span></span> 
   <span data-ttu-id="254a7-135">A Logic Apps konfigurálja, és teszteli hello kapcsolat, annak biztosítása, hogy hello kapcsolat megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="254a7-135">Logic Apps configures and tests hello connection, making sure that hello connection works properly.</span></span>

4. <span data-ttu-id="254a7-136">Adjon meg egy nevet az SAP-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="254a7-136">Enter a name for your SAP connection.</span></span>

5. <span data-ttu-id="254a7-137">hello különböző SAP beállítások is elérhető.</span><span class="sxs-lookup"><span data-stu-id="254a7-137">hello different SAP options are now available.</span></span> <span data-ttu-id="254a7-138">toofind az IDOC kategória, hello listából válassza ki.</span><span class="sxs-lookup"><span data-stu-id="254a7-138">toofind your IDOC category, select from hello list.</span></span> <span data-ttu-id="254a7-139">Vagy manuálisan adja meg a hello elérési utat, és válassza hello HTTP-válasz a hello **törzs** mező:</span><span class="sxs-lookup"><span data-stu-id="254a7-139">Or manually type in hello path, and select hello HTTP response in hello **body** field:</span></span>

     ![SAP művelet](media/logic-apps-using-sap-connector/picture3.png)

6. <span data-ttu-id="254a7-141">Hello művelet létrehozásához hozzáadása egy **HTTP-válasz**.</span><span class="sxs-lookup"><span data-stu-id="254a7-141">Add hello action for creating an **HTTP Response**.</span></span> <span data-ttu-id="254a7-142">az SAP-kimenetből hello hello válaszüzenetet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="254a7-142">hello response message should be from hello SAP output.</span></span>

7. <span data-ttu-id="254a7-143">Mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="254a7-143">Save your logic app.</span></span> <span data-ttu-id="254a7-144">Tesztelje úgy, hogy az IDOC keresztül hello HTTP-eseményindító URL-cím küldésekor.</span><span class="sxs-lookup"><span data-stu-id="254a7-144">Test it by sending an IDOC through hello HTTP trigger URL.</span></span> <span data-ttu-id="254a7-145">Hello IDOC zajlik, után várjon, amíg hello logikai alkalmazás hello válaszát:</span><span class="sxs-lookup"><span data-stu-id="254a7-145">After hello IDOC is sent, wait for hello response from hello logic app:</span></span>   

     > [!TIP]
     > <span data-ttu-id="254a7-146">Ellenőrizze, hogyan túl[logikai alkalmazások figyelése](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="254a7-146">Check out how too[monitor your Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span></span>

<span data-ttu-id="254a7-147">Most, hogy hello SAP összekötő tooyour logikai alkalmazás ad hozzá, Fedezze fel egyéb funkciók:</span><span class="sxs-lookup"><span data-stu-id="254a7-147">Now that hello SAP connector is added tooyour logic app, start exploring other functionalities:</span></span>

- <span data-ttu-id="254a7-148">BAPI</span><span class="sxs-lookup"><span data-stu-id="254a7-148">BAPI</span></span>
- <span data-ttu-id="254a7-149">RFC</span><span class="sxs-lookup"><span data-stu-id="254a7-149">RFC</span></span>

## <a name="get-help"></a><span data-ttu-id="254a7-150">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="254a7-150">Get help</span></span>

<span data-ttu-id="254a7-151">tooask kérdések kérdést, és ismerje meg, milyen egyéb Azure Logic Apps felhasználók végzi, látogasson el hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="254a7-151">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="254a7-152">toohelp Azure Logic Apps alkalmazások és összekötők javítása, szavazhatnak, vagy küldje el a következő hello ötleteket [Azure Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="254a7-152">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="254a7-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="254a7-153">Next steps</span></span>

- <span data-ttu-id="254a7-154">Megtudhatja, hogyan toovalidate, átalakítási és más hasonló BizTalk funkciók hello [vállalati integrációs csomag](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="254a7-154">Learn how toovalidate, transform, and other BizTalk-like functions in hello [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span> 
- <span data-ttu-id="254a7-155">[Csatlakozás tooon helyszíni adatokhoz](../logic-apps/logic-apps-gateway-connection.md) a logic Apps alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="254a7-155">[Connect tooon-premises data](../logic-apps/logic-apps-gateway-connection.md) from logic apps</span></span>
