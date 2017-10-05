---
title: "Az Azure Storage-fiók listája"
description: "Az Azure-eszközkészlet használata az eclipse-ben a tárolási Fiókbeállítások kezelése"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: bbacfcd8-dbf5-4265-a930-59f508de5325
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: f859efa389d3fe0b4b7b16255d57f1aa13123319
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-storage-account-list"></a><span data-ttu-id="d98c9-103">Az Azure Storage-fiók listája</span><span class="sxs-lookup"><span data-stu-id="d98c9-103">Azure Storage Account List</span></span>
<span data-ttu-id="d98c9-104">Az Azure storage-fiókok a JDK, a kiszolgáló és a tetszőleges összetevők, valamint állapot tárolására, gyorsítótárazás használata esetén használandó letöltési helyeiről engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d98c9-104">Azure storage accounts enable download locations to be used for your JDK, application server, and arbitrary components, as well as for storing state when using caching.</span></span> <span data-ttu-id="d98c9-105">Eclipse-ben, amely a projektekhez az Eclipse munkaterületen elérhető ismert storage-fiókok listáját.</span><span class="sxs-lookup"><span data-stu-id="d98c9-105">Eclipse maintains a list of known storage accounts that are available to your projects in your Eclipse workspace.</span></span> <span data-ttu-id="d98c9-106">Megnyitásához a **Tárfiókok** párbeszédpanel, amely segítségével kezelhető a lista belül eclipse-ben kattintson **ablak**, kattintson a **beállítások**, bontsa ki a **Azure**, és kattintson a **Tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="d98c9-106">To open the **Storage Accounts** dialog, which is used to manage that list, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Storage Accounts**.</span></span>

<span data-ttu-id="d98c9-107">Az alábbi látható a **Tárfiókok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d98c9-107">The following shows the **Storage Accounts** dialog.</span></span>

![][ic719496]

<span data-ttu-id="d98c9-108">Ezen a párbeszédpanelen is megnyithatja az egy **fiókok** párbeszédpanelek, amelyek használják a storage-fiókok, például a következő hivatkozásra:</span><span class="sxs-lookup"><span data-stu-id="d98c9-108">This dialog can also be opened from an **Accounts** link on dialog boxes that use storage accounts, such as the following:</span></span>

* <span data-ttu-id="d98c9-109">A **JDK** lapján a **kiszolgálókonfiguráció** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d98c9-109">The **JDK** tab of the **Server Configuration** dialog.</span></span>
* <span data-ttu-id="d98c9-110">A **Server** lapján a **kiszolgálókonfiguráció** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d98c9-110">The **Server** tab of the **Server Configuration** dialog.</span></span>
* <span data-ttu-id="d98c9-111">A **összetevő felvétele** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d98c9-111">The **Add Component** dialog.</span></span>
* <span data-ttu-id="d98c9-112">A **gyorsítótárazását** tulajdonságainak párbeszédpanelét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d98c9-112">The **Caching** properties dialog.</span></span>

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a><span data-ttu-id="d98c9-113">A storage-fiókok használatával egy közzétételi beállítások fájlja importálása</span><span class="sxs-lookup"><span data-stu-id="d98c9-113">To import your storage accounts using a publish settings file</span></span>
1. <span data-ttu-id="d98c9-114">Belül a **Tárfiókok** párbeszédpanel, kattintson a **közzététele beállításfájl importálás**.</span><span class="sxs-lookup"><span data-stu-id="d98c9-114">Within the **Storage Accounts** dialog, click **Import from PUBLISH-SETTINGS file**.</span></span>

2. <span data-ttu-id="d98c9-115">(Kihagyhatja ezt a lépést, ha már mentette a közzétételi beállítások fájlja a helyi számítógépen.) Az a **importálási előfizetési adatok** párbeszédpanel, kattintson a **KÖZZÉTÉTELI-beállítások fájl letöltése**.</span><span class="sxs-lookup"><span data-stu-id="d98c9-115">(Skip this step if you have already saved a publish settings file to your local machine.) In the **Import Subscription Information** dialog, click **Download PUBLISH-SETTINGS File**.</span></span> <span data-ttu-id="d98c9-116">Ha még nem jelentkezik be az Azure-fiókjába, fogja kérni fogja a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="d98c9-116">If you are not yet logged into your Azure account, you will be prompted to log in.</span></span> <span data-ttu-id="d98c9-117">Majd kéri az Azure mentési beállítások fájl közzététele.</span><span class="sxs-lookup"><span data-stu-id="d98c9-117">Then you'll be prompted to save an Azure publish settings file.</span></span> <span data-ttu-id="d98c9-118">(Az eredményül kapott utasításokat a bejelentkezési oldalon - figyelmen kívül hagyhatja ezeket az Azure portál által szolgáltatott és Visual Studio felhasználóknak.) Mentse a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d98c9-118">(You can ignore the resulting instructions shown on the logon pages - they are provided by the Azure portal and are intended for Visual Studio users.) Save it to your local machine.</span></span>

