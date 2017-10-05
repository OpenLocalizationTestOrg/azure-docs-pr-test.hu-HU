---
title: "Azure RemoteApp-lemezkép létrehozása |} Microsoft Docs"
description: "Tudja meg, hogyan használható az Azure RemoteApp-lemezképek létrehozása"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: cb0f9424-6185-45a1-abe9-c23f1edf34f2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 4b8ba6f264f982e03930c5ad4ccdb2d80f2c8665
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-remoteapp-image"></a><span data-ttu-id="838f7-103">Create an Azure RemoteApp image (Azure RemoteApp-rendszerkép létrehozása)</span><span class="sxs-lookup"><span data-stu-id="838f7-103">Create an Azure RemoteApp image</span></span>
> [!IMPORTANT]
> <span data-ttu-id="838f7-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="838f7-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="838f7-105">A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="838f7-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="838f7-106">Az Azure RemoteApp képek az alkalmazásokat, amelyek megosztása a a felhasználók tárolására használ.</span><span class="sxs-lookup"><span data-stu-id="838f7-106">Azure RemoteApp uses images to hold the apps that you share with your users.</span></span> <span data-ttu-id="838f7-107">(Jelenleg igénybe vehet a lemezkép és közül a felhasználók hozzáférést amikor bejelentkeznek az Azure Remoteappba – a virtuális gépek létrehozására használható.) Egy Azure RemoteApp-gyűjtemény létrehozásához a választott alkalmazások, hogy felhőalapú vagy hibrid-e, akkor először hozzon létre kép az alkalmazások telepítése.</span><span class="sxs-lookup"><span data-stu-id="838f7-107">(We take your image and use it to create VMs - that's what the users access when they sign into Azure RemoteApp.) To create an Azure RemoteApp collection with your choice of applications, whether it is cloud or hybrid, you  start by creating an image with those applications installed.</span></span> <span data-ttu-id="838f7-108">Ezután hozzon létre egy gyűjteményt, amely használja ezt a lemezképet, és hozzárendelhet felhasználókat ehhez a gyűjteményhez alkalmazások közzétételére azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="838f7-108">Then, create a collection that uses that image, assign users to the collection, and publish apps to those users.</span></span>

<span data-ttu-id="838f7-109">Több lehetőség közül választhat a létrehozására vagy a lemezképek használatával.</span><span class="sxs-lookup"><span data-stu-id="838f7-109">You have several options for creating or using images.</span></span> <span data-ttu-id="838f7-110">A basic [követelmény](remoteapp-imagereqs.md) a képet, hogy futtassa a Windows Server 2012 R2, telepítve van a távoli asztali munkamenetgazda (RDSH) szerepkör.</span><span class="sxs-lookup"><span data-stu-id="838f7-110">The basic [requirement](remoteapp-imagereqs.md) for an image is that it run Windows Server 2012 R2 and have the Remote Desktop Session Host (RDSH) role installed.</span></span> <span data-ttu-id="838f7-111">Hogyan le, ahol részek lesznek érdekes.</span><span class="sxs-lookup"><span data-stu-id="838f7-111">How you get that is where things get interesting.</span></span>

<span data-ttu-id="838f7-112">Amikor a képek rendelkezik a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="838f7-112">You have the following options when it comes to images:</span></span>

* <span data-ttu-id="838f7-113">Lehet importálni és használni egy [lemezkép-alapú Azure virtuális géphez](remoteapp-image-on-azurevm.md).</span><span class="sxs-lookup"><span data-stu-id="838f7-113">You can import and use an [image based on an Azure virtual machine](remoteapp-image-on-azurevm.md).</span></span> <span data-ttu-id="838f7-114">Ez az egyéni beállítások igénylő-üzleti alkalmazások helyes.</span><span class="sxs-lookup"><span data-stu-id="838f7-114">This is good for line-of-business apps that require custom settings.</span></span> <span data-ttu-id="838f7-115">Testre szabhatja a lemezképet, az alkalmazás működéséhez.</span><span class="sxs-lookup"><span data-stu-id="838f7-115">You can customize the image to work for the app.</span></span>
* <span data-ttu-id="838f7-116">Is [létrehozása és feltöltése az egyéni lemezkép](remoteapp-create-custom-image.md).</span><span class="sxs-lookup"><span data-stu-id="838f7-116">You can [create and upload a custom image](remoteapp-create-custom-image.md).</span></span> <span data-ttu-id="838f7-117">Ez a helyes, ha már rendelkezik egy olyanra, amely a helyszíni távoli asztali szolgáltatások környezethez használja.</span><span class="sxs-lookup"><span data-stu-id="838f7-117">This is good if you already have an image that you use for your on-premises Remote Desktop Services deployment.</span></span>
* <span data-ttu-id="838f7-118">Egyikét használhatja a [sablonrendszerképek](remoteapp-images.md) a RemoteApp-előfizetés tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="838f7-118">You can use one of the [template images](remoteapp-images.md) included in your RemoteApp subscription.</span></span> <span data-ttu-id="838f7-119">Ezeket a lemezképeket jönnek létre, és a RemoteApp csapatától tartja fenn, és néhány szokásos olyan alkalmazásokat tartalmaznak (például az Office suite), elérhetővé teheti a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="838f7-119">These images are created and maintained by the RemoteApp team and contain some standard applications (like the Office suite) that you can make available to your users.</span></span> <span data-ttu-id="838f7-120">Vegye figyelembe, hogy csak az Office 365 Pro Plus kép üzemi környezetben is használható.</span><span class="sxs-lookup"><span data-stu-id="838f7-120">Note that only the Office 365 Pro Plus image can be used in a production setting.</span></span>

<span data-ttu-id="838f7-121">Függetlenül attól, hol szerezheti a lemezképet, vagy hogyan hoz létre, így megértheti érdemes a [alkalmazáskövetelmények](remoteapp-appreqs.md) annak érdekében, hogy az alkalmazás működik jól RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="838f7-121">Regardless of where you get your image or how you create it, you'll want to make sure you understand the [app requirements](remoteapp-appreqs.md) to ensure that your app works well in RemoteApp.</span></span> <span data-ttu-id="838f7-122">Ezt követően a következő lépés, ha a egy [felhő](remoteapp-create-cloud-deployment.md) vagy [hibrid](remoteapp-create-hybrid-deployment.md) gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="838f7-122">Then, the next step is to create a [cloud](remoteapp-create-cloud-deployment.md) or [hybrid](remoteapp-create-hybrid-deployment.md) collection.</span></span>

