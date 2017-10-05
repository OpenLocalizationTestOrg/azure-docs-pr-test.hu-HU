---
title: "A helyszíni fájlrendszer csatlakozni az Azure Logic Apps |} Microsoft Docs"
description: "A logic app munkafolyamat a helyszíni adatátjáró és fájlrendszer összekötő a helyszíni fájl rendszerekhez történő csatlakozáshoz"
keywords: "fájlrendszer"
services: logic-apps
author: derek1ee
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/27/2017
ms.author: LADocs; deli
ms.openlocfilehash: f33e7c58103c57e17e4e273caba1ab9b83f0cd2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-file-systems-from-logic-apps-with-the-file-system-connector"></a><span data-ttu-id="ccbd2-104">Csatlakozás helyszíni fájlrendszer a logic Apps alkalmazásokból, a fájlrendszer-összekötőn keresztül</span><span class="sxs-lookup"><span data-stu-id="ccbd2-104">Connect to on-premises file systems from logic apps with the File System connector</span></span>

<span data-ttu-id="ccbd2-105">Hibrid felhő kapcsolat központi helyet foglalnak el a logic apps, így az adatok kezelése és biztonságos hozzáférés a helyszíni erőforrásokhoz, a logic apps segítségével az helyszíni átjáró.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-105">Hybrid cloud connectivity is central to logic apps, so to manage data and securely access on-premises resources, your logic apps can use the on-premises data gateway.</span></span> <span data-ttu-id="ccbd2-106">Ebben a cikkben megmutatjuk, hogyan csatlakozhat egy helyszíni fájlrendszer egy egyszerű forgatókönyv: másolja át a fájlt, amely fájlmegosztásba a dropbox alkalmazásba feltöltött, majd küldje el e-mailt.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-106">In this article, we show how to connect to an on-premises file system with a basic scenario: copy a file that's uploaded to Dropbox to a file share, then send an email.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ccbd2-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ccbd2-107">Prerequisites</span></span>

