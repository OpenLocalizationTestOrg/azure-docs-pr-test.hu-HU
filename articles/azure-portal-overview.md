---
title: "aaaAzure portál áttekintése |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Microsoft Azure-portálon."
services: 
documentationcenter: 
author: davidwrede
manager: erikre
editor: jimbe
ms.assetid: 53cb9df1-c96a-4f4e-b022-18336cd3d697
ms.service: na
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/16/2015
ms.author: dwrede
ms.openlocfilehash: feca8b5e26e9cb373c5e8e688173d5856050bb28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-portal-overview"></a><span data-ttu-id="2ce20-103">A Microsoft Azure Portal áttekintése</span><span class="sxs-lookup"><span data-stu-id="2ce20-103">Microsoft Azure portal overview</span></span>
<span data-ttu-id="2ce20-104">hello Microsoft Azure-portálra egy olyan központi hely, ahol kioszthatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="2ce20-104">hello Microsoft Azure portal is a central place where you can provision and manage your Azure resources.</span></span>  <span data-ttu-id="2ce20-105">Ez az oktatóanyag fog megismerkedhet a portál hello és bemutatják, hogyan toouse, ezek némelyike a kulcs képességek:</span><span class="sxs-lookup"><span data-stu-id="2ce20-105">This tutorial will familiarize you with hello portal and show you how toouse some of these key capabilities:</span></span>

* <span data-ttu-id="2ce20-106">Az **univerzális piactéren** a Microsoft és egyéb gyártók által fejlesztett alkalmazások ezrei között válogathat, amelyeket megvásárolhat és/vagy letölthet.</span><span class="sxs-lookup"><span data-stu-id="2ce20-106">A **comprehensive marketplace** that lets you browse through thousands of items from Microsoft and other vendors that can be purchased and/or provisioned.</span></span>
* <span data-ttu-id="2ce20-107">A **egységesített és méretezhető tallózási élménynek** így később könnyen toofind hello erőforrások érdeklik, és végrehajthat különböző felügyeleti műveleteket.</span><span class="sxs-lookup"><span data-stu-id="2ce20-107">A **unified and scalable browse experience** that makes it easy toofind hello resources you care about and perform various management operations.</span></span>
* <span data-ttu-id="2ce20-108">A **konzisztens felügyeleti oldalak** (vagy panelek) segítségével az Azure számos szolgáltatását kezelheti a beállítások, műveletek, számlázási adatok, állapotfigyelés, használati adatok és sok más információ egységes megjelenítésével.</span><span class="sxs-lookup"><span data-stu-id="2ce20-108">**Consistent management pages** (or blades) that let you manage Azure’s wide variety of services through a consistent way of exposing settings, actions, billing information, health monitoring and usage data, and much more.</span></span>
* <span data-ttu-id="2ce20-109">A **személyes élmény** , amely lehetővé teszi egy testreszabott indítóképernyőt, hogy a mutat be, amelyet meg szeretne toosee, bejelentkezéskor hello létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2ce20-109">A **personal experience** that lets you create a customized start screen that shows hello information that you want toosee whenever you log in.</span></span>  <span data-ttu-id="2ce20-110">Testreszabhatja a csempéket tartalmazó hello felügyeleti panelt.</span><span class="sxs-lookup"><span data-stu-id="2ce20-110">You can also customize any of hello management blades that contain tiles.</span></span>
  
  ![Az Azure Portal felhasználói felületének bemutatása][UIOrientation]

## <a name="before-you-get-started"></a><span data-ttu-id="2ce20-112">A kezdés előtt</span><span class="sxs-lookup"><span data-stu-id="2ce20-112">Before you get started</span></span>
<span data-ttu-id="2ce20-113">Szüksége lesz egy érvényes Azure-előfizetés toogo az oktatóanyag teljesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2ce20-113">You will need a valid Azure subscription toogo through this tutorial.</span></span>  <span data-ttu-id="2ce20-114">Ha még nem rendelkezik ezzel, most [regisztrálhat egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2ce20-114">If you don’t have one, then [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/) today.</span></span>  <span data-ttu-id="2ce20-115">Ha rendelkezik előfizetéssel, végezheti el: hello portál <https://portal.azure.com>.</span><span class="sxs-lookup"><span data-stu-id="2ce20-115">Once you have a subscription, you can access hello portal at <https://portal.azure.com>.</span></span>

