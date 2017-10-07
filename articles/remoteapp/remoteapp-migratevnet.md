---
title: "a RemoteApp virtuális Hálózatot tooan Azure virtuális Hálózatot a aaaHow toomigrate |} Microsoft Docs"
description: "Megtudhatja, hogyan toomigrate a egy RemoteApp virtuális Hálózatot tooan Azure virtuális hálózat"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: baea5d29-353b-48f8-b47f-806f2163e067
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: c0f8617556c6f1e33eca8322febf67ff33937ecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-a-hybrid-collection-from-a-remoteapp-vnet-tooan-azure-vnet"></a><span data-ttu-id="04161-103">Hogyan toomigrate egy hibrid gyűjteményt a RemoteApp virtuális Hálózatot tooan Azure virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="04161-103">How toomigrate a hybrid collection from a RemoteApp VNET tooan Azure VNET</span></span>
> [!IMPORTANT]
> <span data-ttu-id="04161-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="04161-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="04161-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="04161-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="04161-106">Jó hírünk van!</span><span class="sxs-lookup"><span data-stu-id="04161-106">Good news!</span></span> <span data-ttu-id="04161-107">Jelenleg engedélyezett, toodeploy hibrid RemoteApp-gyűjtemények közvetlenül be a meglévő Azure virtuális hálózatokról (Vnetekről) RemoteApp-specifikus Vnetek létrehozása helyett.</span><span class="sxs-lookup"><span data-stu-id="04161-107">We have enabled you toodeploy hybrid RemoteApp collections directly into your existing Azure virtual networks (VNETs) instead of creating RemoteApp-specific VNETs.</span></span> <span data-ttu-id="04161-108">Ez lehetővé teszi előnyeit hello legújabb VNET funkciók (például ExpressRoute), és adjon a hibrid gyűjtemények közvetlen hálózati hozzáférés tooother Azure-szolgáltatások és virtuális gépek rendszerbe toothat virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="04161-108">This lets you take advantage of hello latest VNET features (like ExpressRoute) and give your hybrid collections direct network access tooother Azure services and virtual machines deployed toothat VNET.</span></span>  <span data-ttu-id="04161-109">(Ez segít jobb teljesítmény és egyszerűbb a telepítő mint VNET – VNET konfigurációk).</span><span class="sxs-lookup"><span data-stu-id="04161-109">(This gets you better performance and easier setup than VNET-to-VNET configurations).</span></span>

<span data-ttu-id="04161-110">Tegyük fel, hogy már létrehozott egy hibrid RemoteApp-gyűjtemény nevű *OriginalCollection* RemoteApp VNET neve *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="04161-110">Let’s say that you’ve already created a hybrid RemoteApp collection called *OriginalCollection* with a RemoteApp VNET called *RemoteAppVNET*.</span></span> <span data-ttu-id="04161-111">Az alábbiakban hello lépéseket toomigrate az új Azure VNET neve tooa *AzureVNET*.</span><span class="sxs-lookup"><span data-stu-id="04161-111">Here are hello steps toomigrate it tooa new Azure VNET called *AzureVNET*.</span></span>

