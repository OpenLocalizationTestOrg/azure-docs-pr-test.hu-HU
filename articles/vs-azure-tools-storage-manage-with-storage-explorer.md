---
title: "Ismerkedés a Tártallózó alkalmazással (előzetes verzió) | Microsoft Docs"
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
ms.openlocfilehash: 1794a86a4185d587cf184a1f61a5720e2ab65e92
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-storage-explorer-preview"></a><span data-ttu-id="ec12c-103">Ismerkedés a Tártallózó alkalmazással (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="ec12c-103">Get started with Storage Explorer (Preview)</span></span>
## <a name="overview"></a><span data-ttu-id="ec12c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ec12c-104">Overview</span></span>
<span data-ttu-id="ec12c-105">Az Azure Tártallózó (előzetes verzió) egy különálló alkalmazás, amelynek segítségével egyszerűen dolgozhat Azure Storage-adatokkal Windows, macOS és Linux rendszereken.</span><span class="sxs-lookup"><span data-stu-id="ec12c-105">Azure Storage Explorer (Preview) is a standalone app that enables you to easily work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="ec12c-106">Ebben a cikkben megismerheti az Azure Storage-fiókok csatlakoztatásának és kezelésének különféle módjait.</span><span class="sxs-lookup"><span data-stu-id="ec12c-106">In this article, you learn the various ways of connecting to and managing your Azure storage accounts.</span></span>

![Microsoft Azure Tártallózó (előzetes verzió)][15]

