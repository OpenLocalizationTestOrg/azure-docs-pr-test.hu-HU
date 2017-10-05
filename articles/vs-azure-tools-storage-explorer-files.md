---
title: "A Storage Explorer (előzetes verzió) használata az Azure File Storage szolgáltatással | Microsoft Docs"
description: "Ismerje meg, hogyan használhatja a fájlmegosztásokat és fájlokat a Storage Explorer előzetes verziójában."
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/09/2017
ms.author: cawa
ms.openlocfilehash: 964691758254531cb92a5b1cbe055ef61d25dba8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-storage-explorer-preview-with-azure-file-storage"></a><span data-ttu-id="d9727-103">A Storage Explorer (előzetes verzió) használata az Azure File Storage szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="d9727-103">Using Storage Explorer (Preview) with Azure File storage</span></span>

<span data-ttu-id="d9727-104">Az Azure File storage egy felhőalapú fájlmegosztást kínáló, SMB protokollt használó szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d9727-104">Azure File storage is a service that offers file shares in the cloud using the standard Server Message Block (SMB) Protocol.</span></span> <span data-ttu-id="d9727-105">Az SMB 2.1 és az SMB 3.0 protokollt is támogatja.</span><span class="sxs-lookup"><span data-stu-id="d9727-105">Both SMB 2.1 and SMB 3.0 are supported.</span></span> <span data-ttu-id="d9727-106">Az Azure File Storage szolgáltatással költséges újraírások nélkül, gyorsan megoldható a fájlmegosztásra támaszkodó, régi típusú alkalmazások áttelepítése az Azure-ra.</span><span class="sxs-lookup"><span data-stu-id="d9727-106">With Azure File storage, you can migrate legacy applications that rely on file shares to Azure quickly and without costly rewrites.</span></span> <span data-ttu-id="d9727-107">A Blob Storage segítségével bárki számára nyilvánosan elérhetővé tehet adatokat, vagy privát módon tárolhat alkalmazásadatokat.</span><span class="sxs-lookup"><span data-stu-id="d9727-107">You can use File storage to expose data publicly to the world, or to store application data privately.</span></span> <span data-ttu-id="d9727-108">Ez a cikk ismerteti, hogyan használhatja a fájlmegosztásokat és fájlokat a Storage Explorer előzetes verziójában.</span><span class="sxs-lookup"><span data-stu-id="d9727-108">In this article, you'll learn how to use Storage Explorer (Preview) to work with file shares and files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9727-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d9727-109">Prerequisites</span></span>

<span data-ttu-id="d9727-110">A cikkben leírt lépések elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="d9727-110">To complete the steps in this article, you'll need the following:</span></span>

