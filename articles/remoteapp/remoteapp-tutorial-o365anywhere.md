---
title: "Azonos Office 365-élmény bármely Azure RemoteAppot használó eszközön | Microsoft Docs"
description: "Ismerje meg, hogyan oszthatja meg bármelyik Office 365-alkalmazást a felhasználóival az Azure RemoteApp segítségével."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 0c971ce9-7d45-4cfb-9737-15b6706047e8
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: guscatal;elizapo
ms.openlocfilehash: 584c781c97097cda3c1455ade05cba8659f11073
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a><span data-ttu-id="3b749-103">Azonos Office 365-élmény bármely Azure RemoteApp szolgáltatást használó eszközön</span><span class="sxs-lookup"><span data-stu-id="3b749-103">Get the same Office 365 experience on any device with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="3b749-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="3b749-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="3b749-105">A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="3b749-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="3b749-106">Ez a cikk ismerteti, hogy hogyan helyezheti üzembe az Office 365-alkalmazásokat a vállat bármelyik eszközén.</span><span class="sxs-lookup"><span data-stu-id="3b749-106">This article will cover how to deploy Office 365 on any device in your company.</span></span> <span data-ttu-id="3b749-107">A felhasználók ugyanolyan képességeket kaphatnak és ugyanolyan felhasználói élményben lehet részük Android-, Apple- és Windows-eszközökön.</span><span class="sxs-lookup"><span data-stu-id="3b749-107">Your users can get the same capabilities and UI experience on Android, Apple and Windows.</span></span>

<span data-ttu-id="3b749-108">Ezt az Azure RemoteApp használatával érheti el az Office 365 méretezhető virtuális gépeken való üzemeltetésével az Azure-ban, amelyekhez a felhasználók csatlakozhatnak.</span><span class="sxs-lookup"><span data-stu-id="3b749-108">We will accomplish this using Azure RemoteApp by hosting Office 365 on scale-able virtual machines in Azure that users can connect to.</span></span> <span data-ttu-id="3b749-109">A virtuális gépek ezen készletét „felhőalapú gyűjteménynek” nevezik.</span><span class="sxs-lookup"><span data-stu-id="3b749-109">This set of virtual machines we call a "cloud collection".</span></span>

