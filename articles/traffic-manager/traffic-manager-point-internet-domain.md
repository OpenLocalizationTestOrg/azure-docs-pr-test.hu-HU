---
title: "a vállalati internetes tartomány tooa Traffic Manager szolgáltatásbeli tartománynevére aaaPoint |} Microsoft Docs"
description: "Ez a cikk segítséget nyújt a vállalati tartomány neve tooa Traffic Manager szolgáltatásbeli tartománynevére mutasson."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 29822946-2d45-4434-ba47-fc180a445cc3
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
ms.openlocfilehash: 84c428f60a1dc70452bf957d98a68c95e0b51715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="point-a-company-internet-domain-tooan-azure-traffic-manager-domain"></a><span data-ttu-id="f1aa6-103">A vállalati internetes tartomány tooan Azure Traffic Manager-tartományra mutasson.</span><span class="sxs-lookup"><span data-stu-id="f1aa6-103">Point a company Internet domain tooan Azure Traffic Manager domain</span></span>

<span data-ttu-id="f1aa6-104">Amikor Traffic Manager-profilt hoz létre, az Azure automatikusan hozzárendel egy DNS-nevet a profilhoz.</span><span class="sxs-lookup"><span data-stu-id="f1aa6-104">When you create a Traffic Manager profile, Azure automatically assigns a DNS name for that profile.</span></span> <span data-ttu-id="f1aa6-105">toouse egy nevet a DNS-zónából, hozzon létre egy CNAME DNS-rekordot, amely leképezi a Traffic Manager-profil tartománynevére toohello.</span><span class="sxs-lookup"><span data-stu-id="f1aa6-105">toouse a name from your DNS zone, create a CNAME DNS record that maps toohello domain name of your Traffic Manager profile.</span></span> <span data-ttu-id="f1aa6-106">Hello Traffic Manager szolgáltatásbeli tartománynevére hello található **általános** hello Traffic Manager-profil szakasza hello konfigurálása lapon.</span><span class="sxs-lookup"><span data-stu-id="f1aa6-106">You can find hello Traffic Manager domain name in hello **General** section on hello Configuration page of hello Traffic Manager profile.</span></span>

<span data-ttu-id="f1aa6-107">Például toopoint neve www.contoso.com toohello Traffic Manager DNS-név contoso.trafficmanager.net, akkor létrehoznia a következő DNS-erőforrásrekordot hello:</span><span class="sxs-lookup"><span data-stu-id="f1aa6-107">For example, toopoint name www.contoso.com toohello Traffic Manager DNS name contoso.trafficmanager.net, you would create hello following DNS resource record:</span></span>

    www.contoso.com IN CNAME contoso.trafficmanager.net

<span data-ttu-id="f1aa6-108">Az összes forgalom kérelmek túl*www.contoso.com* beolvasása irányítja a rendszer túl*contoso.trafficmanager.net*.</span><span class="sxs-lookup"><span data-stu-id="f1aa6-108">All traffic requests too*www.contoso.com* get directed too*contoso.trafficmanager.net*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f1aa6-109">A második szintű tartomány például nem mutathat *contoso.com*, toohello Traffic Manager-tartományra.</span><span class="sxs-lookup"><span data-stu-id="f1aa6-109">You cannot point a second-level domain, such as *contoso.com*, toohello Traffic Manager domain.</span></span> <span data-ttu-id="f1aa6-110">A DNS-protokollszabványok nem engedélyezik a CNAME-rekordokat a másodlagos szintű tartománynevek esetében.</span><span class="sxs-lookup"><span data-stu-id="f1aa6-110">DNS protocol standards do not allow CNAME records for second-level domain names.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1aa6-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1aa6-111">Next steps</span></span>

* [<span data-ttu-id="f1aa6-112">A Traffic Manager útválasztási módszerei</span><span class="sxs-lookup"><span data-stu-id="f1aa6-112">Traffic Manager routing methods</span></span>](traffic-manager-routing-methods.md)
* [<span data-ttu-id="f1aa6-113">Traffic Manager – Profil letiltása, engedélyezése vagy törlése</span><span class="sxs-lookup"><span data-stu-id="f1aa6-113">Traffic Manager - Disable, enable or delete a profile</span></span>](disable-enable-or-delete-a-profile.md)
* [<span data-ttu-id="f1aa6-114">Traffic Manager – Végpont letiltása vagy engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f1aa6-114">Traffic Manager - Disable or enable an endpoint</span></span>](disable-or-enable-an-endpoint.md)
