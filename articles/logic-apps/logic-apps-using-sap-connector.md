---
title: "Az Azure Logic Apps egy helyszíni SAP rendszerhez való csatlakozás |} Microsoft Docs"
description: "A logic app munkafolyamat a helyszíni adatok átjárón keresztül egy helyszíni SAP rendszerhez való csatlakozás"
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
ms.openlocfilehash: 3fea93f558d5a4ef62550fd1f6486903cb812930
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-to-an-on-premises-sap-system-from-logic-apps-with-the-sap-connector"></a><span data-ttu-id="60c05-103">A logic Apps alkalmazásokból SAP összekötő egy helyszíni SAP rendszerhez való csatlakozás</span><span class="sxs-lookup"><span data-stu-id="60c05-103">Connect to an on-premises SAP system from logic apps with the SAP connector</span></span> 

<span data-ttu-id="60c05-104">Az a helyszíni átjáró teszi lehetővé adatok kezelésére, és a biztonságos hozzáférés a helyszíni erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="60c05-104">The on-premises data gateway enables you to manage data, and securely access resources that are on-premises.</span></span> <span data-ttu-id="60c05-105">Ez a témakör bemutatja, hogyan csatlakoztathatja a logic apps egy helyszíni SAP rendszerhez.</span><span class="sxs-lookup"><span data-stu-id="60c05-105">This topic shows how you can connect logic apps to an on-premises SAP system.</span></span> <span data-ttu-id="60c05-106">Ebben a példában a Logic Apps alkalmazást az IDOC-kérelmek HTTP Protokollon keresztül, és visszaküldi a választ vissza.</span><span class="sxs-lookup"><span data-stu-id="60c05-106">In this example, your logic app requests an IDOC over HTTP and sends the response back.</span></span>    

> [!NOTE]
> <span data-ttu-id="60c05-107">Aktuális korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="60c05-107">Current limitations:</span></span> 
> - <span data-ttu-id="60c05-108">A Logic Apps alkalmazást időtúllépés történik, ha a válasz lépéseit nem fejeződik be a [kérelem időkorlátja](./logic-apps-limits-and-config.md).</span><span class="sxs-lookup"><span data-stu-id="60c05-108">Your logic app times out if all steps required for the response don't finish within the [request timeout limit](./logic-apps-limits-and-config.md).</span></span> <span data-ttu-id="60c05-109">Ebben a forgatókönyvben kérelmek előfordulhat, hogy blokkolnánk a hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="60c05-109">In this scenario, requests might get blocked.</span></span> 
> - <span data-ttu-id="60c05-110">A fájlválasztó nem jelenik meg a rendelkezésre álló mezők.</span><span class="sxs-lookup"><span data-stu-id="60c05-110">The file picker does not display all the available fields.</span></span> <span data-ttu-id="60c05-111">Ebben a forgatókönyvben kézzel is hozzáadhatja a elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="60c05-111">In this scenario, you can manually add paths.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60c05-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="60c05-112">Prerequisites</span></span>

