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
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Azure-beli virtuális gépek üzembe helyezése a Cheffel
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Chef automation kézbesítéséhez egy remek eszköz, és állapot-konfiguráció szükséges.

A legújabb felhő-api a kiadástól kezdve Chef biztosít az Azure-ral való zökkenőmentes integrációt, felkínálva képességét tooprovision hello és konfigurációs állapotnak egyetlen parancs használatával telepítheti.

Ez a cikk I mutat be, hogyan tooset fel a Chef környezet tooprovision Azure virtuális gépek és egy házirend vagy az "CookBook" hoz létre, és majd telepítése a cookbook tooan Azure virtuális gép keresztül ismerteti.

Most megkezdéséhez!

## <a name="chef-basics"></a>Chef alapjai
Mielőtt elkezdené, kérem, tekintse át a Chef hello alapvető fogalmait. Nincs nagy anyagot <a href="http://www.chef.io/chef" target="_blank">Itt</a> és egy gyors olvasási rendelkezik, ez a forgatókönyv megkezdése előtt javasolt. Hello alapjai azonban I fog recap, mielőtt azt az első lépések.

a következő diagram hello hello magas szintű Chef architektúráját mutatja be.

![][2]

Chef három fő architekturális részből áll: Chef Server, a Chef ügyfél (csomópont) és a Chef munkaállomásra.

hello Chef kiszolgáló a felügyeleti pont és hello Chef kiszolgáló esetén két lehetőség áll rendelkezésre: üzemeltetett megoldás vagy a helyszíni megoldás. Fogjuk használni egy üzemeltetett megoldás.

hello Chef ügyfél (csomópont) hello ügynök, amely az hello kezelt kiszolgálókon.

hello Chef munkaállomás a rendszergazdai munkaállomás, amelyen azt a házirendek létrehozása és a felügyeleti parancsok. Futtatás hello **kés** parancsot a hello Chef munkaállomás toomanage az infrastruktúra.

"Cookbooks" és "Receptet" hello fogalmát is van. Ezek a hatékonyan hello házirendek azt határozza meg, és tooour kiszolgálókra vonatkoznak.

## <a name="preparing-hello-workstation"></a>Hello munkaállomás előkészítése
Először is lehetővé teszi, hogy előkészítő hello munkaállomásra. A szabványos Windows-munkaállomás használata. A konfigurációs fájlok és cookbooks toocreate egy könyvtár toostore kell.

Először létre kell hoznia egy C:\chef nevű könyvtárat.

Ezután hozzon létre egy második c:\chef\cookbooks nevű könyvtár.

Most kell toodownload az Azure-alapú beállítások fájl, Chef képes kommunikálni az Azure-előfizetés.

