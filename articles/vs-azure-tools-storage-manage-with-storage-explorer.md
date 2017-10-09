---
title: "a Tártallózó (előzetes verzió) használatába aaaGet |} Microsoft Docs"
description: "Azure tárerőforrások kezelése a Tártallózó alkalmazással (előzetes verzió)"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/17/2017
ms.author: kraigb
ms.openlocfilehash: 57737b51baace92858eb07c7dbc3139bd7e041f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-storage-explorer-preview"></a><span data-ttu-id="ad48d-103">Ismerkedés a Tártallózó alkalmazással (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="ad48d-103">Get started with Storage Explorer (Preview)</span></span>
## <a name="overview"></a><span data-ttu-id="ad48d-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ad48d-104">Overview</span></span>
<span data-ttu-id="ad48d-105">Az Azure Tártallózó (előzetes verzió) egy különálló alkalmazás, amely lehetővé teszi tooeasily dolgozhat Azure Storage-adatokkal Windows, a macOS és a Linux.</span><span class="sxs-lookup"><span data-stu-id="ad48d-105">Azure Storage Explorer (Preview) is a standalone app that enables you tooeasily work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="ad48d-106">Ebből a cikkből megismerheti hello csatlakozás az Azure storage-fiókok kezelése tooand különféle módjait.</span><span class="sxs-lookup"><span data-stu-id="ad48d-106">In this article, you learn hello various ways of connecting tooand managing your Azure storage accounts.</span></span>

![Microsoft Azure Tártallózó (előzetes verzió)][15]

