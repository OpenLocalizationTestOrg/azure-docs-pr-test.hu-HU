---
title: "Az Azure DNS áttekintése |} Microsoft Docs"
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
ms.openlocfilehash: 3705457e4c90f8869496f7f5177531bd128d1057
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-dns-overview"></a><span data-ttu-id="31701-104">Az Azure DNS áttekintése</span><span class="sxs-lookup"><span data-stu-id="31701-104">Azure DNS overview</span></span>

<span data-ttu-id="31701-105">A tartománynévrendszer, vagy a DNS-, felelős fordítása (vagy feloldása) az IP-címét egy webhely vagy szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="31701-105">The Domain Name System, or DNS, is responsible for translating (or resolving) a website or service name to its IP address.</span></span> <span data-ttu-id="31701-106">Az Azure DNS egy olyan üzemeltetési szolgáltatás DNS-tartományok, biztosítani a névfeloldást a Microsoft Azure-infrastruktúra használatával.</span><span class="sxs-lookup"><span data-stu-id="31701-106">Azure DNS is a hosting service for DNS domains, providing name resolution using Microsoft Azure infrastructure.</span></span> <span data-ttu-id="31701-107">Ha tartományait az Azure-ban üzemelteti, DNS-rekordjait a többi Azure-szolgáltatáshoz is használt hitelesítő adatokkal, API-kkal, eszközökkel és számlázási információkkal kezelheti.</span><span class="sxs-lookup"><span data-stu-id="31701-107">By hosting your domains in Azure, you can manage your DNS records using the same credentials, APIs, tools, and billing as your other Azure services.</span></span>

![DNS áttekintése](./media/dns-overview/scenario.png)

## <a name="features"></a><span data-ttu-id="31701-109">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="31701-109">Features</span></span>

* <span data-ttu-id="31701-110">**Megbízhatóság és teljesítmény** -DNS-tartományok Azure DNS-ben üzemeltetett globális hálózata Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="31701-110">**Reliability and performance** - DNS domains in Azure DNS are hosted on Azure's global network of DNS name servers.</span></span> <span data-ttu-id="31701-111">Nem egyedi, hogy minden DNS-lekérdezés válaszolt a legközelebbi elérhető DNS-kiszolgáló hálózati is használjuk.</span><span class="sxs-lookup"><span data-stu-id="31701-111">We use Anycast networking so that each DNS query is answered by the closest available DNS server.</span></span> <span data-ttu-id="31701-112">Gyors teljesítmény és a magas rendelkezésre állást biztosít a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="31701-112">This provides both fast performance and high availability for your domain.</span></span>

* <span data-ttu-id="31701-113">**Zökkenőmentes integráció** – az Azure DNS-szolgáltatás segítségével az Azure-szolgáltatások DNS-rekordok kezelése, és adja meg a DNS, valamint a külső erőforrásokhoz is használható.</span><span class="sxs-lookup"><span data-stu-id="31701-113">**Seamless integration** - The Azure DNS service can be used to manage DNS records for your Azure services and can be used to provide DNS for your external resources as well.</span></span> <span data-ttu-id="31701-114">Az Azure DNS integrálva van az Azure portálon, és ugyanazokat a hitelesítő adatokat, számlázási és támogatási szerződése használja, mint a más Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="31701-114">Azure DNS is integrated in the Azure portal and uses the same credentials, billing and support contract as your other Azure services.</span></span>

* <span data-ttu-id="31701-115">**Biztonsági** – az Azure DNS szolgáltatást az Azure Resource Manager alapul.</span><span class="sxs-lookup"><span data-stu-id="31701-115">**Security** - The Azure DNS service is based on Azure Resource Manager.</span></span> <span data-ttu-id="31701-116">Ilyen előnyöket az erőforrás-kezelő szolgáltatásait, például a szerepköralapú hozzáférés-vezérlés, a vizsgálati naplók és a erőforrás zárolását.</span><span class="sxs-lookup"><span data-stu-id="31701-116">As such, it benefits from Resource Manager features such as role-based access control, audit logs, and resource locking.</span></span> <span data-ttu-id="31701-117">A tartományok és a rekordok az Azure portál, Azure PowerShell-parancsmagok és a platformok közötti Azure CLI segítségével is kezelhető.</span><span class="sxs-lookup"><span data-stu-id="31701-117">Your domains and records can be managed via the Azure portal, Azure PowerShell cmdlets, and the cross-platform Azure CLI.</span></span> <span data-ttu-id="31701-118">Automatikus DNS-kezelési igénylő alkalmazásokhoz integrálható a REST API-t és az SDK szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="31701-118">Applications requiring automatic DNS management can integrate with the service via the REST API and SDKs.</span></span>

