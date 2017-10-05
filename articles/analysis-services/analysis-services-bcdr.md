---
title: "Az Azure Analysis Services magas rendelkezésre állású |} Microsoft Docs"
description: "Modulhoz Azure Analysis Services magas rendelkezésre állású."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 84b4c59bac1feeb8611b3a8d783d093ba073e532
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="analysis-services-high-availability"></a><span data-ttu-id="2fac1-103">Analysis Services magas rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="2fac1-103">Analysis Services high availability</span></span>
<span data-ttu-id="2fac1-104">Ez a cikk ismerteti, biztosítva ezzel az Azure Analysis Services-kiszolgálók magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="2fac1-104">This article describes assuring high availability for Azure Analysis Services servers.</span></span> 


## <a name="assuring-high-availability-during-a-service-disruption"></a><span data-ttu-id="2fac1-105">A szolgáltatás szüneteltetése során magas rendelkezésre állású modulhoz</span><span class="sxs-lookup"><span data-stu-id="2fac1-105">Assuring high availability during a service disruption</span></span>
<span data-ttu-id="2fac1-106">Ritka, amíg az Azure-adatközpont kimaradás is rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="2fac1-106">While rare, an Azure data center can have an outage.</span></span> <span data-ttu-id="2fac1-107">Ha kimaradás lép, a business szüneteltetése, előfordulhat, hogy az utolsó néhány percet, vagy előfordulhat, hogy az óra utolsó okoz.</span><span class="sxs-lookup"><span data-stu-id="2fac1-107">When an outage occurs, it causes a business disruption that might last a few minutes or might last for hours.</span></span> <span data-ttu-id="2fac1-108">Magas rendelkezésre állású kiszolgáló redundanciával leggyakrabban érhető el.</span><span class="sxs-lookup"><span data-stu-id="2fac1-108">High availability is most often achieved with server redundancy.</span></span> <span data-ttu-id="2fac1-109">Az Azure Analysis Services redundancia érhet el, további, másodlagos kiszolgálók létrehozása egy vagy több régióban.</span><span class="sxs-lookup"><span data-stu-id="2fac1-109">With Azure Analysis Services, you can achieve redundancy by creating additional, secondary servers in one or more regions.</span></span> <span data-ttu-id="2fac1-110">Ahhoz, hogy biztosítsa az adatok és metaadatok az adott kiszolgálókon, redundáns kiszolgálók létrehozása a szinkronizálás régióban a kiszolgálóval, amely offline állapotba került, akkor is:</span><span class="sxs-lookup"><span data-stu-id="2fac1-110">When creating redundant servers, to assure the data and metadata on those servers is in-sync with the server in a region that has gone offline, you can:</span></span>

* <span data-ttu-id="2fac1-111">Központilag telepíteni modellek redundáns más régiókban.</span><span class="sxs-lookup"><span data-stu-id="2fac1-111">Deploy models to redundant servers in other regions.</span></span> <span data-ttu-id="2fac1-112">Ennél a módszernél adatainak feldolgozása az elsődleges kiszolgáló és a redundáns kiszolgálók a párhuzamos, biztosítva ezzel az összes kiszolgáló a szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="2fac1-112">This method requires processing data on both your primary server and redundant servers in-parallel, assuring all servers are in-sync.</span></span>

* <span data-ttu-id="2fac1-113">Adatbázisok biztonsági mentése az elsődleges kiszolgáló és a visszaállítási redundáns kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="2fac1-113">Back up databases from your primary server and restore on redundant servers.</span></span> <span data-ttu-id="2fac1-114">Például automatizálhatja az Azure storage ütemezett biztonsági mentések, és állítsa vissza a más régiókban található egyéb redundáns kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="2fac1-114">For example, you can automate nightly backups to Azure storage, and restore to other redundant servers in other regions.</span></span> 

<span data-ttu-id="2fac1-115">Mindkét esetben ha az elsődleges kiszolgáló nem tervezett kimaradás, módosítania kell a jelentéskészítési ügyfelek csatlakozni a kiszolgálóhoz különböző területi adatközpontban kapcsolati karakterláncokat.</span><span class="sxs-lookup"><span data-stu-id="2fac1-115">In either case, if your primary server experiences an outage, you must change the connection strings in reporting clients to connect to the server in a different regional datacenter.</span></span> <span data-ttu-id="2fac1-116">Ez a változás érdemes figyelembe venni a végső esetben végezze el, és csak akkor, ha egy katasztrofális regionális adatközpont-meghibásodás után következik be.</span><span class="sxs-lookup"><span data-stu-id="2fac1-116">This change should be considered a last resort and only if a catastrophic regional data center outage occurs.</span></span> <span data-ttu-id="2fac1-117">Valószínűbb egy adatközpont-meghibásodás után az elsődleges kiszolgálót futtató szeretné újra hálózatra tud kapcsolódni sikerült kapcsolatok használatát az összes ügyfél frissítése előtt.</span><span class="sxs-lookup"><span data-stu-id="2fac1-117">It's more likely a data center outage hosting your primary server would come back online before you could update connections on all clients.</span></span> 



## <a name="related-information"></a><span data-ttu-id="2fac1-118">Kapcsolódó információk</span><span class="sxs-lookup"><span data-stu-id="2fac1-118">Related information</span></span>
<span data-ttu-id="2fac1-119">[Biztonsági mentés és helyreállítás](analysis-services-backup.md) </span><span class="sxs-lookup"><span data-stu-id="2fac1-119">[Backup and restore](analysis-services-backup.md) </span></span>  
[<span data-ttu-id="2fac1-120">Az Azure Analysis Services kezelése</span><span class="sxs-lookup"><span data-stu-id="2fac1-120">Manage Azure Analysis Services</span></span>](analysis-services-manage.md) 

