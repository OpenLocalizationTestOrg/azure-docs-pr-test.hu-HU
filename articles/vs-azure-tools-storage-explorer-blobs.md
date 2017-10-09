---
title: "aaaManage Azure Blob Storage-erőforrások a Tártallózó (előzetes verzió) |} Microsoft Docs"
description: "Az Azure Blob-tárolók és Blobok a Tártallózó (előzetes verzió) kezelése"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 2f09e545-ec94-4d89-b96c-14783cc9d7a9
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 503dd061b205875da127378ab48e8d465800fc09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a><span data-ttu-id="987f1-103">A Tártallózó (előzetes verzió) Azure Blob Storage-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="987f1-103">Manage Azure Blob Storage resources with Storage Explorer (Preview)</span></span>
## <a name="overview"></a><span data-ttu-id="987f1-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="987f1-104">Overview</span></span>
<span data-ttu-id="987f1-105">[Az Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához.</span><span class="sxs-lookup"><span data-stu-id="987f1-105">[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span>
<span data-ttu-id="987f1-106">Használhatja a Blob storage tooexpose adatok nyilvánosan toohello globális vagy toostore alkalmazásadatok közvetlenül a Microsoftnak.</span><span class="sxs-lookup"><span data-stu-id="987f1-106">You can use Blob storage tooexpose data publicly toohello world, or toostore application data privately.</span></span> <span data-ttu-id="987f1-107">Ebből a cikkből megtudhatja, hogyan toouse Tártallózó (előzetes verzió) toowork a blob-tárolók és blobok.</span><span class="sxs-lookup"><span data-stu-id="987f1-107">In this article, you'll learn how toouse Storage Explorer (Preview) toowork with blob containers and blobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="987f1-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="987f1-108">Prerequisites</span></span>
<span data-ttu-id="987f1-109">toocomplete hello cikkben leírt lépéseket, az alábbi hello lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="987f1-109">toocomplete hello steps in this article, you'll need hello following:</span></span>

* [<span data-ttu-id="987f1-110">A Tártallózó (előzetes verzió) letöltése és telepítése</span><span class="sxs-lookup"><span data-stu-id="987f1-110">Download and install Storage Explorer (preview)</span></span>](http://www.storageexplorer.com)
* [<span data-ttu-id="987f1-111">Tooa Azure storage-fiók vagy a szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="987f1-111">Connect tooa Azure storage account or service</span></span>](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a><span data-ttu-id="987f1-112">A blob-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="987f1-112">Create a blob container</span></span>
<span data-ttu-id="987f1-113">Minden BLOB egy blob tároló, amely egyszerűen blobok logikai csoportosítása kell lennie.</span><span class="sxs-lookup"><span data-stu-id="987f1-113">All blobs must reside in a blob container, which is simply a logical grouping of blobs.</span></span> <span data-ttu-id="987f1-114">Egy fiók korlátlan számú tárolót tartalmazhat, és minden egyes tároló korlátlan számú BLOB tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="987f1-114">An account can contain an unlimited number of containers, and each container can store an unlimited number of blobs.</span></span>

<span data-ttu-id="987f1-115">hello következő lépések bemutatják, hogyan toocreate egy blob tároló a Tártallózó (előzetes verzió) belül.</span><span class="sxs-lookup"><span data-stu-id="987f1-115">hello following steps illustrate how toocreate a blob container within Storage Explorer (Preview).</span></span>

1. <span data-ttu-id="987f1-116">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="987f1-116">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="987f1-117">Hello bal oldali ablaktáblán bontsa ki a hello tárfiókot, amelyen belül kívánja toocreate hello blob tároló.</span><span class="sxs-lookup"><span data-stu-id="987f1-117">In hello left pane, expand hello storage account within which you wish toocreate hello blob container.</span></span>
3. <span data-ttu-id="987f1-118">Kattintson a jobb gombbal **Blobtárolók**, és válassza – hello helyi menüből – a **Blob-tároló létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="987f1-118">Right-click **Blob Containers**, and - from hello context menu - select **Create Blob Container**.</span></span>

   ![Blob tárolók a helyi menü létrehozása][0]
4. <span data-ttu-id="987f1-120">A szövegmezőben megjelenik, hello alatt **Blobtárolók** mappa.</span><span class="sxs-lookup"><span data-stu-id="987f1-120">A text box will appear below hello **Blob Containers** folder.</span></span> <span data-ttu-id="987f1-121">Adja meg a blob-tároló hello nevét.</span><span class="sxs-lookup"><span data-stu-id="987f1-121">Enter hello name for your blob container.</span></span> <span data-ttu-id="987f1-122">Lásd: hello [tároló elnevezési szabályait](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) listáját szakasza szabályok és a blob tárolók elnevezési korlátozás.</span><span class="sxs-lookup"><span data-stu-id="987f1-122">See hello [Container naming rules](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) section for a list of rules and restrictions on naming blob containers.</span></span>

   ![Szövegmező Blob tárolók létrehozása][1]
5. <span data-ttu-id="987f1-124">Nyomja le az **Enter** befejezése után toocreate hello blob tároló, vagy **Esc** toocancel.</span><span class="sxs-lookup"><span data-stu-id="987f1-124">Press **Enter** when done toocreate hello blob container, or **Esc** toocancel.</span></span> <span data-ttu-id="987f1-125">Ha hello blob tároló sikeresen létrejött, akkor jelenik meg a hello **Blobtárolók** hello mappa kiválasztott tárfiók.</span><span class="sxs-lookup"><span data-stu-id="987f1-125">Once hello blob container has been successfully created, it will be displayed under hello **Blob Containers** folder for hello selected storage account.</span></span>

   ![BLOB-tároló létrehozása][2]

## <a name="view-a-blob-containers-contents"></a><span data-ttu-id="987f1-127">A blob-tároló tartalmának megtekintése</span><span class="sxs-lookup"><span data-stu-id="987f1-127">View a blob container's contents</span></span>
<span data-ttu-id="987f1-128">BLOB tárolók blobok és mappák (BLOB is tartalmazhat) tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="987f1-128">Blob containers contain blobs and folders (that can also contain blobs).</span></span>

<span data-ttu-id="987f1-129">hello következő lépések bemutatják, hogyan tooview hello tartalmát egy blob-tároló belül a Tártallózó (előzetes verzió):</span><span class="sxs-lookup"><span data-stu-id="987f1-129">hello following steps illustrate how tooview hello contents of a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="987f1-130">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="987f1-130">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="987f1-131">Hello bal oldali ablaktáblán bontsa ki a hello blob tároló tooview kívánja tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="987f1-131">In hello left pane, expand hello storage account containing hello blob container you wish tooview.</span></span>
3. <span data-ttu-id="987f1-132">Bontsa ki a hello tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="987f1-132">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="987f1-133">Kattintson a jobb gombbal hello blob tároló meg akarja tooview, és válassza – a hello helyi menüből – **Blob tároló szerkesztő megnyitása**.</span><span class="sxs-lookup"><span data-stu-id="987f1-133">Right-click hello blob container you wish tooview, and - from hello context menu - select **Open Blob Container Editor**.</span></span>
   <span data-ttu-id="987f1-134">Kattintson duplán a hello blob tároló tooview kívánja is.</span><span class="sxs-lookup"><span data-stu-id="987f1-134">You can also double-click hello blob container you wish tooview.</span></span>

   ![Nyissa meg a blob tároló szerkesztő helyi menü][19]
5. <span data-ttu-id="987f1-136">hello fő ablaktábla hello blob tároló tartalmának.</span><span class="sxs-lookup"><span data-stu-id="987f1-136">hello main pane will display hello blob container's contents.</span></span>

   ![A BLOB-tároló szerkesztő][3]

## <a name="delete-a-blob-container"></a><span data-ttu-id="987f1-138">A blob-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="987f1-138">Delete a blob container</span></span>
<span data-ttu-id="987f1-139">BLOB tárolók egyszerűen hozható létre és igény szerint törölve.</span><span class="sxs-lookup"><span data-stu-id="987f1-139">Blob containers can be easily created and deleted as needed.</span></span> <span data-ttu-id="987f1-140">(toosee hogyan toodelete személy blobok, tekintse meg a toohello szakasz [kezelése a blob-tárolóban lévő blobok](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="987f1-140">(toosee how toodelete individual blobs, refer toohello section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="987f1-141">hello következő lépések bemutatják, hogyan toodelete egy blob-tároló belül a Tártallózó (előzetes verzió):</span><span class="sxs-lookup"><span data-stu-id="987f1-141">hello following steps illustrate how toodelete a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="987f1-142">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="987f1-142">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="987f1-143">Hello bal oldali ablaktáblán bontsa ki a hello blob tároló tooview kívánja tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="987f1-143">In hello left pane, expand hello storage account containing hello blob container you wish tooview.</span></span>
3. <span data-ttu-id="987f1-144">Bontsa ki a hello tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="987f1-144">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="987f1-145">Kattintson a jobb gombbal hello blob tároló meg akarja toodelete, és válassza – a hello helyi menüből – **törlése**.</span><span class="sxs-lookup"><span data-stu-id="987f1-145">Right-click hello blob container you wish toodelete, and - from hello context menu - select **Delete**.</span></span>
   <span data-ttu-id="987f1-146">Is **törlése** toodelete hello kijelölt blob tároló.</span><span class="sxs-lookup"><span data-stu-id="987f1-146">You can also press **Delete** toodelete hello currently selected blob container.</span></span>

   ![A blob tároló helyi menü törlése][4]
5. <span data-ttu-id="987f1-148">Válassza ki **Igen** toohello megerősítő párbeszédpanele.</span><span class="sxs-lookup"><span data-stu-id="987f1-148">Select **Yes** toohello confirmation dialog.</span></span>

   ![A blob tároló megerősítő törlése][5]

## <a name="copy-a-blob-container"></a><span data-ttu-id="987f1-150">Másolja a blob-tároló</span><span class="sxs-lookup"><span data-stu-id="987f1-150">Copy a blob container</span></span>
<span data-ttu-id="987f1-151">A Tártallózó (előzetes verzió) lehetővé teszi a blob-tároló toohello vágólapra toocopy, és illessze be egy másik tárfiókhoz BLOB-tároló.</span><span class="sxs-lookup"><span data-stu-id="987f1-151">Storage Explorer (Preview) enables you toocopy a blob container toohello clipboard, and then paste that blob container into another storage account.</span></span> <span data-ttu-id="987f1-152">(toosee hogyan toocopy személy blobok, tekintse meg a toohello szakasz [kezelése a blob-tárolóban lévő blobok](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="987f1-152">(toosee how toocopy individual blobs, refer toohello section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="987f1-153">hello lépések bemutatják, hogyan toocopy egy blob tárolót egy tárolási fiók tooanother.</span><span class="sxs-lookup"><span data-stu-id="987f1-153">hello following steps illustrate how toocopy a blob container from one storage account tooanother.</span></span>

1. <span data-ttu-id="987f1-154">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="987f1-154">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="987f1-155">Hello bal oldali ablaktáblán bontsa ki a hello blob tároló toocopy kívánja tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="987f1-155">In hello left pane, expand hello storage account containing hello blob container you wish toocopy.</span></span>
3. <span data-ttu-id="987f1-156">Bontsa ki a hello tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="987f1-156">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="987f1-157">Kattintson a jobb gombbal hello blob tároló meg akarja toocopy, és válassza – a hello helyi menüből – **másolási Blob tároló**.</span><span class="sxs-lookup"><span data-stu-id="987f1-157">Right-click hello blob container you wish toocopy, and - from hello context menu - select **Copy Blob Container**.</span></span>

   ![Másolja a blob tároló helyi menü][6]
5. <span data-ttu-id="987f1-159">Kattintson a jobb gombbal a kívánt hello "target" tárfiók, amelybe azt szeretné, hogy toopaste hello blob tároló, és válassza – a hello helyi menüből – **Beillesztés Blob tároló**.</span><span class="sxs-lookup"><span data-stu-id="987f1-159">Right-click hello desired "target" storage account into which you want toopaste hello blob container, and - from hello context menu - select **Paste Blob Container**.</span></span>

   ![Beillesztés blob tároló helyi menü][7]

## <a name="get-hello-sas-for-a-blob-container"></a><span data-ttu-id="987f1-161">Hello SAS lekérése blob tárolóhoz</span><span class="sxs-lookup"><span data-stu-id="987f1-161">Get hello SAS for a blob container</span></span>
<span data-ttu-id="987f1-162">A [közös hozzáférésű jogosultságkód (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) delegált hozzáférést tooresources a tárfiókban lévő biztosít.</span><span class="sxs-lookup"><span data-stu-id="987f1-162">A [shared access signature (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) provides delegated access tooresources in your storage account.</span></span>
<span data-ttu-id="987f1-163">Ez azt jelenti, hogy egy ügyfél csak korlátozott engedélyekkel tooobjects a tárfiókban lévő egy megadott időszakban, és engedélyeket, megadott számú anélkül, hogy a fiók hozzáférési kulcsait megosztásához biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="987f1-163">This means that you can grant a client limited permissions tooobjects in your storage account for a specified period of time and with a specified set of permissions, without having to share your account access keys.</span></span>

<span data-ttu-id="987f1-164">hello következő lépések bemutatják, hogyan toocreate egy SAS-t egy blob-tároló:</span><span class="sxs-lookup"><span data-stu-id="987f1-164">hello following steps illustrate how toocreate a SAS for a blob container:</span></span>

1. <span data-ttu-id="987f1-165">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="987f1-165">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="987f1-166">Hello bal oldali ablaktáblán bontsa ki a hello blob tároló, amelynek kívánja tooget Aláírást tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="987f1-166">In hello left pane, expand hello storage account containing hello blob container for which you wish tooget a SAS.</span></span>
3. <span data-ttu-id="987f1-167">Bontsa ki a hello tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="987f1-167">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="987f1-168">Kattintson a jobb gombbal a hello kívánt blob tároló, és válassza – a hello helyi menüből – **közös hozzáférésű Jogosultságkód beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="987f1-168">Right-click hello desired blob container, and - from hello context menu - select **Get Shared Access Signature**.</span></span>

   ![A helyi menü SAS lekérése][8]
5. <span data-ttu-id="987f1-170">A hello **közös hozzáférésű Jogosultságkód** párbeszédpanelen adja meg a hello házirend, érvényességének kezdő és záró dátumát, időzóna, és a hozzáférési szintet hello erőforrás használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="987f1-170">In hello **Shared Access Signature** dialog, specify hello policy, start and expiration dates, time zone, and access levels you want for hello resource.</span></span>

   ![SAS-beállítások beolvasása][9]
6. <span data-ttu-id="987f1-172">Ha befejezte a hello SAS-beállítások, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="987f1-172">When you're finished specifying hello SAS options, select **Create**.</span></span>
7. <span data-ttu-id="987f1-173">Egy második **közös hozzáférésű Jogosultságkód** párbeszédpanel ezután jeleníti meg, hogy listák hello blob tároló hello URL-cím mellett, és használhatja a tooaccess QueryStrings hello tárolási erőforrás.</span><span class="sxs-lookup"><span data-stu-id="987f1-173">A second **Shared Access Signature** dialog will then display that lists hello blob container along with hello URL and QueryStrings you can use tooaccess hello storage resource.</span></span>
   <span data-ttu-id="987f1-174">Válassza ki **másolási** toocopy toohello vágólapra kívánja tovább toohello URL.</span><span class="sxs-lookup"><span data-stu-id="987f1-174">Select **Copy** next toohello URL you wish toocopy toohello clipboard.</span></span>

   ![SAS URL-címének másolása][10]
8. <span data-ttu-id="987f1-176">Ha elkészült, válassza a **Bezárás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="987f1-176">When done, select **Close**.</span></span>

## <a name="manage-access-policies-for-a-blob-container"></a><span data-ttu-id="987f1-177">Egy blob tároló a hozzáférési házirendek kezelése</span><span class="sxs-lookup"><span data-stu-id="987f1-177">Manage Access Policies for a blob container</span></span>
<span data-ttu-id="987f1-178">hello következő lépések bemutatják, hogyan toomanage (hozzáadása és eltávolítása) hozzáférési házirendek egy blob-tároló:</span><span class="sxs-lookup"><span data-stu-id="987f1-178">hello following steps illustrate how toomanage (add and remove) access policies for a blob container:</span></span>

1. <span data-ttu-id="987f1-179">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="987f1-179">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="987f1-180">Hello bal oldali ablaktáblán bontsa ki a hello blob tároló, amelynek hozzáférési házirendek toomanage kívánja tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="987f1-180">In hello left pane, expand hello storage account containing hello blob container whose access policies you wish toomanage.</span></span>
3. <span data-ttu-id="987f1-181">Bontsa ki a hello tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="987f1-181">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="987f1-182">Válassza ki a kívánt blobtárolóban hello, és válassza – a hello helyi menüből – **hozzáférési házirendek kezelése**.</span><span class="sxs-lookup"><span data-stu-id="987f1-182">Select hello desired blob container, and - from hello context menu - select **Manage Access Policies**.</span></span>

   ![Hozzáférési szabályzatok kezelése helyi menü][11]
5. <span data-ttu-id="987f1-184">Hello **hozzáférési házirendek** párbeszédpanel felsorolja hello kijelölt blob tároló már létrehozott hozzáférési házirendekben.</span><span class="sxs-lookup"><span data-stu-id="987f1-184">hello **Access Policies** dialog will list any access policies already created for hello selected blob container.</span></span>

   ![Hozzáférési házirend beállítása][12]        
6. <span data-ttu-id="987f1-186">Kövesse az alábbi lépéseket attól függően, hogy hello hozzáférési házirend fájlkezelési feladat:</span><span class="sxs-lookup"><span data-stu-id="987f1-186">Follow these steps depending on hello access policy management task:</span></span>

   * <span data-ttu-id="987f1-187">**Új hozzáférési szabályzat hozzáadása** – Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="987f1-187">**Add a new access policy** - Select **Add**.</span></span> <span data-ttu-id="987f1-188">Létrehozott, miután hello **hozzáférési házirendek** párbeszédpanel megjeleníti az újonnan hozzáadott hello házirendhez (az alapértelmezett beállításokkal).</span><span class="sxs-lookup"><span data-stu-id="987f1-188">Once generated, hello **Access Policies** dialog will display hello newly added access policy (with default settings).</span></span>
   * <span data-ttu-id="987f1-189">**A hozzáférési házirendek szerkesztése** – végezze el a kívánt módosításokat, és válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="987f1-189">**Edit an access policy** -  Make any desired edits, and select **Save**.</span></span>
   * <span data-ttu-id="987f1-190">**Távolítsa el a hozzáférési házirendek** – Itt adhatja meg **eltávolítása** tooremove kívánja tovább toohello házirend.</span><span class="sxs-lookup"><span data-stu-id="987f1-190">**Remove an access policy** - Select **Remove** next toohello access policy you wish tooremove.</span></span>

## <a name="set-hello-public-access-level-for-a-blob-container"></a><span data-ttu-id="987f1-191">A blob-tároló hello nyilvános hozzáférési szint beállítása</span><span class="sxs-lookup"><span data-stu-id="987f1-191">Set hello Public Access Level for a blob container</span></span>
<span data-ttu-id="987f1-192">Alapértelmezés szerint minden blob tároló túl értéke "Nem nyilvános hozzáférés".</span><span class="sxs-lookup"><span data-stu-id="987f1-192">By default, every blob container is set too"No public access".</span></span>

<span data-ttu-id="987f1-193">hello következő lépések bemutatják, hogyan toospecify nyilvános hozzáférési szint egy blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="987f1-193">hello following steps illustrate how toospecify a public access level for a blob container.</span></span>

1. <span data-ttu-id="987f1-194">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="987f1-194">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="987f1-195">Hello bal oldali ablaktáblán bontsa ki a hello blob tároló, amelynek hozzáférési házirendek toomanage kívánja tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="987f1-195">In hello left pane, expand hello storage account containing hello blob container whose access policies you wish toomanage.</span></span>
3. <span data-ttu-id="987f1-196">Bontsa ki a hello tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="987f1-196">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="987f1-197">Válassza ki a kívánt blobtárolóban hello, és válassza – a hello helyi menüből – **nyilvános hozzáférési szint beállítása**.</span><span class="sxs-lookup"><span data-stu-id="987f1-197">Select hello desired blob container, and - from hello context menu - select **Set Public Access Level**.</span></span>

   ![Állítsa be a nyilvános hozzáférés szint a helyi menü][13]
5. <span data-ttu-id="987f1-199">A hello **tároló nyilvános hozzáférési szint beállítása** párbeszédpanelen adja meg a szükséges hello hozzáférési szintet.</span><span class="sxs-lookup"><span data-stu-id="987f1-199">In hello **Set Container Public Access Level** dialog, specify hello desired access level.</span></span>

   ![Nyilvános hozzáférés szint beállításainak megadása][14]
6. <span data-ttu-id="987f1-201">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="987f1-201">Select **Apply**.</span></span>

## <a name="managing-blobs-in-a-blob-container"></a><span data-ttu-id="987f1-202">A blob-tárolóban lévő blobok kezelése</span><span class="sxs-lookup"><span data-stu-id="987f1-202">Managing blobs in a blob container</span></span>
<span data-ttu-id="987f1-203">A blob-tároló létrehozását követően feltöltése a blob toothat blobtárolóban, töltse le a blob tooyour helyi számítógépen, nyissa meg a blob a helyi számítógépen, és még sok más.</span><span class="sxs-lookup"><span data-stu-id="987f1-203">Once you've created a blob container, you can upload a blob toothat blob container, download a blob tooyour local computer, open a blob on your local computer, and much more.</span></span>

<span data-ttu-id="987f1-204">hello lépések bemutatják, hogyan toomanage hello blobok (és mappákat) a blob-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="987f1-204">hello following steps illustrate how toomanage hello blobs (and folders) within a blob container.</span></span>

1. <span data-ttu-id="987f1-205">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="987f1-205">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="987f1-206">Hello bal oldali ablaktáblán bontsa ki a hello blob tároló toomanage kívánja tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="987f1-206">In hello left pane, expand hello storage account containing hello blob container you wish toomanage.</span></span>
3. <span data-ttu-id="987f1-207">Bontsa ki a hello tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="987f1-207">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="987f1-208">Kattintson duplán a hello blob tároló tooview kívánja.</span><span class="sxs-lookup"><span data-stu-id="987f1-208">Double-click hello blob container you wish tooview.</span></span>
5. <span data-ttu-id="987f1-209">hello fő ablaktábla hello blob tároló tartalmának.</span><span class="sxs-lookup"><span data-stu-id="987f1-209">hello main pane will display hello blob container's contents.</span></span>

   ![Nézet blob tároló][3]
6. <span data-ttu-id="987f1-211">hello fő ablaktábla hello blob tároló tartalmának.</span><span class="sxs-lookup"><span data-stu-id="987f1-211">hello main pane will display hello blob container's contents.</span></span>
7. <span data-ttu-id="987f1-212">Kövesse az alábbi lépéseket attól függően, hello feladat kívánja tooperform:</span><span class="sxs-lookup"><span data-stu-id="987f1-212">Follow these steps depending on hello task you wish tooperform:</span></span>

   * <span data-ttu-id="987f1-213">**Fájlok tooa blob tároló feltöltése**</span><span class="sxs-lookup"><span data-stu-id="987f1-213">**Upload files tooa blob container**</span></span>

     1. <span data-ttu-id="987f1-214">Hello fő ablaktáblán eszköztáron válassza **feltöltése**, majd **fájl feltöltése** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="987f1-214">On hello main pane's toolbar, select **Upload**, and then **Upload Files** from hello drop-down menu.</span></span>

        ![Fájl menü feltöltése][15]
     2. <span data-ttu-id="987f1-216">A hello **fájlok feltöltése** párbeszédpanelen jelölje be hello három pont (**...** ) gombra a hello jobb oldalán hello **fájlok** szövegmezőben tooselect hello (oka) t tooupload kívánja.</span><span class="sxs-lookup"><span data-stu-id="987f1-216">In hello **Upload files** dialog, select hello ellipsis (**…**) button on hello right side of hello **Files** text box tooselect hello file(s) you wish tooupload.</span></span>

        ![Töltse fel a fájlok beállítások][16]
     3. <span data-ttu-id="987f1-218">Adja meg a hello típusú **Blob-típusú**.</span><span class="sxs-lookup"><span data-stu-id="987f1-218">Specify hello type of **Blob type**.</span></span> <span data-ttu-id="987f1-219">hello cikk [az Azure Blob storage .NET használatának első lépései](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) hello hello különbségei különféle blob ismerteti.</span><span class="sxs-lookup"><span data-stu-id="987f1-219">hello article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains hello differences between hello various blob types.</span></span>
     4. <span data-ttu-id="987f1-220">Szükség esetén adjon meg egy cél mappát, amelybe hello kijelölt fájlok lesz feltöltve.</span><span class="sxs-lookup"><span data-stu-id="987f1-220">Optionally, specify a target folder into which hello selected file(s) will be uploaded.</span></span> <span data-ttu-id="987f1-221">Ha hello célmappa nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="987f1-221">If hello target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="987f1-222">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="987f1-222">Select **Upload**.</span></span>
   * <span data-ttu-id="987f1-223">**A mappa tooa blobtárolóban feltöltése**</span><span class="sxs-lookup"><span data-stu-id="987f1-223">**Upload a folder tooa blob container**</span></span>

     1. <span data-ttu-id="987f1-224">Hello fő ablaktáblán eszköztáron válassza **feltöltése**, majd **feltöltése mappa** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="987f1-224">On hello main pane's toolbar, select **Upload**, and then **Upload Folder** from hello drop-down menu.</span></span>

        ![Mappa feltöltése menü][17]
     2. <span data-ttu-id="987f1-226">A hello **feltöltési mappa** párbeszédpanelen jelölje be hello három pont (**...** ) gombra a hello jobb oldalán hello **mappa** szöveg mezőben tooselect hello mappa tartalma tooupload kívánja.</span><span class="sxs-lookup"><span data-stu-id="987f1-226">In hello **Upload folder** dialog, select hello ellipsis (**…**) button on hello right side of hello **Folder** text box tooselect hello folder whose contents you wish tooupload.</span></span>

        ![Töltse fel a mappa beállításai][18]
     3. <span data-ttu-id="987f1-228">Adja meg a hello típusú **Blob-típusú**.</span><span class="sxs-lookup"><span data-stu-id="987f1-228">Specify hello type of **Blob type**.</span></span> <span data-ttu-id="987f1-229">hello cikk [az Azure Blob storage .NET használatának első lépései](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) hello hello különbségei különféle blob ismerteti.</span><span class="sxs-lookup"><span data-stu-id="987f1-229">hello article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains hello differences between hello various blob types.</span></span>
     4. <span data-ttu-id="987f1-230">Szükség esetén adja meg, hogy a célmappa, mely hello a kijelölt mappa tartalma lesz feltöltve.</span><span class="sxs-lookup"><span data-stu-id="987f1-230">Optionally, specify a target folder into which hello selected folder's contents will be uploaded.</span></span> <span data-ttu-id="987f1-231">Ha hello célmappa nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="987f1-231">If hello target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="987f1-232">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="987f1-232">Select **Upload**.</span></span>
   * <span data-ttu-id="987f1-233">**Töltse le a blob tooyour helyi számítógépről**</span><span class="sxs-lookup"><span data-stu-id="987f1-233">**Download a blob tooyour local computer**</span></span>

     1. <span data-ttu-id="987f1-234">Válassza ki a hello blob toodownload kívánja.</span><span class="sxs-lookup"><span data-stu-id="987f1-234">Select hello blob you wish toodownload.</span></span>
     2. <span data-ttu-id="987f1-235">Hello fő ablaktáblán eszköztáron válassza **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="987f1-235">On hello main pane's toolbar, select **Download**.</span></span>
     3. <span data-ttu-id="987f1-236">A hello **adja meg, ahol toosave hello letöltött blob** párbeszédpanelen adja meg a letöltött hello blob kívánt hello helyre, és hello toogive kívánja neve azt.</span><span class="sxs-lookup"><span data-stu-id="987f1-236">In hello **Specify where toosave hello downloaded blob** dialog, specify hello location where you want hello blob downloaded, and hello name you wish toogive it.</span></span>  
     4. <span data-ttu-id="987f1-237">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="987f1-237">Select **Save**.</span></span>
   * <span data-ttu-id="987f1-238">**Nyissa meg a helyi számítógépen blob**</span><span class="sxs-lookup"><span data-stu-id="987f1-238">**Open a blob on your local computer**</span></span>

     1. <span data-ttu-id="987f1-239">Válassza ki a hello blob tooopen kívánja.</span><span class="sxs-lookup"><span data-stu-id="987f1-239">Select hello blob you wish tooopen.</span></span>
     2. <span data-ttu-id="987f1-240">Hello fő ablaktáblán eszköztáron válassza **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="987f1-240">On hello main pane's toolbar, select **Open**.</span></span>
     3. <span data-ttu-id="987f1-241">hello blob le lesznek töltve, és hello blob fájl alaptípusának társított hello alkalmazás használatával megnyitni.</span><span class="sxs-lookup"><span data-stu-id="987f1-241">hello blob will be downloaded and opened using hello application associated with hello blob's underlying file type.</span></span>
   * <span data-ttu-id="987f1-242">**A blob toohello vágólapra másolása**</span><span class="sxs-lookup"><span data-stu-id="987f1-242">**Copy a blob toohello clipboard**</span></span>

     1. <span data-ttu-id="987f1-243">Válassza ki a hello blob toocopy kívánja.</span><span class="sxs-lookup"><span data-stu-id="987f1-243">Select hello blob you wish toocopy.</span></span>
     2. <span data-ttu-id="987f1-244">Hello fő ablaktáblán eszköztáron válassza **másolási**.</span><span class="sxs-lookup"><span data-stu-id="987f1-244">On hello main pane's toolbar, select **Copy**.</span></span>
     3. <span data-ttu-id="987f1-245">Hello bal oldali ablaktáblán, keresse meg a tooanother blob tároló, és kattintson rá duplán tooview azt hello fő ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="987f1-245">In hello left pane, navigate tooanother blob container, and double-click it tooview it in hello main pane.</span></span>
     4. <span data-ttu-id="987f1-246">Hello fő ablaktáblán eszköztáron válassza **Beillesztés** toocreate hello blob egy példányát.</span><span class="sxs-lookup"><span data-stu-id="987f1-246">On hello main pane's toolbar, select **Paste** toocreate a copy of hello blob.</span></span>
   * <span data-ttu-id="987f1-247">**Blobok törléséhez**</span><span class="sxs-lookup"><span data-stu-id="987f1-247">**Delete a blob**</span></span>

     1. <span data-ttu-id="987f1-248">Válassza ki a hello blob toodelete kívánja.</span><span class="sxs-lookup"><span data-stu-id="987f1-248">Select hello blob you wish toodelete.</span></span>
     2. <span data-ttu-id="987f1-249">Hello fő ablaktáblán eszköztáron válassza **törlése**.</span><span class="sxs-lookup"><span data-stu-id="987f1-249">On hello main pane's toolbar, select **Delete**.</span></span>
     3. <span data-ttu-id="987f1-250">Válassza ki **Igen** toohello megerősítő párbeszédpanele.</span><span class="sxs-lookup"><span data-stu-id="987f1-250">Select **Yes** toohello confirmation dialog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="987f1-251">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="987f1-251">Next steps</span></span>
* <span data-ttu-id="987f1-252">Nézet hello [legújabb Tártallózó (előzetes verzió) kibocsátási megjegyzések és videók](http://www.storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="987f1-252">View hello [latest Storage Explorer (Preview) release notes and videos](http://www.storageexplorer.com).</span></span>
* <span data-ttu-id="987f1-253">Ismerje meg, hogyan túl[létrehozása az Azure BLOB, táblák, üzenetsorok és fájlokat használó alkalmazások](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="987f1-253">Learn how too[create applications using Azure blobs, tables, queues, and files](https://azure.microsoft.com/documentation/services/storage/).</span></span>

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png
