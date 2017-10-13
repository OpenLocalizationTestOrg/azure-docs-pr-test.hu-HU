---
title: "aaaAzure RemoteApp-Licenckezelés |} Microsoft Docs"
description: "Ismerje meg az Azure RemoteApp licenckezelését."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: ff8ebd20-61a1-4f10-87a6-234a170534c9
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: dfa808a65ea6b1a78bf74f3daddb9a84e186eebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-licensing-work-in-azure-remoteapp"></a><span data-ttu-id="62f57-103">Hogyan működik a licenckezelés az Azure RemoteAppban?</span><span class="sxs-lookup"><span data-stu-id="62f57-103">How does licensing work in Azure RemoteApp?</span></span>
> [!IMPORTANT]
> <span data-ttu-id="62f57-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="62f57-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="62f57-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="62f57-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="62f57-106">Úgy állított be az Azure remoteappot, létrehozta a sablonokat, és készen áll a toopublish alkalmazások tooyour felhasználók.</span><span class="sxs-lookup"><span data-stu-id="62f57-106">So you've set up your Azure RemoteApp service, created your templates, and are ready toopublish apps tooyour users.</span></span> <span data-ttu-id="62f57-107">Azonban továbbra is fennáll, egy utolsó lépésként toofigure: licencelési.</span><span class="sxs-lookup"><span data-stu-id="62f57-107">But there's still one last thing toofigure out: licensing.</span></span> <span data-ttu-id="62f57-108">Csak hogyan működik a RemoteApp és Remoteappen keresztül megosztott hello alkalmazások licencelési munkahelyi?</span><span class="sxs-lookup"><span data-stu-id="62f57-108">Just how does licensing work for RemoteApp and for hello apps you share through RemoteApp?</span></span>

