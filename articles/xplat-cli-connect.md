---
title: a tooAzure a hello CLI aaaLog |} Microsoft Docs
description: "Azure-előfizetés tooyour csatlakoztatja a hello Azure parancssori felület (CLI) Mac, Linux és Windows rendszerekhez"
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
ms.openlocfilehash: 42682c00c8dea78b2c624e640379716d1d4d7a2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-in-tooazure-from-hello-azure-cli"></a><span data-ttu-id="7bb05-103">Jelentkezzen be az Azure CLI hello tooAzure</span><span class="sxs-lookup"><span data-stu-id="7bb05-103">Log in tooAzure from hello Azure CLI</span></span>
<span data-ttu-id="7bb05-104">hello Azure parancssori felület nyílt forráskódú, platformok közötti parancsokat az Azure-erőforrások használata.</span><span class="sxs-lookup"><span data-stu-id="7bb05-104">hello Azure CLI is a set of open-source, cross-platform commands for working with Azure resources.</span></span> <span data-ttu-id="7bb05-105">Ez a cikk ismerteti hello különböző módokon tooprovide az Azure-fiók hitelesítő adatait tooconnect hello Azure CLI tooyour Azure-előfizetés:</span><span class="sxs-lookup"><span data-stu-id="7bb05-105">This article describes hello different ways tooprovide your Azure account credentials tooconnect hello Azure CLI tooyour Azure subscription:</span></span>

