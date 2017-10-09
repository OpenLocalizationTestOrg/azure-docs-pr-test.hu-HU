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
# <a name="django-hello-world-web-app-on-a-linux-vm"></a>A Linux virtuális gép Django Hello World webalkalmazás
> [!div class="op_single_selector"]
> * [Windows](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [Mac/Linux](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

Az oktatóanyag bemutatja, hogyan toohost Linux Azure virtuális gépek a Django-alapú webhely. Hello az oktatóanyagban feltételezzük, hogy nincs korábbi tapasztalata kapcsolatban az Azure-ral. Hello az oktatóanyag befejezése után a Django-alapú alkalmazások és hello felhőben futó lehet.

Az alábbiak végrehajtásának módját ismerheti meg:

* Állítson be egy Azure virtuális gép toohost Django. Bár ez az oktatóanyag azt ismerteti, hogyan toodo ezt **Linux**, teheti a Windows Server virtuális gépek Azure-ban üzemeltetett hello azonos. 
* A Linux hozzon létre egy új Django-alkalmazáshoz.

hello oktatóanyag bemutatja, hogyan toobuild egy alapszintű Hello World webalkalmazást. hello alkalmazás üzemel az Azure virtuális gépen.

a következő képernyőkép hello befejeződött hello alkalmazás látható:

![Egy böngészőablakban hello Hello World lap megjeleníti az Azure-ban](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a>Hozzon létre, és állítson be egy Azure virtuális gép toohost Django

1. egy Azure virtuális gép hello Ubuntu Server 14.04 LTS terjesztési toocreate lásd: [Linux virtuális gép létrehozása az Azure-portálon hello](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Jelszó-hitelesítés használata a nyilvános SSH-kulcs helyett is használhat.
2. tooedit hello hálózati biztonsági csoport tooallow bejövő HTTP forgalom tooport 80-as, lásd: [hálózati biztonsági csoportok létrehozása az Azure-portálon hello](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).
3. (Választható) Alapértelmezés szerint az új virtuális gép nem rendelkezik egy teljesen minősített tartománynevét (FQDN).  virtuális gép teljes Tartománynevet, és toocreate lásd [hozzon létre egy teljes Tartománynevet hello Azure-portálon a Windows virtuális gépek](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ezt a lépést nem kell az oktatóanyag végrehajtása.

## <a id="setup"></a>Hello fejlesztési környezet beállítása
> [!NOTE]
> Ha tooinstall Python kell, vagy toouse hello klienskódtárait, lásd: hello [Python a telepítési útmutató](../../python-how-to-install.md).

Ubuntu Linux virtuális gép hello Python 2.7 előre telepítve van, de az Apache vagy a Django nem tartozik. Fejezze be a következő lépéseket tooconnect tooyour VM hello és Apache és a Django telepítése:

1. Nyisson meg egy új terminálablakot.
2. tooconnect toohello Azure virtuális Gépen, adja meg a következő parancs hello. Ha a teljes tartománynév nem hozott létre, összekapcsolhatja a hello nyilvános IP-cím, amely hello virtuális gépek összesített hello Azure-portálon jelenik meg.
   
       $ ssh yourusername@yourVmUrl
3. tooinstall Django, adja meg a következő parancsok hello:
   
       $ sudo apt-get install python-setuptools python-pip
       $ sudo pip install django
4. tooinstall Apache mod-wsgi, és adja meg a hello a következő parancsot:
   
       $ sudo apt-get install apache2 libapache2-mod-wsgi

## <a name="create-a-new-django-app"></a>Új Django-alkalmazás létrehozása
1. toouse SSH tooaccess a virtuális gép, nyissa meg hello terminálablakot szakasz megelőző hello használt-e.
2. egy új Django-projekt toocreate adja meg a következő parancsok hello:
   
       $ cd /var/www
       $ sudo django-admin.py startproject helloworld
   
   Hello `django-admin.py` parancsfájl létrehoz egy alapszintű struktúrát Django-alapú webhelyekhez:
   
   * `helloworld/manage.py`segítségével indíthatja el, és állítsa le a Django-alapú webhely üzemeltetéséhez.
   * `helloworld/helloworld/settings.py`az alkalmazás Django-beállításokkal rendelkezik.
   * `helloworld/helloworld/urls.py`hello leképezési kód minden egyes URL-cím és a nézet között van.
3. Hello /var/www/helloworld/helloworld könyvtárban hozzon létre egy új fájlt views.py. Ez a fájl rendelkezik hello nézetben jeleníti meg az hello "hello world" lapon. Adja meg a következő parancsok hello a kód szerkesztése:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. Cserélje le a következő parancsok hello hello hello urls.py fájl tartalma:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-apache"></a>Apache beállítása
1. Hello /etc/apache2/sites-available/helloworld.conf mappában hozzon létre egy Apache virtuális állomás konfigurációs fájlja. Állítsa be a következő értékek hello tartalma toohello. Cserélje le *yourVmName* használ hello gép hello tényleges nevű (például *pyubuntu*).
   
       <VirtualHost *:80>
       ServerName yourVmName
       </VirtualHost>
       WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
       WSGIPythonPath /var/www/helloworld
2. tooactivate hello hely, használjon hello a következő parancsot:
   
       $ sudo a2ensite helloworld
3. Apache, toorestart hello a következő parancsot használja:
   
       $ sudo service apache2 reload
4. A böngészőben hello weblap betöltése:
   
   ![Egy böngészőablakban hello hello world lap megjeleníti az Azure-ban](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

## <a name="shut-down-your-azure-virtual-machine"></a>Az Azure virtuális gép leállítása
Amikor elkészült, az oktatóanyaghoz, azt javasoljuk, hogy állítsa le vagy távolítsa el a hello hello az oktatóanyaghoz létrehozott Azure virtuális Géphez. Ezzel felszabadul erőforrások más oktatóanyagok, és elkerülheti a nélül Azure hálózathasználati költségeket.