- <span data-ttu-id="60c05-113">Telepítse és konfigurálja a legújabb [helyszíni adatátjáró](https://www.microsoft.com/download/details.aspx?id=53127) 1.15.6150.1 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="60c05-113">Install and configure the latest [on-premises data gateway](https://www.microsoft.com/download/details.aspx?id=53127) version 1.15.6150.1 or newer.</span></span> <span data-ttu-id="60c05-114">[A helyszíni átjáró a logikai alkalmazás összekapcsolása](http://aka.ms/logicapps-gateway) lépéseit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="60c05-114">[How to connect to the on-premises data gateway in a logic app](http://aka.ms/logicapps-gateway) lists the steps.</span></span> <span data-ttu-id="60c05-115">A folytatás előtt a helyi gépen telepítenie kell az átjárót.</span><span class="sxs-lookup"><span data-stu-id="60c05-115">The gateway must be installed on an on-premises machine before you can proceed.</span></span>

- <span data-ttu-id="60c05-116">Töltse le és telepítse a legújabb SAP ügyféloldali kódtár ugyanazon a számítógépen, amelyre telepítette az átjáró.</span><span class="sxs-lookup"><span data-stu-id="60c05-116">Download and install the latest SAP client library on the same machine where you installed the data gateway.</span></span> <span data-ttu-id="60c05-117">Használja az alábbi SAP-verziók valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="60c05-117">Use any of the following SAP versions:</span></span> 
    - <span data-ttu-id="60c05-118">SAP-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="60c05-118">SAP Server</span></span>
        - <span data-ttu-id="60c05-119">Egyetlen SAP-kiszolgálóhoz sem, amely támogatja a .NET-összekötő (Ice) 3.0</span><span class="sxs-lookup"><span data-stu-id="60c05-119">Any SAP Server that support the .NET Connector (NCo) 3.0</span></span>
 
    - <span data-ttu-id="60c05-120">SAP-ügyfél</span><span class="sxs-lookup"><span data-stu-id="60c05-120">SAP Client</span></span>
        - <span data-ttu-id="60c05-121">SAP .NET Connector (Ice) 3.0</span><span class="sxs-lookup"><span data-stu-id="60c05-121">SAP .NET Connector (NCo) 3.0</span></span>

## <a name="add-triggers-and-actions-for-connecting-to-your-sap-system"></a><span data-ttu-id="60c05-122">Eseményindítók és műveletek a SAP levelezőrendszerhez való csatlakozás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="60c05-122">Add triggers and actions for connecting to your SAP system</span></span>

<span data-ttu-id="60c05-123">Az SAP-összekötő műveleteket, de nem eseményindítók rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="60c05-123">The SAP connector has actions, but not triggers.</span></span> <span data-ttu-id="60c05-124">Igen azt kell használni egy másik eseményindító a munkafolyamat elején.</span><span class="sxs-lookup"><span data-stu-id="60c05-124">So, we have to use another trigger at the start of the workflow.</span></span> 

1. <span data-ttu-id="60c05-125">Vegye fel a kérelem/válasz eseményindító, majd válassza ki **új lépés**.</span><span class="sxs-lookup"><span data-stu-id="60c05-125">Add the Request/Response trigger, and then select **New step**.</span></span>

2. <span data-ttu-id="60c05-126">Válassza ki **művelet hozzáadása**, majd válassza ki az SAP-összekötő beírásával `SAP` a keresési mezőbe:</span><span class="sxs-lookup"><span data-stu-id="60c05-126">Select **Add an action**, and then select the SAP connector by typing `SAP` in the search field:</span></span>    

     ![SAP-alkalmazáskiszolgáló vagy SAP üzenet kiszolgáló kiválasztása](media/logic-apps-using-sap-connector/sap-action.png)

3. <span data-ttu-id="60c05-128">Válassza ki [ **SAP alkalmazáskiszolgáló** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) vagy [ **SAP üzenet Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm)a SAP-beállításai alapján.</span><span class="sxs-lookup"><span data-stu-id="60c05-128">Select [**SAP Application Server**](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) or [**SAP Message Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), based on your SAP setup.</span></span> <span data-ttu-id="60c05-129">Ha nincs meglévő kapcsolat, a rendszer kéri a hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="60c05-129">If you don't have an existing connection, you are prompted to create one.</span></span>

   1. <span data-ttu-id="60c05-130">Válassza ki **keresztül, a helyszíni adatátjáró**, és adja meg az SAP rendszert:</span><span class="sxs-lookup"><span data-stu-id="60c05-130">Select **Connect via on-premises data gateway**, and enter the details for your SAP system:</span></span>   

       ![Kapcsolati karakterlánc hozzáadása az SAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. <span data-ttu-id="60c05-132">A **átjáró**, egy meglévő átjárót, vagy telepít egy új átjárót, válasszon **átjáró telepítése**.</span><span class="sxs-lookup"><span data-stu-id="60c05-132">Under **Gateway**, select an existing gateway, or to install a new gateway, select **Install Gateway**.</span></span>

        ![Telepítsen egy új átjárót](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. <span data-ttu-id="60c05-134">A részletek megadása után válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="60c05-134">After you enter all the details, select **Create**.</span></span> 
   <span data-ttu-id="60c05-135">A Logic Apps konfigurálja, és ellenőrzi a kapcsolatot, és annak biztosítása, hogy a kapcsolat megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="60c05-135">Logic Apps configures and tests the connection, making sure that the connection works properly.</span></span>

4. <span data-ttu-id="60c05-136">Adjon meg egy nevet az SAP-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="60c05-136">Enter a name for your SAP connection.</span></span>

5. <span data-ttu-id="60c05-137">Az SAP beállításokról is elérhető.</span><span class="sxs-lookup"><span data-stu-id="60c05-137">The different SAP options are now available.</span></span> <span data-ttu-id="60c05-138">Keresse meg az IDOC szóló kategóriáját, válassza ki a listából.</span><span class="sxs-lookup"><span data-stu-id="60c05-138">To find your IDOC category, select from the list.</span></span> <span data-ttu-id="60c05-139">Vagy kézzel írja be az elérési út, és válassza ki a HTTP-válasz a **törzs** mező:</span><span class="sxs-lookup"><span data-stu-id="60c05-139">Or manually type in the path, and select the HTTP response in the **body** field:</span></span>

     ![SAP művelet](media/logic-apps-using-sap-connector/picture3.png)

6. <span data-ttu-id="60c05-141">Adja hozzá a művelet létrehozásához egy **HTTP-válasz**.</span><span class="sxs-lookup"><span data-stu-id="60c05-141">Add the action for creating an **HTTP Response**.</span></span> <span data-ttu-id="60c05-142">A válaszüzenet kell lennie az SAP-kimenetből.</span><span class="sxs-lookup"><span data-stu-id="60c05-142">The response message should be from the SAP output.</span></span>

7. <span data-ttu-id="60c05-143">Mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="60c05-143">Save your logic app.</span></span> <span data-ttu-id="60c05-144">Az idoc-hoz, a HTTP-eseményindító URL keresztül küldött tesztelik azt.</span><span class="sxs-lookup"><span data-stu-id="60c05-144">Test it by sending an IDOC through the HTTP trigger URL.</span></span> <span data-ttu-id="60c05-145">Az IDOC elküldött, várja meg a logikai alkalmazás válasza:</span><span class="sxs-lookup"><span data-stu-id="60c05-145">After the IDOC is sent, wait for the response from the logic app:</span></span>   

     > [!TIP]
     > <span data-ttu-id="60c05-146">Ellenőrizze, hogy miként [logikai alkalmazások figyelése](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="60c05-146">Check out how to [monitor your Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span></span>

<span data-ttu-id="60c05-147">Most, hogy az SAP-összekötőt a Logic Apps alkalmazást ad hozzá, Fedezze fel egyéb funkciók:</span><span class="sxs-lookup"><span data-stu-id="60c05-147">Now that the SAP connector is added to your logic app, start exploring other functionalities:</span></span>

- <span data-ttu-id="60c05-148">BAPI</span><span class="sxs-lookup"><span data-stu-id="60c05-148">BAPI</span></span>
- <span data-ttu-id="60c05-149">RFC</span><span class="sxs-lookup"><span data-stu-id="60c05-149">RFC</span></span>

## <a name="get-help"></a><span data-ttu-id="60c05-150">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="60c05-150">Get help</span></span>

<span data-ttu-id="60c05-151">Látogasson el az [Azure Logic Apps fórumára](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps), ahol kérdéseket tehet fel és válaszolhat meg, valamint megtudhatja, mivel foglalkoznak az Azure Logic Apps más felhasználói.</span><span class="sxs-lookup"><span data-stu-id="60c05-151">To ask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="60c05-152">Ha szeretné segíteni az Azure Logic Apps és összekötők fejlesztését, szavazzon az ötletekre az [Azure Logic Apps felhasználói visszajelzések oldalán](http://aka.ms/logicapps-wish), vagy küldje be saját javaslatait.</span><span class="sxs-lookup"><span data-stu-id="60c05-152">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="60c05-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60c05-153">Next steps</span></span>

- <span data-ttu-id="60c05-154">Ellenőrizze, átalakítására, valamint egyéb hasonló BizTalk funkciót az a [vállalati integrációs csomag](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="60c05-154">Learn how to validate, transform, and other BizTalk-like functions in the [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span> 
- <span data-ttu-id="60c05-155">[A helyszíni adatokhoz](../logic-apps/logic-apps-gateway-connection.md) a logic Apps alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="60c05-155">[Connect to on-premises data](../logic-apps/logic-apps-gateway-connection.md) from logic apps</span></span>
