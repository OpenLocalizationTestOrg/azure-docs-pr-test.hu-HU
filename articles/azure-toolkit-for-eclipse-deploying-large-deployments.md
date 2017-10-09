---
title: "Nagy telepítések aaaDeploying"
description: "Ismerje meg, hogyan toodeploy nagy üzembe helyezése hello Azure eszköztára eclipse-ben."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 6b1d2a7a5e49c78154fc856a221e64ca8dcfbe9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-large-deployments"></a><span data-ttu-id="7155e-103">Nagy központi telepítése</span><span class="sxs-lookup"><span data-stu-id="7155e-103">Deploying Large Deployments</span></span>
<span data-ttu-id="7155e-104">Ha a telepítés túl nagy toobe hello alapértelmezett approot mappában található, használhatja a helyi tároló egyik erőforrásához hello telepítési legfelső szintű mappa a JDK és az alkalmazáskiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="7155e-104">If your deployment is too large toobe contained in hello default approot folder, you can use a local storage resource as hello deployment root folder for your JDK and application server.</span></span>

## <a name="toouse-a-local-storage-resource-as-hello-deployment-root-folder-for-large-deployments"></a><span data-ttu-id="7155e-105">a helyi tároló egyik erőforrásához hello telepítési legfelső szintű mappa a nagyméretű telepítések toouse</span><span class="sxs-lookup"><span data-stu-id="7155e-105">toouse a local storage resource as hello deployment root folder for large deployments</span></span>
1. <span data-ttu-id="7155e-106">Hozzon létre egy új helyi tároló-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="7155e-106">Create a new local storage resource.</span></span> <span data-ttu-id="7155e-107">hello hello erőforrás neve nem lényeges.</span><span class="sxs-lookup"><span data-stu-id="7155e-107">hello name of hello resource does not matter.</span></span> <span data-ttu-id="7155e-108">Tároló-erőforrások hello szerepkör szintjén van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="7155e-108">Storage resources are defined at hello role level.</span></span> <span data-ttu-id="7155e-109">hello leggyorsabb módon tooaccess hello helyi tároló konfigurációs párbeszédpanel, ahol létrehozhat egy új helyi tároló-erőforrás van hello lépések segítségével: kattintson a jobb gombbal hello szerepköre hello **Project Explorer** nézet (bontsa ki a Azure-projekt csomópont Ha hello szerepkör nem látható), kattintson a **Azure**, és kattintson a **helyi tároló**.</span><span class="sxs-lookup"><span data-stu-id="7155e-109">hello quickest way tooaccess hello local storage configuration dialog, from which you could create a new local storage resource, is by using hello following steps: Right-click hello role in hello **Project Explorer** view (expand your Azure project node if you don't see hello role), click **Azure**, and then click **Local Storage**.</span></span> <span data-ttu-id="7155e-110">Hello belül **helyi tároló** párbeszédpanel, kattintson a **Hozzáadás** toocreate egy új helyi tároló-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="7155e-110">Within hello **Local Storage** dialog, click **Add** toocreate a new local storage resource.</span></span>

2. <span data-ttu-id="7155e-111">Set hello szükséges méret tooat legalább 2048 MB-ot (semmit kevesebb problémái lehetnek hello ugyanazon fájl méretét, akkor tapasztal hello approot).</span><span class="sxs-lookup"><span data-stu-id="7155e-111">Set hello desired size tooat least 2048 MB (anything less may cause hello same file size problems as you would encounter in hello approot).</span></span>

3. <span data-ttu-id="7155e-112">Győződjön meg arról, hogy **hello tartalmának törlése hello szerepkörpéldányt újrahasznosítása esetén** be van jelölve; ez segít futtathatják a hello telepítési ügyfélindítási logikája ütközik a már meglévő fájlok hello erőforrás amikor hello szerepkör a példány újrahasznosított.</span><span class="sxs-lookup"><span data-stu-id="7155e-112">Ensure that **Clean hello contents when hello role instance is recycled** is checked; this will help prevent hello deployment's startup logic from running into conflicts with pre-existing files in hello resource when hello role instance is recycled.</span></span>

4. <span data-ttu-id="7155e-113">Győződjön meg arról, hogy hello **környezeti változó tárolásához hello erőforrás könyvtár elérési útja a telepítés utáni** toohello karakterlánc értéke **DEPLOYROOT**.</span><span class="sxs-lookup"><span data-stu-id="7155e-113">Ensure that hello **Environment variable storing hello resource's directory path after deployment** value is set toohello string **DEPLOYROOT**.</span></span> <span data-ttu-id="7155e-114">A helyi tároló-erőforrás párbeszédpanel hasonló toohello következő fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="7155e-114">Your local storage resource dialog will look similar toohello following.</span></span>

   ![][ic667943]

<span data-ttu-id="7155e-115">Azt is megteheti Ha **DEPLOYROOT** hello, *neve* a helyi erőforrás, és hogy ne változtassa meg hello automatikusan generált környezeti változó neve (amely lesz beállítva, túl **DEPLOYROOT_PATH** ebben az esetben), működő lenne, az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="7155e-115">Alternatively, if you use **DEPLOYROOT** as hello *name* of your local resource and you do not change hello automatically-generated environment variable name (which will be set too**DEPLOYROOT_PATH** in that case), that would work for your application as well.</span></span>

<span data-ttu-id="7155e-116">A helyi tároló egyik erőforrásához létrehozásával kapcsolatos további információk találhatók [helyi tároló tulajdonságainak][Local storage properties].</span><span class="sxs-lookup"><span data-stu-id="7155e-116">Additional information about creating a local storage resource can be found at [Local storage properties][Local storage properties].</span></span>

## <a name="see-also"></a><span data-ttu-id="7155e-117">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7155e-117">See Also</span></span>
<span data-ttu-id="7155e-118">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7155e-118">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="7155e-119">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7155e-119">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="7155e-120">[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7155e-120">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="7155e-121">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="7155e-121">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