- <span data-ttu-id="ccbd2-108">Telepítse és konfigurálja a legújabb [helyszíni adatátjáró](https://www.microsoft.com/download/details.aspx?id=53127).</span><span class="sxs-lookup"><span data-stu-id="ccbd2-108">Install and configure the latest [on-premises data gateway](https://www.microsoft.com/download/details.aspx?id=53127).</span></span>
- <span data-ttu-id="ccbd2-109">Telepítse a legújabb a helyszíni adatok gateway, 1.15.6150.1 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-109">Install the latest on-premises data gateway, version 1.15.6150.1 or above.</span></span> <span data-ttu-id="ccbd2-110">[A helyszíni adatok átjárón](http://aka.ms/logicapps-gateway) lépéseit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-110">[Connect to the on-premises data gateway](http://aka.ms/logicapps-gateway) lists the steps.</span></span> <span data-ttu-id="ccbd2-111">A további lépéseket a folytatás előtt a helyi gépen telepítenie kell az átjárót.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-111">The gateway must be installed on an on-premises machine before you can continue with the rest of the steps.</span></span>

## <a name="add-trigger-and-actions-for-connecting-to-your-file-system"></a><span data-ttu-id="ccbd2-112">Adja hozzá eseményindító és műveleteket a fájlrendszer való kapcsolódáshoz</span><span class="sxs-lookup"><span data-stu-id="ccbd2-112">Add trigger and actions for connecting to your file system</span></span>

1. <span data-ttu-id="ccbd2-113">Logikai alkalmazás létrehozása, és vegye fel a Dropbox eseményindító: **fájl létrehozásakor**</span><span class="sxs-lookup"><span data-stu-id="ccbd2-113">Create a logic app, and add this Dropbox trigger: **When a file is created**</span></span> 
2. <span data-ttu-id="ccbd2-114">Válassza ki az eseményindító **tovább** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-114">Under the trigger, choose **Next Step** > **Add an action**.</span></span> 
3. <span data-ttu-id="ccbd2-115">A keresési mezőbe, írja be a `file system` szeretné megjeleníteni az összes támogatott műveletek a fájlrendszer-összekötőhöz.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-115">In the search box, enter `file system` so you can view all supported actions for the File System connector.</span></span>

   ![Keresse meg a fájl összekötő](media/logic-apps-using-file-connector/search-file-connector.png)

2. <span data-ttu-id="ccbd2-117">Válassza ki a **létrehozás fájl** művelet, és a fájlrendszerhez kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-117">Choose the **Create file** action, and create a connection to your file system.</span></span>

   <span data-ttu-id="ccbd2-118">Ha nincs meglévő kapcsolat, a rendszer kéri a hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-118">If you don't have an existing connection, you are prompted to create one.</span></span>

   1. <span data-ttu-id="ccbd2-119">Válasszon **keresztül, a helyszíni adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-119">Choose **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="ccbd2-120">További tulajdonságok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-120">More properties appear.</span></span>
   2. <span data-ttu-id="ccbd2-121">Jelölje ki a legfelső szintű mappát, a fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-121">Select your root folder for your file system.</span></span>
      
       > [!NOTE]
       > <span data-ttu-id="ccbd2-122">A legfelső szintű mappa nem a fő szülőmappa, relatív útvonalakat minden fájl kapcsolatos műveletekhez használt.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-122">The root folder is the main parent folder, which is used for relative paths for all file-related actions.</span></span> <span data-ttu-id="ccbd2-123">Megadhat egy helyi mappába a számítógépen, ahol telepítve van a helyszíni data gateway, vagy a mappa lehet a számítógép által elérhető hálózati megosztáson.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-123">You can specify a local folder on the machine where the on-premises data gateway is installed, or the folder can be a network share that the machine can access.</span></span>

   3. <span data-ttu-id="ccbd2-124">Adja meg a felhasználónevet és jelszót az átjárót.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-124">Enter the username and password for the gateway.</span></span>
   4. <span data-ttu-id="ccbd2-125">Válassza ki a korábban telepített átjárót.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-125">Select the gateway that you previously installed.</span></span>

       ![Kapcsolat beállítása](media/logic-apps-using-file-connector/create-file.png)

3. <span data-ttu-id="ccbd2-127">Miután megadta a részleteit, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-127">After you provide all the details, choose **Create**.</span></span> 

   <span data-ttu-id="ccbd2-128">A Logic Apps konfigurálja, és teszteli a hálózati kapcsolatot, annak biztosítása, hogy a kapcsolat megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-128">Logic Apps configures and tests your connection, making sure that the connection works properly.</span></span> 
   <span data-ttu-id="ccbd2-129">Ha a kapcsolat megfelelően van beállítva, lásd: a korábban kiválasztott művelet beállításait.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-129">If the connection is set up correctly, you see options for the action that you previously selected.</span></span> 
   <span data-ttu-id="ccbd2-130">A fájl system-összekötő mostantól készen áll a használatra.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-130">The file system connector is now ready for use.</span></span>

4. <span data-ttu-id="ccbd2-131">Adja meg, hogy szeretné-e fájlok másolása a Dropbox a helyszíni fájlmegosztás gyökérmappájába.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-131">Specify that you want to copy files from Dropbox to the root folder for your on-premises file share.</span></span>

   ![Fájl művelet létrehozása](media/logic-apps-using-file-connector/create-file-filled.png)

5. <span data-ttu-id="ccbd2-133">Miután a Logic Apps alkalmazást átmásolja a fájlt, egy e-mailt küld, így az új fájl megfelelő felhasználók funkcióját, az Outlook művelet hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-133">After your logic app copies the file, add an Outlook action that sends an email so relevant users know about the new file.</span></span> <span data-ttu-id="ccbd2-134">Adja meg a címzetteket, cím és az e-mail.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-134">Enter the recipients, title, and body of the email.</span></span> 

   <span data-ttu-id="ccbd2-135">A dinamikus tartalom választó, az adatok kimenetek közül választhat a fájl összekötő, hozzáadhat további részleteket az e-mailt.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-135">In the dynamic content selector, you can choose data outputs from the file connector so you can add more details to the email.</span></span>

   ![Küldjön e-mailek művelet](media/logic-apps-using-file-connector/send-email.png)

6. <span data-ttu-id="ccbd2-137">Mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-137">Save your logic app.</span></span> <span data-ttu-id="ccbd2-138">Tesztelje az alkalmazás feltölteni a fájlt a dropbox alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-138">Test your app by uploading a file to Dropbox.</span></span> <span data-ttu-id="ccbd2-139">A helyszíni fájlmegosztáshoz beolvasása átmásolhatja a fájlt, és a műveletekre vonatkozó e-mailt kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-139">The file should get copied to the on-premises file share, and you should receive an email about the operation.</span></span>

   > [!TIP] 
   > <span data-ttu-id="ccbd2-140">Megtudhatja, hogyan [logikai alkalmazások figyelése](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="ccbd2-140">Learn how to [monitor your logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span></span>

<span data-ttu-id="ccbd2-141">Gratulálunk, most már rendelkezik egy működő logikai alkalmazás, amely képes csatlakozni a helyszíni fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-141">Congratulations, you now have a working logic app that can connect to your on-premises file system.</span></span> <span data-ttu-id="ccbd2-142">Próbálja meg az összekötő kínál, például más elavuló felfedezése:</span><span class="sxs-lookup"><span data-stu-id="ccbd2-142">Try exploring other functionalities that the connector offers, for example:</span></span>

- <span data-ttu-id="ccbd2-143">Fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccbd2-143">Create file</span></span>
- <span data-ttu-id="ccbd2-144">Mappában lévő fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="ccbd2-144">List files in folder</span></span>
- <span data-ttu-id="ccbd2-145">Fájl hozzáfűzése</span><span class="sxs-lookup"><span data-stu-id="ccbd2-145">Append file</span></span>
- <span data-ttu-id="ccbd2-146">Fájl törlése</span><span class="sxs-lookup"><span data-stu-id="ccbd2-146">Delete file</span></span>
- <span data-ttu-id="ccbd2-147">Fájl tartalmának lekérdezése</span><span class="sxs-lookup"><span data-stu-id="ccbd2-147">Get file content</span></span>
- <span data-ttu-id="ccbd2-148">Fájl tartalmának beolvasása elérési út segítségével</span><span class="sxs-lookup"><span data-stu-id="ccbd2-148">Get file content using path</span></span>
- <span data-ttu-id="ccbd2-149">Fájl metaadatot beszerezni</span><span class="sxs-lookup"><span data-stu-id="ccbd2-149">Get file metadata</span></span>
- <span data-ttu-id="ccbd2-150">Fájlmetaadatok beolvasása elérési út segítségével</span><span class="sxs-lookup"><span data-stu-id="ccbd2-150">Get file metadata using path</span></span>
- <span data-ttu-id="ccbd2-151">Gyökérmappában lévő fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="ccbd2-151">List files in root folder</span></span>
- <span data-ttu-id="ccbd2-152">Fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="ccbd2-152">Update file</span></span>

## <a name="view-the-swagger"></a><span data-ttu-id="ccbd2-153">A swagger megtekintése</span><span class="sxs-lookup"><span data-stu-id="ccbd2-153">View the swagger</span></span>
<span data-ttu-id="ccbd2-154">Tekintse meg a [részletek swagger](/connectors/fileconnector/).</span><span class="sxs-lookup"><span data-stu-id="ccbd2-154">See the [swagger details](/connectors/fileconnector/).</span></span> 

## <a name="get-help"></a><span data-ttu-id="ccbd2-155">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="ccbd2-155">Get help</span></span>

<span data-ttu-id="ccbd2-156">Látogasson el az [Azure Logic Apps fórumára](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps), ahol kérdéseket tehet fel és válaszolhat meg, valamint megtudhatja, mivel foglalkoznak az Azure Logic Apps más felhasználói.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-156">To ask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="ccbd2-157">Ha szeretné segíteni az Azure Logic Apps és összekötők fejlesztését, szavazzon az ötletekre az [Azure Logic Apps felhasználói visszajelzések oldalán](http://aka.ms/logicapps-wish), vagy küldje be saját javaslatait.</span><span class="sxs-lookup"><span data-stu-id="ccbd2-157">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ccbd2-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ccbd2-158">Next steps</span></span>

- <span data-ttu-id="ccbd2-159">[A helyszíni adatokhoz](../logic-apps/logic-apps-gateway-connection.md) a logic Apps alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="ccbd2-159">[Connect to on-premises data](../logic-apps/logic-apps-gateway-connection.md) from logic apps</span></span>
- <span data-ttu-id="ccbd2-160">További tudnivalók [vállalati integrációs](../logic-apps/logic-apps-enterprise-integration-overview.md)</span><span class="sxs-lookup"><span data-stu-id="ccbd2-160">Learn about [enterprise integration](../logic-apps/logic-apps-enterprise-integration-overview.md)</span></span>
