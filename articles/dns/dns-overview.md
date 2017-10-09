---
title: az Azure DNS aaaOverview |} Microsoft Docs
description: "A Microsoft Azure szolgáltatást tartalmazó DNS áttekintése. A Microsoft Azure-tartomány üzemeltetésére."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: gwallace
ms.openlocfilehash: a10f87c488356469e9c04aabde31129049563891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-overview"></a><span data-ttu-id="987f8-104">Az Azure DNS áttekintése</span><span class="sxs-lookup"><span data-stu-id="987f8-104">Azure DNS overview</span></span>

<span data-ttu-id="987f8-105">Tartománynévrendszer hello, vagy a DNS, felelős fordítása (vagy feloldása) egy webhelyre vagy szolgáltatásba név tooits IP-cím.</span><span class="sxs-lookup"><span data-stu-id="987f8-105">hello Domain Name System, or DNS, is responsible for translating (or resolving) a website or service name tooits IP address.</span></span> <span data-ttu-id="987f8-106">Az Azure DNS egy olyan üzemeltetési szolgáltatás DNS-tartományok, biztosítani a névfeloldást a Microsoft Azure-infrastruktúra használatával.</span><span class="sxs-lookup"><span data-stu-id="987f8-106">Azure DNS is a hosting service for DNS domains, providing name resolution using Microsoft Azure infrastructure.</span></span> <span data-ttu-id="987f8-107">Által az Azure-ban a tartományt üzemeltet, kezelheti a DNS-rekordok hello ugyanazon hitelesítő adatokkal, API-k, eszközök és számlázási, a más Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="987f8-107">By hosting your domains in Azure, you can manage your DNS records using hello same credentials, APIs, tools, and billing as your other Azure services.</span></span>

![DNS áttekintése](./media/dns-overview/scenario.png)

## <a name="features"></a><span data-ttu-id="987f8-109">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="987f8-109">Features</span></span>

* <span data-ttu-id="987f8-110">**Megbízhatóság és teljesítmény** -DNS-tartományok Azure DNS-ben üzemeltetett globális hálózata Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="987f8-110">**Reliability and performance** - DNS domains in Azure DNS are hosted on Azure's global network of DNS name servers.</span></span> <span data-ttu-id="987f8-111">Nem egyedi, hogy minden DNS-lekérdezés melléket hello legközelebbi elérhető DNS-kiszolgáló hálózati is használjuk.</span><span class="sxs-lookup"><span data-stu-id="987f8-111">We use Anycast networking so that each DNS query is answered by hello closest available DNS server.</span></span> <span data-ttu-id="987f8-112">Gyors teljesítmény és a magas rendelkezésre állást biztosít a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="987f8-112">This provides both fast performance and high availability for your domain.</span></span>

* <span data-ttu-id="987f8-113">**Zökkenőmentes integráció** -hello Azure DNS szolgáltatást lehet használt toomanage DNS-rekordjait az Azure-szolgáltatások és használt tooprovide DNS, valamint a külső erőforrások lehetnek.</span><span class="sxs-lookup"><span data-stu-id="987f8-113">**Seamless integration** - hello Azure DNS service can be used toomanage DNS records for your Azure services and can be used tooprovide DNS for your external resources as well.</span></span> <span data-ttu-id="987f8-114">Az Azure DNS integrálva van a hello Azure-portálon, és használja ugyanazokat a hitelesítő adatokat, a számlázásra és a támogatási szerződése hello mint a más Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="987f8-114">Azure DNS is integrated in hello Azure portal and uses hello same credentials, billing and support contract as your other Azure services.</span></span>

