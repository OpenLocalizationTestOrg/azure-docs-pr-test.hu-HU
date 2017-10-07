---
title: "az Azure DNS az Azure PowerShell rögzíti aaaManage DNS |} Microsoft Docs"
description: "DNS-rekordhalmazok és az Azure DNS-rekordok kezelése esetén az Azure DNS-tartomány. Az összes PowerShell-parancsokat rekordhalmazokat és rekordokat műveleteket."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 7136a373-0682-471c-9c28-9e00d2add9c2
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: bfdf116e174d06db0514abdc0ec3f4fc4ee0a079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a>DNS-rekordok és az Azure PowerShell használata Azure DNS rekordhalmazok kezelése

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Ez a cikk bemutatja, hogyan toomanage DNS rögzíti a DNS-zónát az Azure PowerShell használatával. DNS-rekordokat is hello platformfüggetlen használatával felügyelendő [Azure CLI](dns-operations-recordsets-cli.md) vagy hello [Azure-portálon](dns-operations-recordsets-portal.md).

a cikkben szereplő példák hello során feltételezzük, hogy már [Azure PowerShell jelentkezik be, és a DNS-zóna létrehozásakor](dns-operations-dnszones.md).

## <a name="introduction"></a>Bevezetés

DNS-rekordok létrehozása az Azure DNS-, előtt először toounderstand miként Azure DNS rendezi a DNS-rekordhalmazok DNS-rekordokat.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Az Azure DNS DNS-rekordjaival kapcsolatos további információért tekintse meg a [DNS-zónákkal és -rekordokkal](dns-zones-records.md) foglalkozó cikket.


## <a name="create-a-new-dns-record"></a>Hozzon létre egy új DNS-rekord

