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
# <a name="enable-communication-for-role-instances-in-azure"></a><span data-ttu-id="95096-103">Az azure-ban a szerepkörpéldányokért kommunikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="95096-103">Enable communication for role instances in azure</span></span>
<span data-ttu-id="95096-104">Felhőszolgáltatás szerepköreit belső és külső kapcsolatokon keresztül kommunikálnak.</span><span class="sxs-lookup"><span data-stu-id="95096-104">Cloud service roles communicate through internal and external connections.</span></span> <span data-ttu-id="95096-105">Külső kapcsolatot más néven **bemeneti végpontok** közben belső kapcsolatok nevezzük **belső végpont**.</span><span class="sxs-lookup"><span data-stu-id="95096-105">External connections are called **input endpoints** while internal connections are called **internal endpoints**.</span></span> <span data-ttu-id="95096-106">Ez a témakör ismerteti, hogyan toomodify hello [webszolgáltatás](cloud-services-model-and-package.md#csdef) toocreate végpontok.</span><span class="sxs-lookup"><span data-stu-id="95096-106">This topic describes how toomodify hello [service definition](cloud-services-model-and-package.md#csdef) toocreate endpoints.</span></span>

## <a name="input-endpoint"></a><span data-ttu-id="95096-107">Bemeneti végpont</span><span class="sxs-lookup"><span data-stu-id="95096-107">Input endpoint</span></span>
<span data-ttu-id="95096-108">a port toohello kívül tooexpose kívánt hello bemeneti végpont használatos.</span><span class="sxs-lookup"><span data-stu-id="95096-108">hello input endpoint is used when you want tooexpose a port toohello outside.</span></span> <span data-ttu-id="95096-109">Megadhatja, hogy hello protokolltípus és hello port hello végpont, amely mindkét hello külső és belső portok hello végpont majd vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="95096-109">You specify hello protocol type and hello port of hello endpoint which then applies for both hello external and internal ports for hello endpoint.</span></span> <span data-ttu-id="95096-110">Ha azt szeretné, hogy adhatja meg egy másik belső portot hello végpont hello [Helyi_port](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) attribútum.</span><span class="sxs-lookup"><span data-stu-id="95096-110">If you want, you can specify a different internal port for hello endpoint with hello [localPort](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) attribute.</span></span>

<span data-ttu-id="95096-111">hello bemeneti végpont használhatja a következő protokollok hello: **http, https, a tcp, udp**.</span><span class="sxs-lookup"><span data-stu-id="95096-111">hello input endpoint can use hello following protocols: **http, https, tcp, udp**.</span></span>

<span data-ttu-id="95096-112">toocreate bemeneti végpont hozzáadása hello **bemeneti végponthoz** alárendelt elem toohello **végpontok** elem egy webes vagy feldolgozói szerepkör.</span><span class="sxs-lookup"><span data-stu-id="95096-112">toocreate an input endpoint, add hello **InputEndpoint** child element toohello **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a><span data-ttu-id="95096-113">Példány bemeneti végpont</span><span class="sxs-lookup"><span data-stu-id="95096-113">Instance input endpoint</span></span>
<span data-ttu-id="95096-114">Példány bemeneti végpont olyan hasonló tooinput végpontok, de lehetővé teszi adott nyilvánosan elérhető portok minden egyes szerepkörpéldányhoz leképezése továbbítással port hello terheléselosztón.</span><span class="sxs-lookup"><span data-stu-id="95096-114">Instance input endpoints are similar tooinput endpoints but allows you map specific public-facing ports for each individual role instance by using port forwarding on hello load balancer.</span></span> <span data-ttu-id="95096-115">Egy nyilvánosan elérhető portot vagy egy porttartományt is megadhat.</span><span class="sxs-lookup"><span data-stu-id="95096-115">You can specify a single public-facing port, or a range of ports.</span></span>

<span data-ttu-id="95096-116">hello példány bemeneti végpont csak olyan használhat **tcp** vagy **udp** hello protokollként.</span><span class="sxs-lookup"><span data-stu-id="95096-116">hello instance input endpoint can only use **tcp** or **udp** as hello protocol.</span></span>

<span data-ttu-id="95096-117">toocreate példány bemeneti végpontja, vegye fel a hello **InstanceInputEndpoint** alárendelt elem toohello **végpontok** elem egy webes vagy feldolgozói szerepkör.</span><span class="sxs-lookup"><span data-stu-id="95096-117">toocreate an instance input endpoint, add hello **InstanceInputEndpoint** child element toohello **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a><span data-ttu-id="95096-118">A belső végpontot</span><span class="sxs-lookup"><span data-stu-id="95096-118">Internal endpoint</span></span>
<span data-ttu-id="95096-119">Belső végpont példány-példány kommunikációs érhetők el.</span><span class="sxs-lookup"><span data-stu-id="95096-119">Internal endpoints are available for instance-to-instance communication.</span></span> <span data-ttu-id="95096-120">hello port megadása nem kötelező, és ha nincs megadva, a dinamikus port toohello végpont van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="95096-120">hello port is optional and if omitted, a dynamic port is assigned toohello endpoint.</span></span> <span data-ttu-id="95096-121">Porttartomány is használható.</span><span class="sxs-lookup"><span data-stu-id="95096-121">A port range can be used.</span></span> <span data-ttu-id="95096-122">Szerepkör / öt belső végpontok korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="95096-122">There is a limit of five internal endpoints per role.</span></span>

<span data-ttu-id="95096-123">hello belső végpont használhatja a következő protokollok hello: **http, az tcp, udp, bármely**.</span><span class="sxs-lookup"><span data-stu-id="95096-123">hello internal endpoint can use hello following protocols: **http, tcp, udp, any**.</span></span>

<span data-ttu-id="95096-124">toocreate belső bemeneti végpont hozzáadása hello **InternalEndpoint** alárendelt elem toohello **végpontok** elem egy webes vagy feldolgozói szerepkör.</span><span class="sxs-lookup"><span data-stu-id="95096-124">toocreate an internal input endpoint, add hello **InternalEndpoint** child element toohello **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

<span data-ttu-id="95096-125">Porttartomány is használhatja.</span><span class="sxs-lookup"><span data-stu-id="95096-125">You can also use a port range.</span></span>

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a><span data-ttu-id="95096-126">Feldolgozói szerepkörök vs. Webes szerepkörök</span><span class="sxs-lookup"><span data-stu-id="95096-126">Worker roles vs. Web roles</span></span>
<span data-ttu-id="95096-127">A végpontok egy kisebb különbség van, ha a munkavégző és a webes szerepkörök használata.</span><span class="sxs-lookup"><span data-stu-id="95096-127">There is one minor difference with endpoints when working with both worker and web roles.</span></span> <span data-ttu-id="95096-128">hello webes szerepkörnek rendelkeznie kell legalább egy bemeneti végpontot hello segítségével **HTTP** protokoll.</span><span class="sxs-lookup"><span data-stu-id="95096-128">hello web role must have at minimum a single input endpoint using hello **HTTP** protocol.</span></span>

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after hello first InputEndPoint -->
</Endpoints>
```

## <a name="using-hello-net-sdk-tooaccess-an-endpoint"></a><span data-ttu-id="95096-129">Hello .NET SDK tooaccess végpont használatával</span><span class="sxs-lookup"><span data-stu-id="95096-129">Using hello .NET SDK tooaccess an endpoint</span></span>
<span data-ttu-id="95096-130">hello Azure felügyelt kódtár szerepkör példányok toocommunicate futásidőben módszereket kínál.</span><span class="sxs-lookup"><span data-stu-id="95096-130">hello Azure Managed Library provides methods for role instances toocommunicate at runtime.</span></span> <span data-ttu-id="95096-131">A szerepkör példánya belül futó kódból többi szerepkörpéldányon és a végpontok hello megléte kapcsolatos információkat, valamint hello aktuális szerepkörpéldányt információ kérheti le.</span><span class="sxs-lookup"><span data-stu-id="95096-131">From code running within a role instance, you can retrieve information about hello existence of other role instances and their endpoints, as well as information about hello current role instance.</span></span>

> [!NOTE]
> <span data-ttu-id="95096-132">Információ a szerepkörpéldányok, amelyek a felhőalapú szolgáltatás fut, és legalább egy belső végpont definiáló csak kérheti le.</span><span class="sxs-lookup"><span data-stu-id="95096-132">You can only retrieve information about role instances that are running in your cloud service and that define at least one internal endpoint.</span></span> <span data-ttu-id="95096-133">Egy másik szolgáltatást futtató szerepkör-példányok adatait nem lehet megszerezni.</span><span class="sxs-lookup"><span data-stu-id="95096-133">You cannot obtain data about role instances running in a different service.</span></span>
> 
> 

<span data-ttu-id="95096-134">Használhatja a hello [példányok](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) tulajdonság tooretrieve példánya egy.</span><span class="sxs-lookup"><span data-stu-id="95096-134">You can use hello [Instances](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) property tooretrieve instances of a role.</span></span> <span data-ttu-id="95096-135">Első alkalommal hello [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) tooreturn egy hivatkozást a jelenlegi szerepkörnek toohello példányt, és kövesse a hello [szerepkör](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) tulajdonság tooreturn egy hivatkozási toohello szerepkör magát.</span><span class="sxs-lookup"><span data-stu-id="95096-135">First use hello [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) tooreturn a reference toohello current role instance, and then use hello [Role](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) property tooreturn a reference toohello role itself.</span></span>

<span data-ttu-id="95096-136">Csatlakozás tooa szerepkörpéldányt programozott módon hello .NET SDK használatával, esetén viszonylag egyszerű tooaccess hello végpont-információkat.</span><span class="sxs-lookup"><span data-stu-id="95096-136">When you connect tooa role instance programmatically through hello .NET SDK, it's relatively easy tooaccess hello endpoint information.</span></span> <span data-ttu-id="95096-137">Például miután tooa adott szerepkör környezet már csatlakozott, egy adott végpont ezzel a kóddal hello port kaphat:</span><span class="sxs-lookup"><span data-stu-id="95096-137">For example, after you've already connected tooa specific role environment, you can get hello port of a specific endpoint with this code:</span></span>

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

<span data-ttu-id="95096-138">Hello **példányok** tulajdonság gyűjteményének beolvasása az **RoleInstance** objektumok.</span><span class="sxs-lookup"><span data-stu-id="95096-138">hello **Instances** property returns a collection of **RoleInstance** objects.</span></span> <span data-ttu-id="95096-139">Ez a gyűjtemény mindig hello jelenlegi példány tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="95096-139">This collection always contains hello current instance.</span></span> <span data-ttu-id="95096-140">Ha hello szerepkör nem határoz meg egy belső végpont, hello gyűjteménybe hello aktuális példánya, de nincs más példány.</span><span class="sxs-lookup"><span data-stu-id="95096-140">If hello role does not define an internal endpoint, hello collection includes hello current instance but no other instances.</span></span> <span data-ttu-id="95096-141">hello hello gyűjtemény szerepkörpéldányok száma minden esetben 1 hello esetben, ha nem a belső végpontot hello szerepkör van definiálva.</span><span class="sxs-lookup"><span data-stu-id="95096-141">hello number of role instances in hello collection will always be 1 in hello case where no internal endpoint is defined for hello role.</span></span> <span data-ttu-id="95096-142">Hello szerepkör a belső végpont határozza meg, ha a példányok felderíthető futásidőben, és hello gyűjteményben található példányok száma hello toohello több példányban hello szerepkörhöz hello szolgáltatás konfigurációs fájljában megadott felel meg.</span><span class="sxs-lookup"><span data-stu-id="95096-142">If hello role defines an internal endpoint, its instances are discoverable at runtime, and hello number of instances in hello collection will correspond toohello number of instances specified for hello role in hello service configuration file.</span></span>

> [!NOTE]
> <span data-ttu-id="95096-143">hello Azure felügyelt kódtár nem biztosít a többi szerepkörpéldányon hello állapotának meghatározására szolgáló eszköz, de Megvalósíthat ilyen állapotfigyelő értékelések saját kezűleg ezért ha a szolgáltatás ezt a funkciót.</span><span class="sxs-lookup"><span data-stu-id="95096-143">hello Azure Managed Library does not provide a means of determining hello health of other role instances, but you can implement such health assessments yourself if your service needs this functionality.</span></span> <span data-ttu-id="95096-144">Használhat [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) tooobtain információt szerepkörpéldányokat.</span><span class="sxs-lookup"><span data-stu-id="95096-144">You can use [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) tooobtain information about running role instances.</span></span>
> 
> 

<span data-ttu-id="95096-145">toodetermine hello portszámot a példányon a belső végpont hello használható [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) tulajdonság tooreturn egy Dictionary objektum, amely tartalmazza a végpont nevének és a megfelelő IP-címek és portok.</span><span class="sxs-lookup"><span data-stu-id="95096-145">toodetermine hello port number for an internal endpoint on a role instance, you can use hello [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) property tooreturn a Dictionary object that contains endpoint names and their corresponding IP addresses and ports.</span></span> <span data-ttu-id="95096-146">Hello [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) tulajdonság hello IP-cím és port megadott végpont adja vissza.</span><span class="sxs-lookup"><span data-stu-id="95096-146">hello [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) property returns hello IP address and port for a specified endpoint.</span></span> <span data-ttu-id="95096-147">Hello **PublicIPEndpoint** tulajdonság adja vissza egy elosztott terhelésű végpont hello port.</span><span class="sxs-lookup"><span data-stu-id="95096-147">hello **PublicIPEndpoint** property returns hello port for a load balanced endpoint.</span></span> <span data-ttu-id="95096-148">hello IP cím részében hello **PublicIPEndpoint** tulajdonság nincs használatban.</span><span class="sxs-lookup"><span data-stu-id="95096-148">hello IP address portion of hello **PublicIPEndpoint** property is not used.</span></span>

<span data-ttu-id="95096-149">Íme egy példa, amely megismétli a szerepkörpéldányok.</span><span class="sxs-lookup"><span data-stu-id="95096-149">Here is an example that iterates role instances.</span></span>

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

<span data-ttu-id="95096-150">Íme egy példa egy feldolgozói szerepkört, amely lekérdezi a közzétett hello végpont hello szolgáltatás definition keresztül, és elindítja a kapcsolatfigyelést.</span><span class="sxs-lookup"><span data-stu-id="95096-150">Here is an example of a worker role that gets hello endpoint exposed through hello service definition and starts listening for connections.</span></span>

> [!WARNING]
> <span data-ttu-id="95096-151">Ez a kód csak egy telepített szolgáltatáshoz fog működni.</span><span class="sxs-lookup"><span data-stu-id="95096-151">This code will only work for a deployed service.</span></span> <span data-ttu-id="95096-152">Hello Azure Compute Emulator futtatásakor a szolgáltatás konfigurációs elemek, amelyek közvetlenül átemelésre végpontokat hoz létre (**InstanceInputEndpoint** elemek) figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="95096-152">When running in hello Azure Compute Emulator, service configuration elements that create direct port endpoints (**InstanceInputEndpoint** elements) are ignored.</span></span>
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

## <a name="network-traffic-rules-toocontrol-role-communication"></a><span data-ttu-id="95096-153">Hálózati forgalmi szabályok toocontrol szerepkör kommunikáció</span><span class="sxs-lookup"><span data-stu-id="95096-153">Network traffic rules toocontrol role communication</span></span>
<span data-ttu-id="95096-154">Belső végpont meghatározása után hozzáadhat hálózati forgalom (a létrehozott hello végpontok alapuló) szabályok toocontrol hogyan szerepkörpéldányokat kommunikálhatnak egymással.</span><span class="sxs-lookup"><span data-stu-id="95096-154">After you define internal endpoints, you can add network traffic rules (based on hello endpoints that you created) toocontrol how role instances can communicate with each other.</span></span> <span data-ttu-id="95096-155">hello alábbi ábrán látható közötti szabályozásának olyan gyakori forgatókönyveket tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="95096-155">hello following diagram shows some common scenarios for controlling role communication:</span></span>

<span data-ttu-id="95096-156">![A hálózati forgalom szabályok forgatókönyvek](./media/cloud-services-enable-communication-role-instances/scenarios.png "hálózati forgalmi szabályok forgatókönyvek")</span><span class="sxs-lookup"><span data-stu-id="95096-156">![Network Traffic Rules Scenarios](./media/cloud-services-enable-communication-role-instances/scenarios.png "Network Traffic Rules Scenarios")</span></span>

<span data-ttu-id="95096-157">hello alábbi példakód bemutatja szerepkör-definíciók hello szerepkörök hello előző ábrán is látható.</span><span class="sxs-lookup"><span data-stu-id="95096-157">hello following code example shows role definitions for hello roles shown in hello previous diagram.</span></span> <span data-ttu-id="95096-158">Minden egyes szerepkör-definíció definiált legalább egy belső végpontot tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="95096-158">Each role definition includes at least one internal endpoint defined:</span></span>

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
> <span data-ttu-id="95096-159">Rögzített, és automatikusan hozzárendeli a portok a belső végpontok szerepkörök közötti kommunikáció korlátozásának alakulhat ki.</span><span class="sxs-lookup"><span data-stu-id="95096-159">Restriction of communication between roles can occur with internal endpoints of both fixed and automatically assigned ports.</span></span>
> 
> 

<span data-ttu-id="95096-160">Alapértelmezés szerint belső végpont van definiálva, miután kommunikációs is haladjanak bármely szerepkör toohello belső végpont egy szerepkör korlátozások nélkül.</span><span class="sxs-lookup"><span data-stu-id="95096-160">By default, after an internal endpoint is defined, communication can flow from any role toohello internal endpoint of a role without any restrictions.</span></span> <span data-ttu-id="95096-161">toorestrict kommunikációt, hozzá kell adnia egy **NetworkTrafficRules** elem toohello **ServiceDefinition** elem hello szolgáltatásdefiníciós fájlban.</span><span class="sxs-lookup"><span data-stu-id="95096-161">toorestrict communication, you must add a **NetworkTrafficRules** element toohello **ServiceDefinition** element in hello service definition file.</span></span>

### <a name="scenario-1"></a><span data-ttu-id="95096-162">1. forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="95096-162">Scenario 1</span></span>
<span data-ttu-id="95096-163">Csak a hálózati forgalom engedélyezése **WebRole1** túl**WorkerRole1**.</span><span class="sxs-lookup"><span data-stu-id="95096-163">Only allow network traffic from **WebRole1** too**WorkerRole1**.</span></span>

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

### <a name="scenario-2"></a><span data-ttu-id="95096-164">2. forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="95096-164">Scenario 2</span></span>
<span data-ttu-id="95096-165">Csak lehetővé teszi a hálózati forgalmat a **WebRole1** túl**WorkerRole1** és **WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="95096-165">Only allows network traffic from **WebRole1** too**WorkerRole1** and **WorkerRole2**.</span></span>

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

### <a name="scenario-3"></a><span data-ttu-id="95096-166">3. forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="95096-166">Scenario 3</span></span>
<span data-ttu-id="95096-167">Csak lehetővé teszi a hálózati forgalmat a **WebRole1** túl**WorkerRole1**, és **WorkerRole1** túl**WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="95096-167">Only allows network traffic from **WebRole1** too**WorkerRole1**, and **WorkerRole1** too**WorkerRole2**.</span></span>

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

### <a name="scenario-4"></a><span data-ttu-id="95096-168">4. forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="95096-168">Scenario 4</span></span>
<span data-ttu-id="95096-169">Csak lehetővé teszi a hálózati forgalmat a **WebRole1** túl**WorkerRole1**, **WebRole1** túl**WorkerRole2**, és  **WorkerRole1** túl**WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="95096-169">Only allows network traffic from **WebRole1** too**WorkerRole1**, **WebRole1** too**WorkerRole2**, and **WorkerRole1** too**WorkerRole2**.</span></span>

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

<span data-ttu-id="95096-170">Az XML-séma hivatkozása fenti hello elemek található [Itt](https://msdn.microsoft.com/library/azure/gg557551.aspx).</span><span class="sxs-lookup"><span data-stu-id="95096-170">An XML schema reference for hello elements used above can be found [here](https://msdn.microsoft.com/library/azure/gg557551.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="95096-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="95096-171">Next steps</span></span>
<span data-ttu-id="95096-172">További részletek a felhőalapú szolgáltatás hello [modell](cloud-services-model-and-package.md).</span><span class="sxs-lookup"><span data-stu-id="95096-172">Read more about hello Cloud Service [model](cloud-services-model-and-package.md).</span></span>

