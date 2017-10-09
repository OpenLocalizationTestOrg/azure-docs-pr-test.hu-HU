---
title: "az Azure blob storage-összekötőt a Logic Apps a aaaAdd hello |} Microsoft Docs"
description: "Első lépések és a logikai alkalmazás hello Azure blob storage-összekötő konfigurálása"
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
ms.openlocfilehash: add61287ef1b2228ef9d3f54ce082807bad6858b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-blob-storage-connector-in-a-logic-app"></a><span data-ttu-id="3151d-103">A logikai alkalmazás hello Azure blob storage-összekötő használatára</span><span class="sxs-lookup"><span data-stu-id="3151d-103">Use hello Azure blob storage connector in a logic app</span></span>
<span data-ttu-id="3151d-104">Használjon hello Azure Blob storage összekötő tooupload, frissítése, beolvasása, és törölje a blobot, amely a tárfiókon belül logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3151d-104">Use hello Azure Blob storage connector tooupload, update, get, and delete blobs in your storage account, all within a logic app.</span></span>  

<span data-ttu-id="3151d-105">Az Azure blob storage szolgáltatással meg:</span><span class="sxs-lookup"><span data-stu-id="3151d-105">With Azure blob storage, you:</span></span>

* <span data-ttu-id="3151d-106">A munkafolyamat új projektek feltölteni, vagy nemrég frissített fájlok első hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="3151d-106">Build your workflow by uploading new projects, or getting files that have been recently updated.</span></span>
* <span data-ttu-id="3151d-107">Műveletek tooget fájl metaadatok, és törölje a fájlt, a fájlok másolása és további.</span><span class="sxs-lookup"><span data-stu-id="3151d-107">Use actions tooget file metadata, delete a file, copy files, and more.</span></span> <span data-ttu-id="3151d-108">Például frissítésekor egy eszközt az Azure webhelyén (trigger), majd frissítse a fájlt a blob Storage tárolóban (a műveletet).</span><span class="sxs-lookup"><span data-stu-id="3151d-108">For example, when a tool is updated in an Azure web site (a trigger), then update a file in blob storage (an action).</span></span> 

<span data-ttu-id="3151d-109">Ez a témakör bemutatja, hogyan toouse hello blob storage-összekötőt a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3151d-109">This topic shows you how toouse hello blob storage connector in a logic app.</span></span>