Ha az új rekord hello azonos nevet, és írja be egy meglévő rekordként, kell túl[hozzá toohello meglévő rekordhalmaz](#add-a-record-to-an-existing-record-set). Ha az új rekord tartalmazza-e a meglévő rekordok különböző nevű és típusú tooall, új rekordhalmaz toocreate kell. 

### <a name="create-a-records-in-a-new-record-set"></a>"A" rekordokat létrehozni egy új rekordkészlete

Rekordhalmazok hello segítségével hozhatja létre `New-AzureRmDnsRecordSet` parancsmag. A rekordhalmaz létrehozásakor kell toospecify hello rekordhalmazának neve, hello zóna, hello létrehozásakor toolive (TTL), hello rekordtípus és hello rekordok toobe.

hello paramétereket a rekordok tooa rekordhalmaz hozzáadásához hello hello rekordhalmaz típusától függenek. Például egy "A" típusú rekordkészlet használatakor meg kell toospecify hello IP-cím hello paraméter használatával `-IPv4Address`. Más paramétereket az egyéb típusú bejegyzés használnak. Lásd: [további rekordtípusokra](#additional-record-type-examples) részleteiről.

hello alábbi példa létrehoz egy hello relatív nevű DNS-zóna "contoso.com" hello "www" rekordhalmaz. hello rekordhalmaz hello teljesen minősített neve "www.contoso.com". hello típus az "A", és hello élettartam 3600 másodperc. hello rekordhalmaz egyetlen rekordot, IP-cím "1.2.3.4" tartalmaz.

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

toocreate hello "csúcs" zóna beállított egy olyan rekordot (ebben az esetben "contoso.com"), használjon hello rekordhalmazának neve "@" (idézőjelek nélkül):

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Ha rekordhalmaz egynél több rekordot tartalmazó toocreate van szüksége, először hozzon létre egy helyi tömb hello rekordok hozzáadásához, majd adjon át hello tömb túl`New-AzureRmDnsRecordSet` az alábbiak szerint:

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) kulcs-érték párként minden rekordhalmaz használt tooassociate az alkalmazás-specifikus adatok is lehetnek. hello következő példa bemutatja, hogyan toocreate nevű rekordhalmaz két metaadat-bejegyzést, "osztály pénzügyi =" és "környezet éles =".

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

Az Azure DNS "empty" rekordhalmazok, amely működhet, és egy helyőrző tooreserve egy DNS-név DNS-rekordok létrehozása előtt is támogatja. Üres rekordhalmazok hello Azure DNS vezérlő vezérlősík látható, de jelennek meg hello Azure DNS névkiszolgálóit. a következő példa hello létrehoz egy üres rekordhalmaz:

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a>Más típusú rekordok létrehozása

Ha látható részletesen hogyan toocreate "A" rögzíti, a következő példák szemléltetik, hogyan toocreate rekordok egyéb bejegyzéstípusokat Azure DNS által támogatott hello.

Minden esetben megmutatjuk, hogyan állíthatja toocreate rekord egyetlen rekordot tartalmazó. hello korábbi példák "A" rekordok lehet igazítani toocreate metaadatokkal, a több rekordot tartalmazó más típusú rekordhalmazok vagy üres rekordhalmazok toocreate.

Nem ad egy példa toocreate egy SOA típusú rekordhalmaz SOAs elkészítése és minden DNS-zóna törlődik, és nem hozható létre vagy törölt külön-külön. Azonban [SOA módosítható, egy újabb példában látható módon hello](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Egyetlen rekordot tartalmazó AAAA típusú rekordhalmaz létrehozása

```powershell
New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a>Egyetlen rekordot tartalmazó CNAME típusú rekordhalmaz létrehozása

> [!NOTE]
> hello DNS-szabványokból nem teszik lehetővé a CNAME rekordot a zóna hello csúcsán (`-Name '@'`), és nem teszik egynél több rekordot tartalmazó rekordhalmazok.
> 
> További információkért lásd: [CNAME rekordok](dns-zones-records.md#cname-records).


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a>Egyetlen rekordot tartalmazó MX típusú rekordhalmaz létrehozása

Ebben a példában hello rekordhalmaznevet használjuk "@" toocreate egy MX rekord: hello zóna felső pontja (ebben az esetben "contoso.com").


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a>Egyetlen rekordot tartalmazó NS típusú rekordhalmaz létrehozása

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a>Egyetlen rekordot tartalmazó PTR típusú rekordhalmaz létrehozása

Ebben az esetben "my-arpa-zónában (Zone.com)" jelöli hello ARPA névkeresési zóna képviselő az IP-címtartományt. Minden egyes PTR típusú rekord a zóna tooan IP-címet az IP-címtartományon belül felel meg. hello rekordjának neve "10" hello utolsó oktett hello IP-cím, ez a bejegyzés által képviselt IP-címtartományon belül.

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a>Egyetlen rekordot tartalmazó SRV típusú rekordhalmaz létrehozása

Létrehozásakor egy [SRV-rekordhalmaz](dns-zones-records.md#srv-records), adja meg a hello  *\_szolgáltatás* és  *\_protokoll* hello a rekordhalmaz neveként. Nincs szükség tooinclude van "@" hello a rekordhalmaz neveként az SRV rekord létrehozása beállításakor: hello zóna felső pontja.

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a>Egyetlen rekordot tartalmazó TXT rekordhalmaz létrehozása

hello következő példa bemutatja, hogyan toocreate egy TXT rögzíti. További információ a támogatott TXT rekord hello karakterlánc maximális hossza: [TXT rekord](dns-zones-records.md#txt-records).

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a>Rekordkészlet beolvasása

Használjon egy meglévő rekordhalmaz tooretrieve `Get-AzureRmDnsRecordSet`. Ez a parancsmag adja vissza egy helyi hello az Azure DNS-rekordot képviselő objektum.

A `New-AzureRmDnsRecordSet`, megadott hello rekordhalmaz nevének kell lennie egy *relatív* nevét, azaz kell zárnia a hello zóna neve. Szükség toospecify hello rekordtípust, és hello rekordhalmaz tartalmazó hello zóna.

hello következő példa bemutatja, hogyan tooretrieve nevű rekordhalmaz. Ebben a példában hello zóna adott hello `-ZoneName` és `-ResourceGroupName` paraméterek.

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Azt is megteheti, azt is megadhatja a zóna objektummal, átadott hello segítségével hello zóna `-Zone` paraméter.

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a>Lista rekordhalmazok

Is `Get-AzureRmDnsZone` toolist rekordhalmazok zónában, hello kihagyásával őriznek `-Name` és/vagy `-RecordType` paraméterek.

hello alábbi példa adja vissza az összes rekordhalmazok hello zónában:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

hello következő példa bemutatja, hogyan összes rekordkészletek egy adott típusú neve hello rekord kihagyásával beállítása közben hello rekordtípus megadásával kérhető:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

a megadott nevű rekordkészletek összes tooretrieve rekordtípusokhoz, keresztül kell tooretrieve rekordhalmazok majd szűrő hello eredmények:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

A fenti példa minden hello hello zóna is megadott felhasználónévvel hello `-ZoneName` és `-ResourceGroupName`paraméterek (ahogy), vagy adjon meg egy zóna objektum:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-tooan-existing-record-set"></a>A meglévő rekordhalmazt rekord tooan hozzáadása

egy rekord tooan meglévő rekord tooadd beállításához hajtsa végre a következő három lépéseket hello:

1. Hello meglévő rekordkészlet beolvasása

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Adja hozzá a hello új rekord toohello helyi rekordhalmaz. Ez az offline művelet.

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Véglegesítse hello módosítás vissza toohello Azure DNS-szolgáltatás. 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

Használatával `Set-AzureRmDnsRecordSet` *lecseréli* hello meglévő Azure DNS-ben (és a benne található összes rekord) tartalmazó rekordhalmaz hello rekordhalmaz megadott. [ETag ellenőrzések](dns-zones-records.md#etags) használt tooensure egyidejű változtatások nem íródnak felül. Használhatja a választható hello `-Overwrite` kapcsoló toosuppress ezen ellenőrzések.

Ez a feladatütemezési műveletek is lehet *adatcsatornán*, azaz hello rekordhalmaz objektum használatával hello cső, ahelyett hogy paraméterként átadja azt át:

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

a fenti hello példák azt szemléltetik, hogyan állítsa be az "A" rekord tooan meglévő rekord tooadd "A" típusú. A hasonló feladatütemezési műveletek toorecord rekordhalmazok használt tooadd más típusú, és hello `-Ipv4Address` paramétere `Add-AzureRmDnsRecordConfig` más paramétereket adott tooeach rekordtípussal. hello különböző rekordtípusú paramétereinek vannak hello ugyanaz, mint hello `New-AzureRmDnsRecordConfig` parancsmag, ahogy az [további rekordtípusokra](#additional-record-type-examples) felett.

"CNAME" vagy "SOA" típusú rekordhalmazok nem tartalmazhat egynél több rekordot. Ennél a határértéknél hello DNS-szabványokból ered. Az Azure DNS korlátozása nincs.

## <a name="remove-a-record-from-an-existing-record-set"></a>Távolítsa el a rekord egy meglévő rekordkészlete

hello folyamat tooremove rekordhalmaz rekord egy rekord tooan létező, hasonló toohello folyamat tooadd rekordhalmaz:

1. Hello meglévő rekordkészlet beolvasása

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Távolítsa el a hello bejegyzést hello helyi rekordhalmaz objektumból. Ez az offline művelet. hello rekordot eltávolított összes paraméterek között kell lennie terméknévnek pontosan egyeznie kell a meglévő bejegyzés.

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Véglegesítse hello módosítás vissza toohello Azure DNS-szolgáltatás. Választható használata hello `-Overwrite` toosuppress kapcsoló [Etag ellenőrzi](dns-zones-records.md#etags) egyidejű változásait.

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

Feladatütemezési tooremove hello utolsó rekordját rekordhalmaz fent hello használata nem hello rekordhalmaz törlése, inkább egy üres rekordhalmaz elhagyják. a rekordhalmaz teljesen, tooremove lásd: [rekordhalmaz törlése](#delete-a-record-set).

Hasonlóképpen tooadding rekordok tooa rekordhalmaz, műveletek tooremove is átirányítható a rekordhalmaz hello sorozatát:

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

Különböző típusok támogatottak úgy, hogy túl hello megfelelő típusra vonatkozó paramétereket`Remove-AzureRmDnsRecordSet`. hello különböző rekordtípusú paramétereinek vannak hello ugyanaz, mint hello `New-AzureRmDnsRecordConfig` parancsmag, ahogy az [további rekordtípusokra](#additional-record-type-examples) felett.


## <a name="modify-an-existing-record-set"></a>Módosíthatja egy meglévő rekordkészlete

hello a módosítását egy meglévő rekordhalmaz lépésekre hasonló toohello lépései hozzáadása vagy eltávolítása a bejegyzések rekordhalmaz:

1. Hello meglévő rekordhalmazt használatával beolvasása `Get-AzureRmDnsRecordSet`.
2. Hello helyi rekordhalmaz objektum által módosítható:
    * Hozzáadása vagy eltávolítása a bejegyzések
    * A meglévő rekordok hello paraméterek módosítása
    * Metaadatok és az idő toolive (TTL) hello rekord módosítása beállítása
3. A változtatások véglegesítése a határidő hello segítségével `Set-AzureRmDnsRecordSet` parancsmag. Ez *lecseréli* hello meglévő az Azure DNS-tartalmazó rekordhalmaz hello rekordhalmaz megadott.

Használata esetén `Set-AzureRmDnsRecordSet`, [Etag ellenőrzi](dns-zones-records.md#etags) használt tooensure egyidejű változtatások nem íródnak felül. Használhatja a választható hello `-Overwrite` kapcsoló toosuppress ezen ellenőrzések.

### <a name="tooupdate-a-record-in-an-existing-record-set"></a>meglévő rekord rekord tooupdate beállítása

Ebben a példában a Microsoft hello IP-cím egy létező "A" rekord módosítása:

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-an-soa-record"></a>egy SOA rekordot toomodify

Nem adja hozzá vagy rekordok eltávolítása automatikusan létrehozott hello zóna felső pontja, SOA rekordot hello (`-Name "@"`, beleértve az ajánlat jelek). Azonban módosíthatja hello SOA típusú rekordját (kivéve az "állomás") belül hello paraméterek egyikét, és hello rekordhalmaz TTL-t.

a következő példa azt mutatja meg hogyan hello toochange hello *E-mail* hello SOA típusú rekordjának tulajdonsága:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-ns-records-at-hello-zone-apex"></a>hello zóna felső pontja toomodify Névkiszolgálói rekordok

Állítsa be megfelelően hello zóna felső pontja hello NS-rekord automatikusan hozza létre minden DNS-zóna. Hello Azure DNS-név kiszolgálók hozzárendelt toohello zóna hello szerepel benne.

Hozzáadhat további neve kiszolgálók toothis NS rekordhalmaz, toosupport közös futtató tartományok egynél több DNS-szolgáltatónál. Hello TTL-t és a rekordhalmaz metaadatait is módosíthatja. Azonban nem lehet eltávolítani vagy módosítani hello előre megadott Azure DNS névkiszolgálóit.

Vegye figyelembe, hogy ez érvényes csak toohello NS rekord beállított hello zóna felső pontja. Más NS rekordhalmazok, amelyek a zónához (a használt toodelegate gyermekzónákhoz) korlátozás nélkül lehet módosítani.

hello a következő példa bemutatja, hogyan tooadd további neve server toohello NS-rekord állítják hello zóna felső pontja:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-record-set-metadata"></a>a rekordhalmaz toomodify metaadatok

[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) kulcs-érték párként minden rekordhalmaz használt tooassociate az alkalmazás-specifikus adatok is lehetnek.

hello következő példa bemutatja, hogyan toomodify hello metaadatok egy meglévő bejegyzés be:

```powershell
# Get hello record set
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzureRmDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a>A rekordhalmaz törlése

Rekordhalmazok hello segítségével törölhetők `Remove-AzureRmDnsRecordSet` parancsmag. Rekordhalmaz törlése is törli az összes rekordján hello rekordhalmaz.

> [!NOTE]
> Nem lehet törölni hello SOA és NS rekordhalmazok hello zóna csúcsán (`-Name '@'`).  Az Azure DNS automatikusan létrehozza ezeket a beállítást, ha hello zóna lett létrehozva, és automatikusan törli azokat az hello zóna törlődik.

hello következő példa bemutatja, hogyan toodelete nevű rekordhalmaz. Ebben a példában hello rekordhalmazának neve, a rekordhalmaz típusa, a zóna neve és a erőforráscsoport minden egyes megadott explicit módon.

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Azt is megteheti hello rekordhalmaz nevét és típusát is megadható, és hello zónát a megadott objektum használatával:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

Egy harmadik lehetőségként hello rekordhalmaz maga adható meg a rekordhalmaz-objektummal:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

Hello rekordhalmaz egy rekordhalmaz objektum törölt toobe megadásakor [Etag ellenőrzi](dns-zones-records.md#etags) használt tooensure egyidejű változtatások nem törlődnek. Használhatja a választható hello `-Overwrite` kapcsoló toosuppress ezen ellenőrzések.

hello rekordhalmaz objektum is átirányítható paraméterként átadott helyett:

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a>Megerősítés

Hello `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, és `Remove-AzureRmDnsRecordSet` parancsmagok minden megerősítés támogatja.

Minden parancsmagot jóváhagyást kérni fogja, ha hello `$ConfirmPreference` PowerShell preferenciaváltozót értéke `Medium` vagy alacsonyabb. Hello alapértelmezett értéke óta `$ConfirmPreference` van `High`, ezek az üzenetek nem kapnak, amikor hello az alapértelmezett beállításokkal PowerShell.

Felülbírálhatja hello aktuális `$ConfirmPreference` hello használata beállítást `-Confirm` paraméter. Ha megad `-Confirm` vagy `-Confirm:$True` , hello parancsmag megerősítést kér, mielőtt futtatja. Ha megad `-Confirm:$False` , hello parancsmag nem figyelmeztet megerősítést kér. 

További információ `-Confirm` és `$ConfirmPreference`, lásd: [kapcsolatos Preferenciaváltozók](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Következő lépések

További információ [zónák és az Azure DNS-rekordok](dns-zones-records.md).
<br>
Ismerje meg, hogyan túl[védelme a zóna és a rekordok](dns-protect-zones-recordsets.md) Azure DNS használata esetén.
<br>
Felülvizsgálati hello [Azure DNS PowerShell referenciadokumentációt](/powershell/module/azurerm.dns).
