---
title: az Azure RemoteApp tooCitrix XenApp Essentials aaaMigrate |} Microsoft Docs
description: Hogyan toomigrate az Azure RemoteApp tooCitrix XenApp alapjai
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 695a8165-3454-4855-8e21-f2eb2c61201b
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: aa3ce28bc5a86d5b1dd3408196d51935395f55c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-azure-remoteapp-toocitrix-xenapp-essentials"></a><span data-ttu-id="b35b9-103">Azure RemoteApp tooCitrix XenApp Essentials áttelepítése</span><span class="sxs-lookup"><span data-stu-id="b35b9-103">Migrate from Azure RemoteApp tooCitrix XenApp Essentials</span></span>

<span data-ttu-id="b35b9-104">Ha az Azure RemoteApp használ, és azt szeretné, hogy toomigrate tooCitrix XenApp Essentials, nincsenek néhány Előfeltételek tookeep szem előtt.</span><span class="sxs-lookup"><span data-stu-id="b35b9-104">If you use Azure RemoteApp and you want toomigrate tooCitrix XenApp Essentials, there are a few prerequisites tookeep in mind.</span></span> <span data-ttu-id="b35b9-105">Először, olvassa el a Citrix által [részletes műszaki üzembe helyezési útmutató az Citrix XenApp Essentials](https://docs.citrix.com/content/dam/docs/en-us/citrix-cloud/downloads/xenapp-essentials-deployment-guide.pdf) és annak [technikai könyvtárában](http://docs.citrix.com/en-us/citrix-cloud/xenapp-and-xendesktop-service/xenapp-essentials.html).</span><span class="sxs-lookup"><span data-stu-id="b35b9-105">First, read Citrix's [step by step technical deployment guide for Citrix XenApp Essentials](https://docs.citrix.com/content/dam/docs/en-us/citrix-cloud/downloads/xenapp-essentials-deployment-guide.pdf) and its [online technical library](http://docs.citrix.com/en-us/citrix-cloud/xenapp-and-xendesktop-service/xenapp-essentials.html).</span></span> 

## <a name="prerequisite-steps-for-migration"></a><span data-ttu-id="b35b9-106">Áttelepítési Előfeltételek lépései</span><span class="sxs-lookup"><span data-stu-id="b35b9-106">Prerequisite steps for migration</span></span>

1. <span data-ttu-id="b35b9-107">Hozzon létre egy új virtuális hálózatot, vagy felfedheti, mely Azure virtuális hálózat az Azure Resource Manager amelybe Citrix XenApp Essentials fogja telepíteni.</span><span class="sxs-lookup"><span data-stu-id="b35b9-107">Create a new virtual network, or determine which Azure virtual network in Azure Resource Manager into which you'll deploy Citrix XenApp Essentials.</span></span> <span data-ttu-id="b35b9-108">Az Azure RemoteApp használja a klasszikus Azure portálon; hello Citrix XenApp Essentials csak az Azure Resource Manager támogatja.</span><span class="sxs-lookup"><span data-stu-id="b35b9-108">Azure RemoteApp uses hello Azure classic portal; Citrix XenApp Essentials only supports Azure Resource Manager.</span></span>  
2. <span data-ttu-id="b35b9-109">Győződjön meg arról, kiválasztott hello virtuális hálózatban rendelkezik hálózati hozzáférési tooyour tartományvezérlő, mivel a Citrix csak hibrid telepítések támogatja.</span><span class="sxs-lookup"><span data-stu-id="b35b9-109">Ensure that hello virtual network you selected has networking access tooyour domain controller, because Citrix only supports hybrid deployments.</span></span> <span data-ttu-id="b35b9-110">Ha egy felhő üzembe helyezése az Azure RemoteApp használ, győződjön meg arról, hogy a virtuális hálózat rendelkezik-e a hálózati hozzáférés tooan Active Directory-tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="b35b9-110">If you are using a cloud deployment of Azure RemoteApp, ensure that your virtual network has networking access tooan Active Directory domain controller.</span></span> <span data-ttu-id="b35b9-111">Azure Active Directory tartományi szolgáltatások (az Azure Active Directory tartományi szolgáltatások) is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b35b9-111">You can also use Azure Active Directory Domain Services (Azure AD DS).</span></span> 
3. <span data-ttu-id="b35b9-112">Győződjön meg arról, hogy hello DNS helyesen van konfigurálva a hello virtuális hálózathoz, így az első kísérlet sikeres, hogy tartományhoz való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="b35b9-112">Ensure that hello DNS is properly configured for hello virtual network, so that domain join is successful on your first attempt.</span></span> <span data-ttu-id="b35b9-113">Hozzon létre egy virtuális gép (VM) hello kiválasztott virtuális hálózat, és hajtsa végre a manuális tartományhoz csatlakoztatás tooverify adott hello DNS és a tartományhoz való csatlakozást megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="b35b9-113">You can create a virtual machine (VM) in hello virtual network you selected, and perform a manual domain join tooverify that hello DNS and domain join works as expected.</span></span> <span data-ttu-id="b35b9-114">Ez biztosítja, hogy Ön sikeres hello először Citrix XenApp Essentials telepít.</span><span class="sxs-lookup"><span data-stu-id="b35b9-114">This ensures that you are successful hello first time you deploy Citrix XenApp Essentials.</span></span> 
4. <span data-ttu-id="b35b9-115">Szükség esetén hozzon létre egy virtuális hálózati társviszony-létesítés Azure klasszikus portál virtuális hálózat az Azure RemoteApp használ, és az Azure Resource Manager virtuális hálózat között.</span><span class="sxs-lookup"><span data-stu-id="b35b9-115">If needed, create a virtual network peering between an Azure classic portal virtual network you are using with Azure RemoteApp, and your Azure Resource Manager virtual network.</span></span> <span data-ttu-id="b35b9-116">A társviszony-létesítési folyamat akkor működik, ha a két hello hálózatok találhatók hello azonos régióban.</span><span class="sxs-lookup"><span data-stu-id="b35b9-116">This peering process works if hello two networks reside in hello same region.</span></span> <span data-ttu-id="b35b9-117">Ha nem, használja a pont-pont VPN tooconnect hello virtuális hálózatok a hálózatkezeléshez.</span><span class="sxs-lookup"><span data-stu-id="b35b9-117">If they do not, use site-to-site VPN tooconnect hello virtual networks for networking.</span></span> 
5. <span data-ttu-id="b35b9-118">Ha szükséges, olvassa el [hogyan toomigrate adatok Azure RemoteApp-](remoteapp-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="b35b9-118">If needed, read [How toomigrate data into and out of Azure RemoteApp](remoteapp-migrate.md).</span></span> 
6. <span data-ttu-id="b35b9-119">Frissítse a meglévő Azure RemoteApp kép tooinclude hello Citrix VDA összetevő (útmutatásért lásd a hello Citrix dokumentációt).</span><span class="sxs-lookup"><span data-stu-id="b35b9-119">Update your existing Azure RemoteApp image tooinclude hello Citrix VDA component (for instructions, see hello Citrix documentation).</span></span> 
7. <span data-ttu-id="b35b9-120">Nyissa meg toohello Azure piactéren, és a Citrix XenApp Essentials telepítésének megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="b35b9-120">Go toohello Azure Marketplace, and begin Citrix XenApp Essentials deployment.</span></span>

## <a name="other-considerations"></a><span data-ttu-id="b35b9-121">Egyéb szempontok</span><span class="sxs-lookup"><span data-stu-id="b35b9-121">Other considerations</span></span>

<span data-ttu-id="b35b9-122">Vegye figyelembe a következő további szempontok áttelepítésekor hello:</span><span class="sxs-lookup"><span data-stu-id="b35b9-122">Be aware of hello following additional considerations when you migrate:</span></span>
- <span data-ttu-id="b35b9-123">Citrix XenApp Essentials csak a hibrid telepítések támogatja.</span><span class="sxs-lookup"><span data-stu-id="b35b9-123">Citrix XenApp Essentials only supports hybrid deployments.</span></span> <span data-ttu-id="b35b9-124">Más szóval ez igényli hálózati hozzáférési tooa tartományvezérlőjének rendelés tooperform tartományhoz való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="b35b9-124">In other words, it requires network access tooa domain controller in order tooperform domain join.</span></span> <span data-ttu-id="b35b9-125">Ha egy felhő üzembe helyezése az Azure RemoteApp használata esetén használja az Azure Active Directory tartományi szolgáltatások vagy ügyeljen arra, hogy a virtuális hálózati hozzáférési tooActive Directory a tartományhoz való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="b35b9-125">If you are using a cloud deployment of Azure RemoteApp, either use Azure AD DS or ensure that your virtual network has access tooActive Directory for domain join.</span></span> 
- <span data-ttu-id="b35b9-126">Hogyan toomove felhasználói adatok tooCitrix XenApp Essentials: toolearn [hogyan toomigrate adatok Azure RemoteApp-](remoteapp-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="b35b9-126">toolearn how toomove user data tooCitrix XenApp Essentials, see [How toomigrate data into and out of Azure RemoteApp](remoteapp-migrate.md).</span></span> 
- <span data-ttu-id="b35b9-127">Citrix XenApp Essentials csak az Active Directory-fiókokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="b35b9-127">Citrix XenApp Essentials only supports Active Directory accounts.</span></span> <span data-ttu-id="b35b9-128">Microsoft-fiókok (például outlook.com, msn.com vagy hotmail.com) nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="b35b9-128">It does not support Microsoft accounts (such as outlook.com, msn.com, or hotmail.com).</span></span> 

## <a name="citrix-xenapp-essentials-billing"></a><span data-ttu-id="b35b9-129">Citrix XenApp Essentials számlázási</span><span class="sxs-lookup"><span data-stu-id="b35b9-129">Citrix XenApp Essentials billing</span></span>

<span data-ttu-id="b35b9-130">Az árakkal kapcsolatos teljes részletekért lásd: hello [gyakran ismételt kérdések](https://www.citrix.com/global-partners/microsoft/resources/xenapp-essentials-faq.html#tab-30699) és [Citrix a cikk áttekintése](https://www.citrix.com/global-partners/microsoft/remote-app.html).</span><span class="sxs-lookup"><span data-stu-id="b35b9-130">For full details on pricing, see hello [FAQ](https://www.citrix.com/global-partners/microsoft/resources/xenapp-essentials-faq.html#tab-30699) and [Citrix overview article](https://www.citrix.com/global-partners/microsoft/remote-app.html).</span></span> <span data-ttu-id="b35b9-131">Három számlázási összetevőből áll tooCitrix XenApp Essentials:</span><span class="sxs-lookup"><span data-stu-id="b35b9-131">There are three billing components tooCitrix XenApp Essentials:</span></span>

- <span data-ttu-id="b35b9-132">hello Citrix szolgáltatás kell fizetni, amely 12 $ felhasználónként, havonta.</span><span class="sxs-lookup"><span data-stu-id="b35b9-132">hello Citrix service charge, which is $12 per user per month.</span></span> <span data-ttu-id="b35b9-133">Az összes Azure piactéren vásárlás, például ez az az Azure-előfizetéshez társított számlázott toohello fizetési módot.</span><span class="sxs-lookup"><span data-stu-id="b35b9-133">Like all Azure Marketplace purchases, this is billed toohello payment method associated with your Azure subscription.</span></span> <span data-ttu-id="b35b9-134">A nagyvállalati szerződés (EA) ügyfelek Azure-krediteket pénzügyi nem használható.</span><span class="sxs-lookup"><span data-stu-id="b35b9-134">For Enterprise Agreement (EA) customers, Azure monetary credits cannot be used.</span></span> 
- <span data-ttu-id="b35b9-135">Távoli adatok szolgáltatások (RDS) ügyféllicenc (CAL).</span><span class="sxs-lookup"><span data-stu-id="b35b9-135">Remote Data Services (RDS) client access licenses (CALs).</span></span> <span data-ttu-id="b35b9-136">Jelenleg $6.25 hello van kötegelt távoli hozzáférési díj a Citrix XenApp Essentials fizetési hello lehet vásárolni.</span><span class="sxs-lookup"><span data-stu-id="b35b9-136">Currently, you can purchase hello Remote Access Fee that is bundled with hello Citrix XenApp Essentials payment for $6.25.</span></span> <span data-ttu-id="b35b9-137">Ha Ön egy EA ügyfél, a pénzügyi Azure-krediteket toopay is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b35b9-137">If you are an EA customer, you can use Azure monetary credits toopay for this.</span></span> <span data-ttu-id="b35b9-138">Ha azt szeretné, toouse a meglévő távoli asztali szolgáltatások ügyféllicenceit, írjon nekünk az [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com), így a tooyour számlázási érvénybe lépni.</span><span class="sxs-lookup"><span data-stu-id="b35b9-138">If you want toouse your existing RDS CALs, contact us at [arainfo@microsoft.com](mailto:arainfo@microsoft.com), so we can apply this tooyour bill.</span></span> 
- <span data-ttu-id="b35b9-139">Az Azure számítási és tárolási.</span><span class="sxs-lookup"><span data-stu-id="b35b9-139">Azure compute and storage.</span></span> <span data-ttu-id="b35b9-140">Ez hello Azure tárolási költségű és felhasznált számítási fogyasztás hello virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="b35b9-140">This is hello Azure storage cost and compute consumption for hello VMs consumed.</span></span> <span data-ttu-id="b35b9-141">Ügyeljen rá, lehetőséget választva a virtuális gép méretét és a felhasználó sűrűség árképzési.</span><span class="sxs-lookup"><span data-stu-id="b35b9-141">Be aware of pricing when selecting your VM size and user density.</span></span> <span data-ttu-id="b35b9-142">Ha Ön egy EA ügyfél, a pénzügyi Azure-krediteket toopay is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b35b9-142">If you are an EA customer, you can use Azure monetary credits toopay for this.</span></span>

<span data-ttu-id="b35b9-143">Ha további kérdései, akkor a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="b35b9-143">If you still have questions, you can:</span></span>
- <span data-ttu-id="b35b9-144">E-mailben nekünk az [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b35b9-144">Email us at [arainfo@microsoft.com](mailto:arainfo@microsoft.com).</span></span>
- <span data-ttu-id="b35b9-145">[Kérje az Azure támogatási](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="b35b9-145">[Contact Azure support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="b35b9-146">Első lépésként [megnyitása az Azure támogatási esetet](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) toohelp az előfeltételként szükséges lépések 1-5.</span><span class="sxs-lookup"><span data-stu-id="b35b9-146">Start by [opening an Azure support case](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) toohelp with prerequisite steps 1-5.</span></span> <span data-ttu-id="b35b9-147">A lépéseket 6-7 lépjen kapcsolatba a Citrix hello Citrix felügyeleti portálon támogatási jegy megnyitásával.</span><span class="sxs-lookup"><span data-stu-id="b35b9-147">For steps 6-7, contact Citrix by opening a support ticket within hello Citrix management portal.</span></span> 