---
title: "aaaSetting mentése nevű hitelesítő adatok |} Microsoft Docs"
description: "Ismerje meg, hogyan tootooprovide hitelesítő adatokat, hogy a Visual Studio használhatja tooauthenticate kérelmek tooAzure toopublish egy alkalmazás tooAzure a Visual Studio vagy egy meglévő toomonitor felhőalapú szolgáltatás... "
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/22/2017
ms.author: kraigb
ms.openlocfilehash: 3cc147a2f7a3e766293ca282f56078cf24281346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-named-authentication-credentials"></a><span data-ttu-id="2c83a-103">Nevű hitelesítő adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="2c83a-103">Setting Up Named Authentication Credentials</span></span>
<span data-ttu-id="2c83a-104">egy alkalmazás tooAzure a Visual Studio vagy egy meglévő felhőszolgáltatáshoz toomonitor toopublish, meg kell adni hitelesítő adatokat kéri, hogy a Visual Studio tooauthenticate használhatja a tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2c83a-104">toopublish an application tooAzure from Visual Studio or toomonitor an existing cloud service, you must provide credentials that Visual Studio can use tooauthenticate requests tooAzure.</span></span> <span data-ttu-id="2c83a-105">A Visual Studio, ahol bejelentkezhet tooprovide ezeket a hitelesítő adatokat több helyen van.</span><span class="sxs-lookup"><span data-stu-id="2c83a-105">There are several places in Visual Studio where you can sign in tooprovide these credentials.</span></span> <span data-ttu-id="2c83a-106">Például a Server Explorer hello, megnyitható hello helyi menüje hello **Azure** csomópont válassza **csatlakozás Azure-előfizetés tooMicrosoft...** . Amikor bejelentkezik, az Azure-fiókjával társított hello előfizetési adatok érhető el a Visual Studio, és nincs szükség további toodo rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2c83a-106">For example, from hello Server Explorer, you can open hello shortcut menu for hello **Azure** node and choose **Connect tooMicrosoft Azure Subscription...**. When you sign in, hello subscription information associated with your Azure account is available in Visual Studio, and there is nothing more you have toodo.</span></span>

<span data-ttu-id="2c83a-107">Azure-eszközök használatát is támogatja egy régebbi módját adja meg hitelesítő adatokat, a hello előfizetés fájlt (.publishsettings fájlt).</span><span class="sxs-lookup"><span data-stu-id="2c83a-107">Azure Tools also supports an older way of providing credentials, using hello subscription file (.publishsettings file).</span></span> <span data-ttu-id="2c83a-108">Ez a témakör ismerteti a metódus, amely az Azure SDK 2.2 továbbra is támogatja.</span><span class="sxs-lookup"><span data-stu-id="2c83a-108">This topic describes this method, which is still supported in Azure SDK 2.2.</span></span>

<span data-ttu-id="2c83a-109">a következő elemek hello hitelesítési tooAzure szükségesek:</span><span class="sxs-lookup"><span data-stu-id="2c83a-109">hello following items are required for authentication tooAzure:</span></span>

* <span data-ttu-id="2c83a-110">Az előfizetés-Azonosítóval</span><span class="sxs-lookup"><span data-stu-id="2c83a-110">Your subscription ID</span></span>
* <span data-ttu-id="2c83a-111">Egy érvényes X.509 v3 tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="2c83a-111">A valid X.509 v3 certificate</span></span>

> [!NOTE]
> <span data-ttu-id="2c83a-112">hello hello X.509 v3 tanúsítvány kulcs hossza legalább 2048 bites kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2c83a-112">hello length of hello X.509 v3 certificate's key must be at least 2048 bits.</span></span> <span data-ttu-id="2c83a-113">Azure elutasítják bármely olyan tanúsítvány, amely nem felel meg ennek a követelménynek, vagy, amely nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="2c83a-113">Azure will reject any certificate that doesn’t meet this requirement or that isn’t valid.</span></span>
>
>

<span data-ttu-id="2c83a-114">A Visual Studio hitelesítő adatok az előfizetés-Azonosítóval hello Tanúsítványadatok együtt használja.</span><span class="sxs-lookup"><span data-stu-id="2c83a-114">Visual Studio uses your subscription ID together with hello certificate data as credentials.</span></span> <span data-ttu-id="2c83a-115">hello előfizetés (fájl .publishsettings), amely tartalmaz egy hello tanúsítványhoz tartozó nyilvános kulcs hivatkozott hello megfelelő hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="2c83a-115">hello appropriate credentials are referenced in hello subscription file (.publishsettings file), which contains a public key for hello certificate.</span></span> <span data-ttu-id="2c83a-116">hello előfizetés fájl tartalmazhat egynél több előfizetésnek hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="2c83a-116">hello subscription file can contain credentials for more than one subscription.</span></span>

<span data-ttu-id="2c83a-117">Szerkesztheti a hello hello előfizetési adatok **új/szerkesztése előfizetés** párbeszédpanelen, a témakör későbbi részében leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="2c83a-117">You can edit hello subscription information from hello **New/Edit Subscription** dialog box, as explained later in this topic.</span></span>

