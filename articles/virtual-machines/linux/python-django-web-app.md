---
title: "aaaPython webalkalmazást az Azure Linux virtuális gép a djangóval |} Microsoft Docs"
description: "Ismerje meg, hogyan toohost a Django-alapú webalkalmazás az Azure-ban a Linux virtuális gép."
services: virtual-machines-linux
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-resource-manager
ms.assetid: 00ad4c2c-4316-4f9a-913f-f7f49b158db7
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: python
ms.topic: article
ms.date: 05/31/2017
ms.author: huvalo
ms.openlocfilehash: 520c47e19e8ffb4bb866f70772d506ddf76e242c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="django-hello-world-web-app-on-a-linux-vm"></a><span data-ttu-id="0b33a-103">A Linux virtuális gép Django Hello World webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="0b33a-103">Django Hello World web app on a Linux VM</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0b33a-104">Windows</span><span class="sxs-lookup"><span data-stu-id="0b33a-104">Windows</span></span>](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [<span data-ttu-id="0b33a-105">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="0b33a-105">Mac/Linux</span></span>](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

<span data-ttu-id="0b33a-106">Az oktatóanyag bemutatja, hogyan toohost Linux Azure virtuális gépek a Django-alapú webhely.</span><span class="sxs-lookup"><span data-stu-id="0b33a-106">This tutorial shows you how toohost a Django-based website in Linux in Azure Virtual Machines.</span></span> <span data-ttu-id="0b33a-107">Hello az oktatóanyagban feltételezzük, hogy nincs korábbi tapasztalata kapcsolatban az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="0b33a-107">In hello tutorial, we assume no prior experience with Azure.</span></span> <span data-ttu-id="0b33a-108">Hello az oktatóanyag befejezése után a Django-alapú alkalmazások és hello felhőben futó lehet.</span><span class="sxs-lookup"><span data-stu-id="0b33a-108">When you finish hello tutorial, you can have a Django-based application up and running in hello cloud.</span></span>

<span data-ttu-id="0b33a-109">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="0b33a-109">Learn how to:</span></span>

* <span data-ttu-id="0b33a-110">Állítson be egy Azure virtuális gép toohost Django.</span><span class="sxs-lookup"><span data-stu-id="0b33a-110">Set up an Azure virtual machine toohost Django.</span></span> <span data-ttu-id="0b33a-111">Bár ez az oktatóanyag azt ismerteti, hogyan toodo ezt **Linux**, teheti a Windows Server virtuális gépek Azure-ban üzemeltetett hello azonos.</span><span class="sxs-lookup"><span data-stu-id="0b33a-111">Although this tutorial explains how toodo this for **Linux**, you can do hello same for a Windows Server VM hosted in Azure.</span></span> 
* <span data-ttu-id="0b33a-112">A Linux hozzon létre egy új Django-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="0b33a-112">Create a new Django application in Linux.</span></span>

<span data-ttu-id="0b33a-113">hello oktatóanyag bemutatja, hogyan toobuild egy alapszintű Hello World webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0b33a-113">hello tutorial shows you how toobuild a basic Hello World web application.</span></span> <span data-ttu-id="0b33a-114">hello alkalmazás üzemel az Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="0b33a-114">hello application is hosted in an Azure virtual machine.</span></span>

<span data-ttu-id="0b33a-115">a következő képernyőkép hello befejeződött hello alkalmazás látható:</span><span class="sxs-lookup"><span data-stu-id="0b33a-115">hello following screenshot shows hello completed application:</span></span>

