---
title: "Tárfiók aaaAzure"
description: "Az Eclipse hello Azure eszközkészlet használata tárolási beállításainak kezelése"
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
ms.openlocfilehash: 35e25881ca95ae4050a26283e4726d9549b37f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-account-list"></a><span data-ttu-id="e45f4-103">Az Azure Storage-fiók listája</span><span class="sxs-lookup"><span data-stu-id="e45f4-103">Azure Storage Account List</span></span>
<span data-ttu-id="e45f4-104">Az Azure storage-fiókok lehetővé teszik a JDK, a kiszolgáló és a tetszőleges összetevők, valamint a gyorsítótárazás használatakor állapot tárolásához használt helyek toobe letöltése.</span><span class="sxs-lookup"><span data-stu-id="e45f4-104">Azure storage accounts enable download locations toobe used for your JDK, application server, and arbitrary components, as well as for storing state when using caching.</span></span> <span data-ttu-id="e45f4-105">Eclipse-ben elérhető tooyour projektekhez az Eclipse munkaterületen ismert storage-fiókok listáját.</span><span class="sxs-lookup"><span data-stu-id="e45f4-105">Eclipse maintains a list of known storage accounts that are available tooyour projects in your Eclipse workspace.</span></span> <span data-ttu-id="e45f4-106">tooopen hello **Tárfiókok** párbeszédpanel, amely, amely a listában, belül eclipse-ben használt toomanage kattintson **ablak**, kattintson a **beállítások**, bontsa ki a **Azure** , és kattintson a **Tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="e45f4-106">tooopen hello **Storage Accounts** dialog, which is used toomanage that list, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Storage Accounts**.</span></span>

<span data-ttu-id="e45f4-107">hello következő mutatja hello **Tárfiókok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e45f4-107">hello following shows hello **Storage Accounts** dialog.</span></span>

![][ic719496]

<span data-ttu-id="e45f4-108">Ezen a párbeszédpanelen is megnyithatja az egy **fiókok** storage-fiókok, például hello következő használó párbeszédpanelek hivatkozásra:</span><span class="sxs-lookup"><span data-stu-id="e45f4-108">This dialog can also be opened from an **Accounts** link on dialog boxes that use storage accounts, such as hello following:</span></span>

* <span data-ttu-id="e45f4-109">Hello **JDK** hello lapján **kiszolgálókonfiguráció** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e45f4-109">hello **JDK** tab of hello **Server Configuration** dialog.</span></span>
* <span data-ttu-id="e45f4-110">Hello **Server** hello lapján **kiszolgálókonfiguráció** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e45f4-110">hello **Server** tab of hello **Server Configuration** dialog.</span></span>
* <span data-ttu-id="e45f4-111">Hello **összetevő felvétele** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e45f4-111">hello **Add Component** dialog.</span></span>
* <span data-ttu-id="e45f4-112">Hello **gyorsítótárazását** tulajdonságainak párbeszédpanelét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e45f4-112">hello **Caching** properties dialog.</span></span>

## <a name="tooimport-your-storage-accounts-using-a-publish-settings-file"></a><span data-ttu-id="e45f4-113">a tárolási fiókok tooimport a közzétételi beállítások fájlja</span><span class="sxs-lookup"><span data-stu-id="e45f4-113">tooimport your storage accounts using a publish settings file</span></span>
1. <span data-ttu-id="e45f4-114">Hello belül **Tárfiókok** párbeszédpanel, kattintson a **közzététele beállításfájl importálás**.</span><span class="sxs-lookup"><span data-stu-id="e45f4-114">Within hello **Storage Accounts** dialog, click **Import from PUBLISH-SETTINGS file**.</span></span>

2. <span data-ttu-id="e45f4-115">(Kihagyhatja ezt a lépést, ha már mentette a közzétételi beállítások fájl tooyour helyi számítógépen.) A hello **importálási előfizetési adatok** párbeszédpanel, kattintson a **KÖZZÉTÉTELI-beállítások fájl letöltése**.</span><span class="sxs-lookup"><span data-stu-id="e45f4-115">(Skip this step if you have already saved a publish settings file tooyour local machine.) In hello **Import Subscription Information** dialog, click **Download PUBLISH-SETTINGS File**.</span></span> <span data-ttu-id="e45f4-116">Ha még nem jelentkezik be az Azure-fiókjába, a kért toolog fogja.</span><span class="sxs-lookup"><span data-stu-id="e45f4-116">If you are not yet logged into your Azure account, you will be prompted toolog in.</span></span> <span data-ttu-id="e45f4-117">Akkor kérni fogja az Azure toosave közzététele beállításfájl.</span><span class="sxs-lookup"><span data-stu-id="e45f4-117">Then you'll be prompted toosave an Azure publish settings file.</span></span> <span data-ttu-id="e45f4-118">(Hello eredményül kapott utasításokat az hello bejelentkezési oldalakon - figyelmen kívül hagyhatja ezeket hello Azure-portál által biztosított és a Visual Studio felhasználóknak.) Mentse a helyi számítógép tooyour.</span><span class="sxs-lookup"><span data-stu-id="e45f4-118">(You can ignore hello resulting instructions shown on hello logon pages - they are provided by hello Azure portal and are intended for Visual Studio users.) Save it tooyour local machine.</span></span>

