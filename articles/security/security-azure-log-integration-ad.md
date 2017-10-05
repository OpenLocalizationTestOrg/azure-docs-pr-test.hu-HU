---
title: "Vizsgálati naplóit Azure Active Directory integrálása az Azure napló |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítse az Azure napló integrációs szolgáltatást, és integrálhatja a naplók az Azure felügyeleti naplók"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/08/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 8a1295cc86057ed72940e774d0bd423d61142e31
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="integrate-azure-active-directory-audit-logs"></a><span data-ttu-id="2e0a4-103">Integrálása az Azure Active Directory-naplók</span><span class="sxs-lookup"><span data-stu-id="2e0a4-103">Integrate Azure Active Directory audit logs</span></span>

<span data-ttu-id="2e0a4-104">Az Azure Active Directory (Azure AD) naplózási események segítségével azonosíthatja a privilegizált műveletekhez, hogy megtörtént az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-104">Azure Active Directory (Azure AD) audit events help you identify privileged actions that occurred in Azure Active Directory.</span></span> <span data-ttu-id="2e0a4-105">Láthatja, hogy milyen típusú eseményeket vizsgálatával nyomon követett [Azure Active Directory auditnaplójának jelentési eseményei](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).</span><span class="sxs-lookup"><span data-stu-id="2e0a4-105">You can see the types of events that you can track by reviewing [Azure Active Directory audit report events](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2e0a4-106">A cikkben ismertetett-megkezdése előtt át kell néznie a [Ismerkedés](security-azure-log-integration-get-started.md) cikk és a lépéseket van.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-106">Before you attempt the steps in this article, you must review the [Get started](security-azure-log-integration-get-started.md) article and complete the steps there.</span></span>

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a><span data-ttu-id="2e0a4-107">Azure Active directory integrálásának lépései naplók</span><span class="sxs-lookup"><span data-stu-id="2e0a4-107">Steps to integrate Azure Active directory audit logs</span></span>

1. <span data-ttu-id="2e0a4-108">Nyissa meg a parancssort, és futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="2e0a4-108">Open the command prompt and run this command:</span></span>

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. <span data-ttu-id="2e0a4-109">Futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="2e0a4-109">Run this command:</span></span> 
 
   ``azlog createazureid``

   <span data-ttu-id="2e0a4-110">Ez a parancs kéri az Azure bejelentkezési.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-110">This command prompts you for your Azure login.</span></span> <span data-ttu-id="2e0a4-111">A parancs majd hoz létre egy Azure Active Directory szolgáltatás egyszerű az Azure AD-bérlőkkel, amely az Azure-előfizetéseket, amelyben a bejelentkezett felhasználó nem rendszergazda, a társadminisztrátorának vagy a tulajdonos tárolni.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-111">The command then creates an Azure Active Directory service principal in the Azure AD tenants that host the Azure subscriptions in which the logged-in user is an administrator, a co-administrator, or an owner.</span></span> <span data-ttu-id="2e0a4-112">A parancs sikertelen lesz, ha a bejelentkezett felhasználó csak a Vendég felhasználó az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-112">The command will fail if the logged-in user is only a guest user in the Azure AD tenant.</span></span> <span data-ttu-id="2e0a4-113">Hitelesítés az Azure-bA az Azure AD segítségével történik.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-113">Authentication to Azure is done through Azure AD.</span></span> <span data-ttu-id="2e0a4-114">Az Azure AD identity arra, hogy az Azure-előfizetések olvasási hozzáférést egy egyszerű Azure napló integrációs szolgáltatás létrehozásakor létrejön.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-114">Creating a service principal for Azure Log Integration creates the Azure AD identity that is given access to read from Azure subscriptions.</span></span>

3. <span data-ttu-id="2e0a4-115">A következő parancsot adja meg a bérlő azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-115">Run the following command to provide your tenant ID.</span></span> <span data-ttu-id="2e0a4-116">A parancs futtatásához a bérlői rendszergazda szerepkör tagjának lennie kell.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-116">You need to be member of the tenant admin role to run the command.</span></span>

   ``Azlog.exe authorizedirectoryreader tenantId``

   <span data-ttu-id="2e0a4-117">Példa:</span><span class="sxs-lookup"><span data-stu-id="2e0a4-117">Example:</span></span>

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. <span data-ttu-id="2e0a4-118">Ellenőrizze a következő mappák győződjön meg arról, hogy az Azure Active Directory JSON naplófájlok bennük jönnek létre:</span><span class="sxs-lookup"><span data-stu-id="2e0a4-118">Check the following folders to confirm that the Azure Active Directory audit log JSON files are created in them:</span></span>

   * <span data-ttu-id="2e0a4-119">**C:\Users\azlog\AzureActiveDirectoryJson**</span><span class="sxs-lookup"><span data-stu-id="2e0a4-119">**C:\Users\azlog\AzureActiveDirectoryJson**</span></span>
   * <span data-ttu-id="2e0a4-120">**C:\Users\azlog\AzureActiveDirectoryJsonLD**</span><span class="sxs-lookup"><span data-stu-id="2e0a4-120">**C:\Users\azlog\AzureActiveDirectoryJsonLD**</span></span>

