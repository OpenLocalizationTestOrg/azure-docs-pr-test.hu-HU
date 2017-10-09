---
title: "aaaUsing hello Azure CDN tooaccess blobok az egyéni tartomány HTTPS-KAPCSOLATON keresztül"
description: "Ismerje meg, hogyan toointegrate hello Azure CDN szolgáltatás használata a blob storage tooaccess blobok az egyéni tartomány HTTPS-KAPCSOLATON keresztül"
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mihauss
ms.openlocfilehash: 678e24a7dde5cb2f8feea177bb47c92f61035e66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cdn-tooaccess-blobs-with-custom-domains-over-https"></a>Hello Azure CDN tooaccess blobs használata az egyéni tartomány HTTPS-KAPCSOLATON keresztül

Az Azure Content Delivery Network (CDN) HTTPS mostantól támogatja az egyéni tartománynevek.
Kihasználhatja a szolgáltatás tooaccess storage blobs szolgáltatásban az egyéni tartomány over HTTPS használatával. toodo Igen, először tooenable Azure CDN szolgáltatás használata a blob végpont és a térkép hello CDN tooa egyéni tartománynév. Ha ezeket a lépéseket HTTPS engedélyezése az egyéni tartomány egyszerűsített keresztül egy kattintással engedélyezését, teljes tanúsítványkezelés, és nem jelent többletköltséget toonormal a CDN-díjszabást mindezt.

Ez a lehetőség azért fontos, mert azt lehetővé teszi, hogy Ön tooprotect hello adatok biztonságát és sértetlenségét a bizalmas webes alkalmazás adatok az átvitel során. Hello SSL protokoll tooserve forgalom keresztül HTTPS használatával biztosítja az adatok titkosítását, ha hello keresztül továbbítja azokat internet. HTTPS megbízhatóság és a hitelesítési biztosít, valamint a webalkalmazások védi a támadások ellen.

> [!NOTE]
> Továbbá SSL támogatja az egyéni tartománynevek, hello Azure CDN tooproviding segítséget az alkalmazás toodeliver tartalmak nagy sávszélességű méretezése hello világ.
> toolearn több, tekintse meg [hello Azure CDN áttekintése](../cdn/cdn-overview.md).
>
>

## <a name="quick-start"></a>Első lépések

Hello lépéseket szükséges tooenable HTTPS a egyéni blob storage-végponthoz a következők:

1.  [Azure-tárfiók integrálása az Azure CDN](../cdn/cdn-create-a-storage-account-with-cdn.md).
    Ez a cikk végigvezeti a storage-fiók létrehozása a hello Azure portálon, ha még nem meg már.
2.  [Térkép Azure CDN-tartalom tooa egyéni tartomány](../cdn/cdn-map-content-to-custom-domain.md).
3.  [Engedélyezze a HTTPS, egyéni Azure CDN-tartomány](../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Megosztott hozzáférési aláírásokkal

Ha a blob storage endpoint konfigurált toodisallow névtelen olvasási hozzáférés, szüksége lesz a tooprovide egy [közös hozzáférésű Jogosultságkód (SAS)](storage-dotnet-shared-access-signature-part-1.md) tooyour egyéni tartomány elvégezte az egyes kérelmek token. Alapértelmezés szerint a blob storage végpontok letiltása a névtelen olvasási hozzáférés. Lásd: [tárolók és blobok névtelen olvasási hozzáférés kezelése](storage-manage-access-to-resources.md) közös hozzáférésű jogosultságkód olvashat.

Az Azure CDN korlátozások hozzáadott toohello SAS-jogkivonat nem veszi figyelembe. Például minden SAS-tokenje rendelkezik lejárati időt. Ez azt jelenti, hogy a tartalom továbbra is elérhető a egy lejárt SAS használatával, amíg a tartalom hello CDN peremhálózati csomópontjáról véglegesen törlődnek. Azt is szabályozhatja, hogy mennyi ideig adatok hello CDN beállítása hello gyorsítótár válasz fejléc gyorsítótárában van. Lásd: [Azure Storage blobs szolgáltatásban az Azure CDN kezelésében](../cdn/cdn-manage-expiration-of-blob-content.md) utasításokat.

Ha hoz létre, hogy több SAS URL-címéből hello azonos blob végpont, ajánlott bekapcsolja a lekérdezési karakterláncok gyorsítótárazása az az Azure CDN szolgáltatás használata. Ez a tooensure egyedi entitásként kezelt valamennyi URL-címet. Lásd: [szabályozása Azure CDN a lekérdezési karakterláncok gyorsítótárazásának](../cdn/cdn-query-string.md) további információt.

## <a name="http-toohttps-redirection"></a>A HTTP-átirányítás tooHTTPS

HTTP-forgalom tooHTTPS tooredirect választhatja. Ehhez a hello Azure CDN premium ajánlat verizon használatát. Túl kell[használata az Azure CDN szabályok felülbírálása HTTP viselkedés](../cdn/cdn-rules-engine.md) a következő szabálynak:

![](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

"Cdn-végpont-name" CDN-végpont beállított toohello nevét jelenti. Hello legördülő menüből kiválaszthatja ezt az értéket. "Forrás-path" hello elérési belül a statikus tartalmat tároló származási tárfiókját hivatkozik.
Ha egyetlen tároló összes statikus tartalmat tároló, cserélje le a "forrás-path" hello nevű tároló.

Mélyebb bemutatója szabályokat, talál hello [Azure CDN szabályok motor szolgáltatások](../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Árak és számlázás

Egy Azure CDN keresztül érhetők el a blobokat, amikor kell fizetnie [Blob-tároló árak](https://azure.microsoft.com/pricing/details/storage/blobs/) hello peremhálózati csomópontok és hello származási (Blob-tároló), közötti forgalom és [CDN árak](https://azure.microsoft.com/pricing/details/cdn/) hello peremhálózati csomópontok elért adatok számára.

Tegyük fel például, hogy a storage-fiókot, amely használatban van egy Azure CDN használatával USA nyugati régiója. Esetén a hello UK hello egyik hello CDN keresztül a tárfiókban lévő blobok tooaccess, Azure először ellenőrzi hello élcsomópont legközelebbi hello UK az, hogy a blob. Ha található, akkor éri el ezt a példányt hello blob és fogja használni a CDN-díjszabást, mivel a hello CDN van használatban. Ha nem található, Azure másolatot készít a hello blob toohello élcsomóponthoz, ami kilépő eredményezi, és a megadott hello tranzakciós díjakat Blob storage szolgáltatás díjszabása, és hello fájl csomóponton hello él, ami CDN számlázási eredményezi majd hozzáférni.

Ha hello [árképzést ismertető oldalra CDN](https://azure.microsoft.com/pricing/details/cdn/), vegye figyelembe, hogy HTTPS egyéni tartománynevek támogatása csak érhető el az Azure CDN Verizon termékekből (Standard és prémium).

## <a name="next-steps"></a>Következő lépések

[A Blob storage-végponthoz egyéni tartománynév konfigurálása](storage-custom-domain-name.md)
