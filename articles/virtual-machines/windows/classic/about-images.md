---
title: "a Windows virtuális gépek aaaAbout képek |} Microsoft Docs"
description: "További tudnivalók a lemezképek Windows virtuális gépek Azure-ban való használatának módját."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 66ff3fab-8e7f-4dff-b8da-ab1c9c9c9af8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cynthn
ms.openlocfilehash: c7cfa1d018a5e99d5b68f559ec9ae1f14e4dec8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-images-for-windows-virtual-machines"></a><span data-ttu-id="03b5c-103">A Windows virtuális gépek képek kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="03b5c-103">About images for Windows virtual machines</span></span>
> [!IMPORTANT]
> <span data-ttu-id="03b5c-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="03b5c-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="03b5c-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="03b5c-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="03b5c-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="03b5c-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="03b5c-107">Információ keresése és lemezképek használatával hello Resource Manager modellben: [Itt](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="03b5c-107">For information about finding and using images in hello Resource Manager model, see [here](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a><span data-ttu-id="03b5c-108">Lemezképek használata</span><span class="sxs-lookup"><span data-stu-id="03b5c-108">Working with images</span></span>

<span data-ttu-id="03b5c-109">Hello Azure PowerShell-modulra és hello Azure portál toomanage hello képek elérhető tooyour Azure-előfizetés is használhatja.</span><span class="sxs-lookup"><span data-stu-id="03b5c-109">You can use hello Azure PowerShell module and hello Azure portal toomanage hello images available tooyour Azure subscription.</span></span> <span data-ttu-id="03b5c-110">hello Azure PowerShell-modul több parancs beállításokat biztosít, így pontosan mit tudja toosee szeretné, vagy tegye.</span><span class="sxs-lookup"><span data-stu-id="03b5c-110">hello Azure PowerShell module provides more command options, so you can pinpoint exactly what you want toosee or do.</span></span> <span data-ttu-id="03b5c-111">hello Azure-portálon grafikus felhasználói Felülettel biztosít sok hello mindennapos felügyeleti feladatokat.</span><span class="sxs-lookup"><span data-stu-id="03b5c-111">hello Azure portal provides a GUI for many of hello everyday administrative tasks.</span></span>

<span data-ttu-id="03b5c-112">Íme néhány példa hello Azure PowerShell-modult használó.</span><span class="sxs-lookup"><span data-stu-id="03b5c-112">Here are some examples that use hello Azure PowerShell module.</span></span>

* <span data-ttu-id="03b5c-113">**Az összes kép lekérése**:`Get-AzureVMImage`érhető el az aktuális előfizetésben hello lemezképek listáját adja vissza: a lemezképeket és az Azure vagy partnerek által kínáltak mellett.</span><span class="sxs-lookup"><span data-stu-id="03b5c-113">**Get all images**:`Get-AzureVMImage`returns a list of all hello images available in your current subscription: your images and those provided by Azure or partners.</span></span> <span data-ttu-id="03b5c-114">lehet, hogy nagy hello listájában.</span><span class="sxs-lookup"><span data-stu-id="03b5c-114">hello resulting list could be large.</span></span> <span data-ttu-id="03b5c-115">Hogyan hello a következő példák szemléltetik tooget rövidebb listáját.</span><span class="sxs-lookup"><span data-stu-id="03b5c-115">hello next examples show how tooget a shorter list.</span></span>
* <span data-ttu-id="03b5c-116">**Első kép családok**:`Get-AzureVMImage | select ImageFamily` kép családok listájának lekérése jelenít meg karakterláncok **ImageFamily** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="03b5c-116">**Get image families**:`Get-AzureVMImage | select ImageFamily` gets a list of image families by showing strings **ImageFamily** property.</span></span>
* <span data-ttu-id="03b5c-117">**Egy megadott termékcsalád összes képek beolvasása**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`</span><span class="sxs-lookup"><span data-stu-id="03b5c-117">**Get all images in a specific family**: `Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`</span></span>
* <span data-ttu-id="03b5c-118">**Virtuálisgép-rendszerképek található**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` hello DataDiskConfiguration tulajdonságot, amelynek csak érvényes tooVM képek szűréssel működik ez a parancsmag.</span><span class="sxs-lookup"><span data-stu-id="03b5c-118">**Find VM Images**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` This cmdlet works by filtering hello DataDiskConfiguration property, which only applies tooVM Images.</span></span> <span data-ttu-id="03b5c-119">Ebben a példában is szűrők hello kimeneti tooonly hello címke és a lemezkép nevét.</span><span class="sxs-lookup"><span data-stu-id="03b5c-119">This example also filters hello output tooonly hello label and image name.</span></span>
* <span data-ttu-id="03b5c-120">**Általános lemezkép mentéséhez**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`</span><span class="sxs-lookup"><span data-stu-id="03b5c-120">**Save a generalized image**: `Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`</span></span>
* <span data-ttu-id="03b5c-121">**A speciális képének mentéséhez**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`</span><span class="sxs-lookup"><span data-stu-id="03b5c-121">**Save a specialized image**: `Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`</span></span>

  > [!TIP]
  > <span data-ttu-id="03b5c-122">hello OSState paraméter szükséges toocreate Virtuálisgép-lemezképet, amely magában foglalja a hello operációsrendszer-lemez, és adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="03b5c-122">hello OSState parameter is required toocreate a VM image, which includes hello operating system disk and attached data disks.</span></span> <span data-ttu-id="03b5c-123">Ha nem hello paraméter, hello parancsmag létrehoz egy operációsrendszer-lemezképben.</span><span class="sxs-lookup"><span data-stu-id="03b5c-123">If you don’t use hello parameter, hello cmdlet creates an OS image.</span></span> <span data-ttu-id="03b5c-124">hello paraméter hello érték azt jelzi, hogy hello kép általánosítva van, vagy kifejezetten, alapján e hello operációsrendszer-lemez van készítve ismételt felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="03b5c-124">hello value of hello parameter indicates whether hello image is generalized or specialized, based on whether hello operating system disk has been prepared for reuse.</span></span>

* <span data-ttu-id="03b5c-125">**Lemezkép törlése**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`</span><span class="sxs-lookup"><span data-stu-id="03b5c-125">**Delete an image**: `Remove-AzureVMImage –ImageName "MyOldVmImage"`</span></span>

## <a name="next-steps"></a><span data-ttu-id="03b5c-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="03b5c-126">Next Steps</span></span>
<span data-ttu-id="03b5c-127">Emellett [létrehozása a Windows hello Azure-portálon gépek](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="03b5c-127">You can also [create a Windows machine using hello Azure portal](tutorial.md).</span></span>
