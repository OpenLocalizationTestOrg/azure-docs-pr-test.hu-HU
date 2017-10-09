---
title: "App-V aaaUsing alkalmazások az Azure RemoteApp |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse App-V alkalmazások az Azure Remoteappban."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e2292cb2-5c89-4b2b-ab11-74dbacd07c31
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 9cf5c2eeee2a0ce2cf98e1cf6497dffbc6eff016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-app-v-apps-in-azure-remoteapp"></a><span data-ttu-id="307f9-103">App-V alkalmazások használata az Azure Remoteappban</span><span class="sxs-lookup"><span data-stu-id="307f9-103">Using App-V apps in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="307f9-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="307f9-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="307f9-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="307f9-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="307f9-106">Egy Azure RemoteApp hibrid gyűjteményt, amelyhez a tartományhoz való csatlakozást is használhatja az App-V alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="307f9-106">You can use App-V applications in a Azure RemoteApp hybrid collection, which requires domain join.</span></span>

<span data-ttu-id="307f9-107">Mielőtt elkezdené, győződjön meg arról, hogy tooinstall hello App-V 5.1 ügyfél hello legújabb frissítéseit.</span><span class="sxs-lookup"><span data-stu-id="307f9-107">Before you get started, make sure tooinstall hello App-V 5.1 client with hello latest updates.</span></span> <span data-ttu-id="307f9-108">Toocreate szüksége lesz egy [egyéni lemezkép](remoteapp-create-custom-image.md) hello App-V-ügyfél foglal magában.</span><span class="sxs-lookup"><span data-stu-id="307f9-108">You will need toocreate a [custom image](remoteapp-create-custom-image.md) that includes hello App-V client.</span></span>  

<span data-ttu-id="307f9-109">Egyszerű toouse a meglévő App-V infrastruktúra az Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="307f9-109">It’s easy toouse your existing App-V infrastructure with Azure RemoteApp.</span></span> <span data-ttu-id="307f9-110">Hibrid gyűjtemény központilag telepítik egy Azure virtuális Hálózatot, amely rendelkezik hozzáférési tooyour tartományvezérlő, és hello virtuális gépek csatlakoznak a tartományhoz, kihasználhatja a meglévő App-v infrastruktúra és a központi telepítési módszerek tooeasyily állomás App-V alkalmazás az Azure Remoteappban.</span><span class="sxs-lookup"><span data-stu-id="307f9-110">Since a hybrid collection is deployed into an Azure VNET that has access tooyour domain controller and hello VMs are domain joined, you can leverage your existing App-v infrastructure and deployment methods tooeasyily host App-V application in Azure RemoteApp.</span></span> <span data-ttu-id="307f9-111">Az alábbiakban néhány szempontokat, amelyeket kell ismernie az App-V központi telepítési jelenleg hello típus alapján:</span><span class="sxs-lookup"><span data-stu-id="307f9-111">Here are some considerations that you should be aware of based on hello type of App-V deployment you currently have:</span></span>

