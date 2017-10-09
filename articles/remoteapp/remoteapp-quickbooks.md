---
title: az Azure Remoteappban QuickBooks aaaDeploy |} Microsoft Docs
description: Megtudhatja, hogyan tooshare QuickBooks az Azure RemoteApp.
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c5d00753-77c0-4f0d-a5df-9451b46a31d3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c21b1ac311449be2281f9ce157828260e856f55d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a><span data-ttu-id="d4270-103">Hogyan telepítheti az Azure Remoteappban QuickBooks?</span><span class="sxs-lookup"><span data-stu-id="d4270-103">How do you deploy QuickBooks in Azure RemoteApp?</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d4270-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="d4270-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="d4270-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="d4270-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="d4270-106">Hello információk tooshare QuickBooks követően az alkalmazások az Azure Remoteappban használja.</span><span class="sxs-lookup"><span data-stu-id="d4270-106">Use hello following information tooshare QuickBooks as an app in Azure RemoteApp.</span></span>

<span data-ttu-id="d4270-107">Azure RemoteApp QuickBooks 2015 vállalati felhőalapú vagy hibrid gyűjtemény megoszthatja.</span><span class="sxs-lookup"><span data-stu-id="d4270-107">You can share QuickBooks 2015 Enterprise with Azure RemoteApp in either a hybrid or cloud collection.</span></span> <span data-ttu-id="d4270-108">hello vállalati fájlt a QuickBooks adatbázis-kiszolgáló hello Azure RemoteApp-kiszolgáló számítógéptől eltérő operációs rendszert futtató virtuális gép kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d4270-108">hello company file must reside on a VM that is running QuickBooks database server that is separate from hello Azure RemoteApp servers.</span></span> <span data-ttu-id="d4270-109">Soha ne tárolja az hello vállalati fájlt az Azure RemoteApp-rendszerkép - adatvesztés várható, ha ezt teszi.</span><span class="sxs-lookup"><span data-stu-id="d4270-109">Never store hello company file on your Azure RemoteApp image - data loss is expected if you do this.</span></span> <span data-ttu-id="d4270-110">Csak a QuickBooks vállalati szabványos Windows hálózati elérhetők QuickBooks-kiszolgálóval, külső megosztott üzemeltetési hello QuickBooks fájl támogatja.</span><span class="sxs-lookup"><span data-stu-id="d4270-110">Only QuickBooks Enterprise supports hosting hello QuickBooks file on an external share with QuickBooks database server accessible via standard Windows networking.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="d4270-111">hello QuickBooks adatbázis-kiszolgáló hello vállalati fájlt kell lennie egy külön virtuális gépen belül hello ugyanaz, mint hello Azure RemoteApp-gyűjtemény virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="d4270-111">hello QuickBooks database server that is hosting hello company file must reside on a separate VM within hello same VNET as hello Azure RemoteApp collection.</span></span>  
> 
> 

## <a name="steps-toodeploy-quickbooks"></a><span data-ttu-id="d4270-112">Lépéseket toodeploy QuickBooks</span><span class="sxs-lookup"><span data-stu-id="d4270-112">Steps toodeploy QuickBooks</span></span>
1. <span data-ttu-id="d4270-113">Hozzon létre egy Azure virtuális gép és a QuickBooks, QuickBooks adatbázis-kiszolgáló telepítése, és helyezze hello vállalati fájlt egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="d4270-113">Create an Azure VM and install QuickBooks, QuickBooks database server, and place hello company file on a Azure VM.</span></span>  <span data-ttu-id="d4270-114">Győződjön meg arról, hogy tooproperly tűzfalszabályok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d4270-114">Make sure tooproperly configure firewall rules.</span></span>
2. <span data-ttu-id="d4270-115">QuickBooks telepíthető egy [egyéni lemezkép](remoteapp-imageoptions.md) , és hozzon létre egy [Azure RemoteApp-gyűjtemény](remoteapp-collections.md), vagy felhőalapú vagy hibrid, hello belül hol hello Virtuálisgép üzemeltetési hello QuickBooks adatbázis-kiszolgálón, ugyanazt a virtuális Hálózatot pontos Vállalati fájlok helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="d4270-115">Install QuickBooks on a [custom image](remoteapp-imageoptions.md) and create an [Azure RemoteApp collection](remoteapp-collections.md), either cloud or hybrid, within hello exact same VNET where hello VM hosting hello QuickBooks database server with company files resides.</span></span> 
3. <span data-ttu-id="d4270-116">[Közzététel](remoteapp-publish.md) QuickBooks app toousers</span><span class="sxs-lookup"><span data-stu-id="d4270-116">[Publish](remoteapp-publish.md) QuickBooks app toousers</span></span>
4. <span data-ttu-id="d4270-117">Indítsa el a hello QuickBooks Azure RemoteApp-kiszolgálón futó ügyfél, lépjen a szabványos Windows hálózati toohello Virtuálisgép üzemeltetési hello QuickBooks adatbázis-kiszolgáló használatával hello vállalati fájl megnyitásával.</span><span class="sxs-lookup"><span data-stu-id="d4270-117">Launch hello Azure RemoteApp-hosted QuickBooks client, navigate using standard Windows networking toohello VM hosting hello QuickBooks database server and open hello company file.</span></span> 

## <a name="documentation-references"></a><span data-ttu-id="d4270-118">Dokumentáció hivatkozások</span><span class="sxs-lookup"><span data-stu-id="d4270-118">Documentation references</span></span>
* <span data-ttu-id="d4270-119">QuickBooks [által támogatott konfigurációk](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)</span><span class="sxs-lookup"><span data-stu-id="d4270-119">QuickBooks [supported configurations](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)</span></span>
* <span data-ttu-id="d4270-120">QuickBooks [üzembe helyezési lehetőségei](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)</span><span class="sxs-lookup"><span data-stu-id="d4270-120">QuickBooks [deployment options](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)</span></span>

<span data-ttu-id="d4270-121">Is megtekintheti a ignite-on bemutató [a Microsoft Azure RemoteApp felügyeleti alapjait és felügyeleti](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) -előre too1:02:45 tooget toohello QuickBooks része.</span><span class="sxs-lookup"><span data-stu-id="d4270-121">You can also check out my Ignite presentation, [Fundamentals of Microsoft Azure RemoteApp Management and Administration](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) - fast-forward too1:02:45 tooget toohello QuickBooks part.</span></span>

## <a name="deployment-architecture"></a><span data-ttu-id="d4270-122">Üzembe helyezési architektúrája</span><span class="sxs-lookup"><span data-stu-id="d4270-122">Deployment architecture</span></span>
![QuickBooks + az Azure RemoteApp központi telepítés](./media/remoteapp-quickbooks/ra-quickbooks.png)

