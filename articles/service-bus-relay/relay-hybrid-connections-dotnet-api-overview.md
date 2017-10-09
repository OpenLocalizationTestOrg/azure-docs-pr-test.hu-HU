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
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a>Azure Relay a hibrid kapcsolatok .NET Standard API – áttekintés

Ez a cikk foglal össze néhányat hello kulcs Azure Relay a hibrid kapcsolatok .NET szabványos [ügyfél API-k](/dotnet/api/microsoft.azure.relay).
  
## <a name="relay-connection-string-builder"></a>Továbbítási kapcsolati karakterlánc-szerkesztő

Hello [RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] osztály formázza a kapcsolati karakterláncok, amelyek adott tooRelay hibrid kapcsolatok. Használhatja a kapcsolati karakterlánc, illetve ha teljesen új kapcsolati karakterlánc toobuild tooverify hello formátuma. Hello a következő kód egy vonatkozó példáért lásd:

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

Is átadhatja a kapcsolati karakterlánc közvetlenül toohello `RelayConnectionStringBuilder` metódust. Ez a művelet lehetővé teszi tooverify, amely hello kapcsolati karakterlánc formátuma érvénytelen. Ha bármelyik hello paraméter érvénytelen, hello konstruktor hoz létre egy `ArgumentException`.

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

## <a name="hybrid-connection-stream"></a>A hibrid kapcsolat adatfolyam
Hello [HybridConnectionStream] [ HCStream] osztály hello használt elsődleges objektum toosend, és hogy dolgozunk fogadhat adatokat Azure továbbítási végpontok egy [HybridConnectionClient] [ HCClient], vagy egy [HybridConnectionListener][HCListener].

### <a name="getting-a-hybrid-connection-stream"></a>A hibrid kapcsolat adatfolyam beolvasása

#### <a name="listener"></a>Figyelő
Használatával egy [HybridConnectionListener][HCListener], beszerezhet egy `HybridConnectionStream` objektumot az alábbiak szerint:

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection toohello Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a>Ügyfél
Használatával egy [HybridConnectionClient][HCClient], beszerezhet egy `HybridConnectionStream` objektumot az alábbiak szerint:

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection toohello Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a>Adatok fogadása
Hello [HybridConnectionStream] [ HCStream] osztály lehetővé teszi, hogy a kétirányú kommunikációt. A legtöbb esetben folyamatosan kapott hello adatfolyam. Hello adatfolyamból olvassák a szöveg, ha is érdemes lehet toouse egy [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) objektum, amely lehetővé teszi, hogy könnyebben hello adatok elemzése. Például olvasható adatok szövegként, nem `byte[]`.

hello alábbira olvassa be az egyes sornyi szöveget hello adatfolyam amíg törlése van szükség:

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

### <a name="sending-data"></a>Adatok küldése
Ha már létrehozott kapcsolat, egy üzenet toohello továbbítási végpontjának küldhet. Mert hello kapcsolatobjektumot örökli [adatfolyam](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), mint az adatok küldése a `byte[]`. a következő példa azt mutatja meg hogyan hello toodo ezt:

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

Azonban ha azt szeretné, toosend szöveg közvetlenül, minden alkalommal tooencode hello karakterlánc anélkül is burkolása hello `hybridConnectionStream` rendelkező objektum egy [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) objektum.

```csharp
// hello StreamWriter object only needs toobe created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a>Következő lépések
További információ az Azure továbbítási toolearn látogasson el ezeket a hivatkozásokat:

* [Microsoft.Azure.Relay hivatkozás](/dotnet/api/microsoft.azure.relay)
* [Mi az az Azure Relay?](relay-what-is-it.md)
* [Rendelkezésre álló továbbítási API-k](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener