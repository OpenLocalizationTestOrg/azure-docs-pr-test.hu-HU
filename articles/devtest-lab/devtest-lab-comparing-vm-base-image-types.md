---
title: "Az egyéni lemezképek és a DevTest Labs szolgáltatásban képletek összehasonlítása |} Microsoft Docs"
description: "További információk a egyéni lemezképek és a formulák közötti különbségeket, a virtuális gép alapjait, eldöntheti, melyik legjobban megfelel a környezetben."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a3cb259a-7d80-40ec-8ee8-45105704d589
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: tarcher
ms.openlocfilehash: ff771abc26c08f0adb977c29739d2f5c91924b21
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a><span data-ttu-id="1fea1-103">Az egyéni lemezképek és a DevTest Labs szolgáltatásban képletek összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="1fea1-103">Comparing custom images and formulas in DevTest Labs</span></span>
<span data-ttu-id="1fea1-104">Mindkét [egyéni lemezképek](devtest-lab-create-template.md) és [képletek](devtest-lab-manage-formulas.md) alapjait használható [létrehozott új virtuális gépek](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="1fea1-104">Both [custom images](devtest-lab-create-template.md) and [formulas](devtest-lab-manage-formulas.md) can be used as bases for [created new VMs](devtest-lab-add-vm-with-artifacts.md).</span></span> <span data-ttu-id="1fea1-105">Azonban a fő különbség a között egyéni lemezképek és a formulák az, hogy egy egyéni lemezképet egyszerűen csak egy virtuális merevlemez alapján, amíg egy képlete virtuális merevlemez alapuló rendszerképet kép *kívül* előre konfigurált beállítások – például a Virtuálisgép-méretet, a virtuális hálózati, a alhálózati és az összetevők.</span><span class="sxs-lookup"><span data-stu-id="1fea1-105">However, the key distinction between custom images and formulas is that a custom image is simply an image based on a VHD, while a formula is an image based on a VHD *in addition to* preconfigured settings - such as VM Size, virtual network, subnet, and artifacts.</span></span> <span data-ttu-id="1fea1-106">Ezek előre konfigurált beállítások vannak beállítva, hogy a virtuális gép létrehozásakor felülbírálhatja az alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="1fea1-106">These preconfigured settings are set up with default values that can be overridden at the time of VM creation.</span></span> <span data-ttu-id="1fea1-107">Ez a cikk ismerteti, néhány (szakemberek számára) előnyeit és hátrányait (hátrányait) használatával egyéni lemezképek és képletek alapján.</span><span class="sxs-lookup"><span data-stu-id="1fea1-107">This article explains some of the advantages (pros) and disadvantages (cons) to using custom images versus using formulas.</span></span>

## <a name="custom-image-pros-and-cons"></a><span data-ttu-id="1fea1-108">Kép: egyéni előnyei és hátrányai</span><span class="sxs-lookup"><span data-stu-id="1fea1-108">Custom image pros and cons</span></span>
<span data-ttu-id="1fea1-109">Egyéni lemezképek lehetőséget nyújtanak olyan statikus és nem módosítható a kívánt környezetből virtuális gépek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1fea1-109">Custom images provide a static, immutable way to create VMs from a desired environment.</span></span> 

<span data-ttu-id="1fea1-110">**Informatikai szakemberek**</span><span class="sxs-lookup"><span data-stu-id="1fea1-110">**Pros**</span></span>

* <span data-ttu-id="1fea1-111">Virtuális gép kiépítése egyéni lemezképéről esetén gyors, mert semmi nem változik, miután a virtuális gép van hoz létre a lemezképből.</span><span class="sxs-lookup"><span data-stu-id="1fea1-111">VM provisioning from a custom image is fast as nothing changes after the VM is spun up from the image.</span></span> <span data-ttu-id="1fea1-112">Más szóval nincsenek beállítások alkalmazásához, mert az egyéni lemezképet csak a kép vonatkozó beállítások kihagyásával.</span><span class="sxs-lookup"><span data-stu-id="1fea1-112">In other words, there are no settings to apply as the custom image is just an image without settings.</span></span> 
* <span data-ttu-id="1fea1-113">Egyetlen egyéni lemezkép alapján létrehozott virtuális gépek esetén azonosak.</span><span class="sxs-lookup"><span data-stu-id="1fea1-113">VMs created from a single custom image are identical.</span></span>

<span data-ttu-id="1fea1-114">**Hátrányait**</span><span class="sxs-lookup"><span data-stu-id="1fea1-114">**Cons**</span></span>

* <span data-ttu-id="1fea1-115">Ha módosítania néhány szempontja, hogy az egyéni lemezképet, a lemezkép kell újból létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="1fea1-115">If you need to update some aspect of the custom image, the image must be recreated.</span></span>  

## <a name="formula-pros-and-cons"></a><span data-ttu-id="1fea1-116">Képletadat előnyei és hátrányai</span><span class="sxs-lookup"><span data-stu-id="1fea1-116">Formula pros and cons</span></span>
<span data-ttu-id="1fea1-117">Képletek biztosít a virtuális gépek létrehozásához a kívánt konfigurációs beállítások dinamikus módot.</span><span class="sxs-lookup"><span data-stu-id="1fea1-117">Formulas provide a dynamic way to create VMs from the desired configuration/settings.</span></span>

<span data-ttu-id="1fea1-118">**Informatikai szakemberek**</span><span class="sxs-lookup"><span data-stu-id="1fea1-118">**Pros**</span></span>

* <span data-ttu-id="1fea1-119">A környezet változásai keresztül összetevők parancsprogramok rögzíthetők.</span><span class="sxs-lookup"><span data-stu-id="1fea1-119">Changes in the environment can be captured on the fly via artifacts.</span></span> <span data-ttu-id="1fea1-120">Például ha szeretné, hogy egy virtuális Gépet a legújabb bitként a kiadási láncból telepítve, vagy a legújabb kód besorolni a tárházban lévő, egyszerűen megadhat egy összetevő, amely telepíti a legújabb bits vagy felveszi a legújabb kódot a képlet együtt egy cél alapjául szolgáló lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1fea1-120">For example, if you want a VM installed with the latest bits from your release pipeline or enlist the latest code from your repository, you can simply specify an artifact that deploys the latest bits or enlists the latest code in the formula together with a target base image.</span></span> <span data-ttu-id="1fea1-121">A képlet használatával létrehozott virtuális gépet, amikor a legújabb bits kód, telepített/bejegyezve a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="1fea1-121">Whenever this formula is used to create VMs, the latest bits/code are deployed/enlisted to the VM.</span></span> 
* <span data-ttu-id="1fea1-122">Képletek alapértelmezett beállításokat, az egyéni lemezképek nem adhatók meg – például a Virtuálisgép-méretek és a virtuális hálózati beállításokat adhat meg.</span><span class="sxs-lookup"><span data-stu-id="1fea1-122">Formulas can define default settings that custom images cannot provide - such as VM sizes and virtual network settings.</span></span> 
* <span data-ttu-id="1fea1-123">A képlet mentett beállításokat alapértelmezett értékei láthatók, de a virtuális gép létrehozásakor módosítható.</span><span class="sxs-lookup"><span data-stu-id="1fea1-123">The settings saved in a formula are shown as default values, but can be modified when the VM is created.</span></span> 

<span data-ttu-id="1fea1-124">**Hátrányait**</span><span class="sxs-lookup"><span data-stu-id="1fea1-124">**Cons**</span></span>

* <span data-ttu-id="1fea1-125">Virtuális gép létrehozása egy képletet a virtuális gép létrehozása egy egyéni lemezképből több időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="1fea1-125">Creating a VM from a formula can take more time than creating a VM from a custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="1fea1-126">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="1fea1-126">Related blog posts</span></span>
* [<span data-ttu-id="1fea1-127">Egyéni lemezképek vagy képletek?</span><span class="sxs-lookup"><span data-stu-id="1fea1-127">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="1fea1-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1fea1-128">Next steps</span></span>
- [<span data-ttu-id="1fea1-129">DevTest Labs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="1fea1-129">DevTest Labs FAQ</span></span>](devtest-lab-faq.md)