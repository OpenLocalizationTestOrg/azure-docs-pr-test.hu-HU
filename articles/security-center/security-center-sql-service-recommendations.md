---
title: "Azure SQL aaaProtecting szolgáltatás és az adatok az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum címek, amelyek segítenek az Azure Security Center javaslatait védeni kell az adatok és az Azure SQL-szolgáltatás, és maradnak meg a biztonsági házirendeknek megfelelően."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 75d782d3c2418f9645139e4cd6ecb7765c488f91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a><span data-ttu-id="0a4c0-103">Azure SQL-szolgáltatás és az Azure Security Center adatainak védelme</span><span class="sxs-lookup"><span data-stu-id="0a4c0-103">Protecting Azure SQL service and data in Azure Security Center</span></span>
<span data-ttu-id="0a4c0-104">Az Azure Security Center elemzi az Azure-erőforrások hello biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-104">Azure Security Center analyzes hello security state of your Azure resources.</span></span> <span data-ttu-id="0a4c0-105">Amikor a Security Center a potenciális biztonsági hiányosságokat azonosít, hello folyamatán hello szükséges vezérlők, amelyek javaslatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-105">When Security Center identifies potential security vulnerabilities, it creates recommendations that guide you through hello process of configuring hello needed controls.</span></span>  <span data-ttu-id="0a4c0-106">Javaslatok érvényesek tooAzure erőforrástípusok: virtuális gépek (VM), hálózati, SQL és az adatokhoz és alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-106">Recommendations apply tooAzure resource types: virtual machines (VMs), networking, SQL and data, and applications.</span></span>

<span data-ttu-id="0a4c0-107">Ez a cikk foglalkozik tooAzure SQL-szolgáltatás és az adatok vonatkozó javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-107">This article addresses recommendations that apply tooAzure SQL service and data.</span></span> <span data-ttu-id="0a4c0-108">Javaslatok center körül naplózás engedélyezése az Azure SQL-kiszolgálók és adatbázisok, SQL-adatbázisokhoz tartozó titkosítási és az Azure storage-fiók engedélyezése titkosításának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-108">Recommendations center around enabling auditing for Azure SQL servers and databases, enabling encryption for SQL databases, and enabling encryption of your Azure storage account.</span></span>  <span data-ttu-id="0a4c0-109">Használjon hello az alábbi táblázatban a hivatkozás toohelp, ismeri hello elérhető SQL szolgáltatás és az adatok javaslatok és minden egyes funkciója Ha alkalmazza azt.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-109">Use hello table below as a reference toohelp you understand hello available SQL service and data recommendations and what each one does if you apply it.</span></span>

## <a name="available-sql-service-and-data-recommendations"></a><span data-ttu-id="0a4c0-110">Elérhető az SQL szolgáltatás és az adatok javaslatok</span><span class="sxs-lookup"><span data-stu-id="0a4c0-110">Available SQL service and data recommendations</span></span>
| <span data-ttu-id="0a4c0-111">Ajánlás</span><span class="sxs-lookup"><span data-stu-id="0a4c0-111">Recommendation</span></span> | <span data-ttu-id="0a4c0-112">Leírás</span><span class="sxs-lookup"><span data-stu-id="0a4c0-112">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="0a4c0-113">Naplózás és fenyegetésészlelés engedélyezése az SQL-kiszolgálókon</span><span class="sxs-lookup"><span data-stu-id="0a4c0-113">Enable auditing and threat detection on SQL servers</span></span>](security-center-enable-auditing-on-sql-servers.md) |<span data-ttu-id="0a4c0-114">Javasolja, hogy kapcsolja be az Azure SQL-kiszolgálókra (Azure SQL-szolgáltatás csak; nem tartalmazza a virtuális gépeken futó SQL) a naplózás és a fenyegetések észlelésére.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-114">Recommends that you turn on auditing and threat detection for Azure SQL servers (Azure SQL service only; doesn't include SQL running on your virtual machines).</span></span> |
| [<span data-ttu-id="0a4c0-115">Naplózás és fenyegetésészlelés engedélyezése az SQL-adatbázisokon</span><span class="sxs-lookup"><span data-stu-id="0a4c0-115">Enable auditing and threat detection on SQL databases</span></span>](security-center-enable-auditing-on-sql-databases.md) |<span data-ttu-id="0a4c0-116">Javasolja, hogy kapcsolja be az Azure SQL-adatbázisok (Azure SQL-szolgáltatás csak; nem tartalmazza a virtuális gépeken futó SQL) a naplózás és a fenyegetések észlelésére.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-116">Recommends that you turn on auditing and threat detection for Azure SQL databases (Azure SQL service only; doesn't include SQL running on your virtual machines).</span></span> |
| [<span data-ttu-id="0a4c0-117">Az átlátható adattitkosítási engedélyezése az SQL-adatbázisok</span><span class="sxs-lookup"><span data-stu-id="0a4c0-117">Enable Transparent Data Encryption on SQL databases</span></span>](security-center-enable-transparent-data-encryption.md) |<span data-ttu-id="0a4c0-118">Titkosítás az SQL-adatbázisok (csak Azure SQL szolgáltatás) engedélyezését javasolja.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-118">Recommends that you enable encryption for SQL databases (Azure SQL service only).</span></span> |

## <a name="see-also"></a><span data-ttu-id="0a4c0-119">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0a4c0-119">See also</span></span>
<span data-ttu-id="0a4c0-120">További információ az tooother Azure erőforrástípusok, vonatkozó javaslatok toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="0a4c0-120">toolearn more about recommendations that apply tooother Azure resource types, see hello following:</span></span>

* [<span data-ttu-id="0a4c0-121">Az Azure Security Centerben a virtuális gépek védelme</span><span class="sxs-lookup"><span data-stu-id="0a4c0-121">Protecting your virtual machines in Azure Security Center</span></span>](security-center-virtual-machine-recommendations.md)
* [<span data-ttu-id="0a4c0-122">Az alkalmazások az Azure Security Centerben védelme</span><span class="sxs-lookup"><span data-stu-id="0a4c0-122">Protecting your applications in Azure Security Center</span></span>](security-center-application-recommendations.md)
* [<span data-ttu-id="0a4c0-123">Az Azure Security Centerben a hálózat védelme</span><span class="sxs-lookup"><span data-stu-id="0a4c0-123">Protecting your network in Azure Security Center</span></span>](security-center-network-recommendations.md)

<span data-ttu-id="0a4c0-124">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="0a4c0-124">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="0a4c0-125">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-125">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="0a4c0-126">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-126">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="0a4c0-127">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="0a4c0-127">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
