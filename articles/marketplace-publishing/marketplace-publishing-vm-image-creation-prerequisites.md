---
title: "egy virtuálisgép-lemezkép az Azure piactér hello létrehozása aaaTechnical előfeltételei |} Microsoft Docs"
description: "A létrehozása és telepítése a virtuális gép lemezképének toohello Azure piactér mások hello követelmények megértése érdekében toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: fcae4d9e052581e3c1dfe7962e6d50ec31040419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-hello-azure-marketplace"></a><span data-ttu-id="81946-103">A virtuálisgép-lemezkép az Azure piactér hello létrehozása műszaki előfeltételei</span><span class="sxs-lookup"><span data-stu-id="81946-103">Technical prerequisites for creating a virtual machine image for hello Azure Marketplace</span></span>
<span data-ttu-id="81946-104">Hello folyamat megkezdése előtt alaposan és megértettem, hogy hol és miért minden egyes lépést.</span><span class="sxs-lookup"><span data-stu-id="81946-104">Read hello process thoroughly before beginning and understand where and why each step is performed.</span></span> <span data-ttu-id="81946-105">Amennyire csak lehetséges, meg kell készítse elő a vállalati adatok és egyéb adatokat, töltse le a szükséges eszközök, és/vagy technikai összetevő létrehozása hello ajánlat létrehozási folyamat megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="81946-105">As much as possible, you should prepare your company information and other data, download necessary tools, and/or create technical components before beginning hello offer creation process.</span></span> <span data-ttu-id="81946-106">Ezek az elemek egyértelműen kiderül, hogy ez a cikk áttekintése kell lennie.</span><span class="sxs-lookup"><span data-stu-id="81946-106">These items should be clear from reviewing this article.</span></span>  

## <a name="download-needed-tools--applications"></a><span data-ttu-id="81946-107">Töltse le a szükséges eszközök és alkalmazások</span><span class="sxs-lookup"><span data-stu-id="81946-107">Download needed tools & applications</span></span>
<span data-ttu-id="81946-108">A következő elemek készen áll a hello folyamat megkezdése előtt hello kell rendelkezniük:</span><span class="sxs-lookup"><span data-stu-id="81946-108">You should have hello following items ready before beginning hello process:</span></span>