<!--Download your publish settings from [here](https://manage.windowsazure.com/publishsettings/).-->
Töltse le a közzétételi beállítások hello PowerShell Azure használatával [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) parancsot. 

Mentés hello közzététele beállításfájl C:\chef.

## <a name="creating-a-managed-chef-account"></a>Felügyelt Chef fiók létrehozása
Létrehoz egy üzemeltetett Chef fiókot [Itt](https://manage.chef.io/signup).

Hello előfizetési folyamat során fog ismételt toocreate egy új szervezetben.

![][3]

Ha a szervezet létrejött, töltse le a hello starter kit.

![][4]

> [!NOTE]
> Ha megjelenik egy értesítés figyelmezteti, hogy a kulcsok vissza lesz állítva, ez megegyezik ok tooproceed meglévő infrastruktúra nélkül még konfigurálva van.
> 
> 

A starter kit zip-fájl tartalmazza a szervezet konfigurációs fájlokat és a kulcsokat.

## <a name="configuring-hello-chef-workstation"></a>Hello Chef munkaállomás konfigurálása
Bontsa ki a hello chef-starter.zip tooC:\chef hello tartalmát.

Másolja az összes fájlt a chef-starter\chef-tárház\.chef tooyour c:\chef könyvtár.

A címtár most hasonlóan kell kinéznie hello a következő példa.

![][5]

Négy olyan fájlok, például a hello Azure közzétételi fájl c:\chef hello gyökerében lévő most rendelkeznie kell.

hello PEM-fájlok a szervezet és a rendszergazda titkos kulcsok kommunikációhoz tartalmaznak, amíg hello knife.rb fájl a Kés konfigurációját tartalmazza. Fel kell tooedit hello knife.rb fájlt.

Nyissa meg a választott szerkesztővel hello fájlt, és módosítsa a hello "cookbook_path" hello eltávolításával /... az hello elérési útja, így látható a következő módon jelenik meg.

    cookbook_path  ["#{current_dir}/cookbooks"]

Is hozzáadhat a hello következő sora tükröző hello neve az Azure közzététele beállításfájl.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

A knife.rb fájl most alábbihoz hasonló toohello a következő példa.

![][6]

Ezek a sorok fog győződjön meg arról, hogy kés hello cookbooks könyvtárat c:\chef\cookbooks hivatkozik, is használja az Azure közzétételi beállítási fájlját az Azure üzemeltetése során.

## <a name="installing-hello-chef-development-kit"></a>Hello Chef szoftverfejlesztői készlet telepítése
Következő [töltse le és telepítse](http://downloads.getchef.com/chef-dk/windows) hello ChefDK (Chef szoftverfejlesztői készlet) tooset a Chef munkaállomás telepítését.

![][7]

Telepítse a c:\opscode hello alapértelmezett helyét. A telepítés körülbelül 10 percet vesz igénybe.

Ellenőrizze, hogy a PATH változóban bejegyzést tartalmaz a C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Ha nem létezik, győződjön meg arról, adja hozzá az elérési utak!

*Megjegyzés: hello sorrendje a hello ELÉRÉSI fontos!* Amennyiben a opscode elérési út nem hello helyes sorrendben kell problémákat.

Indítsa újra a munkaállomáson, a folytatás előtt.

Ezt követően telepítjük hello kés Azure-bővítményt. Így lehetővé teszi kés hello az "Azure beépülő modul".

Futtassa a következő parancs hello.

    chef gem install knife-azure ––pre

> [!NOTE]
> hello – előtti argumentum biztosítja hello legújabb RC verziójában hello kés Azure beépülő modul, amely biztosít hozzáférést toohello legújabb API-készlet részesül.
> 
> 

Valószínű, hogy a függőségek számát is megtörténik, hello ugyanannyi időt vesz igénybe.

![][8]

tooensure van konfigurálva, a következő parancs futtatása hello.

    knife azure image list

Ha minden megfelelően van telepítve, látni fogja a keresztül elérhető Azure-rendszerképek görgetési listáját.

Gratulálunk! hello munkaállomás be van állítva.

## <a name="creating-a-cookbook"></a>Egy Cookbook létrehozása
Egy Cookbook Chef toodefine használják, hogy a kezelt ügyfél kívánja tooexecute parancsokat. Egy Cookbook létrehozása egyszerű és hello használjuk **chef készítése cookbook** toogenerate a Cookbook sablon parancsot. I fog hív Cookbook webkiszolgálón, szeretném, hogy egy házirendet, amely automatikusan telepíti az IIS.

A C:\Chef könyvtárban futtassa a következő parancs hello.

    chef generate cookbook webserver

Ezzel a hello könyvtárban C:\Chef\cookbooks\webserver fájlokat hoz létre. A felügyelt virtuális gépen a Chef ügyfél tooexecute szeretnénk parancsok készlete toodefine hello most kell.

hello parancsok hello fájl default.rb vannak tárolva. Ebben a fájlban I lesz kell meghatározása parancsok egy halmazát, amely telepíti az IIS szolgáltatást, IIS elindul, és másolja át a sablon toohello wwwroot mappája.

Hello C:\chef\cookbooks\webserver\recipes\default.rb fájl módosítása, és adja hozzá az alábbi hello.

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

Ha elkészült, mentse a hello fájlt.

## <a name="creating-a-template"></a>Egy sablon létrehozása
Ahogy azt korábban említettük, igazolnia kell a default.html lapként használt sablonfájl toogenerate.

Futtassa a következő parancs toogenerate hello sablon hello.

    chef generate template webserver Default.htm

Most lépjen toohello C:\chef\cookbooks\webserver\templates\default\Default.htm.erb fájlt. Néhány egyszerű "Hello World" HTML-kódot hozzáadásával hello fájl szerkesztése, és mentse hello fájlt.

## <a name="upload-hello-cookbook-toohello-chef-server"></a>Hello Cookbook toohello Chef Server feltöltése
Ebben a lépésben azt vannak véve hello Cookbook, amely a helyi gépen létrehoztunk egy példányát, majd ismét feltölteni a toohello Chef birtokolt kiszolgáló. A feltöltést követően hello Cookbook jelenik meg a hello **házirend** fülre.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>A Kés Azure virtuális gép telepítése
Rendszer most egy Azure virtuális gép üzembe helyezéséhez és hello is telepíti az IIS szolgáltatást és az alapértelmezett webes weblap "Webkiszolgáló" Cookbook alkalmazni.

A toodo ennek rendelés, használja a hello **kés azure-kiszolgáló létrehozása** parancsot.

Vagyok hello parancs például a következő jelenik meg.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

hello paraméterek magától értetődő. Helyettesítse be a változó, és futtassa.

> [!NOTE]
> Hello hello parancssor használatával I vagyok is automatizálása a végpont hálózati Állapotszűrő szabályok hello – tcp-végpontok paraméter használatával. I megnyitott portok: 80-as és 3389 tooprovide hozzáférés toomy weblapot és RDP-munkamenetbe.
> 
> 

Hello parancs futtatása után nyissa meg toohello Azure portálon, és megjelenik a gép tooprovision megkezdéséhez.

![][13]

hello parancssor következő jelenik meg.

![][10]

Hello központi telepítés befejezése után, azt kell tudni tooconnect toohello webszolgáltatás 80-as porton keresztül hello port azt kellett megnyitni, ha azt hello kés Azure parancs hello virtuális gép kiépítése. Mivel a virtuális gép egyetlen virtuális gép hello a felhőalapú szolgáltatás, szeretném összekapcsolni hello felhőalapú szolgáltatási URL-cím.

![][11]

Ahogy látja, figyelmeztető kreatív a HTML-kódra.

Ne feledje azt is kapcsolódhatnak a hello Azure-portálon keresztül 3389-es port RDP-munkamenetet.

Ez hasznos lett legyen szeretnék! Nyissa meg, és ma kód használatában az Azure-ral, indítsa el az infrastruktúra!

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
