---
title: "aaaAzure virtuálisgép-telepítéshez olyan Chef |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Chef toodo automatikus-e a virtuális gép központi telepítése és konfigurálása a Microsoft Azure"
services: virtual-machines-windows
documentationcenter: 
author: diegoviso
manager: timlt
tags: azure-service-management,azure-resource-manager
editor: 
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: diviso
ms.openlocfilehash: c5ea98c673b2ee75dd4cedf27e50330af05230d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a><span data-ttu-id="b7a0a-103">Azure-beli virtuális gépek üzembe helyezése a Cheffel</span><span class="sxs-lookup"><span data-stu-id="b7a0a-103">Automating Azure virtual machine deployment with Chef</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="b7a0a-104">Chef automation kézbesítéséhez egy remek eszköz, és állapot-konfiguráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-104">Chef is a great tool for delivering automation and desired state configurations.</span></span>

<span data-ttu-id="b7a0a-105">A legújabb felhő-api a kiadástól kezdve Chef biztosít az Azure-ral való zökkenőmentes integrációt, felkínálva képességét tooprovision hello és konfigurációs állapotnak egyetlen parancs használatával telepítheti.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-105">With our latest cloud-api release, Chef provides seamless integration with Azure, giving you hello ability tooprovision and deploy configuration states through a single command.</span></span>

<span data-ttu-id="b7a0a-106">Ez a cikk I mutat be, hogyan tooset fel a Chef környezet tooprovision Azure virtuális gépek és egy házirend vagy az "CookBook" hoz létre, és majd telepítése a cookbook tooan Azure virtuális gép keresztül ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-106">In this article, I’ll show you how tooset up your Chef environment tooprovision Azure virtual machines and walk you through creating a policy or “CookBook” and then deploying this cookbook tooan Azure virtual machine.</span></span>

<span data-ttu-id="b7a0a-107">Most megkezdéséhez!</span><span class="sxs-lookup"><span data-stu-id="b7a0a-107">Let’s begin!</span></span>

## <a name="chef-basics"></a><span data-ttu-id="b7a0a-108">Chef alapjai</span><span class="sxs-lookup"><span data-stu-id="b7a0a-108">Chef basics</span></span>
<span data-ttu-id="b7a0a-109">Mielőtt elkezdené, kérem, tekintse át a Chef hello alapvető fogalmait.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-109">Before you begin, I suggest you review hello basic concepts of Chef.</span></span> <span data-ttu-id="b7a0a-110">Nincs nagy anyagot <a href="http://www.chef.io/chef" target="_blank">Itt</a> és egy gyors olvasási rendelkezik, ez a forgatókönyv megkezdése előtt javasolt.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-110">There is great material <a href="http://www.chef.io/chef" target="_blank">here</a> and I recommend you have a quick read before you attempt this walkthrough.</span></span> <span data-ttu-id="b7a0a-111">Hello alapjai azonban I fog recap, mielőtt azt az első lépések.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-111">I will however recap hello basics before we get started.</span></span>

<span data-ttu-id="b7a0a-112">a következő diagram hello hello magas szintű Chef architektúráját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-112">hello following diagram depicts hello high-level Chef architecture.</span></span>

![][2]

<span data-ttu-id="b7a0a-113">Chef három fő architekturális részből áll: Chef Server, a Chef ügyfél (csomópont) és a Chef munkaállomásra.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-113">Chef has three main architectural components: Chef Server, Chef Client (node), and Chef Workstation.</span></span>

<span data-ttu-id="b7a0a-114">hello Chef kiszolgáló a felügyeleti pont és hello Chef kiszolgáló esetén két lehetőség áll rendelkezésre: üzemeltetett megoldás vagy a helyszíni megoldás.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-114">hello Chef Server is our management point and there are two options for hello Chef Server: a hosted solution or an on-premises solution.</span></span> <span data-ttu-id="b7a0a-115">Fogjuk használni egy üzemeltetett megoldás.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-115">We will be using a hosted solution.</span></span>