## <a name="prerequisites"></a><span data-ttu-id="ec12c-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ec12c-108">Prerequisites</span></span>
* [<span data-ttu-id="ec12c-109">A Tártallózó (előzetes verzió) letöltése és telepítése</span><span class="sxs-lookup"><span data-stu-id="ec12c-109">Download and install Storage Explorer (Preview)</span></span>](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a><span data-ttu-id="ec12c-110">Csatlakozás egy tárfiókhoz vagy -szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="ec12c-110">Connect to a storage account or service</span></span>
<span data-ttu-id="ec12c-111">A Tártallózó (előzetes verzió) számos különféle módot kínál a tárfiókokhoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="ec12c-111">Storage Explorer (Preview) provides several ways to connect to storage accounts.</span></span> <span data-ttu-id="ec12c-112">Megteheti például a következőt:</span><span class="sxs-lookup"><span data-stu-id="ec12c-112">For example, you can:</span></span>
* <span data-ttu-id="ec12c-113">Csatlakozhat az Azure-előfizetéséhez kapcsolt tárfiókokhoz.</span><span class="sxs-lookup"><span data-stu-id="ec12c-113">Connect to storage accounts associated with your Azure subscriptions.</span></span>
* <span data-ttu-id="ec12c-114">Csatlakozhat olyan tárfiókokhoz és szolgáltatásokhoz, amelyek más Azure-előfizetésekből vannak megosztva.</span><span class="sxs-lookup"><span data-stu-id="ec12c-114">Connect to storage accounts and services that are shared from other Azure subscriptions.</span></span>
* <span data-ttu-id="ec12c-115">Csatlakozhat egy helyi tárolóhoz az Azure Storage Emulator használatával, és kezelheti azt.</span><span class="sxs-lookup"><span data-stu-id="ec12c-115">Connect to and manage local storage by using the Azure Storage Emulator.</span></span> 

<span data-ttu-id="ec12c-116">Emellett használhatja a tárfiókokat a globális és az országos Azure-ban:</span><span class="sxs-lookup"><span data-stu-id="ec12c-116">In addition, you can work with storage accounts in global and national Azure:</span></span>

* <span data-ttu-id="ec12c-117">[Csatlakozás Azure-előfizetéshez](#connect-to-an-azure-subscription): Az Azure-előfizetéséhez tartozó tárolási erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="ec12c-117">[Connect to an Azure subscription](#connect-to-an-azure-subscription): Manage storage resources that belong to your Azure subscription.</span></span>
* <span data-ttu-id="ec12c-118">[Munkavégzés helyi fejlesztési tárterülettel](#work-with-local-development-storage): Helyi tárterület kezelése az Azure Storage Emulator használatával.</span><span class="sxs-lookup"><span data-stu-id="ec12c-118">[Work with local development storage](#work-with-local-development-storage): Manage local storage by using the Azure Storage Emulator.</span></span>
* <span data-ttu-id="ec12c-119">[Külső tárterület csatolása](#attach-or-detach-an-external-storage-account): Más Azure-előfizetések vagy az országos Azure-felhők alá tartozó tárolási erőforrások kezelése a tárfiók fióknevének, kulcsának és végpontjainak használatával.</span><span class="sxs-lookup"><span data-stu-id="ec12c-119">[Attach to external storage](#attach-or-detach-an-external-storage-account): Manage storage resources that belong to another Azure subscription or that are under national Azure clouds by using the storage account's name, key, and endpoints.</span></span>
* <span data-ttu-id="ec12c-120">[Tárfiók csatolása SAS használatával](#attach-storage-account-using-sas): Más Azure-előfizetések alá tartozó tárolási erőforrások kezelése közös hozzáférésű jogosultságkód (SAS) használatával.</span><span class="sxs-lookup"><span data-stu-id="ec12c-120">[Attach a storage account by using an SAS](#attach-storage-account-using-sas): Manage storage resources that belong to another Azure subscription by using a shared access signature (SAS).</span></span>
* <span data-ttu-id="ec12c-121">[Szolgáltatás csatolása SAS használatával](#attach-service-using-sas): Más Azure-előfizetések alá tartozó adott tárolási szolgáltatás (blob tároló, üzenetsor vagy tábla) kezelése SAS használatával.</span><span class="sxs-lookup"><span data-stu-id="ec12c-121">[Attach a service by using an SAS](#attach-service-using-sas): Manage a specific storage service (blob container, queue, or table) that belongs to another Azure subscription by using an SAS.</span></span>

## <a name="connect-to-an-azure-subscription"></a><span data-ttu-id="ec12c-122">Csatlakozás Azure-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="ec12c-122">Connect to an Azure subscription</span></span>
> [!NOTE]
> <span data-ttu-id="ec12c-123">Ha nincs Azure-fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) vagy [aktiválhatja Visual Studio-előfizetése kiemelt előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="ec12c-123">If you don't have an Azure account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
>
>

1. <span data-ttu-id="ec12c-124">A Tártallózóban (előzetes verzió) válassza ki az **Azure-fiók beállításai** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-124">In Storage Explorer (Preview), select **Azure Account settings**.</span></span>

    ![Azure-fiók beállításai][0]

2. <span data-ttu-id="ec12c-126">A bal oldali ablaktábla megjeleníti az összes Microsoft-fiókot, amelybe bejelentkezett.</span><span class="sxs-lookup"><span data-stu-id="ec12c-126">The left pane displays all the Microsoft accounts you've signed in to.</span></span> <span data-ttu-id="ec12c-127">Ha másik fiókhoz szeretne csatlakozni, válassza a **Fiók hozzáadása** lehetőséget, és az útmutatásokat követve jelentkezzen be egy olyan Microsoft-fiókkal, amely legalább egy aktív Azure-előfizetéssel társítva van.</span><span class="sxs-lookup"><span data-stu-id="ec12c-127">To connect to another account, select **Add an account**, and then follow the instructions to sign in with a Microsoft account that is associated with at least one active Azure subscription.</span></span>

    >[!NOTE]
    ><span data-ttu-id="ec12c-128">Jelenleg nem támogatott a csatlakozás az országos Azure felhőkhöz (mint az Azure Germany, az Azure Government vagy az Azure China bejelentkezéssel).</span><span class="sxs-lookup"><span data-stu-id="ec12c-128">Connecting to national Azure (such as Azure Germany, Azure Government, and Azure China via sign-in) is currently not supported.</span></span> <span data-ttu-id="ec12c-129">Az országos Azure Storage-fiókokhoz való csatlakozással kapcsolatban lásd a „Külső tárfiók csatolása vagy leválasztása” szakaszt.</span><span class="sxs-lookup"><span data-stu-id="ec12c-129">See the "Attach or detach an external storage account" section for how to connect to national Azure storage accounts.</span></span>

3. <span data-ttu-id="ec12c-130">Amint sikeresen bejelentkezett egy Microsoft-fiókkal, a bal oldali ablaktáblán megjelenik a fiókhoz társított összes Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="ec12c-130">After you successfully sign in with a Microsoft account, the left pane is populated with the Azure subscriptions associated with that account.</span></span> <span data-ttu-id="ec12c-131">Válassza ki azt az Azure-előfizetést, amellyel dolgozni szeretne, majd válassza az **Alkalmaz** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-131">Select the Azure subscriptions that you want to work with, and then select **Apply**.</span></span> <span data-ttu-id="ec12c-132">(Az **Összes előfizetés** kiválasztásával kijelölheti az összes felsorolt Azure-előfizetést, vagy törölheti mindegyik jelölését.)</span><span class="sxs-lookup"><span data-stu-id="ec12c-132">(Selecting **All subscriptions** toggles selecting all or none of the listed Azure subscriptions.)</span></span>

    ![Azure-előfizetések kiválasztása][3]  
    <span data-ttu-id="ec12c-134">A bal oldali ablaktábla megjeleníti a kiválasztott Azure-előfizetésekhez társított összes tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="ec12c-134">The left pane displays the storage accounts associated with the selected Azure subscriptions.</span></span>

    ![Kiválasztott Azure-előfizetések][4]

## <a name="connect-to-an-azure-stack-subscription"></a><span data-ttu-id="ec12c-136">Csatlakozás Azure Stack-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="ec12c-136">Connect to an Azure Stack subscription</span></span>

<span data-ttu-id="ec12c-137">Az Azure Stack-előfizetéshez való csatlakozásról további információért lásd: [A Storage Explorer csatlakoztatása Azure Stack-előfizetéshez](azure-stack/azure-stack-storage-connect-se.md).</span><span class="sxs-lookup"><span data-stu-id="ec12c-137">For information about connecting to an Azure Stack subscription, see [Connect Storage Explorer to an Azure Stack subscription](azure-stack/azure-stack-storage-connect-se.md).</span></span>

## <a name="work-with-local-development-storage"></a><span data-ttu-id="ec12c-138">Munkavégzés helyi fejlesztési tárterülettel</span><span class="sxs-lookup"><span data-stu-id="ec12c-138">Work with local development storage</span></span>
<span data-ttu-id="ec12c-139">A Tártallózó (előzetes verzió) segítségével a helyi tárterületen is dolgozhat az Azure Storage Emulator használatával.</span><span class="sxs-lookup"><span data-stu-id="ec12c-139">With Storage Explorer (Preview), you can work against local storage by using the Azure Storage Emulator.</span></span> <span data-ttu-id="ec12c-140">Így anélkül is írhat kódot a tárterületre és tesztelheti azt, hogy szüksége lenne egy üzembe helyezett tárfiókra az Azure szolgáltatásban (mivel a tárfiókot az Azure Storage Emulator emulálja).</span><span class="sxs-lookup"><span data-stu-id="ec12c-140">This approach lets you write code against and test storage without necessarily having a storage account deployed on Azure, because the storage account is being emulated by the Azure Storage Emulator.</span></span>

> [!NOTE]
> <span data-ttu-id="ec12c-141">Az Azure Storage Emulator jelenleg kizárólag Windows rendszeren támogatott.</span><span class="sxs-lookup"><span data-stu-id="ec12c-141">The Azure Storage Emulator is currently supported only for Windows.</span></span>
>
>

1. <span data-ttu-id="ec12c-142">A Tártallózó (előzetes verzió) bal oldali ablaktábláján, bontsa ki a **(Helyi és csatolt)** > **Tárfiókok** > **(Fejlesztés)** csomópontját.</span><span class="sxs-lookup"><span data-stu-id="ec12c-142">In the left pane of Storage Explorer (Preview), expand the **(Local and Attached)** > **Storage Accounts** > **(Development)** node.</span></span>

    ![Helyi fejlesztési csomópont][21]

2. <span data-ttu-id="ec12c-144">Ha az Azure Storage Emulator még nincs telepítve, a rendszer az információs sávon kéri erre.</span><span class="sxs-lookup"><span data-stu-id="ec12c-144">If you have not yet installed the Azure Storage Emulator, you are prompted to do so via an infobar.</span></span> <span data-ttu-id="ec12c-145">Ha az információs sáv megjelenik, válassza a **Legújabb verzió letöltése** lehetőséget, és telepítse az emulátort.</span><span class="sxs-lookup"><span data-stu-id="ec12c-145">If the infobar is displayed, select **Download the latest version**, and then install the emulator.</span></span>

    ![Azure Storage Emulator letöltése prompt][22]

3. <span data-ttu-id="ec12c-147">Miután telepítette az emulátort, létrehozhat helyi blobokat, üzenetsorokat és táblákat, és kezelheti őket.</span><span class="sxs-lookup"><span data-stu-id="ec12c-147">After the emulator is installed, you can create and work with local blobs, queues, and tables.</span></span> <span data-ttu-id="ec12c-148">Az egyes tárfióktípusok kezelésével kapcsolatos információkért kattintson az alábbi hivatkozások egyikére:</span><span class="sxs-lookup"><span data-stu-id="ec12c-148">To learn how to work with each storage account type, see one of the following:</span></span>

    * [<span data-ttu-id="ec12c-149">Azure Blob Storage-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="ec12c-149">Manage Azure blob storage resources</span></span>](vs-azure-tools-storage-explorer-blobs.md)
    * <span data-ttu-id="ec12c-150">Azure File Share Storage-erőforrások kezelése: *Hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ec12c-150">Manage Azure file share storage resources: *Coming soon*</span></span>
    * <span data-ttu-id="ec12c-151">Azure Queue Storage-erőforrások kezelése: *Hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ec12c-151">Manage Azure queue storage resources: *Coming soon*</span></span>
    * <span data-ttu-id="ec12c-152">Azure Table Storage-erőforrások kezelése: *Hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ec12c-152">Manage Azure table storage resources: *Coming soon*</span></span>

## <a name="attach-or-detach-an-external-storage-account"></a><span data-ttu-id="ec12c-153">Külső tárfiók csatolása vagy leválasztása</span><span class="sxs-lookup"><span data-stu-id="ec12c-153">Attach or detach an external storage account</span></span>
<span data-ttu-id="ec12c-154">A Tártallózó (előzetes verzió) segítségével külső tárfiókokat csatolhat, így azok könnyen megoszthatók.</span><span class="sxs-lookup"><span data-stu-id="ec12c-154">With Storage Explorer (Preview), you can attach to external storage accounts so that storage accounts can be easily shared.</span></span> <span data-ttu-id="ec12c-155">Ez a szakasz külső tárfiókok csatolását (és leválasztását) írja le.</span><span class="sxs-lookup"><span data-stu-id="ec12c-155">This section explains how to attach to (and detach from) external storage accounts.</span></span>

### <a name="get-the-storage-account-credentials"></a><span data-ttu-id="ec12c-156">Tárfiók hitelesítő adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="ec12c-156">Get the storage account credentials</span></span>
<span data-ttu-id="ec12c-157">A külső tárfiókok megosztásához először az adott fiók tulajdonosának le kell kérnie a fiók hitelesítő adatait (a fióknevet és a kulcsot), majd meg kell osztania azokat a (külső) fiókot csatlakoztatni kívánó személlyel.</span><span class="sxs-lookup"><span data-stu-id="ec12c-157">To share an external storage account, the owner of that account must first get the credentials (account name and key) for the account and then share that information with the person who wants to attach to that (external) account.</span></span> <span data-ttu-id="ec12c-158">A tárfiók hitelesítő adatainak lekérése az Azure Portalon keresztül lehetséges az alábbi lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="ec12c-158">You can obtain the storage account credentials via the Azure portal by doing the following:</span></span>

1. <span data-ttu-id="ec12c-159">Jelentkezzen be az [Azure portálra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ec12c-159">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="ec12c-160">Válassza a **Tallózás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-160">Select **Browse**.</span></span>

3. <span data-ttu-id="ec12c-161">Válassza a **Tárfiókok** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-161">Select **Storage Accounts**.</span></span>

4. <span data-ttu-id="ec12c-162">A **Tárfiókok** panelen válassza ki a kívánt tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="ec12c-162">On the **Storage Accounts** blade, select the desired storage account.</span></span>

5. <span data-ttu-id="ec12c-163">A kiválasztott tárfiók **Beállítások** panelén válassza a **Hozzáférési kulcs** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-163">On the **Settings** blade for the selected storage account, select **Access keys**.</span></span>

    ![Hozzáférési kulcs lehetőség][5]

6. <span data-ttu-id="ec12c-165">A **Hozzáférési kulcs** panelen másolja ki a **Tárfiók neve** és a **kulcs1** értékeket a tárfiók csatolásához.</span><span class="sxs-lookup"><span data-stu-id="ec12c-165">On the **Access keys** blade, copy the **Storage account name** and **key1** values for use when attaching to the storage account.</span></span>

    ![Elérési kulcs][6]

### <a name="attach-to-an-external-storage-account"></a><span data-ttu-id="ec12c-167">Külső tárfiók csatolása</span><span class="sxs-lookup"><span data-stu-id="ec12c-167">Attach to an external storage account</span></span>
<span data-ttu-id="ec12c-168">Külső tárfiók csatolásához szükség van a fiók nevére és kulcsára.</span><span class="sxs-lookup"><span data-stu-id="ec12c-168">To attach to an external storage account, you need the account's name and key.</span></span> <span data-ttu-id="ec12c-169">A „Tárfiók hitelesítő adatainak lekérése” szakasz ismerteti ezen értékek lekérését az Azure Portalról.</span><span class="sxs-lookup"><span data-stu-id="ec12c-169">The "Get the storage account credentials" section explains how to obtain these values from the Azure portal.</span></span> <span data-ttu-id="ec12c-170">A portálon azonban a fiókkulcs neve: **kulcs1**.</span><span class="sxs-lookup"><span data-stu-id="ec12c-170">However, in the portal, the account key is called **key1**.</span></span> <span data-ttu-id="ec12c-171">Tehát ahol a Tártallózó (előzetes verzió) fiókkulcsot kér, a **kulcs1** értéket kell megadni.</span><span class="sxs-lookup"><span data-stu-id="ec12c-171">So where Storage Explorer (Preview) asks for an account key, you enter the **key1** value.</span></span>

1. <span data-ttu-id="ec12c-172">A Tártallózóban (előzetes verzió) válassza ki a **Csatlakozás Azure Storage-hoz** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-172">In Storage Explorer (Preview), select **Connect to Azure storage**.</span></span>

    ![Csatlakozás Azure Storage-hoz lehetőség][23]

2. <span data-ttu-id="ec12c-174">A **Csatlakozás Azure Storage-hoz** párbeszédpanelen adja meg a fiókkulcsot (a **kulcs1** értéket az Azure Portalról), majd válassza a **Tovább** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-174">In the **Connect to Azure Storage** dialog box, specify the account key (the **key1** value from the Azure portal), and then select **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec12c-175">Megadhatja az országos Azure-on található tárfiók tárkapcsolati karakterláncát.</span><span class="sxs-lookup"><span data-stu-id="ec12c-175">You can enter the storage connection string from a storage account on national Azure.</span></span> <span data-ttu-id="ec12c-176">Például a következőhöz hasonló kapcsolati karakterláncokat megadva csatlakozhat az Azure Germany-tárfiókokhoz:</span><span class="sxs-lookup"><span data-stu-id="ec12c-176">For example, to connect to Azure Germany storage accounts, enter connection strings similar to the following:</span></span> 
    >
    >* <span data-ttu-id="ec12c-177">DefaultEndpointsProtocol=https</span><span class="sxs-lookup"><span data-stu-id="ec12c-177">DefaultEndpointsProtocol=https</span></span>
    >* <span data-ttu-id="ec12c-178">AccountName=cawatest03</span><span class="sxs-lookup"><span data-stu-id="ec12c-178">AccountName=cawatest03</span></span>
    >* <span data-ttu-id="ec12c-179">AccountKey=<tárfiók kulcsa></span><span class="sxs-lookup"><span data-stu-id="ec12c-179">AccountKey=<storage_account_key></span></span>
    >* <span data-ttu-id="ec12c-180">EndpointSuffix=core.cloudapi.de</span><span class="sxs-lookup"><span data-stu-id="ec12c-180">EndpointSuffix=core.cloudapi.de</span></span>
    
    ><span data-ttu-id="ec12c-181">A „Tárfiók hitelesítő adatainak lekérése” szakaszban leírtak szerint kérheti le a kapcsolati karakterláncot az Azure Portalról.</span><span class="sxs-lookup"><span data-stu-id="ec12c-181">You can get the connection string from the Azure portal in the same way as described in the "Get the storage account credentials" section.</span></span>

    ![Csatlakozás az Azure Storage-hoz párbeszédpanel][24]

3. <span data-ttu-id="ec12c-183">A **Külső tárterület csatolása** párbeszédpanelen adja meg a tárfiók nevét a **Fióknév** mezőben, adja meg a további kívánt beállításokat, majd ha elkészült, válassza a **Tovább** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-183">In the **Attach External Storage** dialog box, in the **Account name** box, enter the storage account name, specify any other desired settings, and then select **Next**.</span></span>

    ![Külső tárterület csatolása párbeszédpanel][8]

4. <span data-ttu-id="ec12c-185">A **Kapcsolatok összegzése** párbeszédpanelen ellenőrizze az adatokat.</span><span class="sxs-lookup"><span data-stu-id="ec12c-185">In the **Connection Summary** dialog box, verify the information.</span></span> <span data-ttu-id="ec12c-186">Ha bármin változtatni szeretne, válassza a **Vissza** lehetőséget, és írja be újra a kívánt beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ec12c-186">If you want to change anything, select **Back** and reenter the desired settings.</span></span> 

5. <span data-ttu-id="ec12c-187">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec12c-187">Select **Connect**.</span></span>

6. <span data-ttu-id="ec12c-188">A sikeres csatlakozást követően a külső tárfiók a nevét követő **(Külső)** jelöléssel jelenik meg a tárfiókok között.</span><span class="sxs-lookup"><span data-stu-id="ec12c-188">After it is successfully connected, the external storage account is displayed with **(External)** appended to the storage account name.</span></span>

    ![Külső tárfiók csatolásának eredménye][9]

### <a name="detach-from-an-external-storage-account"></a><span data-ttu-id="ec12c-190">Külső tárfiók leválasztása</span><span class="sxs-lookup"><span data-stu-id="ec12c-190">Detach from an external storage account</span></span>
1. <span data-ttu-id="ec12c-191">Kattintson a jobb gombbal a leválasztani kívánt külső tárfiókra, és válassza a **Leválasztás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-191">Right-click the external storage account that you want to detach, and then select **Detach**.</span></span>

    ![Tár leválasztása lehetőség][10]

2. <span data-ttu-id="ec12c-193">A megerősítő üzenetben kattintson az **Igen** gombra a külső tárfiók leválasztásának jóváhagyásához.</span><span class="sxs-lookup"><span data-stu-id="ec12c-193">In the confirmation message, select **Yes** to confirm the detachment from the external storage account.</span></span>

## <a name="attach-a-storage-account-by-using-an-sas"></a><span data-ttu-id="ec12c-194">Tárfiók csatolása SAS használatával</span><span class="sxs-lookup"><span data-stu-id="ec12c-194">Attach a storage account by using an SAS</span></span>
<span data-ttu-id="ec12c-195">Az [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) lehetővé teszi, hogy az Azure-előfizetés rendszergazdája ideiglenes hozzáférést engedélyezzen a tárfiókhoz anélkül, hogy kiadná az Azure-előfizetés hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="ec12c-195">An [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) lets the admin of an Azure subscription grant temporary access to a storage account without having to provide Azure subscription credentials.</span></span>

<span data-ttu-id="ec12c-196">E forgatókönyv szemléltetésére tegyük fel, hogy az „A” felhasználó valamely Azure-előfizetés rendszergazdája, és hozzáférést szeretne engedélyezni „B” felhasználó számára a tárfiókhoz adott időtartamra és meghatározott engedélyekkel:</span><span class="sxs-lookup"><span data-stu-id="ec12c-196">To illustrate this scenario, let's say that UserA is an admin of an Azure subscription, and UserA wants to allow UserB to access a storage account for a limited time with certain permissions:</span></span>

1. <span data-ttu-id="ec12c-197">Az „A” felhasználó létrehoz egy SAS-kódot (amely a tárfiók kapcsolati karakterlánca) egy adott időtartamra és a kívánt engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="ec12c-197">UserA generates an SAS (consisting of the connection string for the storage account) for a specific time period and with the desired permissions.</span></span>

2. <span data-ttu-id="ec12c-198">Az „A” felhasználó megosztja a SAS-kódot a tárfiókhoz hozzáférést igénylő személlyel (esetünkben a „B” felhasználóval).</span><span class="sxs-lookup"><span data-stu-id="ec12c-198">UserA shares the SAS with the person (UserB, in our example) who wants access to the storage account.</span></span>  

3. <span data-ttu-id="ec12c-199">A „B” felhasználó a Tártallózó (előzetes verzió) segítségével csatolja az „A” felhasználóhoz tartozó fiókot a megadott SAS-kód használatával.</span><span class="sxs-lookup"><span data-stu-id="ec12c-199">UserB uses Storage Explorer (Preview) to attach to the account that belongs to UserA by using the supplied SAS.</span></span>

### <a name="get-an-sas-for-the-account-you-want-to-share"></a><span data-ttu-id="ec12c-200">SAS-kód lekérése a megosztani kívánt fiókhoz</span><span class="sxs-lookup"><span data-stu-id="ec12c-200">Get an SAS for the account you want to share</span></span>
1. <span data-ttu-id="ec12c-201">A Tártallózóban (előzetes verzió) kattintson a jobb gombbal a megosztani kívánt tárfiókra, és válassza a **Közös hozzáférésű jogosultságkód igénylése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-201">In Storage Explorer (Preview), right-click the storage account you want share, and then select **Get Shared Access Signature**.</span></span>

    ![SAS beszerzése menüpont][13]

2. <span data-ttu-id="ec12c-203">A **Közös hozzáférésű jogosultságkód** párbeszédpanelen adja meg a kívánt időtartamot és engedélyeket a fiókhoz, és kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec12c-203">In the **Shared Access Signature** dialog box, specify the time frame and permissions that you want for the account, and then select **Create**.</span></span>

    <span data-ttu-id="ec12c-204">![SAS beszerzése párbeszédpanel][14]</span><span class="sxs-lookup"><span data-stu-id="ec12c-204">![Get SAS dialog box][14]</span></span>  
    <span data-ttu-id="ec12c-205">A **Közös hozzáférésű jogosultságkód** párbeszédpanel megjeleníti a SAS kódot.</span><span class="sxs-lookup"><span data-stu-id="ec12c-205">The **Shared Access Signature** dialog box opens and displays the SAS.</span></span>

3. <span data-ttu-id="ec12c-206">Kattintson a **Kapcsolati karakterlánc** mellett a **Másolás** parancsra annak a vágólapra másolásához, majd válassza a **Bezárás** elemet.</span><span class="sxs-lookup"><span data-stu-id="ec12c-206">Next to the **Connection String**, select **Copy** to copy it to the clipboard, and then select **Close**.</span></span>

### <a name="attach-to-the-shared-account-by-using-the-sas"></a><span data-ttu-id="ec12c-207">Közös fiók csatolása a SAS használatával</span><span class="sxs-lookup"><span data-stu-id="ec12c-207">Attach to the shared account by using the SAS</span></span>
1. <span data-ttu-id="ec12c-208">A Tártallózóban (előzetes verzió) válassza ki a **Csatlakozás Azure Storage-hoz** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-208">In Storage Explorer (Preview), select **Connect to Azure storage**.</span></span>

    ![Csatlakozás Azure Storage-hoz lehetőség][23]

2. <span data-ttu-id="ec12c-210">A **Csatlakozás az Azure Storage-hoz** párbeszédpanelen adja meg a kapcsolati karakterláncot, majd válassza a **Tovább** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-210">In the **Connect to Azure Storage** dialog box, specify the connection string, and then select **Next**.</span></span>

    ![Csatlakozás az Azure Storage-hoz párbeszédpanel][24]

3. <span data-ttu-id="ec12c-212">A **Kapcsolatok összegzése** párbeszédpanelen ellenőrizze az adatokat.</span><span class="sxs-lookup"><span data-stu-id="ec12c-212">In the **Connection Summary** dialog box, verify the information.</span></span> <span data-ttu-id="ec12c-213">A változtatáshoz válassza ki a **Vissza** elemet, majd adja meg a kívánt beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ec12c-213">To make changes, select **Back**, and then enter the settings you want.</span></span> 

4. <span data-ttu-id="ec12c-214">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec12c-214">Select **Connect**.</span></span>

5. <span data-ttu-id="ec12c-215">A csatolást követően a tárfiók a megadott fióknevet követő **(SAS)** megjelöléssel jelenik meg a tárfiókok közt.</span><span class="sxs-lookup"><span data-stu-id="ec12c-215">After it is attached, the storage account is displayed with **(SAS)** appended to the account name that you supplied.</span></span>

    ![SAS használatával végzett fiókhoz csatolás eredménye][17]

## <a name="attach-a-service-by-using-an-sas"></a><span data-ttu-id="ec12c-217">Szolgáltatás csatolása SAS használatával</span><span class="sxs-lookup"><span data-stu-id="ec12c-217">Attach a service by using an SAS</span></span>
<span data-ttu-id="ec12c-218">A „Tárfiók csatolása SAS használatával” szakasz mutatja be, hogyan adhat az Azure-előfizetés rendszergazdája ideiglenes hozzáférést a tárfiókhoz a tárfiók SAS-kódjának létrehozásával és megosztásával.</span><span class="sxs-lookup"><span data-stu-id="ec12c-218">The "Attach a storage account by using an SAS" section explains how an Azure subscription admin can grant temporary access to a storage account by generating and sharing an SAS for the storage account.</span></span> <span data-ttu-id="ec12c-219">Hasonlóképpen SAS-kód létrehozható adott szolgáltatásokhoz (blob tárolókhoz, üzenetsorokhoz és táblákhoz) is a tárfiókon belül.</span><span class="sxs-lookup"><span data-stu-id="ec12c-219">Similarly, an SAS can be generated for a specific service (blob container, queue, or table) within a storage account.</span></span>  

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a><span data-ttu-id="ec12c-220">SAS-kód létrehozása a megosztani kívánt fiókhoz</span><span class="sxs-lookup"><span data-stu-id="ec12c-220">Generate an SAS for the service that you want to share</span></span>
<span data-ttu-id="ec12c-221">Ebben a kontextusban a szolgáltatások blob tárolók, üzenetsorok vagy táblák lehetnek.</span><span class="sxs-lookup"><span data-stu-id="ec12c-221">In this context, a service can be a blob container, queue, or table.</span></span> <span data-ttu-id="ec12c-222">SAS létrehozása listázott szolgáltatáshoz az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ec12c-222">To generate the SAS for a listed service, see:</span></span>

* [<span data-ttu-id="ec12c-223">SAS lekérése blob tárolóhoz</span><span class="sxs-lookup"><span data-stu-id="ec12c-223">Get the SAS for a blob container</span></span>](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* <span data-ttu-id="ec12c-224">SAS lekérése fájlmegosztáshoz: *Hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ec12c-224">Get the SAS for a file share: *Coming soon*</span></span>
* <span data-ttu-id="ec12c-225">SAS lekérése üzenetsorhoz: *Hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ec12c-225">Get the SAS for a queue: *Coming soon*</span></span>
* <span data-ttu-id="ec12c-226">SAS lekérése táblához: *Hamarosan elérhető*</span><span class="sxs-lookup"><span data-stu-id="ec12c-226">Get the SAS for a table: *Coming soon*</span></span>

### <a name="attach-to-the-shared-account-service-by-using-the-sas"></a><span data-ttu-id="ec12c-227">Közös fiókszolgáltatás csatolása SAS használatával</span><span class="sxs-lookup"><span data-stu-id="ec12c-227">Attach to the shared account service by using the SAS</span></span>
1. <span data-ttu-id="ec12c-228">A Tártallózóban (előzetes verzió) válassza ki a **Csatlakozás Azure Storage-hoz** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-228">In Storage Explorer (Preview), select **Connect to Azure storage**.</span></span>

    ![Csatlakozás Azure Storage-hoz lehetőség][23]

2. <span data-ttu-id="ec12c-230">A **Csatlakozás az Azure Storage-hoz** párbeszédpanelen adja meg a SAS URI-t, majd válassza a **Tovább** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec12c-230">In the **Connect to Azure Storage** dialog box, specify the SAS URI, and then select **Next**.</span></span>

    ![Csatlakozás az Azure Storage-hoz párbeszédpanel][24]

3. <span data-ttu-id="ec12c-232">A **Kapcsolatok összegzése** párbeszédpanelen ellenőrizze az adatokat.</span><span class="sxs-lookup"><span data-stu-id="ec12c-232">In the **Connection Summary** dialog box, verify the information.</span></span> <span data-ttu-id="ec12c-233">A változtatáshoz válassza ki a **Vissza** elemet, majd adja meg a kívánt beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ec12c-233">To make changes, select **Back**, and then enter the settings you want.</span></span> 

4. <span data-ttu-id="ec12c-234">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec12c-234">Select **Connect**.</span></span>

5. <span data-ttu-id="ec12c-235">A csatolást követően az újonnan csatolt szolgáltatás a **(Szolgáltatás SAS)** csomópont alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ec12c-235">After it is attached, the newly attached service is displayed under the **(Service SAS)** node.</span></span>

    ![SAS használatával megosztott szolgáltatáshoz végzett csatolás eredménye][20]

## <a name="search-for-storage-accounts"></a><span data-ttu-id="ec12c-237">Tárfiókok keresése</span><span class="sxs-lookup"><span data-stu-id="ec12c-237">Search for storage accounts</span></span>
<span data-ttu-id="ec12c-238">Amennyiben tárfiókjai listája túl hosszú, az adott tárfiókok megtalálásának egyszerű módja lehet a keresőmező használata a bal oldali ablaktábla tetején.</span><span class="sxs-lookup"><span data-stu-id="ec12c-238">If you have a long list of storage accounts, a quick way to locate a particular storage account is to use the search box at the top of the left pane.</span></span>

<span data-ttu-id="ec12c-239">Ahogy elkezdi beírni a szöveget a keresőmezőbe, a bal oldali ablaktábla csak azokat a tárfiókokat jeleníti meg, amelyek tartalmazzák az addig beírt szövegre adott találatokat.</span><span class="sxs-lookup"><span data-stu-id="ec12c-239">As you type in the search box, the left pane displays the storage accounts that match the search value you've entered up to that point.</span></span> <span data-ttu-id="ec12c-240">Például az alábbi képernyőfotón látható egy olyan keresés, amely a **tarcher** karakterláncot keresi az összes tárfiók nevében:</span><span class="sxs-lookup"><span data-stu-id="ec12c-240">For example, a search for all storage accounts whose name contains **tarcher** is shown in the following screenshot:</span></span>

![Tárfiók keresése][11]

## <a name="next-steps"></a><span data-ttu-id="ec12c-242">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ec12c-242">Next steps</span></span>
* [<span data-ttu-id="ec12c-243">Azure Blob Storage-erőforrások kezelése a Tártallózó (előzetes verzió) használatával</span><span class="sxs-lookup"><span data-stu-id="ec12c-243">Manage Azure Blob Storage resources with Storage Explorer (Preview)</span></span>](vs-azure-tools-storage-explorer-blobs.md)

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
