---
title: "a Windows Server Azure virtuális gép aaaDjango webalkalmazás |} Microsoft Docs"
description: "Megtudhatja, hogyan toohost egy Django-alapú webhely, az Azure-ban a Windows Server 2012 R2 Datacenter VM hello klasszikus üzembe helyezési modellt."
services: virtual-machines-windows
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-service-management
ms.assetid: e36484d1-afbf-47f5-b755-5e65397dc1c3
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: python
ms.topic: article
ms.date: 05/31/2017
ms.author: huvalo
ms.openlocfilehash: 55847e3c6d6769965be29077e8d4eeebad914637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="django-hello-world-web-app-on-a-windows-server-vm"></a><span data-ttu-id="672e0-103">A Django Hello World webes alkalmazás a Windows Server virtuális gép</span><span class="sxs-lookup"><span data-stu-id="672e0-103">Django Hello World web app on a Windows Server VM</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="672e0-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és a hello klasszikus üzembe helyezési modellel](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="672e0-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and hello classic deployment model](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="672e0-105">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="672e0-105">This article describes hello classic deployment model.</span></span> <span data-ttu-id="672e0-106">Azt javasoljuk, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="672e0-106">We recommend that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="672e0-107">Az oktatóanyag bemutatja, hogyan toohost egy Django-alapú webhelyet a Windows Server Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="672e0-107">This tutorial shows you how toohost a Django-based website in Windows Server in Azure Virtual Machines.</span></span> <span data-ttu-id="672e0-108">Hello az oktatóanyagban feltételezzük, hogy nincs korábbi tapasztalata kapcsolatban az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="672e0-108">In hello tutorial, we assume no prior experience with Azure.</span></span> <span data-ttu-id="672e0-109">Hello az oktatóanyag befejezése után a Django-alapú alkalmazások és hello felhőben futó lehet.</span><span class="sxs-lookup"><span data-stu-id="672e0-109">When you finish hello tutorial, you can have a Django-based application up and running in hello cloud.</span></span>

<span data-ttu-id="672e0-110">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="672e0-110">Learn how to:</span></span>

* <span data-ttu-id="672e0-111">Állítson be egy Azure virtuális gép toohost Django.</span><span class="sxs-lookup"><span data-stu-id="672e0-111">Set up an Azure virtual machine toohost Django.</span></span> <span data-ttu-id="672e0-112">Bár ez az oktatóanyag azt ismerteti, hogyan toodo ezt **Windows Server**, megteheti hello ugyanazt az Azure szolgáltatásban üzemeltetett Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="672e0-112">Although this tutorial explains how toodo this for **Windows Server**, you can do hello same for a Linux VM hosted in Azure.</span></span>
* <span data-ttu-id="672e0-113">A Windows egy új Django-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="672e0-113">Create a new Django application in Windows.</span></span>

<span data-ttu-id="672e0-114">hello oktatóanyag bemutatja, hogyan toobuild egy alapszintű Hello World webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="672e0-114">hello tutorial shows you how toobuild a basic Hello World web application.</span></span> <span data-ttu-id="672e0-115">hello alkalmazás üzemel az Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="672e0-115">hello application is hosted in an Azure virtual machine.</span></span>

<span data-ttu-id="672e0-116">a következő képernyőkép hello befejeződött hello alkalmazás látható:</span><span class="sxs-lookup"><span data-stu-id="672e0-116">hello following screenshot shows hello completed application:</span></span>

![Egy böngészőablakban hello hello world lap megjeleníti az Azure-ban][1]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a><span data-ttu-id="672e0-118">Hozzon létre, és állítson be egy Azure virtuális gép toohost Django</span><span class="sxs-lookup"><span data-stu-id="672e0-118">Create and set up an Azure virtual machine toohost Django</span></span>

1. <span data-ttu-id="672e0-119">a Windows Server 2012 R2 Datacenter terjesztési hello Azure virtuális gép toocreate lásd: [hello Azure-portálon a Windows operációs rendszert futtató virtuális gép létrehozása](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="672e0-119">toocreate an Azure virtual machine with hello Windows Server 2012 R2 Datacenter distribution, see [Create a virtual machine running Windows in hello Azure portal](tutorial.md).</span></span>
2. <span data-ttu-id="672e0-120">Azure toodirect 80-as port forgalom hello webes tooport 80 hello virtuális gépen állítható be:</span><span class="sxs-lookup"><span data-stu-id="672e0-120">Set Azure toodirect port 80 traffic from hello web tooport 80 on hello virtual machine:</span></span>
   
   1. <span data-ttu-id="672e0-121">Az Azure-portálon hello lépjen toohello irányítópult, és válassza ki az újonnan létrehozott virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="672e0-121">In hello Azure portal, go toohello dashboard and select your newly created virtual machine.</span></span>
   2. <span data-ttu-id="672e0-122">Kattintson a **Végpontok**, majd a **Hozzáadás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="672e0-122">Click **Endpoints**, and then click **Add**.</span></span>

     ![Végpont hozzáadása](./media/python-django-web-app/django-helloworld-add-endpoint-new-portal.png)

   3. <span data-ttu-id="672e0-124">A hello **végpont hozzáadása** lap, a **neve**, adja meg **HTTP**.</span><span class="sxs-lookup"><span data-stu-id="672e0-124">On hello **Add endpoint** page, for **Name**, enter **HTTP**.</span></span> <span data-ttu-id="672e0-125">Állítsa be a nyilvános és titkos hello TCP-portok túl**80**.</span><span class="sxs-lookup"><span data-stu-id="672e0-125">Set hello public and private TCP ports too**80**.</span></span>

     ![Adjon meg nevet és a nyilvános és magánhálózati portot adjon meg](./media/python-django-web-app/django-helloworld-add-endpoint-set-ports-new-portal.png)

   4. <span data-ttu-id="672e0-127">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="672e0-127">Click **OK**.</span></span>
     
3. <span data-ttu-id="672e0-128">Hello irányítópultot válassza ki a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="672e0-128">In hello dashboard, select your VM.</span></span> <span data-ttu-id="672e0-129">Kattintson az újonnan létrehozott Azure virtuális gép toohello toouse Remote Desktop Protocol (RDP) tooremotely bejelentkezési **Connect**.</span><span class="sxs-lookup"><span data-stu-id="672e0-129">toouse Remote Desktop Protocol (RDP) tooremotely sign in toohello newly created Azure virtual machine, click **Connect**.</span></span>  

> [!IMPORTANT] 
> <span data-ttu-id="672e0-130">az utasításoknak hello azt feltételezik, hogy jelentkezett toohello virtuális gép megfelelően.</span><span class="sxs-lookup"><span data-stu-id="672e0-130">hello following instructions assume that you signed in toohello virtual machine correctly.</span></span> <span data-ttu-id="672e0-131">Akkor is azt feltételezik, hogy vannak-e kiadása parancsok hello virtuális gépen, és nem a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="672e0-131">They also assume that you are issuing commands in hello virtual machine and not on your local computer.</span></span>

## <span data-ttu-id="672e0-132"><a id="setup"></a>Python, a Django és WFastCGI telepítése</span><span class="sxs-lookup"><span data-stu-id="672e0-132"><a id="setup"> </a>Install Python, Django, and WFastCGI</span></span>
> [!NOTE]
> <span data-ttu-id="672e0-133">az Internet Explorer használatával toodownload, lehetséges, hogy az Internet Explorer tooconfigure **fokozott biztonsági beállítások** beállításait.</span><span class="sxs-lookup"><span data-stu-id="672e0-133">toodownload by using Internet Explorer, you might have tooconfigure Internet Explorer **Enhanced Security Configuration** settings.</span></span> <span data-ttu-id="672e0-134">toodo, kattintson **Start** > **felügyeleti eszközök** > **Kiszolgálókezelő** > **helyi kiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="672e0-134">toodo this, click **Start** > **Administrative Tools** > **Server Manager** > **Local Server**.</span></span> <span data-ttu-id="672e0-135">Kattintson a **Internet Explorer fokozott biztonsági beállítások**, majd válassza ki **ki**.</span><span class="sxs-lookup"><span data-stu-id="672e0-135">Click **IE Enhanced Security Configuration**, and then select **Off**.</span></span>

1. <span data-ttu-id="672e0-136">Telepítse a Python 2.7-es vagy Python 3.4 a legújabb verzióját hello [python.org][python.org].</span><span class="sxs-lookup"><span data-stu-id="672e0-136">Install hello latest versions of Python 2.7 or Python 3.4 from [python.org][python.org].</span></span>
2. <span data-ttu-id="672e0-137">Telepítse a pip használatával hello wfastcgi és a django csomagokat.</span><span class="sxs-lookup"><span data-stu-id="672e0-137">Install hello wfastcgi and django packages using pip.</span></span>
   
    <span data-ttu-id="672e0-138">Python 2.7 esetén használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="672e0-138">For Python 2.7, use hello following command:</span></span>
   
        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django
   
    <span data-ttu-id="672e0-139">Python 3.4 esetén használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="672e0-139">For Python 3.4, use hello following command:</span></span>
   
        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="install-iis-with-fastcgi"></a><span data-ttu-id="672e0-140">Telepítse az IIS FastCGI</span><span class="sxs-lookup"><span data-stu-id="672e0-140">Install IIS with FastCGI</span></span>
* <span data-ttu-id="672e0-141">Internet Information Services (IIS) telepítése a FastCGI-támogatással rendelkező.</span><span class="sxs-lookup"><span data-stu-id="672e0-141">Install Internet Information Services (IIS) with FastCGI support.</span></span> <span data-ttu-id="672e0-142">Ez eltarthat néhány percig tooexecute.</span><span class="sxs-lookup"><span data-stu-id="672e0-142">This might take several minutes tooexecute.</span></span>
   
        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="create-a-new-django-application"></a><span data-ttu-id="672e0-143">Hozzon létre egy új Django-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="672e0-143">Create a new Django application</span></span>
1. <span data-ttu-id="672e0-144">Adja meg a következő parancs hello C:\inetpub\wwwroot, egy új Django-projekt toocreate:</span><span class="sxs-lookup"><span data-stu-id="672e0-144">In C:\inetpub\wwwroot, toocreate a new Django project, enter hello following command:</span></span>
   
   <span data-ttu-id="672e0-145">Python 2.7 esetén használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="672e0-145">For Python 2.7, use hello following command:</span></span>
   
       C:\Python27\Scripts\django-admin.exe startproject helloworld
   
   <span data-ttu-id="672e0-146">Python 3.4 esetén használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="672e0-146">For Python 3.4, use hello following command:</span></span>
   
       C:\Python34\Scripts\django-admin.exe startproject helloworld
   
   ![hello hello New-AzureService parancs eredménye](./media/python-django-web-app/django-helloworld-cmd-new-azure-service.png)
2. <span data-ttu-id="672e0-148">Hello `django-admin` parancs létrehoz egy alapszintű struktúrát Django-alapú webhelyekhez:</span><span class="sxs-lookup"><span data-stu-id="672e0-148">hello `django-admin` command generates a basic structure for Django-based websites:</span></span>
   
   * <span data-ttu-id="672e0-149">`helloworld\manage.py`segítségével indíthatja el, és állítsa le a Django-alapú webhely üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="672e0-149">`helloworld\manage.py` helps you start hosting and stop hosting your Django-based website.</span></span>
   * <span data-ttu-id="672e0-150">`helloworld\helloworld\settings.py`az alkalmazás Django-beállításokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="672e0-150">`helloworld\helloworld\settings.py` has Django settings for your application.</span></span>
   * <span data-ttu-id="672e0-151">`helloworld\helloworld\urls.py`hello leképezési kód minden egyes URL-cím és a nézet között van.</span><span class="sxs-lookup"><span data-stu-id="672e0-151">`helloworld\helloworld\urls.py` has hello mapping code between each URL and its view.</span></span>
3. <span data-ttu-id="672e0-152">Hello C:\inetpub\wwwroot\helloworld\helloworld könyvtárban hozzon létre egy új fájlt views.py.</span><span class="sxs-lookup"><span data-stu-id="672e0-152">In hello C:\inetpub\wwwroot\helloworld\helloworld directory, create a new file named views.py.</span></span> <span data-ttu-id="672e0-153">Ez a fájl rendelkezik hello nézetben jeleníti meg az hello "hello world" lapon.</span><span class="sxs-lookup"><span data-stu-id="672e0-153">This file has hello view that renders hello "hello world" page.</span></span> <span data-ttu-id="672e0-154">Adja meg a következő parancsok hello a kód szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="672e0-154">In your code editor, enter hello following commands:</span></span>
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. <span data-ttu-id="672e0-155">Cserélje le a következő parancsok hello hello hello urls.py fájl tartalma:</span><span class="sxs-lookup"><span data-stu-id="672e0-155">Replace hello contents of hello urls.py file with hello following commands:</span></span>
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-iis"></a><span data-ttu-id="672e0-156">IIS beállítása</span><span class="sxs-lookup"><span data-stu-id="672e0-156">Set up IIS</span></span>
1. <span data-ttu-id="672e0-157">Hello globális applicationhost.config fájlban hello kezelők szakasz zárolásának feloldása.</span><span class="sxs-lookup"><span data-stu-id="672e0-157">In hello global applicationhost.config file, unlock hello handlers section.</span></span>  <span data-ttu-id="672e0-158">Ez lehetővé teszi, hogy a web.config fájl toouse hello Python kezelő.</span><span class="sxs-lookup"><span data-stu-id="672e0-158">This allows your web.config file toouse hello Python handler.</span></span> <span data-ttu-id="672e0-159">Adja hozzá ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="672e0-159">Add this command:</span></span>
   
        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
2. <span data-ttu-id="672e0-160">WFastCGI aktiválása.</span><span class="sxs-lookup"><span data-stu-id="672e0-160">Activate WFastCGI.</span></span> <span data-ttu-id="672e0-161">Ez biztosítja, hogy egy alkalmazás toohello globális applicationhost.config fájlt, amely tooyour Python parancsértelmező végrehajtható és hello wfastcgi.py parancsfájl hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="672e0-161">This adds an application toohello global applicationhost.config file, which refers tooyour Python interpreter executable and hello wfastcgi.py script.</span></span>
   
    <span data-ttu-id="672e0-162">A Python 2.7 esetén:</span><span class="sxs-lookup"><span data-stu-id="672e0-162">In Python 2.7:</span></span>
   
        C:\python27\scripts\wfastcgi-enable
   
    <span data-ttu-id="672e0-163">A Python 3.4 esetén:</span><span class="sxs-lookup"><span data-stu-id="672e0-163">In Python 3.4:</span></span>
   
        C:\python34\scripts\wfastcgi-enable
3. <span data-ttu-id="672e0-164">C:\inetpub\wwwroot\helloworld hozzon létre egy web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="672e0-164">In C:\inetpub\wwwroot\helloworld, create a web.config file.</span></span> <span data-ttu-id="672e0-165">hello értékének hello `scriptProcessor` attribútumot meg kell felelnie lépést megelőző hello hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="672e0-165">hello value of hello `scriptProcessor` attribute should match hello output from hello preceding step.</span></span> <span data-ttu-id="672e0-166">Hello wfastcgi beállítással kapcsolatos további információkért lásd: [pypi wfastcgi][wfastcgi].</span><span class="sxs-lookup"><span data-stu-id="672e0-166">For more information about hello wfastcgi setting, see [pypi wfastcgi][wfastcgi].</span></span>
   
   <span data-ttu-id="672e0-167">A Python 2.7 esetén:</span><span class="sxs-lookup"><span data-stu-id="672e0-167">In  Python 2.7:</span></span>
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
   
   <span data-ttu-id="672e0-168">A Python 3.4 esetén:</span><span class="sxs-lookup"><span data-stu-id="672e0-168">In  Python 3.4:</span></span>
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
4. <span data-ttu-id="672e0-169">Frissítse a hello hello az IIS alapértelmezett webhely toopoint toohello Django projekt mappájának helye:</span><span class="sxs-lookup"><span data-stu-id="672e0-169">Update hello location of hello IIS default website toopoint toohello Django project folder:</span></span>
   
        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"
5. <span data-ttu-id="672e0-170">A böngészőben hello weblap betölteni.</span><span class="sxs-lookup"><span data-stu-id="672e0-170">Load hello webpage in your browser.</span></span>

![Egy böngészőablakban hello hello world oldalát Azure jeleníti meg.][1]

## <a name="shut-down-your-azure-virtual-machine"></a><span data-ttu-id="672e0-172">Az Azure virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="672e0-172">Shut down your Azure virtual machine</span></span>
<span data-ttu-id="672e0-173">Amikor elkészült, az oktatóanyaghoz, azt javasoljuk, hogy állítsa le vagy távolítsa el a hello hello az oktatóanyaghoz létrehozott Azure virtuális Géphez.</span><span class="sxs-lookup"><span data-stu-id="672e0-173">When you're done with this tutorial, we recommend that you shut down or remove hello Azure VM you created for hello tutorial.</span></span> <span data-ttu-id="672e0-174">Ezzel felszabadul erőforrások más oktatóanyagok, és elkerülheti a nélül Azure hálózathasználati költségeket.</span><span class="sxs-lookup"><span data-stu-id="672e0-174">This frees up resources for other tutorials, and you can avoid incurring Azure usage charges.</span></span>

[1]: ./media/python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
