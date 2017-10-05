---
title: "Azure Szolgáltatásvégpontokra"
description: "Az Eclipse Azure eszköztára Azure szolgáltatás végpontjának beállításait ismerteti."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9c6125ec-7278-461e-b69c-ed56e844f742
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 6059c292c2687f1bf3d9be04c03aaaaf6adde945
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-service-endpoints"></a><span data-ttu-id="00aad-103">Azure Szolgáltatásvégpontokra</span><span class="sxs-lookup"><span data-stu-id="00aad-103">Azure Service Endpoints</span></span>
<span data-ttu-id="00aad-104">Azure szolgáltatásvégpontokra határozza meg, hogy az alkalmazás központi telepítése és a globális Azure platform által kezelt, Azure által üzemeltetett Kína, vagy egy olyan magánhálózat 21Vianet Azure platformon.</span><span class="sxs-lookup"><span data-stu-id="00aad-104">Azure service endpoints determine whether your application is deployed to and managed by the global Azure platform, Azure operated by 21Vianet in China, or a private Azure platform.</span></span> <span data-ttu-id="00aad-105">A **Szolgáltatásvégpontok** párbeszédpanel lehetővé teszi, hogy mely használni kívánt Szolgáltatásvégpontok megadását.</span><span class="sxs-lookup"><span data-stu-id="00aad-105">The **Service Endpoints** dialog allows you to specify which service endpoints you want to use.</span></span> <span data-ttu-id="00aad-106">Megnyitásához a **Szolgáltatásvégpontok** párbeszédpaneljén eclipse-ben kattintson **ablak**, kattintson a **beállítások**, bontsa ki a **Azure**, és kattintson a **Szolgáltatásvégpontok**.</span><span class="sxs-lookup"><span data-stu-id="00aad-106">To open the **Service Endpoints** dialog, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Service Endpoints**.</span></span> <span data-ttu-id="00aad-107">Beállítás a **aktív beállítása** mező határozza meg, hogy melyik Azure szolgáltatásvégpontokra fog történni az Azure projektek az aktuális munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="00aad-107">Setting the **Active Set** field determines which Azure service endpoints will be used for the Azure projects in your current workspace.</span></span>

<span data-ttu-id="00aad-108">Az alábbi látható a **Szolgáltatásvégpontok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="00aad-108">The following shows the **Service Endpoints** dialog.</span></span>

![][ic719493]

## <a name="to-set-the-service-endpoints"></a><span data-ttu-id="00aad-109">A szolgáltatás végpontokat</span><span class="sxs-lookup"><span data-stu-id="00aad-109">To set the service endpoints</span></span>
<span data-ttu-id="00aad-110">Az a **Szolgáltatásvégpontok** párbeszédpanelen tegye a következők közül:</span><span class="sxs-lookup"><span data-stu-id="00aad-110">In the **Service Endpoints** dialog, take one of the following actions:</span></span>

* <span data-ttu-id="00aad-111">Ha szeretné-e a globális Azure platformon, használja a **aktív beállítása** legördülő listában válassza ki **windowsazure.com** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="00aad-111">If you want to use the global Azure platform, from the **Active Set** dropdown list, select **windowsazure.com** and click **OK**.</span></span>

* <span data-ttu-id="00aad-112">Ha a Kínában a 21Vianet által működtetett Azure használni kívánt a **aktív beállítása** legördülő listában válassza ki **windowsazure.cn (Kína)** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="00aad-112">If you want to use Azure operated by 21Vianet in China, from the **Active Set** dropdown list, select **windowsazure.cn (China)** and click **OK**.</span></span>

* <span data-ttu-id="00aad-113">Ha szeretné használni a saját Azure platformon:</span><span class="sxs-lookup"><span data-stu-id="00aad-113">If you want to use a private Azure platform:</span></span>

  1. <span data-ttu-id="00aad-114">Kattintson a **Szerkesztés** gombra.</span><span class="sxs-lookup"><span data-stu-id="00aad-114">Click **Edit**.</span></span>

  2. <span data-ttu-id="00aad-115">Megnyílik egy párbeszédpanel, hogy értesítést küld, amely a **Szolgáltatásvégpontok** párbeszédpanel bezárul, és a készletek beállításfájlt fogja megnyitni.</span><span class="sxs-lookup"><span data-stu-id="00aad-115">A dialog box opens, informing you that the **Service Endpoints** dialog will be closed, and the preference sets file will be opened.</span></span> <span data-ttu-id="00aad-116">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="00aad-116">Click **OK**.</span></span>

  3. <span data-ttu-id="00aad-117">A preferencesets.xml fájlban, hozzon létre egy új `preferenceset` elemet.</span><span class="sxs-lookup"><span data-stu-id="00aad-117">In the preferencesets.xml file, create a new `preferenceset` element.</span></span> <span data-ttu-id="00aad-118">Az új elem létrehozása `name`, `blob`, `management`, `portalURL` és `publishsettings` attribútumot, majd adja hozzá az értékek azokat, amelyek megfelelnek a saját Azure platformon.</span><span class="sxs-lookup"><span data-stu-id="00aad-118">For this new element, create `name`, `blob`, `management`, `portalURL` and `publishsettings` attributes, and add values for them that correspond to your private Azure platform.</span></span> <span data-ttu-id="00aad-119">Használhatja a meglévő megadott értékek `preferenceset` elemek sablonként.</span><span class="sxs-lookup"><span data-stu-id="00aad-119">You can use the values provided for the existing `preferenceset` elements as templates.</span></span> <span data-ttu-id="00aad-120">**Megjegyzés:**: használt érték a `blob` attribútum az URL-cím "blob" szöveget kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="00aad-120">**Note**: The value used for the `blob` attribute must contain the text "blob" in the URL.</span></span>

  4. <span data-ttu-id="00aad-121">Mentse és zárja be a preferencesets.xml.</span><span class="sxs-lookup"><span data-stu-id="00aad-121">Save and close preferencesets.xml.</span></span>

  5. <span data-ttu-id="00aad-122">Nyissa meg újra a **Szolgáltatásvégpontok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="00aad-122">Reopen the **Service Endpoints** dialog.</span></span>

  6. <span data-ttu-id="00aad-123">Az a **aktív beállítása** legördülő listában válassza ki az aktív beállítása létrehozott, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="00aad-123">From the **Active Set** dropdown list, select the active set that you created and click **OK**.</span></span>

  7. <span data-ttu-id="00aad-124">Ha létrehozta a saját Azure platformon `preferenceset` elem, kattintva rendelt értékeket módosíthatja a **szerkesztése** gombra a **Services végpontjánál** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="00aad-124">Once you've created your private Azure platform `preferenceset` element, you can change the values assigned to it by clicking the **Edit** button in the **Services Endpoint** dialog.</span></span> <span data-ttu-id="00aad-125">Több privát Azure platformon is létrehozhat `preferenceset` elemek, ha felügyelni.</span><span class="sxs-lookup"><span data-stu-id="00aad-125">You can also create multiple private Azure platform `preferenceset` elements, if you desire.</span></span>

## <a name="see-also"></a><span data-ttu-id="00aad-126">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="00aad-126">See Also</span></span>
<span data-ttu-id="00aad-127">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="00aad-127">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="00aad-128">[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="00aad-128">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="00aad-129">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="00aad-129">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="00aad-130">Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="00aad-130">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->