## <a name="how-toocreate-a-resource"></a><span data-ttu-id="2ce20-116">Hogyan toocreate erőforrás</span><span class="sxs-lookup"><span data-stu-id="2ce20-116">How toocreate a resource</span></span>
<span data-ttu-id="2ce20-117">Az Azure Piactéren elemek százait hozhatja létre egyetlen helyen.</span><span class="sxs-lookup"><span data-stu-id="2ce20-117">Azure has a marketplace with thousands of items that you can create from one place.</span></span>  <span data-ttu-id="2ce20-118">Tegyük fel, azt szeretné, hogy toocreate egy új Windows Server 2012 virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="2ce20-118">Let’s say you want toocreate a new Windows Server 2012 VM.</span></span>  <span data-ttu-id="2ce20-119">hello + új hub a belépési pont hello piactér kiemelt kategóriáinak gondosan összeválogatott listájához készlete.</span><span class="sxs-lookup"><span data-stu-id="2ce20-119">hello +NEW hub is your entry point into a curated set of featured categories from hello marketplace.</span></span>  <span data-ttu-id="2ce20-120">Minden egyes kategória kiemelt elemeket találhat egy kis készletét a hivatkozás toohello teljes piactérre, amely megjeleníti az összes kategóriát és keresést együtt.</span><span class="sxs-lookup"><span data-stu-id="2ce20-120">Each category has a small set of featured items along with a link toohello full marketplace that shows all categories and search.</span></span> <span data-ttu-id="2ce20-121">toocreate, hogy új Windows Server 2012 virtuális Gépet, hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="2ce20-121">toocreate that new Windows Server 2012 VM, perform hello following actions:</span></span>  

1. <span data-ttu-id="2ce20-122">Windows Server 2012-ben megjelent, így választhat hello számítási kategóriát.</span><span class="sxs-lookup"><span data-stu-id="2ce20-122">Windows Server 2012 is featured, so you can select it from hello Compute category.</span></span>  
2. <span data-ttu-id="2ce20-123">Töltse ki az űrlap néhány alapvető mezőjét.</span><span class="sxs-lookup"><span data-stu-id="2ce20-123">Fill out some basic inputs on a form.</span></span>
3. <span data-ttu-id="2ce20-124">Létrehozása"parancsra, és a virtuális gép tooprovision azonnal megkezdődik.</span><span class="sxs-lookup"><span data-stu-id="2ce20-124">Click ‘Create’ and your VM will begin tooprovision immediately.</span></span>

<span data-ttu-id="2ce20-125">hello értesítések központ riasztást küld, ha az erőforrás létrejött, és megnyílik egy felügyeleti panel (is bármikor megkeresheti tooresources később).</span><span class="sxs-lookup"><span data-stu-id="2ce20-125">hello notifications hub will alert you when your resource has been created and a management blade will open (you can always browse tooresources later).</span></span>

![Portálkategóriák][PortalCategories]

## <a name="how-toofind-your-resources"></a><span data-ttu-id="2ce20-127">Hogyan toofind az erőforrások</span><span class="sxs-lookup"><span data-stu-id="2ce20-127">How toofind your resources</span></span>
<span data-ttu-id="2ce20-128">Tooyour kezdőpulton a gyakran használt erőforrásokat akármikor rögzítheti, de nem gyakrabban toobrowse toosomething szükség lehet.</span><span class="sxs-lookup"><span data-stu-id="2ce20-128">You can always pin frequently accessed resources tooyour startboard, but you might need toobrowse toosomething that you don’t frequently access.</span></span>  <span data-ttu-id="2ce20-129">alább látható tallózási központ hello a módon tooget tooall az erőforrások.</span><span class="sxs-lookup"><span data-stu-id="2ce20-129">hello browse hub shown below is your way tooget tooall of your resources.</span></span>  <span data-ttu-id="2ce20-130">Szűrhet előfizetés, kiválaszthat/átméretezhet oszlopokat, és beállíthatja az egyes elemekre kattintva nyissa meg a toohello felügyeleti panelt.</span><span class="sxs-lookup"><span data-stu-id="2ce20-130">You can filter by subscription, choose/resize columns, and navigate toohello management blades by clicking on individual items.</span></span>