* <span data-ttu-id="81946-109">Attól függően, hogy milyen operációs rendszert céloz meg, a telepítés hello [Azure PowerShell-parancsmagok](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) vagy [Linux parancssori felület eszköz](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) a hello [Azure letölti](https://azure.microsoft.com/downloads/) lap.</span><span class="sxs-lookup"><span data-stu-id="81946-109">Depending on which operating system you are targeting, install hello [Azure PowerShell cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) or [Linux command-line interface tool](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) from hello [Azure Downloads](https://azure.microsoft.com/downloads/) page.</span></span>
* <span data-ttu-id="81946-110">Telepítse az Azure Storage Explorer a Codeplex webhelyen.</span><span class="sxs-lookup"><span data-stu-id="81946-110">Install Azure Storage Explorer from CodePlex.</span></span>
* <span data-ttu-id="81946-111">Töltse le, és telepítse a hello hitelesítő vizsgálati eszköz az Azure hitelesített:</span><span class="sxs-lookup"><span data-stu-id="81946-111">Download and install hello Certification Test Tool for Azure Certified:</span></span>
  * <span data-ttu-id="81946-112">[http://go.microsoft.com/fwlink/?LinkId=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span><span class="sxs-lookup"><span data-stu-id="81946-112">[http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span></span> <span data-ttu-id="81946-113">Szüksége van egy Windows-alapú számítógép toorun hello hitelesítő eszközt.</span><span class="sxs-lookup"><span data-stu-id="81946-113">You need a Windows-based computer toorun hello certification tool.</span></span> <span data-ttu-id="81946-114">Ha nem rendelkezik egy Windows-alapú számítógép elérhető, futtathatja a Windows-alapú virtuális gépek használata az Azure-ban hello eszközt.</span><span class="sxs-lookup"><span data-stu-id="81946-114">If you do not have a Windows-based computer available, you can run hello tool using a Windows-based VM in Azure.</span></span>

## <a name="platforms-supported"></a><span data-ttu-id="81946-115">Támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="81946-115">Platforms supported</span></span>
<span data-ttu-id="81946-116">Virtuális gépek Azure-alapú Windows vagy Linux fejleszthet.</span><span class="sxs-lookup"><span data-stu-id="81946-116">You can develop Azure-based VMs on Windows or Linux.</span></span> <span data-ttu-id="81946-117">Hello közzétételi folyamat – például létrehozhat egy Azure-kompatibilis virtuális merevlemez (VHD) – használja a különböző eszközök és lépések attól függően, hogy milyen operációs rendszert használ egyes elemei:</span><span class="sxs-lookup"><span data-stu-id="81946-117">Some elements of hello publishing process--such as creating an Azure-compatible virtual hard disk (VHD)--use different tools and steps depending on which operating system you are using:</span></span>  

* <span data-ttu-id="81946-118">Ha Linux használ, tekintse meg a "Hozzon létre egy Azure-kompatibilis VHD-t (Linux-alapú)" című toohello hello [virtuális gép lemezképének közzétételi útmutató](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="81946-118">If you are using Linux, refer toohello “Create an Azure-compatible VHD (Linux-based)” section of hello [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>
* <span data-ttu-id="81946-119">Ha Windows használ, tekintse meg a toohello "Hozzon létre egy Azure-kompatibilis VHD-t (Windows-alapú)" szakasza hello [virtuális gép lemezképének közzétételi útmutató](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="81946-119">If you are using Windows, refer toohello “Create an Azure-compatible VHD (Windows-based)” section of hello [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>

> [!NOTE]
> <span data-ttu-id="81946-120">El kell érni tooa Windows-alapú gép:</span><span class="sxs-lookup"><span data-stu-id="81946-120">You need access tooa Windows-based machine to:</span></span>
> 
> * <span data-ttu-id="81946-121">Hello hitelesítő érvényesítési eszköz futtatásához.</span><span class="sxs-lookup"><span data-stu-id="81946-121">Run hello certification validation tool.</span></span>
> * <span data-ttu-id="81946-122">Hello megosztott virtuális merevlemez hozzáférési aláírás URL-címe hello VHD hitelesítő küldésének létrehozása.</span><span class="sxs-lookup"><span data-stu-id="81946-122">Create hello VHD shared access signature URL for hello VHD certification submission.</span></span>
> 
> 

## <a name="develop-your-vhd"></a><span data-ttu-id="81946-123">A virtuális merevlemez fejlesztése</span><span class="sxs-lookup"><span data-stu-id="81946-123">Develop your VHD</span></span>
<span data-ttu-id="81946-124">Azure virtuális merevlemezek hello felhőalapú vagy helyszíni fejleszthet:</span><span class="sxs-lookup"><span data-stu-id="81946-124">You can develop Azure VHDs in hello cloud or on-premises:</span></span>

* <span data-ttu-id="81946-125">Felhőalapú fejlesztési azt jelenti, hogy minden fejlesztési lépést távolról elvégzett rezidens Azure virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="81946-125">Cloud-based development means all development steps are performed remotely on a VHD resident on Azure.</span></span>
* <span data-ttu-id="81946-126">A helyi fejlesztési letöltése a virtuális merevlemez és a fájlrendszersérülések segítségével helyszíni infrastruktúra van szükség.</span><span class="sxs-lookup"><span data-stu-id="81946-126">On-premises development requires downloading a VHD and developing it using on-premises infrastructure.</span></span> <span data-ttu-id="81946-127">Bár ez lehetséges, nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="81946-127">Although this is possible, we do not recommend it.</span></span> <span data-ttu-id="81946-128">Vegye figyelembe, hogy Windows vagy SQL fejleszti a helyszíni meg toohave hello vonatkozó helyszíni licenckulcsot.</span><span class="sxs-lookup"><span data-stu-id="81946-128">Note that developing for Windows or SQL on-premises requires you toohave hello relevant on-premises license keys.</span></span> <span data-ttu-id="81946-129">Nem tartalmaznak, vagy az SQL Server telepítése a virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="81946-129">You cannot include or install SQL Server after creating a VM.</span></span> <span data-ttu-id="81946-130">Az ajánlat is hello Azure-portálon a jóváhagyott SQL kép kell alapozni.</span><span class="sxs-lookup"><span data-stu-id="81946-130">You must also base your offer on an approved SQL image from hello Azure portal.</span></span> <span data-ttu-id="81946-131">Ha úgy dönt, hogy a helyszíni toodevelop, néhány lépést működnek, mint ha volt fejlesztése hello felhőben kell elvégeznie.</span><span class="sxs-lookup"><span data-stu-id="81946-131">If you decide toodevelop on-premises, you must perform some steps differently than if you were developing in hello cloud.</span></span> <span data-ttu-id="81946-132">A releváns információt [hozzon létre egy helyszíni Virtuálisgép-lemezkép](marketplace-publishing-vm-image-creation-on-premise.md).</span><span class="sxs-lookup"><span data-stu-id="81946-132">You can find relevant information in [Create an on-premises VM image](marketplace-publishing-vm-image-creation-on-premise.md).</span></span>

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
