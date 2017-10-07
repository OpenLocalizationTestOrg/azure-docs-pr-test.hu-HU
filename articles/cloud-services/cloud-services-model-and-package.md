---
title: "aaaWhat egy felhőalapú szolgáltatás modell és csomag |} Microsoft Docs"
description: "Ismerteti a hello felhő szolgáltatásmodell (.csdef, .cscfg) és a csomagba (.cspkg) az Azure-ban"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4ce2feb5-0437-496c-98da-1fb6dcb7f59e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 5280cdca4810859b6afdbbe1359fc2fabe871894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-cloud-service-model-and-how-do-i-package-it"></a>Hello Felhőszolgáltatás modell és hogyan tegye csomag azt?
Egy felhőalapú szolgáltatás létrehozása az három összetevővel, hello szolgáltatásdefiníció *(.csdef)*, hello szolgáltatás konfigurációs *(.cscfg)*, és a szolgáltatáscsomagot *(.cspkg)*. Mindkét hello **ServiceDefinition.csdef** és **ServiceConfig.cscfg** fájlok XML-alapú, és együttesen: hello modell hello felhőalapú szolgáltatás, és hogyan van konfigurálva; hello szerkezete ismertetik. Hello **ServicePackage.cspkg** egy zip-fájl, amely hello jönnek létre **ServiceDefinition.csdef** és többek között az összes szükséges hello bináris alapú függőségeket tartalmaz. Azure létrehoz egy felhőalapú szolgáltatás mindkét hello **ServicePackage.cspkg** és hello **ServiceConfig.cscfg**.

Miután hello felhőalapú szolgáltatás fut az Azure-ban, konfigurálhatja újra keresztül hello **ServiceConfig.cscfg** fájlt, de hello definíciója nem módosítható.

