---
title: "aaaHow toouse hello service management API (Python) - funkcióismertető"
description: "Ismerje meg, hogyan tooprogrammatically közös szolgáltatás felügyeleti feladatok végrehajtása a Python."
services: cloud-services
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: 61538ec0-1536-4a7e-ae89-95967fe35d73
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/30/2017
ms.author: lmazuel
ms.openlocfilehash: b59622203470e1586484cec4033515edb39ca4d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-management-from-python"></a>Hogyan toouse-kezelés a Python
Ez az útmutató bemutatja, hogyan tooprogrammatically közös szolgáltatás felügyeleti feladatok végrehajtása a Python. Hello **ServiceManagementService** hello osztály [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python) támogatja a programozott hozzáférést toomuch hello szolgáltatás-felügyelettel kapcsolatos funkció elérhető hello [a klasszikus azure portálon] [ management-portal] (például **létrehozása, frissítése és törlése a felhőszolgáltatások, központi telepítések, adatok szolgáltatások és virtuális gépek**). Ez a funkció tooservice felügyeleti programozott hozzáférést igénylő alkalmazások fejlesztése során hasznos lehet.

## <a name="WhatIs"></a>Kezelő újdonságai
hello szolgáltatásfelügyeleti API biztosít a hello szolgáltatás felügyeleti funkcióinak hello keresztül elérhető programozott hozzáférés toomuch [a klasszikus Azure portálon][management-portal]. hello Azure SDK for Python toomanage lehetővé teszi a felhőalapú szolgáltatások és a storage-fiókok.

