---
title: "aaaViewing és állomásnevekkel módosítása |} Microsoft Docs"
description: "Hogyan tooview, és módosítsa az Azure virtuális gépek állomásnevek webes és feldolgozói szerepkörök névfeloldás"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 17d0dd7911754a94db3f37b924b4687da1c70aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="viewing-and-modifying-hostnames"></a>Megtekintése és módosítása az állomásnevek
állomásnév által hivatkozott a szerepkör-példányok toobe tooallow, akkor hello névhez hello értékre kell állítani az egyes szerepkörökhöz hello szolgáltatás konfigurációs fájljában. Így tesz, amely szükséges hello állomás neve toohello hozzáadásával **vmName** hello attribútumának **szerepkör** elemet. hello értékének hello **vmName** attribútum hello névhez az összes szerepkör-példány alapjaként van használatban. Például ha **vmName** van *webrole típusról* , de az adott szerepkör három alkalmazáspéldányra, hello állomásnevek hello példányok lesz *webrole0*, *webrole1* , és *webrole2*. Mert a virtuális gép állomásneve hello hello virtuálisgép-név alapján van feltöltve, nem kell toospecify egy nevet a virtuális gépek hello konfigurációs fájlban. A Microsoft Azure-szolgáltatás konfigurálásával kapcsolatos további információkért lásd: [Azure szolgáltatás konfigurációs séma (.cscfg fájl)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Állomásnevek megtekintése
A felhőszolgáltatás a virtuális gépek és a szerepkörpéldányok hello állomásnevek hello eszközök az alábbi használatával tekintheti meg.

### <a name="azure-portal"></a>Azure Portal
Használhatja a hello [Azure-portálon](http://portal.azure.com) tooview hello állomásnevek hello áttekintése panelen a virtuális gép virtuális gépekhez. Ne feledje, hogy hello panelen látható értéket **neve** és **állomásnév**. Annak ellenére, hogy kezdetben hello azonos hello gazdagép nevének módosítása esetén nem változik hello virtuális gép vagy szerepkörpéldány hello nevét.

Szerepkörpéldányok a hello Azure-portálon is megtekinthetők, de listában hello-példány egy felhőalapú szolgáltatás, hello állomás név nem jelenik meg. Látni fogja az összes olyan példány nevét, de ugyanez a neve nem felel meg a hello állomásnevet.

### <a name="service-configuration-file"></a>Szolgáltatás konfigurációs fájlja
Hello szolgáltatás konfigurációs fájlja egy telepített szolgáltatáshoz letölthető hello **konfigurálása** panel hello szolgáltatás hello Azure-portálon. Majd keressen hello **vmName** hello attribútuma **szerepkörnév** elem toosee hello állomás neve. Ne feledje, hogy a állomásnevet alapjaként szolgál minden szerepkörpéldányt hello állomásnevét. Például ha **vmName** van *webrole típusról* , de az adott szerepkör három alkalmazáspéldányra, hello állomásnevek hello példányok lesz *webrole0*, *webrole1* , és *webrole2*.

### <a name="remote-desktop"></a>A távoli asztal
Miután engedélyezte a távoli asztal (Windows), a Windows PowerShell távoli eljáráshívás (Windows) vagy a SSH (Linux és Windows rendszerekhez) kapcsolatok tooyour virtuális gépek vagy a szerepkörpéldányok, megtekintheti a hello állomásnévvel aktív távoli asztali kapcsolat különböző módokon:

* Írja be az állomásnév hello parancssor vagy a Terminálszolgáltatások SSH.
* Írja be az ipconfig/minden a hello parancs felszólítja (csak Windows).
* Nézet hello számítógépnév hello rendszer beállításai (csak Windows).

### <a name="azure-service-management-rest-api"></a>Azure szolgáltatásfelügyelet REST API-n
A többi ügyfél kövesse az alábbi utasításokat:

1. Győződjön meg arról, hogy rendelkezik-e ügyfél tooconnect toohello Azure-portálon. egy ügyféltanúsítványt, tooobtain lépésekkel hello szereplő [hogyan: letöltése és közzététele beállításokat importálhatja adatait és előfizetési információit](https://msdn.microsoft.com/library/dn385850.aspx). 
2. Állítsa be egy x-ms-version értéke az 2013-11-01 nevű fejlécében bejegyzést.
3. Kérés küldése a hello a következő formátumban: https://management.core.windows.net/\<subscrition-azonosító\>/services/hostedservices/\<szolgáltatásnév\>? beágyazása részletei = true
4. Hello keressen **állomásnév** minden elem **RoleInstance** elemet.

> [!WARNING]
> Megtekintheti hello belső tartományi utótag a felhőalapú szolgáltatás, a REST-hívást válasz hello hello ellenőrzésével **InternalDnsSuffix** elem, vagy futtassa az ipconfig/minden a távoli asztali munkamenetben (Windows), a parancssor vagy futó cat /etc/resolv.conf SSH terminálról (Linux).
> 
> 

## <a name="modifying-a-hostname"></a>Az állomásnév módosítása
Módosíthatja a virtuális gép vagy szerepkörpéldány hello host name egy módosított szolgáltatáskonfigurációs fájlt feltölteni, vagy a távoli asztali munkamenetből hello számítógép átnevezése.

## <a name="next-steps"></a>Következő lépések
[Névfeloldás (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Az Azure szolgáltatás konfigurációs séma (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure-beli virtuális hálózat konfigurációs séma](http://go.microsoft.com/fwlink/?LinkId=248093)

[Adja meg a hálózati konfigurációs fájlokat használja a DNS-beállítások](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