- [<span data-ttu-id="d9727-111">A Tártallózó (előzetes verzió) letöltése és telepítése</span><span class="sxs-lookup"><span data-stu-id="d9727-111">Download and install Storage Explorer (preview)</span></span>](http://www.storageexplorer.com/)

- [<span data-ttu-id="d9727-112">Csatlakozás egy Azure-tárfiókhoz vagy -szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="d9727-112">Connect to a Azure storage account or service</span></span>](https://docs.microsoft.com//azure/vs-azure-tools-storage-manage-with-storage-explorer#connect-to-a-storage-account-or-service)

## <a name="create-a-file-share"></a><span data-ttu-id="d9727-113">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9727-113">Create a File Share</span></span>

<span data-ttu-id="d9727-114">Minden fájlnak fájlmegosztásban kell lennie, amely egyszerűen egy logikai fájlcsoportot jelent.</span><span class="sxs-lookup"><span data-stu-id="d9727-114">All files must reside in a file share, which is simply a logical grouping of files.</span></span> <span data-ttu-id="d9727-115">Egy fiók korlátlan számú fájlmegosztást tartalmazhat, egy adott megosztás pedig korlátlan számú fájl tárolására használható.</span><span class="sxs-lookup"><span data-stu-id="d9727-115">An account can contain an unlimited number of file shares, and each share can store an unlimited number of files.</span></span>

<span data-ttu-id="d9727-116">A következő lépések bemutatják, hogyan hozhat létre fájlmegosztást a Storage Explorer előzetes verziójában.</span><span class="sxs-lookup"><span data-stu-id="d9727-116">The following steps illustrate how to create a file share within Storage Explorer (Preview).</span></span>

1. <span data-ttu-id="d9727-117">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="d9727-117">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="d9727-118">A bal oldali ablaktáblán bontsa ki a tárfiókot, ahol létre kívánja hozni a fájlmegosztást</span><span class="sxs-lookup"><span data-stu-id="d9727-118">In the left pane, expand the storage account within which you wish to create the File Share</span></span>

3. <span data-ttu-id="d9727-119">Kattintson a jobb gombbal a **Fájlmegosztások** elemre, majd a helyi menüben válassza a **Fájlmegosztás létrehozása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-119">Right-click **File Shares**, and - from the context menu - select **Create File Share**.</span></span>

    ![Fájlmegosztás létrehozása](media/vs-azure-tools-storage-explorer-files/image1.png)

4. <span data-ttu-id="d9727-121">A **Fájlmegosztások** mappa alatt megjelenik egy szövegbeviteli mező.</span><span class="sxs-lookup"><span data-stu-id="d9727-121">A text box will appear below the **File Shares** folder.</span></span> <span data-ttu-id="d9727-122">Adja meg a fájlmegosztás nevét.</span><span class="sxs-lookup"><span data-stu-id="d9727-122">Enter the name for your file share.</span></span> <span data-ttu-id="d9727-123">A fájlmegosztások elnevezésére vonatkozó szabályokat és korlátozásokat a [Megosztáselnevezési szabályok](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) című szakaszban olvashatja el.</span><span class="sxs-lookup"><span data-stu-id="d9727-123">See the [Share naming rules](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) section for a list of rules and restrictions on naming file shares.</span></span>

    ![A megosztás elnevezése](media/vs-azure-tools-storage-explorer-files/image2.png)

5. <span data-ttu-id="d9727-125">A név megadása után nyomja le az **Enter** billentyűt az új fájlmegosztás létrehozásához, vagy az **Esc** billentyűt a művelet megszakításához.</span><span class="sxs-lookup"><span data-stu-id="d9727-125">Press **Enter** when done to create the file share, or **Esc** to cancel.</span></span> <span data-ttu-id="d9727-126">A sikeresen létrehozott fájlmegosztás megjelenik a kiválasztott tárfiókhoz tartozó **Fájlmegosztások** mappában.</span><span class="sxs-lookup"><span data-stu-id="d9727-126">Once the file share has been successfully created, it will be displayed under the **File Shares** folder for the selected storage account.</span></span>

    ![Az új megosztás](media/vs-azure-tools-storage-explorer-files/image3.png)

## <a name="view-a-file-shares-contents"></a><span data-ttu-id="d9727-128">Fájlmegosztás tartalmának megtekintése</span><span class="sxs-lookup"><span data-stu-id="d9727-128">View a file share's contents</span></span>

<span data-ttu-id="d9727-129">A fájlmegosztások fájlokat és mappákat tartalmaznak (a mappákban pedig további fájlok lehetnek).</span><span class="sxs-lookup"><span data-stu-id="d9727-129">File shares contain files and folders (that can also contain files).</span></span>

<span data-ttu-id="d9727-130">A következő lépések bemutatják, hogyan tekinthető meg egy fájlmegosztás tartalma a Storage Explorer előzetes verziójában:+</span><span class="sxs-lookup"><span data-stu-id="d9727-130">The following steps illustrate how to view the contents of a file share within Storage Explorer (Preview):+</span></span>

1. <span data-ttu-id="d9727-131">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="d9727-131">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="d9727-132">A bal oldali ablaktáblán bontsa ki a megtekinteni kívánt fájlmegosztást tartalmazó tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="d9727-132">In the left pane, expand the storage account containing the file share you wish to view.</span></span>

3. <span data-ttu-id="d9727-133">Bontsa ki a tárfiók **Fájlmegosztásait**.</span><span class="sxs-lookup"><span data-stu-id="d9727-133">Expand the storage account's **File Shares**.</span></span>

4. <span data-ttu-id="d9727-134">Kattintson a jobb gombbal a megtekinteni kívánt fájlmegosztásra, és a helyi menüben válassza a **Megnyitás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-134">Right-click the file share you wish to view, and - from the context menu - select **Open**.</span></span> <span data-ttu-id="d9727-135">Vagy kattintson duplán a megtekinteni kívánt fájlmegosztásra.</span><span class="sxs-lookup"><span data-stu-id="d9727-135">You can also double-click the file share you wish to view.</span></span>

    ![Megosztás megnyitása](media/vs-azure-tools-storage-explorer-files/image4.png)

5. <span data-ttu-id="d9727-137">A fájlmegosztás tartalma megjelenik a fő ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="d9727-137">The main pane will display the file share's contents.</span></span>
    
    ![A megosztás tartalma](media/vs-azure-tools-storage-explorer-files/image5.png)

## <a name="delete-a-file-share"></a><span data-ttu-id="d9727-139">Fájlmegosztás törlése</span><span class="sxs-lookup"><span data-stu-id="d9727-139">Delete a file share</span></span>

<span data-ttu-id="d9727-140">A fájlmegosztások szükség szerint egyszerűen létrehozhatók és törölhetők.</span><span class="sxs-lookup"><span data-stu-id="d9727-140">File shares can be easily created and deleted as needed.</span></span> <span data-ttu-id="d9727-141">(Az egyes fájlok törlésének módját lásd a [Fájlmegosztásban lévő fájlok kezelése](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container) című szakaszban.)</span><span class="sxs-lookup"><span data-stu-id="d9727-141">(To see how to delete individual files, refer to the section, [Managing files in a file share](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="d9727-142">A következő lépések bemutatják, hogyan törölhet fájlmegosztást a Storage Explorer előzetes verziójában:</span><span class="sxs-lookup"><span data-stu-id="d9727-142">The following steps illustrate how to delete a file share within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="d9727-143">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="d9727-143">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="d9727-144">A bal oldali ablaktáblán bontsa ki a megtekinteni kívánt fájlmegosztást tartalmazó tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="d9727-144">In the left pane, expand the storage account containing the file share you wish to view.</span></span>

3. <span data-ttu-id="d9727-145">Bontsa ki a tárfiók **Fájlmegosztásait**.</span><span class="sxs-lookup"><span data-stu-id="d9727-145">Expand the storage account's **File Shares**.</span></span>

4. <span data-ttu-id="d9727-146">Kattintson a jobb gombbal a törölni kívánt fájlmegosztásra, és a helyi menüben válassza a **Törlés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-146">Right-click the file share you wish to delete, and - from the context menu - select **Delete**.</span></span> <span data-ttu-id="d9727-147">Az aktuálisan kijelölt fájlmegosztás a **Delete** billentyű lenyomásával is törölhető.</span><span class="sxs-lookup"><span data-stu-id="d9727-147">You can also press **Delete** to delete the currently selected file share.</span></span>

    ![Törlés](media/vs-azure-tools-storage-explorer-files/image6.png)

5. <span data-ttu-id="d9727-149">Válassza az **Igen** lehetőséget a megerősítési párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="d9727-149">Select **Yes** to the confirmation dialog.</span></span>
    
    ![Megerősítési párbeszédpanel](media/vs-azure-tools-storage-explorer-files/image7.png)

## <a name="copy-a-file-share"></a><span data-ttu-id="d9727-151">Fájlmegosztás másolása</span><span class="sxs-lookup"><span data-stu-id="d9727-151">Copy a file share</span></span>

<span data-ttu-id="d9727-152">A Storage Explorer (előzetes verzió) lehetővé teszi egy fájlmegosztás vágólapra másolását, majd egy másik tárfiókba történő beillesztését.</span><span class="sxs-lookup"><span data-stu-id="d9727-152">Storage Explorer (Preview) enables you to copy a file share to the clipboard, and then paste that file share into another storage account.</span></span> <span data-ttu-id="d9727-153">(Az egyes fájlok másolásának módját lásd a [Fájlmegosztásban lévő fájlok kezelése](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container) című szakaszban.)</span><span class="sxs-lookup"><span data-stu-id="d9727-153">(To see how to copy individual files, refer to the section, [Managing files in a file share](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="d9727-154">A következő lépések bemutatják, hogyan másolhat át fájlmegosztást egyik tárfiókból a másikba.</span><span class="sxs-lookup"><span data-stu-id="d9727-154">The following steps illustrate how to copy a file share from one storage account to another.</span></span>

1. <span data-ttu-id="d9727-155">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="d9727-155">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="d9727-156">A bal oldali ablaktáblán bontsa ki a másolni kívánt fájlmegosztást tartalmazó tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="d9727-156">In the left pane, expand the storage account containing the file share you wish to copy.</span></span>

3. <span data-ttu-id="d9727-157">Bontsa ki a tárfiók **Fájlmegosztásait**.</span><span class="sxs-lookup"><span data-stu-id="d9727-157">Expand the storage account's **File Shares**.</span></span>

4. <span data-ttu-id="d9727-158">Kattintson a jobb gombbal a másolni kívánt fájlmegosztásra, és a helyi menüben válassza a **Fájlmegosztás másolása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-158">Right-click the file share you wish to copy, and - from the context menu - select **Copy File Share**.</span></span>

    ![Fájlmegosztás másolása](media/vs-azure-tools-storage-explorer-files/image8.png)

5. <span data-ttu-id="d9727-160">Kattintson a jobb gombbal a kívánt „cél” tárfiókra, amelybe a fájlmegosztást be kívánja illeszteni, majd válassza a helyi menüből a **Fájlmegosztás beillesztése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-160">Right-click the desired "target" storage account into which you want to paste the file share, and - from the context menu - select **Paste File Share**.</span></span>

    ![Fájlmegosztás beillesztése](media/vs-azure-tools-storage-explorer-files/image9.png)

## <a name="get-the-sas-for-a-file-share"></a><span data-ttu-id="d9727-162">SAS lekérése fájlmegosztáshoz</span><span class="sxs-lookup"><span data-stu-id="d9727-162">Get the SAS for a file share</span></span>

<span data-ttu-id="d9727-163">A [közös hozzáférésű jogosultságkód (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) delegált hozzáférést biztosít a tárfiókon lévő erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="d9727-163">A [shared access signature (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) provides delegated access to resources in your storage account.</span></span> <span data-ttu-id="d9727-164">Ez azt jelenti, hogy egy adott időszakra megadhatja az ügyfeleknek a tárfiókban lévő objektumokra vonatkozó engedélyek bizonyos készletét a tár hozzáférési kulcsainak megosztása nélkül.</span><span class="sxs-lookup"><span data-stu-id="d9727-164">This means that you can grant a client limited permissions to objects in your storage account for a specified period of time and with a specified set of permissions, without having to share your account access keys.</span></span>

<span data-ttu-id="d9727-165">A következő lépések bemutatják, hogyan hozhat létre SAS-t egy fájlmegosztáshoz:+</span><span class="sxs-lookup"><span data-stu-id="d9727-165">The following steps illustrate how to create a SAS for a file share:+</span></span>

1. <span data-ttu-id="d9727-166">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="d9727-166">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="d9727-167">A bal oldali ablaktáblán bontsa ki azt a fájlmegosztást tartalmazó tárfiókot, amelyhez SAS-t kíván beszerezni.</span><span class="sxs-lookup"><span data-stu-id="d9727-167">In the left pane, expand the storage account containing the file share for which you wish to get a SAS.</span></span>

3. <span data-ttu-id="d9727-168">Bontsa ki a tárfiók **Fájlmegosztásait**.</span><span class="sxs-lookup"><span data-stu-id="d9727-168">Expand the storage account's **File Shares**.</span></span>

4. <span data-ttu-id="d9727-169">Kattintson a jobb gombbal a kívánt fájlmegosztásra, és válassza a helyi menüből a **Közös hozzáférésű jogosultságkód igénylése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-169">Right-click the desired file share, and - from the context menu - select **Get Shared Access Signature**.</span></span>

    ![Közös hozzáférésű jogosultságkód igénylése](media/vs-azure-tools-storage-explorer-files/image10.png)

5. <span data-ttu-id="d9727-171">A **Közös hozzáférésű jogosultságkód** párbeszédpanelen adja meg a szabályzatot, a kezdési és a lejárati dátumokat, az időzónát és az erőforrás kívánt hozzáférési szintjeit.</span><span class="sxs-lookup"><span data-stu-id="d9727-171">In the **Shared Access Signature** dialog, specify the policy, start and expiration dates, time zone, and access levels you want for the resource.</span></span>

    ![SAS párbeszédpanel](media/vs-azure-tools-storage-explorer-files/image11.png)

6. <span data-ttu-id="d9727-173">Az SAS-beállítások megadása után válassza a **Létrehozás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-173">When you're finished specifying the SAS options, select **Create**.</span></span>

7. <span data-ttu-id="d9727-174">Ezután egy második **Közös hozzáférésű jogosultságkód** párbeszédpanel jelenik meg, amelyen fel vannak sorolva a fájlmegosztások, valamint a tárolási erőforrások eléréséhez használható URL-cím és a lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="d9727-174">A second **Shared Access Signature** dialog will then display that lists the file share along with the URL and QueryStrings you can use to access the storage resource.</span></span> <span data-ttu-id="d9727-175">Válassza a **Másolás** parancsot a vágólapra másolni kívánt URL-cím mellett.</span><span class="sxs-lookup"><span data-stu-id="d9727-175">Select **Copy** next to the URL you wish to copy to the clipboard.</span></span>
    
    ![Második SAS párbeszédpanel](media/vs-azure-tools-storage-explorer-files/image12.png)

8. <span data-ttu-id="d9727-177">Ha elkészült, válassza a **Bezárás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-177">When done, select **Close**.</span></span>

## <a name="manage-access-policies-for-a-file-share"></a><span data-ttu-id="d9727-178">Fájlmegosztás hozzáférési szabályzatainak kezelése</span><span class="sxs-lookup"><span data-stu-id="d9727-178">Manage Access Policies for a file share</span></span>

<span data-ttu-id="d9727-179">A következő lépések bemutatják, hogyan történik a fájlmegosztáshoz tartozó hozzáférési szabályzatok kezelése (hozzáadása és eltávolítása):+ .</span><span class="sxs-lookup"><span data-stu-id="d9727-179">The following steps illustrate how to manage (add and remove) access policies for a file share:+ .</span></span> <span data-ttu-id="d9727-180">A hozzáférési szabályzatokkal olyan SAS URL-címek hozhatók létre, amelyeken keresztül a Storage-fájl erőforrás adott időtartamig hozzáférhető.</span><span class="sxs-lookup"><span data-stu-id="d9727-180">The Access Policies is used for creating SAS URLs through which people can use to access the Storage File resource during a defined period of time.</span></span>

1. <span data-ttu-id="d9727-181">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="d9727-181">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="d9727-182">A bal oldali ablaktáblán bontsa ki azt a fájlmegosztást tartalmazó tárfiókot, amelynek a hozzáférési szabályzatait kezelni kívánja.</span><span class="sxs-lookup"><span data-stu-id="d9727-182">In the left pane, expand the storage account containing the file share whose access policies you wish to manage.</span></span>

3. <span data-ttu-id="d9727-183">Bontsa ki a tárfiók **Fájlmegosztásait**.</span><span class="sxs-lookup"><span data-stu-id="d9727-183">Expand the storage account's **File Shares**.</span></span>

4. <span data-ttu-id="d9727-184">Jelölje ki a kívánt fájlmegosztást, és válassza a helyi menüből a **Hozzáférési szabályzatok kezelése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-184">Select the desired file share, and - from the context menu - select **Manage Access Policies**.</span></span>

    ![Hozzáférési szabályzatok kezelése helyi menü](media/vs-azure-tools-storage-explorer-files/image13.png)

5. <span data-ttu-id="d9727-186">A **Hozzáférési szabályzatok** párbeszédpanelen fel lesznek sorolva a kiválasztott fájlmegosztáshoz már létrehozott hozzáférési szabályzatok.</span><span class="sxs-lookup"><span data-stu-id="d9727-186">The **Access Policies** dialog will list any access policies already created for the selected file share.</span></span>
    
    ![Hozzáférési szabályzatok](media/vs-azure-tools-storage-explorer-files/image14.png)

6. <span data-ttu-id="d9727-188">A hozzáférésiszabályzat-kezelési feladattól függően kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d9727-188">Follow these steps depending on the access policy management task:</span></span>
    
    - <span data-ttu-id="d9727-189">**Új hozzáférési szabályzat hozzáadása** – Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-189">**Add a new access policy** - Select **Add**.</span></span> <span data-ttu-id="d9727-190">A **Hozzáférési szabályzatok** megjeleníti az újonnan létrehozott és hozzáadott hozzáférési szabályzatot (az alapértelmezett beállításokkal).</span><span class="sxs-lookup"><span data-stu-id="d9727-190">Once generated, the **Access Policies** dialog will display the newly added access policy (with default settings).</span></span>

    - <span data-ttu-id="d9727-191">**Hozzáférési szabályzat szerkesztése** – Hajtsa végre a kívánt módosításokat, majd válassza a **Mentés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-191">**Edit an access policy** - Make any desired edits, and select **Save**.</span></span>

    - <span data-ttu-id="d9727-192">**Hozzáférési szabályzat eltávolítása** – Válassza az **Eltávolítás** parancsot az eltávolítani kívánt hozzáférési szabályzat mellett.</span><span class="sxs-lookup"><span data-stu-id="d9727-192">**Remove an access policy** - Select **Remove** next to the access policy you wish to remove.</span></span>

7. <span data-ttu-id="d9727-193">Hozzon létre egy új SAS URL-címet a korábban létrehozott hozzáférési szabályzat használatával:</span><span class="sxs-lookup"><span data-stu-id="d9727-193">Create a new SAS URL using the Access Policy you created earlier:</span></span>
    
    ![SAS beszerzése](media/vs-azure-tools-storage-explorer-files/image15.png)
    
    ![Az SAS neve és tulajdonságai](media/vs-azure-tools-storage-explorer-files/image16.png)

## <a name="managing-files-in-a-file-share"></a><span data-ttu-id="d9727-196">Fájlmegosztásban lévő fájlok kezelése</span><span class="sxs-lookup"><span data-stu-id="d9727-196">Managing files in a file share</span></span>

<span data-ttu-id="d9727-197">A fájlmegosztás létrehozása után többek között fájlokat tölthet fel a fájlmegosztásra, fájlokat tölthet le a helyi számítógépre, illetve fájlokat nyithat meg a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d9727-197">Once you've created a file share, you can upload a file to that file share, download a file to your local computer, open a file on your local computer, and much more.</span></span>

<span data-ttu-id="d9727-198">A következő lépések bemutatják, hogyan kezelhetők a fájlok (és mappák) egy fájlmegosztásban.</span><span class="sxs-lookup"><span data-stu-id="d9727-198">The following steps illustrate how to manage the files (and folders) within a file share.</span></span>

1.  <span data-ttu-id="d9727-199">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="d9727-199">Open Storage Explorer (Preview).</span></span>

2.  <span data-ttu-id="d9727-200">A bal oldali ablaktáblán bontsa ki a kezelni kívánt fájlmegosztást tartalmazó tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="d9727-200">In the left pane, expand the storage account containing the file share you wish to manage.</span></span>

3.  <span data-ttu-id="d9727-201">Bontsa ki a tárfiók **Fájlmegosztásait**.</span><span class="sxs-lookup"><span data-stu-id="d9727-201">Expand the storage account's **File Shares**.</span></span>

4.  <span data-ttu-id="d9727-202">Kattintson duplán a megtekinteni kívánt fájlmegosztásra.</span><span class="sxs-lookup"><span data-stu-id="d9727-202">Double-click the file share you wish to view.</span></span>

5.  <span data-ttu-id="d9727-203">A fájlmegosztás tartalma megjelenik a fő ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="d9727-203">The main pane will display the file share's contents.</span></span>

    ![A megosztás tartalma](media/vs-azure-tools-storage-explorer-files/image17.png)

6.  <span data-ttu-id="d9727-205">A fájlmegosztás tartalma megjelenik a fő ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="d9727-205">The main pane will display the file share's contents.</span></span>

7.  <span data-ttu-id="d9727-206">Kövesse az alábbi lépéseket a végrehajtani kívánt feladattól függően:</span><span class="sxs-lookup"><span data-stu-id="d9727-206">Follow these steps depending on the task you wish to perform:</span></span>

    - <span data-ttu-id="d9727-207">**Fájlok feltöltése egy fájlmegosztásba**</span><span class="sxs-lookup"><span data-stu-id="d9727-207">**Upload files to a file share**</span></span>

        <span data-ttu-id="d9727-208">a.</span><span class="sxs-lookup"><span data-stu-id="d9727-208">a.</span></span>  <span data-ttu-id="d9727-209">A fő ablaktábla eszköztárán válassza a **Feltöltés**, majd a legördülő menüből a **Fájlok feltöltése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-209">On the main pane's toolbar, select **Upload**, and then **Upload Files** from the drop-down menu.</span></span>

        ![Fájlok feltöltése](media/vs-azure-tools-storage-explorer-files/image18.png)
        
        <span data-ttu-id="d9727-211">b.</span><span class="sxs-lookup"><span data-stu-id="d9727-211">b.</span></span> <span data-ttu-id="d9727-212">A **Fájlok feltöltése** párbeszédpanelen válassza a **Fájlok** szövegbeviteli mező jobb oldalán lévő, három pontot (**…**) ábrázoló gombot a feltölteni kívánt fájl(ok) kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="d9727-212">In the **Upload files** dialog, select the ellipsis (**…**) button on the right side of the **Files** text box to select the file(s) you wish to upload.</span></span>

        ![Fájlok hozzáadása](media/vs-azure-tools-storage-explorer-files/image19.png)

        <span data-ttu-id="d9727-214">c.</span><span class="sxs-lookup"><span data-stu-id="d9727-214">c.</span></span> <span data-ttu-id="d9727-215">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-215">Select **Upload**.</span></span>

    - <span data-ttu-id="d9727-216">**Mappa feltöltése egy fájlmegosztásba**</span><span class="sxs-lookup"><span data-stu-id="d9727-216">**Upload a folder to a file share**</span></span>
        
        <span data-ttu-id="d9727-217">a.</span><span class="sxs-lookup"><span data-stu-id="d9727-217">a.</span></span> <span data-ttu-id="d9727-218">A fő ablaktábla eszköztárán válassza a **Feltöltés**, majd a legördülő menüből a **Mappa feltöltése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-218">On the main pane's toolbar, select **Upload**, and then **Upload Folder** from the drop-down menu.</span></span>

        ![Mappa feltöltése menü](media/vs-azure-tools-storage-explorer-files/image20.png)

        <span data-ttu-id="d9727-220">b.</span><span class="sxs-lookup"><span data-stu-id="d9727-220">b.</span></span> <span data-ttu-id="d9727-221">A **Mappa feltöltése** párbeszédpanelen a **Mappa** szövegbeviteli mező jobb oldalán lévő, három pontot (**…**) ábrázoló gombbal válassza ki a mappát, amelynek a tartalmát fel kívánja tölteni.</span><span class="sxs-lookup"><span data-stu-id="d9727-221">In the **Upload folder** dialog, select the ellipsis (**…**) button on the right side of the **Folder** text box to select the folder whose contents you wish to upload.</span></span>

        <span data-ttu-id="d9727-222">c.</span><span class="sxs-lookup"><span data-stu-id="d9727-222">c.</span></span> <span data-ttu-id="d9727-223">Igény szerint megadhat egy célmappát, amelybe a kiválasztott mappa tartalma fel lesz töltve.</span><span class="sxs-lookup"><span data-stu-id="d9727-223">Optionally, specify a target folder into which the selected folder's contents will be uploaded.</span></span> <span data-ttu-id="d9727-224">Ha a célmappa nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="d9727-224">If the target folder doesn’t exist, it will be created.</span></span>

        <span data-ttu-id="d9727-225">d.</span><span class="sxs-lookup"><span data-stu-id="d9727-225">d.</span></span> <span data-ttu-id="d9727-226">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-226">Select **Upload**.</span></span>

    - <span data-ttu-id="d9727-227">**Fájl letöltése a helyi számítógépre**</span><span class="sxs-lookup"><span data-stu-id="d9727-227">**Download a file to your local computer**</span></span>
        
        <span data-ttu-id="d9727-228">a.</span><span class="sxs-lookup"><span data-stu-id="d9727-228">a.</span></span> <span data-ttu-id="d9727-229">Jelölje ki a letölteni kívánt fájlt.</span><span class="sxs-lookup"><span data-stu-id="d9727-229">Select the file you wish to download.</span></span>
        
        <span data-ttu-id="d9727-230">b.</span><span class="sxs-lookup"><span data-stu-id="d9727-230">b.</span></span> <span data-ttu-id="d9727-231">A fő ablaktábla eszköztárán válassza a **Letöltés** elemet.</span><span class="sxs-lookup"><span data-stu-id="d9727-231">On the main pane's toolbar, select **Download**.</span></span>
        
        <span data-ttu-id="d9727-232">c.</span><span class="sxs-lookup"><span data-stu-id="d9727-232">c.</span></span> <span data-ttu-id="d9727-233">**A letöltött fájl helyének megadása** párbeszédpanelen adja meg, hogy hová kívánja letölteni a fájlt, és adja meg a fájl kívánt nevét.</span><span class="sxs-lookup"><span data-stu-id="d9727-233">In the **Specify where to save the downloaded file** dialog, specify the location where you want the file downloaded, and the name you wish to give it.</span></span>

        <span data-ttu-id="d9727-234">d.</span><span class="sxs-lookup"><span data-stu-id="d9727-234">d.</span></span> <span data-ttu-id="d9727-235">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="d9727-235">Select **Save**.</span></span>

    - <span data-ttu-id="d9727-236">**Fájl megnyitása a helyi számítógépen**</span><span class="sxs-lookup"><span data-stu-id="d9727-236">**Open a file on your local computer**</span></span>
        
        <span data-ttu-id="d9727-237">a.</span><span class="sxs-lookup"><span data-stu-id="d9727-237">a.</span></span>  <span data-ttu-id="d9727-238">Jelölje ki a megnyitni kívánt fájlt.</span><span class="sxs-lookup"><span data-stu-id="d9727-238">Select the file you wish to open.</span></span>
        
        <span data-ttu-id="d9727-239">b.</span><span class="sxs-lookup"><span data-stu-id="d9727-239">b.</span></span>  <span data-ttu-id="d9727-240">A fő ablaktábla eszköztárán válassza a **Megnyitás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-240">On the main pane's toolbar, select **Open**.</span></span>
        
        <span data-ttu-id="d9727-241">c.</span><span class="sxs-lookup"><span data-stu-id="d9727-241">c.</span></span>  <span data-ttu-id="d9727-242">A rendszer letölti a fájlt, majd megnyitja a fájltípussal társított alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="d9727-242">The file will be downloaded and opened using the application associated with the file's underlying file type.</span></span>

    - <span data-ttu-id="d9727-243">**Fájl másolása a vágólapra**</span><span class="sxs-lookup"><span data-stu-id="d9727-243">**Copy a file to the clipboard**</span></span>

        <span data-ttu-id="d9727-244">a.</span><span class="sxs-lookup"><span data-stu-id="d9727-244">a.</span></span> <span data-ttu-id="d9727-245">Jelölje ki a másolni kívánt fájlt.</span><span class="sxs-lookup"><span data-stu-id="d9727-245">Select the file you wish to copy.</span></span>

        <span data-ttu-id="d9727-246">b.</span><span class="sxs-lookup"><span data-stu-id="d9727-246">b.</span></span> <span data-ttu-id="d9727-247">A fő ablaktábla eszköztárán válassza a **Másolás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9727-247">On the main pane's toolbar, select **Copy**.</span></span>

        <span data-ttu-id="d9727-248">c.</span><span class="sxs-lookup"><span data-stu-id="d9727-248">c.</span></span> <span data-ttu-id="d9727-249">A bal oldali ablaktáblán lépjen egy másik fájlmegosztásra, és kattintson rá duplán a fő ablaktáblán való megtekintéshez.</span><span class="sxs-lookup"><span data-stu-id="d9727-249">In the left pane, navigate to another file share, and double-click it to view it in the main pane.</span></span>

        <span data-ttu-id="d9727-250">d.</span><span class="sxs-lookup"><span data-stu-id="d9727-250">d.</span></span> <span data-ttu-id="d9727-251">A fő ablaktábla eszköztárán válassza a **Beillesztés** lehetőséget a fájl másolatának létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d9727-251">On the main pane's toolbar, select **Paste** to create a copy of the file.</span></span>

    - <span data-ttu-id="d9727-252">**Fájl törlése**</span><span class="sxs-lookup"><span data-stu-id="d9727-252">**Delete a file**</span></span>

        <span data-ttu-id="d9727-253">a.</span><span class="sxs-lookup"><span data-stu-id="d9727-253">a.</span></span> <span data-ttu-id="d9727-254">Jelölje ki a törölni kívánt fájlt.</span><span class="sxs-lookup"><span data-stu-id="d9727-254">Select the file you wish to delete.</span></span>

        <span data-ttu-id="d9727-255">b.</span><span class="sxs-lookup"><span data-stu-id="d9727-255">b.</span></span> <span data-ttu-id="d9727-256">A fő ablaktábla eszköztárán válassza a **Törlés** parancsot.</span><span class="sxs-lookup"><span data-stu-id="d9727-256">On the main pane's toolbar, select **Delete**.</span></span>

        <span data-ttu-id="d9727-257">c.</span><span class="sxs-lookup"><span data-stu-id="d9727-257">c.</span></span> <span data-ttu-id="d9727-258">Válassza az **Igen** lehetőséget a megerősítési párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="d9727-258">Select **Yes** to the confirmation dialog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9727-259">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9727-259">Next steps</span></span>

- <span data-ttu-id="d9727-260">A [Storage Explorer (előzetes verzió) legújabb kibocsátási megjegyzéseinek és videóinak megtekintése](http://www.storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="d9727-260">View the [latest Storage Explorer (Preview) release notes and videos](http://www.storageexplorer.com/).</span></span>

- <span data-ttu-id="d9727-261">Annak megismerése, hogyan [hozhat létre alkalmazásokat Azure-blobok, -táblák, -üzenetsorok és -fájlok használatával](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="d9727-261">Learn how to [create applications using Azure blobs, tables, queues, and files](https://azure.microsoft.com/documentation/services/storage/).</span></span>
