---
title: "aaaTroubleshoot RemoteApp felhőalapú gyűjtemények - létrehozási |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot RemoteApp felhőalapú gyűjtemény létrehozása sikertelen"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: 95eb7797-7b5e-4781-ad23-f15dd85b213d
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 9484ecbdb048ede8df725215b313e049cc7648f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a><span data-ttu-id="954aa-103">Létrehozása RemoteApp felhőalapú gyűjtemények hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="954aa-103">Troubleshoot creating RemoteApp cloud collections</span></span>
> [!IMPORTANT]
> <span data-ttu-id="954aa-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="954aa-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="954aa-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="954aa-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="954aa-106">Ha problémába ütközik egy felhőalapú gyűjtemény létrehozása, tekintse meg a következő információk hello.</span><span class="sxs-lookup"><span data-stu-id="954aa-106">If you are having problems creating a cloud collection, check out hello following information.</span></span>

## <a name="your-image-is-invalid"></a><span data-ttu-id="954aa-107">A kép érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="954aa-107">Your image is invalid</span></span>
<span data-ttu-id="954aa-108">Ha megjelenik egy üzenet, például "GoldImageInvalid", ha vár a Azure tooprovision a gyűjtemény, az azt jelenti, hogy a sablon lemezképe nem felel meg hello [kép követelmények definiált](remoteapp-imagereqs.md).</span><span class="sxs-lookup"><span data-stu-id="954aa-108">If you see a message like, "GoldImageInvalid" when you are waiting for Azure tooprovision your collection, it means that your template image doesn't meet hello [defined image requirements](remoteapp-imagereqs.md).</span></span> <span data-ttu-id="954aa-109">Igen, nyissa meg olvasni azokat [követelmények](remoteapp-imagereqs.md), javítsa ki a lemezkép és toocreate próbálkozzon újra a gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="954aa-109">So, go read those [requirements](remoteapp-imagereqs.md), fix your image, and try toocreate your collection again.</span></span>

## <a name="common-errors-seen-in-hello-azure-management-portal"></a><span data-ttu-id="954aa-110">Gyakori hibák látható a hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="954aa-110">Common errors seen in hello Azure Management portal</span></span>
    DNS server could not be reached
    ProvisioningTimeout

<span data-ttu-id="954aa-111">A felhőalapú gyűjtemények gyakran sikertelen, mert létrehozása során egyéni rendszerképet használ.</span><span class="sxs-lookup"><span data-stu-id="954aa-111">Cloud collections often fail during creation because of you are using custom images.</span></span>  <span data-ttu-id="954aa-112">Ha a fenti hibák hello egyikét látja, és egy egyéni lemezkép toocreate hello gyűjteményt használ, ellenőrizze a dolgok következő hello:</span><span class="sxs-lookup"><span data-stu-id="954aa-112">If you see one of hello above errors and you are using a custom image toocreate hello collection, please check hello following things:</span></span>

* <span data-ttu-id="954aa-113">Győződjön meg arról, hello egyéni lemezképhez feltöltött lemezkép követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="954aa-113">Ensure that hello custom image you uploaded meets image requirements.</span></span>
* <span data-ttu-id="954aa-114">Leggyakrabban hello gyakori probléma a hello lemezkép nem megfelelően a Sysprep segédprogrammal előkészített.</span><span class="sxs-lookup"><span data-stu-id="954aa-114">Most often hello common problem is that hello image was not properly syspreped.</span></span>  
* <span data-ttu-id="954aa-115">Győződjön meg arról hello kép rendszerindítást a Hyper-V belül, vagy próbáljon meg létrehozni egy infrastruktúra-szolgáltatási virtuális gép közvetlenül az Azure-előfizetése hello lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="954aa-115">Verify hello image can boot within Hyper-V or try creating an IAAS VM directly in your Azure subscription using hello image.</span></span> <span data-ttu-id="954aa-116">Ha hello VM tooboot és nem kezdő nem sikerül, akkor ez általában jelzi hello egyéni lemezkép nem lett megfelelően előkészítve.</span><span class="sxs-lookup"><span data-stu-id="954aa-116">If hello VM fails tooboot and not start, then this usually indicates that hello custom image was not prepared correctly.</span></span>  <span data-ttu-id="954aa-117">Ellenőrizze a hello egyéni lemezkép készült hello követően hogyan toocreate egy egyéni sablon rendszerképet a RemoteApp</span><span class="sxs-lookup"><span data-stu-id="954aa-117">Verify hello custom image was built following hello How toocreate a custom template image for RemoteApp</span></span>

<span data-ttu-id="954aa-118">Ha hello Microsoft rendszerképeket az előfizetéskor használunk, próbálja toocreate hello gyűjteményt újra.</span><span class="sxs-lookup"><span data-stu-id="954aa-118">If you are using one of hello Microsoft images included with your subscription, try toocreate hello collection again.</span></span> <span data-ttu-id="954aa-119">Ha nem szűnik hello majd forduljon a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="954aa-119">If hello issue persists then please contact Microsoft support.</span></span>

    PlatformImageTrialModeOnly

<span data-ttu-id="954aa-120">Ha ezt a hibát látva Ez általában azt jelenti, hogy frissítették tooa fizetett fiók, de egy Microsoft által biztosított lemezképet, amely érvényes csak üzemmódban hello próba hello szolgáltatás toouse próbált.</span><span class="sxs-lookup"><span data-stu-id="954aa-120">If you see this error this usually means that you been upgraded tooa paid account but you are trying toouse a Microsoft provided image that is valid only during hello trial mode of hello service.</span></span> <span data-ttu-id="954aa-121">Ebben az esetben toocreate próbálkozzon újra a felhőalapú gyűjtemény, de lehet, hogy toospecify hello megfelelő lemezképet.</span><span class="sxs-lookup"><span data-stu-id="954aa-121">In this case, try toocreate your cloud collection again, but be sure toospecify hello correct image.</span></span>

