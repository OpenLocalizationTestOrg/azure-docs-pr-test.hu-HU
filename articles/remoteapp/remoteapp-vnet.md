---
title: aaaValidate hello Azure VNET toouse az Azure RemoteApp |} Microsoft Docs
description: "Ismerje meg, hogyan toomake meg arról, hogy az Azure virtuális hálózat készen áll az Azure RemoteApp toouse"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b573ba02-4587-4be5-9821-27bd891a73b2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 5587556c264356e6ab6039b983a38cb2b95ed268
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="validate-hello-azure-vnet-toouse-with-azure-remoteapp"></a><span data-ttu-id="5e2d1-103">Az Azure RemoteApp hello Azure VNET toouse ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="5e2d1-103">Validate hello Azure VNET toouse with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5e2d1-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="5e2d1-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="5e2d1-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="5e2d1-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="5e2d1-106">Az Azure RemoteApp egy Azure virtuális Hálózatot használ, érdemes lehet toovalidate hello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="5e2d1-106">Before you use an Azure VNET with Azure RemoteApp, you might want toovalidate hello VNET.</span></span> <span data-ttu-id="5e2d1-107">Ez segít a kapcsolatban előforduló problémák megelőzéséhez.</span><span class="sxs-lookup"><span data-stu-id="5e2d1-107">This helps prevent issues with connectivity.</span></span>

<span data-ttu-id="5e2d1-108">toovalidate az Azure virtuális hálózat, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5e2d1-108">toovalidate your Azure VNET, do hello following:</span></span>

1. <span data-ttu-id="5e2d1-109">Hozzon létre egy Azure virtuális gépen belüli hello Azure VNET azt szeretné, hogy az Azure RemoteApp toouse hello alhálózata.</span><span class="sxs-lookup"><span data-stu-id="5e2d1-109">Create an Azure virtual machine inside hello subnet of hello Azure VNET you want toouse with Azure RemoteApp.</span></span>
2. <span data-ttu-id="5e2d1-110">Hello segítségével csatlakozzon a virtuális gép toothat **Connect** beállítás hello felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="5e2d1-110">Connect toothat VM by using hello **Connect** option in hello management portal.</span></span>
3. <span data-ttu-id="5e2d1-111">Csatlakozás a virtuális gép toohello hello ugyanabban a tartományban, hogy szeretné-e az Azure RemoteApp toouse.</span><span class="sxs-lookup"><span data-stu-id="5e2d1-111">Join hello virtual machine toohello same domain that you want toouse with Azure RemoteApp.</span></span> <span data-ttu-id="5e2d1-112">A hibrid gyűjteményt, amely a tooyour a helyszíni hálózat létrehozásakor, csatlakozzon a hello virtuális gép tooyour helyi tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="5e2d1-112">If you are creating a hybrid collection that connects tooyour on-premises network, join hello virtual machine tooyour local domain.</span></span>

<span data-ttu-id="5e2d1-113">Ha ez sikeres, hello Azure VNET is készen toouse RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="5e2d1-113">If this is successful, hello Azure VNET is ready toouse with RemoteApp.</span></span>

<span data-ttu-id="5e2d1-114">Hello végpont hibrid gyűjtemény munkafolyamat kapcsolatos további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="5e2d1-114">For more information about hello end-to-end hybrid collection workflow, see hello following articles:</span></span>

* [<span data-ttu-id="5e2d1-115">Hogyan tooplan a virtuális hálózat az Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="5e2d1-115">How tooplan your virtual network for Azure RemoteApp</span></span>](remoteapp-planvnet.md)
* [<span data-ttu-id="5e2d1-116">Hibrid gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="5e2d1-116">Create a hybrid collection</span></span>](remoteapp-create-hybrid-deployment.md)
* [<span data-ttu-id="5e2d1-117">Azure RemoteApp gyűjtemény tooyour Azure-beli virtuális hálózathoz (az ExpressRoute támogatása) telepítése</span><span class="sxs-lookup"><span data-stu-id="5e2d1-117">Deploy Azure RemoteApp collection tooyour Azure Virtual Network (with support for ExpressRoute)</span></span>](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)

