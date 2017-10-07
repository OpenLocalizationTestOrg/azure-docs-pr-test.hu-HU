---
title: "aaaIssuer nevét és a BizTalk szolgáltatások Issuer Key |} Microsoft Docs"
description: "Megtudhatja, hogyan tooretrieve Kiállítónevet és Issuer Key Service Bus vagy Access Control (ACS) a BizTalk szolgáltatások. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: cc84c2820724ae3e7fc7c40ddbcd83a169add911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk Services: Kiállító neve és kiállító kulcsa

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Az Azure BizTalk szolgáltatások hello Service Bus kibocsátó neve és Issuer Key és hello Access Control kibocsátó neve és Issuer Key használja. Konkrétan:

| Tevékenység | Mely kibocsátó neve és Issuer Key |
| --- | --- |
| A Visual Studio az alkalmazás telepítéséhez |Hozzáférés-vezérlő Kibocsátónév és Issuer Key |
| Hello Azure BizTalk szolgáltatások portál konfigurálása |Hozzáférés-vezérlő Kibocsátónév és Issuer Key |
| LOB-továbbítók hello BizTalk Adapter szolgáltatások a Visual Studio létrehozása |Service Bus kibocsátó neve és Issuer Key |

Ez a témakör a hello lépéseket tooretrieve hello kibocsátó neve és Issuer Key sorolja fel. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Hozzáférés-vezérlő Kibocsátónév és Issuer Key
hello Access Control kibocsátó neve és Issuer Key hello következő használják:

* Az Azure BizTalk szolgáltatás alkalmazás létrehozása a Visual Studio: toosuccessfully a Visual Studio tooAzure BizTalk szolgáltatás alkalmazást helyezi üzembe, adja meg a hozzáférés-vezérlő Kibocsátónév hello és Issuer Key. 
* hello Azure BizTalk szolgáltatások portálja: BizTalk szolgáltatás létrehozása, és nyissa meg a hello BizTalk Services portálra, a hozzáférés-vezérlési kibocsátó neve és Issuer Key automatikusan be vannak jegyezve a telepítések esetén a hello értékeitől hozzáférés-vezérlést.

### <a name="get-hello-access-control-issuer-name-and-issuer-key"></a>Első Access Control kibocsátó neve és Issuer Key hello

a hitelesítés és a get toouse ACS hello kibocsátó neve és Issuer Key értékek, hello általános lépéseket tartalmazza:

1. Telepítse a hello [Azure Powershell-parancsmagok](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Adja hozzá az Azure-fiókjával:`Add-AzureAccount`
3. Az előfizetés nevét adja vissza:`get-azuresubscription`
4. Jelölje ki az előfizetését:`select-azuresubscription <name of your subscription>` 
5. Új névtér létrehozása:`new-azuresbnamespace <name for hello service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`

    Példa:`new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`
      
5. Hello új ACS névtér (amely több percet is igénybe vehet) jön létre, amikor hello kibocsátó neve és Issuer Key értékek listáját hello kapcsolati karakterlánc: 

    ```
    Name                  : biztalksbnamespace
    Region                : South Central US
    DefaultKey            : abcdefghijklmnopqrstuvwxyz
    Status                : Active
    CreatedAt             : 10/18/2016 9:36:30 PM
    AcsManagementEndpoint : https://biztalksbnamespace-sb.accesscontrol.windows.net/
    ServiceBusEndpoint    : https://biztalksbnamespace.servicebus.windows.net/
    ConnectionString      : Endpoint=sb://biztalksbnamespace.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue=abcdefghijklmnopqrstuvwxyz
    NamespaceType         : Messaging
    ```

toosummarize:  
Kiállító neve = SharedSecretIssuer  
Kiállító kulcsát = SharedSecretKey

A hello további [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) parancsmag. 

## <a name="service-bus-issuer-name-and-issuer-key"></a>Service Bus kibocsátó neve és Issuer Key
Service Bus kibocsátó neve és Issuer Key BizTalk Adapter szolgáltatások használják. BizTalk szolgáltatások projektre a Visual Studio, a hello BizTalk Adapter szolgáltatások tooconnect tooan helyszíni üzletági (LOB) rendszert használ. tooconnect, hello LOB Relay létrehozásához, és adja meg a LOB-rendszer részleteit. Ennek során is be hello Service Bus kibocsátó neve és Issuer Key.

### <a name="tooretrieve-hello-service-bus-issuer-name-and-issuer-key"></a>Service Bus Kibocsátónév tooretrieve hello és Issuer Key
1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Hello bal oldali navigációs panelen, jelölje ki a **Service Bus**.
3. Válassza ki a névteret. Hello tálcán, válassza ki a **kapcsolatadatok**. Ez megjeleníti a hello **alapértelmezett kibocsátó** (kibocsátó neve) és **alapértelmezett kulcs** (Issuer Key). Az értékekre másolhatók.  

toosummarize:  
Kiállító neve alapértelmezett kibocsátó =  
Kiállító kulcsát alapértelmezett kulcs =

## <a name="next"></a>Következő lépés
További Azure BizTalk szolgáltatások témakörök:

* [Hello Azure BizTalk szolgáltatások SDK telepítése](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Oktatóanyag: Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Az Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Lásd még:
* [Hogyan: használata ACS felügyeleti szolgáltatás tooConfigure szolgáltatás-identitások](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Kiépítési állapot diagramja](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Irányítópult, Figyelés és Méret lapok](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Biztonsági mentés és visszaállítás](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Szabályozás](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

