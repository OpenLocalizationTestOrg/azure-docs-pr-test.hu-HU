---
title: "Vállalati internetes tartomány átirányítása Traffic Manager szolgáltatásbeli tartománynévre | Microsoft Docs"
description: "Ez a cikk segít átirányítani a vállalata tartománynevét egy Traffic Manager szolgáltatásbeli tartománynevére."
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
ms.openlocfilehash: 0322b3510cfd4f94031d8c1db8f1cc032b997fa8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a><span data-ttu-id="13ba0-103">Vállalati internetes tartomány átirányítása Azure Traffic Manager-tartományra</span><span class="sxs-lookup"><span data-stu-id="13ba0-103">Point a company Internet domain to an Azure Traffic Manager domain</span></span>

<span data-ttu-id="13ba0-104">Amikor Traffic Manager-profilt hoz létre, az Azure automatikusan hozzárendel egy DNS-nevet a profilhoz.</span><span class="sxs-lookup"><span data-stu-id="13ba0-104">When you create a Traffic Manager profile, Azure automatically assigns a DNS name for that profile.</span></span> <span data-ttu-id="13ba0-105">A DNS-zónájából való név felhasználásához hozzon létre egy CNAME DNS-rekordot, amely leképezi a Traffic Manager-profiljának tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="13ba0-105">To use a name from your DNS zone, create a CNAME DNS record that maps to the domain name of your Traffic Manager profile.</span></span> <span data-ttu-id="13ba0-106">A Traffic Manager tartományneve a Traffic Manager-profil Konfiguráció lapjának **Általános** részében tekinthető meg.</span><span class="sxs-lookup"><span data-stu-id="13ba0-106">You can find the Traffic Manager domain name in the **General** section on the Configuration page of the Traffic Manager profile.</span></span>

<span data-ttu-id="13ba0-107">Ha például a www.contoso.com vállalati tartománynevet szeretné a Traffic Manager szolgáltatásbeli DNS-névre (contoso.trafficmanager.net) átirányítani, az alábbi DNS-erőforrásrekordot kell létrehoznia:</span><span class="sxs-lookup"><span data-stu-id="13ba0-107">For example, to point name www.contoso.com to the Traffic Manager DNS name contoso.trafficmanager.net, you would create the following DNS resource record:</span></span>

    www.contoso.com IN CNAME contoso.trafficmanager.net

<span data-ttu-id="13ba0-108">A rendszer ezentúl a *www.contoso.com* tartományhoz érkező összes forgalomkérést átirányítja a *contoso.trafficmanager.net* tartománynak.</span><span class="sxs-lookup"><span data-stu-id="13ba0-108">All traffic requests to *www.contoso.com* get directed to *contoso.trafficmanager.net*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="13ba0-109">A második szintű tartomány, például a *contoso.com*, nem irányítható á Traffic Manager-tartományra.</span><span class="sxs-lookup"><span data-stu-id="13ba0-109">You cannot point a second-level domain, such as *contoso.com*, to the Traffic Manager domain.</span></span> <span data-ttu-id="13ba0-110">A DNS-protokollszabványok nem engedélyezik a CNAME-rekordokat a másodlagos szintű tartománynevek esetében.</span><span class="sxs-lookup"><span data-stu-id="13ba0-110">DNS protocol standards do not allow CNAME records for second-level domain names.</span></span>

## <a name="next-steps"></a><span data-ttu-id="13ba0-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13ba0-111">Next steps</span></span>

* [<span data-ttu-id="13ba0-112">A Traffic Manager útválasztási módszerei</span><span class="sxs-lookup"><span data-stu-id="13ba0-112">Traffic Manager routing methods</span></span>](traffic-manager-routing-methods.md)
* [<span data-ttu-id="13ba0-113">Traffic Manager – Profil letiltása, engedélyezése vagy törlése</span><span class="sxs-lookup"><span data-stu-id="13ba0-113">Traffic Manager - Disable, enable or delete a profile</span></span>](disable-enable-or-delete-a-profile.md)
* [<span data-ttu-id="13ba0-114">Traffic Manager – Végpont letiltása vagy engedélyezése</span><span class="sxs-lookup"><span data-stu-id="13ba0-114">Traffic Manager - Disable or enable an endpoint</span></span>](disable-or-enable-an-endpoint.md)
