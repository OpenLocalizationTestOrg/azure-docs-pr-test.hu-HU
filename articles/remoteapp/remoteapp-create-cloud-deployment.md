---
title: "az Azure RemoteApp felhőalapú gyűjtemény aaaHow toocreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate a központi telepítés menti az adatokat az Azure RemoteApp hello Azure felhőben."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 4d7c6956-7e4a-4a41-b7f2-7e5832bf01e3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: a072ad19d8293016382831d48d0af8e0f5e0d458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-cloud-collection-of-azure-remoteapp"></a><span data-ttu-id="84ef0-103">Hogyan toocreate az Azure RemoteApp felhőalapú gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="84ef0-103">How toocreate a cloud collection of Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="84ef0-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="84ef0-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="84ef0-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="84ef0-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="84ef0-106">Kétféle [Azure RemoteApp-gyűjtemény](remoteapp-collections.md) létezik:</span><span class="sxs-lookup"><span data-stu-id="84ef0-106">There are two kinds of [Azure RemoteApp collections](remoteapp-collections.md):</span></span> 

* <span data-ttu-id="84ef0-107">Felhő: teljesen az Azure-ban található.</span><span class="sxs-lookup"><span data-stu-id="84ef0-107">Cloud: resides completely in Azure.</span></span> <span data-ttu-id="84ef0-108">Választhat toosave minden adat hello felhőben (, egy kizárólag felhőalapú gyűjtemény) vagy tooconnect a gyűjtemény tooa VNET és van az adatok mentése.</span><span class="sxs-lookup"><span data-stu-id="84ef0-108">You can choose toosave all data in hello cloud (so a cloud-only collection) or tooconnect your collection tooa VNET and save data there.</span></span>   
* <span data-ttu-id="84ef0-109">Hibrid: tartalmazza a virtuális hálózati hozzáférés a helyszíni - ehhez szükséges hello használata az Azure AD és a helyszíni Active Directory-környezetben.</span><span class="sxs-lookup"><span data-stu-id="84ef0-109">Hybrid: includes a virtual network for on-premises access - this requires hello use of Azure AD and an on-premises Active Directory environment.</span></span>

<span data-ttu-id="84ef0-110">Ez az oktatóanyag bemutatja, hogyan hello felhőalapú gyűjtemény létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="84ef0-110">This tutorial walks you through hello process of creating a cloud collection.</span></span> <span data-ttu-id="84ef0-111">Négy lépésben történik:</span><span class="sxs-lookup"><span data-stu-id="84ef0-111">There are four steps:</span></span> 

1. <span data-ttu-id="84ef0-112">Hozzon létre egy Azure RemoteApp-gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="84ef0-112">Create an Azure RemoteApp collection.</span></span>
2. <span data-ttu-id="84ef0-113">Választható lehetőségként konfigurálhatja a címtár-szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="84ef0-113">Optionally configure directory synchronization.</span></span> <span data-ttu-id="84ef0-114">Az Azure AD használata + Active Directory rendelkezik toosynchronize felhasználók, partnerek és jelszavak a helyszíni Active Directory tooyour az Azure AD bérlőhöz.</span><span class="sxs-lookup"><span data-stu-id="84ef0-114">If you are using Azure AD + Active Directory, you have toosynchronize users, contacts, and passwords from your on-premises Active Directory tooyour Azure AD tenant.</span></span>
3. <span data-ttu-id="84ef0-115">Alkalmazások közzététele.</span><span class="sxs-lookup"><span data-stu-id="84ef0-115">Publish apps.</span></span>
4. <span data-ttu-id="84ef0-116">Felhasználói hozzáférés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="84ef0-116">Configure user access.</span></span>

<span data-ttu-id="84ef0-117">**Előkészületek**</span><span class="sxs-lookup"><span data-stu-id="84ef0-117">**Before you begin**</span></span>

<span data-ttu-id="84ef0-118">Hello gyűjtemény létrehozása előtt toodo hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="84ef0-118">You need toodo hello following before creating hello collection:</span></span>

