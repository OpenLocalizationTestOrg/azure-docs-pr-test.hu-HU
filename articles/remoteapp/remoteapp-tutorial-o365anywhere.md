---
title: "aaaGet hello azonos Office 365-élmény bármely Azure RemoteApp eszközön |} Microsoft Docs"
description: "Megtudhatja, hogyan tooshare bármilyen Office 365-alkalmazást a felhasználóival az Azure RemoteApp segítségével."
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
ms.openlocfilehash: 140056c22c8c69b9ec605318e35a72b144da07eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-same-office-365-experience-on-any-device-with-azure-remoteapp"></a><span data-ttu-id="1fe82-103">Get hello azonos Office 365-élmény bármely Azure RemoteApp eszközön</span><span class="sxs-lookup"><span data-stu-id="1fe82-103">Get hello same Office 365 experience on any device with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1fe82-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="1fe82-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="1fe82-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="1fe82-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="1fe82-106">Ez a cikk hogyan toodeploy Office 365 a vállalat bármely eszközön.</span><span class="sxs-lookup"><span data-stu-id="1fe82-106">This article will cover how toodeploy Office 365 on any device in your company.</span></span> <span data-ttu-id="1fe82-107">A felhasználók férhetnek hello ugyanazokat a képességeket, és Android-, Apple- és Windows felhasználói felület tapasztalattal.</span><span class="sxs-lookup"><span data-stu-id="1fe82-107">Your users can get hello same capabilities and UI experience on Android, Apple and Windows.</span></span>

<span data-ttu-id="1fe82-108">Ezt az Azure RemoteApp használatával érheti el az Office 365 méretezhető virtuális gépeken való üzemeltetésével az Azure-ban, amelyekhez a felhasználók csatlakozhatnak.</span><span class="sxs-lookup"><span data-stu-id="1fe82-108">We will accomplish this using Azure RemoteApp by hosting Office 365 on scale-able virtual machines in Azure that users can connect to.</span></span> <span data-ttu-id="1fe82-109">A virtuális gépek ezen készletét „felhőalapú gyűjteménynek” nevezik.</span><span class="sxs-lookup"><span data-stu-id="1fe82-109">This set of virtual machines we call a "cloud collection".</span></span>

