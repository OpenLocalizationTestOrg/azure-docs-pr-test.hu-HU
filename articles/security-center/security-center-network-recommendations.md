---
title: "aaaProtecting a hálózat az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum címek, amelyek segítenek az Azure Security Center javaslatait az Azure-hálózat védelme és maradnak meg a biztonsági házirendeknek megfelelően."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
ms.openlocfilehash: 053738da432edf13b40172fb44d2044702dd8211
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-network-in-azure-security-center"></a><span data-ttu-id="5a8be-103">Az Azure Security Centerben a hálózat védelme</span><span class="sxs-lookup"><span data-stu-id="5a8be-103">Protecting your network in Azure Security Center</span></span>
<span data-ttu-id="5a8be-104">Az Azure Security Center elemzi az Azure-erőforrások hello biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="5a8be-104">Azure Security Center analyzes hello security state of your Azure resources.</span></span> <span data-ttu-id="5a8be-105">Amikor a Security Center a potenciális biztonsági hiányosságokat azonosít, hello folyamatán hello szükséges vezérlők, amelyek javaslatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5a8be-105">When Security Center identifies potential security vulnerabilities, it creates recommendations that guide you through hello process of configuring hello needed controls.</span></span>  <span data-ttu-id="5a8be-106">Javaslatok érvényesek tooAzure erőforrástípusok: virtuális gépek (VM), hálózati, SQL és alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="5a8be-106">Recommendations apply tooAzure resource types: virtual machines (VMs), networking, SQL, and applications.</span></span>

<span data-ttu-id="5a8be-107">Ez a cikk foglalkozik tooyour hálózati vonatkozó javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="5a8be-107">This article addresses recommendations that apply tooyour network.</span></span>  <span data-ttu-id="5a8be-108">Hálózati javaslatok központ következő generációs tűzfal, a hálózati biztonsági csoportok, az konfigurálása bejövő forgalomra vonatkozó szabályokat és több.</span><span class="sxs-lookup"><span data-stu-id="5a8be-108">Network recommendations center around next generation firewalls, Network Security Groups, configuring inbound traffic rules, and more.</span></span>  <span data-ttu-id="5a8be-109">Használjon hello az alábbi táblázatban, egy hivatkozás toohelp ismeri hello rendelkezésre álló hálózati javaslatok és minden egyes funkciója Ha alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="5a8be-109">Use hello table below as a reference toohelp you understand hello available network recommendations and what each one does if you apply it.</span></span>

## <a name="available-network-recommendations"></a><span data-ttu-id="5a8be-110">Rendelkezésre álló hálózati javaslatok</span><span class="sxs-lookup"><span data-stu-id="5a8be-110">Available network recommendations</span></span>
| <span data-ttu-id="5a8be-111">Ajánlás</span><span class="sxs-lookup"><span data-stu-id="5a8be-111">Recommendation</span></span> | <span data-ttu-id="5a8be-112">Leírás</span><span class="sxs-lookup"><span data-stu-id="5a8be-112">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="5a8be-113">Újgenerációs tűzfal hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5a8be-113">Add a Next Generation Firewall</span></span>](security-center-add-next-generation-firewall.md) |<span data-ttu-id="5a8be-114">Javasolja, hogy ad hozzá a következő generációs tűzfal (NGFW) az egy Microsoft partnert tooincrease a biztonsági védelmet.</span><span class="sxs-lookup"><span data-stu-id="5a8be-114">Recommends that you add a Next Generation Firewall (NGFW) from a Microsoft partner tooincrease your security protections.</span></span> |
| [<span data-ttu-id="5a8be-115">Csak az újgenerációs tűzfalon keresztül haladjon a forgalom</span><span class="sxs-lookup"><span data-stu-id="5a8be-115">Route traffic through NGFW only</span></span>](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |<span data-ttu-id="5a8be-116">Javasolja, hogy konfigurálja a hálózati biztonsági csoport (NSG) szabályok, amelyek a bejövő forgalom tooyour keresztül az NGFW VM kényszerítése.</span><span class="sxs-lookup"><span data-stu-id="5a8be-116">Recommends that you configure network security group (NSG) rules that force inbound traffic tooyour VM through your NGFW.</span></span> |
| [<span data-ttu-id="5a8be-117">Hálózati biztonsági csoportok engedélyezi az alhálózatokat vagy a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="5a8be-117">Enable Network Security Groups on subnets or virtual machines</span></span>](security-center-enable-network-security-groups.md) |<span data-ttu-id="5a8be-118">Az alhálózatok és virtuális gépek NSG-k engedélyezését javasolja.</span><span class="sxs-lookup"><span data-stu-id="5a8be-118">Recommends that you enable NSGs on subnets or VMs.</span></span> |
| [<span data-ttu-id="5a8be-119">Internetre irányuló végpont-en keresztüli hozzáférés korlátozása</span><span class="sxs-lookup"><span data-stu-id="5a8be-119">Restrict access through Internet facing endpoint</span></span>](security-center-restrict-access-through-internet-facing-endpoints.md) |<span data-ttu-id="5a8be-120">Azt javasolja, hogy az NSG-ket konfigurálhat a bejövő forgalomra vonatkozó szabályokat.</span><span class="sxs-lookup"><span data-stu-id="5a8be-120">Recommends that you configure inbound traffic rules for NSGs.</span></span> |

## <a name="see-also"></a><span data-ttu-id="5a8be-121">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5a8be-121">See also</span></span>
<span data-ttu-id="5a8be-122">További információ az tooother Azure erőforrástípusok, vonatkozó javaslatok toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="5a8be-122">toolearn more about recommendations that apply tooother Azure resource types, see hello following:</span></span>

* [<span data-ttu-id="5a8be-123">Az Azure Security Centerben a virtuális gépek védelme</span><span class="sxs-lookup"><span data-stu-id="5a8be-123">Protecting your virtual machines in Azure Security Center</span></span>](security-center-virtual-machine-recommendations.md)
* [<span data-ttu-id="5a8be-124">Az alkalmazások az Azure Security Centerben védelme</span><span class="sxs-lookup"><span data-stu-id="5a8be-124">Protecting your applications in Azure Security Center</span></span>](security-center-application-recommendations.md)
* [<span data-ttu-id="5a8be-125">Az Azure Security Centerben az Azure SQL-szolgáltatás védelme</span><span class="sxs-lookup"><span data-stu-id="5a8be-125">Protecting your Azure SQL service in Azure Security Center</span></span>](security-center-sql-service-recommendations.md)

<span data-ttu-id="5a8be-126">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="5a8be-126">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="5a8be-127">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="5a8be-127">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="5a8be-128">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="5a8be-128">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="5a8be-129">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5a8be-129">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
