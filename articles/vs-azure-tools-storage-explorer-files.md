---
title: "a Tártallózó (előzetes verzió) az Azure File storage aaaUsing |} Microsoft Docs"
description: "Megtudhatja, hogyan megtudhatja, hogyan toouse Tártallózó (előzetes verzió) toowork fájllal közösen használja, és fájlokat."
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
ms.openlocfilehash: 98eb3cde711ae3dbfdb6ffaec23ae24f822370e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-storage-explorer-preview-with-azure-file-storage"></a><span data-ttu-id="97544-103">A Storage Explorer (előzetes verzió) használata az Azure File Storage szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="97544-103">Using Storage Explorer (Preview) with Azure File storage</span></span>

<span data-ttu-id="97544-104">Az Azure File storage egy olyan szolgáltatás, amely kínál a fájl osztja meg a hello felhő használatával hello szabványos Server Message Block (SMB) protokollt.</span><span class="sxs-lookup"><span data-stu-id="97544-104">Azure File storage is a service that offers file shares in hello cloud using hello standard Server Message Block (SMB) Protocol.</span></span> <span data-ttu-id="97544-105">Az SMB 2.1 és az SMB 3.0 protokollt is támogatja.</span><span class="sxs-lookup"><span data-stu-id="97544-105">Both SMB 2.1 and SMB 3.0 are supported.</span></span> <span data-ttu-id="97544-106">Az Azure File storage szolgáltatással telepítheti át gyorsan és költséges újraírások nélkül fájl megosztások tooAzure támaszkodó örökölt alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="97544-106">With Azure File storage, you can migrate legacy applications that rely on file shares tooAzure quickly and without costly rewrites.</span></span> <span data-ttu-id="97544-107">Használhatja a fájladatok tárolási tooexpose nyilvánosan toohello globális vagy toostore alkalmazásadatok közvetlenül a Microsoftnak.</span><span class="sxs-lookup"><span data-stu-id="97544-107">You can use File storage tooexpose data publicly toohello world, or toostore application data privately.</span></span> <span data-ttu-id="97544-108">Ebből a cikkből megtudhatja, hogyan toouse Tártallózó (előzetes verzió) toowork fájllal közösen használja, és fájlokat.</span><span class="sxs-lookup"><span data-stu-id="97544-108">In this article, you'll learn how toouse Storage Explorer (Preview) toowork with file shares and files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97544-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="97544-109">Prerequisites</span></span>

<span data-ttu-id="97544-110">toocomplete hello cikkben leírt lépéseket, az alábbi hello lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="97544-110">toocomplete hello steps in this article, you'll need hello following:</span></span>

