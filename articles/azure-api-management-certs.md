---
title: "Az Azure felügyeleti API-tanúsítvány feltöltése |} Microsoft Docs"
description: "Megtudhatja, hogyan athe felügyeleti API-tanúsítvány feltöltése a klasszikus Azure portálon."
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
ms.openlocfilehash: 9dc438e927acd9aef38f06807fabf3dda9b021c9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a><span data-ttu-id="57bf9-103">Az Azure szolgáltatásfelügyeleti API Management-tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="57bf9-103">Upload an Azure Management API Management Certificate</span></span>
<span data-ttu-id="57bf9-104">Felügyeleti tanúsítványok hitelesítik magukat az Azure által biztosított klasszikus üzembe helyezési modellel teszik lehetővé.</span><span class="sxs-lookup"><span data-stu-id="57bf9-104">Management certificates allow you to authenticate with the classic deployment model provided by Azure.</span></span> <span data-ttu-id="57bf9-105">Számos programok telepítése és eszközök (például a Visual Studio vagy az Azure SDK-val) és a különböző Azure-szolgáltatások telepítési automatizálására használható ezeket a tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="57bf9-105">Many programs and tools (such as Visual Studio or the Azure SDK) use these certificates to automate configuration and deployment of various Azure services.</span></span> 

> [!WARNING]
> <span data-ttu-id="57bf9-106">légy óvatos!</span><span class="sxs-lookup"><span data-stu-id="57bf9-106">Be careful!</span></span> <span data-ttu-id="57bf9-107">Ezek a típusok, a tanúsítványok bárki hitelesíti magát azokat kezelheti az előfizetést, amelyekhez tartoznak.</span><span class="sxs-lookup"><span data-stu-id="57bf9-107">These types of certificates allow anyone who authenticates with them to manage the subscription they are associated with.</span></span>
>
>

<span data-ttu-id="57bf9-108">Ha szeretne további információ az Azure (beleértve a önaláírt tanúsítvány létrehozása), lásd: [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).</span><span class="sxs-lookup"><span data-stu-id="57bf9-108">If you'd like more information about Azure certificates (including creating a self-signed certificate), see [Certificates overview for Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).</span></span>

<span data-ttu-id="57bf9-109">Is [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) automation célokra Ügyfélkód hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="57bf9-109">You can also use [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) to authenticate client-code for automation purposes.</span></span>

## <a name="upload-a-management-certificate"></a><span data-ttu-id="57bf9-110">A felügyeleti tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="57bf9-110">Upload a management certificate</span></span>
<span data-ttu-id="57bf9-111">Ha elvégezte a felügyeleti tanúsítvány létrehozása, (.cer-fájl csak a nyilvános kulcs) feltöltheti a portált.</span><span class="sxs-lookup"><span data-stu-id="57bf9-111">Once you have a management certificate created, (.cer file with only the public key) you can upload it into the portal.</span></span> <span data-ttu-id="57bf9-112">Ha a tanúsítvány áll rendelkezésre a portálon, bárki, aki egyező tanúsítványt (titkos kulcs) csatlakozzon a felügyeleti API-n keresztül, és a társított előfizetés erőforrásokhoz férnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="57bf9-112">When the certificate is available in the portal, anyone with a matching certificate (private key) can connect through the Management API and access the resources for the associated subscription.</span></span>

1. <span data-ttu-id="57bf9-113">Jelentkezzen be az [Azure portálra](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="57bf9-113">Log in to the [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="57bf9-114">Kattintson a **további szolgáltatások** alsó Azure-szolgáltatások listája, majd válassza **előfizetések** a a _általános_ szolgáltatási csoport.</span><span class="sxs-lookup"><span data-stu-id="57bf9-114">Click **More services** at the bottom Azure service list, then select **Subscriptions** in the _General_ service group.</span></span>

    ![Előfizetés menü](./media/azure-api-management-certs/subscriptions_menu.png)

3. <span data-ttu-id="57bf9-116">Ügyeljen arra, hogy válassza ki a megfelelő előfizetést, amelyet szeretne társítani a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="57bf9-116">Make sure to select the correct subscription that you want to associate with the certificate.</span></span>     
4. <span data-ttu-id="57bf9-117">Miután kiválasztotta a megfelelő előfizetés, nyomja le az **felügyeleti tanúsítványok** a a _beállítások_ csoport.</span><span class="sxs-lookup"><span data-stu-id="57bf9-117">After you have selected the correct subscription, press **Management certificates** in the _Settings_ group.</span></span>

    ![Beállítások](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. <span data-ttu-id="57bf9-119">Nyomja meg a **feltöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="57bf9-119">Press the **Upload** button.</span></span>

    ![A lapon a tanúsítványok feltöltése](./media/azure-api-management-certs/certificates_page.png)
6. <span data-ttu-id="57bf9-121">Töltse ki a párbeszédpanelen információkat, és nyomja le az **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="57bf9-121">Fill out the dialog information and press **Upload**.</span></span>

    ![Beállítások](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a><span data-ttu-id="57bf9-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="57bf9-123">Next steps</span></span>
<span data-ttu-id="57bf9-124">Most, hogy egy előfizetéshez társított felügyeleti tanúsítvány, (miután helyileg a megfelelő tanúsítvány telepítése) programokon keresztül csatlakozhat a [klasszikus üzembe helyezési modellel REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) és automatizálhatja a különböző Azure-erőforrások, amelyek is, hogy az előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="57bf9-124">Now that you have a management certificate associated with a subscription, you can (after you have installed the matching certificate locally) programmatically connect to the [classic deployment model REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) and automate the various Azure resources that are also associated with that subscription.</span></span>
