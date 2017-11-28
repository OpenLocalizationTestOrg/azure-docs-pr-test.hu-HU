---
title: "Adja hozzá az Azure blob storage összekötőt a Logic Apps |} Microsoft Docs"
description: "Első lépések és az Azure blob storage-összekötő konfigurálása a logikai alkalmazás"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5dc3f75-6bea-420b-b250-183668d2848d
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/02/2017
ms.author: mandia; ladocs
ms.openlocfilehash: bc7908868828bd1628633cf9e57f8c44f8000827
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-blob-storage-connector-in-a-logic-app"></a><span data-ttu-id="9830e-103">A logikai alkalmazás az Azure blob storage-összekötő használatára</span><span class="sxs-lookup"><span data-stu-id="9830e-103">Use the Azure blob storage connector in a logic app</span></span>
<span data-ttu-id="9830e-104">Az Azure Blob storage-összekötő használatával töltse fel, frissítése, beszerzése, illetve törölni a blobot, amely a tárfiókon belül egy logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9830e-104">Use the Azure Blob storage connector to upload, update, get, and delete blobs in your storage account, all within a logic app.</span></span>  

<span data-ttu-id="9830e-105">Az Azure blob storage szolgáltatással meg:</span><span class="sxs-lookup"><span data-stu-id="9830e-105">With Azure blob storage, you:</span></span>

* <span data-ttu-id="9830e-106">A munkafolyamat új projektek feltölteni, vagy nemrég frissített fájlok első hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="9830e-106">Build your workflow by uploading new projects, or getting files that have been recently updated.</span></span>
* <span data-ttu-id="9830e-107">Fájl metaadatot beszerezni, törölje a fájlt, a fájlok másolása és további műveleteket használni.</span><span class="sxs-lookup"><span data-stu-id="9830e-107">Use actions to get file metadata, delete a file, copy files, and more.</span></span> <span data-ttu-id="9830e-108">Például frissítésekor egy eszközt az Azure webhelyén (trigger), majd frissítse a fájlt a blob Storage tárolóban (a műveletet).</span><span class="sxs-lookup"><span data-stu-id="9830e-108">For example, when a tool is updated in an Azure web site (a trigger), then update a file in blob storage (an action).</span></span> 

<span data-ttu-id="9830e-109">Ez a témakör bemutatja, hogyan a blob storage összekötő használatára a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9830e-109">This topic shows you how to use the blob storage connector in a logic app.</span></span>