## <a name="create-a-cloud-collection"></a><span data-ttu-id="1fe82-110">Felhőalapú gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="1fe82-110">Create a cloud collection</span></span>
<span data-ttu-id="1fe82-111">Először az Azure-fiók létrehozását követően nyissa meg túl**RemoteApp** bal oldali hello hello hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="1fe82-111">First after you have created an Azure account, navigate too**RemoteApp** by clicking on hello link on hello left side.</span></span>
<span data-ttu-id="1fe82-112">![Azure RemoteApp ábrázoló hello Azure portál](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span><span class="sxs-lookup"><span data-stu-id="1fe82-112">![Showing Azure RemoteApp on hello Azure Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span></span>

<span data-ttu-id="1fe82-113">Folytassa a kattintva **új** hello alsó és a "gyors létrehozása" gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="1fe82-113">Then continue by clicking **new** on hello bottom and "quick creating" a collection.</span></span> <span data-ttu-id="1fe82-114">Adjon meg nevet, hello régió, hello előfizetés, hello terv és hello kép "Office Professional 2013", amely a Microsoft biztosítja.</span><span class="sxs-lookup"><span data-stu-id="1fe82-114">Provide a name, hello region, hello subscription, hello plan and hello image "Office Proffesional 2013" that we provide.</span></span>
<span data-ttu-id="1fe82-115">![Párbeszédpanel létrehozása](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span><span class="sxs-lookup"><span data-stu-id="1fe82-115">![Create Dialog](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span></span>

<span data-ttu-id="1fe82-116">Miután befejezte a hello űrlap hello gyűjtemény létrehozásának folyamata elindul.</span><span class="sxs-lookup"><span data-stu-id="1fe82-116">Once you finish hello form hello collection creation process should start.</span></span> <span data-ttu-id="1fe82-117">A is tarthat tooan órát.</span><span class="sxs-lookup"><span data-stu-id="1fe82-117">This may take up tooan hour or so.</span></span>

![Várakozás](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

<span data-ttu-id="1fe82-119">Miután hello folyamat befejeződik, fog megjelenni ehhez hasonló.</span><span class="sxs-lookup"><span data-stu-id="1fe82-119">Once hello process is done, it will look something like this.</span></span> <span data-ttu-id="1fe82-120">Ha a **Publishing** (Közzététel) elemre kattint, láthatja, hogy a legtöbb Office-alkalmazás már közzé van téve.</span><span class="sxs-lookup"><span data-stu-id="1fe82-120">If we click **Publishing** we can see that most Office applications have been published for us already.</span></span>
<span data-ttu-id="1fe82-121">![Létrehozott gyűjtemény](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span><span class="sxs-lookup"><span data-stu-id="1fe82-121">![Collection created](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span></span>

![Közzétett alkalmazások](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

<span data-ttu-id="1fe82-123">Ezen a ponton is hozzáadhat további felhasználóknak, akik számára hozzáférést toothis gyűjtemény kattintva **felhasználói hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="1fe82-123">At this point you can also add more users that have access toothis collection by clicking **User Access**.</span></span>
<span data-ttu-id="1fe82-124">![Felhasználói hozzáférés konfigurálása](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span><span class="sxs-lookup"><span data-stu-id="1fe82-124">![Configure user access](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span></span>

<span data-ttu-id="1fe82-125">Most próbáljon meg csatlakozni tooOffice 365!</span><span class="sxs-lookup"><span data-stu-id="1fe82-125">Now let's try out connecting tooOffice 365!</span></span>

## <a name="connect-toooffice-365"></a><span data-ttu-id="1fe82-126">Csatlakozás tooOffice 365</span><span class="sxs-lookup"><span data-stu-id="1fe82-126">Connect tooOffice 365</span></span>
<span data-ttu-id="1fe82-127">Azt fogja látogasson túl[https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), görgessen lefelé, és kattintson a **le ügyfeleket** tooinstall hello Azure RemoteApp-ügyfelet a hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="1fe82-127">We'll head over too[https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), scroll down  and click **Download clients** tooinstall hello Azure RemoteApp client on hello device you're on.</span></span> <span data-ttu-id="1fe82-128">hello az alábbi képernyőképek a Windowsra vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="1fe82-128">hello screenshots below are for Windows.</span></span>

<span data-ttu-id="1fe82-129">Hello alkalmazás indítása után meg kell adnia toosign be a Microsoft-fiókjával (korábban "Live ID"), használjon most hello ugyanazt, mint amit az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="1fe82-129">Once hello application starts you'll be asked toosign in with your Microsoft account (formerly called a "Live ID"), use hello same one as your Azure account for now.</span></span> <span data-ttu-id="1fe82-130">Amikor bejelentkezik, egy értesítés jelenik meg az új meghívókról, kattintson rá, ekkor az alábbihoz hasonló listának kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="1fe82-130">When you're signed in you should see a notification about new invitations, click there and you should see a list like one below.</span></span> <span data-ttu-id="1fe82-131">Fogadja el a hello meghívóra, amelyben az Azure-fiók tulajdonosának e-mail címe.</span><span class="sxs-lookup"><span data-stu-id="1fe82-131">Accept hello invitation that matches your Azure account owner email.</span></span>

![Új meghívó](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

<span data-ttu-id="1fe82-133">Így néz ki, ha vannak új meghívók.</span><span class="sxs-lookup"><span data-stu-id="1fe82-133">What it looks like when there are new invitations.</span></span>

![Egy alkalmazás elfogadása](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

<span data-ttu-id="1fe82-135">Hello meghívó elfogadása után megtekintheti az összes hello Office-alkalmazásokba hello Azure RemoteApp-ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="1fe82-135">Once you accept hello invitation you should see all hello Office apps in hello Azure RemoteApp client.</span></span>

![Alkalmazások listája](./media/remoteapp-tutorial-o365anywhere/9-work.png)

<span data-ttu-id="1fe82-137">Elemre bármelyik ezek hello alkalmazás elindul az hello Azure virtuális gép, és minden beállítás el!</span><span class="sxs-lookup"><span data-stu-id="1fe82-137">When you click on any of these hello application should start on hello Azure virtual machine and you should be all set!</span></span> <span data-ttu-id="1fe82-138">Jó munkát!</span><span class="sxs-lookup"><span data-stu-id="1fe82-138">Enjoy!</span></span>

![indítás](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![powerpoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)

