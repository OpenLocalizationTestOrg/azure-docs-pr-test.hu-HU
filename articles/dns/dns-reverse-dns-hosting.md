---
title: "aaaHosting névkeresési DNS zónák az Azure DNS |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure DNS toohost hello névkeresési DNS-címkeresési zónák az IP-címtartományokhoz"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 24feb8ef1c75a7d91938867f348fed1190046e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a>Az Azure DNS-névkeresési DNS-címkeresési zónák üzemeltetéséhez

Ez a cikk azt ismerteti, hogyan toohost hello névkeresési DNS-keresési a hozzárendelt IP-címtartományokhoz az Azure DNS-zónák. hello névkeresési zóna által képviselt hello IP-címtartományok hozzárendelése tooyour a szervezeten belül, általában az Internetszolgáltató által.

tooconfigure névkeresési DNS az Azure tulajdonában lévő IP-cím tooyour Azure-szolgáltatások című [hello névkeresési konfigurálásához hello IP-címek lefoglalt tooyour Azure szolgáltatás](dns-reverse-dns-for-azure-services.md).

A cikk elolvasása előtt meg kell ismernie a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md).

Ez a cikk bemutatja, hogyan hello lépéseket toocreate az első névkeresési DNS-zóna és a rekord hello Azure portál Azure PowerShell, az Azure CLI 1.0 vagy az Azure CLI 2.0 használatával.

## <a name="create-a-reverse-lookup-dns-zone"></a>A névkeresési DNS-zóna létrehozása

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com)
1. Hello központ menüben kattintson, majd **új** > **hálózati** >, majd **DNS-zóna** tooopen hello **hozzon létre DNS-zóna**panelen.

   ![DNS-zóna](./media/dns-reverse-dns-hosting/figure1.png)

