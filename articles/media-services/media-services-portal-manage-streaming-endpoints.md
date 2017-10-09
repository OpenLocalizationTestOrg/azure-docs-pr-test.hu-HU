---
title: "adatfolyam-végpontok az Azure-portálon hello aaaManage |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toomanage streamvégpontok a hello Azure-portálon."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: dfa9352894d37edb317a6334d7f109419deb362b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-streaming-endpoints-with-hello-azure-portal"></a>Adatfolyam-végpontokat kezelheti a hello Azure-portálon

Ez a témakör bemutatja, hogyan toouse hello Azure portál toomanage streamvégpontok. 

>[!NOTE]
>Győződjön meg arról, hogy tooreview hello [áttekintése](media-services-streaming-endpoints-overview.md) témakör. 

Hogyan tooscale hello streamvégpont kapcsolatos információkért lásd: [ez](media-services-portal-scale-streaming-endpoints.md) témakör.

## <a name="start-managing-streaming-endpoints"></a>Adatfolyam-továbbítási végpontok kezelése – első lépések 

a fiók streamvégpontok toostart hello követően.

1. A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.
2. A hello **beállítások** panelen válassza **adatfolyam-végpontok**.
   
    ![Adatfolyam-továbbítási végpontra](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> Csak számlázása, amikor a Streamvégpontot futó állapotban van.

## <a name="adddelete-a-streaming-endpoint"></a>A streamvégpont hozzáadása vagy törlése

>[!NOTE]
>hello alapértelmezett streamvégpontból nem lehet törölni.

adatfolyam-végpontot a tooadd vagy törlése hello Azure-portálon, a következő hello:

1. a streamvégpont tooadd hello kattintson **+ végpont** hello oldal hello tetején. 

    Érdemes lehet több adatfolyam-végpontot, ha azt tervezi, hogy toohave különböző tartalomtovábbító vagy a CDN és a közvetlen hozzáférést.

2. toodelete streamvégpontra, nyomja le az ENTER **törlése** gombra.      
3. Kattintson a hello **Start** gomb toostart hello streamvégpontra.
   
    ![Adatfolyam-továbbítási végpontra](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <a id="configure_streaming_endpoints"></a>Hello Streaming Endpoint konfigurálása
Adatfolyam-továbbítási végpontra lehetővé teszi tooconfigure hello következő tulajdonságai:

* Hozzáférés-vezérlés
* Gyorsítótár-vezérlő
* Kereszt-hely hozzáférési házirendek

Ezek a Tulajdonságok kapcsolatos részletes információkért lásd: [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).

A streamvégpont hello következő tevékenységek végrehajtásával konfigurálhatja:

1. Válassza ki a hello tooconfigure kívánt streamvégpontra.
2. Kattintson a **beállítások**.

A következő hello mezők rövid leírását.

![Adatfolyam-továbbítási végpontra](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. A gyorsítótár maximális házirend: használt tooconfigure gyorsítótár élettartamát az eszközök kiszolgált keresztül a streamvégpontra. Ha nincs megadva érték, hello alapértelmezett szolgál. hello alapértelmezett értékeket közvetlenül az Azure storage-ban is definiálhatók. Adatfolyam-továbbítási végpontra hello Azure CDN engedélyezett, ha nem kellene hello gyorsítótár házirend érték tooless mint 600 másodperc.  
2. Engedélyezett IP-címek: használt toospecify IP-címeket, akkor engedélyezni tooconnect toohello közzétett streamvégpontra. Ha nincs megadva IP-címeket, IP-címeket tud tooconnect lesz. IP-címeket adhat meg egyetlen IP-címet (például 10.0.0.1), egy IP-címet és egy CIDR alhálózati maszk (például "10.0.0.1/22") IP-címtartomány vagy egy IP-címtartomány IP-címet és egy pontozott decimális alhálózati maszk segítségével (például "10.0.0.1(255.255.255.0)').
3. Akamai aláírás fejléc hitelesítési konfigurációját: használt toospecify aláírás fejléc hitelesítési kérelem Akamai kiszolgálókról konfigurálását. Lejáratának UTC formátumban van.

## <a name="scale-your-premium-streaming-endpoint"></a>Az adatfolyam-továbbítási végpontra prémium méretezése

További információ [ebben](media-services-portal-scale-streaming-endpoints.md) a témakörben érhető el.

## <a id="enable_cdn"></a>Azure CDN-integráció engedélyezése

Amikor létrehoz egy új fiókot, alapértelmezett végpont Azure CDN adatfolyam-integráció alapértelmezés szerint engedélyezve van.

Ha később szeretné toodisable/enable hello CDN, az adatfolyam-továbbítási végpontjának hello kell **leállt** állapotát. Hello Azure CDN integrációs tooget engedélyezve van és aktív hello módosítások toobe too2 óra között az összes CDN POP hello eltarthat. Azonban Ön indítsa el a streamelési végpont és működésének felfüggesztése nélkül adatfolyam adatfolyam-továbbítási végpontra hello és hello integrációs végrehajtása után hello adatfolyam kézbesíti a rendszer a hello CDN. Hello időszak kiépítése során a streamvégpontján lesz **indítása** állapotát, és előfordulhat, hogy tekintse át az degredad teljesítményét.

A minden hello az Azure data központok execpt Kína és szövetségi kormányzati régiók engedélyezve van a CDN-integráció.

Ha engedélyezve van, hello **hozzáférés-vezérlés**, **egyéni állomásnév** és **Akamai aláírás hitelesítési** beállítások lekérdezi engedélyezve.
 
> [!IMPORTANT]
> Azure Media Services-integráció az Azure CDN van megvalósítva a **Azure CDN Verizon** standard adatfolyam-végpontok. Prémium szintű adatfolyam-végpontok beállítható, hogy az összes **árképzési szinteket, és szolgáltatók Azure CDN**. Azure CDN funkciókkal kapcsolatos további információkért lásd: hello [CDN áttekintésével](../cdn/cdn-overview.md).
 
### <a name="additional-considerations"></a>Néhány fontos megjegyzés

* CDN engedélyezve van a streamvégpontján, amikor az ügyfelek tartalmat nem lehet közvetlenül hello forrásból igényelnek. Ha hello képességét tootest kell a tartalmat, vagy a CDN nélkül, egy másik streamvégpontra, amely nem engedélyezett CDN is létrehozhat.
* A folyamatos átviteli végpont állomásnév marad hello azonos CDN engedélyezése után. Nincs szükség a toomake bármely módosítások tooyour media services munkafolyamat CDN engedélyezése után. Például ha a folyamatos átviteli végpont állomásnevéhez strasbourg.streaming.mediaservices.windows.net, miután engedélyezte a CDN, használatos hello pontos azonos állomásnévvel.
* Az új streamvégpontok engedélyezéséhez CDN egyszerűen egy új végpont; létrehozása meglévő streamvégpontok kell toofirst stop hello végpont és engedélyezését vagy letiltását hello CDN.
* Adatfolyam-továbbítási végpontra standard csak használatával konfigurálható **Verizon standard szintű CDN-szolgáltató** az Azure felügyeleti portáljáról. Azonban más Azure CDN szolgáltatók REST API-k használatával engedélyezheti.

## <a name="configure-cdn-profile"></a>CDN-profil konfigurálása

CDN-profil hello hello kiválasztásával konfigurálhatja **kezelése CDN** hello elejéről gombra.

![Adatfolyam-továbbítási végpontra](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a>Következő lépések
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

