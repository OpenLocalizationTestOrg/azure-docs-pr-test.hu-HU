---
title: "az Azure DNS használatával aaaManage DNS-rekordok hello Azure CLI 1.0 |} Microsoft Docs"
description: "DNS-rekordhalmazok és az Azure DNS-rekordok kezelése esetén az Azure DNS-tartomány. Minden CLI 1.0 parancsok rekordhalmazokat és rekordokat műveleteket."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/20/2016
ms.author: jonatul
ms.openlocfilehash: 1f01450b0839f712cb1d96be318766bac581fea1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-in-azure-dns-using-hello-azure-cli-10"></a>Az Azure DNS-hello Azure CLI 1.0 használata DNS-rekordok kezelése

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Ez a cikk bemutatja, hogyan toomanage DNS-rekordokat a DNS-zóna használatával hello platformfüggetlen Azure parancssori felület (CLI), amely megtalálható a Windows, Mac és Linux. A DNS-rekordok segítségével is kezelheti [Azure PowerShell](dns-operations-recordsets.md) vagy hello [Azure-portálon](dns-operations-recordsets-portal.md).

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

* [Az Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) -hello klasszikus és resource management üzembe helyezési modellel a parancssori felületen.
* [Az Azure CLI 2.0](dns-operations-recordsets-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell.

a cikkben szereplő példák hello során feltételezzük, hogy már [hello Azure CLI 1.0-s jelentkezik be, és a DNS-zóna létrehozásakor](dns-operations-dnszones-cli-nodejs.md).

## <a name="introduction"></a>Bevezetés

DNS-rekordok létrehozása az Azure DNS-, előtt először toounderstand miként Azure DNS rendezi a DNS-rekordhalmazok DNS-rekordokat.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Az Azure DNS DNS-rekordjaival kapcsolatos további információért tekintse meg a [DNS-zónákkal és -rekordokkal](dns-zones-records.md) foglalkozó cikket.

## <a name="create-a-dns-record"></a>DNS-rekord létrehozása

egy DNS-rekord toocreate hello használata `azure network dns record-set add-record` parancsot. További segítségért lásd: `azure network dns record-set add-record -h`.

Rekord létrehozásakor toospecify hello erőforráscsoport-nevet, zónanév van szüksége, a rekordhalmaz nevét, a hello rekord típusát és a létrehozandó hello rekord hello részleteit. hello megadott rekordhalmaz nevének kell lennie egy *relatív* nevét, azaz kell zárnia a hello zóna neve.

Ha hello rekordkészlet már nem létezik, ezzel a paranccsal létrejön az Ön. Ha hello rekordkészlet már létezik, a parancs adda hello toohello meglévő rekordhalmaz megadott rekord.

Új rekordhalmaz létrehozásakor az alapértelmezett élettartam (time-to-live, TTL) értéke 3600 lesz. Hogyan toouse különböző TTLs: kapcsolatos utasításokat [DNS rekordhalmaz létrehozása](#create-a-dns-record-set).

hello alábbi példa létrehoz egy A rekordot nevű *www* hello zónában *contoso.com* hello erőforráscsoportban *MyResourceGroup*. IP-címét egy rekord hello hello *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

toocreate rekord hello csúcs hello zóna (ebben az esetben "a contoso.com"), hello rekord nevet "@", hello idézőjelekkel együtt:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

## <a name="create-a-dns-record-set"></a>A DNS-rekordhalmaz létrehozása

Hello DNS-rekord fenti példák hello, vagy meglévő rekordhalmazt hozzáadott tooan volt, vagy hello rekordhalmaz létrehozásának *implicit módon*. Hello rekordhalmaz is létrehozhat *explicit módon* előtt tooit hozzáadása rögzíti. Az Azure DNS támogatja az "empty" rekordhalmazok, amely működhet, és egy helyőrző tooreserve egy DNS-név DNS-rekordok létrehozása előtt. Üres rekordhalmazok legyenek láthatók a hello Azure DNS vezérlősík vezérlőként, de nem szerepelnek a hello Azure DNS névkiszolgálóit.

Rekordhalmazok hello használatával hozhatók létre `azure network dns record-set create` parancsot. További segítségért lásd: `azure network dns record-set create -h`.

Be explicit módon hello rekord létrehozása lehetővé teszi a toospecify rekordhalmaz tulajdonságait, például hello [idő-Élettartam (TTL)](dns-zones-records.md#time-to-live) és metaadatokat. [A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) kulcs-érték párként minden rekordhalmaz használt tooassociate az alkalmazás-specifikus adatok is lehetnek.

hello alábbi példa létrehoz egy üres rekordhalmaz 60 másodperces TTL, hello segítségével `--ttl` paraméter (rövid alak `-l`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --ttl 60
```

hello alábbi példa létrehoz két metaadat-bejegyzést, rekordkészlet "osztály pénzügyi =" és "környezet éles =", hello segítségével `--metadata` paraméter (rövid alak `-m`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

Egy üres rekordhalmaz hozunk létre, rekordok segítségével adhatók `azure network dns record-set add-record` leírtak [hozzon létre egy DNS-rekord](#create-a-dns-record).

## <a name="create-records-of-other-types"></a>Más típusú rekordok létrehozása

Hogy látott részletesen hogyan toocreate "A" rögzíti, hello a következő példák szemléltetik, hogyan toocreate rekord egyéb típusú bejegyzés a támogatott Azure DNS által.

hello paraméterek használt toospecify hello rekord adatok hello típusú rekord lehet hello függenek. Az "A" típusú rekord, akkor adja meg például hello IPv4-cím hello paraméterrel `-a <IPv4 address>`. hello paraméterek, a különböző rekordtípusú is listázva lehet használatával `azure network dns record-set add-record -h`.

Minden esetben megmutatjuk, hogyan toocreate egyetlen rekordot. hello rekord rekordhalmaz meglévő hozzáadott toohello, vagy rekordhalmaz implicit módon létrehozva. További információ a rekordhalmazok létrehozásához, és definiáló rekordot paraméter explicit módon, olvassa el [DNS rekordhalmaz létrehozása](#create-a-dns-record-set).

Nem ad egy példa toocreate egy SOA típusú rekordhalmaz SOAs elkészítése és minden DNS-zóna törlődik, és nem hozható létre vagy törölt külön-külön. Azonban [SOA módosítható, egy újabb példában látható módon hello](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record"></a>Az AAAA-rekord létrehozása

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-aaaa AAAA --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a>Hozzon létre egy CNAME rekordot

> [!NOTE]
> hello DNS-szabványokból nem teszik lehetővé a CNAME rekordot a zóna hello csúcsán (`-Name "@"`), és nem teszik egynél több rekordot tartalmazó rekordhalmazok.
> 
> További információkért lásd: [CNAME rekordok](dns-zones-records.md#cname-records).

```azurecli
azure network dns record-set add-record  MyResourceGroup contoso.com  test-cname CNAME --cname www.contoso.com
```

### <a name="create-an-mx-record"></a>Az MX-rekord létrehozása

Ebben a példában hello rekordhalmaznevet használjuk "@" toocreate hello MX rekord: hello zóna felső pontja (ebben az esetben "a contoso.com").

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "@" MX --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a>NS-rekord létrehozása

```azurecli
azure network dns record-set add-record MyResourceGroup  contoso.com  test-ns NS --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a>A PTR típusú rekord létrehozása

Ebben az esetben "my-arpa-zónában (Zone.com)" jelöli hello ARPA zóna képviselő az IP-címtartományt. Minden egyes PTR típusú rekord a zóna tooan IP-címet az IP-címtartományon belül felel meg.  hello rekordjának neve "10" hello utolsó oktett hello IP-cím, ez a bejegyzés által képviselt IP-címtartományon belül.

```azurecli
azure network dns record-set add-record MyResourceGroup my-arpa-zone.com "10" PTR --ptrdname "myservice.contoso.com"
```

### <a name="create-an-srv-record"></a>Az SRV rekord létrehozása

Létrehozásakor egy [SRV-rekordhalmaz](dns-zones-records.md#srv-records), adja meg a hello  *\_szolgáltatás* és  *\_protokoll* hello a rekordhalmaz neveként. Nincs szükség tooinclude van "@" hello a rekordhalmaz neveként az SRV rekord létrehozása beállításakor: hello zóna felső pontja.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "_sip._tls" SRV --priority 10 --weight 5 --port 8080 --target "sip.contoso.com"
```

### <a name="create-a-txt-record"></a>TXT-rekord létrehozása

hello következő példa bemutatja, hogyan toocreate egy TXT rögzíti. További információ a támogatott TXT rekord hello karakterlánc maximális hossza: [TXT rekord](dns-zones-records.md#txt-records).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-txt TXT --text "This is a TXT record"
```

## <a name="get-a-record-set"></a>Rekordkészlet beolvasása

Használjon egy meglévő rekordhalmaz tooretrieve `azure network dns record-set show`. További segítségért lásd: `azure network dns record-set show -h`.

Egy rekord és a rekordhalmaz létrehozásakor hello rekord állíthat be, a megadott név lehet a *relatív* nevét, azaz kell zárnia a hello zóna neve. Szükség toospecify hello rekordtípust, hello rekordot tartalmazó hello zóna beállítása, hello hello zóna tartalmazó erőforráscsoport.

hello alábbi példa hello rekordot be *www* , adjon meg egy zónából *contoso.com* erőforráscsoportban *MyResourceGroup*:

```azurecli
azure network dns record-set show MyResourceGroup contoso.com www A
```

## <a name="list-record-sets"></a>Lista rekordhalmazok

Minden rekordot a DNS-zónák hello segítségével listázhatja `azure network dns record-set list` parancsot. További segítségért lásd: `azure network dns record-set list -h`.

Ebben a példában adja vissza az összes rekordhalmazok hello zónában *contoso.com*, erőforráscsoportban *MyResourceGroup*, név vagy rekordtípus függetlenül:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```

Ebben a példában, amelyek megfelelnek a megadott rekordtípus (ebben az esetben az "A" rekordok) hello minden rekordkészletet ad vissza:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com --type A
```

## <a name="add-a-record-tooan-existing-record-set"></a>A meglévő rekordhalmazt rekord tooan hozzáadása

Használhat `azure network dns record-set add-record` mindkét toocreate egy új rekordot a rekordhalmaz vagy tooadd rekord tooan meglévő rekordhalmaz.

További információkért lásd: [hozzon létre egy DNS-rekord](#create-a-dns-record) és [más típusú rekordok létrehozásának](#create-records-of-other-types) felett.

## <a name="remove-a-record-from-an-existing-record-set"></a>Távolítsa el a rekord egy meglévő rekordhalmaz.

meglévő rekordhalmaz használata tooremove egy DNS-bejegyzést `azure network dns record-set delete-record`. További segítségért lásd: `azure network dns record-set delete-record -h`.

Törli a DNS-rekordot a rekordhalmaz. Utolsó rekordját hello a rekordkészlet törlésekor hello rekordhalmaz maga van **nem** törölték. Ehelyett egy üres rekordhalmaz marad. toodelete hello rekordhalmaz helyett, lásd: [rekordhalmaz törlése](#delete-a-record-set).

Toospecify kell hello törölt rekord toobe és azt törölni kell, használatával hello zóna hello ugyanazokkal a paraméterekkel, egy rekord segítségével létrehozásakor `azure network dns record-set add-record`. Ezek a paraméterek ismertetett [DNS-rekord létrehozása](#create-a-dns-record) és [más típusú rekordok létrehozásának](#create-records-of-other-types) fent.

Ez a parancs felszólítja megerősítést kér. Ez a kérdés hello letilthatók `--quiet` váltás (rövid alak `-q`).

a következő példa törlések hello hello "1.2.3.4" hello rekordból beállítása a nevesített értékű egy olyan rekordot *www* hello zónában *contoso.com*, hello erőforráscsoportban *MyResourceGroup*. hello megerősítési kérés le van tiltva.

```azurecli
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4 --quiet
```

## <a name="modify-an-existing-record-set"></a>Módosíthatja egy meglévő rekordkészlete

Minden rekordhalmaz tartalmaz egy [idő-Élettartam (TTL)](dns-zones-records.md#time-to-live), [metaadatok](dns-zones-records.md#tags-and-metadata), és a DNS-rekordokat. hello alábbi szakaszok azt ismertetik, hogyan toomodify minden ezeket a tulajdonságokat.

### <a name="toomodify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a>az A, AAAA, MX, NS, PTR, SRV és TXT rekord toomodify

Adjon meg egy, AAAA, MX, NS, PTR, SRV és TXT meglévő bejegyzés toomodify, először adjon egy új rekordot, és a delete hello meglévő rekord. Részletes útmutatást toodelete és rekordok hozzáadásához, tekintse meg a cikk korábbi szakaszaiban hello.

hello a következő példa bemutatja, hogyan toomodify az "A" rekord, az IP-cím 1.2.3.4 tooIP cím 5.6.7.8:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 5.6.7.8
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

### <a name="toomodify-a-cname-record"></a>egy olyan CNAME rekordot toomodify

egy olyan CNAME rekordot toomodify használja `azure network dns record-set add-record` tooadd hello új rekord értéket. Egyéb típusú bejegyzés eltérően a CNAME rekordkészlete csak egyetlen rekordot tartalmazhat. Hello meglévő rekord ezért *helyett* amikor hello új rekord kerül, és nem kell külön-külön törölt toobe.  Akkor lesz felszólító tooaccept a csere.

hello példa módosítja hello CNAME rekordkészlete *www* hello zónában *contoso.com*, erőforráscsoportban *MyResourceGroup*, toopoint túl "www.fabrikam.net" helyett a meglévő érték:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www CNAME --cname www.fabrikam.net
``` 

### <a name="toomodify-an-soa-record"></a>egy SOA rekordot toomodify

Használjon `azure network dns record-set set-soa-record` toomodify hello SOA a megadott DNS-zónák. További segítségért lásd: `azure network dns record-set set-soa-record -h`.

hello következő példa bemutatja, hogyan hello SOA tooset hello "e-mail" tulajdonságának hello zóna rekordja *contoso.com* hello erőforráscsoportban *MyResourceGroup*:

```azurecli
azure network dns record-set set-soa-record rg1 contoso.com --email admin.contoso.com
```


### <a name="toomodify-ns-records-at-hello-zone-apex"></a>hello zóna felső pontja toomodify Névkiszolgálói rekordok

Állítsa be megfelelően hello zóna felső pontja hello NS-rekord automatikusan hozza létre minden DNS-zóna. Hello Azure DNS-név kiszolgálók hozzárendelt toohello zóna hello szerepel benne.

Hozzáadhat további neve kiszolgálók toothis NS rekordhalmaz, toosupport közös futtató tartományok egynél több DNS-szolgáltatónál. Hello TTL-t és a rekordhalmaz metaadatait is módosíthatja. Azonban nem lehet eltávolítani vagy módosítani hello előre megadott Azure DNS névkiszolgálóit.

Vegye figyelembe, hogy ez érvényes csak toohello NS rekord beállított hello zóna felső pontja. Más NS rekordhalmazok, amelyek a zónához (a használt toodelegate gyermekzónákhoz) korlátozás nélkül lehet módosítani.

hello a következő példa bemutatja, hogyan tooadd további neve server toohello NS-rekord állítják hello zóna felső pontja:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="toomodify-hello-ttl-of-an-existing-record-set"></a>egy meglévő bejegyzés TTL toomodify hello beállítása

toomodify hello TTL-t egy meglévő bejegyzés megadásához használja `azure network dns record-set set`. További segítségért lásd: `azure network dns record-set set -h`.

hello a következő példa bemutatja, hogyan toomodify rekordhalmaz TTL-t, ez az eset too60 másodperc:

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --ttl 60
```

### <a name="toomodify-hello-metadata-of-an-existing-record-set"></a>egy meglévő rekordkészlet toomodify hello metaadatai

[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) kulcs-érték párként minden rekordhalmaz használt tooassociate az alkalmazás-specifikus adatok is lehetnek. egy meglévő bejegyzés toomodify hello metaadatok megadásához használja `azure network dns record-set set`. További segítségért lásd: `azure network dns record-set set -h`.

hello következő példa bemutatja, hogyan toomodify nevű rekordhalmaz két metaadat-bejegyzést, "osztály pénzügyi =" és "környezet éles =", hello segítségével `--metadata` paraméter (rövid alak `-m`). Vegye figyelembe, hogy a meglévő metaadatokat *helyett* által hello értékekből.

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

## <a name="delete-a-record-set"></a>A rekordhalmaz törlése

Rekordhalmazok hello segítségével törölhetők `azure network dns record-set delete` parancsot. További segítségért lásd: `azure network dns record-set delete -h`. Rekordhalmaz törlése is törli az összes rekordján hello rekordhalmaz.

> [!NOTE]
> Nem lehet törölni hello SOA és NS rekordhalmazok hello zóna csúcsán (`-Name "@"`).  Ezek automatikusan létrejönnek hello zóna mikor jött létre, és hello zóna törlésekor automatikusan törlődik.

hello alábbi példa törli hello rekordhalmaz elnevezett *www* , adjon meg egy hello zónából *contoso.com* erőforráscsoportban *MyResourceGroup*:

```azurecli
azure network dns record-set delete MyResourceGroup contoso.com www A
```

Biztosan felszólító tooconfirm hello törlési műveletet. a parancssorba toosuppress hello használata `--quiet` váltás (rövid alak `-q`).

## <a name="next-steps"></a>Következő lépések

További információ [zónák és az Azure DNS-rekordok](dns-zones-records.md).
<br>
Ismerje meg, hogyan túl[védelme a zóna és a rekordok](dns-protect-zones-recordsets.md) Azure DNS használata esetén.