- [<span data-ttu-id="97544-111">A Tártallózó (előzetes verzió) letöltése és telepítése</span><span class="sxs-lookup"><span data-stu-id="97544-111">Download and install Storage Explorer (preview)</span></span>](http://www.storageexplorer.com/)

- [<span data-ttu-id="97544-112">Tooa Azure storage-fiók vagy a szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="97544-112">Connect tooa Azure storage account or service</span></span>](https://docs.microsoft.com//azure/vs-azure-tools-storage-manage-with-storage-explorer#connect-to-a-storage-account-or-service)

## <a name="create-a-file-share"></a><span data-ttu-id="97544-113">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="97544-113">Create a File Share</span></span>

<span data-ttu-id="97544-114">Minden fájlnak fájlmegosztásban kell lennie, amely egyszerűen egy logikai fájlcsoportot jelent.</span><span class="sxs-lookup"><span data-stu-id="97544-114">All files must reside in a file share, which is simply a logical grouping of files.</span></span> <span data-ttu-id="97544-115">Egy fiók korlátlan számú fájlmegosztást tartalmazhat, egy adott megosztás pedig korlátlan számú fájl tárolására használható.</span><span class="sxs-lookup"><span data-stu-id="97544-115">An account can contain an unlimited number of file shares, and each share can store an unlimited number of files.</span></span>

<span data-ttu-id="97544-116">hello lépések bemutatják, hogyan toocreate fájlmegosztás belül a Tártallózó (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="97544-116">hello following steps illustrate how toocreate a file share within Storage Explorer (Preview).</span></span>

1. <span data-ttu-id="97544-117">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="97544-117">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="97544-118">Hello bal oldali ablaktáblán bontsa ki a hello tárfiókot, amelyen belül kívánja toocreate hello fájlmegosztás</span><span class="sxs-lookup"><span data-stu-id="97544-118">In hello left pane, expand hello storage account within which you wish toocreate hello File Share</span></span>

3. <span data-ttu-id="97544-119">Kattintson a jobb gombbal **fájlmegosztások**, és válassza – hello helyi menüből – a **fájlmegosztás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="97544-119">Right-click **File Shares**, and - from hello context menu - select **Create File Share**.</span></span>

    ![Fájlmegosztás létrehozása](media/vs-azure-tools-storage-explorer-files/image1.png)

4. <span data-ttu-id="97544-121">A szövegmezőben megjelenik, hello alatt **fájlmegosztások** mappát.</span><span class="sxs-lookup"><span data-stu-id="97544-121">A text box will appear below hello **File Shares** folder.</span></span> <span data-ttu-id="97544-122">Adja meg a fájlmegosztás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="97544-122">Enter hello name for your file share.</span></span> <span data-ttu-id="97544-123">Lásd: hello [elnevezési szabályok megosztása](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) szakasz listáját a szabályok és a fájlmegosztásokat naming korlátozásai.</span><span class="sxs-lookup"><span data-stu-id="97544-123">See hello [Share naming rules](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) section for a list of rules and restrictions on naming file shares.</span></span>

    ![Elnevezési hello megosztás](media/vs-azure-tools-storage-explorer-files/image2.png)

5. <span data-ttu-id="97544-125">Nyomja le az **Enter** kész toocreate hello fájlmegosztáshoz, amikor vagy **Esc** toocancel.</span><span class="sxs-lookup"><span data-stu-id="97544-125">Press **Enter** when done toocreate hello file share, or **Esc** toocancel.</span></span> <span data-ttu-id="97544-126">Hello fájlmegosztás sikeres létrehozását követően megjelenik a hello **fájlmegosztások** hello mappa kiválasztott tárfiók.</span><span class="sxs-lookup"><span data-stu-id="97544-126">Once hello file share has been successfully created, it will be displayed under hello **File Shares** folder for hello selected storage account.</span></span>

    ![hello új megosztás](media/vs-azure-tools-storage-explorer-files/image3.png)

## <a name="view-a-file-shares-contents"></a><span data-ttu-id="97544-128">Fájlmegosztás tartalmának megtekintése</span><span class="sxs-lookup"><span data-stu-id="97544-128">View a file share's contents</span></span>

<span data-ttu-id="97544-129">A fájlmegosztások fájlokat és mappákat tartalmaznak (a mappákban pedig további fájlok lehetnek).</span><span class="sxs-lookup"><span data-stu-id="97544-129">File shares contain files and folders (that can also contain files).</span></span>

<span data-ttu-id="97544-130">hello következő lépések bemutatják, hogyan fájl tartalmának tooview hello megosztása belül a Tártallózó (előzetes verzió): +</span><span class="sxs-lookup"><span data-stu-id="97544-130">hello following steps illustrate how tooview hello contents of a file share within Storage Explorer (Preview):+</span></span>

1. <span data-ttu-id="97544-131">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="97544-131">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="97544-132">Hello bal oldali ablaktáblán bontsa ki a tooview kívánja hello fájlmegosztás tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="97544-132">In hello left pane, expand hello storage account containing hello file share you wish tooview.</span></span>

3. <span data-ttu-id="97544-133">Bontsa ki a hello tárfiók **fájlmegosztások**.</span><span class="sxs-lookup"><span data-stu-id="97544-133">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="97544-134">Kattintson a jobb gombbal hello fájlmegosztás meg akarja tooview, és válassza – a hello helyi menüből – **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="97544-134">Right-click hello file share you wish tooview, and - from hello context menu - select **Open**.</span></span> <span data-ttu-id="97544-135">Kattintson duplán a tooview kívánja hello fájlmegosztás is.</span><span class="sxs-lookup"><span data-stu-id="97544-135">You can also double-click hello file share you wish tooview.</span></span>

    ![Megosztás megnyitása](media/vs-azure-tools-storage-explorer-files/image4.png)

5. <span data-ttu-id="97544-137">hello fő ablaktábla hello fájl megosztott tartalmat.</span><span class="sxs-lookup"><span data-stu-id="97544-137">hello main pane will display hello file share's contents.</span></span>
    
    ![hello a megosztás fájlkiszolgálója tartalma](media/vs-azure-tools-storage-explorer-files/image5.png)

## <a name="delete-a-file-share"></a><span data-ttu-id="97544-139">Fájlmegosztás törlése</span><span class="sxs-lookup"><span data-stu-id="97544-139">Delete a file share</span></span>

<span data-ttu-id="97544-140">A fájlmegosztások szükség szerint egyszerűen létrehozhatók és törölhetők.</span><span class="sxs-lookup"><span data-stu-id="97544-140">File shares can be easily created and deleted as needed.</span></span> <span data-ttu-id="97544-141">(toosee hogyan toodelete fájlokat, tekintse meg a toohello szakasz [fájljait egy fájlmegosztásban kezelése](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="97544-141">(toosee how toodelete individual files, refer toohello section, [Managing files in a file share](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="97544-142">hello lépések bemutatják, hogyan toodelete fájlmegosztás belül a Tártallózó (előzetes verzió):</span><span class="sxs-lookup"><span data-stu-id="97544-142">hello following steps illustrate how toodelete a file share within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="97544-143">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="97544-143">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="97544-144">Hello bal oldali ablaktáblán bontsa ki a tooview kívánja hello fájlmegosztás tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="97544-144">In hello left pane, expand hello storage account containing hello file share you wish tooview.</span></span>

3. <span data-ttu-id="97544-145">Bontsa ki a hello tárfiók **fájlmegosztások**.</span><span class="sxs-lookup"><span data-stu-id="97544-145">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="97544-146">Kattintson a jobb gombbal hello fájlmegosztás meg akarja toodelete, és válassza – a hello helyi menüből – **törlése**.</span><span class="sxs-lookup"><span data-stu-id="97544-146">Right-click hello file share you wish toodelete, and - from hello context menu - select **Delete**.</span></span> <span data-ttu-id="97544-147">Is **törlése** toodelete hello kijelölt fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="97544-147">You can also press **Delete** toodelete hello currently selected file share.</span></span>

    ![Törlés](media/vs-azure-tools-storage-explorer-files/image6.png)

5. <span data-ttu-id="97544-149">Válassza ki **Igen** toohello megerősítő párbeszédpanele.</span><span class="sxs-lookup"><span data-stu-id="97544-149">Select **Yes** toohello confirmation dialog.</span></span>
    
    ![Megerősítési párbeszédpanel](media/vs-azure-tools-storage-explorer-files/image7.png)

## <a name="copy-a-file-share"></a><span data-ttu-id="97544-151">Fájlmegosztás másolása</span><span class="sxs-lookup"><span data-stu-id="97544-151">Copy a file share</span></span>

<span data-ttu-id="97544-152">A Tártallózó (előzetes verzió) lehetővé teszi egy fájl megosztási toohello vágólapra toocopy, és illessze be egy másik tárfiókhoz fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="97544-152">Storage Explorer (Preview) enables you toocopy a file share toohello clipboard, and then paste that file share into another storage account.</span></span> <span data-ttu-id="97544-153">(toosee hogyan toocopy fájlokat, tekintse meg a toohello szakasz [fájljait egy fájlmegosztásban kezelése](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="97544-153">(toosee how toocopy individual files, refer toohello section, [Managing files in a file share](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="97544-154">hello lépések bemutatják, hogyan toocopy egy fájlt egy tárolási fiók tooanother megoszthatja.</span><span class="sxs-lookup"><span data-stu-id="97544-154">hello following steps illustrate how toocopy a file share from one storage account tooanother.</span></span>

1. <span data-ttu-id="97544-155">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="97544-155">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="97544-156">Hello bal oldali ablaktáblán bontsa ki a toocopy kívánja hello fájlmegosztás tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="97544-156">In hello left pane, expand hello storage account containing hello file share you wish toocopy.</span></span>

3. <span data-ttu-id="97544-157">Bontsa ki a hello tárfiók **fájlmegosztások**.</span><span class="sxs-lookup"><span data-stu-id="97544-157">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="97544-158">Kattintson a jobb gombbal hello fájlmegosztás meg akarja toocopy, és válassza – a hello helyi menüből – **másolási fájlmegosztás**.</span><span class="sxs-lookup"><span data-stu-id="97544-158">Right-click hello file share you wish toocopy, and - from hello context menu - select **Copy File Share**.</span></span>

    ![Fájlmegosztás másolása](media/vs-azure-tools-storage-explorer-files/image8.png)

5. <span data-ttu-id="97544-160">Kattintson a jobb gombbal a kívánt hello "target" tárfiók, amelybe azt szeretné, hogy toopaste hello fájlmegosztás, és válassza – a hello helyi menüből – **Beillesztés fájlmegosztás**.</span><span class="sxs-lookup"><span data-stu-id="97544-160">Right-click hello desired "target" storage account into which you want toopaste hello file share, and - from hello context menu - select **Paste File Share**.</span></span>

    ![Fájlmegosztás beillesztése](media/vs-azure-tools-storage-explorer-files/image9.png)

## <a name="get-hello-sas-for-a-file-share"></a><span data-ttu-id="97544-162">Hello SAS lekérése fájlmegosztás</span><span class="sxs-lookup"><span data-stu-id="97544-162">Get hello SAS for a file share</span></span>

<span data-ttu-id="97544-163">A [közös hozzáférésű jogosultságkód (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) delegált hozzáférést tooresources a tárfiókban lévő biztosít.</span><span class="sxs-lookup"><span data-stu-id="97544-163">A [shared access signature (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) provides delegated access tooresources in your storage account.</span></span> <span data-ttu-id="97544-164">Ez azt jelenti, hogy egy ügyfél csak korlátozott engedélyekkel tooobjects a tárfiókban lévő egy megadott időszakban, és engedélyeket, megadott számú anélkül, hogy tooshare a tárelérési kulcsok biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="97544-164">This means that you can grant a client limited permissions tooobjects in your storage account for a specified period of time and with a specified set of permissions, without having tooshare your account access keys.</span></span>

<span data-ttu-id="97544-165">hello következő lépések bemutatják, hogyan toocreate egy SAS-t egy fájl megosztása: +</span><span class="sxs-lookup"><span data-stu-id="97544-165">hello following steps illustrate how toocreate a SAS for a file share:+</span></span>

1. <span data-ttu-id="97544-166">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="97544-166">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="97544-167">Hello bal oldali ablaktáblájában bontsa ki, amelynek kívánja tooget SAS hello fájlmegosztás tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="97544-167">In hello left pane, expand hello storage account containing hello file share for which you wish tooget a SAS.</span></span>

3. <span data-ttu-id="97544-168">Bontsa ki a hello tárfiók **fájlmegosztások**.</span><span class="sxs-lookup"><span data-stu-id="97544-168">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="97544-169">Kattintson a jobb gombbal a kívánt fájlmegosztás hello, és válassza – a hello helyi menüből – **közös hozzáférésű Jogosultságkód beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="97544-169">Right-click hello desired file share, and - from hello context menu - select **Get Shared Access Signature**.</span></span>

    ![Közös hozzáférésű jogosultságkód igénylése](media/vs-azure-tools-storage-explorer-files/image10.png)

5. <span data-ttu-id="97544-171">A hello **közös hozzáférésű Jogosultságkód** párbeszédpanelen adja meg a hello házirend, érvényességének kezdő és záró dátumát, időzóna, és a hozzáférési szintet hello erőforrás használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="97544-171">In hello **Shared Access Signature** dialog, specify hello policy, start and expiration dates, time zone, and access levels you want for hello resource.</span></span>

    ![SAS párbeszédpanel](media/vs-azure-tools-storage-explorer-files/image11.png)

6. <span data-ttu-id="97544-173">Ha befejezte a hello SAS-beállítások, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="97544-173">When you're finished specifying hello SAS options, select **Create**.</span></span>

7. <span data-ttu-id="97544-174">Egy második **közös hozzáférésű Jogosultságkód** párbeszédpanel ezután jeleníti meg, hogy listák hello fájlmegosztás hello URL-cím mellett, és használhatja a tooaccess QueryStrings hello tárolási erőforrás.</span><span class="sxs-lookup"><span data-stu-id="97544-174">A second **Shared Access Signature** dialog will then display that lists hello file share along with hello URL and QueryStrings you can use tooaccess hello storage resource.</span></span> <span data-ttu-id="97544-175">Válassza ki **másolási** toocopy toohello vágólapra kívánja tovább toohello URL.</span><span class="sxs-lookup"><span data-stu-id="97544-175">Select **Copy** next toohello URL you wish toocopy toohello clipboard.</span></span>
    
    ![Második SAS párbeszédpanel](media/vs-azure-tools-storage-explorer-files/image12.png)

8. <span data-ttu-id="97544-177">Ha elkészült, válassza a **Bezárás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="97544-177">When done, select **Close**.</span></span>

## <a name="manage-access-policies-for-a-file-share"></a><span data-ttu-id="97544-178">Fájlmegosztás hozzáférési szabályzatainak kezelése</span><span class="sxs-lookup"><span data-stu-id="97544-178">Manage Access Policies for a file share</span></span>

<span data-ttu-id="97544-179">hello következő lépések bemutatják, hogyan toomanage (hozzáadása és eltávolítása) hozzáférési házirendek egy fájlmegosztás: +.</span><span class="sxs-lookup"><span data-stu-id="97544-179">hello following steps illustrate how toomanage (add and remove) access policies for a file share:+ .</span></span> <span data-ttu-id="97544-180">hello hozzáférési házirendek SAS URL-címek, amelyek személy tooaccess hello tárolófájl erőforrás használhatja egy meghatározott időszakban létrehozására szolgál.</span><span class="sxs-lookup"><span data-stu-id="97544-180">hello Access Policies is used for creating SAS URLs through which people can use tooaccess hello Storage File resource during a defined period of time.</span></span>

1. <span data-ttu-id="97544-181">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="97544-181">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="97544-182">Hello bal oldali ablaktáblán bontsa ki a hello fájlmegosztást, amelynek hozzáférési házirendek toomanage kívánja tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="97544-182">In hello left pane, expand hello storage account containing hello file share whose access policies you wish toomanage.</span></span>

3. <span data-ttu-id="97544-183">Bontsa ki a hello tárfiók **fájlmegosztások**.</span><span class="sxs-lookup"><span data-stu-id="97544-183">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="97544-184">Válassza ki a kívánt fájlmegosztás hello, és válassza – a hello helyi menüből – **hozzáférési házirendek kezelése**.</span><span class="sxs-lookup"><span data-stu-id="97544-184">Select hello desired file share, and - from hello context menu - select **Manage Access Policies**.</span></span>

    ![Hozzáférési szabályzatok kezelése helyi menü](media/vs-azure-tools-storage-explorer-files/image13.png)

5. <span data-ttu-id="97544-186">Hello **hozzáférési házirendek** párbeszédpanel megjelennek a kijelölt hello fájlmegosztás már létrehozott hozzáférési házirendekben.</span><span class="sxs-lookup"><span data-stu-id="97544-186">hello **Access Policies** dialog will list any access policies already created for hello selected file share.</span></span>
    
    ![Hozzáférési szabályzatok](media/vs-azure-tools-storage-explorer-files/image14.png)

6. <span data-ttu-id="97544-188">Kövesse az alábbi lépéseket attól függően, hogy hello hozzáférési házirend fájlkezelési feladat:</span><span class="sxs-lookup"><span data-stu-id="97544-188">Follow these steps depending on hello access policy management task:</span></span>
    
    - <span data-ttu-id="97544-189">**Új hozzáférési szabályzat hozzáadása** – Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="97544-189">**Add a new access policy** - Select **Add**.</span></span> <span data-ttu-id="97544-190">Létrehozott, miután hello **hozzáférési házirendek** párbeszédpanel megjeleníti az újonnan hozzáadott hello házirendhez (az alapértelmezett beállításokkal).</span><span class="sxs-lookup"><span data-stu-id="97544-190">Once generated, hello **Access Policies** dialog will display hello newly added access policy (with default settings).</span></span>

    - <span data-ttu-id="97544-191">**Hozzáférési szabályzat szerkesztése** – Hajtsa végre a kívánt módosításokat, majd válassza a **Mentés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="97544-191">**Edit an access policy** - Make any desired edits, and select **Save**.</span></span>

    - <span data-ttu-id="97544-192">**Távolítsa el a hozzáférési házirendek** – Itt adhatja meg **eltávolítása** tooremove kívánja tovább toohello házirend.</span><span class="sxs-lookup"><span data-stu-id="97544-192">**Remove an access policy** - Select **Remove** next toohello access policy you wish tooremove.</span></span>

7. <span data-ttu-id="97544-193">Hozzon létre egy új SAS URL-CÍMÉT a korábban létrehozott házirend hello:</span><span class="sxs-lookup"><span data-stu-id="97544-193">Create a new SAS URL using hello Access Policy you created earlier:</span></span>
    
    ![SAS beszerzése](media/vs-azure-tools-storage-explorer-files/image15.png)
    
    ![Az SAS neve és tulajdonságai](media/vs-azure-tools-storage-explorer-files/image16.png)

## <a name="managing-files-in-a-file-share"></a><span data-ttu-id="97544-196">Fájlmegosztásban lévő fájlok kezelése</span><span class="sxs-lookup"><span data-stu-id="97544-196">Managing files in a file share</span></span>

<span data-ttu-id="97544-197">Ha létrehozta a fájlmegosztást, fájlmegosztás toothat fájl feltöltéséhez, töltse le a fájl tooyour helyi számítógépen, nyissa meg a fájlt a helyi számítógépen, és még sok más.</span><span class="sxs-lookup"><span data-stu-id="97544-197">Once you've created a file share, you can upload a file toothat file share, download a file tooyour local computer, open a file on your local computer, and much more.</span></span>

<span data-ttu-id="97544-198">hello következő lépések bemutatják, hogyan toomanage hello fájlokat (és mappákat) a fájlon belüli megosztani.</span><span class="sxs-lookup"><span data-stu-id="97544-198">hello following steps illustrate how toomanage hello files (and folders) within a file share.</span></span>

1.  <span data-ttu-id="97544-199">Nyissa meg a Tártallózót (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="97544-199">Open Storage Explorer (Preview).</span></span>

2.  <span data-ttu-id="97544-200">Hello bal oldali ablaktáblán bontsa ki a toomanage kívánja hello fájlmegosztás tartalmazó hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="97544-200">In hello left pane, expand hello storage account containing hello file share you wish toomanage.</span></span>

3.  <span data-ttu-id="97544-201">Bontsa ki a hello tárfiók **fájlmegosztások**.</span><span class="sxs-lookup"><span data-stu-id="97544-201">Expand hello storage account's **File Shares**.</span></span>

4.  <span data-ttu-id="97544-202">Kattintson duplán a tooview kívánja hello fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="97544-202">Double-click hello file share you wish tooview.</span></span>

5.  <span data-ttu-id="97544-203">hello fő ablaktábla hello fájl megosztott tartalmat.</span><span class="sxs-lookup"><span data-stu-id="97544-203">hello main pane will display hello file share's contents.</span></span>

    ![hello a megosztás fájlkiszolgálója tartalma](media/vs-azure-tools-storage-explorer-files/image17.png)

6.  <span data-ttu-id="97544-205">hello fő ablaktábla hello fájl megosztott tartalmat.</span><span class="sxs-lookup"><span data-stu-id="97544-205">hello main pane will display hello file share's contents.</span></span>

7.  <span data-ttu-id="97544-206">Kövesse az alábbi lépéseket attól függően, hello feladat kívánja tooperform:</span><span class="sxs-lookup"><span data-stu-id="97544-206">Follow these steps depending on hello task you wish tooperform:</span></span>

    - <span data-ttu-id="97544-207">**Fájlok tooa fájlmegosztás feltöltése**</span><span class="sxs-lookup"><span data-stu-id="97544-207">**Upload files tooa file share**</span></span>

        <span data-ttu-id="97544-208">a.</span><span class="sxs-lookup"><span data-stu-id="97544-208">a.</span></span>  <span data-ttu-id="97544-209">Hello fő ablaktáblán eszköztáron válassza **feltöltése**, majd **fájl feltöltése** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="97544-209">On hello main pane's toolbar, select **Upload**, and then **Upload Files** from hello drop-down menu.</span></span>

        ![Fájlok feltöltése](media/vs-azure-tools-storage-explorer-files/image18.png)
        
        <span data-ttu-id="97544-211">b.</span><span class="sxs-lookup"><span data-stu-id="97544-211">b.</span></span> <span data-ttu-id="97544-212">A hello **fájlok feltöltése** párbeszédpanelen jelölje be hello három pont (**...** ) gombra a hello jobb oldalán hello **fájlok** szövegmezőben tooselect hello (oka) t tooupload kívánja.</span><span class="sxs-lookup"><span data-stu-id="97544-212">In hello **Upload files** dialog, select hello ellipsis (**…**) button on hello right side of hello **Files** text box tooselect hello file(s) you wish tooupload.</span></span>

        ![Fájlok hozzáadása](media/vs-azure-tools-storage-explorer-files/image19.png)

        <span data-ttu-id="97544-214">c.</span><span class="sxs-lookup"><span data-stu-id="97544-214">c.</span></span> <span data-ttu-id="97544-215">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="97544-215">Select **Upload**.</span></span>

    - <span data-ttu-id="97544-216">**A mappa tooa fájlmegosztás feltöltése**</span><span class="sxs-lookup"><span data-stu-id="97544-216">**Upload a folder tooa file share**</span></span>
        
        <span data-ttu-id="97544-217">a.</span><span class="sxs-lookup"><span data-stu-id="97544-217">a.</span></span> <span data-ttu-id="97544-218">Hello fő ablaktáblán eszköztáron válassza **feltöltése**, majd **feltöltése mappa** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="97544-218">On hello main pane's toolbar, select **Upload**, and then **Upload Folder** from hello drop-down menu.</span></span>

        ![Mappa feltöltése menü](media/vs-azure-tools-storage-explorer-files/image20.png)

        <span data-ttu-id="97544-220">b.</span><span class="sxs-lookup"><span data-stu-id="97544-220">b.</span></span> <span data-ttu-id="97544-221">A hello **feltöltési mappa** párbeszédpanelen jelölje be hello három pont (**...** ) gombra a hello jobb oldalán hello **mappa** szöveg mezőben tooselect hello mappa tartalma tooupload kívánja.</span><span class="sxs-lookup"><span data-stu-id="97544-221">In hello **Upload folder** dialog, select hello ellipsis (**…**) button on hello right side of hello **Folder** text box tooselect hello folder whose contents you wish tooupload.</span></span>

        <span data-ttu-id="97544-222">c.</span><span class="sxs-lookup"><span data-stu-id="97544-222">c.</span></span> <span data-ttu-id="97544-223">Szükség esetén adja meg, hogy a célmappa, mely hello a kijelölt mappa tartalma lesz feltöltve.</span><span class="sxs-lookup"><span data-stu-id="97544-223">Optionally, specify a target folder into which hello selected folder's contents will be uploaded.</span></span> <span data-ttu-id="97544-224">Ha hello célmappa nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="97544-224">If hello target folder doesn’t exist, it will be created.</span></span>

        <span data-ttu-id="97544-225">d.</span><span class="sxs-lookup"><span data-stu-id="97544-225">d.</span></span> <span data-ttu-id="97544-226">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="97544-226">Select **Upload**.</span></span>

    - <span data-ttu-id="97544-227">**Töltse le a fájl tooyour helyi számítógépről**</span><span class="sxs-lookup"><span data-stu-id="97544-227">**Download a file tooyour local computer**</span></span>
        
        <span data-ttu-id="97544-228">a.</span><span class="sxs-lookup"><span data-stu-id="97544-228">a.</span></span> <span data-ttu-id="97544-229">Válassza ki a toodownload kívánja hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="97544-229">Select hello file you wish toodownload.</span></span>
        
        <span data-ttu-id="97544-230">b.</span><span class="sxs-lookup"><span data-stu-id="97544-230">b.</span></span> <span data-ttu-id="97544-231">Hello fő ablaktáblán eszköztáron válassza **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="97544-231">On hello main pane's toolbar, select **Download**.</span></span>
        
        <span data-ttu-id="97544-232">c.</span><span class="sxs-lookup"><span data-stu-id="97544-232">c.</span></span> <span data-ttu-id="97544-233">A hello **adja meg, ahol toosave hello letöltött fájl** párbeszédpanelen adja meg a hello helyének letöltött hello fájlt, és hello neve toogive kívánja azt.</span><span class="sxs-lookup"><span data-stu-id="97544-233">In hello **Specify where toosave hello downloaded file** dialog, specify hello location where you want hello file downloaded, and hello name you wish toogive it.</span></span>

        <span data-ttu-id="97544-234">d.</span><span class="sxs-lookup"><span data-stu-id="97544-234">d.</span></span> <span data-ttu-id="97544-235">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="97544-235">Select **Save**.</span></span>

    - <span data-ttu-id="97544-236">**Fájl megnyitása a helyi számítógépen**</span><span class="sxs-lookup"><span data-stu-id="97544-236">**Open a file on your local computer**</span></span>
        
        <span data-ttu-id="97544-237">a.</span><span class="sxs-lookup"><span data-stu-id="97544-237">a.</span></span>  <span data-ttu-id="97544-238">Válassza ki a tooopen kívánja hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="97544-238">Select hello file you wish tooopen.</span></span>
        
        <span data-ttu-id="97544-239">b.</span><span class="sxs-lookup"><span data-stu-id="97544-239">b.</span></span>  <span data-ttu-id="97544-240">Hello fő ablaktáblán eszköztáron válassza **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="97544-240">On hello main pane's toolbar, select **Open**.</span></span>
        
        <span data-ttu-id="97544-241">c.</span><span class="sxs-lookup"><span data-stu-id="97544-241">c.</span></span>  <span data-ttu-id="97544-242">hello fájl tölthető le, és hello fájl fájl alaptípusának társított hello alkalmazás használatával megnyitni.</span><span class="sxs-lookup"><span data-stu-id="97544-242">hello file will be downloaded and opened using hello application associated with hello file's underlying file type.</span></span>

    - <span data-ttu-id="97544-243">**A fájl toohello vágólapra másolása**</span><span class="sxs-lookup"><span data-stu-id="97544-243">**Copy a file toohello clipboard**</span></span>

        <span data-ttu-id="97544-244">a.</span><span class="sxs-lookup"><span data-stu-id="97544-244">a.</span></span> <span data-ttu-id="97544-245">Válassza ki a toocopy kívánja hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="97544-245">Select hello file you wish toocopy.</span></span>

        <span data-ttu-id="97544-246">b.</span><span class="sxs-lookup"><span data-stu-id="97544-246">b.</span></span> <span data-ttu-id="97544-247">Hello fő ablaktáblán eszköztáron válassza **másolási**.</span><span class="sxs-lookup"><span data-stu-id="97544-247">On hello main pane's toolbar, select **Copy**.</span></span>

        <span data-ttu-id="97544-248">c.</span><span class="sxs-lookup"><span data-stu-id="97544-248">c.</span></span> <span data-ttu-id="97544-249">Hello bal oldali ablaktáblán, keresse meg a fájlmegosztás tooanother, és kattintson rá duplán tooview azt hello fő ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="97544-249">In hello left pane, navigate tooanother file share, and double-click it tooview it in hello main pane.</span></span>

        <span data-ttu-id="97544-250">d.</span><span class="sxs-lookup"><span data-stu-id="97544-250">d.</span></span> <span data-ttu-id="97544-251">Hello fő ablaktáblán eszköztáron válassza **Beillesztés** toocreate hello fájl egy példányát.</span><span class="sxs-lookup"><span data-stu-id="97544-251">On hello main pane's toolbar, select **Paste** toocreate a copy of hello file.</span></span>

    - <span data-ttu-id="97544-252">**Fájl törlése**</span><span class="sxs-lookup"><span data-stu-id="97544-252">**Delete a file**</span></span>

        <span data-ttu-id="97544-253">a.</span><span class="sxs-lookup"><span data-stu-id="97544-253">a.</span></span> <span data-ttu-id="97544-254">Válassza ki a toodelete kívánja hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="97544-254">Select hello file you wish toodelete.</span></span>

        <span data-ttu-id="97544-255">b.</span><span class="sxs-lookup"><span data-stu-id="97544-255">b.</span></span> <span data-ttu-id="97544-256">Hello fő ablaktáblán eszköztáron válassza **törlése**.</span><span class="sxs-lookup"><span data-stu-id="97544-256">On hello main pane's toolbar, select **Delete**.</span></span>

        <span data-ttu-id="97544-257">c.</span><span class="sxs-lookup"><span data-stu-id="97544-257">c.</span></span> <span data-ttu-id="97544-258">Válassza ki **Igen** toohello megerősítő párbeszédpanele.</span><span class="sxs-lookup"><span data-stu-id="97544-258">Select **Yes** toohello confirmation dialog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97544-259">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97544-259">Next steps</span></span>

- <span data-ttu-id="97544-260">Nézet hello [legújabb Tártallózó (előzetes verzió) kibocsátási megjegyzések és videók](http://www.storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="97544-260">View hello [latest Storage Explorer (Preview) release notes and videos](http://www.storageexplorer.com/).</span></span>

- <span data-ttu-id="97544-261">Ismerje meg, hogyan túl[létrehozása az Azure BLOB, táblák, üzenetsorok és fájlokat használó alkalmazások](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="97544-261">Learn how too[create applications using Azure blobs, tables, queues, and files](https://azure.microsoft.com/documentation/services/storage/).</span></span>