## <a name="create-a-cloud-collection"></a><span data-ttu-id="3b749-110">Felhőalapú gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="3b749-110">Create a cloud collection</span></span>
<span data-ttu-id="3b749-111">Miután létrehozott egy Azure-fiókot, először navigáljon a **RemoteAppra** a bal oldalon található hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="3b749-111">First after you have created an Azure account, navigate to **RemoteApp** by clicking on the link on the left side.</span></span>
<span data-ttu-id="3b749-112">![Az Azure RemoteApp megjelenítése az Azure Portalon](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span><span class="sxs-lookup"><span data-stu-id="3b749-112">![Showing Azure RemoteApp on the Azure Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span></span>

<span data-ttu-id="3b749-113">Ezután alul kattintson a **new** (új) , majd a gyűjtemény „gyors létrehozása” elemre.</span><span class="sxs-lookup"><span data-stu-id="3b749-113">Then continue by clicking **new** on the bottom and "quick creating" a collection.</span></span> <span data-ttu-id="3b749-114">Adjon meg egy nevet, a régiót, az előfizetését, a díjcsomagot és az általunk biztosított „Office Professional 2013” rendszerképet.</span><span class="sxs-lookup"><span data-stu-id="3b749-114">Provide a name, the region, the subscription, the plan and the image "Office Proffesional 2013" that we provide.</span></span>
<span data-ttu-id="3b749-115">![Párbeszédpanel létrehozása](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span><span class="sxs-lookup"><span data-stu-id="3b749-115">![Create Dialog](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span></span>

<span data-ttu-id="3b749-116">Miután kitölti az űrlapot, elindul a gyűjtemény létrehozásának folyamata.</span><span class="sxs-lookup"><span data-stu-id="3b749-116">Once you finish the form the collection creation process should start.</span></span> <span data-ttu-id="3b749-117">Ez akár egy órát is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="3b749-117">This may take up to an hour or so.</span></span>

![Várakozás](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

<span data-ttu-id="3b749-119">Miután a folyamat befejeződik, a következőhöz hasonlóan fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="3b749-119">Once the process is done, it will look something like this.</span></span> <span data-ttu-id="3b749-120">Ha a **Publishing** (Közzététel) elemre kattint, láthatja, hogy a legtöbb Office-alkalmazás már közzé van téve.</span><span class="sxs-lookup"><span data-stu-id="3b749-120">If we click **Publishing** we can see that most Office applications have been published for us already.</span></span>
<span data-ttu-id="3b749-121">![Létrehozott gyűjtemény](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span><span class="sxs-lookup"><span data-stu-id="3b749-121">![Collection created](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span></span>

![Közzétett alkalmazások](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

<span data-ttu-id="3b749-123">Ezen a ponton a **User Access** (Felhasználói hozzáférés) elemre kattintva felvehet több felhasználót, akik hozzáférhetnek a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="3b749-123">At this point you can also add more users that have access to this collection by clicking **User Access**.</span></span>
<span data-ttu-id="3b749-124">![Felhasználói hozzáférés konfigurálása](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span><span class="sxs-lookup"><span data-stu-id="3b749-124">![Configure user access](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span></span>

<span data-ttu-id="3b749-125">Most próbáljon meg csatlakozni az Office 365-höz.</span><span class="sxs-lookup"><span data-stu-id="3b749-125">Now let's try out connecting to Office 365!</span></span>

## <a name="connect-to-office-365"></a><span data-ttu-id="3b749-126">Csatlakozás az Office 365-höz</span><span class="sxs-lookup"><span data-stu-id="3b749-126">Connect to Office 365</span></span>
<span data-ttu-id="3b749-127">Nyissa meg a [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/) webhelyet, görgessen a lap aljára, és a **Download clients** (Ügyfelek letöltése) elemre kattintva telepítse az Azure RemoteApp-ügyfelet az épp használt eszközre.</span><span class="sxs-lookup"><span data-stu-id="3b749-127">We'll head over to [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), scroll down  and click **Download clients** to install the Azure RemoteApp client on the device you're on.</span></span> <span data-ttu-id="3b749-128">Az alábbi képernyőképek a Windowsra vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="3b749-128">The screenshots below are for Windows.</span></span>

<span data-ttu-id="3b749-129">Miután elindul az alkalmazás, a rendszer felkéri, hogy jelentkezzen be Microsoft-fiókjával (korábban „Live ID”). Egyelőre használja ugyanazt, mint amit az Azure-fiókhoz használt.</span><span class="sxs-lookup"><span data-stu-id="3b749-129">Once the application starts you'll be asked to sign in with your Microsoft account (formerly called a "Live ID"), use the same one as your Azure account for now.</span></span> <span data-ttu-id="3b749-130">Amikor bejelentkezik, egy értesítés jelenik meg az új meghívókról, kattintson rá, ekkor az alábbihoz hasonló listának kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="3b749-130">When you're signed in you should see a notification about new invitations, click there and you should see a list like one below.</span></span> <span data-ttu-id="3b749-131">Kattintson arra a meghívóra, amelyben az Azure-fiók tulajdonosának e-mail-címe szerepel.</span><span class="sxs-lookup"><span data-stu-id="3b749-131">Accept the invitation that matches your Azure account owner email.</span></span>

![Új meghívó](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

<span data-ttu-id="3b749-133">Így néz ki, ha vannak új meghívók.</span><span class="sxs-lookup"><span data-stu-id="3b749-133">What it looks like when there are new invitations.</span></span>

![Egy alkalmazás elfogadása](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

<span data-ttu-id="3b749-135">Miután elfogadja a meghívót, az összes Office-alkalmazás látható az Azure RemoteApp-ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="3b749-135">Once you accept the invitation you should see all the Office apps in the Azure RemoteApp client.</span></span>

![Alkalmazások listája](./media/remoteapp-tutorial-o365anywhere/9-work.png)

<span data-ttu-id="3b749-137">Ha bármelyik alkalmazásra kattint, az alkalmazás elindul az Azure virtuális gépen, és készen áll a használatra.</span><span class="sxs-lookup"><span data-stu-id="3b749-137">When you click on any of these the application should start on the Azure virtual machine and you should be all set!</span></span> <span data-ttu-id="3b749-138">Jó munkát!</span><span class="sxs-lookup"><span data-stu-id="3b749-138">Enjoy!</span></span>

![indítás](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![powerpoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)

