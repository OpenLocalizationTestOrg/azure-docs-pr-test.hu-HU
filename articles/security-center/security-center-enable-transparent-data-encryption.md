---
title: "Engedélyezze az átlátható adattitkosítást az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan valósítja meg az Azure Security Center ajánlás ** engedélyezése átlátszó adatok titkosítás **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 2a2963affdbff3710ad08f86c6ed4e6304335559
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a><span data-ttu-id="621fd-103">Engedélyezze az átlátható adattitkosítást az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="621fd-103">Enable Transparent Data Encryption in Azure Security Center</span></span>
<span data-ttu-id="621fd-104">Azure Security Center javasolni fogja engedélyezése átlátszó Data Encryption (TDE) az SQL-adatbázisok, ha a TDE nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="621fd-104">Azure Security Center will recommend that you enable Transparent Data Encryption (TDE) on SQL databases if TDE is not already enabled.</span></span> <span data-ttu-id="621fd-105">TDE védi az adatokat, és segítséget nyújt a megfelelőségi követelményeknek való megfelelést az adatbázis, a társított biztonsági másolatok és a tranzakciós naplófájlok nyugalmi, anélkül, hogy az alkalmazást módosítani kellene történő.</span><span class="sxs-lookup"><span data-stu-id="621fd-105">TDE protects your data and helps you meet compliance requirements by encrypting your database, associated backups, and transaction log files at rest, without requiring changes to your application.</span></span> <span data-ttu-id="621fd-106">További részletek további [átlátható adattitkosítást az Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span><span class="sxs-lookup"><span data-stu-id="621fd-106">To learn more see [Transparent Data Encryption with Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span></span>

<span data-ttu-id="621fd-107">Ez a javaslat vonatkozik az Azure SQL-szolgáltatás csak; a virtuális gépeken futó SQL nem tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="621fd-107">This recommendation applies to the Azure SQL service only; doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="621fd-108">Ez a dokumentum egy üzembe helyezést szemléltető példa segítségével mutatja be a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="621fd-108">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="621fd-109">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="621fd-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="621fd-110">A javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="621fd-110">Implement the recommendation</span></span>
1. <span data-ttu-id="621fd-111">Az a **javaslatok** panelen válassza **átlátható adattitkosítási engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="621fd-111">In the **Recommendations** blade, select **Enable Transparent Data Encryption**.</span></span>
   <span data-ttu-id="621fd-112">![Transzparens adattitkosítás engedélyezése][1]</span><span class="sxs-lookup"><span data-stu-id="621fd-112">![Enable Transparent Data Encryption][1]</span></span>
2. <span data-ttu-id="621fd-113">Ekkor megnyílik a **átlátható adattitkosítási engedélyezése az SQL-adatbázisok** panelen.</span><span class="sxs-lookup"><span data-stu-id="621fd-113">This opens the **Enable Transparent Data Encryption on SQL databases** blade.</span></span> <span data-ttu-id="621fd-114">Válassza ki a TDE engedélyezése az SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="621fd-114">Select a SQL database to enable TDE on.</span></span>
   <span data-ttu-id="621fd-115">![Válassza ki az SQL-adatbázis a TDE engedélyezése][2]</span><span class="sxs-lookup"><span data-stu-id="621fd-115">![Select SQL DB to enable TDE on][2]</span></span>
3. <span data-ttu-id="621fd-116">Az a **átlátható adattitkosítás** panelen válassza **ON** adatok titkosítását, és válassza a **mentése** a panel felső szalagon.</span><span class="sxs-lookup"><span data-stu-id="621fd-116">On the **Transparent data encryption** blade, select **ON** under Data encryption and select **Save** in the top ribbon of the blade.</span></span>
   <span data-ttu-id="621fd-117">![Kapcsolja be a TDE][3]</span><span class="sxs-lookup"><span data-stu-id="621fd-117">![Turn on TDE][3]</span></span>

   <span data-ttu-id="621fd-118">Ha a TDE engedélyezve van a kiválasztott SQL-adatbázis a **titkosítási állapotát** módosul **titkosított**.</span><span class="sxs-lookup"><span data-stu-id="621fd-118">Once TDE is enabled on the selected SQL database, the **Encryption status** will change to **Encrypted**.</span></span>    

   ![A titkosítás állapota][4]

## <a name="see-also"></a><span data-ttu-id="621fd-120">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="621fd-120">See also</span></span>
<span data-ttu-id="621fd-121">Ez a cikk bemutatta megvalósításához a Security Center ajánlás "Engedélyezés átlátható adattitkosítási."</span><span class="sxs-lookup"><span data-stu-id="621fd-121">This article showed you how to implement the Security Center recommendation "Enable Transparent Data Encryption."</span></span> <span data-ttu-id="621fd-122">Erőforráscsoportoknál kapcsolatos további tudnivalókért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="621fd-122">To learn more about SQL TDE, see the following:</span></span>

* [<span data-ttu-id="621fd-123">Az Azure SQL Database átlátható adattitkosítás</span><span class="sxs-lookup"><span data-stu-id="621fd-123">Transparent Data Encryption with Azure SQL Database</span></span>](https://msdn.microsoft.com/library/dn948096)
* [<span data-ttu-id="621fd-124">Ismerkedés a transzparens adatok titkosítás (TDE)</span><span class="sxs-lookup"><span data-stu-id="621fd-124">Get started with Transparent Data Encryption (TDE)</span></span>](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

<span data-ttu-id="621fd-125">A Security Centerrel kapcsolatos további információkért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="621fd-125">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="621fd-126">[Biztonsági szabályzatok beállítása az Azure Security Centerben](security-center-policies.md) – Ez a cikk bemutatja, hogyan konfigurálhat biztonsági házirendeket Azure-előfizetései és -erőforráscsoportjai számára.</span><span class="sxs-lookup"><span data-stu-id="621fd-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="621fd-127">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="621fd-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="621fd-128">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – útmutató az Azure-erőforrások állapotának figyelésére.</span><span class="sxs-lookup"><span data-stu-id="621fd-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="621fd-129">[Biztonsági riasztások kezelése és válaszadás a riasztásokra az Azure Security Centerben](security-center-managing-and-responding-alerts.md) – A biztonsági riasztások kezelése és az azokra való reagálás.</span><span class="sxs-lookup"><span data-stu-id="621fd-129">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="621fd-130">[Partneri megoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md) – Útmutató a partneri megoldások biztonsági állapotának monitorozásához.</span><span class="sxs-lookup"><span data-stu-id="621fd-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="621fd-131">[Azure Security Center – gyakran ismételt kérdések](security-center-faq.md) – Gyakran ismételt kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="621fd-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="621fd-132">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) --az Azure biztonsági legfrissebb hírek és információ.</span><span class="sxs-lookup"><span data-stu-id="621fd-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