<span data-ttu-id="b7a0a-116">hello Chef ügyfél (csomópont) hello ügynök, amely az hello kezelt kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-116">hello Chef Client (node) is hello agent that sits on hello servers you are managing.</span></span>

<span data-ttu-id="b7a0a-117">hello Chef munkaállomás a rendszergazdai munkaállomás, amelyen azt a házirendek létrehozása és a felügyeleti parancsok.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-117">hello Chef Workstation is our admin workstation where we create our policies and execute our management commands.</span></span> <span data-ttu-id="b7a0a-118">Futtatás hello **kés** parancsot a hello Chef munkaállomás toomanage az infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-118">We run hello **knife** command from hello Chef Workstation toomanage our infrastructure.</span></span>

<span data-ttu-id="b7a0a-119">"Cookbooks" és "Receptet" hello fogalmát is van.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-119">There is also hello concept of “Cookbooks” and “Recipes”.</span></span> <span data-ttu-id="b7a0a-120">Ezek a hatékonyan hello házirendek azt határozza meg, és tooour kiszolgálókra vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-120">These are effectively hello policies we define and apply tooour servers.</span></span>

## <a name="preparing-hello-workstation"></a><span data-ttu-id="b7a0a-121">Hello munkaállomás előkészítése</span><span class="sxs-lookup"><span data-stu-id="b7a0a-121">Preparing hello workstation</span></span>
<span data-ttu-id="b7a0a-122">Először is lehetővé teszi, hogy előkészítő hello munkaállomásra.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-122">First, lets prep hello workstation.</span></span> <span data-ttu-id="b7a0a-123">A szabványos Windows-munkaállomás használata.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-123">I’m using a standard Windows workstation.</span></span> <span data-ttu-id="b7a0a-124">A konfigurációs fájlok és cookbooks toocreate egy könyvtár toostore kell.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-124">We need toocreate a directory toostore our config files and cookbooks.</span></span>

<span data-ttu-id="b7a0a-125">Először létre kell hoznia egy C:\chef nevű könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-125">First create a directory called C:\chef.</span></span>

<span data-ttu-id="b7a0a-126">Ezután hozzon létre egy második c:\chef\cookbooks nevű könyvtár.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-126">Then create a second directory called c:\chef\cookbooks.</span></span>

<span data-ttu-id="b7a0a-127">Most kell toodownload az Azure-alapú beállítások fájl, Chef képes kommunikálni az Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-127">We now need toodownload our Azure settings file so Chef can communicate with our Azure subscription.</span></span>