<span data-ttu-id="62f57-109">A RemoteApp használatához nincs szükség semmilyen Windows-licencre vagy távoli asztali ügyféllicencre.</span><span class="sxs-lookup"><span data-stu-id="62f57-109">RemoteApp does not require any Windows licenses or Remote Desktop CALs.</span></span> <span data-ttu-id="62f57-110">Az előfizetés gondoskodik a RemoteApp oldaláról hello.</span><span class="sxs-lookup"><span data-stu-id="62f57-110">Your subscription takes care of hello RemoteApp side itself.</span></span> <span data-ttu-id="62f57-111">(Kivétel hello hello részleteit [díjszabások](https://azure.microsoft.com/pricing/details/remoteapp).)</span><span class="sxs-lookup"><span data-stu-id="62f57-111">(Check out hello details of hello [pricing plans](https://azure.microsoft.com/pricing/details/remoteapp).)</span></span>

<span data-ttu-id="62f57-112">Az előfizetéshez mellékelt rendszerképet hello használatakor megoszthatja, különálló licenc nélkül rendszerképen telepített hello alkalmazások bármelyikét.</span><span class="sxs-lookup"><span data-stu-id="62f57-112">If you use one of hello images that is included in your subscription, you can share any of hello apps installed on that image without needing a separate license.</span></span> <span data-ttu-id="62f57-113">Például ha hello Windows Server 2012 R2 sablon kép toobuild használja a gyűjtemény, megoszthatja a System Center Endpoint Protection a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="62f57-113">For example, if you use hello Windows Server 2012 R2 template image toobuild your collection, you can share System Center Endpoint Protection with your users.</span></span> <span data-ttu-id="62f57-114">hello csak a kivételek toothis szabály olyan Office 365 ProPlus, amelyhez különálló előfizetés szükséges, és Office 2013, amely nem osztható meg termelési gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="62f57-114">hello only exceptions toothis rule are Office 365 ProPlus, which requires a separate subscription, and Office 2013, which cannot be shared in a production collection.</span></span>

<span data-ttu-id="62f57-115">Ha azt szeretné, hogy a Remoteapphoz mellékelt toouse hello Office 365 sablon rendszerképet, rendelkeznie kell egy *meglévő* Office 365 ProPlus terv.</span><span class="sxs-lookup"><span data-stu-id="62f57-115">If you want toouse hello Office 365 template image that comes with RemoteApp, you must have an *existing* Office 365 ProPlus plan.</span></span> <span data-ttu-id="62f57-116">hello bármely Office 365-alkalmazás tesz közzé egy egyéni sablon használatával esetében is igaz.</span><span class="sxs-lookup"><span data-stu-id="62f57-116">hello same is true for any Office 365 app that you publish using a custom template.</span></span> <span data-ttu-id="62f57-117">A saját előfizetésével kell tooactivate hello alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="62f57-117">You need tooactivate hello apps with your own subscription.</span></span> <span data-ttu-id="62f57-118">Ez a próba- és a fizetett előfizetésekre is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="62f57-118">This is true for both trial and paid subscriptions.</span></span> <span data-ttu-id="62f57-119">Ha azt szeretné toouse hello Office 365 sablon rendszerképet hello próbaidőszakban *és még nem rendelkezik előfizetéssel*, toohello Office 365-oldal túl lépjen[regisztráljon](https://go.microsoft.com/fwlink/p/?LinkID=403802) egy próba-előfizetésre vonatkozó.</span><span class="sxs-lookup"><span data-stu-id="62f57-119">If you want toouse hello Office 365 template image during hello trial, *and you don't already have a subscription*, go toohello Office 365 page too[sign up](https://go.microsoft.com/fwlink/p/?LinkID=403802) for a trial subscription.</span></span> <span data-ttu-id="62f57-120">További információkért lásd a [Hogyan működik együtt a RemoteApp és az Office?](remoteapp-o365.md) (Hogyan működik együtt a RemoteApp és az Office) témakört.</span><span class="sxs-lookup"><span data-stu-id="62f57-120">See [How do RemoteApp and Office work together?](remoteapp-o365.md) for more information.</span></span>

<span data-ttu-id="62f57-121">Ha hello próbaverziós időszak alatt nem kívánja tooget az Office 365 próba-előfizetést, használja a Remoteapphoz mellékelt hello Office 2013 Professional Plus sablon rendszerképet.</span><span class="sxs-lookup"><span data-stu-id="62f57-121">If, during hello trial period, you don't want tooget an Office 365 trial subscription, use hello Office 2013 Professional Plus template image that comes with RemoteApp.</span></span> <span data-ttu-id="62f57-122">Ez a sablon csak 30 napig használható, és alakítható át fizetett gyűjteménnyé.</span><span class="sxs-lookup"><span data-stu-id="62f57-122">This template image can only be used for 30 days and cannot be converted into a paid collection.</span></span>

<span data-ttu-id="62f57-123">Más alkalmazások esetén van szüksége, hogy rendelkezik-e hello licenc tooshare hello app toomake.</span><span class="sxs-lookup"><span data-stu-id="62f57-123">For other apps, you need toomake sure that you have hello license tooshare hello app.</span></span>

<span data-ttu-id="62f57-124">Ez így érthető, ugye?</span><span class="sxs-lookup"><span data-stu-id="62f57-124">That makes sense, right?</span></span> <span data-ttu-id="62f57-125">Közzéteheti a kívánt alkalmazást, hogy tooshare jogilag jogosultak.</span><span class="sxs-lookup"><span data-stu-id="62f57-125">You can publish any app that you are legally entitled tooshare.</span></span> <span data-ttu-id="62f57-126">És meg kell toomake meg arról, hogy valóban jogosult tooshare a programokat.</span><span class="sxs-lookup"><span data-stu-id="62f57-126">And you need toomake sure you really are entitled tooshare your programs.</span></span>

<span data-ttu-id="62f57-127">Vegye figyelembe, hogy felhőalapú gyűjtemény esetében nem használhat ügyféllicencet vagy mennyiségi licencelési megállapodást.</span><span class="sxs-lookup"><span data-stu-id="62f57-127">Please note that you cannot use a CAL or Volume License agreement in a cloud collection.</span></span> <span data-ttu-id="62f57-128">Ön *is* a mennyiségi licencelési megállapodást tooactivate alkalmazások használata a hibrid gyűjtemények (kivéve az Office esetében).</span><span class="sxs-lookup"><span data-stu-id="62f57-128">You *can* use a Volume License agreement tooactivate applications in your hybrid collection (except for Office).</span></span> <span data-ttu-id="62f57-129">Csupán be kell őket a sablon hello mennyiségi licencelési adathordozóról kép tooinstall.</span><span class="sxs-lookup"><span data-stu-id="62f57-129">You just need tooinstall them on your template image from hello Volume License media.</span></span> <span data-ttu-id="62f57-130">Kövesse hello információk hello alkalmazás szállító tooinstall licencek távoli asztali környezetben.</span><span class="sxs-lookup"><span data-stu-id="62f57-130">Follow hello information from hello application vendor tooinstall licenses in a Remote Desktop environment.</span></span>
