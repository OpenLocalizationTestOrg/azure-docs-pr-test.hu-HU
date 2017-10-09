---
title: "az Azure továbbítási .NET szabványos API-k hello aaaOverview |} Microsoft Docs"
description: "Továbbítják a .NET-szabvány API – áttekintés"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b1da9ac1-811b-4df7-a22c-ccd013405c40
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: sethm
ms.openlocfilehash: c90e00e809bd44eb0fbbff5eb03dfc8afa486523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a><span data-ttu-id="5a4fd-103">Azure Relay a hibrid kapcsolatok .NET Standard API – áttekintés</span><span class="sxs-lookup"><span data-stu-id="5a4fd-103">Azure Relay Hybrid Connections .NET Standard API overview</span></span>

<span data-ttu-id="5a4fd-104">Ez a cikk foglal össze néhányat hello kulcs Azure Relay a hibrid kapcsolatok .NET szabványos [ügyfél API-k](/dotnet/api/microsoft.azure.relay).</span><span class="sxs-lookup"><span data-stu-id="5a4fd-104">This article summarizes some of hello key Azure Relay Hybrid Connections .NET Standard [client APIs](/dotnet/api/microsoft.azure.relay).</span></span>
  
## <a name="relay-connection-string-builder"></a><span data-ttu-id="5a4fd-105">Továbbítási kapcsolati karakterlánc-szerkesztő</span><span class="sxs-lookup"><span data-stu-id="5a4fd-105">Relay Connection String Builder</span></span>

<span data-ttu-id="5a4fd-106">Hello [RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] osztály formázza a kapcsolati karakterláncok, amelyek adott tooRelay hibrid kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-106">hello [RelayConnectionStringBuilder][RelayConnectionStringBuilder] class formats connection strings that are specific tooRelay Hybrid Connections.</span></span> <span data-ttu-id="5a4fd-107">Használhatja a kapcsolati karakterlánc, illetve ha teljesen új kapcsolati karakterlánc toobuild tooverify hello formátuma.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-107">You can use it tooverify hello format of a connection string, or toobuild a connection string from scratch.</span></span> <span data-ttu-id="5a4fd-108">Hello a következő kód egy vonatkozó példáért lásd:</span><span class="sxs-lookup"><span data-stu-id="5a4fd-108">See hello following code for an example:</span></span>

```csharp
var endpoint = "{Relay namespace}";
var entityPath = "{Name of hello Hybrid Connection}";
var sharedAccessKeyName = "{SAS key name}";
var sharedAccessKey = "{SAS key value}";

var connectionStringBuilder = new RelayConnectionStringBuilder()
{
    Endpoint = endpoint,
    EntityPath = entityPath,
    SharedAccessKeyName = sasKeyName,
    SharedAccessKey = sasKeyValue
};
```

<span data-ttu-id="5a4fd-109">Is átadhatja a kapcsolati karakterlánc közvetlenül toohello `RelayConnectionStringBuilder` metódust.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-109">You can also pass a connection string directly toohello `RelayConnectionStringBuilder` method.</span></span> <span data-ttu-id="5a4fd-110">Ez a művelet lehetővé teszi tooverify, amely hello kapcsolati karakterlánc formátuma érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-110">This operation enables you tooverify that hello connection string is in a valid format.</span></span> <span data-ttu-id="5a4fd-111">Ha bármelyik hello paraméter érvénytelen, hello konstruktor hoz létre egy `ArgumentException`.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-111">If any of hello parameters are invalid, hello constructor generates an `ArgumentException`.</span></span>

```csharp
var myConnectionString = "{RelayConnectionString}";
// Declare hello connectionStringBuilder so that it can be used outside of hello loop if needed
RelayConnectionStringBuilder connectionStringBuilder;
try
{
    // Create hello connectionStringBuilder using hello supplied connection string
    connectionStringBuilder = new RelayConnectionStringBuilder(myConnectionString);
}
catch (ArgumentException ae)
{
    // Perform some error handling
}
```

## <a name="hybrid-connection-stream"></a><span data-ttu-id="5a4fd-112">A hibrid kapcsolat adatfolyam</span><span class="sxs-lookup"><span data-stu-id="5a4fd-112">Hybrid Connection Stream</span></span>
<span data-ttu-id="5a4fd-113">Hello [HybridConnectionStream] [ HCStream] osztály hello használt elsődleges objektum toosend, és hogy dolgozunk fogadhat adatokat Azure továbbítási végpontok egy [HybridConnectionClient] [ HCClient], vagy egy [HybridConnectionListener][HCListener].</span><span class="sxs-lookup"><span data-stu-id="5a4fd-113">hello [HybridConnectionStream][HCStream] class is hello primary object used toosend and receive data from an Azure Relay endpoint, whether you are working with a [HybridConnectionClient][HCClient], or a [HybridConnectionListener][HCListener].</span></span>

### <a name="getting-a-hybrid-connection-stream"></a><span data-ttu-id="5a4fd-114">A hibrid kapcsolat adatfolyam beolvasása</span><span class="sxs-lookup"><span data-stu-id="5a4fd-114">Getting a Hybrid Connection Stream</span></span>

