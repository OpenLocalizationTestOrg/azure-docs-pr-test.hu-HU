---
title: "az Azure Logic Apps fájlrendszerek aaaConnect tooon helyszíni |} Microsoft Docs"
description: "A logic app munkafolyamat hello helyszíni az átjáró és a fájlrendszer összekötő kapcsolódó tooon helyszíni fájlrendszer"
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
ms.openlocfilehash: beb5565293def4aba81f63f19e77d7498aac38c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-file-systems-from-logic-apps-with-hello-file-system-connector"></a><span data-ttu-id="6da3c-104">Csatlakozás tooon helyszíni fájlrendszer a logic Apps alkalmazásokból hello fájlrendszer összekötőn keresztül</span><span class="sxs-lookup"><span data-stu-id="6da3c-104">Connect tooon-premises file systems from logic apps with hello File System connector</span></span>

<span data-ttu-id="6da3c-105">Hibrid felhő kapcsolat központi toologic alkalmazásokat, így toomanage adatok és a biztonságos hozzáférés a helyszíni erőforrásokkal, a logic apps hello helyszíni data gateway használható.</span><span class="sxs-lookup"><span data-stu-id="6da3c-105">Hybrid cloud connectivity is central toologic apps, so toomanage data and securely access on-premises resources, your logic apps can use hello on-premises data gateway.</span></span> <span data-ttu-id="6da3c-106">Ebben a cikkben megmutatjuk, hogyan tooconnect tooan helyszíni fájlrendszer egy egyszerű forgatókönyv: másolja át a fájlt meg feltöltött tooDropbox tooa fájlmegosztásba, majd küldje el e-mailt.</span><span class="sxs-lookup"><span data-stu-id="6da3c-106">In this article, we show how tooconnect tooan on-premises file system with a basic scenario: copy a file that's uploaded tooDropbox tooa file share, then send an email.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6da3c-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6da3c-107">Prerequisites</span></span>

