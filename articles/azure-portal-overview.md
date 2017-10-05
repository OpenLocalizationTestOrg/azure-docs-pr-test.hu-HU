---
title: "Az Azure Portal áttekintése | Microsoft Docs"
description: "Ismerje meg, a Microsoft Azure Portalt használatát."
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
ms.openlocfilehash: 71820306716c6297085a29f3ceab89b55396bfe6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="microsoft-azure-portal-overview"></a><span data-ttu-id="f0277-103">A Microsoft Azure Portal áttekintése</span><span class="sxs-lookup"><span data-stu-id="f0277-103">Microsoft Azure portal overview</span></span>
<span data-ttu-id="f0277-104">A Microsoft Azure Portal egy központi felület, ahol kioszthatja és kezelheti az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="f0277-104">The Microsoft Azure portal is a central place where you can provision and manage your Azure resources.</span></span>  <span data-ttu-id="f0277-105">Ez az oktatóanyag bemutatja a portált, illetve megmutatja, hogyan használhatja néhány főbb funkcióját</span><span class="sxs-lookup"><span data-stu-id="f0277-105">This tutorial will familiarize you with the portal and show you how to use some of these key capabilities:</span></span>

* <span data-ttu-id="f0277-106">Az **univerzális piactéren** a Microsoft és egyéb gyártók által fejlesztett alkalmazások ezrei között válogathat, amelyeket megvásárolhat és/vagy letölthet.</span><span class="sxs-lookup"><span data-stu-id="f0277-106">A **comprehensive marketplace** that lets you browse through thousands of items from Microsoft and other vendors that can be purchased and/or provisioned.</span></span>
* <span data-ttu-id="f0277-107">Az **egységesített és méretezhető tallózási élménynek** köszönhetően könnyen megtalálhatja a fontos erőforrásokat és végrehajthat különböző felügyeleti műveleteket.</span><span class="sxs-lookup"><span data-stu-id="f0277-107">A **unified and scalable browse experience** that makes it easy to find the resources you care about and perform various management operations.</span></span>
* <span data-ttu-id="f0277-108">A **konzisztens felügyeleti oldalak** (vagy panelek) segítségével az Azure számos szolgáltatását kezelheti a beállítások, műveletek, számlázási adatok, állapotfigyelés, használati adatok és sok más információ egységes megjelenítésével.</span><span class="sxs-lookup"><span data-stu-id="f0277-108">**Consistent management pages** (or blades) that let you manage Azure’s wide variety of services through a consistent way of exposing settings, actions, billing information, health monitoring and usage data, and much more.</span></span>
* <span data-ttu-id="f0277-109">A **személyes élmény** részeként létrehozhat egy testreszabott indítóképernyőt, amely azokat az információkat jeleníti meg, amelyeket látni kíván a bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="f0277-109">A **personal experience** that lets you create a customized start screen that shows the information that you want to see whenever you log in.</span></span>  <span data-ttu-id="f0277-110">A csempéket tartalmazó mindegyik felügyeleti panelt testreszabhatja.</span><span class="sxs-lookup"><span data-stu-id="f0277-110">You can also customize any of the management blades that contain tiles.</span></span>
  
  ![Az Azure Portal felhasználói felületének bemutatása][UIOrientation]