1. <span data-ttu-id="04161-112">A hello **hálózatok** hello lapján [kezelési portál](http://manage.windowsazure.com/), hozzon létre egy VNETET nevű *AzureVNET*használatával hello ugyanazon a helyen, a DNS-konfiguráció és a címtér (legalább egy a hello *AzureVNET* alhálózatok), ha Ön használt *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="04161-112">On hello **Networks** tab in hello [management portal](http://manage.windowsazure.com/), create a VNET called *AzureVNET*, using hello same location, DNS configuration, and address space (for at least one of hello *AzureVNET* subnets) as you used for *RemoteAppVNET*.</span></span>
2. <span data-ttu-id="04161-113">Konfigurálása *AzureVNET* tooeither gazdagép vagy a hálózati kapcsolat toohello Active Directory központi telepítéssel rendelkezik, amely *OriginalCollection* tartományhoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="04161-113">Configure *AzureVNET* tooeither host or have network connectivity toohello Active Directory deployment that *OriginalCollection* is domain joined to.</span></span>
3. <span data-ttu-id="04161-114">A hello **távoli alkalmazások** lapra, hozzon létre egy új RemoteApp-gyűjtemény nevű *új gyűjtemény*.</span><span class="sxs-lookup"><span data-stu-id="04161-114">On hello **RemoteApps** tab, create a new RemoteApp collection called *New Collection*.</span></span> <span data-ttu-id="04161-115">(Használata hello **hozzon létre virtuális hálózaton** beállítás, nem **Gyorslétrehozás**.)</span><span class="sxs-lookup"><span data-stu-id="04161-115">(Use hello **Create with VNET** option, not **Quick Create**.)</span></span>
4. <span data-ttu-id="04161-116">Konfigurálása *NewCollection* toobe telepített tooa alhálózatának *AzureVNET*.</span><span class="sxs-lookup"><span data-stu-id="04161-116">Configure *NewCollection* toobe deployed tooa subnet in *AzureVNET*.</span></span>
5. <span data-ttu-id="04161-117">Konfigurálása *NewCollection* toouse hello ugyanazt a képet, és, ha Ön használt tartományi csatlakozási adatainak *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="04161-117">Configure *NewCollection* toouse hello same image and domain join information as you used for *OriginalCollection*.</span></span>
6. <span data-ttu-id="04161-118">Néhány óra elteltével *NewCollection* fog megjelenni az adatgyűjtési lista egy aktív állapotú.</span><span class="sxs-lookup"><span data-stu-id="04161-118">After a few hours, *NewCollection* will show up in your collection list with an Active state.</span></span>

<span data-ttu-id="04161-119">Mostantól Ha nem kívánja toomigrate gyűjteményből hello eredeti gyűjtemény toohello új felhasználói adatokat, teendőkre ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="04161-119">Now, if you DON’T need toomigrate any user information from hello original collection toohello new collection, do these steps next:</span></span>

1. <span data-ttu-id="04161-120">Törlés *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="04161-120">Delete *OriginalCollection*.</span></span>
2. <span data-ttu-id="04161-121">Törlés *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="04161-121">Delete *RemoteAppVNET*.</span></span>

<span data-ttu-id="04161-122">És elkészült!</span><span class="sxs-lookup"><span data-stu-id="04161-122">And, you’re done!</span></span>

<span data-ttu-id="04161-123">Alternatív megoldásként Ha tájékoztatásra van szüksége toomigrate felhasználói hello eredeti gyűjtemény toohello új gyűjtemény, teendőkre ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="04161-123">Alternately, if you DO need toomigrate user information from hello original collection toohello new collection, do these steps next:</span></span>

1. <span data-ttu-id="04161-124">Az e-mailek küldése túl[ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) az Azure-előfizetése Azonosítóját, hello az eredeti gyűjtemény nevét, és az új gyűjtemény hello nevét, és kérje meg toomigrate a felhasználói adatokat.</span><span class="sxs-lookup"><span data-stu-id="04161-124">Send an email too[remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) with your Azure subscription ID, hello name of your original collection, and hello name of your new collection, and ask them toomigrate your user information.</span></span>
2. <span data-ttu-id="04161-125">2 munkanapon belül hello RemoteApp csapatától áthelyezi hello felhasználói hozzáférési lista és az összes felhasználói dokumentumokat és a felhasználói beállítások hello eredeti gyűjtemény toohello új gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="04161-125">Within 2 business days hello RemoteApp team will move hello user access list and all user documents and user settings from hello original collection toohello new collection.</span></span>
3. <span data-ttu-id="04161-126">Törlés *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="04161-126">Delete *OriginalCollection*.</span></span>
4. <span data-ttu-id="04161-127">Törlés *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="04161-127">Delete *RemoteAppVNET*.</span></span>

<span data-ttu-id="04161-128">És mostantól elkészült!</span><span class="sxs-lookup"><span data-stu-id="04161-128">And now, you’re done!</span></span>

<span data-ttu-id="04161-129">Ha kérdése van, vagy különleges segítségre van szüksége, írjon e-mailt [ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).</span><span class="sxs-lookup"><span data-stu-id="04161-129">If you have any questions or need special assistance, please email [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).</span></span>

