---
title: "aaaAzure Szolgáltatásvégpontok"
description: "Hello Azure eszköztára Eclipse hello Azure szolgáltatás végpontjának beállításait ismerteti."
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
ms.openlocfilehash: 357aa56409a894719077f2c8f302575c8ebb6883
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-endpoints"></a><span data-ttu-id="6932e-103">Azure Szolgáltatásvégpontokra</span><span class="sxs-lookup"><span data-stu-id="6932e-103">Azure Service Endpoints</span></span>
<span data-ttu-id="6932e-104">Azure szolgáltatásvégpontokra határozza meg,-e az alkalmazás telepített tooand hello globális Azure platform által kezelt, Azure által üzemeltetett Kína, vagy egy olyan magánhálózat 21Vianet Azure platformon.</span><span class="sxs-lookup"><span data-stu-id="6932e-104">Azure service endpoints determine whether your application is deployed tooand managed by hello global Azure platform, Azure operated by 21Vianet in China, or a private Azure platform.</span></span> <span data-ttu-id="6932e-105">Hello **Szolgáltatásvégpontok** párbeszédpanelen kiválaszthatja, amely szolgáltatás végpontjait toouse kívánt toospecify.</span><span class="sxs-lookup"><span data-stu-id="6932e-105">hello **Service Endpoints** dialog allows you toospecify which service endpoints you want toouse.</span></span> <span data-ttu-id="6932e-106">tooopen hello **Szolgáltatásvégpontok** párbeszédpaneljén eclipse-ben kattintson **ablak**, kattintson a **beállítások**, bontsa ki a **Azure**, és kattintson a **Szolgáltatás végpontjait**.</span><span class="sxs-lookup"><span data-stu-id="6932e-106">tooopen hello **Service Endpoints** dialog, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Service Endpoints**.</span></span> <span data-ttu-id="6932e-107">A beállítás hello **aktív beállítása** mező határozza meg, hogy melyik Azure szolgáltatás végpontok használandó hello Azure projektek az aktuális munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="6932e-107">Setting hello **Active Set** field determines which Azure service endpoints will be used for hello Azure projects in your current workspace.</span></span>

<span data-ttu-id="6932e-108">hello következő mutatja hello **Szolgáltatásvégpontok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6932e-108">hello following shows hello **Service Endpoints** dialog.</span></span>

![][ic719493]

## <a name="tooset-hello-service-endpoints"></a><span data-ttu-id="6932e-109">tooset hello szolgáltatás végpontok</span><span class="sxs-lookup"><span data-stu-id="6932e-109">tooset hello service endpoints</span></span>
<span data-ttu-id="6932e-110">A hello **Szolgáltatásvégpontok** párbeszédpanelen tegye a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="6932e-110">In hello **Service Endpoints** dialog, take one of hello following actions:</span></span>

* <span data-ttu-id="6932e-111">Ha azt szeretné, toouse hello globális Azure platformon, a hello **aktív beállítása** legördülő listában válassza ki **windowsazure.com** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6932e-111">If you want toouse hello global Azure platform, from hello **Active Set** dropdown list, select **windowsazure.com** and click **OK**.</span></span>

* <span data-ttu-id="6932e-112">Ha azt szeretné, Azure Kínában hello a 21Vianet által működtetett toouse **aktív beállítása** legördülő listában válassza ki **windowsazure.cn (Kína)** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6932e-112">If you want toouse Azure operated by 21Vianet in China, from hello **Active Set** dropdown list, select **windowsazure.cn (China)** and click **OK**.</span></span>

* <span data-ttu-id="6932e-113">Ha azt szeretné, hogy a saját Azure platformon toouse:</span><span class="sxs-lookup"><span data-stu-id="6932e-113">If you want toouse a private Azure platform:</span></span>

  1. <span data-ttu-id="6932e-114">Kattintson a **Szerkesztés** gombra.</span><span class="sxs-lookup"><span data-stu-id="6932e-114">Click **Edit**.</span></span>

  2. <span data-ttu-id="6932e-115">Megnyílik egy párbeszédpanel, amely arról tájékoztatja, hogy hello **Szolgáltatásvégpontok** párbeszédpanel bezárul, és hello beállítása beállításfájlt fognak megnyílni.</span><span class="sxs-lookup"><span data-stu-id="6932e-115">A dialog box opens, informing you that hello **Service Endpoints** dialog will be closed, and hello preference sets file will be opened.</span></span> <span data-ttu-id="6932e-116">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6932e-116">Click **OK**.</span></span>

  3. <span data-ttu-id="6932e-117">Hello preferencesets.xml fájlban, hozzon létre egy új `preferenceset` elemet.</span><span class="sxs-lookup"><span data-stu-id="6932e-117">In hello preferencesets.xml file, create a new `preferenceset` element.</span></span> <span data-ttu-id="6932e-118">Az új elem létrehozása `name`, `blob`, `management`, `portalURL` és `publishsettings` attribútumok, és azokat, amelyek megfelelnek a saját Azure platformon tooyour értékek hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="6932e-118">For this new element, create `name`, `blob`, `management`, `portalURL` and `publishsettings` attributes, and add values for them that correspond tooyour private Azure platform.</span></span> <span data-ttu-id="6932e-119">Hello meglévő előírt hello értékeket is használhat `preferenceset` elemek sablonként.</span><span class="sxs-lookup"><span data-stu-id="6932e-119">You can use hello values provided for hello existing `preferenceset` elements as templates.</span></span> <span data-ttu-id="6932e-120">**Megjegyzés:**: hello használt érték hello `blob` attribútum hello szöveget "blob" hello URL-címet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="6932e-120">**Note**: hello value used for hello `blob` attribute must contain hello text "blob" in hello URL.</span></span>

  4. <span data-ttu-id="6932e-121">Mentse és zárja be a preferencesets.xml.</span><span class="sxs-lookup"><span data-stu-id="6932e-121">Save and close preferencesets.xml.</span></span>

  5. <span data-ttu-id="6932e-122">Nyissa meg újra a hello **Szolgáltatásvégpontok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6932e-122">Reopen hello **Service Endpoints** dialog.</span></span>

  6. <span data-ttu-id="6932e-123">A hello **aktív beállítása** legördülő listából válassza hello aktív beállítása létrehozott, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="6932e-123">From hello **Active Set** dropdown list, select hello active set that you created and click **OK**.</span></span>

  7. <span data-ttu-id="6932e-124">Ha létrehozta a saját Azure platformon `preferenceset` elem, módosíthatja hello hozzárendelt értékek tooit hello kattintva **szerkesztése** hello gombjára **Services végpontjánál** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6932e-124">Once you've created your private Azure platform `preferenceset` element, you can change hello values assigned tooit by clicking hello **Edit** button in hello **Services Endpoint** dialog.</span></span> <span data-ttu-id="6932e-125">Több privát Azure platformon is létrehozhat `preferenceset` elemek, ha felügyelni.</span><span class="sxs-lookup"><span data-stu-id="6932e-125">You can also create multiple private Azure platform `preferenceset` elements, if you desire.</span></span>

## <a name="see-also"></a><span data-ttu-id="6932e-126">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="6932e-126">See Also</span></span>
<span data-ttu-id="6932e-127">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="6932e-127">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="6932e-128">[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="6932e-128">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="6932e-129">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="6932e-129">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="6932e-130">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="6932e-130">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->