1. A hello **hozzon létre DNS-zóna** panelen, a DNS-zóna neve. hello zóna neve hello kialakított az IPv4 és IPv6-előtagok másképp van. Vagy hello utasításokat a [IPV4](#ipv4) vagy [IPv6](#ipv6) tooname a zónát. Ha végzett a kattintson **létrehozása** toocreate hello zóna.

### <a name="ipv4"></a>IPv4-alapú

egy IPv4-névkeresési zóna neve hello hello IP-címtartomány, amely alapul. Nem lehet a következő formátumban hello: `<IPv4 network prefix in reverse order>.in-addr.arpa`. Tekintse meg a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md#ipv4).

> [!NOTE]
> Az Azure DNS-classless névkeresési DNS-címkeresési zónák létrehozásakor használnia kell a kötőjel (`-`) helyett egy perjel ("/") hello zóna nevét.
>
> Például a hello IP-címtartomány 192.0.2.128/26, kell használnia `128-26.2.0.192.in-addr.arpa` hello zóna neve helyett `128/26.2.0.192.in-addr.arpa`.
>
> Ennek az az oka, mind támogatottak hello DNS szabványokkal, DNS-zónák tartalmazó hello perjel (`/`) karakter használata nem támogatott Azure DNS-ben.

hello következő példa bemutatja, hogyan toocreate egy osztály C fordított nevű DNS-zóna `2.0.192.in-addr.arpa` az Azure DNS hello Azure-portálon keresztül:

 ![DNS-zóna létrehozása](./media/dns-reverse-dns-hosting/figure2.png)

hello "Erőforráscsoport helye" hello erőforrásnak hello helyét határozza meg, és nincs hatással van a hello DNS-zónát. mindig "global" Hello DNS-zóna helyét, és nem jelenik meg.

hello következő példák azt szemléltetik, hogyan toocomplete ennek a feladatnak az Azure PowerShell és az Azure parancssori felület hello:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

egy IPv6-névkeresési zóna neve hello hello a következő formátumban kell lennie: `<IPv6 network prefix in reverse order>.ip6.arpa`.  Tekintse meg a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md#ipv6).


hello következő példa bemutatja, hogyan toocreate egy IPv6 névkeresési DNS-zóna nevű `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` az Azure DNS hello Azure-portálon keresztül:

 ![DNS-zóna létrehozása](./media/dns-reverse-dns-hosting/figure3.png)

hello "Erőforráscsoport helye" hello erőforrásnak hello helyét határozza meg, és nincs hatással van a hello DNS-zónát. mindig "global" Hello DNS-zóna helyét, és nem jelenik meg.

hello következő példák azt szemléltetik, hogyan toocomplete ennek a feladatnak az Azure PowerShell és az Azure parancssori felület hello:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a>AzureCLI 1.0

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a>A címfeloldási DNS-zóna delegálása

A címfeloldási DNS-zóna hozunk létre, gondoskodnia kell hello zóna hello szülőzónában van átadva. DNS-delegálás lehetővé teszi, hogy a hello DNS nevét feloldási folyamat toofind hello üzemeltető névkiszolgálókat a címfeloldási DNS-zóna. Ez lehetővé teszi, hogy ezen kiszolgálók tooanswer DNS fordított névlekérdezései hello IP-címek a címtartomány.

Címkeresési zónák, DNS-zóna delegálása hello folyamatán ismertetett [delegálása a tartományi tooAzure DNS](dns-delegate-domain-azure-dns.md). A delegálás névkeresési zónák hello működik ugyanúgy. hello egyetlen különbség, hogy kell-e tooconfigure hello névkiszolgálók a hello a tartományregisztráló neve helyett az IP-címtartományt biztosító Internetszolgáltató.

## <a name="create-a-dns-ptr-record"></a>DNS PTR-rekord létrehozása

### <a name="ipv4"></a>IPv4-alapú

hello alábbi példa bemutatja, hogyan PTR típusú rekord létrehozása az Azure DNS-névkeresési DNS-zóna hello folyamatán. Más rekordtípusokhoz és toomodify meglévő rekordokat: [kezelése DNS-rekordok és a rekordhalmazok hello Azure-portálon](dns-operations-recordsets-portal.md).

1.  Hello hello tetején **DNS-zóna** panelen válassza **+ rekordhalmaz** tooopen hello **adja hozzá a rekordhalmaz** panelen.

 ![DNS-zóna](./media/dns-reverse-dns-hosting/figure4.png)

1. A hello **adja hozzá a rekordhalmaz** panelen. 
1. Válassza ki **PTR** hello record "**típus**" menü.  
1. hello hello rekordhalmaz nevét PTR-rekordot kell toobe hello többi hello IPv4-cím fordított sorrendben. Ebben a példában hello első három oktettjének már fel vannak töltve hello Zónanév (.2.0.192) részeként. Ezért csak hello utolsó oktett hello neve mezőben van megadva. Például nevezze el az volt a rekordhalmaz "**15**" amelynek IP-cím 192.0.2.15 erőforrás.  
1. A hello "**tartománynév**" mezőbe írja be a hello teljesen minősített tartománynevét (FQDN) használatával hello IP hello erőforrás.
1. Válassza ki **OK** hello panel toocreate hello DNS-rekord hello alján.

 ![rekordhalmaz hozzáadása](./media/dns-reverse-dns-hosting/figure5.png)

hello következő példákban hogyan toocomplete PowerShell és hello AzureCLI ezt a feladatot:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a>AzureCLI 1.0

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a>IPv6

hello a következő példa bemutatja, hogyan hello új "PTR" rekord létrehozásának folyamatán. Más rekordtípusokhoz és toomodify meglévő rekordokat: [kezelése DNS-rekordok és a rekordhalmazok hello Azure-portálon](dns-operations-recordsets-portal.md).

1. Hello hello tetején **DNS-zóna panel**, jelölje be **+ rekordhalmaz** tooopen hello **adja hozzá a rekordhalmaz** panelen.

  ![DNS-zónák panel](./media/dns-reverse-dns-hosting/figure6.png)

2. A hello **adja hozzá a rekordhalmaz** panelen. 
3. Válassza ki **PTR** hello record "**típus**" menü.  
4. hello hello rekordhalmaz nevét PTR-rekordot kell toobe hello többi hello IPv6-cím fordított sorrendben. Nem minden nulla tömörítési tartalmaznia kell. Ebben a példában hello első 64 bites hello IPv6 már fel van töltve hello Zónanév (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa) részeként. Ezért csak hello utolsó 64 bites hello neve mezőben megadva. hello hello IP-cím utolsó 64 bites időszak használata hello elválasztó mindegyik hexadecimális szám között fordított sorrendben kerülnek. Például nevezze el az volt a rekordhalmaz "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" amelynek IP-cím 2001:0db8:abdc:0000:f524:10bc:1af9:405e erőforrás.  
5. A hello "**tartománynév**" mezőbe írja be a hello teljesen minősített tartománynevét (FQDN) használatával hello IP hello erőforrás.
6. Válassza ki **OK** hello panel toocreate hello DNS-rekord hello alján.

![Adja hozzá a rekordhalmaz panel](./media/dns-reverse-dns-hosting/figure7.png)

hello következő példákban hogyan toocomplete PowerShell és hello AzureCLI ezt a feladatot:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a>AzureCLI 1.0

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a>A rekordok megtekintése

tooview hello rekordok hozott létre, keresse meg a DNS-zónát tooyour hello Azure-portálon. A hello alacsonyabb hello részét **DNS-zóna** panelen láthatja hello rekordok hello DNS-zónához. Hello alapértelmezett NS és SOA típusú rekordok, minden zóna jöttek létre, és a létrehozott új rekordok kell megjelennie.

### <a name="ipv4"></a>IPv4-alapú

DNS zóna panelen IPv4 PTR-rekordok:

![DNS-zónák panel](./media/dns-reverse-dns-hosting/figure8.png)

hello alábbi példák megjelenítése hogyan tooview hello PTR rögzíti a PowerShell használatával vagy az Azure parancssori felület hello:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

DNS zóna panelen IPv6 PTR-rekordok:

![DNS-zónák panel](./media/dns-reverse-dns-hosting/figure9.png)

Az alábbiakban hello hogyan tooview hello rögzíti a PowerShell és hello AzureCLI példák:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a>GYIK

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Képes-e üzemeltetni névkeresési DNS-címkeresési zónák az Internetszolgáltató által hozzárendelt IP-címblokkok az Azure DNS szolgáltatásra?

Igen. Hello (ARPA) névkeresési zónák az Azure DNS-ben a saját IP-címtartományok teljes mértékben támogatott.

Hozzon létre hello névkeresési zónát az Azure DNS-amint ez a cikk azt, majd az Internetszolgáltató túl együttműködve[delegált hello zóna](dns-domain-delegation.md).  Hello PTR-rekordok kezelhetők az egyes névkeresési a hello módon egyéb bejegyzéstípusokat.

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a>A fordított DNS keresési zónához költségének üzemeltető mennyi használ?

Hello címfeloldási DNS-zóna üzemeltető, az az Internetszolgáltató által hozzárendelt IP-Címblokk az Azure DNS-rendszer díjakon [Azure DNS normál díjszabás](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>IPv4 és IPv6 címek az Azure DNS-névkeresési DNS-címkeresési zónák is futtatni?

Igen. Ez a cikk azt ismerteti, hogyan toocreate IPv4 és IPv6 névkeresési DNS zónák Azure DNS-ben.

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a>Beimportálhatok egy meglévő címfeloldási DNS-zóna?

Igen. Hello Azure CLI tooimport meglévő DNS-zóna Azure DNS-ben is használhatja. Ez a módszer a címkeresési zónák és a névkeresési zóna listáját.

További információkért lásd: [importálásának és exportálásának használatával DNS zóna fájlt hello Azure CLI](dns-import-export.md).

## <a name="next-steps"></a>Következő lépések

A címfeloldási DNS-további információkért lásd: [Wikipedia névkeresési DNS](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Ismerje meg, hogyan túl[címfeloldási DNS-rekordjait az Azure-szolgáltatások kezelése](dns-reverse-dns-for-azure-services.md).