#### <a name="listener"></a><span data-ttu-id="5a4fd-115">Figyelő</span><span class="sxs-lookup"><span data-stu-id="5a4fd-115">Listener</span></span>
<span data-ttu-id="5a4fd-116">Használatával egy [HybridConnectionListener][HCListener], beszerezhet egy `HybridConnectionStream` objektumot az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5a4fd-116">Using a [HybridConnectionListener][HCListener], you can obtain a `HybridConnectionStream` object as follows:</span></span>

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection toohello Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a><span data-ttu-id="5a4fd-117">Ügyfél</span><span class="sxs-lookup"><span data-stu-id="5a4fd-117">Client</span></span>
<span data-ttu-id="5a4fd-118">Használatával egy [HybridConnectionClient][HCClient], beszerezhet egy `HybridConnectionStream` objektumot az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5a4fd-118">Using a [HybridConnectionClient][HCClient], you can obtain a `HybridConnectionStream` object as follows:</span></span>

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection toohello Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a><span data-ttu-id="5a4fd-119">Adatok fogadása</span><span class="sxs-lookup"><span data-stu-id="5a4fd-119">Receiving data</span></span>
<span data-ttu-id="5a4fd-120">Hello [HybridConnectionStream] [ HCStream] osztály lehetővé teszi, hogy a kétirányú kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-120">hello [HybridConnectionStream][HCStream] class enables two-way communication.</span></span> <span data-ttu-id="5a4fd-121">A legtöbb esetben folyamatosan kapott hello adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-121">In most cases, you continuously receive from hello stream.</span></span> <span data-ttu-id="5a4fd-122">Hello adatfolyamból olvassák a szöveg, ha is érdemes lehet toouse egy [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) objektum, amely lehetővé teszi, hogy könnyebben hello adatok elemzése.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-122">If you are reading text from hello stream, you may also want toouse a [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) object, which enables easier parsing of hello data.</span></span> <span data-ttu-id="5a4fd-123">Például olvasható adatok szövegként, nem `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-123">For example, you can read data as text, rather than as `byte[]`.</span></span>

<span data-ttu-id="5a4fd-124">hello alábbira olvassa be az egyes sornyi szöveget hello adatfolyam amíg törlése van szükség:</span><span class="sxs-lookup"><span data-stu-id="5a4fd-124">hello following code reads individual lines of text from hello stream until a cancellation is requested:</span></span>

```csharp
// Create a CancellationToken, so that we can cancel hello while loop
var cancellationToken = new CancellationToken();
// Create a StreamReader from hello 'hybridConnectionStream`
var streamReader = new StreamReader(hybridConnectionStream);

while (!cancellationToken.IsCancellationRequested)
{
    // Read a line of input until a newline is encountered
    var line = await streamReader.ReadLineAsync();
    if (string.IsNullOrEmpty(line))
    {
        // If there's no input data, we will signal that 
        // we will no longer send data on this connection
        // and then break out of hello processing loop.
        await hybridConnectionStream.ShutdownAsync(cancellationToken);
        break;
    }
}
```

### <a name="sending-data"></a><span data-ttu-id="5a4fd-125">Adatok küldése</span><span class="sxs-lookup"><span data-stu-id="5a4fd-125">Sending data</span></span>
<span data-ttu-id="5a4fd-126">Ha már létrehozott kapcsolat, egy üzenet toohello továbbítási végpontjának küldhet.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-126">Once you have a connection established, you can send a message toohello Relay endpoint.</span></span> <span data-ttu-id="5a4fd-127">Mert hello kapcsolatobjektumot örökli [adatfolyam](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), mint az adatok küldése a `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-127">Because hello connection object inherits [Stream](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), send your data as a `byte[]`.</span></span> <span data-ttu-id="5a4fd-128">a következő példa azt mutatja meg hogyan hello toodo ezt:</span><span class="sxs-lookup"><span data-stu-id="5a4fd-128">hello following example shows how toodo this:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

<span data-ttu-id="5a4fd-129">Azonban ha azt szeretné, toosend szöveg közvetlenül, minden alkalommal tooencode hello karakterlánc anélkül is burkolása hello `hybridConnectionStream` rendelkező objektum egy [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) objektum.</span><span class="sxs-lookup"><span data-stu-id="5a4fd-129">However, if you want toosend text directly, without needing tooencode hello string each time, you can wrap hello `hybridConnectionStream` object with a [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) object.</span></span>

```csharp
// hello StreamWriter object only needs toobe created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a><span data-ttu-id="5a4fd-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a4fd-130">Next steps</span></span>
<span data-ttu-id="5a4fd-131">További információ az Azure továbbítási toolearn látogasson el ezeket a hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="5a4fd-131">toolearn more about Azure Relay, visit these links:</span></span>

* [<span data-ttu-id="5a4fd-132">Microsoft.Azure.Relay hivatkozás</span><span class="sxs-lookup"><span data-stu-id="5a4fd-132">Microsoft.Azure.Relay reference</span></span>](/dotnet/api/microsoft.azure.relay)
* [<span data-ttu-id="5a4fd-133">Mi az az Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="5a4fd-133">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="5a4fd-134">Rendelkezésre álló továbbítási API-k</span><span class="sxs-lookup"><span data-stu-id="5a4fd-134">Available Relay APIs</span></span>](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener