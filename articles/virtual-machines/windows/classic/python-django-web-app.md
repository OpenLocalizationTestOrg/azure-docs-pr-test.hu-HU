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
# <a name="django-hello-world-web-app-on-a-windows-server-vm"></a>A Django Hello World webes alkalmazás a Windows Server virtuális gép

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és a hello klasszikus üzembe helyezési modellel](../../../resource-manager-deployment-model.md). Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben. Azt javasoljuk, hogy az új telepítések esetén hello Resource Manager modellt használja.

Az oktatóanyag bemutatja, hogyan toohost egy Django-alapú webhelyet a Windows Server Azure virtuális gépeken. Hello az oktatóanyagban feltételezzük, hogy nincs korábbi tapasztalata kapcsolatban az Azure-ral. Hello az oktatóanyag befejezése után a Django-alapú alkalmazások és hello felhőben futó lehet.

Az alábbiak végrehajtásának módját ismerheti meg:

* Állítson be egy Azure virtuális gép toohost Django. Bár ez az oktatóanyag azt ismerteti, hogyan toodo ezt **Windows Server**, megteheti hello ugyanazt az Azure szolgáltatásban üzemeltetett Linux virtuális gép.
* A Windows egy új Django-alkalmazás létrehozása

hello oktatóanyag bemutatja, hogyan toobuild egy alapszintű Hello World webalkalmazást. hello alkalmazás üzemel az Azure virtuális gépen.

a következő képernyőkép hello befejeződött hello alkalmazás látható:

![Egy böngészőablakban hello hello world lap megjeleníti az Azure-ban][1]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a>Hozzon létre, és állítson be egy Azure virtuális gép toohost Django

1. a Windows Server 2012 R2 Datacenter terjesztési hello Azure virtuális gép toocreate lásd: [hello Azure-portálon a Windows operációs rendszert futtató virtuális gép létrehozása](tutorial.md).
2. Azure toodirect 80-as port forgalom hello webes tooport 80 hello virtuális gépen állítható be:
   
   1. Az Azure-portálon hello lépjen toohello irányítópult, és válassza ki az újonnan létrehozott virtuális gépet.
   2. Kattintson a **Végpontok**, majd a **Hozzáadás** lehetőségre.

     ![Végpont hozzáadása](./media/python-django-web-app/django-helloworld-add-endpoint-new-portal.png)

   3. A hello **végpont hozzáadása** lap, a **neve**, adja meg **HTTP**. Állítsa be a nyilvános és titkos hello TCP-portok túl**80**.

     ![Adjon meg nevet és a nyilvános és magánhálózati portot adjon meg](./media/python-django-web-app/django-helloworld-add-endpoint-set-ports-new-portal.png)

   4. Kattintson az **OK** gombra.
     
3. Hello irányítópultot válassza ki a virtuális Gépet. Kattintson az újonnan létrehozott Azure virtuális gép toohello toouse Remote Desktop Protocol (RDP) tooremotely bejelentkezési **Connect**.  

> [!IMPORTANT] 
> az utasításoknak hello azt feltételezik, hogy jelentkezett toohello virtuális gép megfelelően. Akkor is azt feltételezik, hogy vannak-e kiadása parancsok hello virtuális gépen, és nem a helyi számítógépen.

## <a id="setup"></a>Python, a Django és WFastCGI telepítése
> [!NOTE]
> az Internet Explorer használatával toodownload, lehetséges, hogy az Internet Explorer tooconfigure **fokozott biztonsági beállítások** beállításait. toodo, kattintson **Start** > **felügyeleti eszközök** > **Kiszolgálókezelő** > **helyi kiszolgáló**. Kattintson a **Internet Explorer fokozott biztonsági beállítások**, majd válassza ki **ki**.

1. Telepítse a Python 2.7-es vagy Python 3.4 a legújabb verzióját hello [python.org][python.org].
2. Telepítse a pip használatával hello wfastcgi és a django csomagokat.
   
    Python 2.7 esetén használja a következő parancs hello:
   
        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django
   
    Python 3.4 esetén használja a következő parancs hello:
   
        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="install-iis-with-fastcgi"></a>Telepítse az IIS FastCGI
* Internet Information Services (IIS) telepítése a FastCGI-támogatással rendelkező. Ez eltarthat néhány percig tooexecute.
   
        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="create-a-new-django-application"></a>Hozzon létre egy új Django-alkalmazáshoz
1. Adja meg a következő parancs hello C:\inetpub\wwwroot, egy új Django-projekt toocreate:
   
   Python 2.7 esetén használja a következő parancs hello:
   
       C:\Python27\Scripts\django-admin.exe startproject helloworld
   
   Python 3.4 esetén használja a következő parancs hello:
   
       C:\Python34\Scripts\django-admin.exe startproject helloworld
   
   ![hello hello New-AzureService parancs eredménye](./media/python-django-web-app/django-helloworld-cmd-new-azure-service.png)
2. Hello `django-admin` parancs létrehoz egy alapszintű struktúrát Django-alapú webhelyekhez:
   
   * `helloworld\manage.py`segítségével indíthatja el, és állítsa le a Django-alapú webhely üzemeltetéséhez.
   * `helloworld\helloworld\settings.py`az alkalmazás Django-beállításokkal rendelkezik.
   * `helloworld\helloworld\urls.py`hello leképezési kód minden egyes URL-cím és a nézet között van.
3. Hello C:\inetpub\wwwroot\helloworld\helloworld könyvtárban hozzon létre egy új fájlt views.py. Ez a fájl rendelkezik hello nézetben jeleníti meg az hello "hello world" lapon. Adja meg a következő parancsok hello a kód szerkesztése:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. Cserélje le a következő parancsok hello hello hello urls.py fájl tartalma:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-iis"></a>IIS beállítása
1. Hello globális applicationhost.config fájlban hello kezelők szakasz zárolásának feloldása.  Ez lehetővé teszi, hogy a web.config fájl toouse hello Python kezelő. Adja hozzá ezt a parancsot:
   
        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
2. WFastCGI aktiválása. Ez biztosítja, hogy egy alkalmazás toohello globális applicationhost.config fájlt, amely tooyour Python parancsértelmező végrehajtható és hello wfastcgi.py parancsfájl hivatkozik.
   
    A Python 2.7 esetén:
   
        C:\python27\scripts\wfastcgi-enable
   
    A Python 3.4 esetén:
   
        C:\python34\scripts\wfastcgi-enable
3. C:\inetpub\wwwroot\helloworld hozzon létre egy web.config fájlt. hello értékének hello `scriptProcessor` attribútumot meg kell felelnie lépést megelőző hello hello kimenetét. Hello wfastcgi beállítással kapcsolatos további információkért lásd: [pypi wfastcgi][wfastcgi].
   
   A Python 2.7 esetén:
   
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
   
   A Python 3.4 esetén:
   
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
4. Frissítse a hello hello az IIS alapértelmezett webhely toopoint toohello Django projekt mappájának helye:
   
        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"
5. A böngészőben hello weblap betölteni.

![Egy böngészőablakban hello hello world oldalát Azure jeleníti meg.][1]

## <a name="shut-down-your-azure-virtual-machine"></a>Az Azure virtuális gép leállítása
Amikor elkészült, az oktatóanyaghoz, azt javasoljuk, hogy állítsa le vagy távolítsa el a hello hello az oktatóanyaghoz létrehozott Azure virtuális Géphez. Ezzel felszabadul erőforrások más oktatóanyagok, és elkerülheti a nélül Azure hálózathasználati költségeket.

[1]: ./media/python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