<span data-ttu-id="2e0a4-121">A következő videó bemutatja a cikkben szereplő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2e0a4-121">The following video demonstrates the steps covered in this article:</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> <span data-ttu-id="2e0a4-122">A JSON-fájlokban szereplő információk üzembe a biztonsági információk és eseménykezelő rendszer (SIEM) kapcsolatos tudnivalókat forduljon a SIEM gyártójához.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-122">For specific instructions on bringing the information in the JSON files into your security information and event management (SIEM) system, contact your SIEM vendor.</span></span>

<span data-ttu-id="2e0a4-123">Közösségi támogatás érhető el a [Azure napló integrációs MSDN fórumon](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration).</span><span class="sxs-lookup"><span data-stu-id="2e0a4-123">Community assistance is available through the [Azure Log Integration MSDN Forum](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration).</span></span> <span data-ttu-id="2e0a4-124">Ezen a fórumon lehetővé teszi, hogy a személyek Azure napló integráció közösségi egymással kérdéseket, válaszokat, tippeket és trükkök támogatásához.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-124">This forum enables people in the Azure Log Integration community to support each other with questions, answers, tips, and tricks.</span></span> <span data-ttu-id="2e0a4-125">Ezenkívül az Azure napló integrációs csoport ezen a fórumon figyeli, és amikor azt is segít.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-125">In addition, the Azure Log Integration team monitors this forum and helps whenever it can.</span></span>

<span data-ttu-id="2e0a4-126">Is megnyithatja a [támogatási kérelem](../azure-supportability/how-to-create-azure-support-request.md).</span><span class="sxs-lookup"><span data-stu-id="2e0a4-126">You can also open a [support request](../azure-supportability/how-to-create-azure-support-request.md).</span></span> <span data-ttu-id="2e0a4-127">Válassza ki **napló integrációs** a szolgáltatást, amelynek támogatási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-127">Select **Log Integration** as the service for which you are requesting support.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e0a4-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e0a4-128">Next steps</span></span>
<span data-ttu-id="2e0a4-129">Azure napló integrációs kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="2e0a4-129">To learn more about Azure Log Integration, see:</span></span>

* <span data-ttu-id="2e0a4-130">[A Microsoft Azure napló integráció az Azure-naplók](https://www.microsoft.com/download/details.aspx?id=53324): A letöltőközpontból weblap, amely részleteit, rendszerkövetelmények és telepítési utasításokat az Azure napló integráció.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-130">[Microsoft Azure Log Integration for Azure logs](https://www.microsoft.com/download/details.aspx?id=53324): This Download Center page gives details, system requirements, and installation instructions for Azure Log Integration.</span></span>
* <span data-ttu-id="2e0a4-131">[Bevezetés az Azure napló integrációs](security-azure-log-integration-overview.md): Ez a cikk bemutatja a Azure napló integrációs, annak főbb funkcióit és annak működéséről.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-131">[Introduction to Azure Log Integration](security-azure-log-integration-overview.md): This article introduces you to Azure Log Integration, its key capabilities, and how it works.</span></span>
* <span data-ttu-id="2e0a4-132">[Partner konfigurációs lépések](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Ebben a blogbejegyzésben bemutatja, hogyan használható Splunk, HP ArcSight és az IBM QRadar partneri megoldások Azure napló-integráció konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-132">[Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): This blog post shows you how to configure Azure Log Integration to work with partner solutions Splunk, HP ArcSight, and IBM QRadar.</span></span>
* <span data-ttu-id="2e0a4-133">[Az Azure napló integrációs gyakran ismételt kérdések](security-azure-log-integration-faq.md): Ez a cikk Azure napló integrációs kapcsolatos kérdésekre ad választ.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-133">[Azure Log Integration FAQ](security-azure-log-integration-faq.md): This article answers questions about Azure Log Integration.</span></span>
* <span data-ttu-id="2e0a4-134">[A Security Center riasztásait integrálása Azure napló integrációs](../security-center/security-center-integrating-alerts-with-log-integration.md): Ez a cikk bemutatja, hogyan szinkronizálása a Security Center riasztásait, együtt a virtuális gép biztonsági események által gyűjtött Azure Diagnostics és az Azure naplózási naplókat, a naplóelemzési vagy a SIEM-megoldás.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-134">[Integrating Security Center alerts with Azure Log Integration](../security-center/security-center-integrating-alerts-with-log-integration.md): This article shows you how to sync Security Center alerts, along with virtual machine security events collected by Azure Diagnostics and Azure audit logs, with your log analytics or SIEM solution.</span></span>
* <span data-ttu-id="2e0a4-135">[Az Azure Diagnostics és az Azure új szolgáltatásai naplók](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): Ebben a blogbejegyzésben bemutatja Azure vizsgálati naplók és egyéb szolgáltatásokat, amelyek segítenek betekintést nyerhet az Azure-erőforrások működésére.</span><span class="sxs-lookup"><span data-stu-id="2e0a4-135">[New features for Azure Diagnostics and Azure audit logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): This blog post introduces you to Azure audit logs and other features that help you gain insights into the operations of your Azure resources.</span></span>
