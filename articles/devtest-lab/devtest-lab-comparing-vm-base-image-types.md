---
title: "aaaComparing egyéni lemezképek és a formulák a DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Egyéni lemezképek és a formulák hello különbségei ismerje meg, a virtuális gép alapjait, eldöntheti, melyik legjobban megfelel a környezetben."
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
ms.openlocfilehash: 3c1d88dfe0ff94b8e825bb7a0b4aca3341c9330d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a><span data-ttu-id="96985-103">Az egyéni lemezképek és a DevTest Labs szolgáltatásban képletek összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="96985-103">Comparing custom images and formulas in DevTest Labs</span></span>
<span data-ttu-id="96985-104">Mindkét [egyéni lemezképek](devtest-lab-create-template.md) és [képletek](devtest-lab-manage-formulas.md) alapjait használható [létrehozott új virtuális gépek](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="96985-104">Both [custom images](devtest-lab-create-template.md) and [formulas](devtest-lab-manage-formulas.md) can be used as bases for [created new VMs](devtest-lab-add-vm-with-artifacts.md).</span></span> <span data-ttu-id="96985-105">Azonban hello a fő különbség a között egyéni lemezképek és a formulák az, hogy egy egyéni lemezképet egyszerűen csak egy virtuális merevlemez alapján, amíg egy képlete virtuális merevlemez alapuló rendszerképet kép *kívül* előre konfigurált beállítások – például a Virtuálisgép-méretet, a virtuális hálózat alhálózat, és az összetevők.</span><span class="sxs-lookup"><span data-stu-id="96985-105">However, hello key distinction between custom images and formulas is that a custom image is simply an image based on a VHD, while a formula is an image based on a VHD *in addition to* preconfigured settings - such as VM Size, virtual network, subnet, and artifacts.</span></span> <span data-ttu-id="96985-106">Ezek előre konfigurált beállítások vannak beállítva, hogy a virtuális gép létrehozása hello időpontjában felülbírálható az alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="96985-106">These preconfigured settings are set up with default values that can be overridden at hello time of VM creation.</span></span> <span data-ttu-id="96985-107">Ez a cikk ismerteti azokat hello előnyeit (szakemberek számára), és tartózókat (hátrányait) toousing egyéni lemezképek és képletek alapján.</span><span class="sxs-lookup"><span data-stu-id="96985-107">This article explains some of hello advantages (pros) and disadvantages (cons) toousing custom images versus using formulas.</span></span>

## <a name="custom-image-pros-and-cons"></a><span data-ttu-id="96985-108">Kép: egyéni előnyei és hátrányai</span><span class="sxs-lookup"><span data-stu-id="96985-108">Custom image pros and cons</span></span>
<span data-ttu-id="96985-109">Egyéni lemezképek adjon meg egy statikus és nem módosítható módja a toocreate virtuális gépek a kívánt környezetből.</span><span class="sxs-lookup"><span data-stu-id="96985-109">Custom images provide a static, immutable way toocreate VMs from a desired environment.</span></span> 

<span data-ttu-id="96985-110">**Informatikai szakemberek**</span><span class="sxs-lookup"><span data-stu-id="96985-110">**Pros**</span></span>

* <span data-ttu-id="96985-111">Virtuálisgép-létrehozásnál egyéni lemezképéről esetén gyors, mert semmi módosítása után a virtuális gép van hoz létre hello hello lemezképből.</span><span class="sxs-lookup"><span data-stu-id="96985-111">VM provisioning from a custom image is fast as nothing changes after hello VM is spun up from hello image.</span></span> <span data-ttu-id="96985-112">Ez azt jelenti amelyeket nem beállítások tooapply hello egyéni lemezkép csak a kép vonatkozó beállítások kihagyásával.</span><span class="sxs-lookup"><span data-stu-id="96985-112">In other words, there are no settings tooapply as hello custom image is just an image without settings.</span></span> 
* <span data-ttu-id="96985-113">Egyetlen egyéni lemezkép alapján létrehozott virtuális gépek esetén azonosak.</span><span class="sxs-lookup"><span data-stu-id="96985-113">VMs created from a single custom image are identical.</span></span>

<span data-ttu-id="96985-114">**Hátrányait**</span><span class="sxs-lookup"><span data-stu-id="96985-114">**Cons**</span></span>

* <span data-ttu-id="96985-115">Ha tooupdate kell bizonyos elemeinek hello egyéni lemezképet, hello lemezképet kell újból létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="96985-115">If you need tooupdate some aspect of hello custom image, hello image must be recreated.</span></span>  

## <a name="formula-pros-and-cons"></a><span data-ttu-id="96985-116">Képletadat előnyei és hátrányai</span><span class="sxs-lookup"><span data-stu-id="96985-116">Formula pros and cons</span></span>
<span data-ttu-id="96985-117">Képletek számára egy dinamikus módon toocreate virtuális gépek hello szükséges beállításokat.</span><span class="sxs-lookup"><span data-stu-id="96985-117">Formulas provide a dynamic way toocreate VMs from hello desired configuration/settings.</span></span>

<span data-ttu-id="96985-118">**Informatikai szakemberek**</span><span class="sxs-lookup"><span data-stu-id="96985-118">**Pros**</span></span>

* <span data-ttu-id="96985-119">Hello környezet változásait a hello parancsprogramok keresztül összetevők rögzíthetők.</span><span class="sxs-lookup"><span data-stu-id="96985-119">Changes in hello environment can be captured on hello fly via artifacts.</span></span> <span data-ttu-id="96985-120">Például, ha szeretné, hogy a virtuális gépek hello legújabb bitként a kiadási láncból telepítve, vagy besorolás hello legújabb kód a tárházban lévő, egyszerűen megadhatja az összetevő telepíti a legújabb bits hello vagy hello legújabb kód együtt hello képletben vesz egy cél alapjául szolgáló lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="96985-120">For example, if you want a VM installed with hello latest bits from your release pipeline or enlist hello latest code from your repository, you can simply specify an artifact that deploys hello latest bits or enlists hello latest code in hello formula together with a target base image.</span></span> <span data-ttu-id="96985-121">Ha ez a képlet használt toocreate virtuális gépeket, hello legújabb bit/kód telepített/bejegyezve toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="96985-121">Whenever this formula is used toocreate VMs, hello latest bits/code are deployed/enlisted toohello VM.</span></span> 
* <span data-ttu-id="96985-122">Képletek alapértelmezett beállításokat, az egyéni lemezképek nem adhatók meg – például a Virtuálisgép-méretek és a virtuális hálózati beállításokat adhat meg.</span><span class="sxs-lookup"><span data-stu-id="96985-122">Formulas can define default settings that custom images cannot provide - such as VM sizes and virtual network settings.</span></span> 
* <span data-ttu-id="96985-123">a képlet mentett hello beállításokat alapértelmezett értékei láthatók, de hello virtuális gép létrehozásakor módosítható.</span><span class="sxs-lookup"><span data-stu-id="96985-123">hello settings saved in a formula are shown as default values, but can be modified when hello VM is created.</span></span> 

<span data-ttu-id="96985-124">**Hátrányait**</span><span class="sxs-lookup"><span data-stu-id="96985-124">**Cons**</span></span>

* <span data-ttu-id="96985-125">Virtuális gép létrehozása egy képletet a virtuális gép létrehozása egy egyéni lemezképből több időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="96985-125">Creating a VM from a formula can take more time than creating a VM from a custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="96985-126">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="96985-126">Related blog posts</span></span>
* [<span data-ttu-id="96985-127">Egyéni lemezképek vagy képletek?</span><span class="sxs-lookup"><span data-stu-id="96985-127">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="96985-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="96985-128">Next steps</span></span>
- [<span data-ttu-id="96985-129">DevTest Labs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="96985-129">DevTest Labs FAQ</span></span>](devtest-lab-faq.md)