---
title: "A Tártallózó (előzetes verzió) Azure Blob Storage-erőforrások kezelése |} Microsoft Docs"
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
ms.openlocfilehash: f833027203441e12340bd93f3570de073d297223
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a><span data-ttu-id="2676b-103">A Tártallózó (előzetes verzió) Azure Blob Storage-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="2676b-103">Manage Azure Blob Storage resources with Storage Explorer (Preview)</span></span>
## <a name="overview"></a><span data-ttu-id="2676b-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2676b-104">Overview</span></span>
<span data-ttu-id="2676b-105">[Az Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a világon HTTP vagy HTTPS PROTOKOLLON keresztül tárolásához.</span><span class="sxs-lookup"><span data-stu-id="2676b-105">[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span>
<span data-ttu-id="2676b-106">A Blob Storage segítségével bárki számára nyilvánosan elérhetővé tehet adatokat, vagy privát módon tárolhat alkalmazásadatokat.</span><span class="sxs-lookup"><span data-stu-id="2676b-106">You can use Blob storage to expose data publicly to the world, or to store application data privately.</span></span> <span data-ttu-id="2676b-107">Ebből a cikkből megtudhatja, hogyan Tártallózó (előzetes verzió) történő együttműködésre blob tárolók és blobok használatához.</span><span class="sxs-lookup"><span data-stu-id="2676b-107">In this article, you'll learn how to use Storage Explorer (Preview) to work with blob containers and blobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2676b-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2676b-108">Prerequisites</span></span>
<span data-ttu-id="2676b-109">A cikkben leírt lépések elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="2676b-109">To complete the steps in this article, you'll need the following:</span></span>

* [<span data-ttu-id="2676b-110">A Tártallózó (előzetes verzió) letöltése és telepítése</span><span class="sxs-lookup"><span data-stu-id="2676b-110">Download and install Storage Explorer (preview)</span></span>](http://www.storageexplorer.com)
* [<span data-ttu-id="2676b-111">Csatlakozás egy Azure-tárfiókhoz vagy -szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="2676b-111">Connect to a Azure storage account or service</span></span>](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a><span data-ttu-id="2676b-112">A blob-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="2676b-112">Create a blob container</span></span>
<span data-ttu-id="2676b-113">Minden BLOB egy blob tároló, amely egyszerűen blobok logikai csoportosítása kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2676b-113">All blobs must reside in a blob container, which is simply a logical grouping of blobs.</span></span> <span data-ttu-id="2676b-114">Egy fiók korlátlan számú tárolót tartalmazhat, és minden egyes tároló korlátlan számú BLOB tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="2676b-114">An account can contain an unlimited number of containers, and each container can store an unlimited number of blobs.</span></span>

<span data-ttu-id="2676b-115">A következő lépések bemutatják, hogyan lehet a Tártallózó (előzetes verzió) belül blob tárolókat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="2676b-115">The following steps illustrate how to create a blob container within Storage Explorer (Preview).</span></span>

1. <span data-ttu-id="2676b-116">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="2676b-116">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2676b-117">A bal oldali ablaktáblán bontsa ki a tárfiók, amelyen belül a blob-tároló létrehozása kívánja.</span><span class="sxs-lookup"><span data-stu-id="2676b-117">In the left pane, expand the storage account within which you wish to create the blob container.</span></span>
3. <span data-ttu-id="2676b-118">Kattintson a jobb gombbal **Blobtárolók**, és válassza – a helyi menüből – a **Blob-tároló létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="2676b-118">Right-click **Blob Containers**, and - from the context menu - select **Create Blob Container**.</span></span>

   ![Blob tárolók a helyi menü létrehozása][0]
4. <span data-ttu-id="2676b-120">Szövegmező alatt megjelenik a **Blobtárolók** mappa.</span><span class="sxs-lookup"><span data-stu-id="2676b-120">A text box will appear below the **Blob Containers** folder.</span></span> <span data-ttu-id="2676b-121">Adja meg a blob-tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="2676b-121">Enter the name for your blob container.</span></span> <span data-ttu-id="2676b-122">Tekintse meg a [tároló elnevezési szabályait](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) listáját szakasza szabályok és a blob tárolók elnevezési korlátozás.</span><span class="sxs-lookup"><span data-stu-id="2676b-122">See the [Container naming rules](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) section for a list of rules and restrictions on naming blob containers.</span></span>

   ![Szövegmező Blob tárolók létrehozása][1]
5. <span data-ttu-id="2676b-124">Nyomja le az **Enter** végzett a blob-tároló létrehozásához vagy **Esc** megszakítja a műveletet.</span><span class="sxs-lookup"><span data-stu-id="2676b-124">Press **Enter** when done to create the blob container, or **Esc** to cancel.</span></span> <span data-ttu-id="2676b-125">A blob-tároló sikeres létrehozását követően megjelenik a a **Blobtárolók** a kiválasztott tárolási fiók mappáját.</span><span class="sxs-lookup"><span data-stu-id="2676b-125">Once the blob container has been successfully created, it will be displayed under the **Blob Containers** folder for the selected storage account.</span></span>

   ![BLOB-tároló létrehozása][2]

## <a name="view-a-blob-containers-contents"></a><span data-ttu-id="2676b-127">A blob-tároló tartalmának megtekintése</span><span class="sxs-lookup"><span data-stu-id="2676b-127">View a blob container's contents</span></span>
<span data-ttu-id="2676b-128">BLOB tárolók blobok és mappák (BLOB is tartalmazhat) tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="2676b-128">Blob containers contain blobs and folders (that can also contain blobs).</span></span>

<span data-ttu-id="2676b-129">A következő lépések bemutatják a Tártallózó (előzetes verzió) belül egy blob tároló tartalmának megtekintése:</span><span class="sxs-lookup"><span data-stu-id="2676b-129">The following steps illustrate how to view the contents of a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="2676b-130">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="2676b-130">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2676b-131">A bal oldali ablaktáblán bontsa ki a tárfiók a blob-tároló tartalmazó meg szeretné tekinteni.</span><span class="sxs-lookup"><span data-stu-id="2676b-131">In the left pane, expand the storage account containing the blob container you wish to view.</span></span>
3. <span data-ttu-id="2676b-132">Bontsa ki a tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="2676b-132">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2676b-133">Kattintson a jobb gombbal a blob-tároló kíván megtekinteni, és válassza – a helyi menüből – **Blob tároló szerkesztő megnyitása**.</span><span class="sxs-lookup"><span data-stu-id="2676b-133">Right-click the blob container you wish to view, and - from the context menu - select **Open Blob Container Editor**.</span></span>
   <span data-ttu-id="2676b-134">Kattintson duplán a megtekinteni kívánt blobtárolóban is.</span><span class="sxs-lookup"><span data-stu-id="2676b-134">You can also double-click the blob container you wish to view.</span></span>

   ![Nyissa meg a blob tároló szerkesztő helyi menü][19]
5. <span data-ttu-id="2676b-136">A fő ablaktáblán jelenik meg a blob-tároló tartalmának.</span><span class="sxs-lookup"><span data-stu-id="2676b-136">The main pane will display the blob container's contents.</span></span>

   ![A BLOB-tároló szerkesztő][3]

## <a name="delete-a-blob-container"></a><span data-ttu-id="2676b-138">A blob-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="2676b-138">Delete a blob container</span></span>
<span data-ttu-id="2676b-139">BLOB tárolók egyszerűen hozható létre és igény szerint törölve.</span><span class="sxs-lookup"><span data-stu-id="2676b-139">Blob containers can be easily created and deleted as needed.</span></span> <span data-ttu-id="2676b-140">(Egyes blobok törlése, tekintse át a részt történő [kezelése a blob-tárolóban lévő blobok](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="2676b-140">(To see how to delete individual blobs, refer to the section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="2676b-141">A következő lépések bemutatják, hogyan lehet a Tártallózó (előzetes verzió) belül egy blob-tároló törlése:</span><span class="sxs-lookup"><span data-stu-id="2676b-141">The following steps illustrate how to delete a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="2676b-142">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="2676b-142">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2676b-143">A bal oldali ablaktáblán bontsa ki a tárfiók a blob-tároló tartalmazó meg szeretné tekinteni.</span><span class="sxs-lookup"><span data-stu-id="2676b-143">In the left pane, expand the storage account containing the blob container you wish to view.</span></span>
3. <span data-ttu-id="2676b-144">Bontsa ki a tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="2676b-144">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2676b-145">Kattintson a jobb gombbal a blob-tároló törlése, és válassza – a helyi menüből – kíván **törlése**.</span><span class="sxs-lookup"><span data-stu-id="2676b-145">Right-click the blob container you wish to delete, and - from the context menu - select **Delete**.</span></span>
   <span data-ttu-id="2676b-146">Is **törlése** a jelenleg kijelölt blob-tároló törlése.</span><span class="sxs-lookup"><span data-stu-id="2676b-146">You can also press **Delete** to delete the currently selected blob container.</span></span>

   ![A blob tároló helyi menü törlése][4]
5. <span data-ttu-id="2676b-148">Válassza az **Igen** lehetőséget a megerősítési párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="2676b-148">Select **Yes** to the confirmation dialog.</span></span>

   ![A blob tároló megerősítő törlése][5]

## <a name="copy-a-blob-container"></a><span data-ttu-id="2676b-150">Másolja a blob-tároló</span><span class="sxs-lookup"><span data-stu-id="2676b-150">Copy a blob container</span></span>
<span data-ttu-id="2676b-151">A Tártallózó (előzetes verzió) lehetővé teszi egy blob-tároló másolása a vágólapra, majd, hogy a blob-tároló illessze be egy másik tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="2676b-151">Storage Explorer (Preview) enables you to copy a blob container to the clipboard, and then paste that blob container into another storage account.</span></span> <span data-ttu-id="2676b-152">(Egyes blobot másolni, tekintse át a részt, hogyan [kezelése a blob-tárolóban lévő blobok](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="2676b-152">(To see how to copy individual blobs, refer to the section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="2676b-153">A következő lépések bemutatják, hogyan lehet egy blob-tároló másolhat egy tárfiók egy másikra.</span><span class="sxs-lookup"><span data-stu-id="2676b-153">The following steps illustrate how to copy a blob container from one storage account to another.</span></span>

1. <span data-ttu-id="2676b-154">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="2676b-154">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2676b-155">A bal oldali ablaktáblán bontsa ki a tárfiók a blob-tároló tartalmazó másolni kívánja.</span><span class="sxs-lookup"><span data-stu-id="2676b-155">In the left pane, expand the storage account containing the blob container you wish to copy.</span></span>
3. <span data-ttu-id="2676b-156">Bontsa ki a tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="2676b-156">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2676b-157">Kattintson a jobb gombbal a blob-tároló kívánja másolni, és válassza – a helyi menüből – **másolási Blob tároló**.</span><span class="sxs-lookup"><span data-stu-id="2676b-157">Right-click the blob container you wish to copy, and - from the context menu - select **Copy Blob Container**.</span></span>

   ![Másolja a blob tároló helyi menü][6]
5. <span data-ttu-id="2676b-159">Kattintson a jobb gombbal a kívánt "target" tárfiók, amelybe a illessze be a blob-tároló, és válassza – a helyi menüből – szeretne **illessze be a Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="2676b-159">Right-click the desired "target" storage account into which you want to paste the blob container, and - from the context menu - select **Paste Blob Container**.</span></span>

   ![Beillesztés blob tároló helyi menü][7]

## <a name="get-the-sas-for-a-blob-container"></a><span data-ttu-id="2676b-161">SAS lekérése blob tárolóhoz</span><span class="sxs-lookup"><span data-stu-id="2676b-161">Get the SAS for a blob container</span></span>
<span data-ttu-id="2676b-162">A [közös hozzáférésű jogosultságkód (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) delegált hozzáférést biztosít a tárfiókon lévő erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="2676b-162">A [shared access signature (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) provides delegated access to resources in your storage account.</span></span>
<span data-ttu-id="2676b-163">Ez azt jelenti, hogy egy adott időszakra megadhatja az ügyfeleknek a tárfiókban lévő objektumokra vonatkozó engedélyek bizonyos készletét a tár hozzáférési kulcsainak megosztása nélkül.</span><span class="sxs-lookup"><span data-stu-id="2676b-163">This means that you can grant a client limited permissions to objects in your storage account for a specified period of time and with a specified set of permissions, without having to share your account access keys.</span></span>

<span data-ttu-id="2676b-164">A következő lépések bemutatják egy SAS-t egy blob-tároló létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2676b-164">The following steps illustrate how to create a SAS for a blob container:</span></span>

1. <span data-ttu-id="2676b-165">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="2676b-165">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2676b-166">A bal oldali ablaktáblán bontsa ki a tárfiók a blob tároló, amely a SAS-kód lekérése kíván tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="2676b-166">In the left pane, expand the storage account containing the blob container for which you wish to get a SAS.</span></span>
3. <span data-ttu-id="2676b-167">Bontsa ki a tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="2676b-167">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2676b-168">Kattintson a jobb gombbal a kívánt blob tároló, és válassza – a helyi menüből – **közös hozzáférésű Jogosultságkód beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="2676b-168">Right-click the desired blob container, and - from the context menu - select **Get Shared Access Signature**.</span></span>

   ![A helyi menü SAS lekérése][8]
5. <span data-ttu-id="2676b-170">A **Közös hozzáférésű jogosultságkód** párbeszédpanelen adja meg a szabályzatot, a kezdési és a lejárati dátumokat, az időzónát és az erőforrás kívánt hozzáférési szintjeit.</span><span class="sxs-lookup"><span data-stu-id="2676b-170">In the **Shared Access Signature** dialog, specify the policy, start and expiration dates, time zone, and access levels you want for the resource.</span></span>

   ![SAS-beállítások beolvasása][9]
6. <span data-ttu-id="2676b-172">Az SAS-beállítások megadása után válassza a **Létrehozás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2676b-172">When you're finished specifying the SAS options, select **Create**.</span></span>
7. <span data-ttu-id="2676b-173">Egy második **közös hozzáférésű Jogosultságkód** párbeszédpanel fogja tartalmazni, amely tartalmazza a blob-tároló URL-cím és a tárolási erőforrások eléréséhez használhatja QueryStrings együtt.</span><span class="sxs-lookup"><span data-stu-id="2676b-173">A second **Shared Access Signature** dialog will then display that lists the blob container along with the URL and QueryStrings you can use to access the storage resource.</span></span>
   <span data-ttu-id="2676b-174">Válassza a **Másolás** parancsot a vágólapra másolni kívánt URL-cím mellett.</span><span class="sxs-lookup"><span data-stu-id="2676b-174">Select **Copy** next to the URL you wish to copy to the clipboard.</span></span>

   ![SAS URL-címének másolása][10]
8. <span data-ttu-id="2676b-176">Ha elkészült, válassza a **Bezárás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2676b-176">When done, select **Close**.</span></span>

## <a name="manage-access-policies-for-a-blob-container"></a><span data-ttu-id="2676b-177">Egy blob tároló a hozzáférési házirendek kezelése</span><span class="sxs-lookup"><span data-stu-id="2676b-177">Manage Access Policies for a blob container</span></span>
<span data-ttu-id="2676b-178">A következő lépések bemutatják, hogyan kezelheti (hozzáadása és eltávolítása) hozzáférési házirendek egy blob-tároló:</span><span class="sxs-lookup"><span data-stu-id="2676b-178">The following steps illustrate how to manage (add and remove) access policies for a blob container:</span></span>

1. <span data-ttu-id="2676b-179">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="2676b-179">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2676b-180">A bal oldali ablaktáblán bontsa ki a tárfiók a blob tároló, amelynek hozzáférési házirendeket, kezelni akarja tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="2676b-180">In the left pane, expand the storage account containing the blob container whose access policies you wish to manage.</span></span>
3. <span data-ttu-id="2676b-181">Bontsa ki a tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="2676b-181">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2676b-182">Válassza ki a kívánt blob tároló, és válassza – a helyi menüből – **hozzáférési házirendek kezelése**.</span><span class="sxs-lookup"><span data-stu-id="2676b-182">Select the desired blob container, and - from the context menu - select **Manage Access Policies**.</span></span>

   ![Hozzáférési szabályzatok kezelése helyi menü][11]
5. <span data-ttu-id="2676b-184">A **hozzáférési házirendek** párbeszédpanel felsorolja a kijelölt blob-tároló már létre hozzáférési házirendekben.</span><span class="sxs-lookup"><span data-stu-id="2676b-184">The **Access Policies** dialog will list any access policies already created for the selected blob container.</span></span>

   ![Hozzáférési házirend beállítása][12]        
6. <span data-ttu-id="2676b-186">A hozzáférésiszabályzat-kezelési feladattól függően kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2676b-186">Follow these steps depending on the access policy management task:</span></span>

   * <span data-ttu-id="2676b-187">**Új hozzáférési szabályzat hozzáadása** – Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2676b-187">**Add a new access policy** - Select **Add**.</span></span> <span data-ttu-id="2676b-188">A **Hozzáférési szabályzatok** megjeleníti az újonnan létrehozott és hozzáadott hozzáférési szabályzatot (az alapértelmezett beállításokkal).</span><span class="sxs-lookup"><span data-stu-id="2676b-188">Once generated, the **Access Policies** dialog will display the newly added access policy (with default settings).</span></span>
   * <span data-ttu-id="2676b-189">**A hozzáférési házirendek szerkesztése** – végezze el a kívánt módosításokat, és válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2676b-189">**Edit an access policy** -  Make any desired edits, and select **Save**.</span></span>
   * <span data-ttu-id="2676b-190">**Hozzáférési szabályzat eltávolítása** – Válassza az **Eltávolítás** parancsot az eltávolítani kívánt hozzáférési szabályzat mellett.</span><span class="sxs-lookup"><span data-stu-id="2676b-190">**Remove an access policy** - Select **Remove** next to the access policy you wish to remove.</span></span>

## <a name="set-the-public-access-level-for-a-blob-container"></a><span data-ttu-id="2676b-191">A nyilvános hozzáférési szint beállítása a blob-tároló</span><span class="sxs-lookup"><span data-stu-id="2676b-191">Set the Public Access Level for a blob container</span></span>
<span data-ttu-id="2676b-192">Alapértelmezés szerint minden blob tároló "Nem nyilvános hozzáférés" értéke.</span><span class="sxs-lookup"><span data-stu-id="2676b-192">By default, every blob container is set to "No public access".</span></span>

<span data-ttu-id="2676b-193">A következő lépések bemutatják, hogyan lehet egy blob-tároló nyilvános hozzáférés szintet adjon meg.</span><span class="sxs-lookup"><span data-stu-id="2676b-193">The following steps illustrate how to specify a public access level for a blob container.</span></span>

1. <span data-ttu-id="2676b-194">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="2676b-194">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2676b-195">A bal oldali ablaktáblán bontsa ki a tárfiók a blob tároló, amelynek hozzáférési házirendeket, kezelni akarja tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="2676b-195">In the left pane, expand the storage account containing the blob container whose access policies you wish to manage.</span></span>
3. <span data-ttu-id="2676b-196">Bontsa ki a tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="2676b-196">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2676b-197">Válassza ki a kívánt blob tároló, és válassza – a helyi menüből – **nyilvános hozzáférési szint beállítása**.</span><span class="sxs-lookup"><span data-stu-id="2676b-197">Select the desired blob container, and - from the context menu - select **Set Public Access Level**.</span></span>

   ![Állítsa be a nyilvános hozzáférés szint a helyi menü][13]
5. <span data-ttu-id="2676b-199">Az a **tároló nyilvános hozzáférési szint beállítása** párbeszédpanelen adja meg a kívánt hozzáférési szintjét.</span><span class="sxs-lookup"><span data-stu-id="2676b-199">In the **Set Container Public Access Level** dialog, specify the desired access level.</span></span>

   ![Nyilvános hozzáférés szint beállításainak megadása][14]
6. <span data-ttu-id="2676b-201">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="2676b-201">Select **Apply**.</span></span>

## <a name="managing-blobs-in-a-blob-container"></a><span data-ttu-id="2676b-202">A blob-tárolóban lévő blobok kezelése</span><span class="sxs-lookup"><span data-stu-id="2676b-202">Managing blobs in a blob container</span></span>
<span data-ttu-id="2676b-203">Egy blob-tároló létrehozását követően egy blob feltöltése a blob-tárolóhoz, blob letöltése a helyi számítógépen, nyissa meg a blob a helyi számítógépen, és még sok más.</span><span class="sxs-lookup"><span data-stu-id="2676b-203">Once you've created a blob container, you can upload a blob to that blob container, download a blob to your local computer, open a blob on your local computer, and much more.</span></span>

<span data-ttu-id="2676b-204">A következő lépések bemutatják a blobok (és a mappák) kezelése a blob-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="2676b-204">The following steps illustrate how to manage the blobs (and folders) within a blob container.</span></span>

1. <span data-ttu-id="2676b-205">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="2676b-205">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2676b-206">A bal oldali ablaktáblán bontsa ki a tárfiók a blob-tároló tartalmazó felügyelni kíván.</span><span class="sxs-lookup"><span data-stu-id="2676b-206">In the left pane, expand the storage account containing the blob container you wish to manage.</span></span>
3. <span data-ttu-id="2676b-207">Bontsa ki a tárfiók **Blobtárolók**.</span><span class="sxs-lookup"><span data-stu-id="2676b-207">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2676b-208">Kattintson duplán a blob-tároló meg szeretné tekinteni.</span><span class="sxs-lookup"><span data-stu-id="2676b-208">Double-click the blob container you wish to view.</span></span>
5. <span data-ttu-id="2676b-209">A fő ablaktáblán jelenik meg a blob-tároló tartalmának.</span><span class="sxs-lookup"><span data-stu-id="2676b-209">The main pane will display the blob container's contents.</span></span>

   ![Nézet blob tároló][3]
6. <span data-ttu-id="2676b-211">A fő ablaktáblán jelenik meg a blob-tároló tartalmának.</span><span class="sxs-lookup"><span data-stu-id="2676b-211">The main pane will display the blob container's contents.</span></span>
7. <span data-ttu-id="2676b-212">Kövesse az alábbi lépéseket a végrehajtani kívánt feladattól függően:</span><span class="sxs-lookup"><span data-stu-id="2676b-212">Follow these steps depending on the task you wish to perform:</span></span>

   * <span data-ttu-id="2676b-213">**Fájlok feltöltése a blob-tároló**</span><span class="sxs-lookup"><span data-stu-id="2676b-213">**Upload files to a blob container**</span></span>

     1. <span data-ttu-id="2676b-214">A fő ablaktábla eszköztárán válassza a **Feltöltés**, majd a legördülő menüből a **Fájlok feltöltése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2676b-214">On the main pane's toolbar, select **Upload**, and then **Upload Files** from the drop-down menu.</span></span>

        ![Fájl menü feltöltése][15]
     2. <span data-ttu-id="2676b-216">A **Fájlok feltöltése** párbeszédpanelen válassza a **Fájlok** szövegbeviteli mező jobb oldalán lévő, három pontot (**…**) ábrázoló gombot a feltölteni kívánt fájl(ok) kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="2676b-216">In the **Upload files** dialog, select the ellipsis (**…**) button on the right side of the **Files** text box to select the file(s) you wish to upload.</span></span>

        ![Töltse fel a fájlok beállítások][16]
     3. <span data-ttu-id="2676b-218">Adja meg, milyen típusú **Blob-típusú**.</span><span class="sxs-lookup"><span data-stu-id="2676b-218">Specify the type of **Blob type**.</span></span> <span data-ttu-id="2676b-219">A cikk [az Azure Blob storage .NET használatának első lépései](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) a blob különféle közötti különbségeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="2676b-219">The article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains the differences between the various blob types.</span></span>
     4. <span data-ttu-id="2676b-220">Szükség esetén adja meg, hogy a célmappa, amelybe a kijelölt fájlok lesz feltöltve.</span><span class="sxs-lookup"><span data-stu-id="2676b-220">Optionally, specify a target folder into which the selected file(s) will be uploaded.</span></span> <span data-ttu-id="2676b-221">Ha a célmappa nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="2676b-221">If the target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="2676b-222">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2676b-222">Select **Upload**.</span></span>
   * <span data-ttu-id="2676b-223">**Töltse fel egy mappát egy blob-tárolóba**</span><span class="sxs-lookup"><span data-stu-id="2676b-223">**Upload a folder to a blob container**</span></span>

     1. <span data-ttu-id="2676b-224">A fő ablaktábla eszköztárán válassza a **Feltöltés**, majd a legördülő menüből a **Mappa feltöltése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2676b-224">On the main pane's toolbar, select **Upload**, and then **Upload Folder** from the drop-down menu.</span></span>

        ![Mappa feltöltése menü][17]
     2. <span data-ttu-id="2676b-226">A **Mappa feltöltése** párbeszédpanelen a **Mappa** szövegbeviteli mező jobb oldalán lévő, három pontot (**…**) ábrázoló gombbal válassza ki a mappát, amelynek a tartalmát fel kívánja tölteni.</span><span class="sxs-lookup"><span data-stu-id="2676b-226">In the **Upload folder** dialog, select the ellipsis (**…**) button on the right side of the **Folder** text box to select the folder whose contents you wish to upload.</span></span>

        ![Töltse fel a mappa beállításai][18]
     3. <span data-ttu-id="2676b-228">Adja meg, milyen típusú **Blob-típusú**.</span><span class="sxs-lookup"><span data-stu-id="2676b-228">Specify the type of **Blob type**.</span></span> <span data-ttu-id="2676b-229">A cikk [az Azure Blob storage .NET használatának első lépései](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) a blob különféle közötti különbségeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="2676b-229">The article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains the differences between the various blob types.</span></span>
     4. <span data-ttu-id="2676b-230">Igény szerint megadhat egy célmappát, amelybe a kiválasztott mappa tartalma fel lesz töltve.</span><span class="sxs-lookup"><span data-stu-id="2676b-230">Optionally, specify a target folder into which the selected folder's contents will be uploaded.</span></span> <span data-ttu-id="2676b-231">Ha a célmappa nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="2676b-231">If the target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="2676b-232">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2676b-232">Select **Upload**.</span></span>
   * <span data-ttu-id="2676b-233">**Egy blob letöltése a helyi számítógépen**</span><span class="sxs-lookup"><span data-stu-id="2676b-233">**Download a blob to your local computer**</span></span>

     1. <span data-ttu-id="2676b-234">Válassza ki a letölteni kívánt blob.</span><span class="sxs-lookup"><span data-stu-id="2676b-234">Select the blob you wish to download.</span></span>
     2. <span data-ttu-id="2676b-235">A fő ablaktábla eszköztárán válassza a **Letöltés** elemet.</span><span class="sxs-lookup"><span data-stu-id="2676b-235">On the main pane's toolbar, select **Download**.</span></span>
     3. <span data-ttu-id="2676b-236">Az a **hová szeretné menteni a letöltött blob** párbeszédpanelen adja meg a helyét, a letöltött blob, és adjon neki kívánt nevét.</span><span class="sxs-lookup"><span data-stu-id="2676b-236">In the **Specify where to save the downloaded blob** dialog, specify the location where you want the blob downloaded, and the name you wish to give it.</span></span>  
     4. <span data-ttu-id="2676b-237">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="2676b-237">Select **Save**.</span></span>
   * <span data-ttu-id="2676b-238">**Nyissa meg a helyi számítógépen blob**</span><span class="sxs-lookup"><span data-stu-id="2676b-238">**Open a blob on your local computer**</span></span>

     1. <span data-ttu-id="2676b-239">Válassza ki a blob, nyissa meg a kívánt.</span><span class="sxs-lookup"><span data-stu-id="2676b-239">Select the blob you wish to open.</span></span>
     2. <span data-ttu-id="2676b-240">A fő ablaktábla eszköztárán válassza a **Megnyitás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2676b-240">On the main pane's toolbar, select **Open**.</span></span>
     3. <span data-ttu-id="2676b-241">A blob le lesznek töltve, és a blob fájl alaptípusának társított alkalmazás használatával megnyitni.</span><span class="sxs-lookup"><span data-stu-id="2676b-241">The blob will be downloaded and opened using the application associated with the blob's underlying file type.</span></span>
   * <span data-ttu-id="2676b-242">**A blob másolása a vágólapra**</span><span class="sxs-lookup"><span data-stu-id="2676b-242">**Copy a blob to the clipboard**</span></span>

     1. <span data-ttu-id="2676b-243">Válassza ki a másolni kívánt blob.</span><span class="sxs-lookup"><span data-stu-id="2676b-243">Select the blob you wish to copy.</span></span>
     2. <span data-ttu-id="2676b-244">A fő ablaktábla eszköztárán válassza a **Másolás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2676b-244">On the main pane's toolbar, select **Copy**.</span></span>
     3. <span data-ttu-id="2676b-245">A bal oldali ablaktáblán keresse meg egy másik blob-tároló, és azt a fő ablaktáblán megtekintéséhez kattintson rá duplán.</span><span class="sxs-lookup"><span data-stu-id="2676b-245">In the left pane, navigate to another blob container, and double-click it to view it in the main pane.</span></span>
     4. <span data-ttu-id="2676b-246">Válassza a fő ablaktáblán eszköztár **Beillesztés** a blob másolatának létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="2676b-246">On the main pane's toolbar, select **Paste** to create a copy of the blob.</span></span>
   * <span data-ttu-id="2676b-247">**Blobok törléséhez**</span><span class="sxs-lookup"><span data-stu-id="2676b-247">**Delete a blob**</span></span>

     1. <span data-ttu-id="2676b-248">Válassza ki a törölni kívánt blob.</span><span class="sxs-lookup"><span data-stu-id="2676b-248">Select the blob you wish to delete.</span></span>
     2. <span data-ttu-id="2676b-249">A fő ablaktábla eszköztárán válassza a **Törlés** parancsot.</span><span class="sxs-lookup"><span data-stu-id="2676b-249">On the main pane's toolbar, select **Delete**.</span></span>
     3. <span data-ttu-id="2676b-250">Válassza az **Igen** lehetőséget a megerősítési párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="2676b-250">Select **Yes** to the confirmation dialog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2676b-251">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2676b-251">Next steps</span></span>
* <span data-ttu-id="2676b-252">A [Storage Explorer (előzetes verzió) legújabb kibocsátási megjegyzéseinek és videóinak megtekintése](http://www.storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="2676b-252">View the [latest Storage Explorer (Preview) release notes and videos](http://www.storageexplorer.com).</span></span>
* <span data-ttu-id="2676b-253">Annak megismerése, hogyan [hozhat létre alkalmazásokat Azure-blobok, -táblák, -üzenetsorok és -fájlok használatával](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="2676b-253">Learn how to [create applications using Azure blobs, tables, queues, and files](https://azure.microsoft.com/documentation/services/storage/).</span></span>

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
