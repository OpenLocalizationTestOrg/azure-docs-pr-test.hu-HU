---
title: "Azure RemoteApp lemezkép aaaCreate |} Microsoft Docs"
description: "További tudnivalók: hello lehetőségeit az Azure RemoteApp-lemezképek létrehozása"
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
ms.openlocfilehash: 54e63b6fa13addfcda96ce581910e1ac48d91e70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-remoteapp-image"></a><span data-ttu-id="b797e-103">Create an Azure RemoteApp image (Azure RemoteApp-rendszerkép létrehozása)</span><span class="sxs-lookup"><span data-stu-id="b797e-103">Create an Azure RemoteApp image</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b797e-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="b797e-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="b797e-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="b797e-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="b797e-106">Az Azure RemoteApp képek toohold hello alkalmazásokat a felhasználóival megosztott használja.</span><span class="sxs-lookup"><span data-stu-id="b797e-106">Azure RemoteApp uses images toohold hello apps that you share with your users.</span></span> <span data-ttu-id="b797e-107">(Azt a képet, és felhasználása azt toocreate virtuális gépek - közül mi hello felhasználók érhető el, amikor bejelentkeznek az Azure Remoteappba.) toocreate egy Azure RemoteApp-gyűjteményt a választott az alkalmazások, hogy felhőalapú vagy hibrid,-e, először hozzon létre kép az alkalmazások telepítése.</span><span class="sxs-lookup"><span data-stu-id="b797e-107">(We take your image and use it toocreate VMs - that's what hello users access when they sign into Azure RemoteApp.) toocreate an Azure RemoteApp collection with your choice of applications, whether it is cloud or hybrid, you  start by creating an image with those applications installed.</span></span> <span data-ttu-id="b797e-108">Ezután hozzon létre egy gyűjteményt, amely használja ezt a lemezképet, felhasználók toohello gyűjtemény hozzárendelése és alkalmazások toothose felhasználók közzététele.</span><span class="sxs-lookup"><span data-stu-id="b797e-108">Then, create a collection that uses that image, assign users toohello collection, and publish apps toothose users.</span></span>

<span data-ttu-id="b797e-109">Több lehetőség közül választhat a létrehozására vagy a lemezképek használatával.</span><span class="sxs-lookup"><span data-stu-id="b797e-109">You have several options for creating or using images.</span></span> <span data-ttu-id="b797e-110">Alapszintű hello [követelmény](remoteapp-imagereqs.md) a képet, hogy futtassa a Windows Server 2012 R2, telepítve hello távoli asztali munkamenetgazda (RDSH) szerepkör.</span><span class="sxs-lookup"><span data-stu-id="b797e-110">hello basic [requirement](remoteapp-imagereqs.md) for an image is that it run Windows Server 2012 R2 and have hello Remote Desktop Session Host (RDSH) role installed.</span></span> <span data-ttu-id="b797e-111">Hogyan le, ahol részek lesznek érdekes.</span><span class="sxs-lookup"><span data-stu-id="b797e-111">How you get that is where things get interesting.</span></span>

<span data-ttu-id="b797e-112">Hello tooimages ismét a következő lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="b797e-112">You have hello following options when it comes tooimages:</span></span>

* <span data-ttu-id="b797e-113">Lehet importálni és használni egy [lemezkép-alapú Azure virtuális géphez](remoteapp-image-on-azurevm.md).</span><span class="sxs-lookup"><span data-stu-id="b797e-113">You can import and use an [image based on an Azure virtual machine](remoteapp-image-on-azurevm.md).</span></span> <span data-ttu-id="b797e-114">Ez az egyéni beállítások igénylő-üzleti alkalmazások helyes.</span><span class="sxs-lookup"><span data-stu-id="b797e-114">This is good for line-of-business apps that require custom settings.</span></span> <span data-ttu-id="b797e-115">Testre szabhatja a hello kép toowork hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b797e-115">You can customize hello image toowork for hello app.</span></span>
* <span data-ttu-id="b797e-116">Is [létrehozása és feltöltése az egyéni lemezkép](remoteapp-create-custom-image.md).</span><span class="sxs-lookup"><span data-stu-id="b797e-116">You can [create and upload a custom image](remoteapp-create-custom-image.md).</span></span> <span data-ttu-id="b797e-117">Ez a helyes, ha már rendelkezik egy olyanra, amely a helyszíni távoli asztali szolgáltatások környezethez használja.</span><span class="sxs-lookup"><span data-stu-id="b797e-117">This is good if you already have an image that you use for your on-premises Remote Desktop Services deployment.</span></span>
* <span data-ttu-id="b797e-118">Hello használhat [sablonrendszerképek](remoteapp-images.md) a RemoteApp-előfizetés tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b797e-118">You can use one of hello [template images](remoteapp-images.md) included in your RemoteApp subscription.</span></span> <span data-ttu-id="b797e-119">Ezeket a lemezképeket jönnek létre, és hello RemoteApp csapatától tartja fenn, és néhány szokásos alkalmazást az, hogy elérhető tooyour felhasználók (például az hello Office suite) tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="b797e-119">These images are created and maintained by hello RemoteApp team and contain some standard applications (like hello Office suite) that you can make available tooyour users.</span></span> <span data-ttu-id="b797e-120">Vegye figyelembe, hogy csak hello Office 365 Pro Plus lemezkép üzemi környezetben is használható.</span><span class="sxs-lookup"><span data-stu-id="b797e-120">Note that only hello Office 365 Pro Plus image can be used in a production setting.</span></span>

<span data-ttu-id="b797e-121">Függetlenül attól, hol szerezheti a lemezképet, vagy hogyan hoz létre, érdemes tisztában lennie hello toomake [alkalmazáskövetelmények](remoteapp-appreqs.md) tooensure, amely az alkalmazás működik jól RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="b797e-121">Regardless of where you get your image or how you create it, you'll want toomake sure you understand hello [app requirements](remoteapp-appreqs.md) tooensure that your app works well in RemoteApp.</span></span> <span data-ttu-id="b797e-122">Ezt követően hello következő lépésre toocreate egy [felhő](remoteapp-create-cloud-deployment.md) vagy [hibrid](remoteapp-create-hybrid-deployment.md) gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="b797e-122">Then, hello next step is toocreate a [cloud](remoteapp-create-cloud-deployment.md) or [hybrid](remoteapp-create-hybrid-deployment.md) collection.</span></span>