![Tallózási központ][BrowseHub]

## <a name="how-toomanage-and-delegate-access-tooa-resource"></a><span data-ttu-id="2ce20-132">Hogyan toomanage és a delegált hozzáférés tooa erőforrás</span><span class="sxs-lookup"><span data-stu-id="2ce20-132">How toomanage and delegate access tooa resource</span></span>
<span data-ttu-id="2ce20-133">Ezen a panelen csatlakoztassa toohello virtuális gépet a távoli asztali kapcsolattal, ellenőrizhet fontos teljesítménymutatókat, szabályozhatja a hozzáférést toothis VM szerepköralapú hozzáférés-vezérléssel (RBAC) használata, hello virtuális gép konfigurálása, és egyéb fontos felügyeleti feladatok elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="2ce20-133">From this blade you can connect toohello virtual machine using remote desktop, monitor key performance metrics, control access toothis VM using role based access (RBAC), configure hello VM, and perform other important management tasks.</span></span>  <span data-ttu-id="2ce20-134">Hozzáférés szerepkörön alapuló delegálása fontos toomanaging léptékben.</span><span class="sxs-lookup"><span data-stu-id="2ce20-134">Delegating access based on role is critical toomanaging at scale.</span></span>  <span data-ttu-id="2ce20-135">Kattintson a [Itt](active-directory/role-based-access-control-configure.md) toolearn további információt.</span><span class="sxs-lookup"><span data-stu-id="2ce20-135">Click [here](active-directory/role-based-access-control-configure.md) toolearn more about it.</span></span> <span data-ttu-id="2ce20-136">toodelegate tooa erőforrás elérésére, hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="2ce20-136">toodelegate access tooa resource, perform hello following actions:</span></span>

1. <span data-ttu-id="2ce20-137">Keresse meg a tooyour erőforrás.</span><span class="sxs-lookup"><span data-stu-id="2ce20-137">Browse tooyour resource.</span></span>
2. <span data-ttu-id="2ce20-138">Kattintson az "Összes beállítás" hello Essentials szakaszban.</span><span class="sxs-lookup"><span data-stu-id="2ce20-138">Click ‘All settings’ in hello Essentials section.</span></span>
3. <span data-ttu-id="2ce20-139">Kattintson a "Felhasználók" hello-beállítások listájáról.</span><span class="sxs-lookup"><span data-stu-id="2ce20-139">Click ‘Users’ in hello settings list.</span></span>
4. <span data-ttu-id="2ce20-140">Kattintson a "Hozzáadás" hello parancs sávon.</span><span class="sxs-lookup"><span data-stu-id="2ce20-140">Click ‘Add’ in hello command bar.</span></span>
5. <span data-ttu-id="2ce20-141">Válassza ki a felhasználót és a szerepkört.</span><span class="sxs-lookup"><span data-stu-id="2ce20-141">Choose a user and a role.</span></span>

![Erőforrás kezelése][ManageResource]

