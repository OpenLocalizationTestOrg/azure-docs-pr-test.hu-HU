---
title: "Azure Active Directory-naplók napló integrációja aaaAzure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall hello Azure napló integrációs szolgáltatás, és integrálhatja a naplók az Azure felügyeleti naplók"
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
ms.openlocfilehash: 3ee8fa3b8b5e9bd33202e57ed5327cd8d3127f00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-active-directory-audit-logs"></a><span data-ttu-id="9fd01-103">Integrálása az Azure Active Directory-naplók</span><span class="sxs-lookup"><span data-stu-id="9fd01-103">Integrate Azure Active Directory audit logs</span></span>

<span data-ttu-id="9fd01-104">Az Azure Active Directory (Azure AD) naplózási események segítségével azonosíthatja a privilegizált műveletekhez, hogy megtörtént az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="9fd01-104">Azure Active Directory (Azure AD) audit events help you identify privileged actions that occurred in Azure Active Directory.</span></span> <span data-ttu-id="9fd01-105">Megtekintheti a hello típusú események vizsgálatával nyomon követett [Azure Active Directory auditnaplójának jelentési eseményei](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).</span><span class="sxs-lookup"><span data-stu-id="9fd01-105">You can see hello types of events that you can track by reviewing [Azure Active Directory audit report events](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9fd01-106">A cikkben ismertetett hello-megkezdése előtt át kell néznie hello [Ismerkedés](security-azure-log-integration-get-started.md) és annak lépésekkel hello van.</span><span class="sxs-lookup"><span data-stu-id="9fd01-106">Before you attempt hello steps in this article, you must review hello [Get started](security-azure-log-integration-get-started.md) article and complete hello steps there.</span></span>

## <a name="steps-toointegrate-azure-active-directory-audit-logs"></a><span data-ttu-id="9fd01-107">A naplók lépéseket toointegrate Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="9fd01-107">Steps toointegrate Azure Active directory audit logs</span></span>

1. <span data-ttu-id="9fd01-108">Nyissa meg a hello parancssort, és futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="9fd01-108">Open hello command prompt and run this command:</span></span>

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. <span data-ttu-id="9fd01-109">Futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="9fd01-109">Run this command:</span></span> 
 
   ``azlog createazureid``

   <span data-ttu-id="9fd01-110">Ez a parancs kéri az Azure bejelentkezési.</span><span class="sxs-lookup"><span data-stu-id="9fd01-110">This command prompts you for your Azure login.</span></span> <span data-ttu-id="9fd01-111">hello parancs majd hoz létre egy Azure Active Directory szolgáltatás egyszerű hello Azure AD bérlők üzemeltető hello Azure-előfizetéseket, mely hello bejelentkezett felhasználó, a rendszergazda, a társadminisztrátorának vagy a tulajdonos.</span><span class="sxs-lookup"><span data-stu-id="9fd01-111">hello command then creates an Azure Active Directory service principal in hello Azure AD tenants that host hello Azure subscriptions in which hello logged-in user is an administrator, a co-administrator, or an owner.</span></span> <span data-ttu-id="9fd01-112">hello parancs sikertelen lesz, ha hello bejelentkezett felhasználó csak a Vendég felhasználó hello Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="9fd01-112">hello command will fail if hello logged-in user is only a guest user in hello Azure AD tenant.</span></span> <span data-ttu-id="9fd01-113">Hitelesítési tooAzure az Azure AD segítségével történik.</span><span class="sxs-lookup"><span data-stu-id="9fd01-113">Authentication tooAzure is done through Azure AD.</span></span> <span data-ttu-id="9fd01-114">Hello arra, hogy hozzáférési tooread az Azure-előfizetések az Azure AD identity egy egyszerű Azure napló integrációs szolgáltatás létrehozásakor létrejön.</span><span class="sxs-lookup"><span data-stu-id="9fd01-114">Creating a service principal for Azure Log Integration creates hello Azure AD identity that is given access tooread from Azure subscriptions.</span></span>

3. <span data-ttu-id="9fd01-115">Futtassa a következő parancs tooprovide hello a bérlő azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="9fd01-115">Run hello following command tooprovide your tenant ID.</span></span> <span data-ttu-id="9fd01-116">Hello Bérlői rendszergazda szerepkör toorun hello parancs toobe tagja van szüksége.</span><span class="sxs-lookup"><span data-stu-id="9fd01-116">You need toobe member of hello tenant admin role toorun hello command.</span></span>

   ``Azlog.exe authorizedirectoryreader tenantId``

   <span data-ttu-id="9fd01-117">Példa:</span><span class="sxs-lookup"><span data-stu-id="9fd01-117">Example:</span></span>

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. <span data-ttu-id="9fd01-118">Ellenőrizze, hogy a következő mappák tooconfirm, amely Azure Active Directory JSON naplófájlok hello hello jönnek létre őket:</span><span class="sxs-lookup"><span data-stu-id="9fd01-118">Check hello following folders tooconfirm that hello Azure Active Directory audit log JSON files are created in them:</span></span>

   * <span data-ttu-id="9fd01-119">**C:\Users\azlog\AzureActiveDirectoryJson**</span><span class="sxs-lookup"><span data-stu-id="9fd01-119">**C:\Users\azlog\AzureActiveDirectoryJson**</span></span>
   * <span data-ttu-id="9fd01-120">**C:\Users\azlog\AzureActiveDirectoryJsonLD**</span><span class="sxs-lookup"><span data-stu-id="9fd01-120">**C:\Users\azlog\AzureActiveDirectoryJsonLD**</span></span>

<span data-ttu-id="9fd01-121">hello következő videó bemutatja a cikkben szereplő hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9fd01-121">hello following video demonstrates hello steps covered in this article:</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> <span data-ttu-id="9fd01-122">A JSON-fájlok hello hello információk üzembe a biztonsági információk és eseménykezelő rendszer (SIEM) kapcsolatos tudnivalókat forduljon a SIEM gyártójához.</span><span class="sxs-lookup"><span data-stu-id="9fd01-122">For specific instructions on bringing hello information in hello JSON files into your security information and event management (SIEM) system, contact your SIEM vendor.</span></span>

<span data-ttu-id="9fd01-123">Közösségi támogatás érhető el hello [Azure napló integrációs MSDN fórumon](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration).</span><span class="sxs-lookup"><span data-stu-id="9fd01-123">Community assistance is available through hello [Azure Log Integration MSDN Forum](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration).</span></span> <span data-ttu-id="9fd01-124">Ezen a fórumon lehetővé teszi, hogy munkatársai hello Azure napló integráció közösségi toosupport egymással kérdéseket, válaszokat, tippeket és trükkök.</span><span class="sxs-lookup"><span data-stu-id="9fd01-124">This forum enables people in hello Azure Log Integration community toosupport each other with questions, answers, tips, and tricks.</span></span> <span data-ttu-id="9fd01-125">Ezenkívül hello Azure napló integrációs csoport ezen a fórumon figyeli, és amikor azt is segít.</span><span class="sxs-lookup"><span data-stu-id="9fd01-125">In addition, hello Azure Log Integration team monitors this forum and helps whenever it can.</span></span>

<span data-ttu-id="9fd01-126">Is megnyithatja a [támogatási kérelem](../azure-supportability/how-to-create-azure-support-request.md).</span><span class="sxs-lookup"><span data-stu-id="9fd01-126">You can also open a [support request](../azure-supportability/how-to-create-azure-support-request.md).</span></span> <span data-ttu-id="9fd01-127">Válassza ki **napló integrációs** hello szolgáltatást, amelynek támogatási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="9fd01-127">Select **Log Integration** as hello service for which you are requesting support.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9fd01-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9fd01-128">Next steps</span></span>
<span data-ttu-id="9fd01-129">toolearn Azure napló-nal kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="9fd01-129">toolearn more about Azure Log Integration, see:</span></span>

* <span data-ttu-id="9fd01-130">[A Microsoft Azure napló integráció az Azure-naplók](https://www.microsoft.com/download/details.aspx?id=53324): A letöltőközpontból weblap, amely részleteit, rendszerkövetelmények és telepítési utasításokat az Azure napló integráció.</span><span class="sxs-lookup"><span data-stu-id="9fd01-130">[Microsoft Azure Log Integration for Azure logs](https://www.microsoft.com/download/details.aspx?id=53324): This Download Center page gives details, system requirements, and installation instructions for Azure Log Integration.</span></span>
* <span data-ttu-id="9fd01-131">[Bevezetés tooAzure napló integrációs](security-azure-log-integration-overview.md): Ez a cikk bemutatja a napló integrációs tooAzure, annak főbb funkcióit és annak működéséről.</span><span class="sxs-lookup"><span data-stu-id="9fd01-131">[Introduction tooAzure Log Integration](security-azure-log-integration-overview.md): This article introduces you tooAzure Log Integration, its key capabilities, and how it works.</span></span>
* <span data-ttu-id="9fd01-132">[Partner konfigurációs lépések](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Ebben a blogbejegyzésben bemutatja, hogyan tooconfigure Azure napló integrációs toowork a partnermegoldások Splunk, HP ArcSight, és az IBM QRadar.</span><span class="sxs-lookup"><span data-stu-id="9fd01-132">[Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): This blog post shows you how tooconfigure Azure Log Integration toowork with partner solutions Splunk, HP ArcSight, and IBM QRadar.</span></span>
* <span data-ttu-id="9fd01-133">[Az Azure napló integrációs gyakran ismételt kérdések](security-azure-log-integration-faq.md): Ez a cikk Azure napló integrációs kapcsolatos kérdésekre ad választ.</span><span class="sxs-lookup"><span data-stu-id="9fd01-133">[Azure Log Integration FAQ](security-azure-log-integration-faq.md): This article answers questions about Azure Log Integration.</span></span>
* <span data-ttu-id="9fd01-134">[A Security Center riasztásait integrálása Azure napló integrációs](../security-center/security-center-integrating-alerts-with-log-integration.md): Ez a cikk bemutatja, hogyan toosync Security Center riasztást küld, és a virtuális gép biztonsági események Azure Diagnostics és az Azure naplókat, és a naplóelemzési által gyűjtött vagy SIEM-megoldás.</span><span class="sxs-lookup"><span data-stu-id="9fd01-134">[Integrating Security Center alerts with Azure Log Integration](../security-center/security-center-integrating-alerts-with-log-integration.md): This article shows you how toosync Security Center alerts, along with virtual machine security events collected by Azure Diagnostics and Azure audit logs, with your log analytics or SIEM solution.</span></span>
* <span data-ttu-id="9fd01-135">[Az Azure Diagnostics és az Azure új szolgáltatásai naplók](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): Ebben a blogbejegyzésben bemutatja tooAzure naplókat, és egyéb szolgáltatásokat, amelyek segítenek betekintést nyerhet az Azure-erőforrások hello működésére.</span><span class="sxs-lookup"><span data-stu-id="9fd01-135">[New features for Azure Diagnostics and Azure audit logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): This blog post introduces you tooAzure audit logs and other features that help you gain insights into hello operations of your Azure resources.</span></span>