![Egy böngészőablakban hello Hello World lap megjeleníti az Azure-ban](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a><span data-ttu-id="0b33a-117">Hozzon létre, és állítson be egy Azure virtuális gép toohost Django</span><span class="sxs-lookup"><span data-stu-id="0b33a-117">Create and set up an Azure virtual machine toohost Django</span></span>

1. <span data-ttu-id="0b33a-118">egy Azure virtuális gép hello Ubuntu Server 14.04 LTS terjesztési toocreate lásd: [Linux virtuális gép létrehozása az Azure-portálon hello](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0b33a-118">toocreate an Azure virtual machine with hello Ubuntu Server 14.04 LTS distribution, see [Create a Linux virtual machine in hello Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="0b33a-119">Jelszó-hitelesítés használata a nyilvános SSH-kulcs helyett is használhat.</span><span class="sxs-lookup"><span data-stu-id="0b33a-119">You also can choose password authentication instead of using an SSH public key.</span></span>
2. <span data-ttu-id="0b33a-120">tooedit hello hálózati biztonsági csoport tooallow bejövő HTTP forgalom tooport 80-as, lásd: [hálózati biztonsági csoportok létrehozása az Azure-portálon hello](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="0b33a-120">tooedit hello network security group tooallow incoming HTTP traffic tooport 80, see [Create network security groups in hello Azure portal](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span></span>
3. <span data-ttu-id="0b33a-121">(Választható) Alapértelmezés szerint az új virtuális gép nem rendelkezik egy teljesen minősített tartománynevét (FQDN).</span><span class="sxs-lookup"><span data-stu-id="0b33a-121">(Optional) By default, your new virtual machine doesn't have a fully qualified domain name (FQDN).</span></span>  <span data-ttu-id="0b33a-122">virtuális gép teljes Tartománynevet, és toocreate lásd [hozzon létre egy teljes Tartománynevet hello Azure-portálon a Windows virtuális gépek](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0b33a-122">toocreate a VM with an FQDN, see [Create an FQDN in hello Azure portal for a Windows VM](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="0b33a-123">Ezt a lépést nem kell az oktatóanyag végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="0b33a-123">This step is not required for completing this tutorial.</span></span>

## <span data-ttu-id="0b33a-124"><a id="setup"></a>Hello fejlesztési környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="0b33a-124"><a id="setup"> </a>Set up hello development environment</span></span>
> [!NOTE]
> <span data-ttu-id="0b33a-125">Ha tooinstall Python kell, vagy toouse hello klienskódtárait, lásd: hello [Python a telepítési útmutató](../../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="0b33a-125">If you need tooinstall Python or want toouse hello client libraries, see hello [Python installation guide](../../python-how-to-install.md).</span></span>

<span data-ttu-id="0b33a-126">Ubuntu Linux virtuális gép hello Python 2.7 előre telepítve van, de az Apache vagy a Django nem tartozik.</span><span class="sxs-lookup"><span data-stu-id="0b33a-126">hello Ubuntu Linux VM has Python 2.7 preinstalled, but it doesn't come with Apache or Django.</span></span> <span data-ttu-id="0b33a-127">Fejezze be a következő lépéseket tooconnect tooyour VM hello és Apache és a Django telepítése:</span><span class="sxs-lookup"><span data-stu-id="0b33a-127">Complete hello following steps tooconnect tooyour VM and install Apache and Django:</span></span>

1. <span data-ttu-id="0b33a-128">Nyisson meg egy új terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="0b33a-128">Open a new Terminal window.</span></span>
2. <span data-ttu-id="0b33a-129">tooconnect toohello Azure virtuális Gépen, adja meg a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="0b33a-129">tooconnect toohello Azure VM, enter hello following command.</span></span> <span data-ttu-id="0b33a-130">Ha a teljes tartománynév nem hozott létre, összekapcsolhatja a hello nyilvános IP-cím, amely hello virtuális gépek összesített hello Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0b33a-130">If you didn't create an FQDN, you can connect by using hello public IP address that's displayed in hello virtual machine summary in hello Azure portal.</span></span>
   
       $ ssh yourusername@yourVmUrl
3. <span data-ttu-id="0b33a-131">tooinstall Django, adja meg a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="0b33a-131">tooinstall Django, enter hello following commands:</span></span>
   
       $ sudo apt-get install python-setuptools python-pip
       $ sudo pip install django
4. <span data-ttu-id="0b33a-132">tooinstall Apache mod-wsgi, és adja meg a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0b33a-132">tooinstall Apache with mod-wsgi, enter hello following command:</span></span>
   
       $ sudo apt-get install apache2 libapache2-mod-wsgi

## <a name="create-a-new-django-app"></a><span data-ttu-id="0b33a-133">Új Django-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b33a-133">Create a new Django app</span></span>
1. <span data-ttu-id="0b33a-134">toouse SSH tooaccess a virtuális gép, nyissa meg hello terminálablakot szakasz megelőző hello használt-e.</span><span class="sxs-lookup"><span data-stu-id="0b33a-134">toouse SSH tooaccess your VM, open hello Terminal window you used in hello preceding section.</span></span>
2. <span data-ttu-id="0b33a-135">egy új Django-projekt toocreate adja meg a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="0b33a-135">toocreate a new Django project, enter hello following commands:</span></span>
   
       $ cd /var/www
       $ sudo django-admin.py startproject helloworld
   
   <span data-ttu-id="0b33a-136">Hello `django-admin.py` parancsfájl létrehoz egy alapszintű struktúrát Django-alapú webhelyekhez:</span><span class="sxs-lookup"><span data-stu-id="0b33a-136">hello `django-admin.py` script generates a basic structure for Django-based websites:</span></span>
   
   * <span data-ttu-id="0b33a-137">`helloworld/manage.py`segítségével indíthatja el, és állítsa le a Django-alapú webhely üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="0b33a-137">`helloworld/manage.py` helps you start hosting and stop hosting your Django-based website.</span></span>
   * <span data-ttu-id="0b33a-138">`helloworld/helloworld/settings.py`az alkalmazás Django-beállításokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0b33a-138">`helloworld/helloworld/settings.py` has Django settings for your application.</span></span>
   * <span data-ttu-id="0b33a-139">`helloworld/helloworld/urls.py`hello leképezési kód minden egyes URL-cím és a nézet között van.</span><span class="sxs-lookup"><span data-stu-id="0b33a-139">`helloworld/helloworld/urls.py` has hello mapping code between each URL and its view.</span></span>
3. <span data-ttu-id="0b33a-140">Hello /var/www/helloworld/helloworld könyvtárban hozzon létre egy új fájlt views.py.</span><span class="sxs-lookup"><span data-stu-id="0b33a-140">In hello /var/www/helloworld/helloworld directory, create a new file named views.py.</span></span> <span data-ttu-id="0b33a-141">Ez a fájl rendelkezik hello nézetben jeleníti meg az hello "hello world" lapon.</span><span class="sxs-lookup"><span data-stu-id="0b33a-141">This file has hello view that renders hello "hello world" page.</span></span> <span data-ttu-id="0b33a-142">Adja meg a következő parancsok hello a kód szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="0b33a-142">In your code editor, enter hello following commands:</span></span>
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. <span data-ttu-id="0b33a-143">Cserélje le a következő parancsok hello hello hello urls.py fájl tartalma:</span><span class="sxs-lookup"><span data-stu-id="0b33a-143">Replace hello contents of hello urls.py file with hello following commands:</span></span>
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-apache"></a><span data-ttu-id="0b33a-144">Apache beállítása</span><span class="sxs-lookup"><span data-stu-id="0b33a-144">Set up Apache</span></span>
1. <span data-ttu-id="0b33a-145">Hello /etc/apache2/sites-available/helloworld.conf mappában hozzon létre egy Apache virtuális állomás konfigurációs fájlja.</span><span class="sxs-lookup"><span data-stu-id="0b33a-145">In hello /etc/apache2/sites-available/helloworld.conf folder, create an Apache virtual host configuration file.</span></span> <span data-ttu-id="0b33a-146">Állítsa be a következő értékek hello tartalma toohello.</span><span class="sxs-lookup"><span data-stu-id="0b33a-146">Set hello contents toohello following values.</span></span> <span data-ttu-id="0b33a-147">Cserélje le *yourVmName* használ hello gép hello tényleges nevű (például *pyubuntu*).</span><span class="sxs-lookup"><span data-stu-id="0b33a-147">Replace *yourVmName* with hello actual name of hello machine you are using (for example, *pyubuntu*).</span></span>
   
       <VirtualHost *:80>
       ServerName yourVmName
       </VirtualHost>
       WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
       WSGIPythonPath /var/www/helloworld
2. <span data-ttu-id="0b33a-148">tooactivate hello hely, használjon hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0b33a-148">tooactivate hello site, use hello following command:</span></span>
   
       $ sudo a2ensite helloworld
3. <span data-ttu-id="0b33a-149">Apache, toorestart hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="0b33a-149">toorestart Apache, use hello following command:</span></span>
   
       $ sudo service apache2 reload
4. <span data-ttu-id="0b33a-150">A böngészőben hello weblap betöltése:</span><span class="sxs-lookup"><span data-stu-id="0b33a-150">Load hello webpage in your browser:</span></span>
   
   ![Egy böngészőablakban hello hello world lap megjeleníti az Azure-ban](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

## <a name="shut-down-your-azure-virtual-machine"></a><span data-ttu-id="0b33a-152">Az Azure virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="0b33a-152">Shut down your Azure virtual machine</span></span>
<span data-ttu-id="0b33a-153">Amikor elkészült, az oktatóanyaghoz, azt javasoljuk, hogy állítsa le vagy távolítsa el a hello hello az oktatóanyaghoz létrehozott Azure virtuális Géphez.</span><span class="sxs-lookup"><span data-stu-id="0b33a-153">When you're done with this tutorial, we recommend that you shut down or remove hello Azure VM you created for hello tutorial.</span></span> <span data-ttu-id="0b33a-154">Ezzel felszabadul erőforrások más oktatóanyagok, és elkerülheti a nélül Azure hálózathasználati költségeket.</span><span class="sxs-lookup"><span data-stu-id="0b33a-154">This frees up resources for other tutorials, and you can avoid incurring Azure usage charges.</span></span>

