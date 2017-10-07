---
title: "az Azure felügyeleti API tanúsítvány aaaUpload |} Microsoft Docs"
description: "Ismerje meg, hogyan tooupload athe felügyeleti API a klasszikus Azure portál hello tanúsítvány."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: na
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: adegeo
ms.openlocfilehash: 8294d7131cfb01dba664bd4fd04b6fc22c1e93ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a><span data-ttu-id="e0040-103">Az Azure szolgáltatásfelügyeleti API Management-tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="e0040-103">Upload an Azure Management API Management Certificate</span></span>
<span data-ttu-id="e0040-104">Felügyeleti tanúsítványok engedélyezése az Azure által biztosított tooauthenticate hello klasszikus üzembe helyezési modellt.</span><span class="sxs-lookup"><span data-stu-id="e0040-104">Management certificates allow you tooauthenticate with hello classic deployment model provided by Azure.</span></span> <span data-ttu-id="e0040-105">Számos programok telepítése és eszközök (például a Visual Studio vagy hello Azure SDK-t) használni ezeket a tanúsítványokat tooautomate konfigurációs és különböző Azure-szolgáltatásokhoz központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="e0040-105">Many programs and tools (such as Visual Studio or hello Azure SDK) use these certificates tooautomate configuration and deployment of various Azure services.</span></span> 

> [!WARNING]
> <span data-ttu-id="e0040-106">légy óvatos!</span><span class="sxs-lookup"><span data-stu-id="e0040-106">Be careful!</span></span> <span data-ttu-id="e0040-107">Ezek a típusok, a tanúsítványok bárki hitelesíti magát őket toomanage hello előfizetéshez vannak hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="e0040-107">These types of certificates allow anyone who authenticates with them toomanage hello subscription they are associated with.</span></span>
>
>

<span data-ttu-id="e0040-108">Ha szeretne további információ az Azure (beleértve a önaláírt tanúsítvány létrehozása), lásd: [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).</span><span class="sxs-lookup"><span data-stu-id="e0040-108">If you'd like more information about Azure certificates (including creating a self-signed certificate), see [Certificates overview for Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).</span></span>

<span data-ttu-id="e0040-109">Is [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) tooauthenticate-Ügyfélkód automation célokra.</span><span class="sxs-lookup"><span data-stu-id="e0040-109">You can also use [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) tooauthenticate client-code for automation purposes.</span></span>

## <a name="upload-a-management-certificate"></a><span data-ttu-id="e0040-110">A felügyeleti tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="e0040-110">Upload a management certificate</span></span>
<span data-ttu-id="e0040-111">Ha elvégezte a felügyeleti tanúsítvány létrehozása, (.cer-fájl csak hello nyilvános kulcs) feltöltheti hello portált.</span><span class="sxs-lookup"><span data-stu-id="e0040-111">Once you have a management certificate created, (.cer file with only hello public key) you can upload it into hello portal.</span></span> <span data-ttu-id="e0040-112">Hello tanúsítvány nem érhető el a hello portál, bárki, aki egyező tanúsítványt (titkos kulcs) hello felügyeleti API és a hozzáférés hello hello tartozó előfizetés erőforrásainak keresztül kapcsolódhatnak.</span><span class="sxs-lookup"><span data-stu-id="e0040-112">When hello certificate is available in hello portal, anyone with a matching certificate (private key) can connect through hello Management API and access hello resources for hello associated subscription.</span></span>

1. <span data-ttu-id="e0040-113">Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e0040-113">Log in toohello [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="e0040-114">Kattintson a **további szolgáltatások** hello alsó Azure-szolgáltatások listáját, majd válassza **előfizetések** a hello _általános_ szolgáltatási csoport.</span><span class="sxs-lookup"><span data-stu-id="e0040-114">Click **More services** at hello bottom Azure service list, then select **Subscriptions** in hello _General_ service group.</span></span>

    ![Előfizetés menü](./media/azure-api-management-certs/subscriptions_menu.png)

3. <span data-ttu-id="e0040-116">Ellenőrizze, hogy tooselect hello megfelelő előfizetés, amelyet az tooassociate hello tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="e0040-116">Make sure tooselect hello correct subscription that you want tooassociate with hello certificate.</span></span>     
4. <span data-ttu-id="e0040-117">Miután kiválasztotta a megfelelő előfizetés hello, nyomja le az **felügyeleti tanúsítványok** a hello _beállítások_ csoport.</span><span class="sxs-lookup"><span data-stu-id="e0040-117">After you have selected hello correct subscription, press **Management certificates** in hello _Settings_ group.</span></span>

    ![Beállítások](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. <span data-ttu-id="e0040-119">Nyomja le az hello **feltöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e0040-119">Press hello **Upload** button.</span></span>

    ![A lapon a tanúsítványok feltöltése](./media/azure-api-management-certs/certificates_page.png)
6. <span data-ttu-id="e0040-121">Töltse ki a hello párbeszédpanel adatai, és nyomja le az **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="e0040-121">Fill out hello dialog information and press **Upload**.</span></span>

    ![Beállítások](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a><span data-ttu-id="e0040-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e0040-123">Next steps</span></span>
<span data-ttu-id="e0040-124">Most, hogy egy előfizetéshez társított felügyeleti tanúsítvány, (a frissítés telepítése után helyileg a tanúsítvány megfelelő hello) programozott módon csatlakoztathatja toohello [klasszikus üzembe helyezési modellel REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) és automatizálásához hello különböző Azure-erőforrások is az adott előfizetéshez társítva van.</span><span class="sxs-lookup"><span data-stu-id="e0040-124">Now that you have a management certificate associated with a subscription, you can (after you have installed hello matching certificate locally) programmatically connect toohello [classic deployment model REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) and automate hello various Azure resources that are also associated with that subscription.</span></span>
