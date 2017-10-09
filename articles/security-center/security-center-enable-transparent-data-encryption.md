---
title: "aaaEnable átlátható adattitkosítást az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** engedélyezése átlátszó adatok titkosítás **."
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
ms.openlocfilehash: 94c6e9a1feddaa48faac6c835d416c4d131cd5c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a><span data-ttu-id="5082f-103">Engedélyezze az átlátható adattitkosítást az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="5082f-103">Enable Transparent Data Encryption in Azure Security Center</span></span>
<span data-ttu-id="5082f-104">Azure Security Center javasolni fogja engedélyezése átlátszó Data Encryption (TDE) az SQL-adatbázisok, ha a TDE nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="5082f-104">Azure Security Center will recommend that you enable Transparent Data Encryption (TDE) on SQL databases if TDE is not already enabled.</span></span> <span data-ttu-id="5082f-105">TDE védi az adatokat, és segítséget nyújt a megfelelőségi követelményeknek a adatbázis, a társított biztonsági másolatok és a tranzakciós naplófájlok nyugalmi, anélkül, hogy a módosítások tooyour alkalmazás titkosításával.</span><span class="sxs-lookup"><span data-stu-id="5082f-105">TDE protects your data and helps you meet compliance requirements by encrypting your database, associated backups, and transaction log files at rest, without requiring changes tooyour application.</span></span> <span data-ttu-id="5082f-106">több lásd toolearn [átlátható adattitkosítást az Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span><span class="sxs-lookup"><span data-stu-id="5082f-106">toolearn more see [Transparent Data Encryption with Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span></span>

<span data-ttu-id="5082f-107">Ez a javaslat alkalmazása toohello Azure SQL-szolgáltatás csak; a virtuális gépeken futó SQL nem tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5082f-107">This recommendation applies toohello Azure SQL service only; doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="5082f-108">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="5082f-108">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="5082f-109">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="5082f-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="5082f-110">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="5082f-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="5082f-111">A hello **javaslatok** panelen válassza **átlátható adattitkosítási engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="5082f-111">In hello **Recommendations** blade, select **Enable Transparent Data Encryption**.</span></span>
   <span data-ttu-id="5082f-112">![Transzparens adattitkosítás engedélyezése][1]</span><span class="sxs-lookup"><span data-stu-id="5082f-112">![Enable Transparent Data Encryption][1]</span></span>
2. <span data-ttu-id="5082f-113">Ekkor megnyílik a hello **átlátható adattitkosítási engedélyezése az SQL-adatbázisok** panelen.</span><span class="sxs-lookup"><span data-stu-id="5082f-113">This opens hello **Enable Transparent Data Encryption on SQL databases** blade.</span></span> <span data-ttu-id="5082f-114">Jelölje be egy SQL-adatbázis tooenable TDE.</span><span class="sxs-lookup"><span data-stu-id="5082f-114">Select a SQL database tooenable TDE on.</span></span>
   <span data-ttu-id="5082f-115">![Az SQL DB tooenable TDE kiválasztása][2]</span><span class="sxs-lookup"><span data-stu-id="5082f-115">![Select SQL DB tooenable TDE on][2]</span></span>
3. <span data-ttu-id="5082f-116">A hello **átlátható adattitkosítás** panelen válassza **ON** adatok titkosítását, és válassza a **mentése** hello felső szalagon hello panelről.</span><span class="sxs-lookup"><span data-stu-id="5082f-116">On hello **Transparent data encryption** blade, select **ON** under Data encryption and select **Save** in hello top ribbon of hello blade.</span></span>
   <span data-ttu-id="5082f-117">![Kapcsolja be a TDE][3]</span><span class="sxs-lookup"><span data-stu-id="5082f-117">![Turn on TDE][3]</span></span>

   <span data-ttu-id="5082f-118">Ha a TDE engedélyezve van a hello kijelölt SQL-adatbázis hello **titkosítási állapotát** túl változik**titkosított**.</span><span class="sxs-lookup"><span data-stu-id="5082f-118">Once TDE is enabled on hello selected SQL database, hello **Encryption status** will change too**Encrypted**.</span></span>    

   ![A titkosítás állapota][4]

## <a name="see-also"></a><span data-ttu-id="5082f-120">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5082f-120">See also</span></span>
<span data-ttu-id="5082f-121">Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "Enable átlátható adattitkosítási."</span><span class="sxs-lookup"><span data-stu-id="5082f-121">This article showed you how tooimplement hello Security Center recommendation "Enable Transparent Data Encryption."</span></span> <span data-ttu-id="5082f-122">toolearn Erőforráscsoportoknál, kapcsolatos további információkért tekintse meg a hello következőt:</span><span class="sxs-lookup"><span data-stu-id="5082f-122">toolearn more about SQL TDE, see hello following:</span></span>

* [<span data-ttu-id="5082f-123">Az Azure SQL Database átlátható adattitkosítás</span><span class="sxs-lookup"><span data-stu-id="5082f-123">Transparent Data Encryption with Azure SQL Database</span></span>](https://msdn.microsoft.com/library/dn948096)
* [<span data-ttu-id="5082f-124">Ismerkedés a transzparens adatok titkosítás (TDE)</span><span class="sxs-lookup"><span data-stu-id="5082f-124">Get started with Transparent Data Encryption (TDE)</span></span>](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

<span data-ttu-id="5082f-125">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="5082f-125">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="5082f-126">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="5082f-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="5082f-127">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="5082f-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="5082f-128">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="5082f-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="5082f-129">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="5082f-129">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="5082f-130">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="5082f-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="5082f-131">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5082f-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="5082f-132">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="5082f-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