<span data-ttu-id="9830e-110">A Logic Apps kapcsolatos további információkért lásd: [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9830e-110">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

<span data-ttu-id="9830e-111">A Logic Apps kapcsolatos további információkért lásd: [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9830e-111">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-azure-blob-storage"></a><span data-ttu-id="9830e-112">Csatlakozás az Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="9830e-112">Connect to Azure blob storage</span></span>
<span data-ttu-id="9830e-113">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="9830e-113">Before your logic app can access any service, you first create a *connection* to the service.</span></span> <span data-ttu-id="9830e-114">Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="9830e-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="9830e-115">Például szeretne csatlakozni egy tárfiókot, először létre kell hoznia egy blob-tároló *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="9830e-115">For example, to connect to a storage account, you first create a blob storage *connection*.</span></span> <span data-ttu-id="9830e-116">VPN-kapcsolat létrehozásához adja meg a hitelesítő adatokkal kell általában hozzáférhetnek a szolgáltatáshoz való kapcsolódás esetén.</span><span class="sxs-lookup"><span data-stu-id="9830e-116">To create a connection, enter the credentials you normally use to access the service you are connecting to.</span></span> <span data-ttu-id="9830e-117">Ezért az Azure storage adja meg a hitelesítő adatokat a tárfiókhoz a VPN-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9830e-117">So with Azure storage, enter the credentials to your storage account to create the connection.</span></span> 

#### <a name="create-the-connection"></a><span data-ttu-id="9830e-118">A kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="9830e-118">Create the connection</span></span>
> [!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a><span data-ttu-id="9830e-119">Eseményindítók</span><span class="sxs-lookup"><span data-stu-id="9830e-119">Use a trigger</span></span>
<span data-ttu-id="9830e-120">Ez az összekötő nem rendelkezik a eseményindítókat.</span><span class="sxs-lookup"><span data-stu-id="9830e-120">This connector does not have any triggers.</span></span> <span data-ttu-id="9830e-121">Más eseményindítók segítségével indítsa el a logikai alkalmazás, például az ismétlődési eseményindítót, egy HTTP Webhook eseményindító, eseményindítók érhető el a többi összekötőt, és több.</span><span class="sxs-lookup"><span data-stu-id="9830e-121">Use other triggers to start the logic app, such as a Recurrence trigger, an HTTP Webhook trigger, triggers available with other connectors, and more.</span></span> <span data-ttu-id="9830e-122">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md) példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="9830e-122">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) provides an example.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="9830e-123">Egy művelettel</span><span class="sxs-lookup"><span data-stu-id="9830e-123">Use an action</span></span>
<span data-ttu-id="9830e-124">Egy művelet során a logikai alkalmazás definiált munkafolyamat által végzett.</span><span class="sxs-lookup"><span data-stu-id="9830e-124">An action is an operation carried out by the workflow defined in a logic app.</span></span>

1. <span data-ttu-id="9830e-125">Kattintson a plusz ikonra.</span><span class="sxs-lookup"><span data-stu-id="9830e-125">Select the plus sign.</span></span> <span data-ttu-id="9830e-126">Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy az egyik a **további** beállítások.</span><span class="sxs-lookup"><span data-stu-id="9830e-126">You see several choices: **Add an action**, **Add a condition**, or one of the **More** options.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. <span data-ttu-id="9830e-127">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="9830e-127">Choose **Add an action**.</span></span>
3. <span data-ttu-id="9830e-128">A szövegmezőbe írja be a "blob" az összes elérhető műveletek listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9830e-128">In the text box, type “blob” to get a list of all the available actions.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. <span data-ttu-id="9830e-129">Válassza ki a fenti példában **AzureBlob - elérési út használatával Get fájlmetaadata**.</span><span class="sxs-lookup"><span data-stu-id="9830e-129">In our example, choose **AzureBlob - Get file metadata using path**.</span></span> <span data-ttu-id="9830e-130">Ha már létezik egy kapcsolat, majd válassza ki a **...** Válassza ki a fájlt (objektumválasztó megjelenítése) gombra.</span><span class="sxs-lookup"><span data-stu-id="9830e-130">If a connection already exists, then select the **...** (Show Picker) button to select a file.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    <span data-ttu-id="9830e-131">Ha a kapcsolati adatokat kéri, adja meg a részleteket a VPN-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9830e-131">If you are prompted for the connection information, then enter the details to create the connection.</span></span> <span data-ttu-id="9830e-132">[A kapcsolat létrehozása](connectors-create-api-azureblobstorage.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="9830e-132">[Create the connection](connectors-create-api-azureblobstorage.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="9830e-133">Ebben a példában azt megszerezni a metaadatokat egy fájl.</span><span class="sxs-lookup"><span data-stu-id="9830e-133">In this example, we get the metadata of a file.</span></span> <span data-ttu-id="9830e-134">A metaadatok, vegye fel egy újabb műveletet, amely létrehoz egy új fájlt egy másik összekötő segítségével.</span><span class="sxs-lookup"><span data-stu-id="9830e-134">To see the metadata, add another action that creates a new file using another connector.</span></span> <span data-ttu-id="9830e-135">Adja hozzá például a onedrive vállalati verzió művelet által létrehozott egy új "teszt" fájlt a metaadatok alapján.</span><span class="sxs-lookup"><span data-stu-id="9830e-135">For example, add a OneDrive action that creates a new "test" file based on the metadata.</span></span> 


5. <span data-ttu-id="9830e-136">**Mentés** a módosításokat (bal felső sarkában az eszköztár).</span><span class="sxs-lookup"><span data-stu-id="9830e-136">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="9830e-137">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="9830e-137">Your logic app is saved and may be automatically enabled.</span></span>

> [!TIP]
> <span data-ttu-id="9830e-138">[A Tártallózó](http://storageexplorer.com/) kiváló eszköz a több tárfiókot kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="9830e-138">[Storage Explorer](http://storageexplorer.com/) is a great tool to  manage multiple storage accounts.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="9830e-139">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="9830e-139">Connector-specific details</span></span>

<span data-ttu-id="9830e-140">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/azureblobconnector/).</span><span class="sxs-lookup"><span data-stu-id="9830e-140">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/azureblobconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9830e-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9830e-141">Next steps</span></span>
<span data-ttu-id="9830e-142">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9830e-142">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="9830e-143">Az egyéb rendelkezésre álló összekötők Logic Apps, megismerkedhet a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="9830e-143">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