3. <span data-ttu-id="d98c9-119">Még mindig a **importálási előfizetési adatok** párbeszédpanel, kattintson a **Tallózás** gombra, válassza ki a korábban mentett helyileg közzétételi beállítások fájlja, majd **nyissa meg a**.</span><span class="sxs-lookup"><span data-stu-id="d98c9-119">Still in the **Import Subscription Information** dialog, click the **Browse** button, select the publish settings file that you saved locally previously, and then click **Open**.</span></span>

4. <span data-ttu-id="d98c9-120">Kattintson a **OK** bezárásához a **importálási előfizetési adatok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d98c9-120">Click **OK** to close the **Import Subscription Information** dialog.</span></span>

## <a name="to-create-a-new-storage-account"></a><span data-ttu-id="d98c9-121">Egy új tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="d98c9-121">To create a new storage account</span></span>
1. <span data-ttu-id="d98c9-122">Belül a **Tárfiókok** párbeszédpanel, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d98c9-122">Within the **Storage Accounts** dialog, click **Add**.</span></span>

2. <span data-ttu-id="d98c9-123">Belül a **Tárfiók hozzáadása** párbeszédpanel, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="d98c9-123">Within the **Add Storage Account** dialog, click **New**.</span></span>

3. <span data-ttu-id="d98c9-124">Belül a **új Tárfiók** párbeszédpanelen adja meg a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="d98c9-124">Within the **New Storage Account** dialog, specify values for the following:</span></span>

   * <span data-ttu-id="d98c9-125">Tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="d98c9-125">Storage account name.</span></span>

   * <span data-ttu-id="d98c9-126">A tárfiók helye.</span><span class="sxs-lookup"><span data-stu-id="d98c9-126">Location of the storage account.</span></span>

   * <span data-ttu-id="d98c9-127">A tárfiók leírását.</span><span class="sxs-lookup"><span data-stu-id="d98c9-127">Description of the storage account.</span></span>

   * <span data-ttu-id="d98c9-128">Az előfizetést, amelyhez a storage-fiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="d98c9-128">The subscription to which the storage account belongs.</span></span>

4. <span data-ttu-id="d98c9-129">Kattintson a **OK** bezárásához a **új Tárfiók** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d98c9-129">Click **OK** to close the **New Storage Account** dialog.</span></span>

<span data-ttu-id="d98c9-130">A tárfiók keletkezik több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="d98c9-130">It may take several minutes for your storage account to be created.</span></span> <span data-ttu-id="d98c9-131">Létrehozása után kattintson **OK** bezárásához a **Tárfiók hozzáadása** párbeszédpanel, és az új tárfiók nem kerülnek be a rendelkezésre álló tár fiókok listáját.</span><span class="sxs-lookup"><span data-stu-id="d98c9-131">After it is created, click **OK** to close the **Add Storage Account** dialog, and your new storage account will be added to the list of available storage accounts.</span></span>

## <a name="to-add-an-existing-storage-account-to-the-list"></a><span data-ttu-id="d98c9-132">Meglévő tárfiók hozzáadása a listához</span><span class="sxs-lookup"><span data-stu-id="d98c9-132">To add an existing storage account to the list</span></span>
1. <span data-ttu-id="d98c9-133">Ha még nem rendelkezik Azure-tárfiókot, hozzon létre egyet a felsorolt lépéseket követve a **létrehozni egy új tárolási fiók szakasz** felett.</span><span class="sxs-lookup"><span data-stu-id="d98c9-133">If you do not already have a Azure storage account, create one by following the steps listed in the **To create a new storage account section** above.</span></span> <span data-ttu-id="d98c9-134">(Másik lehetőségként létrehozhat egy új tárfiókot, a [Azure felügyeleti portálon][Azure Management Portal].)</span><span class="sxs-lookup"><span data-stu-id="d98c9-134">(Alternatively, you can create a new storage account at the [Azure Management Portal][Azure Management Portal].)</span></span>