## <a name="what-would-you-like-tooknow-more-about"></a>Mit szeretne többet tooknow?
* További információk hello tooknow kívánt [ServiceDefinition.csdef](#csdef) és [ServiceConfig.cscfg](#cscfg) fájlokat.
* Arról, hogy már ismeri, adja meg [néhány példa](#next-steps) a Mi lehet konfigurálni.
* Szeretném toocreate hello [ServicePackage.cspkg](#cspkg).
* Visual Studio használok, és szeretnék...
  * [Felhőalapú szolgáltatás létrehozása][vs_create]
  * [Egy meglévő felhőszolgáltatáshoz újrakonfigurálása][vs_reconfigure]
  * [Egy Felhőszolgáltatás-projekt telepítése][vs_deploy]
  * [Távoli asztali kapcsolatot a felhő példánya.][remotedesktop]

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
Hello **ServiceDefinition.csdef** fájl határozza meg az Azure tooconfigure által használt hello beállításokat egy felhőalapú szolgáltatás. Hello [Azure szolgáltatás definíciós séma (.csdef fájl)](https://msdn.microsoft.com/library/azure/ee758711.aspx) hello engedélyezett formátum biztosít a szolgáltatásdefiníciós fájlban. hello alábbi példa bemutatja, amelyek az hello webes és feldolgozói szerepkörök hello-beállítások:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Olvassa el a toohello [szolgáltatás definíciós séma](https://msdn.microsoft.com/library/azure/ee758711.aspx) kra itt használt hello XML-séma, azonban itt van néhány hello elemek gyors magyarázata:

**Webhelyek**  
Webhelyekhez vagy webes alkalmazásokhoz, amelyek az IIS7 hello definícióit tartalmazza.

**InputEndpoints**  
-Végpontokat hello definícióját tartalmazza, toocontact hello felhőalapú szolgáltatás használt.

**InternalEndpoints**  
Szerepkör példányok toocommunicate egymással által használt hello definícióit tartalmazza.

**ConfigurationSettings**  
Egy adott szerepkör szolgáltatások hello beállítás definícióit tartalmazza.

**Tanúsítványok**  
A tanúsítványok, a szerepkör szükséges hello definícióit tartalmazza. hello előző példakód bemutatja a hello konfigurálása Azure Connect használt tanúsítvány.

**LocalResources**  
Helyi tároló-erőforrások hello definícióit tartalmazza. A helyi tároló egyik erőforrásához hello fájlrendszerben hello virtuális gép szerepkör példánya fut. fenntartott könyvtár.

**Importálása**  
Importált modulok hello definícióit tartalmazza. hello előző példakódban hello modulok távoli asztali kapcsolat és az Azure Connect jelennek meg.

**Indítása**  
Hello szerepkör indításakor futtatott feladatokat tartalmazza. hello feladatok .cmd vagy végrehajtható fájlban vannak definiálva.

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
hello konfiguráció hello a felhőalapú szolgáltatás határozza meg a hello hello értékek **ServiceConfiguration.cscfg** fájlt. Megadja a hello toodeploy szeretné az egyes szerepkörökhöz, a fájlban található példányok száma. hello értékek hello szolgáltatásdefiníciós fájlban meghatározott hello konfigurációs beállítások hozzáadása toohello szolgáltatás konfigurációs fájljában. bármely hello felhőszolgáltatás társított felügyeleti tanúsítványok ujjlenyomatait hello is bekerül toohello fájlt. Hello [Azure szolgáltatás konfigurációs séma (.cscfg fájl)](https://msdn.microsoft.com/library/azure/ee758710.aspx) hello engedélyezett formátum biztosít a szolgáltatás konfigurációs fájljában.

hello szolgáltatás konfigurációs fájlja nem hello alkalmazás együtt van csomagolva, de külön fájlként feltöltött tooAzure pedig használt tooconfigure hello felhőalapú szolgáltatás. A felhőalapú szolgáltatás ismételt üzembe helyezésével feltöltheti egy új szolgáltatás konfigurációs fájljában. hello konfigurációs értékek hello felhőszolgáltatás módosítása hello felhőalapú szolgáltatás futása közben. hello alábbi példa bemutatja hello konfigurációs beállítások hello webes és feldolgozói szerepkörök adható meg:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Olvassa el a toohello [szolgáltatás konfigurációs séma](https://msdn.microsoft.com/library/azure/ee758710.aspx) jobb megértése hello az XML-séma itt használni, itt azonban egy gyors hello elemek magyarázata:

**Példányok**  
Konfigurálja az hello futtató hello szerepkör példányainak számát. tooprevent a felhőalapú szolgáltatást a frissítéskor esetlegesen elérhetetlenné válik, javasoljuk, hogy telepít-e a felé néző webes szerepkörök egynél több példánya. Egynél több példány telepítésével hello toohello irányelveinek vannak betartásáért [Azure számítási szolgáltatás szolgáltatói szerződésben (SLA)](http://azure.microsoft.com/support/legal/sla/), amely biztosítja, hogy az 99,95 % külső kapcsolatot internetre szerepkörökhöz, ha két vagy több szerepkör példányok a szolgáltatás telepítésére.

**ConfigurationSettings**  
A szerepkör példánya fut hello hello-beállítások konfigurálása. hello hello neve `<Setting>` elemet meg kell egyeznie a hello beállításdefiníciókra hello szolgáltatásdefiníciós fájlban.

**Tanúsítványok**  
Konfigurálja az hello hello szolgáltatás által használt tanúsítványok. hello előző példakód bemutatja, hogyan toodefine hello hello RemoteAccess modul tanúsítványa. hello értékének hello *ujjlenyomat* attribútumot úgy kell beállítani a hello tanúsítvány toouse toohello ujjlenyomata.

<p/>

> [!NOTE]
> hello tanúsítványának ujjlenyomata hello felveheti toohello konfigurációs fájlt egy szövegszerkesztőben. Vagy hello érték hello vehetők fel **tanúsítványok** hello lapján **tulajdonságok** hello szerepkör a Visual Studio oldalán.
> 
> 

## <a name="defining-ports-for-role-instances"></a>A szerepkörpéldányokért portok meghatározása
Azure lehetővé teszi, hogy csak egy belépési pont tooa webes szerepkör. Ami azt jelenti, hogy az összes forgalom egy IP-címen keresztül történik. A webhelyek tooshare port konfigurálásával hello host fejléc toodirect hello kérelem toohello megfelelő helyen konfigurálhatja. Az alkalmazások toolisten toowell ismert portok hello IP-cím is elvégezheti.

hello következő minta hello konfiguráció látható egy webes szerepkör webhely és a webes alkalmazásokkal együtt. hello webhely hello alapértelmezett belépési helyére, a 80-as port van beállítva, és egy másodlagos állomásfejléc "mail.mysite.cloudapp.net" is nevezett konfigurált tooreceive kérelmeinek hello webalkalmazások.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" port="80" />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" hostheader="mail.mysite.cloudapp.net" />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-hello-configuration-of-a-role"></a>Egy szerepkör hello konfiguráció módosítása
Frissítheti a hello konfigurációját a felhőalapú szolgáltatás, az Azure nélkül offline állapotba helyezése hello szolgáltatás futása közben. toochange konfigurációs adatokat, vagy feltöltheti egy új konfigurációs fájlt, vagy Szerkesztés hello konfigurációs fájl helyezze el, majd telepítse a szolgáltatást futtató tooyour. hello következő módosítható a szolgáltatás toohello konfigurációját:

* **A konfigurációs beállítások hello értékek módosítása**  
  Amikor egy konfigurációs beállítás módosításokat, a szerepkör példánya választhat tooapply hello módosítása közben hello példány online állapotban, vagy toorecycle hello példány szabályosan és hello módosítás alkalmazásához, miközben hello példány offline állapotban.
* **Hello szolgáltatás topológiát szerepkörpéldányt beállítani módosítása**  
  Topológia változtatások nincsenek hatással a futó példányát, kivéve ahol távolít el egy példányát. Összes többi példányt általában nem szükséges toobe felhasználását; azonban választhatja ki toorecycle szerepkörpéldányokat válasz tooa topológiamódosítás.
* **Tanúsítvány-ujjlenyomat hello módosítása**  
  A tanúsítvány csak akkor frissíthető, ha a szerepkörpéldány offline állapotban. Ha egy tanúsítvány hozzáadott, törölték, vagy módosítható, amíg a szerepkör példánya online állapotban, a Azure szabályosan hello példány offline tooupdate hello tanúsítvány szükséges, és ismét online állapotba hello módosítás befejezése után.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Konfigurációs módosítások szolgáltatás futásidejű események kezelése
Hello [Azure futásidejű kódtár](https://msdn.microsoft.com/library/azure/mt419365.aspx) hello tartalmaz [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) névtér, amely osztályok biztosít az Azure környezetben szerepkörről hello interakció. Hello [roleenvironment-et](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) osztály határozza meg a következő előtt és után konfigurációmódosítás előállított események hello:

* **[Módosítása](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) esemény**  
  Ez akkor fordul elő, mielőtt hello konfigurációváltozás alkalmazott tooa megadott felkínálva hello szerepkörpéldányokat le egy alkalommal tootake szükség szerepkör példánya.
* **[Módosított](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) esemény**  
  Egy szerepkör megadott példánya alkalmazott tooa hello konfigurációs módosítás után következik be.

> [!NOTE]
> Tanúsítvány módosítások mindig egy hello példánya offline állapotba, mert azok nem indíthat hello RoleEnvironment.Changing vagy RoleEnvironment.Changed eseményeket.
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
egy alkalmazás az Azure-ban felhőszolgáltatásként toodeploy, kell első csomag hello alkalmazás hello megfelelő formátumban. Használhatja a hello **CSPack** parancssori eszköz (hello telepített [Azure SDK](https://azure.microsoft.com/downloads/)) toocreate hello csomag fájlt egy másik tooVisual Studio.

**CSPack** használ hello hello szolgáltatás definíciós fájl és szolgáltatás konfigurációs fájl toodefine hello hello csomag tartalmának tartalmát. **CSPack** hoz létre egy alkalmazás-csomagfájlját (.cspkg), hogy feltöltheti tooAzure hello segítségével [Azure-portálon](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Alapértelmezés szerint hello csomag nevű `[ServiceDefinitionFileName].cspkg`, de megadhat egy másik nevet a hello segítségével `/out` lehetőség a **CSPack**.

**CSPack** itt található:  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> (Windowson) CSPack.exe érhető hello futtatásával **Microsoft Azure parancssori** hello SDK telepített helyi.  
> 
> Programfuttatást hello CSPack.exe önmagában toosee dokumentációjában az összes lehetséges hello kapcsolók és parancsok.
> 
> 

<p />

> [!TIP]
> Futtassa helyileg a felhőalapú szolgáltatás hello **Microsoft Azure Compute Emulator**, használja a hello **/copyonly** lehetőséget. Ez a beállítás hello alkalmazás tooa directory elrendezés, amelyből kell futtathatók legyenek hello compute emulator hello bináris fájlokat másolja.
> 
> 

### <a name="example-command-toopackage-a-cloud-service"></a>Példa parancs toopackage egy felhőalapú szolgáltatás
hello alábbi példa létrehoz egy alkalmazáscsomagot, amely a webes szerepkör hello információkat tartalmaz. hello parancs hello szolgáltatás definíciós fájl toouse határozza meg, ahol bináris fájlok lehet hello directory található, hello hello csomag fájl nevét.

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

Ha hello alkalmazás egy webes és feldolgozói szerepkörök is tartalmaz, hello következő parancsot használja:

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

Ha hello változóit az alábbiak szerint:

| Változó | Érték |
| --- | --- |
| \[Könyvtárnév\] |hello alkönyvtár gyökérkönyvtárban hello projekt, amely tartalmazza az Azure-projekt hello hello .csdef fájl. |
| \[ServiceDefinition\] |hello hello szolgáltatásdefiníciós fájl neve. Alapértelmezés szerint a fájl neve ServiceDefinition.csdef. |
| \[Kimenetifajlneve\] |hello hello nevét csomagfájl jönnek létre. Általában értéke hello alkalmazás toohello nevét. Ha nincs fájl neve meg van adva, a hello alkalmazáscsomag elemként jön létre \[ApplicationName\].cspkg. |
| \[RoleName\] |hello szerepkör hello szolgáltatásdefiníciós fájlban meghatározott hello neve. |
| \[RoleBinariesDirectory] |hello hello hello szerepkör bináris fájljainak helyét. |
| \[VirtualPath\] |hello hello szolgáltatás definíció webhelyhez tartozó szakaszban meghatározott hello fizikai könyvtárak minden egyes virtuális elérési úthoz. |
| \[PhysicalPath\] |hello hello hely csomópontot hello szolgáltatás definíciójában meghatározott minden egyes virtuális elérési hello tartalmának fizikai könyvtárak. |
| \[RoleAssemblyName\] |hello bináris fájl hello szerepkör hello neve. |

## <a name="next-steps"></a>Következő lépések
I vagyok létrehozni egy cloud service-csomag, és szeretnék...

* [Távoli asztal beállítása a felhő példánya.][remotedesktop]
* [Egy Felhőszolgáltatás-projekt telepítése][deploy]

Visual Studio használok, és szeretnék...

* [Új felhőalapú szolgáltatás létrehozása][vs_create]
* [Egy meglévő felhőszolgáltatáshoz újrakonfigurálása][vs_reconfigure]
* [Egy Felhőszolgáltatás-projekt telepítése][vs_deploy]
* [Távoli asztal beállítása a felhő példánya.][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
