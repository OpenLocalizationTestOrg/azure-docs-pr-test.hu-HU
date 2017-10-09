---
title: "a Linux virtuális gép sínek webhelyen fonetikus aaaHost |} Microsoft Docs"
description: "Állítsa be, és egy Ruby webhelyen sínek-alapú Azure-ban a Linux virtuális gépek üzemeltetéséhez."
services: virtual-machines-linux
documentationcenter: ruby
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: aad32685-3550-4bff-9c73-beb8d70b3291
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: ruby
ms.topic: article
ms.date: 06/27/2017
ms.author: robmcm
ms.openlocfilehash: c545c24fc6c89497854bbe55a6d0d1d0b072c386
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a><span data-ttu-id="c2734-103">A Ruby on Rails webalkalmazás használata Azure-beli virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="c2734-103">Ruby on Rails Web application on an Azure VM</span></span>
<span data-ttu-id="c2734-104">Ez az oktatóanyag bemutatja, hogyan toohost egy Ruby sínek webhelyen Azure Linux virtuális gépek használata.</span><span class="sxs-lookup"><span data-stu-id="c2734-104">This tutorial shows how toohost a Ruby on Rails website on Azure using a Linux virtual machine.</span></span>  

<span data-ttu-id="c2734-105">Ez az oktatóanyag Ubuntu Server 14.04 LTS segítségével lett érvényesítve.</span><span class="sxs-lookup"><span data-stu-id="c2734-105">This tutorial was validated using Ubuntu Server 14.04 LTS.</span></span> <span data-ttu-id="c2734-106">Ha egy másik Linux-terjesztés használatához szükség lehet a toomodify hello tooinstall sínek lépések.</span><span class="sxs-lookup"><span data-stu-id="c2734-106">If you use a different Linux distribution, you might need toomodify hello steps tooinstall Rails.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c2734-107">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c2734-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="c2734-108">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="c2734-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="c2734-109">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="c2734-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
>
>

## <a name="create-an-azure-vm"></a><span data-ttu-id="c2734-110">Egy Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2734-110">Create an Azure VM</span></span>
<span data-ttu-id="c2734-111">Először hozzon létre egy Azure virtuális gép egy Linux-lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="c2734-111">Start by creating an Azure VM with a Linux image.</span></span>

<span data-ttu-id="c2734-112">toocreate hello VM, hello Azure-portált használja, vagy hello Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="c2734-112">toocreate hello VM, you can use hello Azure portal or hello Azure Command-Line Interface (CLI).</span></span>