3. <span data-ttu-id="e45f4-119">Továbbra is a hello **importálási előfizetési adatok** párbeszédpanel, hello kattintson **Tallózás** gombra, jelölje be hello közzététele beállításfájl korábban mentett helyileg, és kattintson a **megnyitása**.</span><span class="sxs-lookup"><span data-stu-id="e45f4-119">Still in hello **Import Subscription Information** dialog, click hello **Browse** button, select hello publish settings file that you saved locally previously, and then click **Open**.</span></span>

4. <span data-ttu-id="e45f4-120">Kattintson a **OK** tooclose hello **importálási előfizetési adatok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e45f4-120">Click **OK** tooclose hello **Import Subscription Information** dialog.</span></span>

## <a name="toocreate-a-new-storage-account"></a><span data-ttu-id="e45f4-121">új tárfiók toocreate</span><span class="sxs-lookup"><span data-stu-id="e45f4-121">toocreate a new storage account</span></span>
1. <span data-ttu-id="e45f4-122">Hello belül **Tárfiókok** párbeszédpanel, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="e45f4-122">Within hello **Storage Accounts** dialog, click **Add**.</span></span>

2. <span data-ttu-id="e45f4-123">Hello belül **Tárfiók hozzáadása** párbeszédpanel, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="e45f4-123">Within hello **Add Storage Account** dialog, click **New**.</span></span>

3. <span data-ttu-id="e45f4-124">Hello belül **új Tárfiók** párbeszédpanelen adható meg érték a következő hello:</span><span class="sxs-lookup"><span data-stu-id="e45f4-124">Within hello **New Storage Account** dialog, specify values for hello following:</span></span>

   * <span data-ttu-id="e45f4-125">Tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="e45f4-125">Storage account name.</span></span>

   * <span data-ttu-id="e45f4-126">Hello tárfiók helye.</span><span class="sxs-lookup"><span data-stu-id="e45f4-126">Location of hello storage account.</span></span>

   * <span data-ttu-id="e45f4-127">Hello tárfiók leírását.</span><span class="sxs-lookup"><span data-stu-id="e45f4-127">Description of hello storage account.</span></span>

   * <span data-ttu-id="e45f4-128">hello előfizetés toowhich hello tárfiók tartozik.</span><span class="sxs-lookup"><span data-stu-id="e45f4-128">hello subscription toowhich hello storage account belongs.</span></span>

4. <span data-ttu-id="e45f4-129">Kattintson a **OK** tooclose hello **új Tárfiók** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e45f4-129">Click **OK** tooclose hello **New Storage Account** dialog.</span></span>

<span data-ttu-id="e45f4-130">A tárolási fiók toobe létrehozása több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="e45f4-130">It may take several minutes for your storage account toobe created.</span></span> <span data-ttu-id="e45f4-131">Kattintson a létrehozás után **OK** tooclose hello **Tárfiók hozzáadása** párbeszédpanel, és az új tárfiók hozzáadódnak toohello rendelkezésre álló tár listája.</span><span class="sxs-lookup"><span data-stu-id="e45f4-131">After it is created, click **OK** tooclose hello **Add Storage Account** dialog, and your new storage account will be added toohello list of available storage accounts.</span></span>

## <a name="tooadd-an-existing-storage-account-toohello-list"></a><span data-ttu-id="e45f4-132">egy meglévő tárolási toohello fióklista tooadd</span><span class="sxs-lookup"><span data-stu-id="e45f4-132">tooadd an existing storage account toohello list</span></span>
1. <span data-ttu-id="e45f4-133">Ha még nem rendelkezik egy fiókot, hozzon létre egyet által az Azure storage hello lépések felsorolt hello **toocreate új tárolási fiók szakasz** felett.</span><span class="sxs-lookup"><span data-stu-id="e45f4-133">If you do not already have a Azure storage account, create one by following hello steps listed in hello **toocreate a new storage account section** above.</span></span> <span data-ttu-id="e45f4-134">(Másik lehetőségként létrehozhat egy új tárfiókot hello, [Azure felügyeleti portálon][Azure Management Portal].)</span><span class="sxs-lookup"><span data-stu-id="e45f4-134">(Alternatively, you can create a new storage account at hello [Azure Management Portal][Azure Management Portal].)</span></span>

