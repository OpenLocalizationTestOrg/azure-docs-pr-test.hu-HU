---
title: "aaaCloud szolgáltatások szerepkör konfigurációs XPath Adatlap |} Microsoft Docs"
description: "hello is használhatja a hello felhőalapú szolgáltatás szerepkör konfigurációs tooexpose beállításaiban, egy környezeti változó különböző XPath beállításokat."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c51e4493-0643-4d05-bc44-06c76bcbf7d1
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 27f98f956a1c790c9bb30f9fefe1ab1736b2b150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>XPath környezeti változó, teszi közzé a szerepkör konfigurációs beállításai
Hello cloud service munkavégző vagy a webes szerepkör szolgáltatásdefiníciós fájl teszi közzé futásidejű konfigurációs értékeket környezeti változóként. a következő XPath értékek hello (amely tooAPI értékek) támogatottak.

Ezeket az XPath értékeket is elérhetők hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) könyvtárban. 

## <a name="app-running-in-emulator"></a>Emulátoron futó alkalmazáshoz
Azt jelzi, hogy adott hello az alkalmazás fut. hello emulátorban.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/Deployment/@emulated" |
| Kód |var x = RoleEnvironment.IsEmulated; |

## <a name="deployment-id"></a>Központi telepítési azonosítója
Lekéri a hello példány hello üzembehelyezés-azonosító.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/Deployment/@id" |
| Kód |var deploymentId = RoleEnvironment.DeploymentId; |

## <a name="role-id"></a>Szerepkör-azonosítója
Lekéri a hello aktuális szerepkör-Azonosítót hello példány.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@id" |
| Kód |var azonosítója = RoleEnvironment.CurrentRoleInstance.Id; |

## <a name="update-domain"></a>Tartomány frissítése
Lekéri a hello frissítési tartomány hello példány.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@updateDomain" |
| Kód |var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |

## <a name="fault-domain"></a>Hibatartomány
Lekéri a hello tartalék tartomány hello példány.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@faultDomain" |
| Kód |var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |

## <a name="role-name"></a>Szerepkör neve
Hello példányok hello szerepkör nevét kéri le.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@roleName" |
| Kód |var rname = RoleEnvironment.CurrentRoleInstance.Role.Name; |

## <a name="config-setting"></a>Konfigurációs beállítás
Lekéri hello értéket hello adott konfigurációs beállítás.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/ roleenvironment-et vagy a CurrentInstance/ConfigurationSettings/ConfigurationSetting [@name= 'Setting1']/@value" |
| Kód |var beállítás = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |

## <a name="local-storage-path"></a>Tárolási helyi elérési útja
Lekéri a hello példány hello helyi tárolási elérési útja.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/ roleenvironment-et vagy a CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@path" |
| Kód |var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |

## <a name="local-storage-size"></a>Helyi tárterület mérete
Lekéri a hello helyi tároló hello példány hello méretét.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/ roleenvironment-et vagy a CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@sizeInMB" |
| Kód |var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Végpont protokoll
Lekéri a hello végpont protokoll hello példányhoz.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/ roleenvironment-et/CurrentInstance/végpont vagy végpont [@name= 'Endpoint1']/@protocol" |
| Kód |var prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protokoll; |

## <a name="endpoint-ip"></a>Végpont IP
Lekérdezi hello megadott végpont IP-címet.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/ roleenvironment-et/CurrentInstance/végpont vagy végpont [@name= 'Endpoint1']/@address" |
| Kód |var cím = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Végpont portja
Lekéri a hello végponti port hello példányhoz.

| Típus | Példa |
| --- | --- |
| XPath |XPath = "/ roleenvironment-et/CurrentInstance/végpont vagy végpont [@name= 'Endpoint1']/@port" |
| Kód |var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port; |

## <a name="example"></a>Példa
A feldolgozói szerepkör, amely egy indítási tevékenységhez hoz létre egy környezeti változó neve, például `TestIsEmulated` toohello beállítása [ @emulated xpath értékét](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) fájlt.

Hozzon létre egy [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) csomag.

Engedélyezése [távoli asztal](cloud-services-role-enable-remote-desktop.md) szerepkör.