### <a name="azure-portal"></a><span data-ttu-id="c2734-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c2734-113">Azure portal</span></span>
1. <span data-ttu-id="c2734-114">Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="c2734-114">Sign into hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="c2734-115">Kattintson a **új**, hello keresőmezőbe írja be a "Ubuntu Server 14.04".</span><span class="sxs-lookup"><span data-stu-id="c2734-115">Click **New**, then type "Ubuntu Server 14.04" in hello search box.</span></span> <span data-ttu-id="c2734-116">Kattintson a hello keresés által visszaadott hello bejegyzésre.</span><span class="sxs-lookup"><span data-stu-id="c2734-116">Click hello entry returned by hello search.</span></span> <span data-ttu-id="c2734-117">Hello telepítési modell kiválasztása **klasszikus**, majd kattintson a "Létrehozás" gombra.</span><span class="sxs-lookup"><span data-stu-id="c2734-117">For hello deployment model, select **Classic**, then click "Create".</span></span>
3. <span data-ttu-id="c2734-118">Hello alapvető beállítások panelen adja meg a szükséges hello mezők értékét: nevet (a virtuális gép hello), felhasználónév, hitelesítés típusa és hello megfelelő hitelesítő adatok, Azure-előfizetés, erőforráscsoportot és helyet.</span><span class="sxs-lookup"><span data-stu-id="c2734-118">In hello Basics blade, supply values for hello required fields: Name (for hello VM), User name, Authentication type and hello corresponding credentials, Azure subscription, Resource group, and Location.</span></span>

   ![Hozzon létre egy új Ubuntu kép](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. <span data-ttu-id="c2734-120">Hello virtuális gép üzembe helyezése után kattintson a hello virtuális gép nevét, majd kattintson **végpontok** a hello **beállítások** kategóriát.</span><span class="sxs-lookup"><span data-stu-id="c2734-120">After hello VM is provisioned, click on hello VM name, and click **Endpoints** in hello **Settings** category.</span></span> <span data-ttu-id="c2734-121">Található hello SSH végpont tartozó **önálló**.</span><span class="sxs-lookup"><span data-stu-id="c2734-121">Find hello SSH endpoint, listed under **Standalone**.</span></span>

   ![Alapértelmezett végpont](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a><span data-ttu-id="c2734-123">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c2734-123">Azure CLI</span></span>
<span data-ttu-id="c2734-124">Hello kövesse [hozzon létre egy virtuális gép futó Linux][vm-instructions].</span><span class="sxs-lookup"><span data-stu-id="c2734-124">Follow hello steps in [Create a Virtual Machine Running Linux][vm-instructions].</span></span>

<span data-ttu-id="c2734-125">Hello virtuális gép üzembe helyezése után is ki lehet hello SSH végpont hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c2734-125">After hello VM is provisioned, you can get hello SSH endpoint by running hello following command:</span></span>

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a><span data-ttu-id="c2734-126">Ruby sínen telepítése</span><span class="sxs-lookup"><span data-stu-id="c2734-126">Install Ruby on Rails</span></span>
1. <span data-ttu-id="c2734-127">SSH tooconnect toohello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="c2734-127">Use SSH tooconnect toohello VM.</span></span>
2. <span data-ttu-id="c2734-128">Hello SSH-munkamenetet, a parancsok tooinstall Ruby követően a virtuális gép hello hello használata:</span><span class="sxs-lookup"><span data-stu-id="c2734-128">From hello SSH session, use hello following commands tooinstall Ruby on hello VM:</span></span>

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > hello brightbox repository contains hello current Ruby distribution.

    <span data-ttu-id="c2734-129">hello telepítés néhány percet vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="c2734-129">hello installation may take a few minutes.</span></span> <span data-ttu-id="c2734-130">Ha elkészült, használja a következő parancs tooverify, hogy telepítve van-e a Ruby hello:</span><span class="sxs-lookup"><span data-stu-id="c2734-130">When it completes, use hello following command tooverify that Ruby is installed:</span></span>

        ruby -v

3. <span data-ttu-id="c2734-131">Használjon hello következő tooinstall sínek parancsot:</span><span class="sxs-lookup"><span data-stu-id="c2734-131">Use hello following command tooinstall Rails:</span></span>

        sudo gem install rails --no-rdoc --no-ri -V

    <span data-ttu-id="c2734-132">Hello--nem-rdoc és – hello dokumentációját, amely gyorsabb telepítése nem-ri jelzők tooskip használja.</span><span class="sxs-lookup"><span data-stu-id="c2734-132">Use hello --no-rdoc and --no-ri flags tooskip installing hello documentation, which is faster.</span></span>
    <span data-ttu-id="c2734-133">Ez a parancs valószínűleg tooexecute, így hozzáadása hello -V hello telepítési folyamat kapcsolatos információkat jeleníti meg hosszabb időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="c2734-133">This command will likely take a long time tooexecute, so adding hello -V will display information about hello installation progress.</span></span>

## <a name="create-and-run-an-app"></a><span data-ttu-id="c2734-134">Hozzon létre, és az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="c2734-134">Create and run an app</span></span>
<span data-ttu-id="c2734-135">Miközben továbbra is jelentkezik SSH-n keresztül, futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="c2734-135">While still logged in via SSH, run hello following commands:</span></span>

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

<span data-ttu-id="c2734-136">Hello [új](http://guides.rubyonrails.org/command_line.html#rails-new) parancs létrehoz egy új sínek alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c2734-136">hello [new](http://guides.rubyonrails.org/command_line.html#rails-new) command creates a new Rails app.</span></span> <span data-ttu-id="c2734-137">Hello [server](http://guides.rubyonrails.org/command_line.html#rails-server) indítása hello WEBrick webkiszolgáló sínek a parancsot.</span><span class="sxs-lookup"><span data-stu-id="c2734-137">hello [server](http://guides.rubyonrails.org/command_line.html#rails-server) command starts hello WEBrick web server that comes with Rails.</span></span> <span data-ttu-id="c2734-138">(Az éles környezetben való használathoz, akkor érdemes toouse egy másik kiszolgálót, például Unicorn vagy utas.)</span><span class="sxs-lookup"><span data-stu-id="c2734-138">(For production use, you would probably want toouse a different server, such as Unicorn or Passenger.)</span></span>

<span data-ttu-id="c2734-139">Kimeneti hasonló toohello következő kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="c2734-139">You should see output similar toohello following.</span></span>

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C tooshutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a><span data-ttu-id="c2734-140">Végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c2734-140">Add an endpoint</span></span>
1. <span data-ttu-id="c2734-141">Nyissa meg toohello [Azure portálra] [https://portal.azure.com] válassza ki a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="c2734-141">Go toohello [Azure portal][https://portal.azure.com] and select your VM.</span></span>

2. <span data-ttu-id="c2734-142">Válassza ki **VÉGPONTOK** a hello **beállítások** hello bal oldali szélének hello lap mentén.</span><span class="sxs-lookup"><span data-stu-id="c2734-142">Select **ENDPOINTS** in hello **Settings** along hello left edge hello page.</span></span>

3. <span data-ttu-id="c2734-143">Kattintson a **hozzáadása** hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="c2734-143">Click **ADD** at hello top of hello page.</span></span>

4. <span data-ttu-id="c2734-144">A hello **végpont hozzáadása** párbeszédpanel lapján adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="c2734-144">In hello **Add endpoint** dialog page, enter hello following information:</span></span>

   * <span data-ttu-id="c2734-145">**Név**: HTTP</span><span class="sxs-lookup"><span data-stu-id="c2734-145">**Name**: HTTP</span></span>
   * <span data-ttu-id="c2734-146">**Protokoll**: TCP</span><span class="sxs-lookup"><span data-stu-id="c2734-146">**Protocol**: TCP</span></span>
   * <span data-ttu-id="c2734-147">**Nyilvános port**: 80</span><span class="sxs-lookup"><span data-stu-id="c2734-147">**Public port**: 80</span></span>
   * <span data-ttu-id="c2734-148">**Magánhálózati port**: 3000</span><span class="sxs-lookup"><span data-stu-id="c2734-148">**Private port**: 3000</span></span>
   * <span data-ttu-id="c2734-149">**Lebegőpontos PI cím**: letiltva</span><span class="sxs-lookup"><span data-stu-id="c2734-149">**Floating PI address**: Disabled</span></span>
   * <span data-ttu-id="c2734-150">**Hozzáférés-vezérlési lista - rendelés**: 1001, vagy egy másik érték, amely beállítja a hozzáférési szabály hello prioritását.</span><span class="sxs-lookup"><span data-stu-id="c2734-150">**Access control list - Order**: 1001, or another value that sets hello priority of this access rule.</span></span>
   * <span data-ttu-id="c2734-151">**Hozzáférés-vezérlési lista - név**: allowHTTP</span><span class="sxs-lookup"><span data-stu-id="c2734-151">**Access control list - Name**: allowHTTP</span></span>
   * <span data-ttu-id="c2734-152">**Hozzáférés-vezérlési lista - művelet**: engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c2734-152">**Access control list - Action**: permit</span></span>
   * <span data-ttu-id="c2734-153">**Hozzáférés-vezérlési lista - távoli alhálózati**: 1.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="c2734-153">**Access control list - Remote subnet**: 1.0.0.0/16</span></span>

     <span data-ttu-id="c2734-154">Ezt a végpontot, amely átirányítja a forgalmat toohello magánhálózati port az 3000, ahol hello sínek a kiszolgáló figyel a 80-as nyilvános port van.</span><span class="sxs-lookup"><span data-stu-id="c2734-154">This endpoint  has a public port of 80 that will route traffic toohello private port of 3000, where hello Rails server is listening.</span></span> <span data-ttu-id="c2734-155">hello ACL-szabályhoz lehetővé teszi, hogy a nyilvános forgalom 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="c2734-155">hello access control list rule allows public traffic on port 80.</span></span>

     ![új végpont](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. <span data-ttu-id="c2734-157">Kattintson az OK toosave hello végpontra.</span><span class="sxs-lookup"><span data-stu-id="c2734-157">Click OK toosave hello endpoint.</span></span>

6. <span data-ttu-id="c2734-158">Egy üzenet megjelenjen-e arról, hogy **mentése a virtuális gép végpontjának**.</span><span class="sxs-lookup"><span data-stu-id="c2734-158">A message should appear that states **Saving virtual machine endpoint**.</span></span> <span data-ttu-id="c2734-159">Ez az üzenet megjelenik, amint hello-végpont esetében aktív.</span><span class="sxs-lookup"><span data-stu-id="c2734-159">Once this message disappears, hello endpoint is active.</span></span> <span data-ttu-id="c2734-160">Navigáljon a virtuális gép toohello DNS-neve most is tesztelheti az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c2734-160">You may now test your application by navigating toohello DNS name of your virtual machine.</span></span> <span data-ttu-id="c2734-161">hello webhely hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="c2734-161">hello website should appear similar toohello following:</span></span>

    ![alapértelmezett sínek lap][default-rails-cloud]

## <a name="next-steps"></a><span data-ttu-id="c2734-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2734-163">Next steps</span></span>
<span data-ttu-id="c2734-164">Ebben az oktatóanyagban hasonló hello lépéseket a legtöbb manuálisan.</span><span class="sxs-lookup"><span data-stu-id="c2734-164">In this tutorial, you did most of hello steps manually.</span></span> <span data-ttu-id="c2734-165">Éles környezetben kívánja írni az alkalmazást a fejlesztési számítógépen, majd központilag telepíti az Azure virtuális gép toohello.</span><span class="sxs-lookup"><span data-stu-id="c2734-165">In a production environment, you would write your app on a development machine and deploy it toohello Azure VM.</span></span> <span data-ttu-id="c2734-166">A legtöbb éles környezetekben is, hello sínek alkalmazás például az Apache vagy NginX, mely leírókat kérelem hello sínek alkalmazás és a kiszolgáló statikus erőforrások útválasztási toomultiple példánya egy másik kiszolgáló folyamat együtt üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="c2734-166">Also, most production environments host hello Rails application in conjunction with another server process such as Apache or NginX, which handles request routing toomultiple instances of hello Rails application and serving static resources.</span></span> <span data-ttu-id="c2734-167">További információkért lásd: http://rubyonrails.org/deploy/.</span><span class="sxs-lookup"><span data-stu-id="c2734-167">For more information, see http://rubyonrails.org/deploy/.</span></span>

<span data-ttu-id="c2734-168">toolearn Ruby, sínen kapcsolatos további információkért látogasson el a hello [sínek útmutatók a Ruby][rails-guides].</span><span class="sxs-lookup"><span data-stu-id="c2734-168">toolearn more about Ruby on Rails, visit hello [Ruby on Rails Guides][rails-guides].</span></span>

<span data-ttu-id="c2734-169">Azure-szolgáltatások toouse Ruby alkalmazás, lásd:</span><span class="sxs-lookup"><span data-stu-id="c2734-169">toouse Azure services from your Ruby application, see:</span></span>

* <span data-ttu-id="c2734-170">[Blobok strukturálatlan adatok tárolásához][blobs]</span><span class="sxs-lookup"><span data-stu-id="c2734-170">[Store unstructured data using blobs][blobs]</span></span>
* <span data-ttu-id="c2734-171">[Tároló kulcs/érték párok táblák használata][tables]</span><span class="sxs-lookup"><span data-stu-id="c2734-171">[Store key/value pairs using tables][tables]</span></span>
* <span data-ttu-id="c2734-172">[Nagy sávszélesség tartalomszolgáltatás a tartalom Delivery Network hello][cdn-howto]</span><span class="sxs-lookup"><span data-stu-id="c2734-172">[Serve high bandwidth content with hello Content Delivery Network][cdn-howto]</span></span>

<!-- WA.com links -->
[blobs]:../../../storage/blobs/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]:https://azure.microsoft.com/develop/ruby/app-services/
[tables]:../../../cosmos-db/table-storage-how-to-use-ruby.md
[vm-instructions]:createportal.md

<!-- External Links -->
[rails-guides]:http://guides.rubyonrails.org/
[sqlite3]:http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]:./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]:./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]:./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