2. <span data-ttu-id="d98c9-135">Belül a **Tárfiókok** párbeszédpanel, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d98c9-135">Within the **Storage Accounts** dialog, click **Add**.</span></span>

3. <span data-ttu-id="d98c9-136">Belül a **Tárfiók hozzáadása** párbeszédpanelen adja meg az értékeket **neve** és **hozzáférési kulcs**.</span><span class="sxs-lookup"><span data-stu-id="d98c9-136">Within the **Add Storage Account** dialog, enter values for **Name** and **Access Key**.</span></span> <span data-ttu-id="d98c9-137">A fiók nevét és hívóbetűjét egy meglévő Azure-tárfiókot kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d98c9-137">The account name and access key must be for an existing Azure storage account.</span></span> <span data-ttu-id="d98c9-138">Használja a **tárolási** szakasza a [Azure felügyeleti portálon] [ Azure Management Portal] a tárfiókok neve és kulcsok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="d98c9-138">Use the **Storage** section of the [Azure Management Portal][Azure Management Portal] to view your storage account names and keys.</span></span> <span data-ttu-id="d98c9-139">A **Tárfiók hozzáadása** párbeszédpanel az alábbihoz hasonlóan fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="d98c9-139">Your **Add Storage Account** dialog will look similar to the following.</span></span>
   
   ![][ic719497]

4. <span data-ttu-id="d98c9-140">Kattintson a **OK** bezárásához a **Tárfiók hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d98c9-140">Click **OK** to close the **Add Storage Account** dialog.</span></span>

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a><span data-ttu-id="d98c9-141">Új hozzáférési kulcs használatához a tárfiók módosítása</span><span class="sxs-lookup"><span data-stu-id="d98c9-141">To modify a storage account to use a new access key</span></span>
1. <span data-ttu-id="d98c9-142">Belül a **Tárfiókok** párbeszédpanel, kattintson a tároló fiókra szerkesztése szeretne, és kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="d98c9-142">Within the **Storage Accounts** dialog, click the storage account that you want to edit and then click **Edit**.</span></span>

2. <span data-ttu-id="d98c9-143">Belül a **szerkesztése Tárfiók_elérési_kulcsa** párbeszédpanelen módosítsa a **hozzáférési kulcs** érték.</span><span class="sxs-lookup"><span data-stu-id="d98c9-143">Within the **Edit Storage Account Access Key** dialog, modify the **Access Key** value.</span></span>

3. <span data-ttu-id="d98c9-144">Kattintson a **OK** bezárásához a **szerkesztése Tárfiók_elérési_kulcsa** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d98c9-144">Click **OK** to close the **Edit Storage Account Access Key** dialog.</span></span>

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a><span data-ttu-id="d98c9-145">A tárfiók eltávolítása a listáról, az eclipse-ben karbantartása</span><span class="sxs-lookup"><span data-stu-id="d98c9-145">To remove a storage account from the list maintained in Eclipse</span></span>
1. <span data-ttu-id="d98c9-146">Belül a **Tárfiókok** párbeszédpanel, kattintson a tároló fiókra szerkesztése szeretne, és kattintson **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="d98c9-146">Within the **Storage Accounts** dialog, click the storage account that you want to edit and then click **Remove**.</span></span>

2. <span data-ttu-id="d98c9-147">Kattintson a **OK** amikor a rendszer kéri a tárfiók eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="d98c9-147">Click **OK** when prompted to remove the storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="d98c9-148">A tárfiók keresztül eltávolítása a **Tárfiókok** párbeszédpanel csak eltávolítja azt a listában tárfiókok megtekinthető Eclipse belül.</span><span class="sxs-lookup"><span data-stu-id="d98c9-148">Removing the storage account through the **Storage Accounts** dialog only removes it from the list of storage accounts viewable within Eclipse.</span></span> <span data-ttu-id="d98c9-149">A tárfiók nem távolítja el az Azure-előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="d98c9-149">It does not remove the storage account from your Azure subscription.</span></span> <span data-ttu-id="d98c9-150">Emellett a tárfiók sikerült listájában jelennek meg újra a után Eclipse Újratölti az előfizetés részleteit.</span><span class="sxs-lookup"><span data-stu-id="d98c9-150">Additionally, the storage account could appear again in your list after Eclipse reloads the details of your subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="d98c9-151">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d98c9-151">See Also</span></span>
<span data-ttu-id="d98c9-152">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d98c9-152">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="d98c9-153">[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d98c9-153">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="d98c9-154">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d98c9-154">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="d98c9-155">Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="d98c9-155">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
