---
title: "Cloud Services szerepkörénél aaaCommunication |} Microsoft Docs"
description: "Cloud Services szerepkör példánya lehet meghatározva a végpontjai (http, https, a tcp, udp) számukra hello kívül, vagy más szerepkör-példányok közötti kommunikációt."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7008a083-acbe-4fb8-ae60-b837ef971ca1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: 1fb39215ceb8a3f0381ef5e108c1149de115ff8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-communication-for-role-instances-in-azure"></a>Az azure-ban a szerepkörpéldányokért kommunikáció engedélyezése
Felhőszolgáltatás szerepköreit belső és külső kapcsolatokon keresztül kommunikálnak. Külső kapcsolatot más néven **bemeneti végpontok** közben belső kapcsolatok nevezzük **belső végpont**. Ez a témakör ismerteti, hogyan toomodify hello [webszolgáltatás](cloud-services-model-and-package.md#csdef) toocreate végpontok.

## <a name="input-endpoint"></a>Bemeneti végpont
a port toohello kívül tooexpose kívánt hello bemeneti végpont használatos. Megadhatja, hogy hello protokolltípus és hello port hello végpont, amely mindkét hello külső és belső portok hello végpont majd vonatkozik. Ha azt szeretné, hogy adhatja meg egy másik belső portot hello végpont hello [Helyi_port](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) attribútum.

hello bemeneti végpont használhatja a következő protokollok hello: **http, https, a tcp, udp**.

toocreate bemeneti végpont hozzáadása hello **bemeneti végponthoz** alárendelt elem toohello **végpontok** elem egy webes vagy feldolgozói szerepkör.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Példány bemeneti végpont
Példány bemeneti végpont olyan hasonló tooinput végpontok, de lehetővé teszi adott nyilvánosan elérhető portok minden egyes szerepkörpéldányhoz leképezése továbbítással port hello terheléselosztón. Egy nyilvánosan elérhető portot vagy egy porttartományt is megadhat.

hello példány bemeneti végpont csak olyan használhat **tcp** vagy **udp** hello protokollként.

toocreate példány bemeneti végpontja, vegye fel a hello **InstanceInputEndpoint** alárendelt elem toohello **végpontok** elem egy webes vagy feldolgozói szerepkör.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>A belső végpontot
Belső végpont példány-példány kommunikációs érhetők el. hello port megadása nem kötelező, és ha nincs megadva, a dinamikus port toohello végpont van hozzárendelve. Porttartomány is használható. Szerepkör / öt belső végpontok korlátozva van.

hello belső végpont használhatja a következő protokollok hello: **http, az tcp, udp, bármely**.

toocreate belső bemeneti végpont hozzáadása hello **InternalEndpoint** alárendelt elem toohello **végpontok** elem egy webes vagy feldolgozói szerepkör.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Porttartomány is használhatja.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Feldolgozói szerepkörök vs. Webes szerepkörök
A végpontok egy kisebb különbség van, ha a munkavégző és a webes szerepkörök használata. hello webes szerepkörnek rendelkeznie kell legalább egy bemeneti végpontot hello segítségével **HTTP** protokoll.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after hello first InputEndPoint -->
</Endpoints>
```

## <a name="using-hello-net-sdk-tooaccess-an-endpoint"></a>Hello .NET SDK tooaccess végpont használatával
hello Azure felügyelt kódtár szerepkör példányok toocommunicate futásidőben módszereket kínál. A szerepkör példánya belül futó kódból többi szerepkörpéldányon és a végpontok hello megléte kapcsolatos információkat, valamint hello aktuális szerepkörpéldányt információ kérheti le.

> [!NOTE]
> Információ a szerepkörpéldányok, amelyek a felhőalapú szolgáltatás fut, és legalább egy belső végpont definiáló csak kérheti le. Egy másik szolgáltatást futtató szerepkör-példányok adatait nem lehet megszerezni.
> 
> 

Használhatja a hello [példányok](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) tulajdonság tooretrieve példánya egy. Első alkalommal hello [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) tooreturn egy hivatkozást a jelenlegi szerepkörnek toohello példányt, és kövesse a hello [szerepkör](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) tulajdonság tooreturn egy hivatkozási toohello szerepkör magát.

Csatlakozás tooa szerepkörpéldányt programozott módon hello .NET SDK használatával, esetén viszonylag egyszerű tooaccess hello végpont-információkat. Például miután tooa adott szerepkör környezet már csatlakozott, egy adott végpont ezzel a kóddal hello port kaphat:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

Hello **példányok** tulajdonság gyűjteményének beolvasása az **RoleInstance** objektumok. Ez a gyűjtemény mindig hello jelenlegi példány tartalmaz. Ha hello szerepkör nem határoz meg egy belső végpont, hello gyűjteménybe hello aktuális példánya, de nincs más példány. hello hello gyűjtemény szerepkörpéldányok száma minden esetben 1 hello esetben, ha nem a belső végpontot hello szerepkör van definiálva. Hello szerepkör a belső végpont határozza meg, ha a példányok felderíthető futásidőben, és hello gyűjteményben található példányok száma hello toohello több példányban hello szerepkörhöz hello szolgáltatás konfigurációs fájljában megadott felel meg.

> [!NOTE]
> hello Azure felügyelt kódtár nem biztosít a többi szerepkörpéldányon hello állapotának meghatározására szolgáló eszköz, de Megvalósíthat ilyen állapotfigyelő értékelések saját kezűleg ezért ha a szolgáltatás ezt a funkciót. Használhat [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) tooobtain információt szerepkörpéldányokat.
> 
> 

toodetermine hello portszámot a példányon a belső végpont hello használható [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) tulajdonság tooreturn egy Dictionary objektum, amely tartalmazza a végpont nevének és a megfelelő IP-címek és portok. Hello [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) tulajdonság hello IP-cím és port megadott végpont adja vissza. Hello **PublicIPEndpoint** tulajdonság adja vissza egy elosztott terhelésű végpont hello port. hello IP cím részében hello **PublicIPEndpoint** tulajdonság nincs használatban.

Íme egy példa, amely megismétli a szerepkörpéldányok.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Íme egy példa egy feldolgozói szerepkört, amely lekérdezi a közzétett hello végpont hello szolgáltatás definition keresztül, és elindítja a kapcsolatfigyelést.

> [!WARNING]
> Ez a kód csak egy telepített szolgáltatáshoz fog működni. Hello Azure Compute Emulator futtatásakor a szolgáltatás konfigurációs elemek, amelyek közvetlenül átemelésre végpontokat hoz létre (**InstanceInputEndpoint** elemek) figyelmen kívül lesznek hagyva.
> 
> 

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;

        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;

        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);

        // Bind socket listener toointernal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block hello thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set hello maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see hello MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-toocontrol-role-communication"></a>Hálózati forgalmi szabályok toocontrol szerepkör kommunikáció
Belső végpont meghatározása után hozzáadhat hálózati forgalom (a létrehozott hello végpontok alapuló) szabályok toocontrol hogyan szerepkörpéldányokat kommunikálhatnak egymással. hello alábbi ábrán látható közötti szabályozásának olyan gyakori forgatókönyveket tartalmaz:

![A hálózati forgalom szabályok forgatókönyvek](./media/cloud-services-enable-communication-role-instances/scenarios.png "hálózati forgalmi szabályok forgatókönyvek")

hello alábbi példakód bemutatja szerepkör-definíciók hello szerepkörök hello előző ábrán is látható. Minden egyes szerepkör-definíció definiált legalább egy belső végpontot tartalmaz:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
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
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [!NOTE]
> Rögzített, és automatikusan hozzárendeli a portok a belső végpontok szerepkörök közötti kommunikáció korlátozásának alakulhat ki.
> 
> 

Alapértelmezés szerint belső végpont van definiálva, miután kommunikációs is haladjanak bármely szerepkör toohello belső végpont egy szerepkör korlátozások nélkül. toorestrict kommunikációt, hozzá kell adnia egy **NetworkTrafficRules** elem toohello **ServiceDefinition** elem hello szolgáltatásdefiníciós fájlban.

### <a name="scenario-1"></a>1. forgatókönyv
Csak a hálózati forgalom engedélyezése **WebRole1** túl**WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>2. forgatókönyv
Csak lehetővé teszi a hálózati forgalmat a **WebRole1** túl**WorkerRole1** és **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>3. forgatókönyv
Csak lehetővé teszi a hálózati forgalmat a **WebRole1** túl**WorkerRole1**, és **WorkerRole1** túl**WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>4. forgatókönyv
Csak lehetővé teszi a hálózati forgalmat a **WebRole1** túl**WorkerRole1**, **WebRole1** túl**WorkerRole2**, és  **WorkerRole1** túl**WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Az XML-séma hivatkozása fenti hello elemek található [Itt](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Következő lépések
További részletek a felhőalapú szolgáltatás hello [modell](cloud-services-model-and-package.md).