toouse hello Service Management API-t kell túl[az Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"></a>Fogalmak
hello Azure SDK for Python becsomagolja hello [Azure szolgáltatásfelügyeleti API][svc-mgmt-rest-api], vagyis a REST API-t. Az API műveletei SSL-kapcsolaton keresztül mennek végbe, és X.509 v3 tanúsítványok használatával kölcsönösen hitelesítettek. hello szolgáltatás előfordulhat, hogy érhetők el az Azure-ban, vagy hello interneten keresztül közvetlenül futó bármely alkalmazásból, amely egy HTTPS-kéréseket küldhet és fogadhat egy HTTPS-válasz szolgáltatás.

## <a name="Installation"></a>Telepítése
Ebben a cikkben leírt összes hello funkciók érhetők el hello `azure-servicemanagement-legacy` csomagot, amely a pip használatával telepítheti. (Például, ha új tooPython) telepítésével kapcsolatos további információkért lásd: Ez a cikk: [telepítése Python és hello Azure SDK](../python-how-to-install.md)

## <a name="Connect"></a>Hogyan: csatlakozás tooservice felügyeleti
Azure-előfizetése Azonosítóját és egy érvényes felügyeleti tanúsítvány szüksége tooconnect toohello szolgáltatásfelügyelet végpont. Ezt úgy szerezheti be az előfizetés-Azonosítóval keresztül hello [a klasszikus Azure portálon][management-portal].

> [!NOTE]
> Az OpenSSL jön létre, amikor a Windows rendszeren futó lehetséges toouse tanúsítványok már.  Python 2.7.4 igényel vagy újabb. .Pfx-tanúsítványok valószínűleg törlődni fog a jövőben hello támogatása óta ajánlott felhasználók toouse OpenSSL .pfx, helyett.
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Felügyeleti tanúsítványok Windows vagy Mac/Linux (OpenSSL)
Használhat [OpenSSL](http://www.openssl.org/) toocreate saját felügyeleti tanúsítványát.  Toocreate két tanúsítványt, egy a hello server ténylegesen szüksége (egy `.cer` fájl), a másik hello ügyfél (egy `.pem` fájlt). toocreate hello `.pem` fájlt, hajtható végre:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

toocreate hello `.cer` tanúsítvány, hajtható végre:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Az Azure-tanúsítványokkal kapcsolatos további információkért lásd: [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services-certs-create.md). OpenSSL-paraméter teljes leírását lásd: hello dokumentációját a [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Miután létrehozta ezeket a fájlokat, meg kell-e tooupload hello `.cer` keresztül hello lapja hello "Beállítások" hello "Feltöltés" művelettel tooAzure fájl [a klasszikus Azure portálon][management-portal], és toomake jegyezze fel kell hello menteni `.pem` fájlt.

Ha beszerezte az előfizetés-Azonosítóval, létrehozott egy tanúsítványt, és fel kell tölteni a hello `.cer` fájl tooAzure csatlakozáskor toohello Azure felügyeleti végpont úgy, hogy a hello előfizetése azonosítóját és hello elérési toohello `.pem` fájl túl**ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Az előző példában hello `sms` van egy **ServiceManagementService** objektum. Hello **ServiceManagementService** osztály hello használt elsődleges osztály toomanage van Azure-szolgáltatásokhoz.

### <a name="management-certificates-on-windows-makecert"></a>A Windows (MakeCert) felügyeleti tanúsítványok
Létrehozhat egy önaláírt felügyeleti tanúsítvány a gép használt `makecert.exe`.  Nyissa meg a **Visual Studio parancssorból** , egy **rendszergazda** , és használja a következő parancsot, hogy hello *AzureCertificate* szeretné hello tanúsítványnévvel rendelkező toouse.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

hello parancs létrehoz hello `.cer` fájlt, és telepíti azt a hello **személyes** tanúsítványtárolójába. További információkért lásd: [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services-certs-create.md).

Hello tanúsítvány létrehozását követően tooupload hello kell `.cer` keresztül hello lapja hello "Beállítások" hello "Feltöltés" művelettel tooAzure fájl [a klasszikus Azure portálon][management-portal].

Ha beszerezte az előfizetés-Azonosítóval, létrehozott egy tanúsítványt, és fel kell tölteni a hello `.cer` fájl tooAzure csatlakozáskor toohello Azure felügyeleti végpont átadásával hello előfizetése azonosítóját és hello tanúsítvány hello helyét a **Személyes** tanúsítványtárolót túl**ServiceManagementService** (ebben az esetben cserélje le *AzureCertificate* hello nevet, a tanúsítvány):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Az előző példában hello `sms` van egy **ServiceManagementService** objektum. Hello **ServiceManagementService** osztály hello használt elsődleges osztály toomanage van Azure-szolgáltatásokhoz.

## <a name="ListAvailableLocations"></a>Hogyan: elérhető helyről felsorolása
toolist hello helyeken elérhető szolgáltatásait üzemeltető, használja a hello **lista\_helyek** módszert:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Amikor létrehoz egy felhőalapú szolgáltatás, vagy a társzolgáltatás tooprovide egy érvényes helyet kell. Hello **lista\_helyek** metódus mindig hello jelenleg elérhető helyről naprakész listáját adja vissza. Től írásának pillanatában hello elérhető helyről a következők:

* Nyugat-Európa
* Észak-Európa
* Délkelet-Ázsia
* Kelet-Ázsia
* USA középső régiója
* USA északi középső régiója
* USA déli középső régiója
* USA nyugati régiója
* USA keleti régiója
* Kelet-Japán
* Nyugat-Japán
* Dél-Brazília
* Kelet-Ausztrália
* Délkelet-Ausztrália

## <a name="CreateCloudService"></a>Hogyan: felhőalapú szolgáltatás létrehozása
Amikor létrehoz egy alkalmazást, és futtassa az Azure-ban, hello kódjával és a konfigurációjával együtt nevezzük az Azure [felhőalapú szolgáltatás] [ cloud service] (néven egy *üzemeltetett szolgáltatás* a korábbi Azure ad). Hello **létrehozása\_üzemeltetett\_szolgáltatás** módszer lehetővé teszi egy új üzemeltetett szolgáltatás toocreate azáltal, hogy egy futtatott szolgáltatás nevét (amelyen az Azure-ban egyedinek kell lennie), a címkék (automatikusan kódolt toobase64), egy Leírás, és egy helyet.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Hello előfizetés az összes üzemeltetett hello szolgáltatást is listázhatja **lista\_üzemeltetett\_szolgáltatások** módszert:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Ha azt szeretné, hogy egy adott tárolt szolgáltatás adatainak tooget, azt is megteheti úgy, hogy hello üzemeltetett szolgáltatás neve toohello **beolvasása\_üzemeltetett\_szolgáltatás\_tulajdonságok** módszert:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Miután létrehozott egy felhőalapú szolgáltatás, a kód toohello hello szolgáltatást telepítheti **létrehozása\_telepítési** metódust.

## <a name="DeleteCloudService"></a>Hogyan: egy felhőalapú szolgáltatás törlése
Egy felhőalapú szolgáltatás törölheti hello szolgáltatás neve toohello átadásával **törlése\_üzemeltetett\_szolgáltatás** módszert:

    sms.delete_hosted_service('myhostedservice')

A szolgáltatás törlése előtt hello szolgáltatás központi telepítéseinek először törölni kell. (Lásd: [hogyan: törölje a központi telepítést](#DeleteDeployment) részletes.)

## <a name="DeleteDeployment"></a>Útmutató: a központi telepítés törlése
a központi telepítés toodelete hello használata **törlése\_telepítési** metódust. hello következő példa bemutatja, hogyan toodelete a központi telepítés nevű `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"></a>Hogyan: storage szolgáltatás létrehozása
A [társzolgáltatás](../storage/common/storage-create-storage-account.md) hozzáférést tud biztosítani, hogy tooAzure [Blobok](../storage/blobs/storage-python-how-to-use-blob-storage.md), [táblák](../cosmos-db/table-storage-how-to-use-python.md), és [várólisták](../storage/queues/storage-python-how-to-use-queue-storage.md). toocreate tárolási szolgáltatásként, van szüksége (kisbetűket 3 és 24 közötti és egyedi az Azure) hello szolgáltatás nevét, leírását, (felfelé too100 karakterek, automatikusan kódolt toobase64) címke és egy helyet. hello a következő példa bemutatja, hogyan toocreate tárolási funkciókat biztosító szolgáltatást, a hely megadásával.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Vegye figyelembe, hogy a fenti példa hello hello hello állapotának **létrehozása\_tárolási\_fiók** művelet által visszaadott eredmény hello átadásával kérhető **létrehozása\_tároló \_fiók** toohello **beolvasása\_művelet\_állapot** metódust.  

A storage-fiókok és a hello tulajdonságaik listázhatja **lista\_tárolási\_fiókok** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"></a>Útmutató: a társzolgáltatás törlése
Törölheti a társzolgáltatás úgy, hogy hello tárolási szolgáltatás neve toohello **törlése\_tárolási\_fiók** metódust. A storage szolgáltatás törlése (blobot, táblát és üzenetsort) hello szolgáltatásban tárolt összes adat törlése.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"></a>Hogyan: listából a rendelkezésre álló operációs rendszeren
toolist hello operációs rendszerek szolgáltatásait üzemeltető, rendelkezésre álló hello használata **lista\_működő\_rendszerek** módszert:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Másik lehetőségként használhatja a hello **lista\_működő\_rendszer\_családok** metódus, amely csoportosítja hello operációs rendszerek termékcsalád:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"></a>Hogyan: hozzon létre egy operációsrendszer-lemezképet
az operációs rendszer lemezképének toohello lemezképtárba tooadd hello használata **hozzáadása\_os\_kép** metódus:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

toolist hello operációsrendszer-lemezképek a rendelkezésre álló hello használata **lista\_os\_képek** metódust. Összes platform képek és felhasználói képeket tartalmaz:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"></a>Útmutató: az operációsrendszer-lemezkép törlése
egy felhasználói lemezkép toodelete hello használata **törlése\_os\_kép** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"></a>Útmutató: virtuális gép létrehozása
a virtuális gépek toocreate, először toocreate egy [felhőalapú szolgáltatás](#CreateCloudService).  Ezután hozzon létre hello virtuálisgép-példány használatával hello **létrehozása\_virtuális\_gép\_telepítési** metódus:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set hello location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where hello VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"></a>Útmutató: virtuális gép törlése
a virtuális gépek toodelete, előbb törölnie hello állításra hello **törlése\_telepítési** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

hello felhőszolgáltatás majd törölheti hello segítségével **törlése\_üzemeltetett\_szolgáltatás** módszert:

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>A virtuális gép létrehozása a rögzített virtuálisgép-lemezkép
egy Virtuálisgép-lemezkép toocapture, először meghívja a hello **rögzítése\_vm\_kép** módszer:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace hello below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Ezt követően toomake arról, hogy hello lemezkép rögzítése sikerült, használja a hello **lista\_vm\_képek** API-t, és győződjön meg arról, hogy a lemezkép hello eredmény jelenik meg:

    images = sms.list_vm_images()

toofinally hello rögzített lemezképpel hello virtuális gép létrehozása, használata hello **létrehozása\_virtuális\_gép\_telepítési** előtt, de ezúttal adjon át hello vm_image_name helyette módszer

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set hello location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

További részletek toolearn, hogyan toocapture egy Linux virtuális gép: [hogyan tooCapture Linux virtuális gépek.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

További részletek toolearn, hogyan toocapture egy Windows rendszerű virtuális gép: [hogyan tooCapture Windows virtuális gépként.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="What's Next"></a>Következő lépések
Most, hogy megismerte a service management hello alapjait, hogy elérhető-hello [teljes API referenciadokumentációt tartalmaz hello Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) , és végezze el összetett feladatokat könnyen toomanage a python alkalmazást.

További információkért lásd: hello [Python fejlesztői központ](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect tooservice management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/services/cloud-services/