## <a name="how-tooget-help"></a><span data-ttu-id="2ce20-143">Hogyan tooget Súgó</span><span class="sxs-lookup"><span data-stu-id="2ce20-143">How tooget help</span></span>
<span data-ttu-id="2ce20-144">Ha problémába ütközik, mi szívesen segítünk Önnek.</span><span class="sxs-lookup"><span data-stu-id="2ce20-144">If you ever have a problem, we’re here for you.</span></span>  <span data-ttu-id="2ce20-145">hello portál rendelkezik egy Súgó és támogatási oldallal, amely segít a hello probléma megoldásában.</span><span class="sxs-lookup"><span data-stu-id="2ce20-145">hello portal has a help and support page that can point you in hello right direction.</span></span>  <span data-ttu-id="2ce20-146">Attól függően a [támogatási csomag](https://azure.microsoft.com/support/plans/), közvetlenül a hello portálon is létrehozhat támogatási jegyeket.</span><span class="sxs-lookup"><span data-stu-id="2ce20-146">Depending on your [support plan](https://azure.microsoft.com/support/plans/), you can also create support tickets directly in hello portal.</span></span>  <span data-ttu-id="2ce20-147">A támogatási jegy létrehozása után hello életciklusát hello jegyet hello portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="2ce20-147">After creating a support ticket, you can manage hello lifecycle of hello ticket from within hello portal.</span></span> <span data-ttu-id="2ce20-148">Toohello Súgó és támogatás oldalt tooBrowse -> Súgó + támogatás.</span><span class="sxs-lookup"><span data-stu-id="2ce20-148">You can get toohello help and support page by navigating tooBrowse -> Help + support.</span></span>  

![Súgó és támogatás][HelpSupport]

## <a name="summary"></a><span data-ttu-id="2ce20-150">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="2ce20-150">Summary</span></span>
<span data-ttu-id="2ce20-151">Tekintsük át, hogy mit tudott meg ebből az oktatóanyagból:</span><span class="sxs-lookup"><span data-stu-id="2ce20-151">Let’s review what you learned in this tutorial:</span></span>

* <span data-ttu-id="2ce20-152">Megtudta, hogyan toosign, szerezzen be előfizetést, és keresse meg a toohello portál</span><span class="sxs-lookup"><span data-stu-id="2ce20-152">You learned how toosign up, get a subscription, and browse toohello portal</span></span>
* <span data-ttu-id="2ce20-153">Kapott objektumorientált hello portáljának felhasználói felület és megtanulta, hogyan toocreate, és keresse meg az erőforrások</span><span class="sxs-lookup"><span data-stu-id="2ce20-153">You got oriented with hello portal UI and learned how toocreate and browse resources</span></span>
* <span data-ttu-id="2ce20-154">Ön megtanulta, hogyan toocreate egy erőforrást, és keresse meg az erőforrások</span><span class="sxs-lookup"><span data-stu-id="2ce20-154">You learned how toocreate a resource and browse resources</span></span>
* <span data-ttu-id="2ce20-155">Hello struktúra vagy felügyeleti panelt, és hogyan a különböző típusú erőforrások egységes kezelésének is megismerte</span><span class="sxs-lookup"><span data-stu-id="2ce20-155">You learned about hello structure or management blades and how you can consistently manage different types of resources</span></span>
* <span data-ttu-id="2ce20-156">Megtudta, hogyan toocustomize hello portál toobring hello toohello elöl és középen az Ön számára legfontosabb adatokat</span><span class="sxs-lookup"><span data-stu-id="2ce20-156">You learned how toocustomize hello portal toobring hello information you care about toohello front and center</span></span>
* <span data-ttu-id="2ce20-157">Megtudta, hogyan toocontrol hozzáférés tooresources szerepkör alapú hozzáférés-(vezérléssel RBAC)</span><span class="sxs-lookup"><span data-stu-id="2ce20-157">You learned how toocontrol access tooresources using role based access (RBAC)</span></span>
* <span data-ttu-id="2ce20-158">Megtudta, hogyan tooget Súgó és támogatás</span><span class="sxs-lookup"><span data-stu-id="2ce20-158">You learned how tooget help and support</span></span>

<span data-ttu-id="2ce20-159">hello Microsoft Azure portál jelentősen leegyszerűsíti felépítését és kezelését az alkalmazások hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="2ce20-159">hello Microsoft Azure portal radically simplifies building and managing your applications in hello cloud.</span></span>  <span data-ttu-id="2ce20-160">Vessen egy pillantást hello [felügyeleti blog](https://azure.microsoft.com/blog/topics/management/) tookeep toodate mentése mivel folyamatosan [figyelő toofeedback](https://feedback.azure.com/forums/223579-azure-preview-portal/) és fejlesztjük a portált.</span><span class="sxs-lookup"><span data-stu-id="2ce20-160">Take a look at hello [management blog](https://azure.microsoft.com/blog/topics/management/) tookeep up toodate as we’re constantly [listening toofeedback](https://feedback.azure.com/forums/223579-azure-preview-portal/) and making improvements.</span></span>  <span data-ttu-id="2ce20-161">[ScottGu blogja](http://weblogs.asp.net/scottgu) van egy másik remek hely toolook az összes Azure-hírt.</span><span class="sxs-lookup"><span data-stu-id="2ce20-161">[ScottGu’s blog](http://weblogs.asp.net/scottgu) is another great place toolook for all Azure updates.</span></span>

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png