## <a name="before-you-get-started"></a><span data-ttu-id="f0277-112">A kezdés előtt</span><span class="sxs-lookup"><span data-stu-id="f0277-112">Before you get started</span></span>
<span data-ttu-id="f0277-113">Érvényes Azure-előfizetéssel kell rendelkeznie az oktatóanyag megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="f0277-113">You will need a valid Azure subscription to go through this tutorial.</span></span>  <span data-ttu-id="f0277-114">Ha még nem rendelkezik ezzel, most [regisztrálhat egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f0277-114">If you don’t have one, then [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/) today.</span></span>  <span data-ttu-id="f0277-115">Ha rendelkezik előfizetéssel, a portálhoz a <https://portal.azure.com> webhelyen férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="f0277-115">Once you have a subscription, you can access the portal at <https://portal.azure.com>.</span></span>

## <a name="how-to-create-a-resource"></a><span data-ttu-id="f0277-116">Erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0277-116">How to create a resource</span></span>
<span data-ttu-id="f0277-117">Az Azure Piactéren elemek százait hozhatja létre egyetlen helyen.</span><span class="sxs-lookup"><span data-stu-id="f0277-117">Azure has a marketplace with thousands of items that you can create from one place.</span></span>  <span data-ttu-id="f0277-118">Tegyük fel, hogy egy új Windows Server 2012 virtuális gépet kíván létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f0277-118">Let’s say you want to create a new Windows Server 2012 VM.</span></span>  <span data-ttu-id="f0277-119">Az +ÚJ felület a belépési pont a piactér kiemelt kategóriáinak gondosan összeválogatott listájához.</span><span class="sxs-lookup"><span data-stu-id="f0277-119">The +NEW hub is your entry point into a curated set of featured categories from the marketplace.</span></span>  <span data-ttu-id="f0277-120">Mindegyik kategóriában kiemelt elemeket találhat, valamint egy hivatkozást a teljes piactérre, amely megjeleníti az összes kategóriát és keresést.</span><span class="sxs-lookup"><span data-stu-id="f0277-120">Each category has a small set of featured items along with a link to the full marketplace that shows all categories and search.</span></span> <span data-ttu-id="f0277-121">Az új Windows Server 2012 virtuális gép létrehozásához hajtsa végre a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="f0277-121">To create that new Windows Server 2012 VM, perform the following actions:</span></span>  

1. <span data-ttu-id="f0277-122">A Windows Server 2012 kiemelt elem, ezért kiválaszthatja a Számítás kategóriából.</span><span class="sxs-lookup"><span data-stu-id="f0277-122">Windows Server 2012 is featured, so you can select it from the Compute category.</span></span>  
2. <span data-ttu-id="f0277-123">Töltse ki az űrlap néhány alapvető mezőjét.</span><span class="sxs-lookup"><span data-stu-id="f0277-123">Fill out some basic inputs on a form.</span></span>
3. <span data-ttu-id="f0277-124">Kattintson a „Létrehozás” lehetőségre, és a virtuális gép kiépítése azonnal megkezdődik.</span><span class="sxs-lookup"><span data-stu-id="f0277-124">Click ‘Create’ and your VM will begin to provision immediately.</span></span>

<span data-ttu-id="f0277-125">Az értesítési központ riasztást küld, amikor az erőforrás létrejött, majd megnyílik egy felügyeleti panel (az erőforrásokat később is bármikor megkeresheti).</span><span class="sxs-lookup"><span data-stu-id="f0277-125">The notifications hub will alert you when your resource has been created and a management blade will open (you can always browse to resources later).</span></span>

![Portálkategóriák][PortalCategories]

## <a name="how-to-find-your-resources"></a><span data-ttu-id="f0277-127">Az erőforrások megkeresése</span><span class="sxs-lookup"><span data-stu-id="f0277-127">How to find your resources</span></span>
<span data-ttu-id="f0277-128">A gyakran használt erőforrásokat akármikor rögzítheti a kezdőpulton, azonban előfordulhat, hogy a ritkábban használtakat tallózással kell megkeresnie.</span><span class="sxs-lookup"><span data-stu-id="f0277-128">You can always pin frequently accessed resources to your startboard, but you might need to browse to something that you don’t frequently access.</span></span>  <span data-ttu-id="f0277-129">Az alább látható tallózási központ segítségével érheti el az összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="f0277-129">The browse hub shown below is your way to get to all of your resources.</span></span>  <span data-ttu-id="f0277-130">Szűrhet előfizetés alapján, kiválaszthat/átméretezhet oszlopokat, illetve megnyithatja a felügyeleti paneleket az egyes elemekre kattintva.</span><span class="sxs-lookup"><span data-stu-id="f0277-130">You can filter by subscription, choose/resize columns, and navigate to the management blades by clicking on individual items.</span></span>

![Tallózási központ][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a><span data-ttu-id="f0277-132">Erőforrás-elérés kezelése és delegálása</span><span class="sxs-lookup"><span data-stu-id="f0277-132">How to manage and delegate access to a resource</span></span>
<span data-ttu-id="f0277-133">Ezen a panelen csatlakozhat a virtuális géphez távoli asztali kapcsolattal, ellenőrizhet fontos teljesítménymutatókat, szabályozhatja a hozzáférést a virtuális géphez szerepköralapú hozzáférés-vezérléssel (RBAC), konfigurálhatja a virtuális gépet, illetve végezhet egyéb fontos felügyeleti műveleteket.</span><span class="sxs-lookup"><span data-stu-id="f0277-133">From this blade you can connect to the virtual machine using remote desktop, monitor key performance metrics, control access to this VM using role based access (RBAC), configure the VM, and perform other important management tasks.</span></span>  <span data-ttu-id="f0277-134">A hozzáférés szerepkörön alapuló delegálása fontos a tömeges felügyelethez.</span><span class="sxs-lookup"><span data-stu-id="f0277-134">Delegating access based on role is critical to managing at scale.</span></span>  <span data-ttu-id="f0277-135">További információkért kattintson [ide](active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="f0277-135">Click [here](active-directory/role-based-access-control-configure.md) to learn more about it.</span></span> <span data-ttu-id="f0277-136">Egy erőforráshoz való hozzáférés delegálásához hajtsa végre a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="f0277-136">To delegate access to a resource, perform the following actions:</span></span>

1. <span data-ttu-id="f0277-137">Keresse meg az erőforrást.</span><span class="sxs-lookup"><span data-stu-id="f0277-137">Browse to your resource.</span></span>
2. <span data-ttu-id="f0277-138">Kattintson az „Összes beállítás” lehetőségre az Essentials szakaszban.</span><span class="sxs-lookup"><span data-stu-id="f0277-138">Click ‘All settings’ in the Essentials section.</span></span>
3. <span data-ttu-id="f0277-139">Kattintson a „Felhasználók” lehetőségre a beállításlistában.</span><span class="sxs-lookup"><span data-stu-id="f0277-139">Click ‘Users’ in the settings list.</span></span>
4. <span data-ttu-id="f0277-140">Kattintson a „Hozzáadás” gombra a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="f0277-140">Click ‘Add’ in the command bar.</span></span>
5. <span data-ttu-id="f0277-141">Válassza ki a felhasználót és a szerepkört.</span><span class="sxs-lookup"><span data-stu-id="f0277-141">Choose a user and a role.</span></span>

![Erőforrás kezelése][ManageResource]

## <a name="how-to-get-help"></a><span data-ttu-id="f0277-143">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="f0277-143">How to get help</span></span>
<span data-ttu-id="f0277-144">Ha problémába ütközik, mi szívesen segítünk Önnek.</span><span class="sxs-lookup"><span data-stu-id="f0277-144">If you ever have a problem, we’re here for you.</span></span>  <span data-ttu-id="f0277-145">A portál rendelkezik egy súgó és támogatási oldallal, amely segít a probléma megoldásában.</span><span class="sxs-lookup"><span data-stu-id="f0277-145">The portal has a help and support page that can point you in the right direction.</span></span>  <span data-ttu-id="f0277-146">A [támogatási csomagjától](https://azure.microsoft.com/support/plans/) függően közvetlenül a portálon is létrehozhat támogatási jegyeket.</span><span class="sxs-lookup"><span data-stu-id="f0277-146">Depending on your [support plan](https://azure.microsoft.com/support/plans/), you can also create support tickets directly in the portal.</span></span>  <span data-ttu-id="f0277-147">A támogatási jegy létrehozása után a jegyet a teljes életciklusán keresztül kezelheti a portálon.</span><span class="sxs-lookup"><span data-stu-id="f0277-147">After creating a support ticket, you can manage the lifecycle of the ticket from within the portal.</span></span> <span data-ttu-id="f0277-148">A súgó és támogatás oldalt a Tallózás -> Súgó + támogatás paranccsal is elérheti.</span><span class="sxs-lookup"><span data-stu-id="f0277-148">You can get to the help and support page by navigating to Browse -> Help + support.</span></span>  

![Súgó és támogatás][HelpSupport]

## <a name="summary"></a><span data-ttu-id="f0277-150">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="f0277-150">Summary</span></span>
<span data-ttu-id="f0277-151">Tekintsük át, hogy mit tudott meg ebből az oktatóanyagból:</span><span class="sxs-lookup"><span data-stu-id="f0277-151">Let’s review what you learned in this tutorial:</span></span>

* <span data-ttu-id="f0277-152">Megtudta, hogyan lehet regisztrálni, előfizetést beszerezni és elérni a portált</span><span class="sxs-lookup"><span data-stu-id="f0277-152">You learned how to sign up, get a subscription, and browse to the portal</span></span>
* <span data-ttu-id="f0277-153">Megismerte a portál felhasználói felületét, valamint megtudta, hogyan hozhat létre és kereshet meg erőforrásokat</span><span class="sxs-lookup"><span data-stu-id="f0277-153">You got oriented with the portal UI and learned how to create and browse resources</span></span>
* <span data-ttu-id="f0277-154">Megtudta, hogyan hozhat létre és kereshet meg erőforrásokat</span><span class="sxs-lookup"><span data-stu-id="f0277-154">You learned how to create a resource and browse resources</span></span>
* <span data-ttu-id="f0277-155">Megismerte a felügyeleti panelek struktúráját, illetve a különböző típusú erőforrások egységes kezelésének módját</span><span class="sxs-lookup"><span data-stu-id="f0277-155">You learned about the structure or management blades and how you can consistently manage different types of resources</span></span>
* <span data-ttu-id="f0277-156">Megtudta, hogyan szabhatja testre a portált, hogy az Önnek fontos információkat helyezze a középpontba</span><span class="sxs-lookup"><span data-stu-id="f0277-156">You learned how to customize the portal to bring the information you care about to the front and center</span></span>
* <span data-ttu-id="f0277-157">Megtudta, hogyan szabályozhatja az erőforrásokhoz való hozzáférést szerepköralapú hozzáférés-vezérléssel (RBAC)</span><span class="sxs-lookup"><span data-stu-id="f0277-157">You learned how to control access to resources using role based access (RBAC)</span></span>
* <span data-ttu-id="f0277-158">Megtudta, hogyan kérhet segítséget és támogatást</span><span class="sxs-lookup"><span data-stu-id="f0277-158">You learned how to get help and support</span></span>

<span data-ttu-id="f0277-159">A Microsoft Azure Portal jelentősen leegyszerűsíti az alkalmazások felépítését és kezelését a felhőben.</span><span class="sxs-lookup"><span data-stu-id="f0277-159">The Microsoft Azure portal radically simplifies building and managing your applications in the cloud.</span></span>  <span data-ttu-id="f0277-160">A [felügyeleti blog](https://azure.microsoft.com/blog/topics/management/) követésével mindig naprakész maradhat, mivel folyamatosan [figyelünk a visszajelzésekre](https://feedback.azure.com/forums/223579-azure-preview-portal/) és fejlesztjük a portált.</span><span class="sxs-lookup"><span data-stu-id="f0277-160">Take a look at the [management blog](https://azure.microsoft.com/blog/topics/management/) to keep up to date as we’re constantly [listening to feedback](https://feedback.azure.com/forums/223579-azure-preview-portal/) and making improvements.</span></span>  <span data-ttu-id="f0277-161">[ScottGu blogja](http://weblogs.asp.net/scottgu) egy másik remek hely, ahol megtalálhatja az összes Azure-hírt.</span><span class="sxs-lookup"><span data-stu-id="f0277-161">[ScottGu’s blog](http://weblogs.asp.net/scottgu) is another great place to look for all Azure updates.</span></span>

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png
