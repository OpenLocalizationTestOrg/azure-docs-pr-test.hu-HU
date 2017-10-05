---
title: "Hozzon létre egy virtuálisgép-lemezkép az Azure piactéren műszaki előfeltételei |} Microsoft Docs"
description: "A létrehozása és telepítése a virtuálisgép-lemezkép megvásárlásához mások az Azure piactéren vonatkozó követelmények megértése érdekében."
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
ms.openlocfilehash: af3e2ad623d8d7bfafe676411f9ae3fbee78aab8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a><span data-ttu-id="bf288-103">Hozzon létre egy virtuálisgép-lemezkép az Azure piactéren műszaki előfeltételei</span><span class="sxs-lookup"><span data-stu-id="bf288-103">Technical prerequisites for creating a virtual machine image for the Azure Marketplace</span></span>
<span data-ttu-id="bf288-104">A folyamat megkezdése előtt alaposan és megértettem, hogy hol és miért minden egyes lépést.</span><span class="sxs-lookup"><span data-stu-id="bf288-104">Read the process thoroughly before beginning and understand where and why each step is performed.</span></span> <span data-ttu-id="bf288-105">Amennyire csak lehetséges, meg kell készítse elő a vállalati adatok és egyéb adatokat, töltse le a szükséges eszközök, és/vagy technikai összetevő létrehozása az ajánlat létrehozási folyamat megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="bf288-105">As much as possible, you should prepare your company information and other data, download necessary tools, and/or create technical components before beginning the offer creation process.</span></span> <span data-ttu-id="bf288-106">Ezek az elemek egyértelműen kiderül, hogy ez a cikk áttekintése kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bf288-106">These items should be clear from reviewing this article.</span></span>  

## <a name="download-needed-tools--applications"></a><span data-ttu-id="bf288-107">Töltse le a szükséges eszközök és alkalmazások</span><span class="sxs-lookup"><span data-stu-id="bf288-107">Download needed tools & applications</span></span>
<span data-ttu-id="bf288-108">Készen áll a megkezdése előtt a következő elemeket kell rendelkezniük:</span><span class="sxs-lookup"><span data-stu-id="bf288-108">You should have the following items ready before beginning the process:</span></span>