<span data-ttu-id="31701-119">Az Azure DNS jelenleg nem támogatja tartománynevek megvásárlását.</span><span class="sxs-lookup"><span data-stu-id="31701-119">Azure DNS does not currently support purchasing of domain names.</span></span> <span data-ttu-id="31701-120">Tartományok beszerezni kívánt, ha meg szeretné használni, a külső tartományregisztráló nevét.</span><span class="sxs-lookup"><span data-stu-id="31701-120">If you want to purchase domains, you need to use a third-party domain name registrar.</span></span> <span data-ttu-id="31701-121">A regisztráló általában egy kis éves díj költségek.</span><span class="sxs-lookup"><span data-stu-id="31701-121">The registrar typically charges a small annual fee.</span></span> <span data-ttu-id="31701-122">A tartomány DNS-rekordok Management majd az Azure DNS-lehet üzemeltetni.</span><span class="sxs-lookup"><span data-stu-id="31701-122">The domains can then be hosted in Azure DNS for management of DNS records.</span></span> <span data-ttu-id="31701-123">Lásd: [az Azure DNS-tartomány delegálása az](dns-domain-delegation.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="31701-123">See [Delegate a Domain to Azure DNS](dns-domain-delegation.md) for details.</span></span>

## <a name="pricing"></a><span data-ttu-id="31701-124">Díjszabás</span><span class="sxs-lookup"><span data-stu-id="31701-124">Pricing</span></span>

<span data-ttu-id="31701-125">DNS számlázási az Azure-ban és a DNS-lekérdezések száma által üzemeltetett DNS-zónák számát alapul.</span><span class="sxs-lookup"><span data-stu-id="31701-125">DNS billing is based on the number of DNS zones hosted in Azure and by the number of DNS queries.</span></span> <span data-ttu-id="31701-126">További információt a díjszabás látogasson el [Azure DNS árképzési](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="31701-126">To learn more about pricing visit [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).</span></span>

## <a name="faq"></a><span data-ttu-id="31701-127">GYIK</span><span class="sxs-lookup"><span data-stu-id="31701-127">FAQ</span></span>

<span data-ttu-id="31701-128">További Azure DNS szolgáltatással kapcsolatos gyakran ismételt kérdések: a [Azure DNS gyakran ismételt kérdések](dns-faq.md).</span><span class="sxs-lookup"><span data-stu-id="31701-128">For frequently asked questions about Azure DNS, see the [Azure DNS FAQ](dns-faq.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="31701-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="31701-129">Next steps</span></span>

<span data-ttu-id="31701-130">További információk a DNS-zónák és rekordok ellátogatva: [DNS-zónák és áttekintése rögzíti](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="31701-130">Learn about DNS zones and records by visiting: [DNS zones and records overview](dns-zones-records.md).</span></span>

<span data-ttu-id="31701-131">Megtudhatja, hogyan [hozzon létre egy DNS-zóna](./dns-getstarted-create-dnszone-portal.md) Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="31701-131">Learn how to [create a DNS zone](./dns-getstarted-create-dnszone-portal.md) in Azure DNS.</span></span>

<span data-ttu-id="31701-132">Ebben a dokumentumban az Azure egyéb lényeges [hálózat képességeivel](../networking/networking-overview.md) ismerkedhet meg.</span><span class="sxs-lookup"><span data-stu-id="31701-132">Learn about some of the other key [networking capabilities](../networking/networking-overview.md) of Azure.</span></span>

