---
title: "aaaAzure RemoteApp kép követelményei |} Microsoft Docs"
description: "További tudnivalók létrehozása az Azure RemoteApp használt képek toobe hello követelményei"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7cbb90f4-6dc9-462c-a429-088cdb57414e
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 4e35203eb93a866d4e0bd591d42b34746c7ffa4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="requirements-for-azure-remoteapp-images"></a><span data-ttu-id="d2e05-103">Azure RemoteApp-lemezképek követelményei</span><span class="sxs-lookup"><span data-stu-id="d2e05-103">Requirements for Azure RemoteApp images</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d2e05-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="d2e05-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="d2e05-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="d2e05-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="d2e05-106">Az Azure RemoteApp használja a Windows Server 2012 R2 kép toohost, amelyet meg szeretne tooshare a felhasználók minden hello program.</span><span class="sxs-lookup"><span data-stu-id="d2e05-106">Azure RemoteApp uses a Windows Server 2012 R2 image toohost all hello programs that you want tooshare with your users.</span></span> <span data-ttu-id="d2e05-107">egyéni lemezkép toocreate, megkezdheti a meglévő lemezkép vagy [hozzon létre egy újat](remoteapp-create-custom-image.md).</span><span class="sxs-lookup"><span data-stu-id="d2e05-107">toocreate a custom image, you can start with an existing image or [create a new one](remoteapp-create-custom-image.md).</span></span>

> [!TIP]
> <span data-ttu-id="d2e05-108">Tudta, hogy az Azure RemoteApp előfizetés hozzáférést tud biztosítani, hogy tooa Windows Server 2012 R2 rendszerképnél a hello Azure virtuális gép katalógusában, hogy toocreate saját sablon rendszerképet?</span><span class="sxs-lookup"><span data-stu-id="d2e05-108">Did you know that your Azure RemoteApp subscription gives you access tooa Windows Server 2012 R2 image in hello Azure VM gallery that you can use toocreate your own template image?</span></span> <span data-ttu-id="d2e05-109">[Kivétel](remoteapp-image-on-azurevm.md).</span><span class="sxs-lookup"><span data-stu-id="d2e05-109">[Check it out](remoteapp-image-on-azurevm.md).</span></span>  
> 
> 

<span data-ttu-id="d2e05-110">az Azure RemoteApp használata esetén feltölthető hello kép hello követelményei a következők:</span><span class="sxs-lookup"><span data-stu-id="d2e05-110">hello requirements for hello image that can be uploaded for use with Azure RemoteApp are:</span></span>