- <span data-ttu-id="6da3c-108">Telepítse és konfigurálja a hello legújabb [helyszíni adatátjáró](https://www.microsoft.com/download/details.aspx?id=53127).</span><span class="sxs-lookup"><span data-stu-id="6da3c-108">Install and configure hello latest [on-premises data gateway](https://www.microsoft.com/download/details.aspx?id=53127).</span></span>
- <span data-ttu-id="6da3c-109">Telepíteni a hello legújabb helyszíni data gateway 1.15.6150.1 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="6da3c-109">Install hello latest on-premises data gateway, version 1.15.6150.1 or above.</span></span> <span data-ttu-id="6da3c-110">[Csatlakozás helyszíni adatátjáró toohello](http://aka.ms/logicapps-gateway) listák hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6da3c-110">[Connect toohello on-premises data gateway](http://aka.ms/logicapps-gateway) lists hello steps.</span></span> <span data-ttu-id="6da3c-111">hello átjáró rendszerre telepíthető egy helyszíni gépen hello további hello lépéseit a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="6da3c-111">hello gateway must be installed on an on-premises machine before you can continue with hello rest of hello steps.</span></span>

## <a name="add-trigger-and-actions-for-connecting-tooyour-file-system"></a><span data-ttu-id="6da3c-112">Eseményindító és műveletek tooyour fájlrendszer kapcsolódás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6da3c-112">Add trigger and actions for connecting tooyour file system</span></span>

1. <span data-ttu-id="6da3c-113">Logikai alkalmazás létrehozása, és vegye fel a Dropbox eseményindító: **fájl létrehozásakor**</span><span class="sxs-lookup"><span data-stu-id="6da3c-113">Create a logic app, and add this Dropbox trigger: **When a file is created**</span></span> 
2. <span data-ttu-id="6da3c-114">Hello eseményindító, válassza ki **tovább** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6da3c-114">Under hello trigger, choose **Next Step** > **Add an action**.</span></span> 
3. <span data-ttu-id="6da3c-115">Hello keresési mezőbe, írja be a `file system` szeretné megjeleníteni az összes támogatott műveletek hello fájlrendszer összekötő.</span><span class="sxs-lookup"><span data-stu-id="6da3c-115">In hello search box, enter `file system` so you can view all supported actions for hello File System connector.</span></span>

   ![Keresse meg a fájl összekötő](media/logic-apps-using-file-connector/search-file-connector.png)

2. <span data-ttu-id="6da3c-117">Válassza ki a hello **létrehozás fájl** művelet, és hozzon létre egy kapcsolat tooyour fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="6da3c-117">Choose hello **Create file** action, and create a connection tooyour file system.</span></span>

   <span data-ttu-id="6da3c-118">Ha nincs meglévő kapcsolat, egy felszólító toocreate áll.</span><span class="sxs-lookup"><span data-stu-id="6da3c-118">If you don't have an existing connection, you are prompted toocreate one.</span></span>

   1. <span data-ttu-id="6da3c-119">Válasszon **keresztül, a helyszíni adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="6da3c-119">Choose **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="6da3c-120">További tulajdonságok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="6da3c-120">More properties appear.</span></span>
   2. <span data-ttu-id="6da3c-121">Jelölje ki a legfelső szintű mappát, a fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="6da3c-121">Select your root folder for your file system.</span></span>
      
       > [!NOTE]
       > <span data-ttu-id="6da3c-122">hello gyökérmappája hello fő fölérendelt mappája, relatív útvonalakat minden fájl kapcsolatos műveletekhez használt.</span><span class="sxs-lookup"><span data-stu-id="6da3c-122">hello root folder is hello main parent folder, which is used for relative paths for all file-related actions.</span></span> <span data-ttu-id="6da3c-123">Megadhat egy helyi mappába hello gépen, ahol hello helyszíni az átjáró telepítve van, vagy hello mappa lehet egy hálózati megosztást, amelyet a gép hello férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="6da3c-123">You can specify a local folder on hello machine where hello on-premises data gateway is installed, or hello folder can be a network share that hello machine can access.</span></span>

   3. <span data-ttu-id="6da3c-124">Hello felhasználónév és jelszó megadása hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="6da3c-124">Enter hello username and password for hello gateway.</span></span>
   4. <span data-ttu-id="6da3c-125">Válassza ki a korábban telepített hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="6da3c-125">Select hello gateway that you previously installed.</span></span>

       ![Kapcsolat beállítása](media/logic-apps-using-file-connector/create-file.png)

3. <span data-ttu-id="6da3c-127">Miután megadta az összes hello részletes, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6da3c-127">After you provide all hello details, choose **Create**.</span></span> 

   <span data-ttu-id="6da3c-128">A Logic Apps konfigurálja, és teszteli a hálózati kapcsolatot, annak biztosítása, hogy hello kapcsolat megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="6da3c-128">Logic Apps configures and tests your connection, making sure that hello connection works properly.</span></span> 
   <span data-ttu-id="6da3c-129">Ha hello kapcsolat megfelelően van beállítva, beállítások, amelyeket korábban hello a művelethez látható.</span><span class="sxs-lookup"><span data-stu-id="6da3c-129">If hello connection is set up correctly, you see options for hello action that you previously selected.</span></span> 
   <span data-ttu-id="6da3c-130">hello fájl system-összekötő mostantól készen áll a használatra.</span><span class="sxs-lookup"><span data-stu-id="6da3c-130">hello file system connector is now ready for use.</span></span>

4. <span data-ttu-id="6da3c-131">Adja meg, hogy a Dropbox toohello gyökérmappa toocopy fájlok a helyszíni fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="6da3c-131">Specify that you want toocopy files from Dropbox toohello root folder for your on-premises file share.</span></span>

   ![Fájl művelet létrehozása](media/logic-apps-using-file-connector/create-file-filled.png)

5. <span data-ttu-id="6da3c-133">A logic app másolatok hello fájl után egy e-mailt küld a megfelelő felhasználóhoz hello új fájl funkcióját, az Outlook művelet hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="6da3c-133">After your logic app copies hello file, add an Outlook action that sends an email so relevant users know about hello new file.</span></span> <span data-ttu-id="6da3c-134">Adja meg a hello címzetteknek, a jogcímre és az üdvözlő e-mail törzsét.</span><span class="sxs-lookup"><span data-stu-id="6da3c-134">Enter hello recipients, title, and body of hello email.</span></span> 

   <span data-ttu-id="6da3c-135">Hello dinamikus tartalom választó, az adatok kimenetek közül választhat hello fájl összekötő, hozzáadhat további részleteket toohello e-mailt.</span><span class="sxs-lookup"><span data-stu-id="6da3c-135">In hello dynamic content selector, you can choose data outputs from hello file connector so you can add more details toohello email.</span></span>

   ![Küldjön e-mailek művelet](media/logic-apps-using-file-connector/send-email.png)

6. <span data-ttu-id="6da3c-137">Mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6da3c-137">Save your logic app.</span></span> <span data-ttu-id="6da3c-138">Az alkalmazás tesztelése a fájl tooDropbox feltöltésével.</span><span class="sxs-lookup"><span data-stu-id="6da3c-138">Test your app by uploading a file tooDropbox.</span></span> <span data-ttu-id="6da3c-139">hello fájl kapja meg a másolt toohello helyszíni fájlmegosztást, és hello műveletekre vonatkozó e-mailt kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="6da3c-139">hello file should get copied toohello on-premises file share, and you should receive an email about hello operation.</span></span>

   > [!TIP] 
   > <span data-ttu-id="6da3c-140">Ismerje meg, hogyan túl[logikai alkalmazások figyelése](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="6da3c-140">Learn how too[monitor your logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span></span>

<span data-ttu-id="6da3c-141">Gratulálunk, most már rendelkezik, amely képes kapcsolódni a helyi fájlrendszer tooyour működő logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6da3c-141">Congratulations, you now have a working logic app that can connect tooyour on-premises file system.</span></span> <span data-ttu-id="6da3c-142">Próbálja más összekötő ajánlatokat, például hello elavuló felfedezése:</span><span class="sxs-lookup"><span data-stu-id="6da3c-142">Try exploring other functionalities that hello connector offers, for example:</span></span>

- <span data-ttu-id="6da3c-143">Fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="6da3c-143">Create file</span></span>
- <span data-ttu-id="6da3c-144">Mappában lévő fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="6da3c-144">List files in folder</span></span>
- <span data-ttu-id="6da3c-145">Fájl hozzáfűzése</span><span class="sxs-lookup"><span data-stu-id="6da3c-145">Append file</span></span>
- <span data-ttu-id="6da3c-146">Fájl törlése</span><span class="sxs-lookup"><span data-stu-id="6da3c-146">Delete file</span></span>
- <span data-ttu-id="6da3c-147">Fájl tartalmának lekérdezése</span><span class="sxs-lookup"><span data-stu-id="6da3c-147">Get file content</span></span>
- <span data-ttu-id="6da3c-148">Fájl tartalmának beolvasása elérési út segítségével</span><span class="sxs-lookup"><span data-stu-id="6da3c-148">Get file content using path</span></span>
- <span data-ttu-id="6da3c-149">Fájl metaadatot beszerezni</span><span class="sxs-lookup"><span data-stu-id="6da3c-149">Get file metadata</span></span>
- <span data-ttu-id="6da3c-150">Fájlmetaadatok beolvasása elérési út segítségével</span><span class="sxs-lookup"><span data-stu-id="6da3c-150">Get file metadata using path</span></span>
- <span data-ttu-id="6da3c-151">Gyökérmappában lévő fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="6da3c-151">List files in root folder</span></span>
- <span data-ttu-id="6da3c-152">Fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="6da3c-152">Update file</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="6da3c-153">Nézet hello swagger</span><span class="sxs-lookup"><span data-stu-id="6da3c-153">View hello swagger</span></span>
<span data-ttu-id="6da3c-154">Lásd: hello [részletek swagger](/connectors/fileconnector/).</span><span class="sxs-lookup"><span data-stu-id="6da3c-154">See hello [swagger details](/connectors/fileconnector/).</span></span> 

## <a name="get-help"></a><span data-ttu-id="6da3c-155">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="6da3c-155">Get help</span></span>

<span data-ttu-id="6da3c-156">tooask kérdések kérdést, és ismerje meg, milyen egyéb Azure Logic Apps felhasználók végzi, látogasson el hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="6da3c-156">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="6da3c-157">toohelp Azure Logic Apps alkalmazások és összekötők javítása, szavazhatnak, vagy küldje el a következő hello ötleteket [Azure Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="6da3c-157">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6da3c-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6da3c-158">Next steps</span></span>

- <span data-ttu-id="6da3c-159">[Csatlakozás tooon helyszíni adatokhoz](../logic-apps/logic-apps-gateway-connection.md) a logic Apps alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="6da3c-159">[Connect tooon-premises data](../logic-apps/logic-apps-gateway-connection.md) from logic apps</span></span>
- <span data-ttu-id="6da3c-160">További tudnivalók [vállalati integrációs](../logic-apps/logic-apps-enterprise-integration-overview.md)</span><span class="sxs-lookup"><span data-stu-id="6da3c-160">Learn about [enterprise integration](../logic-apps/logic-apps-enterprise-integration-overview.md)</span></span>