* <span data-ttu-id="84ef0-119">[Regisztráció](https://azure.microsoft.com/services/remoteapp/) az Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="84ef0-119">[Sign up](https://azure.microsoft.com/services/remoteapp/) for Azure RemoteApp.</span></span> 
* <span data-ttu-id="84ef0-120">Hello felhasználók toogrant eléréséhez használni kívánt információt gyűjteni.</span><span class="sxs-lookup"><span data-stu-id="84ef0-120">Gather information about hello users that you want toogrant access to.</span></span> <span data-ttu-id="84ef0-121">Ez lehet, vagy a Microsoft-fiók adatainak, vagy az Active Directory munkahelyi fiók adatait, a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="84ef0-121">This can be either Microsoft account information or Active Directory work account information for users.</span></span>
* <span data-ttu-id="84ef0-122">Ez az eljárás azt feltételezi, hogy vagy folyamatos toouse egy, az előfizetés részeként hello sablonrendszerképek vagy, hogy már rendelkezik feltölteni kívánt toouse hello sablon rendszerképet.</span><span class="sxs-lookup"><span data-stu-id="84ef0-122">This procedure assumes you are either going toouse one of hello template images provided as part of your subscription or that you have already uploaded hello template image you want toouse.</span></span> <span data-ttu-id="84ef0-123">Ha egy másik sablonlemezkép tooupload van szüksége, azt hello sablon rendszerképek lapon teheti meg.</span><span class="sxs-lookup"><span data-stu-id="84ef0-123">If you need tooupload a different template image, you can do that from hello Template Images page.</span></span> <span data-ttu-id="84ef0-124">Kattintson **töltse fel a sablon lemezképe** hello hello varázsló lépéseit kövesse.</span><span class="sxs-lookup"><span data-stu-id="84ef0-124">Just click **upload a template image** and follow hello steps in hello wizard.</span></span> 
* <span data-ttu-id="84ef0-125">Szeretné, hogy toouse hello Office 365 ProPlus-rendszerképet?</span><span class="sxs-lookup"><span data-stu-id="84ef0-125">Want toouse hello Office 365 ProPlus image?</span></span> <span data-ttu-id="84ef0-126">Információkat [Itt](remoteapp-officesubscription.md).</span><span class="sxs-lookup"><span data-stu-id="84ef0-126">Check out info [here](remoteapp-officesubscription.md).</span></span>
* <span data-ttu-id="84ef0-127">Szeretné, hogy LOB programok telepítése és tooprovide egyéni alkalmazásokba?</span><span class="sxs-lookup"><span data-stu-id="84ef0-127">Want tooprovide custom apps or LOB programs?</span></span> <span data-ttu-id="84ef0-128">Hozzon létre egy új [kép](remoteapp-imageoptions.md) és a felhőalapú gyűjtemény használatát.</span><span class="sxs-lookup"><span data-stu-id="84ef0-128">Create a new [image](remoteapp-imageoptions.md) and use it in your cloud collection.</span></span>
* <span data-ttu-id="84ef0-129">Mérje fel, hogy mikor szükséges tooconnect tooa virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="84ef0-129">Figure out whether you need tooconnect tooa VNET.</span></span> <span data-ttu-id="84ef0-130">Ha úgy dönt, hogy a virtuális hálózat tooconnect tooa, győződjön meg arról, megfelel-e hello [irányelvek méretezése](remoteapp-vnetsizing.md) és, hogy az informatikai [kapcsolódhatnak tooRemoteApp](remoteapp-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="84ef0-130">If you choose tooconnect tooa VNET, make sure it meets hello [sizing guidelines](remoteapp-vnetsizing.md) and that it [can connect tooRemoteApp](remoteapp-vnet.md).</span></span> <span data-ttu-id="84ef0-131">Tekintse meg a hello [hálózatok tervezési cikk ](remoteapp-planvnet.md)további információt.</span><span class="sxs-lookup"><span data-stu-id="84ef0-131">Check out hello [VNET planning article ](remoteapp-planvnet.md)for more information.</span></span>
* <span data-ttu-id="84ef0-132">Ha egy virtuális Hálózatot használ, döntse el, hogy kívánja-e toojoin azt tooyour helyi Active Directory-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="84ef0-132">If you're using a VNET, decide whether you want toojoin it tooyour local Active Directory domain.</span></span>

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a><span data-ttu-id="84ef0-133">1. lépés: - Felhőalapú gyűjtemény létrehozása, vagy egy virtuális hálózat nélkül</span><span class="sxs-lookup"><span data-stu-id="84ef0-133">Step 1: Create a cloud collection - with or without a VNET</span></span>
<span data-ttu-id="84ef0-134">Használjon hello alábbi lépéseit túl**csak felhőalapú gyűjtemény létrehozása**:</span><span class="sxs-lookup"><span data-stu-id="84ef0-134">Use hello following steps too**create a cloud-only collection**:</span></span>

1. <span data-ttu-id="84ef0-135">Hello felügyeleti portál lépjen a toohello RemoteApp lapra.</span><span class="sxs-lookup"><span data-stu-id="84ef0-135">In hello management portal, go toohello RemoteApp page.</span></span>
2. <span data-ttu-id="84ef0-136">Kattintson a **új > Gyorslétrehozás**.</span><span class="sxs-lookup"><span data-stu-id="84ef0-136">Click **New > Quick Create**.</span></span>
3. <span data-ttu-id="84ef0-137">Adja meg a gyűjtemény nevét, majd válassza ki a régiót.</span><span class="sxs-lookup"><span data-stu-id="84ef0-137">Enter a name for your collection, and select your region.</span></span>
4. <span data-ttu-id="84ef0-138">Válassza ki a megjeleníteni kívánt toouse – standard és alapszintű hello terv.</span><span class="sxs-lookup"><span data-stu-id="84ef0-138">Choose hello plan that you want toouse - standard or basic.</span></span>
5. <span data-ttu-id="84ef0-139">Válassza ki a sablon toouse hello ehhez a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="84ef0-139">Choose hello template toouse for this collection.</span></span> 
   
    <span data-ttu-id="84ef0-140">**Tipp:** RemoteApp az előfizetés részeként elérhető [sablonrendszerképek](remoteapp-images.md) , amelyik tartalmazza az Office 365 vagy Office 2013 (kipróbálás) a programok, egyes közzétett (például a Word) és mások toopublish kész.</span><span class="sxs-lookup"><span data-stu-id="84ef0-140">**Tip:** Your subscription for RemoteApp comes with [template images](remoteapp-images.md) that contain Office 365 or Office 2013 (for trial use) programs, some published (such as Word) and others ready toopublish.</span></span> <span data-ttu-id="84ef0-141">Is létrehozhat egy új [kép](remoteapp-imageoptions.md) és a felhőalapú gyűjtemény használatát.</span><span class="sxs-lookup"><span data-stu-id="84ef0-141">You can also create a new [image](remoteapp-imageoptions.md) and use it in your cloud collection.</span></span>
6. <span data-ttu-id="84ef0-142">Kattintson a **RemoteApp-gyűjtemény létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="84ef0-142">Click **Create RemoteApp collection**.</span></span>
   
    <span data-ttu-id="84ef0-143">**Fontos:** is eltarthat, too30 perc tooprovision a gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="84ef0-143">**Important:** It can take up too30 minutes tooprovision your collection.</span></span>

<span data-ttu-id="84ef0-144">Az Azure RemoteApp-gyűjtemény létrehozása után kattintson duplán a hello gyűjtemény hello nevét.</span><span class="sxs-lookup"><span data-stu-id="84ef0-144">After your Azure RemoteApp collection has been created, double-click hello name of hello collection.</span></span> <span data-ttu-id="84ef0-145">Amely megjelenik a hello **gyors üzembe helyezés** lap – Ez az, amikor befejezte a hello gyűjtésének konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="84ef0-145">That will bring up hello **Quick Start** page - this is where you finish configuring hello collection.</span></span>

<span data-ttu-id="84ef0-146">Használjon hello alábbi lépéseit toocreate egy **felhő + VNET gyűjtemény**:</span><span class="sxs-lookup"><span data-stu-id="84ef0-146">Use hello following steps toocreate a **cloud + VNET collection**:</span></span>

1. <span data-ttu-id="84ef0-147">Hello felügyeleti portál lépjen a toohello Azure RemoteApp lapra.</span><span class="sxs-lookup"><span data-stu-id="84ef0-147">In hello management portal, go toohello Azure RemoteApp page.</span></span>
2. <span data-ttu-id="84ef0-148">Kattintson a **új** > **hozzon létre virtuális hálózaton**.</span><span class="sxs-lookup"><span data-stu-id="84ef0-148">Click **New** > **Create with VNET**.</span></span>
3. <span data-ttu-id="84ef0-149">Adja meg a gyűjtemény nevét.</span><span class="sxs-lookup"><span data-stu-id="84ef0-149">Enter a name for your collection.</span></span>
4. <span data-ttu-id="84ef0-150">Válassza ki a megjeleníteni kívánt toouse – standard és alapszintű hello terv.</span><span class="sxs-lookup"><span data-stu-id="84ef0-150">Choose hello plan that you want toouse - standard or basic.</span></span>
5. <span data-ttu-id="84ef0-151">Válassza ki a virtuális hálózat már létrehozott hello.</span><span class="sxs-lookup"><span data-stu-id="84ef0-151">Choose hello VNET you already created.</span></span> <span data-ttu-id="84ef0-152">Nem tudja, hogyan toodo, amely?</span><span class="sxs-lookup"><span data-stu-id="84ef0-152">Don't know how toodo that?</span></span> <span data-ttu-id="84ef0-153">Most hello lépésekre a hello [hibrid](remoteapp-create-hybrid-deployment.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="84ef0-153">For now, hello steps are in hello [hybrid](remoteapp-create-hybrid-deployment.md) topic.</span></span>
6. <span data-ttu-id="84ef0-154">Döntse el toojoin a gyűjtemény tooyour tartomány.</span><span class="sxs-lookup"><span data-stu-id="84ef0-154">Decide whether you want toojoin your collection tooyour domain.</span></span> <span data-ttu-id="84ef0-155">Ha igen, szüksége lesz toouse AD Connect toointegrate az Azure AD és az Active Directory-környezetbe.</span><span class="sxs-lookup"><span data-stu-id="84ef0-155">If yes, you'll need toouse AD Connect toointegrate Azure AD and your Active Directory environment.</span></span> <span data-ttu-id="84ef0-156">Amely is tartalmazza az alábbi **2. lépés**.</span><span class="sxs-lookup"><span data-stu-id="84ef0-156">That's covered in below in **Step 2**.</span></span>
7. <span data-ttu-id="84ef0-157">Kattintson a **RemoteApp-gyűjtemény létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="84ef0-157">Click **Create RemoteApp collection**.</span></span>

## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a><span data-ttu-id="84ef0-158">2. lépés: (Opcionális) Active Directory címtár-szinkronizálás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84ef0-158">Step 2: Configure Active Directory directory synchronization (optional)</span></span>
<span data-ttu-id="84ef0-159">Ha azt szeretné, hogy toouse Active Directory, Azure RemoteApp címtár-szinkronizálás Azure Active Directory és a helyszíni Active Directory toosynchronize felhasználók, partnerek és jelszavak tooyour Azure Active Directory-bérlő között van szükség.</span><span class="sxs-lookup"><span data-stu-id="84ef0-159">If you want toouse Active Directory, Azure RemoteApp requires directory synchronization between Azure Active Directory and your on-premises Active Directory toosynchronize users,  contacts, and passwords tooyour Azure Active Directory tenant.</span></span> <span data-ttu-id="84ef0-160">Lásd: [Active Directory konfigurálása az Azure RemoteApp](remoteapp-ad.md) tervezési információkat.</span><span class="sxs-lookup"><span data-stu-id="84ef0-160">See [Configuring Active Directory for Azure RemoteApp](remoteapp-ad.md) for planning information.</span></span> <span data-ttu-id="84ef0-161">Is elvégezheti közvetlenül túl[AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) információt.</span><span class="sxs-lookup"><span data-stu-id="84ef0-161">You can also go directly too[AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) for information.</span></span>

## <a name="step-3-publish-apps"></a><span data-ttu-id="84ef0-162">3. lépés: Az alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="84ef0-162">Step 3: Publish apps</span></span>
<span data-ttu-id="84ef0-163">Egy Azure RemoteApp alkalmazás hello alkalmazás vagy a program, hogy megadja a tooyour felhasználók.</span><span class="sxs-lookup"><span data-stu-id="84ef0-163">An Azure RemoteApp app is hello app or program that you provide tooyour users.</span></span> <span data-ttu-id="84ef0-164">Hello gyűjtemény feltöltött hello sablonlemezkép található.</span><span class="sxs-lookup"><span data-stu-id="84ef0-164">It is located in hello template image you uploaded for hello collection.</span></span> <span data-ttu-id="84ef0-165">Amikor egy felhasználó egy alkalmazást, a hello app hozzáfér-e a helyi környezetben toorun megjelenik, de valóban egy virtuális gép az Azure-ban futó.</span><span class="sxs-lookup"><span data-stu-id="84ef0-165">When a user accesses an app, hello app appears toorun in their local environment, but it is really running in a virtual machine in Azure.</span></span> 

<span data-ttu-id="84ef0-166">Mielőtt a felhasználók úgy férhetnek hozzá az alkalmazásokhoz, toopublish kell őket – alkalmazások megadható, hogy a felhasználók férhetnek hozzá a hello alkalmazások közzététele hello távoli asztali ügyfél.</span><span class="sxs-lookup"><span data-stu-id="84ef0-166">Before your users can access apps, you need toopublish them – publishing apps lets your users access hello apps through hello Remote Desktop client.</span></span>

<span data-ttu-id="84ef0-167">Közzéteheti több alkalmazások tooyour Azure RemoteApp-gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="84ef0-167">You can publish multiple apps tooyour Azure RemoteApp collection.</span></span> <span data-ttu-id="84ef0-168">Hello közzétételi lapon kattintson **közzététel** tooadd programot.</span><span class="sxs-lookup"><span data-stu-id="84ef0-168">From hello publishing page, click **Publish** tooadd a program.</span></span> <span data-ttu-id="84ef0-169">Vagy közzéteheti a hello **Start** menüjéből hello sablonlemezkép vagy hello alkalmazás hello sablon rendszerképre hello elérési út megadásával.</span><span class="sxs-lookup"><span data-stu-id="84ef0-169">You can either publish from hello **Start** menu of hello template image or by specifying hello path on hello template image for hello app.</span></span> <span data-ttu-id="84ef0-170">Ha tooadd választhat hello **Start** menüben válassza ki a hello app toopublish.</span><span class="sxs-lookup"><span data-stu-id="84ef0-170">If you choose tooadd from hello **Start** menu, choose hello app toopublish.</span></span> <span data-ttu-id="84ef0-171">Ha úgy dönt, hogy tooprovide hello elérési toohello app, nevezze el a hello alkalmazást, és hello elérési toowhere hello sablonlemezkép van telepítve.</span><span class="sxs-lookup"><span data-stu-id="84ef0-171">If you choose tooprovide hello path toohello app, provide a name for hello app and hello path toowhere it is installed on hello template image.</span></span>

## <a name="step-4-configure-user-access"></a><span data-ttu-id="84ef0-172">4. lépés: A felhasználói hozzáférés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84ef0-172">Step 4: Configure user access</span></span>
<span data-ttu-id="84ef0-173">Most, hogy a gyűjtemény hozott létre, szüksége tooadd hello felhasználók megjeleníteni kívánt toobe képes toouse a távoli erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="84ef0-173">Now that you have created your collection, you need tooadd hello users that you want toobe able toouse your remote resources.</span></span> <span data-ttu-id="84ef0-174">Active Directory használata, ha hello meg az hello az előfizetéshez tartozó hozzáférési tooneed tooexist az Active Directory-bérlő hello adni használó felhasználók toocreate ebben a gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="84ef0-174">If you are using Active Directory, hello users that you provide access tooneed tooexist in hello Active Directory tenant associated with hello subscription you used toocreate this collection.</span></span>

1. <span data-ttu-id="84ef0-175">Hello gyors kezdés lapon kattintson **felhasználói hozzáférés konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="84ef0-175">From hello Quick Start page, click **Configure user access**.</span></span> 
2. <span data-ttu-id="84ef0-176">Adja meg a hello munkahelyi fiókjával (az Active Directory) vagy Microsoft-fiók, amelyet toogrant hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="84ef0-176">Enter hello work account (from Active Directory) or Microsoft account that you want toogrant access for.</span></span>
   
   <span data-ttu-id="84ef0-177">**Megjegyzések:**</span><span class="sxs-lookup"><span data-stu-id="84ef0-177">**Notes:**</span></span> 
   
   <span data-ttu-id="84ef0-178">Győződjön meg arról, hogy használja-e hello  *user@domain.com*  formátumban.</span><span class="sxs-lookup"><span data-stu-id="84ef0-178">Make sure that you use hello *user@domain.com* format.</span></span>
   
   <span data-ttu-id="84ef0-179">Ha használ Office 365 ProPlus a gyűjteményben, a hello Active Directory identitások a felhasználók számára kell használnia.</span><span class="sxs-lookup"><span data-stu-id="84ef0-179">If you are using Office 365 ProPlus in your collection, you must use hello Active Directory identities for your users.</span></span> <span data-ttu-id="84ef0-180">Ezzel a megoldással licencelési ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="84ef0-180">This helps validate licensing.</span></span> 
3. <span data-ttu-id="84ef0-181">Hello felhasználók ellenőrzését követően kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="84ef0-181">After hello users are validated, click **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84ef0-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84ef0-182">Next steps</span></span>
<span data-ttu-id="84ef0-183">Ennyi az egész-sikeresen létrehozta és telepítve az Azure RemoteApp felhőalapú gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="84ef0-183">That's it - you have successfully created and deployed your Azure RemoteApp cloud collection.</span></span> <span data-ttu-id="84ef0-184">következő lépés hello toohave töltse le és hello távoli asztali ügyfél telepítése a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="84ef0-184">hello next step is toohave your users download and install hello Remote Desktop client.</span></span> <span data-ttu-id="84ef0-185">Hello Azure RemoteApp gyors kezdés lapon található hello ügyfél hello URL-címe.</span><span class="sxs-lookup"><span data-stu-id="84ef0-185">You can find hello URL for hello client on hello Azure RemoteApp Quick Start page.</span></span> <span data-ttu-id="84ef0-186">Ezt követően, hogy a felhasználók a hello ügyfél, és közzétette hello alkalmazások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="84ef0-186">Then, have users log into hello client and access hello apps you published.</span></span>

### <a name="help-us-help-you"></a><span data-ttu-id="84ef0-187">Segítsen nekünk, hogy segítsünk</span><span class="sxs-lookup"><span data-stu-id="84ef0-187">Help us help you</span></span>
<span data-ttu-id="84ef0-188">Tudta, hogy a hozzáadása toorating ebben a cikkben és a hozzászólások írása az alábbi tehet módosítások toohello cikkbe?</span><span class="sxs-lookup"><span data-stu-id="84ef0-188">Did you know that in addition toorating this article and making comments down below, you can make changes toohello article itself?</span></span> <span data-ttu-id="84ef0-189">Valami hiányzik?</span><span class="sxs-lookup"><span data-stu-id="84ef0-189">Something missing?</span></span> <span data-ttu-id="84ef0-190">Valami nem működik?</span><span class="sxs-lookup"><span data-stu-id="84ef0-190">Something wrong?</span></span> <span data-ttu-id="84ef0-191">Írtam valami olyat, ami nem egyértelmű?</span><span class="sxs-lookup"><span data-stu-id="84ef0-191">Did I write something that's just confusing?</span></span> <span data-ttu-id="84ef0-192">Görgessen fel, és kattintson a **Szerkesztés a Githubon** toomake módosítások - ezek hozzánk kerülnek jóváhagyásra toous, és majd, miután jóváhagytuk őket, itt fognak megjelenni a módosításokat és fejlesztéseket azonnal itt.</span><span class="sxs-lookup"><span data-stu-id="84ef0-192">Scroll up and click **Edit on GitHub** toomake changes - those will come toous for review, and then, once we sign off on them, you'll see your changes and improvements right here.</span></span>