| <span data-ttu-id="307f9-112">Konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="307f9-112">Configuration options</span></span> |  | <span data-ttu-id="307f9-113">Pozitív</span><span class="sxs-lookup"><span data-stu-id="307f9-113">Positive</span></span> | <span data-ttu-id="307f9-114">Negatív.</span><span class="sxs-lookup"><span data-stu-id="307f9-114">Negative</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="307f9-115">Kézbesítési módszer</span><span class="sxs-lookup"><span data-stu-id="307f9-115">Delivery method</span></span> |<span data-ttu-id="307f9-116">Adatfolyam-továbbítási (igény)</span><span class="sxs-lookup"><span data-stu-id="307f9-116">Streaming (on-demand)</span></span> |<span data-ttu-id="307f9-117">Alkalmazás áll mindig a legújabb és friss hello</span><span class="sxs-lookup"><span data-stu-id="307f9-117">App is always hello latest and fresh</span></span> |<span data-ttu-id="307f9-118">Első késleltetés</span><span class="sxs-lookup"><span data-stu-id="307f9-118">First time latency</span></span> |
| <span data-ttu-id="307f9-119">Csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="307f9-119">Mounted</span></span> |<span data-ttu-id="307f9-120">Leggyorsabb; alkalmazás már szerepel a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="307f9-120">Fastest; app is already present on hello VM</span></span> |<span data-ttu-id="307f9-121">Túlzott növekedést - vesz fel helyet a kép (127 GB lehet)</span><span class="sxs-lookup"><span data-stu-id="307f9-121">Bloat - takes up image space (127 GB limit)</span></span> | |
| <span data-ttu-id="307f9-122">Alkalmazás helye tároló</span><span class="sxs-lookup"><span data-stu-id="307f9-122">App location storage</span></span> |<span data-ttu-id="307f9-123">A megosztott tartalom</span><span class="sxs-lookup"><span data-stu-id="307f9-123">Shared content</span></span> |<span data-ttu-id="307f9-124">Az Azure RemoteApp-példány memóriáját alkalmazás fut</span><span class="sxs-lookup"><span data-stu-id="307f9-124">App runs in memory of Azure RemoteApp instance</span></span> |<span data-ttu-id="307f9-125">Memória és jó kapcsolat toostreaming (fájl) kiszolgáló hello alkalmazást tartalmazó eats</span><span class="sxs-lookup"><span data-stu-id="307f9-125">Eats memory and good connection toostreaming (file) server where hello app resides</span></span> |
| <span data-ttu-id="307f9-126">Lemez (gyorsítótárazott)</span><span class="sxs-lookup"><span data-stu-id="307f9-126">Disk (Cached)</span></span> |<span data-ttu-id="307f9-127">Gyors végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="307f9-127">Fast execution.</span></span> <span data-ttu-id="307f9-128">Nem függ a rendelkezésre állási tartalomforrás alkalmazás</span><span class="sxs-lookup"><span data-stu-id="307f9-128">App not dependent on availability of Content Source</span></span> |<span data-ttu-id="307f9-129">Túlzott növekedést - vesz fel helyet a kép (127 GB lehet)</span><span class="sxs-lookup"><span data-stu-id="307f9-129">Bloat - takes up image space (127 GB limit)</span></span> | |
| <span data-ttu-id="307f9-130">Célcsoport-kezelési</span><span class="sxs-lookup"><span data-stu-id="307f9-130">Targeting</span></span> |<span data-ttu-id="307f9-131">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="307f9-131">User</span></span> |<span data-ttu-id="307f9-132">Teljes önálló App-V infrastruktúrájára van szükség</span><span class="sxs-lookup"><span data-stu-id="307f9-132">Requires full standalone App-V infrastructure</span></span> | |
| <span data-ttu-id="307f9-133">Globális (számítógép)</span><span class="sxs-lookup"><span data-stu-id="307f9-133">Global (machine)</span></span> |<span data-ttu-id="307f9-134">Előzetes közzététel vagy használatával közzétételi célként kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="307f9-134">Pre-publish or target using Publishing server</span></span> |<span data-ttu-id="307f9-135">Van szükség tooupdate a kép: Azure tooupdate hello app (nagy).</span><span class="sxs-lookup"><span data-stu-id="307f9-135">Need tooupdate your Azure image if you want tooupdate hello app (huge).</span></span> <span data-ttu-id="307f9-136">Foglal helyet a lemezképet.</span><span class="sxs-lookup"><span data-stu-id="307f9-136">Takes up some space on image.</span></span> | |

 <span data-ttu-id="307f9-137">Az egyéni lemezképet, és a hibrid gyűjtemény létrehozása után az alkalmazás közzétételére, felhasználók hozzárendelése és a meglévő App-V alkalmazások tooany eszközről, bárhonnan kézbesíteni Azure Remoteappban üzemeltetett élvez.</span><span class="sxs-lookup"><span data-stu-id="307f9-137">After you create your custom image and your hybrid collection, publish your application, assign users and enjoy your existing App-V applications hosted in Azure RemoteApp delivered tooany device anywhere.</span></span>