2. <span data-ttu-id="e45f4-135">Hello belül **Tárfiókok** párbeszédpanel, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="e45f4-135">Within hello **Storage Accounts** dialog, click **Add**.</span></span>

3. <span data-ttu-id="e45f4-136">Hello belül **Tárfiók hozzáadása** párbeszédpanelen adja meg az értékeket **neve** és **hozzáférési kulcs**.</span><span class="sxs-lookup"><span data-stu-id="e45f4-136">Within hello **Add Storage Account** dialog, enter values for **Name** and **Access Key**.</span></span> <span data-ttu-id="e45f4-137">hello nevét és a hozzáférési kulcsot a meglévő Azure-tárfiók kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e45f4-137">hello account name and access key must be for an existing Azure storage account.</span></span> <span data-ttu-id="e45f4-138">Használjon hello **tárolási** hello szakasza [Azure felügyeleti portálon] [ Azure Management Portal] tooview a tárfiók nevét és a kulcsok.</span><span class="sxs-lookup"><span data-stu-id="e45f4-138">Use hello **Storage** section of hello [Azure Management Portal][Azure Management Portal] tooview your storage account names and keys.</span></span> <span data-ttu-id="e45f4-139">A **Tárfiók hozzáadása** párbeszédpanel hasonló toohello következő fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="e45f4-139">Your **Add Storage Account** dialog will look similar toohello following.</span></span>
   
   ![][ic719497]

4. <span data-ttu-id="e45f4-140">Kattintson a **OK** tooclose hello **Tárfiók hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e45f4-140">Click **OK** tooclose hello **Add Storage Account** dialog.</span></span>

## <a name="toomodify-a-storage-account-toouse-a-new-access-key"></a><span data-ttu-id="e45f4-141">a tárolási fiók toouse új hozzáférési kulcs toomodify</span><span class="sxs-lookup"><span data-stu-id="e45f4-141">toomodify a storage account toouse a new access key</span></span>
1. <span data-ttu-id="e45f4-142">Hello belül **Tárfiókok** párbeszédpanel, kattintson a tárfiók hello tooedit szeretne, és kattintson a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="e45f4-142">Within hello **Storage Accounts** dialog, click hello storage account that you want tooedit and then click **Edit**.</span></span>

2. <span data-ttu-id="e45f4-143">Hello belül **szerkesztése Tárfiók_elérési_kulcsa** párbeszédpanelen hello módosítása **hozzáférési kulcs** érték.</span><span class="sxs-lookup"><span data-stu-id="e45f4-143">Within hello **Edit Storage Account Access Key** dialog, modify hello **Access Key** value.</span></span>

3. <span data-ttu-id="e45f4-144">Kattintson a **OK** tooclose hello **szerkesztése Tárfiók_elérési_kulcsa** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e45f4-144">Click **OK** tooclose hello **Edit Storage Account Access Key** dialog.</span></span>

## <a name="tooremove-a-storage-account-from-hello-list-maintained-in-eclipse"></a><span data-ttu-id="e45f4-145">tooremove hello listából tárfiók tartani az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="e45f4-145">tooremove a storage account from hello list maintained in Eclipse</span></span>
1. <span data-ttu-id="e45f4-146">Hello belül **Tárfiókok** párbeszédpanel, kattintson a tárfiók hello tooedit szeretne, és kattintson a **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="e45f4-146">Within hello **Storage Accounts** dialog, click hello storage account that you want tooedit and then click **Remove**.</span></span>

2. <span data-ttu-id="e45f4-147">Kattintson a **OK** amikor felszólító tooremove hello storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="e45f4-147">Click **OK** when prompted tooremove hello storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="e45f4-148">Hello keresztül hello tárfiók eltávolítása **Tárfiókok** párbeszédpanel csak eltávolítja hello listája megtekinthető Eclipse belül.</span><span class="sxs-lookup"><span data-stu-id="e45f4-148">Removing hello storage account through hello **Storage Accounts** dialog only removes it from hello list of storage accounts viewable within Eclipse.</span></span> <span data-ttu-id="e45f4-149">Hello tárfiók nem távolítja el az Azure-előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="e45f4-149">It does not remove hello storage account from your Azure subscription.</span></span> <span data-ttu-id="e45f4-150">Emellett hello tárfiók sikerült listájában jelennek meg újra a után Eclipse Újratölti az előfizetés hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="e45f4-150">Additionally, hello storage account could appear again in your list after Eclipse reloads hello details of your subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="e45f4-151">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e45f4-151">See Also</span></span>
<span data-ttu-id="e45f4-152">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e45f4-152">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="e45f4-153">[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e45f4-153">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="e45f4-154">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e45f4-154">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="e45f4-155">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="e45f4-155">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