* <span data-ttu-id="d2e05-111">Egyéni alkalmazások helyileg hello rendszerképre ne tároljon adatokat.</span><span class="sxs-lookup"><span data-stu-id="d2e05-111">Custom applications don’t store data locally on hello image.</span></span> <span data-ttu-id="d2e05-112">Ezeket a lemezképeket állapot nélküli és alkalmazások csak tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="d2e05-112">These images are stateless and should only contain applications.</span></span>
* <span data-ttu-id="d2e05-113">hello kép nem tartalmaz adatokat, akkor szakadhat meg.</span><span class="sxs-lookup"><span data-stu-id="d2e05-113">hello image does not contain data that can be lost.</span></span>
* <span data-ttu-id="d2e05-114">hello képméret MB többszörösének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d2e05-114">hello image size should be a multiple of MBs.</span></span> <span data-ttu-id="d2e05-115">Ha egy olyanra, amely nincs egy pontos több tooupload, hello feltöltése sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="d2e05-115">If you try tooupload an image that is not an exact multiple, hello upload will fail.</span></span>
* <span data-ttu-id="d2e05-116">hello a kép mérete 127 GB méretűnek kell lennie, vagy kisebb.</span><span class="sxs-lookup"><span data-stu-id="d2e05-116">hello image size must be 127 GB or smaller.</span></span>
* <span data-ttu-id="d2e05-117">A VHD-fájlt kell lennie (VHDX-fájlok jelenleg nem támogatottak).</span><span class="sxs-lookup"><span data-stu-id="d2e05-117">It must be on a VHD file (VHDX files are not currently supported).</span></span>
* <span data-ttu-id="d2e05-118">hello virtuális merevlemez nem lehet a 2. generációs virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="d2e05-118">hello VHD must not be a generation 2 virtual machine.</span></span>
* <span data-ttu-id="d2e05-119">rögzített méretű vagy dinamikusan bővülő hello VHD lehet.</span><span class="sxs-lookup"><span data-stu-id="d2e05-119">hello VHD can be either fixed-size or dynamically expanding.</span></span> <span data-ttu-id="d2e05-120">Dinamikusan bővülő virtuális merevlemez használata ajánlott, mivel kevesebb idő tooupload tooAzure, mint a rögzített méretű VHD-fájl szükséges.</span><span class="sxs-lookup"><span data-stu-id="d2e05-120">A dynamically expanding VHD is recommended because it takes less time tooupload tooAzure than a fixed-size VHD file.</span></span>
* <span data-ttu-id="d2e05-121">hello lemez inicializálni kell hello fő rendszertöltő rekord (MBR) particionálási stílus használatával.</span><span class="sxs-lookup"><span data-stu-id="d2e05-121">hello disk must be initialized using hello Master Boot Record (MBR) partitioning style.</span></span> <span data-ttu-id="d2e05-122">GUID partíciós tábla (GPT) partícióval hello nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="d2e05-122">hello GUID partition table (GPT) partition style is not supported.</span></span>
* <span data-ttu-id="d2e05-123">hello VHD-t tartalmaznia kell a Windows Server 2012 R2 egy telepítés.</span><span class="sxs-lookup"><span data-stu-id="d2e05-123">hello VHD must contain a single installation of Windows Server 2012 R2.</span></span> <span data-ttu-id="d2e05-124">Több kötet, de csak egy Windows példányának tartalmazó tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="d2e05-124">It can contain multiple volumes, but only one that contains an installation of Windows.</span></span>
* <span data-ttu-id="d2e05-125">hello távoli asztali munkamenetgazda (RDSH) szerepkör és hello asztali élmény szolgáltatást telepíteni kell.</span><span class="sxs-lookup"><span data-stu-id="d2e05-125">hello Remote Desktop Session Host (RDSH) role and hello Desktop Experience feature must be installed.</span></span>
* <span data-ttu-id="d2e05-126">Távoli asztali Kapcsolatszervező szerepkör hello kell *nem* telepíthető.</span><span class="sxs-lookup"><span data-stu-id="d2e05-126">hello Remote Desktop Connection Broker role must *not* be installed.</span></span>
* <span data-ttu-id="d2e05-127">hello titkosított fájlrendszer (EFS) le kell tiltani.</span><span class="sxs-lookup"><span data-stu-id="d2e05-127">hello Encrypting File System (EFS) must be disabled.</span></span>
* <span data-ttu-id="d2e05-128">hello képnek kell lennie a Sysprep segédprogrammal előkészített hello paraméterek használatával **/oobe / generalize shutdown** (ne használja az hello **/mode:vm** paraméter).</span><span class="sxs-lookup"><span data-stu-id="d2e05-128">hello image must be SYSPREPed using hello parameters **/oobe /generalize /shutdown** (DO NOT use hello **/mode:vm** parameter).</span></span>
* <span data-ttu-id="d2e05-129">A pillanatkép-lánc virtuális merevlemez feltöltése nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="d2e05-129">Uploading your VHD from a snapshot chain is not supported.</span></span>

<span data-ttu-id="d2e05-130">Lásd: [hozzon létre egy Azure RemoteApp-rendszerkép](remoteapp-imageoptions.md) olvashat az Azure RemoteApp-lemezképek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d2e05-130">See [Create an Azure RemoteApp image](remoteapp-imageoptions.md) for more information about creating images for Azure RemoteApp.</span></span>