<span data-ttu-id="2c83a-118">Ha azt szeretné toocreate tanúsítvány saját magának, olvassa el a toohello utasításait [létrehozása és feltöltése az Azure felügyeleti tanúsítvánnyal](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) , majd töltse fel manuálisan hello tanúsítvány toohello [a klasszikus Azure portálon ](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="2c83a-118">If you want toocreate a certificate yourself, you can refer toohello instructions in [Create and Upload a Management Certificate for Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) and then manually upload hello certificate toohello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span>

> [!NOTE]
> <span data-ttu-id="2c83a-119">Ezeket a hitelesítő adatokat, amelyek a Visual Studio igényli a felhőszolgáltatások nem toomanage hello ugyanazokat a hitelesítő adatokat, amelyek a szükséges tooauthenticate hello Azure storage szolgáltatások egy kérelmet.</span><span class="sxs-lookup"><span data-stu-id="2c83a-119">These credentials that Visual Studio requires toomanage your cloud services aren’t hello same credentials that are required tooauthenticate a request against hello Azure storage services.</span></span>
>
>

## <a name="import-set-up-or-edit-authentication-credentials-in-visual-studio"></a><span data-ttu-id="2c83a-120">Importálja, állítsa be, vagy a Visual Studio hitelesítő adatok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="2c83a-120">Import, set up, or edit authentication credentials in Visual Studio</span></span>
<span data-ttu-id="2c83a-121">Importálja, állítsa be, vagy módosítsa a hitelesítő adatok a hello **új előfizetés** párbeszédpanelt, amely akkor jelenik meg, ha hajt végre a következő művelet hello:</span><span class="sxs-lookup"><span data-stu-id="2c83a-121">You can import, set up, or modify your authentication credentials in hello **New Subscription** dialog box, which appears if you perform hello following action:</span></span>

* <span data-ttu-id="2c83a-122">A **Server Explorer**, nyissa meg hello helyi menüje hello **Azure** csomópontot, válassza a **kezelése és a szűrő előfizetések...** , válassza ki a hello **tanúsítványok** lapot, és hello a következőket:</span><span class="sxs-lookup"><span data-stu-id="2c83a-122">In **Server Explorer**, open hello shortcut menu for hello **Azure** node, choose **Manage and Filter Subscriptions...**, choose hello **Certificates** tab, and do one of hello following:</span></span>

    * <span data-ttu-id="2c83a-123">Válasszon **importálása** tooopen hello importálása a Microsoft Azure-előfizetések párbeszédpanel le hello hello előfizetések fájl jelenleg betöltött előfizetés, a Tallózás tooits letöltési hely, és importálja a használatra hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="2c83a-123">Choose **Import** tooopen hello Import Microsoft Azure Subscriptions dialog where you can download hello  subscriptions file for hello currently loaded subscription, browse tooits download location, and then import it for use in authentication.</span></span>
    * <span data-ttu-id="2c83a-124">Válasszon **új** tooopen-hello új előfizetés párbeszédpanel, ahol beállíthat egy új előfizetés hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="2c83a-124">Choose **New** tooopen hello New Subscription dialog where you can set up a new subscription for use in authentication.</span></span>
    * <span data-ttu-id="2c83a-125">Válasszon **szerkesztése** (Miután kiválasztotta a aktív előfizetéssel) tooopen hello előfizetés szerkesztése párbeszédpanelen egy meglévő előfizetéshez hitelesítési használható szerkesztésére.</span><span class="sxs-lookup"><span data-stu-id="2c83a-125">Choose **Edit** (after choosing your active subscription) tooopen hello Edit Subscription dialog where you can edit an existing subscription for use in authentication.</span></span> 

<span data-ttu-id="2c83a-126">hello következő eljárás azt feltételezi, hogy hello **új előfizetés** párbeszédpanel nyitva.</span><span class="sxs-lookup"><span data-stu-id="2c83a-126">hello following procedure assumes that hello **New Subscription** dialog box is open.</span></span>

### <a name="tooset-up-authentication-credentials-in-visual-studio"></a><span data-ttu-id="2c83a-127">a Visual Studio hitelesítő adatok mentése tooset</span><span class="sxs-lookup"><span data-stu-id="2c83a-127">tooset up authentication credentials in Visual Studio</span></span>
1. <span data-ttu-id="2c83a-128">A hello **jelöljön ki egy létező tanúsítványt** hitelesítési listáját, válasszon egy tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="2c83a-128">In hello **Select an existing certificate** for authentication list, choose a certificate.</span></span>
2. <span data-ttu-id="2c83a-129">Válassza ki a hello **másolási hello teljes elérési útja** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="2c83a-129">Choose hello **Copy hello full path** link.</span></span> <span data-ttu-id="2c83a-130">hello hello tanúsítvány (.cer-fájl) elérési út vágólapra másolt toohello.</span><span class="sxs-lookup"><span data-stu-id="2c83a-130">hello path for hello certificate (.cer file) is copied toohello Clipboard.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="2c83a-131">toopublish az Azure-alkalmazást a Visual Studio eszközből, fel kell töltenie a tanúsítvány toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="2c83a-131">toopublish your Azure application from Visual Studio, you must upload this certificate toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
   >
   >
3. <span data-ttu-id="2c83a-132">tooupload hello tanúsítvány toohello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="2c83a-132">tooupload hello certificate toohello Azure portal:</span></span>

   1. <span data-ttu-id="2c83a-133">Nyissa meg hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="2c83a-133">Open hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
   2. <span data-ttu-id="2c83a-134">Ha a rendszer kéri, jelentkezzen be toohello portálon, és keresse meg a túl**beállítások**, **felügyeleti tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="2c83a-134">If prompted, sign in toohello portal and then navigate too**Settings**, **Management Certificates**.</span></span>
   3. <span data-ttu-id="2c83a-135">Hello felügyeleti tanúsítványok ablaktábláján válassza **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="2c83a-135">In hello Management certificates pane, choose **Upload**.</span></span>
   4. <span data-ttu-id="2c83a-136">Válassza ki az Azure-előfizetéssel, illessze be hello fájljának teljes elérési útját hello .cer imént létrehozott, és válassza **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="2c83a-136">Select your Azure subscription, paste hello full path of hello .cer file that you just created, and choose **Upload**.</span></span>
