---
title: "aaaWhat az hello Azure RemoteApp sablon rendszerképei? | Microsoft Docs"
description: "Tudnivalók az Azure Remoteapphez mellékelt hello sablonrendszerképek."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7f8442b2-81da-421e-a453-aa53ba2066b7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ea012cec8dc581a8bd4a5a138ce302de19d5c6af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-in-hello-azure-remoteapp-template-images"></a><span data-ttu-id="737ee-104">Mi az az hello Azure RemoteApp sablon rendszerképei?</span><span class="sxs-lookup"><span data-stu-id="737ee-104">What is in hello Azure RemoteApp template images?</span></span>
> [!IMPORTANT]
> <span data-ttu-id="737ee-105">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="737ee-105">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="737ee-106">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="737ee-106">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="737ee-107">Az Azure RemoteApp-előfizetés három sablon rendszerképet tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="737ee-107">Your Azure RemoteApp subscription includes three template images:</span></span>

* <span data-ttu-id="737ee-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="737ee-108">Windows Server 2012</span></span>
* <span data-ttu-id="737ee-109">Microsoft Office 365 ProPlus (a használatához Office 365-előfizetés szükséges)</span><span class="sxs-lookup"><span data-stu-id="737ee-109">Microsoft Office 365 ProPlus (Office 365 subscription required)</span></span>
* <span data-ttu-id="737ee-110">Microsoft Office 2013 Professional Plus (csak próbaverzió)</span><span class="sxs-lookup"><span data-stu-id="737ee-110">Microsoft Office 2013 Professional Plus (trial only)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="737ee-111">Az Azure RemoteApp előfizetés hozzáférést toohello szoftver hello képek hello kivétellel Office 365 ProPlus, amelyhez különálló előfizetés szükséges, és Office 2013, amely a termelésben nem használható.</span><span class="sxs-lookup"><span data-stu-id="737ee-111">Your Azure RemoteApp subscription grants you access toohello software in hello images, with hello exception of Office 365 ProPlus, which requires a separate subscription, and Office 2013, which cannot be used in production.</span></span> <span data-ttu-id="737ee-112">Ez azt jelenti, hogy megoszthatja lévő hello sablon rendszerképek hello programokat és alkalmazásokat a felhasználóival.</span><span class="sxs-lookup"><span data-stu-id="737ee-112">This means that you can share hello programs or apps on hello template images with your users.</span></span> <span data-ttu-id="737ee-113">Például létrehozhat egy gyűjteményt, amely hello Windows Server 2012 R2 rendszerképet használja, ha közzéteheti System Center Endpoint Protection a tooaccess felhasználókat a Remoteappen keresztül.</span><span class="sxs-lookup"><span data-stu-id="737ee-113">For example, if you create a collection that uses hello Windows Server 2012 R2 image, you can publish System Center Endpoint Protection for users tooaccess through RemoteApp.</span></span>
> 
> <span data-ttu-id="737ee-114">Tekintse meg a hello [RemoteApp licensing részletek](remoteapp-licensing.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="737ee-114">Check out hello [RemoteApp licensing details](remoteapp-licensing.md) for more information.</span></span> <span data-ttu-id="737ee-115">És [Office használata az Azure RemoteApp](remoteapp-o365.md) a hello Office licencelési információ.</span><span class="sxs-lookup"><span data-stu-id="737ee-115">And [Using Office with Azure RemoteApp](remoteapp-o365.md) for hello Office licensing info.</span></span>
> 
> 

<span data-ttu-id="737ee-116">Az egyes rendszerképek tartalmának részleteiért olvasson tovább.</span><span class="sxs-lookup"><span data-stu-id="737ee-116">Read on for details on what each image contains.</span></span>

## <a name="windows-server-2012-r2--hello-vanilla-image"></a><span data-ttu-id="737ee-117">Windows Server 2012 R2 ("hello eredeti rendszerkép")</span><span class="sxs-lookup"><span data-stu-id="737ee-117">Windows Server 2012 R2  ("hello vanilla image")</span></span>
<span data-ttu-id="737ee-118">Ez a rendszerkép a Microsoft Windows Server 2012 R2 Datacenter operációs rendszeren alapul, és hello következő szerepköröket és szolgáltatásokat telepítette toomeet hello Azure RemoteApp sablon rendszerképek követelményeinek:</span><span class="sxs-lookup"><span data-stu-id="737ee-118">This image is based on Microsoft Windows Server 2012 R2 Datacenter operating system and has hello following roles and features installed toomeet hello requirements for Azure RemoteApp template images:</span></span>

* <span data-ttu-id="737ee-119">.NET-keretrendszer 4.5, 3.5.1, 3.5</span><span class="sxs-lookup"><span data-stu-id="737ee-119">.NET Framework 4.5, 3.5.1, 3.5</span></span>
* <span data-ttu-id="737ee-120">Asztali élmény</span><span class="sxs-lookup"><span data-stu-id="737ee-120">Desktop Experience</span></span>
* <span data-ttu-id="737ee-121">Szabadkézi beviteli és kézírás-felismerő szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="737ee-121">Ink and Handwriting Services</span></span>
* <span data-ttu-id="737ee-122">Multimédia alaprendszer</span><span class="sxs-lookup"><span data-stu-id="737ee-122">Media Foundation</span></span>
* <span data-ttu-id="737ee-123">Távoli asztali munkamenetgazda</span><span class="sxs-lookup"><span data-stu-id="737ee-123">Remote Desktop Session Host</span></span>
* <span data-ttu-id="737ee-124">Windows PowerShell 4.0</span><span class="sxs-lookup"><span data-stu-id="737ee-124">Windows PowerShell 4.0</span></span>
* <span data-ttu-id="737ee-125">Windows PowerShell integrált parancsprogram-kezelési környezet (ISE)</span><span class="sxs-lookup"><span data-stu-id="737ee-125">Windows PowerShell ISE</span></span>
* <span data-ttu-id="737ee-126">WoW64-támogatás</span><span class="sxs-lookup"><span data-stu-id="737ee-126">WoW64 Support</span></span>

<span data-ttu-id="737ee-127">Ez a rendszerkép is rendelkezik a következő telepített alkalmazások hello:</span><span class="sxs-lookup"><span data-stu-id="737ee-127">This image also has hello following applications installed:</span></span>

* <span data-ttu-id="737ee-128">Adobe Flash Player</span><span class="sxs-lookup"><span data-stu-id="737ee-128">Adobe Flash Player</span></span>
* <span data-ttu-id="737ee-129">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="737ee-129">Microsoft Silverlight</span></span>
* <span data-ttu-id="737ee-130">Microsoft System Center 2012 Endpoint Protection</span><span class="sxs-lookup"><span data-stu-id="737ee-130">Microsoft System Center 2012 Endpoint Protection</span></span>
* <span data-ttu-id="737ee-131">Microsoft Windows Media Player</span><span class="sxs-lookup"><span data-stu-id="737ee-131">Microsoft Windows Media Player</span></span>

## <a name="microsoft-office-365-proplus-subscription-required"></a><span data-ttu-id="737ee-132">Microsoft Office 365 ProPlus (a használatához előfizetés szükséges)</span><span class="sxs-lookup"><span data-stu-id="737ee-132">Microsoft Office 365 ProPlus (subscription required)</span></span>
<span data-ttu-id="737ee-133">Az Office 365 van hello leginkább kért alkalmazás, ezért létrehoztunk egy "egyéni" rendszerképet, a toowork.</span><span class="sxs-lookup"><span data-stu-id="737ee-133">Office 365 is hello most requested application, so we created a "custom" image for you toowork with.</span></span>

<span data-ttu-id="737ee-134">Ez a rendszerkép hello eredeti rendszerkép bővített verziója, és hello Microsoft Office 365 ProPlus következő összetevői telepítve van ezen kívül hello Windows Server 2012 R2 rendszerképnél ismertetett toohello összetevők:</span><span class="sxs-lookup"><span data-stu-id="737ee-134">This image is an extension of hello vanilla image and has hello following components of Microsoft Office 365 ProPlus installed in addition toohello components described in hello Windows Server 2012 R2 image:</span></span>

* <span data-ttu-id="737ee-135">Hozzáférés</span><span class="sxs-lookup"><span data-stu-id="737ee-135">Access</span></span>
* <span data-ttu-id="737ee-136">Excel</span><span class="sxs-lookup"><span data-stu-id="737ee-136">Excel</span></span>
* <span data-ttu-id="737ee-137">Lync</span><span class="sxs-lookup"><span data-stu-id="737ee-137">Lync</span></span>
* <span data-ttu-id="737ee-138">OneNote</span><span class="sxs-lookup"><span data-stu-id="737ee-138">OneNote</span></span>
* <span data-ttu-id="737ee-139">Onedrive vállalati verzió (vegye figyelembe, hogy hello sync-ügynök nem támogatott az Azure RemoteApp használata)</span><span class="sxs-lookup"><span data-stu-id="737ee-139">OneDrive for Business (note that hello sync agent is not supported for use with Azure RemoteApp)</span></span>
* <span data-ttu-id="737ee-140">Outlook</span><span class="sxs-lookup"><span data-stu-id="737ee-140">Outlook</span></span>
* <span data-ttu-id="737ee-141">PowerPoint</span><span class="sxs-lookup"><span data-stu-id="737ee-141">PowerPoint</span></span>
* <span data-ttu-id="737ee-142">Word</span><span class="sxs-lookup"><span data-stu-id="737ee-142">Word</span></span>
* <span data-ttu-id="737ee-143">Microsoft Office Nyelvi ellenőrző eszközök</span><span class="sxs-lookup"><span data-stu-id="737ee-143">Microsoft Office Proofing Tools</span></span>

<span data-ttu-id="737ee-144">hello lemezképet is tartalmaz, a Visio Pro és a Project Pro alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="737ee-144">hello image also includes Visio Pro and Project Pro.</span></span>

<span data-ttu-id="737ee-145">És alkalmazások, valamint a következő hello:</span><span class="sxs-lookup"><span data-stu-id="737ee-145">And hello following applications, as well:</span></span>

* <span data-ttu-id="737ee-146">SQL Native Client</span><span class="sxs-lookup"><span data-stu-id="737ee-146">SQL Native client</span></span>
* <span data-ttu-id="737ee-147">ODBC-illesztő</span><span class="sxs-lookup"><span data-stu-id="737ee-147">ODBC Driver</span></span>
* <span data-ttu-id="737ee-148">SQL Server Data Mining-ügyfél</span><span class="sxs-lookup"><span data-stu-id="737ee-148">SQL Server Data Mining client</span></span>
* <span data-ttu-id="737ee-149">MasterDataServices-ügyfél</span><span class="sxs-lookup"><span data-stu-id="737ee-149">MasterDataServices client</span></span>
* <span data-ttu-id="737ee-150">Microsoft Publisher</span><span class="sxs-lookup"><span data-stu-id="737ee-150">Microsoft Publisher</span></span>
* <span data-ttu-id="737ee-151">PowerQuery</span><span class="sxs-lookup"><span data-stu-id="737ee-151">PowerQuery</span></span>
* <span data-ttu-id="737ee-152">PowerMap</span><span class="sxs-lookup"><span data-stu-id="737ee-152">PowerMap</span></span>

<span data-ttu-id="737ee-153">Az Office 365 ProPlus-alkalmazások összes funkciója csak az Office 365 ProPlus-előfizetéssel rendelkező felhasználók számára érhető el.</span><span class="sxs-lookup"><span data-stu-id="737ee-153">Full functionality of Office 365 ProPlus apps is available only for users who have an Office 365 ProPlus plan.</span></span> <span data-ttu-id="737ee-154">Hello Office 365-előfizetésben tervek című témakörben olvashat [Office 365 service-csomagokról](http://technet.microsoft.com/library/office-365-plan-options.aspx).</span><span class="sxs-lookup"><span data-stu-id="737ee-154">For more details on hello Office 365 subscription plans see [Office 365 service plans](http://technet.microsoft.com/library/office-365-plan-options.aspx).</span></span> <span data-ttu-id="737ee-155">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="737ee-155">Still have questions?</span></span> <span data-ttu-id="737ee-156">Tekintse meg a hello [Office 365 + RemoteApp](remoteapp-o365.md) információkat.</span><span class="sxs-lookup"><span data-stu-id="737ee-156">Check out hello [Office 365 + RemoteApp](remoteapp-o365.md) information.</span></span> <span data-ttu-id="737ee-157">Emellett olvassa el a hello új cikket, [hogyan toouse az Office 365-előfizetéshez az Azure RemoteApp](remoteapp-officesubscription.md).</span><span class="sxs-lookup"><span data-stu-id="737ee-157">Also check out hello new article, [How toouse your Office 365 subscription with Azure RemoteApp](remoteapp-officesubscription.md).</span></span>

<span data-ttu-id="737ee-158">Vegye figyelembe, hogy szüksége toolicense Office 365 ProPlus, a Visio Pro és a Project Pro külön-külön – ezek mindegyike saját licenccel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="737ee-158">Note that you need toolicense Office 365 ProPlus, Visio Pro, and Project Pro separately - they each have their own license.</span></span>

## <a name="microsoft-office-2013-professional-plus-trial-only"></a><span data-ttu-id="737ee-159">Microsoft Office 2013 Professional Plus (csak próbaverzió)</span><span class="sxs-lookup"><span data-stu-id="737ee-159">Microsoft Office 2013 Professional Plus (trial only)</span></span>
<span data-ttu-id="737ee-160">Hello ingyenes próbaverziós időszak alatt hello szolgáltatás hello Office 2013 lemezképpel tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="737ee-160">During hello free trial period, you can test hello service with hello Office 2013 image.</span></span>

<span data-ttu-id="737ee-161">Ez a rendszerkép hello eredeti rendszerkép bővített verziója, és hello Microsoft Office 2013 Professional Plus következő összetevői telepítve van ezen kívül hello Windows Server 2012 R2 rendszerképnél ismertetett toohello összetevők:</span><span class="sxs-lookup"><span data-stu-id="737ee-161">This image is an extension of hello vanilla image and has hello following components of Microsoft Office 2013 Professional Plus installed in addition toohello components described in hello Windows Server 2012 R2 image:</span></span>

* <span data-ttu-id="737ee-162">Hozzáférés</span><span class="sxs-lookup"><span data-stu-id="737ee-162">Access</span></span>
* <span data-ttu-id="737ee-163">Excel</span><span class="sxs-lookup"><span data-stu-id="737ee-163">Excel</span></span>
* <span data-ttu-id="737ee-164">Lync</span><span class="sxs-lookup"><span data-stu-id="737ee-164">Lync</span></span>
* <span data-ttu-id="737ee-165">OneNote</span><span class="sxs-lookup"><span data-stu-id="737ee-165">OneNote</span></span>
* <span data-ttu-id="737ee-166">Onedrive vállalati verzió (vegye figyelembe, hogy hello sync-ügynök nem támogatott az Azure RemoteApp használata)</span><span class="sxs-lookup"><span data-stu-id="737ee-166">OneDrive for Business (note that hello sync agent is not supported for use with Azure RemoteApp)</span></span>
* <span data-ttu-id="737ee-167">Outlook</span><span class="sxs-lookup"><span data-stu-id="737ee-167">Outlook</span></span>
* <span data-ttu-id="737ee-168">PowerPoint</span><span class="sxs-lookup"><span data-stu-id="737ee-168">PowerPoint</span></span>
* <span data-ttu-id="737ee-169">Project</span><span class="sxs-lookup"><span data-stu-id="737ee-169">Project</span></span>
* <span data-ttu-id="737ee-170">Visio</span><span class="sxs-lookup"><span data-stu-id="737ee-170">Visio</span></span>
* <span data-ttu-id="737ee-171">Word</span><span class="sxs-lookup"><span data-stu-id="737ee-171">Word</span></span>
* <span data-ttu-id="737ee-172">Microsoft Office Nyelvi ellenőrző eszközök</span><span class="sxs-lookup"><span data-stu-id="737ee-172">Microsoft Office Proofing Tools</span></span>

> [!IMPORTANT]
> <span data-ttu-id="737ee-173">**Jogi információk:** Ez a rendszerkép nem tartalmaz Microsoft Office-licencet, és *nem használható termelési környezetben*.</span><span class="sxs-lookup"><span data-stu-id="737ee-173">**Legal information:** This image does not include a Microsoft Office license and *cannot be used for production*.</span></span> <span data-ttu-id="737ee-174">hello Office 2013 Professional Plus-rendszerkép csak próbaverziós felhasználásra számára készült.</span><span class="sxs-lookup"><span data-stu-id="737ee-174">hello Office 2013 Professional Plus image is intended for trial use only.</span></span> <span data-ttu-id="737ee-175">Ha azt szeretné, toouse Office-alkalmazások az Azure Remoteappban termelési környezetben, meg kell toouse hello Office 365 ProPlus-rendszerképet.</span><span class="sxs-lookup"><span data-stu-id="737ee-175">If you want toouse Office apps in Azure RemoteApp for production, you need toouse hello Office 365 ProPlus image.</span></span> <span data-ttu-id="737ee-176">Az Office licenckezelésével kapcsolatos további részletekért tekintse meg a [Using Office 365 with Azure RemoteApp](remoteapp-o365.md) (Az Office és az Azure RemoteApp használata) című témakört</span><span class="sxs-lookup"><span data-stu-id="737ee-176">For more details on licensing Office, see [Using Office 365 with Azure RemoteApp](remoteapp-o365.md)</span></span>
> 
> 