<span data-ttu-id="3151d-110">További információk a Logic Apps toolearn lásd [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3151d-110">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

<span data-ttu-id="3151d-111">További információk a Logic Apps toolearn lásd [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3151d-111">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooazure-blob-storage"></a><span data-ttu-id="3151d-112">Csatlakozás tooAzure blob-tároló</span><span class="sxs-lookup"><span data-stu-id="3151d-112">Connect tooAzure blob storage</span></span>
<span data-ttu-id="3151d-113">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3151d-113">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="3151d-114">Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="3151d-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="3151d-115">Például tooconnect tooa tárfiókot, először létre kell hoznia egy blob-tároló *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="3151d-115">For example, tooconnect tooa storage account, you first create a blob storage *connection*.</span></span> <span data-ttu-id="3151d-116">toocreate a kapcsolat létrehozásakor, adja meg a hello hitelesítő adatokkal kell általában tooaccess hello szolgáltatáshoz való kapcsolódás esetén.</span><span class="sxs-lookup"><span data-stu-id="3151d-116">toocreate a connection, enter hello credentials you normally use tooaccess hello service you are connecting to.</span></span> <span data-ttu-id="3151d-117">Ezért az Azure storage hello hitelesítő adatok tooyour fiók toocreate hello tárolókapcsolat meg.</span><span class="sxs-lookup"><span data-stu-id="3151d-117">So with Azure storage, enter hello credentials tooyour storage account toocreate hello connection.</span></span> 

#### <a name="create-hello-connection"></a><span data-ttu-id="3151d-118">Hello kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="3151d-118">Create hello connection</span></span>
> [!INCLUDE [Create a connection tooAzure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a><span data-ttu-id="3151d-119">Eseményindítók</span><span class="sxs-lookup"><span data-stu-id="3151d-119">Use a trigger</span></span>
<span data-ttu-id="3151d-120">Ez az összekötő nem rendelkezik a eseményindítókat.</span><span class="sxs-lookup"><span data-stu-id="3151d-120">This connector does not have any triggers.</span></span> <span data-ttu-id="3151d-121">Használjon más eseményindítók toostart hello logikai alkalmazás, például az ismétlődési eseményindítót, egy HTTP Webhook eseményindító, eseményindítók érhető el a többi összekötőt, és több.</span><span class="sxs-lookup"><span data-stu-id="3151d-121">Use other triggers toostart hello logic app, such as a Recurrence trigger, an HTTP Webhook trigger, triggers available with other connectors, and more.</span></span> <span data-ttu-id="3151d-122">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md) példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="3151d-122">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) provides an example.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="3151d-123">Egy művelettel</span><span class="sxs-lookup"><span data-stu-id="3151d-123">Use an action</span></span>
<span data-ttu-id="3151d-124">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="3151d-124">An action is an operation carried out by hello workflow defined in a logic app.</span></span>

1. <span data-ttu-id="3151d-125">Válassza ki a hello plusz jel.</span><span class="sxs-lookup"><span data-stu-id="3151d-125">Select hello plus sign.</span></span> <span data-ttu-id="3151d-126">Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy egy hello **további** beállítások.</span><span class="sxs-lookup"><span data-stu-id="3151d-126">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. <span data-ttu-id="3151d-127">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="3151d-127">Choose **Add an action**.</span></span>
3. <span data-ttu-id="3151d-128">Hello szövegmezőbe írja be a "blob" tooget összes hello elérhető műveletek listáját.</span><span class="sxs-lookup"><span data-stu-id="3151d-128">In hello text box, type “blob” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. <span data-ttu-id="3151d-129">Válassza ki a fenti példában **AzureBlob - elérési út használatával Get fájlmetaadata**.</span><span class="sxs-lookup"><span data-stu-id="3151d-129">In our example, choose **AzureBlob - Get file metadata using path**.</span></span> <span data-ttu-id="3151d-130">Ha már létezik egy kapcsolat, majd válassza ki hello **...** (Objektumválasztó megjelenítése) gomb tooselect egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="3151d-130">If a connection already exists, then select hello **...** (Show Picker) button tooselect a file.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    <span data-ttu-id="3151d-131">Ha hello kapcsolati adatokat kéri, adja meg hello részletek toocreate hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="3151d-131">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="3151d-132">[Hello kapcsolat létrehozása](connectors-create-api-azureblobstorage.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="3151d-132">[Create hello connection](connectors-create-api-azureblobstorage.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="3151d-133">Ebben a példában azt lekérése hello metaadat-fájl.</span><span class="sxs-lookup"><span data-stu-id="3151d-133">In this example, we get hello metadata of a file.</span></span> <span data-ttu-id="3151d-134">toosee hello metaadatok, egy másik művelet, amely létrehoz egy új fájlt egy másik összekötővel hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3151d-134">toosee hello metadata, add another action that creates a new file using another connector.</span></span> <span data-ttu-id="3151d-135">Adja hozzá például a onedrive vállalati verzió művelet, amely létrehoz egy új "teszt" hello metaadatok alapján.</span><span class="sxs-lookup"><span data-stu-id="3151d-135">For example, add a OneDrive action that creates a new "test" file based on hello metadata.</span></span> 


5. <span data-ttu-id="3151d-136">**Mentés** a módosításokat (bal felső sarkában hello eszköztár).</span><span class="sxs-lookup"><span data-stu-id="3151d-136">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="3151d-137">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="3151d-137">Your logic app is saved and may be automatically enabled.</span></span>

> [!TIP]
> <span data-ttu-id="3151d-138">[A Tártallózó](http://storageexplorer.com/) egy remek eszköz túl kezelése több tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="3151d-138">[Storage Explorer](http://storageexplorer.com/) is a great tool too manage multiple storage accounts.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="3151d-139">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="3151d-139">Connector-specific details</span></span>

<span data-ttu-id="3151d-140">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/azureblobconnector/).</span><span class="sxs-lookup"><span data-stu-id="3151d-140">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/azureblobconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3151d-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3151d-141">Next steps</span></span>
<span data-ttu-id="3151d-142">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3151d-142">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="3151d-143">Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="3151d-143">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