## <a name="prerequisites"></a><span data-ttu-id="ad48d-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ad48d-108">Prerequisites</span></span>
* [<span data-ttu-id="ad48d-109">A Tártallózó (előzetes verzió) letöltése és telepítése</span><span class="sxs-lookup"><span data-stu-id="ad48d-109">Download and install Storage Explorer (Preview)</span></span>](http://www.storageexplorer.com)

## <a name="connect-tooa-storage-account-or-service"></a><span data-ttu-id="ad48d-110">Csatlakozás tooa tárfiók vagy szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ad48d-110">Connect tooa storage account or service</span></span>
<span data-ttu-id="ad48d-111">A Tártallózó (előzetes verzió) számos lehetőséget biztosít tooconnect toostorage fiókok.</span><span class="sxs-lookup"><span data-stu-id="ad48d-111">Storage Explorer (Preview) provides several ways tooconnect toostorage accounts.</span></span> <span data-ttu-id="ad48d-112">Megteheti például a következőt:</span><span class="sxs-lookup"><span data-stu-id="ad48d-112">For example, you can:</span></span>
* <span data-ttu-id="ad48d-113">Csatlakozás az Azure-előfizetésekkel társított toostorage fiókokat.</span><span class="sxs-lookup"><span data-stu-id="ad48d-113">Connect toostorage accounts associated with your Azure subscriptions.</span></span>
* <span data-ttu-id="ad48d-114">Csatlakoztatja a toostorage fiókokat és szolgáltatásokat, megosztott a többi Azure-előfizetések.</span><span class="sxs-lookup"><span data-stu-id="ad48d-114">Connect toostorage accounts and services that are shared from other Azure subscriptions.</span></span>
* <span data-ttu-id="ad48d-115">Csatlakozás tooand helyi tárterület kezelése hello Azure Storage Emulator használatával.</span><span class="sxs-lookup"><span data-stu-id="ad48d-115">Connect tooand manage local storage by using hello Azure Storage Emulator.</span></span> 

<span data-ttu-id="ad48d-116">Emellett használhatja a tárfiókokat a globális és az országos Azure-ban:</span><span class="sxs-lookup"><span data-stu-id="ad48d-116">In addition, you can work with storage accounts in global and national Azure:</span></span>

* <span data-ttu-id="ad48d-117">[Csatlakozás Azure-előfizetés tooan](#connect-to-an-azure-subscription): tooyour Azure-előfizetéséhez tartozó tárolási erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="ad48d-117">[Connect tooan Azure subscription](#connect-to-an-azure-subscription): Manage storage resources that belong tooyour Azure subscription.</span></span>
* <span data-ttu-id="ad48d-118">[Helyi fejlesztési tárolási együttműködve](#work-with-local-development-storage): helyi tárterület kezelése hello Azure Storage Emulator használatával.</span><span class="sxs-lookup"><span data-stu-id="ad48d-118">[Work with local development storage](#work-with-local-development-storage): Manage local storage by using hello Azure Storage Emulator.</span></span>
* <span data-ttu-id="ad48d-119">[Tooexternal tárolási csatolása](#attach-or-detach-an-external-storage-account): tooanother Azure-előfizetéséhez tartozó tárolási erőforrások kezelése vagy a nemzeti Azure felhők hello tárolási fiók nevét, a kulcs és a végpontok segítségével.</span><span class="sxs-lookup"><span data-stu-id="ad48d-119">[Attach tooexternal storage](#attach-or-detach-an-external-storage-account): Manage storage resources that belong tooanother Azure subscription or that are under national Azure clouds by using hello storage account's name, key, and endpoints.</span></span>
* <span data-ttu-id="ad48d-120">[A storage-fiók csatolása a SAS használatával](#attach-storage-account-using-sas): a közös hozzáférésű jogosultságkód (SAS) használatával tooanother Azure-előfizetéséhez tartozó tárolási erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="ad48d-120">[Attach a storage account by using an SAS](#attach-storage-account-using-sas): Manage storage resources that belong tooanother Azure subscription by using a shared access signature (SAS).</span></span>
* <span data-ttu-id="ad48d-121">[Szolgáltatás csatolása a SAS használatával](#attach-service-using-sas): egy adott tárolási szolgáltatás (blob tároló, sor vagy tábla), amely tooanother Azure-előfizetés tartozik egy SAS használatával kezelheti.</span><span class="sxs-lookup"><span data-stu-id="ad48d-121">[Attach a service by using an SAS](#attach-service-using-sas): Manage a specific storage service (blob container, queue, or table) that belongs tooanother Azure subscription by using an SAS.</span></span>

## <a name="connect-tooan-azure-subscription"></a><span data-ttu-id="ad48d-122">Csatlakozás Azure-előfizetés tooan</span><span class="sxs-lookup"><span data-stu-id="ad48d-122">Connect tooan Azure subscription</span></span>
> [!NOTE]
> <span data-ttu-id="ad48d-123">Ha nincs Azure-fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) vagy [aktiválhatja Visual Studio-előfizetése kiemelt előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="ad48d-123">If you don't have an Azure account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
>
>

1. <span data-ttu-id="ad48d-124">A Tártallózóban (előzetes verzió) válassza ki az **Azure-fiók beállításai** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ad48d-124">In Storage Explorer (Preview), select **Azure Account settings**.</span></span>

    ![Azure-fiók beállításai][0]

2. <span data-ttu-id="ad48d-126">hello bal oldali ablaktáblán megjelennek azok összes hello Microsoft-fiókkal jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="ad48d-126">hello left pane displays all hello Microsoft accounts you've signed in to.</span></span> <span data-ttu-id="ad48d-127">tooconnect tooanother fiókját, válassza **vegyen fel egy fiókot**, és kövesse a hello utasításokat toosign be legalább egy aktív Azure-előfizetéssel társított Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="ad48d-127">tooconnect tooanother account, select **Add an account**, and then follow hello instructions toosign in with a Microsoft account that is associated with at least one active Azure subscription.</span></span>

    >[!NOTE]
    ><span data-ttu-id="ad48d-128">Csatlakozás Azure (például a Németországi Azure Azure Government és bejelentkezési keresztül Azure Kína) toonational jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ad48d-128">Connecting toonational Azure (such as Azure Germany, Azure Government, and Azure China via sign-in) is currently not supported.</span></span> <span data-ttu-id="ad48d-129">Lásd: hello "Attach vagy külső tárfiók leválasztása" szakaszban arról, hogyan tooconnect toonational Azure storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="ad48d-129">See hello "Attach or detach an external storage account" section for how tooconnect toonational Azure storage accounts.</span></span>

3. <span data-ttu-id="ad48d-130">Miután sikeresen bejelentkezett Microsoft-fiókkal, hello bal oldali ablaktáblán a telepítéskor hello fiókhoz társított Azure-előfizetések.</span><span class="sxs-lookup"><span data-stu-id="ad48d-130">After you successfully sign in with a Microsoft account, hello left pane is populated with hello Azure subscriptions associated with that account.</span></span> <span data-ttu-id="ad48d-131">Válassza ki az Azure-előfizetések, amelyek szeretné, hogy a toowork, és válassza hello **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-131">Select hello Azure subscriptions that you want toowork with, and then select **Apply**.</span></span> <span data-ttu-id="ad48d-132">(Kiválasztásával **előfizetéseket** kiválasztásával vagy hello váltógombok felsorolt Azure-előfizetések.)</span><span class="sxs-lookup"><span data-stu-id="ad48d-132">(Selecting **All subscriptions** toggles selecting all or none of hello listed Azure subscriptions.)</span></span>

    ![Azure-előfizetések kiválasztása][3]  
    <span data-ttu-id="ad48d-134">hello bal oldali panelen kiválasztott hello Azure-előfizetéssel társított tárfiókokat hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ad48d-134">hello left pane displays hello storage accounts associated with hello selected Azure subscriptions.</span></span>

    ![Kiválasztott Azure-előfizetések][4]

## <a name="connect-tooan-azure-stack-subscription"></a><span data-ttu-id="ad48d-136">Csatlakozás tooan Azure verem előfizetés</span><span class="sxs-lookup"><span data-stu-id="ad48d-136">Connect tooan Azure Stack subscription</span></span>

<span data-ttu-id="ad48d-137">Csatlakozás tooan Azure verem előfizetés kapcsolatos információkért lásd: [Tártallózó csatlakozás tooan Azure verem előfizetés](azure-stack/azure-stack-storage-connect-se.md).</span><span class="sxs-lookup"><span data-stu-id="ad48d-137">For information about connecting tooan Azure Stack subscription, see [Connect Storage Explorer tooan Azure Stack subscription](azure-stack/azure-stack-storage-connect-se.md).</span></span>

## <a name="work-with-local-development-storage"></a><span data-ttu-id="ad48d-138">Munkavégzés helyi fejlesztési tárterülettel</span><span class="sxs-lookup"><span data-stu-id="ad48d-138">Work with local development storage</span></span>
<span data-ttu-id="ad48d-139">A Tártallózó (előzetes verzió) használhat elleni helyi tároló hello Azure Storage Emulator használatával.</span><span class="sxs-lookup"><span data-stu-id="ad48d-139">With Storage Explorer (Preview), you can work against local storage by using hello Azure Storage Emulator.</span></span> <span data-ttu-id="ad48d-140">Ez a megközelítés lehetővé teszi kódot a tárterületre és a vizsgálati tárolási írását nélkül feltétlenül egy tárfiókot, Azure-ban telepített mert hello tárfiók hello Azure Storage Emulator tárfiókra.</span><span class="sxs-lookup"><span data-stu-id="ad48d-140">This approach lets you write code against and test storage without necessarily having a storage account deployed on Azure, because hello storage account is being emulated by hello Azure Storage Emulator.</span></span>

> [!NOTE]
> <span data-ttu-id="ad48d-141">csak a Windows hello Azure Storage Emulator jelenleg támogatott.</span><span class="sxs-lookup"><span data-stu-id="ad48d-141">hello Azure Storage Emulator is currently supported only for Windows.</span></span>
>
>

1. <span data-ttu-id="ad48d-142">Hello a Tártallózó (előzetes verzió) bal oldali ablaktáblán, bontsa ki a hello **(helyi és kapcsolódó)** > **Tárfiókok** > **(fejlesztés)** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ad48d-142">In hello left pane of Storage Explorer (Preview), expand hello **(Local and Attached)** > **Storage Accounts** > **(Development)** node.</span></span>

    ![Helyi fejlesztési csomópont][21]

2. <span data-ttu-id="ad48d-144">Ha még nem telepítette hello Azure Storage Emulator,-e felszólító toodo Igen, az információs sávon.</span><span class="sxs-lookup"><span data-stu-id="ad48d-144">If you have not yet installed hello Azure Storage Emulator, you are prompted toodo so via an infobar.</span></span> <span data-ttu-id="ad48d-145">Ha hello információs sáv megjelenik, jelölje be **hello legújabb verzió letöltése**, majd telepítse a hello emulátor.</span><span class="sxs-lookup"><span data-stu-id="ad48d-145">If hello infobar is displayed, select **Download hello latest version**, and then install hello emulator.</span></span>

    ![Azure Storage Emulator letöltése prompt][22]

3. <span data-ttu-id="ad48d-147">Hello emulátor telepítése után hozzon létre, és együttműködik a helyi blobokat, üzenetsorokat és táblákat.</span><span class="sxs-lookup"><span data-stu-id="ad48d-147">After hello emulator is installed, you can create and work with local blobs, queues, and tables.</span></span> <span data-ttu-id="ad48d-148">hogyan írja be mindegyik storage-fiók toowork, toolearn lásd: hello a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="ad48d-148">toolearn how toowork with each storage account type, see one of hello following:</span></span>

    * [<span data-ttu-id="ad48d-149">Azure Blob Storage-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="ad48d-149">Manage Azure blob storage resources</span></span>](vs-azure-tools-storage-explorer-blobs.md)
    * <span data-ttu-id="ad48d-150">Azure File Share Storage-erőforrások kezelése: *Hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ad48d-150">Manage Azure file share storage resources: *Coming soon*</span></span>
    * <span data-ttu-id="ad48d-151">Azure Queue Storage-erőforrások kezelése: *Hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ad48d-151">Manage Azure queue storage resources: *Coming soon*</span></span>
    * <span data-ttu-id="ad48d-152">Azure Table Storage-erőforrások kezelése: *Hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ad48d-152">Manage Azure table storage resources: *Coming soon*</span></span>

## <a name="attach-or-detach-an-external-storage-account"></a><span data-ttu-id="ad48d-153">Külső tárfiók csatolása vagy leválasztása</span><span class="sxs-lookup"><span data-stu-id="ad48d-153">Attach or detach an external storage account</span></span>
<span data-ttu-id="ad48d-154">A Tártallózó (előzetes verzió) csatolhat a szolgáltatáskéréshez tooexternal storage-fiókok, hogy a storage-fiókok azok könnyen megoszthatóak.</span><span class="sxs-lookup"><span data-stu-id="ad48d-154">With Storage Explorer (Preview), you can attach tooexternal storage accounts so that storage accounts can be easily shared.</span></span> <span data-ttu-id="ad48d-155">Ez a szakasz azt ismerteti, hogyan tooattach too(and detach from) külső tárfiókok.</span><span class="sxs-lookup"><span data-stu-id="ad48d-155">This section explains how tooattach too(and detach from) external storage accounts.</span></span>

### <a name="get-hello-storage-account-credentials"></a><span data-ttu-id="ad48d-156">Hello tárfiók hitelesítő adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="ad48d-156">Get hello storage account credentials</span></span>
<span data-ttu-id="ad48d-157">külső tárfiók tooshare, az adott fiók tulajdonosának hello kell szereznie hello fiók hello hitelesítő adatokat (a fióknevet és a kulcs) és oszthat meg ezt az információt tooattach toothat (külső) fiók kívánó hello személlyel.</span><span class="sxs-lookup"><span data-stu-id="ad48d-157">tooshare an external storage account, hello owner of that account must first get hello credentials (account name and key) for hello account and then share that information with hello person who wants tooattach toothat (external) account.</span></span> <span data-ttu-id="ad48d-158">Beszerezhet hello tárfiók hitelesítő adatainak hello Azure-portálon keresztül hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="ad48d-158">You can obtain hello storage account credentials via hello Azure portal by doing hello following:</span></span>

1. <span data-ttu-id="ad48d-159">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ad48d-159">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="ad48d-160">Válassza a **Tallózás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ad48d-160">Select **Browse**.</span></span>

3. <span data-ttu-id="ad48d-161">Válassza a **Tárfiókok** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ad48d-161">Select **Storage Accounts**.</span></span>

4. <span data-ttu-id="ad48d-162">A hello **Tárfiókok** panelen, a select hello kívánt tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="ad48d-162">On hello **Storage Accounts** blade, select hello desired storage account.</span></span>

5. <span data-ttu-id="ad48d-163">A hello **beállítások** hello panelen kiválasztott tárfiók, jelölje be **hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-163">On hello **Settings** blade for hello selected storage account, select **Access keys**.</span></span>

    ![Hozzáférési kulcs lehetőség][5]

6. <span data-ttu-id="ad48d-165">A hello **hívóbetűk** panelen, a Másolás hello **tárfióknév** és **key1** értékek toohello tárfiók csatolásához.</span><span class="sxs-lookup"><span data-stu-id="ad48d-165">On hello **Access keys** blade, copy hello **Storage account name** and **key1** values for use when attaching toohello storage account.</span></span>

    ![Elérési kulcs][6]

### <a name="attach-tooan-external-storage-account"></a><span data-ttu-id="ad48d-167">Tooan külső tárfiók csatolása</span><span class="sxs-lookup"><span data-stu-id="ad48d-167">Attach tooan external storage account</span></span>
<span data-ttu-id="ad48d-168">tooattach tooan külső tárfiók, kell hello fiók nevét és a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="ad48d-168">tooattach tooan external storage account, you need hello account's name and key.</span></span> <span data-ttu-id="ad48d-169">hello "Get hello tárfiók hitelesítő adatainak" szakasz ismerteti, hogyan ezeket az értékeket tooobtain hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="ad48d-169">hello "Get hello storage account credentials" section explains how tooobtain these values from hello Azure portal.</span></span> <span data-ttu-id="ad48d-170">Azonban a hello portal hello fiókkulcs nevezik **key1**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-170">However, in hello portal, hello account key is called **key1**.</span></span> <span data-ttu-id="ad48d-171">Ezért a Tártallózó (előzetes verzió) fiókkulcs kér, ahol megadhatja hello **key1** érték.</span><span class="sxs-lookup"><span data-stu-id="ad48d-171">So where Storage Explorer (Preview) asks for an account key, you enter hello **key1** value.</span></span>

1. <span data-ttu-id="ad48d-172">A Tártallózó (előzetes verzió), válassza ki a **csatlakoztassa tooAzure tárterületet**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-172">In Storage Explorer (Preview), select **Connect tooAzure storage**.</span></span>

    ![Csatlakozási tooAzure tárolás beállítása][23]

2. <span data-ttu-id="ad48d-174">Hello a **tooAzure tárolási csatlakozás** párbeszédpanel hello fiókkulcs adja meg (hello **key1** hello Azure-portálon értéket), majd válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-174">In hello **Connect tooAzure Storage** dialog box, specify hello account key (hello **key1** value from hello Azure portal), and then select **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ad48d-175">A tárfiók nemzeti Azure adhat meg hello tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="ad48d-175">You can enter hello storage connection string from a storage account on national Azure.</span></span> <span data-ttu-id="ad48d-176">Tooconnect tooAzure Németország storage-fiókok, írja be például a következő: kapcsolati karakterláncok hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="ad48d-176">For example, tooconnect tooAzure Germany storage accounts, enter connection strings similar toohello following:</span></span> 
    >
    >* <span data-ttu-id="ad48d-177">DefaultEndpointsProtocol=https</span><span class="sxs-lookup"><span data-stu-id="ad48d-177">DefaultEndpointsProtocol=https</span></span>
    >* <span data-ttu-id="ad48d-178">AccountName=cawatest03</span><span class="sxs-lookup"><span data-stu-id="ad48d-178">AccountName=cawatest03</span></span>
    >* <span data-ttu-id="ad48d-179">AccountKey=<tárfiók kulcsa></span><span class="sxs-lookup"><span data-stu-id="ad48d-179">AccountKey=<storage_account_key></span></span>
    >* <span data-ttu-id="ad48d-180">EndpointSuffix=core.cloudapi.de</span><span class="sxs-lookup"><span data-stu-id="ad48d-180">EndpointSuffix=core.cloudapi.de</span></span>
    
    ><span data-ttu-id="ad48d-181">Hello kapcsolati karakterlánc beolvasása hello Azure portál a hello azonos hasonlóan hello "Hello tárfiók hitelesítő adatainak beolvasása" szakasz.</span><span class="sxs-lookup"><span data-stu-id="ad48d-181">You can get hello connection string from hello Azure portal in hello same way as described in hello "Get hello storage account credentials" section.</span></span>

    ![Csatlakozás tooAzure tárolási párbeszédpanel][24]

3. <span data-ttu-id="ad48d-183">A hello **külső tárterület csatolása** párbeszédpanel hello **fióknév** mezőbe, írja be a tárfiók neve hello, adja meg az egyéb beállításait, és válassza **következő**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-183">In hello **Attach External Storage** dialog box, in hello **Account name** box, enter hello storage account name, specify any other desired settings, and then select **Next**.</span></span>

    ![Külső tárterület csatolása párbeszédpanel][8]

4. <span data-ttu-id="ad48d-185">A hello **kapcsolat összegzés** párbeszédpanel hello információk ellenőrizze, hogy.</span><span class="sxs-lookup"><span data-stu-id="ad48d-185">In hello **Connection Summary** dialog box, verify hello information.</span></span> <span data-ttu-id="ad48d-186">Ha semmi toochange, jelölje be **vissza** és írja be újra a hello szükséges beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ad48d-186">If you want toochange anything, select **Back** and reenter hello desired settings.</span></span> 

5. <span data-ttu-id="ad48d-187">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ad48d-187">Select **Connect**.</span></span>

6. <span data-ttu-id="ad48d-188">Miután sikeresen csatlakoztatva van, hello külső tárfiók jelenik meg, amely **(külső)** toohello tárfiók neve lesz hozzáfűzve.</span><span class="sxs-lookup"><span data-stu-id="ad48d-188">After it is successfully connected, hello external storage account is displayed with **(External)** appended toohello storage account name.</span></span>

    ![Külső tárfiók tooan kapcsolódás eredménye][9]

### <a name="detach-from-an-external-storage-account"></a><span data-ttu-id="ad48d-190">Külső tárfiók leválasztása</span><span class="sxs-lookup"><span data-stu-id="ad48d-190">Detach from an external storage account</span></span>
1. <span data-ttu-id="ad48d-191">Kattintson a jobb gombbal a külső tárfiók hello, szeretné, hogy toodetach, és válassza **leválasztási**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-191">Right-click hello external storage account that you want toodetach, and then select **Detach**.</span></span>

    ![Tár leválasztása lehetőség][10]

2. <span data-ttu-id="ad48d-193">Hello megerősítő üzenetet, jelölje ki **Igen** hello külső tárfiók leválasztásának tooconfirm hello.</span><span class="sxs-lookup"><span data-stu-id="ad48d-193">In hello confirmation message, select **Yes** tooconfirm hello detachment from hello external storage account.</span></span>

## <a name="attach-a-storage-account-by-using-an-sas"></a><span data-ttu-id="ad48d-194">Tárfiók csatolása SAS használatával</span><span class="sxs-lookup"><span data-stu-id="ad48d-194">Attach a storage account by using an SAS</span></span>
<span data-ttu-id="ad48d-195">Egy [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) lehetővé teszi, hogy a Üdvözöljük a rendszergazdákat az Azure-előfizetés adjon ideiglenes hozzáférést tooa tárfiók tooprovide Azure-előfizetés hitelesítő adatok nélkül.</span><span class="sxs-lookup"><span data-stu-id="ad48d-195">An [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) lets hello admin of an Azure subscription grant temporary access tooa storage account without having tooprovide Azure subscription credentials.</span></span>

<span data-ttu-id="ad48d-196">tooillustrate ebben az esetben most mondja ki, hogy "a" felhasználó Azure-előfizetés rendszergazdája, és "a" felhasználó szeretne tooallow "b" felhasználó tooaccess tárolási fiók adott időtartamra és meghatározott engedélyekkel:</span><span class="sxs-lookup"><span data-stu-id="ad48d-196">tooillustrate this scenario, let's say that UserA is an admin of an Azure subscription, and UserA wants tooallow UserB tooaccess a storage account for a limited time with certain permissions:</span></span>

1. <span data-ttu-id="ad48d-197">"A" felhasználó időszak és hello szükséges engedélyekkel az adott időszak hoz létre egy SAS (álló hello tárfiók hello kapcsolati karakterlánc).</span><span class="sxs-lookup"><span data-stu-id="ad48d-197">UserA generates an SAS (consisting of hello connection string for hello storage account) for a specific time period and with hello desired permissions.</span></span>

2. <span data-ttu-id="ad48d-198">"A" felhasználó megosztások SAS hello személlyel hello ("b" felhasználó, a fenti példában) hozzáférési toohello tárfiók kívánó.</span><span class="sxs-lookup"><span data-stu-id="ad48d-198">UserA shares hello SAS with hello person (UserB, in our example) who wants access toohello storage account.</span></span>  

3. <span data-ttu-id="ad48d-199">"B" felhasználó a Tártallózó (előzetes verzió) tooattach toohello fiókot használja, amelyet a tooUserA tartozik hello megadott SAS.</span><span class="sxs-lookup"><span data-stu-id="ad48d-199">UserB uses Storage Explorer (Preview) tooattach toohello account that belongs tooUserA by using hello supplied SAS.</span></span>

### <a name="get-an-sas-for-hello-account-you-want-tooshare"></a><span data-ttu-id="ad48d-200">Az SAS lekérése hello fióknevet, amelyet az tooshare</span><span class="sxs-lookup"><span data-stu-id="ad48d-200">Get an SAS for hello account you want tooshare</span></span>
1. <span data-ttu-id="ad48d-201">A Tártallózó (előzetes verzió), kattintson a jobb gombbal szeretné osztani, majd válassza ki hello tárfiók **közös hozzáférésű Jogosultságkód beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-201">In Storage Explorer (Preview), right-click hello storage account you want share, and then select **Get Shared Access Signature**.</span></span>

    ![SAS beszerzése menüpont][13]

2. <span data-ttu-id="ad48d-203">A hello **közös hozzáférésű Jogosultságkód** párbeszédpanelen adja meg a hello időtartamot és engedélyeket, hello fiók, és válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-203">In hello **Shared Access Signature** dialog box, specify hello time frame and permissions that you want for hello account, and then select **Create**.</span></span>

    <span data-ttu-id="ad48d-204">![SAS beszerzése párbeszédpanel][14]</span><span class="sxs-lookup"><span data-stu-id="ad48d-204">![Get SAS dialog box][14]</span></span>  
    <span data-ttu-id="ad48d-205">Hello **közös hozzáférésű Jogosultságkód** párbeszédpanel megnyílik, és hello SAS jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ad48d-205">hello **Shared Access Signature** dialog box opens and displays hello SAS.</span></span>

3. <span data-ttu-id="ad48d-206">Következő toohello **kapcsolati karakterlánc**, jelölje be **másolási** toocopy azt toohello vágólapra, majd válassza ki **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-206">Next toohello **Connection String**, select **Copy** toocopy it toohello clipboard, and then select **Close**.</span></span>

### <a name="attach-toohello-shared-account-by-using-hello-sas"></a><span data-ttu-id="ad48d-207">Megosztott toohello fiók csatolása hello SAS segítségével</span><span class="sxs-lookup"><span data-stu-id="ad48d-207">Attach toohello shared account by using hello SAS</span></span>
1. <span data-ttu-id="ad48d-208">A Tártallózó (előzetes verzió), válassza ki a **csatlakoztassa tooAzure tárterületet**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-208">In Storage Explorer (Preview), select **Connect tooAzure storage**.</span></span>

    ![Csatlakozási tooAzure tárolás beállítása][23]

2. <span data-ttu-id="ad48d-210">A hello **tooAzure tárolási csatlakozás** párbeszédpanelen adja meg a hello kapcsolati karakterláncot, majd válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-210">In hello **Connect tooAzure Storage** dialog box, specify hello connection string, and then select **Next**.</span></span>

    ![Csatlakozás tooAzure tárolási párbeszédpanel][24]

3. <span data-ttu-id="ad48d-212">A hello **kapcsolat összegzés** párbeszédpanel hello információk ellenőrizze, hogy.</span><span class="sxs-lookup"><span data-stu-id="ad48d-212">In hello **Connection Summary** dialog box, verify hello information.</span></span> <span data-ttu-id="ad48d-213">toomake módosításokat, válassza ki **vissza**, és írja be a kívánt hello beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ad48d-213">toomake changes, select **Back**, and then enter hello settings you want.</span></span> 

4. <span data-ttu-id="ad48d-214">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ad48d-214">Select **Connect**.</span></span>

5. <span data-ttu-id="ad48d-215">Miután csatlakozik, hello tárfiók jelenik meg, amely **(SAS)** megadott fióknév toohello lesz hozzáfűzve.</span><span class="sxs-lookup"><span data-stu-id="ad48d-215">After it is attached, hello storage account is displayed with **(SAS)** appended toohello account name that you supplied.</span></span>

    ![Csatolt tooan fiók SAS használatával eredménye][17]

## <a name="attach-a-service-by-using-an-sas"></a><span data-ttu-id="ad48d-217">Szolgáltatás csatolása SAS használatával</span><span class="sxs-lookup"><span data-stu-id="ad48d-217">Attach a service by using an SAS</span></span>
<span data-ttu-id="ad48d-218">hello "Tárfiók csatolása a SAS használatával" szakasz ismerteti, hogyan egy Azure-előfizetés rendszergazdája biztosíthat ideiglenes hozzáférést tooa storage-fiók létrehozása és megosztása hello tárfiók egy SAS-kód.</span><span class="sxs-lookup"><span data-stu-id="ad48d-218">hello "Attach a storage account by using an SAS" section explains how an Azure subscription admin can grant temporary access tooa storage account by generating and sharing an SAS for hello storage account.</span></span> <span data-ttu-id="ad48d-219">Hasonlóképpen SAS-kód létrehozható adott szolgáltatásokhoz (blob tárolókhoz, üzenetsorokhoz és táblákhoz) is a tárfiókon belül.</span><span class="sxs-lookup"><span data-stu-id="ad48d-219">Similarly, an SAS can be generated for a specific service (blob container, queue, or table) within a storage account.</span></span>  

### <a name="generate-an-sas-for-hello-service-that-you-want-tooshare"></a><span data-ttu-id="ad48d-220">Hello szolgáltatást, amelyet az tooshare egy SAS-kód készítése</span><span class="sxs-lookup"><span data-stu-id="ad48d-220">Generate an SAS for hello service that you want tooshare</span></span>
<span data-ttu-id="ad48d-221">Ebben a kontextusban a szolgáltatások blob tárolók, üzenetsorok vagy táblák lehetnek.</span><span class="sxs-lookup"><span data-stu-id="ad48d-221">In this context, a service can be a blob container, queue, or table.</span></span> <span data-ttu-id="ad48d-222">toogenerate hello SAS felsorolt szolgáltatáshoz, lásd:</span><span class="sxs-lookup"><span data-stu-id="ad48d-222">toogenerate hello SAS for a listed service, see:</span></span>

* [<span data-ttu-id="ad48d-223">Hello SAS lekérése blob tárolóhoz</span><span class="sxs-lookup"><span data-stu-id="ad48d-223">Get hello SAS for a blob container</span></span>](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* <span data-ttu-id="ad48d-224">Hello SAS lekérése fájlmegosztás: *hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ad48d-224">Get hello SAS for a file share: *Coming soon*</span></span>
* <span data-ttu-id="ad48d-225">Hello SAS lekérése várólista: *hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ad48d-225">Get hello SAS for a queue: *Coming soon*</span></span>
* <span data-ttu-id="ad48d-226">Hello SAS lekérése táblához: *hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ad48d-226">Get hello SAS for a table: *Coming soon*</span></span>

### <a name="attach-toohello-shared-account-service-by-using-hello-sas"></a><span data-ttu-id="ad48d-227">A szolgáltatás hello SAS toohello közös fiók csatolása</span><span class="sxs-lookup"><span data-stu-id="ad48d-227">Attach toohello shared account service by using hello SAS</span></span>
1. <span data-ttu-id="ad48d-228">A Tártallózó (előzetes verzió), válassza ki a **csatlakoztassa tooAzure tárterületet**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-228">In Storage Explorer (Preview), select **Connect tooAzure storage**.</span></span>

    ![Csatlakozási tooAzure tárolás beállítása][23]

2. <span data-ttu-id="ad48d-230">A hello **tooAzure tárolási csatlakozás** párbeszédpanelen adja meg a hello SAS URI-t, és válassza **következő**.</span><span class="sxs-lookup"><span data-stu-id="ad48d-230">In hello **Connect tooAzure Storage** dialog box, specify hello SAS URI, and then select **Next**.</span></span>

    ![Csatlakozás tooAzure tárolási párbeszédpanel][24]

3. <span data-ttu-id="ad48d-232">A hello **kapcsolat összegzés** párbeszédpanel hello információk ellenőrizze, hogy.</span><span class="sxs-lookup"><span data-stu-id="ad48d-232">In hello **Connection Summary** dialog box, verify hello information.</span></span> <span data-ttu-id="ad48d-233">toomake módosításokat, válassza ki **vissza**, és írja be a kívánt hello beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ad48d-233">toomake changes, select **Back**, and then enter hello settings you want.</span></span> 

4. <span data-ttu-id="ad48d-234">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ad48d-234">Select **Connect**.</span></span>

5. <span data-ttu-id="ad48d-235">Miután csatlakozik, hello újonnan csatolt szolgáltatás alatt jelenik meg hello **(szolgáltatás SAS)** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ad48d-235">After it is attached, hello newly attached service is displayed under hello **(Service SAS)** node.</span></span>

    ![Eredménye tooa megosztott szolgáltatás csatolása a SAS használatával][20]

## <a name="search-for-storage-accounts"></a><span data-ttu-id="ad48d-237">Tárfiókok keresése</span><span class="sxs-lookup"><span data-stu-id="ad48d-237">Search for storage accounts</span></span>
<span data-ttu-id="ad48d-238">Ha storage-fiókok listája túl hosszú, egy adott tárfiókok gyorsan toolocate toouse hello keresőmezőbe hello tetején hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="ad48d-238">If you have a long list of storage accounts, a quick way toolocate a particular storage account is toouse hello search box at hello top of hello left pane.</span></span>

<span data-ttu-id="ad48d-239">Hello keresőmezőbe írja be, mert hello bal oldali ablak megjeleníti hello tárfiókokat jeleníti toothat pont be a megadott hello keresett érték.</span><span class="sxs-lookup"><span data-stu-id="ad48d-239">As you type in hello search box, hello left pane displays hello storage accounts that match hello search value you've entered up toothat point.</span></span> <span data-ttu-id="ad48d-240">Például egy keresési összes tárfiókot, amelynek a neve tartalmazza **tarcher** megjelenik-e a következő képernyőkép hello:</span><span class="sxs-lookup"><span data-stu-id="ad48d-240">For example, a search for all storage accounts whose name contains **tarcher** is shown in hello following screenshot:</span></span>

![Tárfiók keresése][11]

## <a name="next-steps"></a><span data-ttu-id="ad48d-242">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad48d-242">Next steps</span></span>
* [<span data-ttu-id="ad48d-243">Azure Blob Storage-erőforrások kezelése a Tártallózó (előzetes verzió) használatával</span><span class="sxs-lookup"><span data-stu-id="ad48d-243">Manage Azure Blob Storage resources with Storage Explorer (Preview)</span></span>](vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
[25]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-certificate-azure-stack.png
[26]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/export-root-cert-azure-stack.png
[27]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/import-azure-stack-cert-storage-explorer.png
[28]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-target-azure-stack.png
[29]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-azure-stack-account.png
[30]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-accounts-azure-stack.png
[31]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/azure-stack-storage-account-list.png