* <span data-ttu-id="987f8-115">**Biztonsági** -hello Azure DNS szolgáltatást az Azure Resource Manager alapul.</span><span class="sxs-lookup"><span data-stu-id="987f8-115">**Security** - hello Azure DNS service is based on Azure Resource Manager.</span></span> <span data-ttu-id="987f8-116">Ilyen előnyöket az erőforrás-kezelő szolgáltatásait, például a szerepköralapú hozzáférés-vezérlés, a vizsgálati naplók és a erőforrás zárolását.</span><span class="sxs-lookup"><span data-stu-id="987f8-116">As such, it benefits from Resource Manager features such as role-based access control, audit logs, and resource locking.</span></span> <span data-ttu-id="987f8-117">A tartományok és a rekordok hello Azure-portálon az Azure PowerShell-parancsmagok segítségével kezelhetők, és platformfüggetlen Azure CLI hello.</span><span class="sxs-lookup"><span data-stu-id="987f8-117">Your domains and records can be managed via hello Azure portal, Azure PowerShell cmdlets, and hello cross-platform Azure CLI.</span></span> <span data-ttu-id="987f8-118">Automatikus DNS-kezelési igénylő alkalmazások integrálhatók a hello keresztül hello szolgáltatás REST API-t és az SDK-k.</span><span class="sxs-lookup"><span data-stu-id="987f8-118">Applications requiring automatic DNS management can integrate with hello service via hello REST API and SDKs.</span></span>

<span data-ttu-id="987f8-119">Az Azure DNS jelenleg nem támogatja tartománynevek megvásárlását.</span><span class="sxs-lookup"><span data-stu-id="987f8-119">Azure DNS does not currently support purchasing of domain names.</span></span> <span data-ttu-id="987f8-120">Ha azt szeretné, hogy toopurchase tartományok, egy külső regisztrációs toouse kell.</span><span class="sxs-lookup"><span data-stu-id="987f8-120">If you want toopurchase domains, you need toouse a third-party domain name registrar.</span></span> <span data-ttu-id="987f8-121">hello regisztráló általában egy kis éves díj költségek.</span><span class="sxs-lookup"><span data-stu-id="987f8-121">hello registrar typically charges a small annual fee.</span></span> <span data-ttu-id="987f8-122">hello tartományok majd az Azure DNS-lehet üzemeltetni, a DNS-rekordok kezelése.</span><span class="sxs-lookup"><span data-stu-id="987f8-122">hello domains can then be hosted in Azure DNS for management of DNS records.</span></span> <span data-ttu-id="987f8-123">Lásd: [delegálása a tartományi tooAzure DNS](dns-domain-delegation.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="987f8-123">See [Delegate a Domain tooAzure DNS](dns-domain-delegation.md) for details.</span></span>

## <a name="pricing"></a><span data-ttu-id="987f8-124">Díjszabás</span><span class="sxs-lookup"><span data-stu-id="987f8-124">Pricing</span></span>

<span data-ttu-id="987f8-125">DNS számlázási hello száma az Azure-ban és a DNS-lekérdezések száma hello által üzemeltetett DNS-zónák alapul.</span><span class="sxs-lookup"><span data-stu-id="987f8-125">DNS billing is based on hello number of DNS zones hosted in Azure and by hello number of DNS queries.</span></span> <span data-ttu-id="987f8-126">látogasson el árazással kapcsolatos további toolearn [Azure DNS árképzési](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="987f8-126">toolearn more about pricing visit [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).</span></span>

## <a name="faq"></a><span data-ttu-id="987f8-127">GYIK</span><span class="sxs-lookup"><span data-stu-id="987f8-127">FAQ</span></span>

<span data-ttu-id="987f8-128">További Azure DNS szolgáltatással kapcsolatos gyakran ismételt kérdések: hello [Azure DNS gyakran ismételt kérdések](dns-faq.md).</span><span class="sxs-lookup"><span data-stu-id="987f8-128">For frequently asked questions about Azure DNS, see hello [Azure DNS FAQ](dns-faq.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="987f8-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="987f8-129">Next steps</span></span>

<span data-ttu-id="987f8-130">További információk a DNS-zónák és rekordok ellátogatva: [DNS-zónák és áttekintése rögzíti](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="987f8-130">Learn about DNS zones and records by visiting: [DNS zones and records overview](dns-zones-records.md).</span></span>

<span data-ttu-id="987f8-131">Ismerje meg, hogyan túl[hozzon létre egy DNS-zóna](./dns-getstarted-create-dnszone-portal.md) Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="987f8-131">Learn how too[create a DNS zone](./dns-getstarted-create-dnszone-portal.md) in Azure DNS.</span></span>

<span data-ttu-id="987f8-132">Megismerhet néhány hello más kulcs [hálózati lehetőségeket](../networking/networking-overview.md) Azure.</span><span class="sxs-lookup"><span data-stu-id="987f8-132">Learn about some of hello other key [networking capabilities](../networking/networking-overview.md) of Azure.</span></span>