* <span data-ttu-id="7bb05-106">Futtassa a hello `azure login` CLI parancs tooauthenticate Azure Active Directoryn keresztül.</span><span class="sxs-lookup"><span data-stu-id="7bb05-106">Run hello `azure login` CLI command tooauthenticate through Azure Active Directory.</span></span> <span data-ttu-id="7bb05-107">A metódus hozzáférést tud biztosítani, hogy mindkét tooCLI parancsok [módok parancs](#cli-command-modes).</span><span class="sxs-lookup"><span data-stu-id="7bb05-107">This method gives you access tooCLI commands in both [command modes](#cli-command-modes).</span></span> <span data-ttu-id="7bb05-108">További beállítások nélkül hello parancs futtatásakor `azure login` webes portálon keresztül interaktív bejelentkezés toocontinue kéri.</span><span class="sxs-lookup"><span data-stu-id="7bb05-108">When you run hello command without additional options, `azure login` prompts you toocontinue logging in interactively through a web portal.</span></span> <span data-ttu-id="7bb05-109">A további `azure login` beállítások parancsot, tekintse meg a cikket, vagy a típus hello forgatókönyvek `azure login --help`.</span><span class="sxs-lookup"><span data-stu-id="7bb05-109">For additional `azure login` command options, see hello scenarios in this article, or type `azure login --help`.</span></span>
* <span data-ttu-id="7bb05-110">Ha csak toouse Azure szolgáltatásfelügyelet módban parancssori felület parancsait (nem ajánlott az új telepítések esetén), töltse le, és telepítse a közzétételi beállítások fájlja a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7bb05-110">If you only need toouse Azure Service Management mode CLI commands (not recommended for most new deployments), you can download and install a publish settings file on your computer.</span></span>

<span data-ttu-id="7bb05-111">Ha még nem telepítette a hello CLI, lásd: [telepítés hello Azure CLI](cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="7bb05-111">If you haven't already installed hello CLI, see [Install hello Azure CLI](cli-install-nodejs.md).</span></span> <span data-ttu-id="7bb05-112">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="7bb05-112">If you don't have an Azure subscription, you can create a [free account](http://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

<span data-ttu-id="7bb05-113">Másik fiók identitások és az Azure-előfizetések kapcsolatos háttér, lásd: [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).</span><span class="sxs-lookup"><span data-stu-id="7bb05-113">For background about different account identities and Azure subscriptions, see [How Azure subscriptions are associated with Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).</span></span>

## <a name="scenario-1-azure-login-with-interactive-login"></a><span data-ttu-id="7bb05-114">1. forgatókönyv: azure bejelentkezési interaktív bejelentkezéskor</span><span class="sxs-lookup"><span data-stu-id="7bb05-114">Scenario 1: azure login with interactive login</span></span>
<span data-ttu-id="7bb05-115">Az egyes fiókok hello CLI használatához toorun `azure login` és folytassa a bejelentkezési folyamat hello webböngészőn keresztül egy webes portál nevezett folyamat *interaktív bejelentkezési*.</span><span class="sxs-lookup"><span data-stu-id="7bb05-115">With certain accounts, hello CLI requires you toorun `azure login` and then continue hello login process with a web browser through a web portal, a process called *interactive login*.</span></span> <span data-ttu-id="7bb05-116">A leggyakoribb oka, hogy munkahelyi vagy iskolai fiókkal (más néven egy *szervezeti fiók*), amely toorequire többtényezős hitelesítés be van állítva.</span><span class="sxs-lookup"><span data-stu-id="7bb05-116">A common reason is when you have a work or school account (also called an *organizational account*) that is set up toorequire multifactor authentication.</span></span> <span data-ttu-id="7bb05-117">Is használhat, interaktív bejelentkezés Microsoft-fiókjával, ha azt szeretné, hogy toouse erőforrás-kezelő módban parancsok.</span><span class="sxs-lookup"><span data-stu-id="7bb05-117">Also use interactive login with your Microsoft account, when you want toouse Resource Manager mode commands.</span></span>

<span data-ttu-id="7bb05-118">Interaktív bejelentkezési adatai könnyen: típus `azure login` --nélkül bármely beállításai – ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="7bb05-118">Interactive login is easy: type `azure login` -- without any options -- as shown in hello following example:</span></span>

```
azure login
```                                                                                             

<span data-ttu-id="7bb05-119">hello eredmény az alábbihoz hasonló hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="7bb05-119">hello output appears something like hello following:</span></span>

```         
info:    Executing command login
info:    toosign in, use a web browser tooopen hello page http://aka.ms/devicelogin. Enter hello code XXXXXXXXX tooauthenticate.
```
<span data-ttu-id="7bb05-120">Másolja a hello parancs kimenetében tooyou kínált hello kódot, és nyissa meg a böngésző toohttp://aka.ms/devicelogin vagy más lap, ha meg van adva.</span><span class="sxs-lookup"><span data-stu-id="7bb05-120">Copy hello code offered tooyou in hello command output, and open a browser toohttp://aka.ms/devicelogin, or other page if specified.</span></span> <span data-ttu-id="7bb05-121">(Megnyithat egy böngészőt a hello ugyanarra a számítógépre, vagy egy másik számítógépen vagy eszközön.) Hello kód megadása, és ezután felszólító tooenter hello felhasználónév és jelszó hello identitás toouse keresi.</span><span class="sxs-lookup"><span data-stu-id="7bb05-121">(You can open a browser on hello same computer, or on a different computer or device.) Enter hello code, and then you are prompted tooenter hello username and password for hello identity you want toouse.</span></span> <span data-ttu-id="7bb05-122">A folyamat befejezése után a hello parancs-rendszerhéj hello bejelentkezési befejezése.</span><span class="sxs-lookup"><span data-stu-id="7bb05-122">When that process completes, hello command shell completes hello login.</span></span> <span data-ttu-id="7bb05-123">Ez lehet hasonlót:</span><span class="sxs-lookup"><span data-stu-id="7bb05-123">It might look something like:</span></span>

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> <span data-ttu-id="7bb05-124">Az interaktív bejelentkezés hitelesítés és engedélyezés történik az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="7bb05-124">With interactive login, authentication and authorization are performed using Azure Active Directory.</span></span> <span data-ttu-id="7bb05-125">Ha egy Microsoft-fiók identitást használja, a hello bejelentkezési folyamat az Azure Active Directory alapértelmezett tartomány éri el.</span><span class="sxs-lookup"><span data-stu-id="7bb05-125">If you use a Microsoft account identity, hello login process accesses your Azure Active Directory default domain.</span></span> <span data-ttu-id="7bb05-126">(Ha volt egy ingyenes Azure-fiókot, Azure Active Directory automatikusan létrejön egy alapértelmezett tartományt a fiókhoz.)</span><span class="sxs-lookup"><span data-stu-id="7bb05-126">(If you signed up for a free Azure account, Azure Active Directory automatically created a default domain for your account.)</span></span>
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a><span data-ttu-id="7bb05-127">2. forgatókönyv: azure jelentkezzen be egy felhasználónevet és jelszót</span><span class="sxs-lookup"><span data-stu-id="7bb05-127">Scenario 2: azure login with a username and password</span></span>
<span data-ttu-id="7bb05-128">Használjon hello `azure login` hello felhasználónév parancsot (`-u`) paraméter tooauthenticate, ha azt szeretné, hogy toouse munkahelyi vagy iskolai fiókkal, amely nem igényel a többtényezős hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="7bb05-128">Use hello `azure login` command with hello username (`-u`) parameter tooauthenticate when you want toouse a work or school account that doesn't require multifactor authentication.</span></span> <span data-ttu-id="7bb05-129">Parancssorból hello hello jelszót kéri (vagy opcionálisan átadhatók hello jelszó hello további paraméterként `azure login` parancs).</span><span class="sxs-lookup"><span data-stu-id="7bb05-129">You are prompted at hello command line for hello password (or you can optionally pass hello password as an additional parameter of hello `azure login` command).</span></span> <span data-ttu-id="7bb05-130">hello alábbi példa továbbítja hello szervezeti fiók felhasználóneve:</span><span class="sxs-lookup"><span data-stu-id="7bb05-130">hello following example passes hello username of an organizational account:</span></span>

    azure login -u myUserName@contoso.onmicrosoft.com

<span data-ttu-id="7bb05-131">Rendszer már tooenter kéri a jelszót:</span><span class="sxs-lookup"><span data-stu-id="7bb05-131">You are then prompted tooenter your password:</span></span>

    info:    Executing command login
    Password: *********

<span data-ttu-id="7bb05-132">hello bejelentkezési folyamat majd befejeződik.</span><span class="sxs-lookup"><span data-stu-id="7bb05-132">hello login process then completes.</span></span>

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

<span data-ttu-id="7bb05-133">Ha ezeket a hitelesítő adatokat az első alkalommal bejelentkezik, tooverify megkérdezi, hogy kívánja toocache egy hitelesítési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="7bb05-133">If this is your first time logging in with these credentials, you are asked tooverify that you wish toocache an authentication token.</span></span> <span data-ttu-id="7bb05-134">Ez a kérdés is akkor fordul elő, ha korábban már használt hello `azure logout` (hello cikk későbbi részében leírt) parancsot.</span><span class="sxs-lookup"><span data-stu-id="7bb05-134">This prompt also occurs if you previously used hello `azure logout` command (described later in hello article).</span></span> <span data-ttu-id="7bb05-135">toobypass automation-forgatókönyvek esetén a parancssor futtatása `azure login` a hello `-q` paraméter.</span><span class="sxs-lookup"><span data-stu-id="7bb05-135">toobypass this prompt for automation scenarios, run `azure login` with hello `-q` parameter.</span></span>

## <a name="scenario-3-azure-login-with-a-service-principal"></a><span data-ttu-id="7bb05-136">3. forgatókönyv: azure jelentkezzen be egy egyszerű szolgáltatásnév</span><span class="sxs-lookup"><span data-stu-id="7bb05-136">Scenario 3: azure login with a service principal</span></span>
<span data-ttu-id="7bb05-137">Ha az Active Directory-alkalmazás szolgáltatásnevet létrehozni, és hello szolgáltatás egyszerű megfelelő jogosultságokkal rendelkezik az előfizetés, használhatja a hello `azure login` parancs tooauthenticate hello szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="7bb05-137">If you create a service principal for an Active Directory application, and hello service principal has permissions on your subscription, you can use hello `azure login` command tooauthenticate hello service principal.</span></span> <span data-ttu-id="7bb05-138">A forgatókönyvtől függően biztosíthatja hello hitelesítő adatait egyszerű hello szolgáltatást a hello explicit paraméterekként `azure login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="7bb05-138">Depending on your scenario, you could provide hello credentials of hello service principal as explicit parameters of hello `azure login` command.</span></span> <span data-ttu-id="7bb05-139">Például hello parancsban továbbítja hello egyszerű szolgáltatásnév és az Active Directory-bérlőazonosító beszerzése:</span><span class="sxs-lookup"><span data-stu-id="7bb05-139">For example, hello following command passes hello service principal name and Active Directory tenant ID:</span></span>

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

<span data-ttu-id="7bb05-140">Ön nem, akkor a kért tooprovide hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="7bb05-140">You are then prompted tooprovide hello password.</span></span> <span data-ttu-id="7bb05-141">A parancssori felület parancsprogram vagy alkalmazás kód hello hitelesítő, vagy nem interaktív használni egy tanúsítvány tooauthenticate hello szolgáltatás egyszerű automatizálási esetekben is.</span><span class="sxs-lookup"><span data-stu-id="7bb05-141">You can also provide hello credentials through a CLI script or application code, or use a certificate tooauthenticate hello service principal non-interactively for automation scenarios.</span></span> <span data-ttu-id="7bb05-142">Részletek és példák: [hitelesítéséhez az Azure Resource Manager szolgáltatásnevet](resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="7bb05-142">For details and examples, see [Authenticating a service principal with Azure Resource Manager](resource-group-authenticate-service-principal-cli.md).</span></span>

## <a name="scenario-4-use-a-publish-settings-file"></a><span data-ttu-id="7bb05-143">4. forgatókönyv: Használja a közzétételi beállítások fájlja</span><span class="sxs-lookup"><span data-stu-id="7bb05-143">Scenario 4: Use a publish settings file</span></span>
<span data-ttu-id="7bb05-144">Ha csak toouse hello Azure szolgáltatásfelügyelet módban parancssori felület parancsait (például toodeploy Azure virtuális gépek hello klasszikus üzembe helyezési modellel), akkor csatlakozhatnak a közzétételi beállítások fájlja.</span><span class="sxs-lookup"><span data-stu-id="7bb05-144">If you only need toouse hello Azure Service Management mode CLI commands (for example, toodeploy Azure VMs in hello classic deployment model), you can connect using a publish settings file.</span></span> <span data-ttu-id="7bb05-145">Ez a módszer tanúsítványt telepít a helyi számítógépen, amely lehetővé teszi tooperform kapcsolatos felügyeleti feladatok mindaddig, amíg hello előfizetés és a hello tanúsítvány érvényesek.</span><span class="sxs-lookup"><span data-stu-id="7bb05-145">This method installs a certificate on your local computer that allows you tooperform management tasks for as long as hello subscription and hello certificate are valid.</span></span>

* <span data-ttu-id="7bb05-146">**toodownload hello közzététele beállításfájl** a fiókot, győződjön meg arról, hogy CLI szolgáltatásfelügyelet módban van, írja be a hello `azure config mode asm`.</span><span class="sxs-lookup"><span data-stu-id="7bb05-146">**toodownload hello publish settings file** for your account, ensure that hello CLI is in Service Management mode by typing `azure config mode asm`.</span></span> <span data-ttu-id="7bb05-147">Ezután futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="7bb05-147">Then run hello following command:</span></span>

        azure account download

<span data-ttu-id="7bb05-148">Ez megnyitja az alapértelmezett böngészőt, és kéri a toohello toosign [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="7bb05-148">This opens your default browser and prompts you toosign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="7bb05-149">Miután bejelentkezik, egy `.publishsettings` fájl letöltését.</span><span class="sxs-lookup"><span data-stu-id="7bb05-149">After you sign in, a `.publishsettings` file downloads.</span></span> <span data-ttu-id="7bb05-150">Jegyezze fel a letöltött fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="7bb05-150">Make note of where this file is saved.</span></span>

> [!NOTE]
> <span data-ttu-id="7bb05-151">Ha a fiók több Azure Active Directory-bérlő tartozik, esetleg felszólító tooselect, amely Active Directory, a közzétételi beállítások kívánja toodownload fájlt.</span><span class="sxs-lookup"><span data-stu-id="7bb05-151">If your account is associated with multiple Azure Active Directory tenants, you may be prompted tooselect which Active Directory you wish toodownload a publish settings file for.</span></span>
>
>

<span data-ttu-id="7bb05-152">A kijelölt hello letöltési oldal használatával, vagy a klasszikus Azure portálon hello felkeresésével hello kijelölt Active Directory lesz hello alapértelmezett hello klasszikus portál és a letöltési oldal használják.</span><span class="sxs-lookup"><span data-stu-id="7bb05-152">Once selected using hello download page, or by visiting hello Azure classic portal, hello selected Active Directory becomes hello default used by hello classic portal and download page.</span></span> <span data-ttu-id="7bb05-153">Ha alapértelmezett lett létrehozva, akkor tekintse meg a hello szöveg "**ide tooreturn toohello kiválasztása lapon**" hello letöltési oldala hello tetején.</span><span class="sxs-lookup"><span data-stu-id="7bb05-153">Once a default has been established, you see hello text '**click here tooreturn toohello selection page**' at hello top of hello download page.</span></span> <span data-ttu-id="7bb05-154">A megadott hivatkozás tooreturn toohello kiválasztása lapon hello használja.</span><span class="sxs-lookup"><span data-stu-id="7bb05-154">Use hello provided link tooreturn toohello selection page.</span></span>

* <span data-ttu-id="7bb05-155">**tooimport hello közzététele beállításfájl**- ben futtassa hello következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="7bb05-155">**tooimport hello publish settings file**, run hello following command:</span></span>

        azure account import <path tooyour .publishsettings file>

> [!IMPORTANT]
> <span data-ttu-id="7bb05-156">Importálás után a közzétételi beállítások, törölnie kell a hello `.publishsettings` fájlt.</span><span class="sxs-lookup"><span data-stu-id="7bb05-156">After importing your publish settings, you should delete hello `.publishsettings` file.</span></span> <span data-ttu-id="7bb05-157">Hello Azure CLI által már nem szükséges, és biztonsági kockázatot jelent, akkor lehet, hogy használt toogain hozzáférés tooyour előfizetés.</span><span class="sxs-lookup"><span data-stu-id="7bb05-157">It is no longer required by hello Azure CLI and presents a security risk as it could be used toogain access tooyour subscription.</span></span>
>
>

## <a name="cli-command-modes"></a><span data-ttu-id="7bb05-158">Parancssori felület üzemmódok</span><span class="sxs-lookup"><span data-stu-id="7bb05-158">CLI command modes</span></span>
<span data-ttu-id="7bb05-159">hello Azure CLI használata az Azure-erőforrások, különböző parancs készletekkel két üzemmódok ismerteti:</span><span class="sxs-lookup"><span data-stu-id="7bb05-159">hello Azure CLI provides two command modes for working with Azure resources, with different command sets:</span></span>

* <span data-ttu-id="7bb05-160">**Resource Manager módra** – az Azure-erőforrások hello Resource Manager üzembe helyezési modellel működik.</span><span class="sxs-lookup"><span data-stu-id="7bb05-160">**Resource Manager mode** - for working with Azure resources in hello Resource Manager deployment model.</span></span> <span data-ttu-id="7bb05-161">tooset ebben a módban futtassa `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="7bb05-161">tooset this mode, run `azure config mode arm`.</span></span>
* <span data-ttu-id="7bb05-162">**Szolgáltatásfelügyeleti módban** – az Azure-erőforrások hello klasszikus üzembe helyezési modellel működik.</span><span class="sxs-lookup"><span data-stu-id="7bb05-162">**Service Management mode** - for working with Azure resources in hello classic deployment model.</span></span> <span data-ttu-id="7bb05-163">tooset ebben a módban futtassa `azure config mode asm`.</span><span class="sxs-lookup"><span data-stu-id="7bb05-163">tooset this mode, run `azure config mode asm`.</span></span>

<span data-ttu-id="7bb05-164">Első telepítésekor hello jelenlegi kiadásában hello CLI erőforrás-kezelő módban van.</span><span class="sxs-lookup"><span data-stu-id="7bb05-164">When first installed, hello current release of hello CLI is in Resource Manager mode.</span></span>

> [!NOTE]
> <span data-ttu-id="7bb05-165">hello erőforrás-kezelő és a szolgáltatás felügyeleti üzemmód, egymást kölcsönösen kizáró.</span><span class="sxs-lookup"><span data-stu-id="7bb05-165">hello Resource Manager mode and Service Management mode are mutually exclusive.</span></span> <span data-ttu-id="7bb05-166">Ez azt jelenti, hogy egy módban létrehozott erőforrások nem felügyelhetők a hello más módot.</span><span class="sxs-lookup"><span data-stu-id="7bb05-166">That is, resources created in one mode cannot be managed from hello other mode.</span></span>
>
>

## <a name="multiple-subscriptions"></a><span data-ttu-id="7bb05-167">Több előfizetés</span><span class="sxs-lookup"><span data-stu-id="7bb05-167">Multiple subscriptions</span></span>
<span data-ttu-id="7bb05-168">Ha több Azure-előfizetéssel rendelkezik, csatlakozás tooAzure engedélyezi a hozzáférést a hitelesítő adatok társított tooall előfizetések.</span><span class="sxs-lookup"><span data-stu-id="7bb05-168">If you have multiple Azure subscriptions, connecting tooAzure grants access tooall subscriptions associated with your credentials.</span></span> <span data-ttu-id="7bb05-169">Egy előfizetés hello alapértelmezett beállítást választotta, és hello Azure CLI által használt műveletek végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="7bb05-169">One subscription is selected as hello default, and used by hello Azure CLI when performing operations.</span></span> <span data-ttu-id="7bb05-170">Megtekintheti a hello előfizetések, beleértve a hello alapértelmezett előfizetésben, hello segítségével `azure account list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="7bb05-170">You can view hello subscriptions, including hello current default subscription, using hello `azure account list` command.</span></span> <span data-ttu-id="7bb05-171">Ez a parancs visszaadja az adatokat hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="7bb05-171">This command returns information similar toohello following:</span></span>

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

<span data-ttu-id="7bb05-172">A listát megelőző hello, hello **aktuális** oszlopban látható, hogy az alapértelmezett előfizetésben hello Azure-al-1.</span><span class="sxs-lookup"><span data-stu-id="7bb05-172">In hello preceding list, hello **Current** column indicates hello current default subscription as Azure-sub-1.</span></span> <span data-ttu-id="7bb05-173">toochange hello alapértelmezett előfizetés, használjon hello `azure account set` parancsot, és adja meg, hogy kívánja-e toobe hello alapértelmezett hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="7bb05-173">toochange hello default subscription, use hello `azure account set` command, and specify hello subscription that you wish toobe hello default.</span></span> <span data-ttu-id="7bb05-174">Példa:</span><span class="sxs-lookup"><span data-stu-id="7bb05-174">For example:</span></span>

    azure account set Azure-sub-2

<span data-ttu-id="7bb05-175">Hello alapértelmezett előfizetés tooAzure-al-2 értékre változik.</span><span class="sxs-lookup"><span data-stu-id="7bb05-175">This changes hello default subscription tooAzure-sub-2.</span></span>

> [!NOTE]
> <span data-ttu-id="7bb05-176">Hello alapértelmezett előfizetés módosítása azonnal érvénybe lép, és globális módosítva; új Azure parancssori felület parancsait, hogy futtatja azokat hello azonos parancssori vagy egy másik példány, hello új alapértelmezett előfizetés használatára.</span><span class="sxs-lookup"><span data-stu-id="7bb05-176">Changing hello default subscription takes effect immediately, and is a global change; new Azure CLI commands, whether you run them from hello same command-line instance or a different instance, use hello new default subscription.</span></span>
>
>

<span data-ttu-id="7bb05-177">Ha kívánja toouse egy nem alapértelmezett előfizetés hello Azure CLI-t, de nem szeretné toochange hello aktuális alapértelmezett, használhatja a hello `--subscription` hello parancs lehetőséget, majd adjon meg hello hello előfizetés kívánja toouse hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="7bb05-177">If you wish toouse a non-default subscription with hello Azure CLI, but don't want toochange hello current default, you can use hello `--subscription` option for hello command and provide hello name of hello subscription you wish toouse for hello operation.</span></span>

<span data-ttu-id="7bb05-178">Ha csatlakoztatott tooyour Azure-előfizetéssel, megkezdheti a hello Azure CLI parancsok toowork használata Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="7bb05-178">Once you are connected tooyour Azure subscription, you can start using hello Azure CLI commands toowork with Azure resources.</span></span>

## <a name="storage-of-cli-settings"></a><span data-ttu-id="7bb05-179">Parancssori beállítások tárolására</span><span class="sxs-lookup"><span data-stu-id="7bb05-179">Storage of CLI settings</span></span>
<span data-ttu-id="7bb05-180">E bejelentkezés hello `azure login` parancs vagy importálása a közzétételi beállítások, a CLI-profil és a naplók tárolja egy `.azure` könyvtárban található a `user` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="7bb05-180">Whether you log in with hello `azure login` command or import publish settings, your CLI profile and logs are stored in a `.azure` directory located in your `user` directory.</span></span> <span data-ttu-id="7bb05-181">A `user` könyvtár az operációs rendszer által védett.</span><span class="sxs-lookup"><span data-stu-id="7bb05-181">Your `user` directory is protected by your operating system.</span></span> <span data-ttu-id="7bb05-182">Javasoljuk azonban, hogy a további lépéseket tooencrypt el a `user` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="7bb05-182">However, we recommend that you take additional steps tooencrypt your `user` directory.</span></span> <span data-ttu-id="7bb05-183">Ehhez a következő módokon hello:</span><span class="sxs-lookup"><span data-stu-id="7bb05-183">You can do so in hello following ways:</span></span>

* <span data-ttu-id="7bb05-184">A Windows hello directory tulajdonságainak módosítása vagy a BitLocker használja.</span><span class="sxs-lookup"><span data-stu-id="7bb05-184">On Windows, modify hello directory properties or use BitLocker.</span></span>
* <span data-ttu-id="7bb05-185">A Mac kapcsolja be a FileVault hello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="7bb05-185">On Mac, turn on FileVault for hello directory.</span></span>
* <span data-ttu-id="7bb05-186">Ubuntu, a szolgáltatással hello titkosított otthoni könyvtár.</span><span class="sxs-lookup"><span data-stu-id="7bb05-186">On Ubuntu, use hello Encrypted Home directory feature.</span></span> <span data-ttu-id="7bb05-187">Más Linux terjesztésekről hasonló funkciókat kínál.</span><span class="sxs-lookup"><span data-stu-id="7bb05-187">Other Linux distributions offer similar features.</span></span>

## <a name="logging-out"></a><span data-ttu-id="7bb05-188">Kijelentkezés</span><span class="sxs-lookup"><span data-stu-id="7bb05-188">Logging out</span></span>
<span data-ttu-id="7bb05-189">toolog ki, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="7bb05-189">toolog out, use hello following command:</span></span>

    azure logout -u <username>

<span data-ttu-id="7bb05-190">Ha hello előfizetések társított hello fiók csak hitelesítése az Active Directory kijelentkezés helyi hello-profil törlése hello előfizetés információit.</span><span class="sxs-lookup"><span data-stu-id="7bb05-190">If hello subscriptions associated with hello account are only authenticated with Active Directory, logging out deletes hello subscription information from hello local profile.</span></span> <span data-ttu-id="7bb05-191">Azonban a közzétételi beállítások fájlja is importált hello elő, ha kijelentkezés csak törli az Active Directory kapcsolatos információkat hello helyi profilt.</span><span class="sxs-lookup"><span data-stu-id="7bb05-191">However, if a publish settings file was also imported for hello subscriptions, logging out only deletes Active Directory related information from hello local profile.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7bb05-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7bb05-192">Next steps</span></span>
* <span data-ttu-id="7bb05-193">Azure parancssori felület parancsait toouse, lásd: [Azure parancssori felület parancsait erőforrás-kezelő módban](virtual-machines/azure-cli-arm-commands.md) és [Azure parancssori felület parancsait szolgáltatásfelügyelet módban](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="7bb05-193">toouse Azure CLI commands, see [Azure CLI commands in Resource Manager mode](virtual-machines/azure-cli-arm-commands.md) and [Azure CLI commands in Service Management mode](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>
* <span data-ttu-id="7bb05-194">toolearn hello Azure parancssori Felülettel kapcsolatos további információkért töltse le a forráskódot, jelentse az esetleges problémákat, vagy toohello projekt közre, látogasson el a hello [hello Azure CLI GitHub-tárházban](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="7bb05-194">toolearn more about hello Azure CLI, download source code, report problems, or contribute toohello project, visit hello [GitHub repository for hello Azure CLI](https://github.com/azure/azure-xplat-cli).</span></span>
* <span data-ttu-id="7bb05-195">Ha az Azure CLI hello vagy Azure problémákat tapasztal, keresse fel a hello [Azure fórumok](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span><span class="sxs-lookup"><span data-stu-id="7bb05-195">If you encounter problems using hello Azure CLI, or Azure, visit hello [Azure Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span></span>