<!--Download your publish settings from [here](https://manage.windowsazure.com/publishsettings/).-->
<span data-ttu-id="b7a0a-128">Töltse le a közzétételi beállítások hello PowerShell Azure használatával [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-128">Download your publish settings using hello PowerShell Azure [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) command.</span></span> 

<span data-ttu-id="b7a0a-129">Mentés hello közzététele beállításfájl C:\chef.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-129">Save hello publish settings file in C:\chef.</span></span>

## <a name="creating-a-managed-chef-account"></a><span data-ttu-id="b7a0a-130">Felügyelt Chef fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7a0a-130">Creating a managed Chef account</span></span>
<span data-ttu-id="b7a0a-131">Létrehoz egy üzemeltetett Chef fiókot [Itt](https://manage.chef.io/signup).</span><span class="sxs-lookup"><span data-stu-id="b7a0a-131">Sign up for a hosted Chef account [here](https://manage.chef.io/signup).</span></span>

<span data-ttu-id="b7a0a-132">Hello előfizetési folyamat során fog ismételt toocreate egy új szervezetben.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-132">During hello signup process, you will be asked toocreate a new organization.</span></span>

![][3]

<span data-ttu-id="b7a0a-133">Ha a szervezet létrejött, töltse le a hello starter kit.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-133">Once your organization is created, download hello starter kit.</span></span>

![][4]

> [!NOTE]
> <span data-ttu-id="b7a0a-134">Ha megjelenik egy értesítés figyelmezteti, hogy a kulcsok vissza lesz állítva, ez megegyezik ok tooproceed meglévő infrastruktúra nélkül még konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-134">If you receive a prompt warning you that your keys will be reset, it’s ok tooproceed as we have no existing infrastructure configured as yet.</span></span>
> 
> 

<span data-ttu-id="b7a0a-135">A starter kit zip-fájl tartalmazza a szervezet konfigurációs fájlokat és a kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-135">This starter kit zip file contains your organization config files and keys.</span></span>

## <a name="configuring-hello-chef-workstation"></a><span data-ttu-id="b7a0a-136">Hello Chef munkaállomás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b7a0a-136">Configuring hello Chef workstation</span></span>
<span data-ttu-id="b7a0a-137">Bontsa ki a hello chef-starter.zip tooC:\chef hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-137">Extract hello content of hello chef-starter.zip tooC:\chef.</span></span>

<span data-ttu-id="b7a0a-138">Másolja az összes fájlt a chef-starter\chef-tárház\.chef tooyour c:\chef könyvtár.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-138">Copy all files under chef-starter\chef-repo\.chef tooyour c:\chef directory.</span></span>

<span data-ttu-id="b7a0a-139">A címtár most hasonlóan kell kinéznie hello a következő példa.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-139">Your directory should now look something like hello following example.</span></span>

![][5]

<span data-ttu-id="b7a0a-140">Négy olyan fájlok, például a hello Azure közzétételi fájl c:\chef hello gyökerében lévő most rendelkeznie kell.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-140">You should now have four files including hello Azure publishing file in hello root of c:\chef.</span></span>

<span data-ttu-id="b7a0a-141">hello PEM-fájlok a szervezet és a rendszergazda titkos kulcsok kommunikációhoz tartalmaznak, amíg hello knife.rb fájl a Kés konfigurációját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-141">hello PEM files contain your organization and admin private keys for communication while hello knife.rb file contains your knife configuration.</span></span> <span data-ttu-id="b7a0a-142">Fel kell tooedit hello knife.rb fájlt.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-142">We will need tooedit hello knife.rb file.</span></span>

<span data-ttu-id="b7a0a-143">Nyissa meg a választott szerkesztővel hello fájlt, és módosítsa a hello "cookbook_path" hello eltávolításával /... az hello elérési útja, így látható a következő módon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-143">Open hello file in your editor of choice and modify hello “cookbook_path” by removing hello /../ from hello path so it appears as shown next.</span></span>

    cookbook_path  ["#{current_dir}/cookbooks"]

<span data-ttu-id="b7a0a-144">Is hozzáadhat a hello következő sora tükröző hello neve az Azure közzététele beállításfájl.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-144">Also add hello following line reflecting hello name of your Azure publish settings file.</span></span>

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

<span data-ttu-id="b7a0a-145">A knife.rb fájl most alábbihoz hasonló toohello a következő példa.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-145">Your knife.rb file should now look similar toohello following example.</span></span>

![][6]

<span data-ttu-id="b7a0a-146">Ezek a sorok fog győződjön meg arról, hogy kés hello cookbooks könyvtárat c:\chef\cookbooks hivatkozik, is használja az Azure közzétételi beállítási fájlját az Azure üzemeltetése során.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-146">These lines will ensure that Knife references hello cookbooks directory under c:\chef\cookbooks, and also uses our Azure Publish Settings file during Azure operations.</span></span>

## <a name="installing-hello-chef-development-kit"></a><span data-ttu-id="b7a0a-147">Hello Chef szoftverfejlesztői készlet telepítése</span><span class="sxs-lookup"><span data-stu-id="b7a0a-147">Installing hello Chef Development Kit</span></span>
<span data-ttu-id="b7a0a-148">Következő [töltse le és telepítse](http://downloads.getchef.com/chef-dk/windows) hello ChefDK (Chef szoftverfejlesztői készlet) tooset a Chef munkaállomás telepítését.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-148">Next [download and install](http://downloads.getchef.com/chef-dk/windows) hello ChefDK (Chef Development Kit) tooset up your Chef Workstation.</span></span>

![][7]

<span data-ttu-id="b7a0a-149">Telepítse a c:\opscode hello alapértelmezett helyét.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-149">Install in hello default location of c:\opscode.</span></span> <span data-ttu-id="b7a0a-150">A telepítés körülbelül 10 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-150">This install will take around 10 minutes.</span></span>

<span data-ttu-id="b7a0a-151">Ellenőrizze, hogy a PATH változóban bejegyzést tartalmaz a C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin</span><span class="sxs-lookup"><span data-stu-id="b7a0a-151">Confirm your PATH variable contains entries for C:\opscode\chefdk\bin;C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin</span></span>

<span data-ttu-id="b7a0a-152">Ha nem létezik, győződjön meg arról, adja hozzá az elérési utak!</span><span class="sxs-lookup"><span data-stu-id="b7a0a-152">If they are not there, make sure you add these paths!</span></span>

<span data-ttu-id="b7a0a-153">*Megjegyzés: hello sorrendje a hello ELÉRÉSI fontos!*</span><span class="sxs-lookup"><span data-stu-id="b7a0a-153">*NOTE hello ORDER OF hello PATH IS IMPORTANT!*</span></span> <span data-ttu-id="b7a0a-154">Amennyiben a opscode elérési út nem hello helyes sorrendben kell problémákat.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-154">If your opscode paths are not in hello correct order you will have issues.</span></span>

<span data-ttu-id="b7a0a-155">Indítsa újra a munkaállomáson, a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-155">Reboot your workstation before you continue.</span></span>

<span data-ttu-id="b7a0a-156">Ezt követően telepítjük hello kés Azure-bővítményt.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-156">Next, we will install hello Knife Azure extension.</span></span> <span data-ttu-id="b7a0a-157">Így lehetővé teszi kés hello az "Azure beépülő modul".</span><span class="sxs-lookup"><span data-stu-id="b7a0a-157">This provides Knife with hello “Azure Plugin”.</span></span>

<span data-ttu-id="b7a0a-158">Futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-158">Run hello following command.</span></span>

    chef gem install knife-azure ––pre

> [!NOTE]
> <span data-ttu-id="b7a0a-159">hello – előtti argumentum biztosítja hello legújabb RC verziójában hello kés Azure beépülő modul, amely biztosít hozzáférést toohello legújabb API-készlet részesül.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-159">hello –pre argument ensures you are receiving hello latest RC version of hello Knife Azure Plugin which provides access toohello latest set of APIs.</span></span>
> 
> 

<span data-ttu-id="b7a0a-160">Valószínű, hogy a függőségek számát is megtörténik, hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-160">It’s likely that a number of dependencies will also be installed at hello same time.</span></span>

![][8]

<span data-ttu-id="b7a0a-161">tooensure van konfigurálva, a következő parancs futtatása hello.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-161">tooensure everything is configured correctly, run hello following command.</span></span>

    knife azure image list

<span data-ttu-id="b7a0a-162">Ha minden megfelelően van telepítve, látni fogja a keresztül elérhető Azure-rendszerképek görgetési listáját.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-162">If everything is configured correctly, you will see a list of available Azure images scroll through.</span></span>

<span data-ttu-id="b7a0a-163">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="b7a0a-163">Congratulations.</span></span> <span data-ttu-id="b7a0a-164">hello munkaállomás be van állítva.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-164">hello workstation is set up!</span></span>

## <a name="creating-a-cookbook"></a><span data-ttu-id="b7a0a-165">Egy Cookbook létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7a0a-165">Creating a Cookbook</span></span>
<span data-ttu-id="b7a0a-166">Egy Cookbook Chef toodefine használják, hogy a kezelt ügyfél kívánja tooexecute parancsokat.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-166">A Cookbook is used by Chef toodefine a set of commands that you wish tooexecute on your managed client.</span></span> <span data-ttu-id="b7a0a-167">Egy Cookbook létrehozása egyszerű és hello használjuk **chef készítése cookbook** toogenerate a Cookbook sablon parancsot.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-167">Creating a Cookbook is straightforward and we use hello **chef generate cookbook** command toogenerate our Cookbook template.</span></span> <span data-ttu-id="b7a0a-168">I fog hív Cookbook webkiszolgálón, szeretném, hogy egy házirendet, amely automatikusan telepíti az IIS.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-168">I will be calling my Cookbook web server as I would like a policy that automatically deploys IIS.</span></span>

<span data-ttu-id="b7a0a-169">A C:\Chef könyvtárban futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-169">Under your C:\Chef directory run hello following command.</span></span>

    chef generate cookbook webserver

<span data-ttu-id="b7a0a-170">Ezzel a hello könyvtárban C:\Chef\cookbooks\webserver fájlokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-170">This will generate a set of files under hello directory C:\Chef\cookbooks\webserver.</span></span> <span data-ttu-id="b7a0a-171">A felügyelt virtuális gépen a Chef ügyfél tooexecute szeretnénk parancsok készlete toodefine hello most kell.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-171">We now need toodefine hello set of commands we would like our Chef client tooexecute on our managed virtual machine.</span></span>

<span data-ttu-id="b7a0a-172">hello parancsok hello fájl default.rb vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-172">hello commands are stored in hello file default.rb.</span></span> <span data-ttu-id="b7a0a-173">Ebben a fájlban I lesz kell meghatározása parancsok egy halmazát, amely telepíti az IIS szolgáltatást, IIS elindul, és másolja át a sablon toohello wwwroot mappája.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-173">In this file, I’ll be defining a set of commands that installs IIS, starts IIS and copies a template file toohello wwwroot folder.</span></span>

<span data-ttu-id="b7a0a-174">Hello C:\chef\cookbooks\webserver\recipes\default.rb fájl módosítása, és adja hozzá az alábbi hello.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-174">Modify hello C:\chef\cookbooks\webserver\recipes\default.rb file and add hello following lines.</span></span>

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

<span data-ttu-id="b7a0a-175">Ha elkészült, mentse a hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-175">Save hello file once you are done.</span></span>

## <a name="creating-a-template"></a><span data-ttu-id="b7a0a-176">Egy sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7a0a-176">Creating a template</span></span>
<span data-ttu-id="b7a0a-177">Ahogy azt korábban említettük, igazolnia kell a default.html lapként használt sablonfájl toogenerate.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-177">As we mentioned previously, we need toogenerate a template file which will be used as our default.html page.</span></span>

<span data-ttu-id="b7a0a-178">Futtassa a következő parancs toogenerate hello sablon hello.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-178">Run hello following command toogenerate hello template.</span></span>

    chef generate template webserver Default.htm

<span data-ttu-id="b7a0a-179">Most lépjen toohello C:\chef\cookbooks\webserver\templates\default\Default.htm.erb fájlt.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-179">Now navigate toohello C:\chef\cookbooks\webserver\templates\default\Default.htm.erb file.</span></span> <span data-ttu-id="b7a0a-180">Néhány egyszerű "Hello World" HTML-kódot hozzáadásával hello fájl szerkesztése, és mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-180">Edit hello file by adding some simple “Hello World” HTML code, and then save hello file.</span></span>

## <a name="upload-hello-cookbook-toohello-chef-server"></a><span data-ttu-id="b7a0a-181">Hello Cookbook toohello Chef Server feltöltése</span><span class="sxs-lookup"><span data-stu-id="b7a0a-181">Upload hello Cookbook toohello Chef Server</span></span>
<span data-ttu-id="b7a0a-182">Ebben a lépésben azt vannak véve hello Cookbook, amely a helyi gépen létrehoztunk egy példányát, majd ismét feltölteni a toohello Chef birtokolt kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-182">In this step, we are taking a copy of hello Cookbook that we have created on our local machine and uploading it toohello Chef Hosted Server.</span></span> <span data-ttu-id="b7a0a-183">A feltöltést követően hello Cookbook jelenik meg a hello **házirend** fülre.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-183">Once uploaded, hello Cookbook will appear under hello **Policy** tab.</span></span>

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a><span data-ttu-id="b7a0a-184">A Kés Azure virtuális gép telepítése</span><span class="sxs-lookup"><span data-stu-id="b7a0a-184">Deploy a virtual machine with Knife Azure</span></span>
<span data-ttu-id="b7a0a-185">Rendszer most egy Azure virtuális gép üzembe helyezéséhez és hello is telepíti az IIS szolgáltatást és az alapértelmezett webes weblap "Webkiszolgáló" Cookbook alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-185">We will now deploy an Azure virtual machine and apply hello “Webserver” Cookbook which will install our IIS web service and default web page.</span></span>

<span data-ttu-id="b7a0a-186">A toodo ennek rendelés, használja a hello **kés azure-kiszolgáló létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-186">In order toodo this, use hello **knife azure server create** command.</span></span>

<span data-ttu-id="b7a0a-187">Vagyok hello parancs például a következő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-187">Am example of hello command appears next.</span></span>

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

<span data-ttu-id="b7a0a-188">hello paraméterek magától értetődő.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-188">hello parameters are self-explanatory.</span></span> <span data-ttu-id="b7a0a-189">Helyettesítse be a változó, és futtassa.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-189">Substitute your particular variables and run.</span></span>

> [!NOTE]
> <span data-ttu-id="b7a0a-190">Hello hello parancssor használatával I vagyok is automatizálása a végpont hálózati Állapotszűrő szabályok hello – tcp-végpontok paraméter használatával.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-190">Through hello hello command line, I’m also automating my endpoint network filter rules by using hello –tcp-endpoints parameter.</span></span> <span data-ttu-id="b7a0a-191">I megnyitott portok: 80-as és 3389 tooprovide hozzáférés toomy weblapot és RDP-munkamenetbe.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-191">I’ve opened up ports 80 and 3389 tooprovide access toomy web page and RDP session.</span></span>
> 
> 

<span data-ttu-id="b7a0a-192">Hello parancs futtatása után nyissa meg toohello Azure portálon, és megjelenik a gép tooprovision megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-192">Once you run hello command, go toohello Azure portal and you will see your machine begin tooprovision.</span></span>

![][13]

<span data-ttu-id="b7a0a-193">hello parancssor következő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-193">hello command prompt appears next.</span></span>

![][10]

<span data-ttu-id="b7a0a-194">Hello központi telepítés befejezése után, azt kell tudni tooconnect toohello webszolgáltatás 80-as porton keresztül hello port azt kellett megnyitni, ha azt hello kés Azure parancs hello virtuális gép kiépítése.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-194">Once hello deployment is complete, we should be able tooconnect toohello web service over port 80 as we had opened hello port when we provisioned hello virtual machine with hello Knife Azure command.</span></span> <span data-ttu-id="b7a0a-195">Mivel a virtuális gép egyetlen virtuális gép hello a felhőalapú szolgáltatás, szeretném összekapcsolni hello felhőalapú szolgáltatási URL-cím.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-195">As this virtual machine is hello only virtual machine in my cloud service, I’ll connect it with hello cloud service url.</span></span>

![][11]

<span data-ttu-id="b7a0a-196">Ahogy látja, figyelmeztető kreatív a HTML-kódra.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-196">As you can see, I got creative with my HTML code.</span></span>

<span data-ttu-id="b7a0a-197">Ne feledje azt is kapcsolódhatnak a hello Azure-portálon keresztül 3389-es port RDP-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="b7a0a-197">Don’t forget we can also connect through an RDP session from hello Azure portal via port 3389.</span></span>

<span data-ttu-id="b7a0a-198">Ez hasznos lett legyen szeretnék!</span><span class="sxs-lookup"><span data-stu-id="b7a0a-198">I hope this has been helpful!</span></span> <span data-ttu-id="b7a0a-199">Nyissa meg, és ma kód használatában az Azure-ral, indítsa el az infrastruktúra!</span><span class="sxs-lookup"><span data-stu-id="b7a0a-199">Go  and start your infrastructure as code journey with Azure today!</span></span>

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png


<!--Link references-->