* <span data-ttu-id="bf288-109">Attól függően, hogy milyen operációs rendszert céloz meg, telepítse a [Azure PowerShell-parancsmagok](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) vagy [Linux parancssori felület eszköz](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) a a [Azure letölti](https://azure.microsoft.com/downloads/) lap.</span><span class="sxs-lookup"><span data-stu-id="bf288-109">Depending on which operating system you are targeting, install the [Azure PowerShell cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) or [Linux command-line interface tool](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) from the [Azure Downloads](https://azure.microsoft.com/downloads/) page.</span></span>
* <span data-ttu-id="bf288-110">Telepítse az Azure Storage Explorer a Codeplex webhelyen.</span><span class="sxs-lookup"><span data-stu-id="bf288-110">Install Azure Storage Explorer from CodePlex.</span></span>
* <span data-ttu-id="bf288-111">Töltse le, és a hitelesítésszolgáltató vizsgálati eszköz telepítése az Azure hitelesített:</span><span class="sxs-lookup"><span data-stu-id="bf288-111">Download and install the Certification Test Tool for Azure Certified:</span></span>
  * <span data-ttu-id="bf288-112">[http://go.microsoft.com/fwlink/?LinkId=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span><span class="sxs-lookup"><span data-stu-id="bf288-112">[http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span></span> <span data-ttu-id="bf288-113">A hitelesítésszolgáltató eszköz futtatásához Windows-alapú számítógépre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="bf288-113">You need a Windows-based computer to run the certification tool.</span></span> <span data-ttu-id="bf288-114">Ha nem rendelkezik egy Windows-alapú számítógép elérhető, futtathatja az eszközt a Windows-alapú virtuális gépek használata az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="bf288-114">If you do not have a Windows-based computer available, you can run the tool using a Windows-based VM in Azure.</span></span>

## <a name="platforms-supported"></a><span data-ttu-id="bf288-115">Támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="bf288-115">Platforms supported</span></span>
<span data-ttu-id="bf288-116">Virtuális gépek Azure-alapú Windows vagy Linux fejleszthet.</span><span class="sxs-lookup"><span data-stu-id="bf288-116">You can develop Azure-based VMs on Windows or Linux.</span></span> <span data-ttu-id="bf288-117">A közzétételi folyamat – például létrehozhat egy Azure-kompatibilis virtuális merevlemez (VHD) – használja a különböző eszközök és lépések attól függően, hogy milyen operációs rendszert használ egyes elemei:</span><span class="sxs-lookup"><span data-stu-id="bf288-117">Some elements of the publishing process--such as creating an Azure-compatible virtual hard disk (VHD)--use different tools and steps depending on which operating system you are using:</span></span>  

* <span data-ttu-id="bf288-118">Ha Linux használ, tekintse át a "Hozzon létre egy Azure-kompatibilis VHD-t (Linux-alapú)" részt a a [virtuális gép lemezképének közzétételi útmutató](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="bf288-118">If you are using Linux, refer to the “Create an Azure-compatible VHD (Linux-based)” section of the [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>
* <span data-ttu-id="bf288-119">Ha Windows használ, tekintse át a "Hozzon létre egy Azure-kompatibilis VHD-t (Windows-alapú)" részt a a [virtuális gép lemezképének közzétételi útmutató](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="bf288-119">If you are using Windows, refer to the “Create an Azure-compatible VHD (Windows-based)” section of the [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>

> [!NOTE]
> <span data-ttu-id="bf288-120">A Windows-alapú gép elérésére lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="bf288-120">You need access to a Windows-based machine to:</span></span>
> 
> * <span data-ttu-id="bf288-121">A hitelesítésszolgáltató érvényesítési eszköz futtatásához.</span><span class="sxs-lookup"><span data-stu-id="bf288-121">Run the certification validation tool.</span></span>
> * <span data-ttu-id="bf288-122">Hozzon létre a virtuális merevlemez hitelesítő küldése a megosztott virtuális merevlemez hozzáférési aláírás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="bf288-122">Create the VHD shared access signature URL for the VHD certification submission.</span></span>
> 
> 

## <a name="develop-your-vhd"></a><span data-ttu-id="bf288-123">A virtuális merevlemez fejlesztése</span><span class="sxs-lookup"><span data-stu-id="bf288-123">Develop your VHD</span></span>
<span data-ttu-id="bf288-124">Azure virtuális merevlemezek, a felhőben, vagy a helyszíni fejleszthet:</span><span class="sxs-lookup"><span data-stu-id="bf288-124">You can develop Azure VHDs in the cloud or on-premises:</span></span>

* <span data-ttu-id="bf288-125">Felhőalapú fejlesztési azt jelenti, hogy minden fejlesztési lépést távolról elvégzett rezidens Azure virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="bf288-125">Cloud-based development means all development steps are performed remotely on a VHD resident on Azure.</span></span>
* <span data-ttu-id="bf288-126">A helyi fejlesztési letöltése a virtuális merevlemez és a fájlrendszersérülések segítségével helyszíni infrastruktúra van szükség.</span><span class="sxs-lookup"><span data-stu-id="bf288-126">On-premises development requires downloading a VHD and developing it using on-premises infrastructure.</span></span> <span data-ttu-id="bf288-127">Bár ez lehetséges, nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="bf288-127">Although this is possible, we do not recommend it.</span></span> <span data-ttu-id="bf288-128">Vegye figyelembe, hogy fejleszti a Windows vagy SQL helyszíni szükséges hozzá a megfelelő helyszíni licenckulcsot.</span><span class="sxs-lookup"><span data-stu-id="bf288-128">Note that developing for Windows or SQL on-premises requires you to have the relevant on-premises license keys.</span></span> <span data-ttu-id="bf288-129">Nem tartalmaznak, vagy az SQL Server telepítése a virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="bf288-129">You cannot include or install SQL Server after creating a VM.</span></span> <span data-ttu-id="bf288-130">Az ajánlat is jóváhagyott SQL lemezkép az Azure portálról kell alapozni.</span><span class="sxs-lookup"><span data-stu-id="bf288-130">You must also base your offer on an approved SQL image from the Azure portal.</span></span> <span data-ttu-id="bf288-131">Ha úgy dönt, hogy a helyszíni fejlesztése, néhány lépést működnek, mint ha volt fejlesztése a felhőben kell elvégeznie.</span><span class="sxs-lookup"><span data-stu-id="bf288-131">If you decide to develop on-premises, you must perform some steps differently than if you were developing in the cloud.</span></span> <span data-ttu-id="bf288-132">A releváns információt [hozzon létre egy helyszíni Virtuálisgép-lemezkép](marketplace-publishing-vm-image-creation-on-premise.md).</span><span class="sxs-lookup"><span data-stu-id="bf288-132">You can find relevant information in [Create an on-premises VM image](marketplace-publishing-vm-image-creation-on-premise.md).</span></span>

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
