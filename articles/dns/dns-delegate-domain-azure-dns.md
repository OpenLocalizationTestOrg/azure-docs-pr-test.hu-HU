---
title: "aaaDelegate a tartomány tooAzure DNS |} Microsoft Docs"
description: "Ismerje meg, hogyan toochange tartományok delegálását és használata Azure DNS neve kiszolgálók tooprovide tartomány üzemeltetéséhez."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.openlocfilehash: f780bdaa416150e5e3afe6c6845dc75ba54b6203
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="delegate-a-domain-tooazure-dns"></a>A tartomány tooAzure DNS delegálása

Az Azure DNS lehetővé teszi a DNS-zóna toohost, és egy tartományhoz az Azure-ban hello DNS-rekordok kezelése. Ahhoz, hogy a tartomány tooreach Azure DNS-ben a DNS-lekérdezések, hello tartomány rendelkezik toobe hello szülőtartomány tooAzure DNS átadva. Ne feledje Azure DNS nincs hello tartományregisztráló. Ez a cikk azt ismerteti, hogyan toodelegate a tartomány tooAzure DNS.

A tartományokhoz a regisztráló vásárolt a regisztráló kínál hello beállítás tooset be ezeket a Névkiszolgálói rekordokat. Nincs tooown egy tartományi toocreate ezt a nevet a DNS-zóna Azure DNS-ben. Azonban hello regisztráló tooown hello tartomány tooset hello delegálás tooAzure DNS mentése szükséges.

Tegyük fel például, a "contoso.net" hello tartomány vásárlása, és hozzon létre egy hello neve "contoso.net" zónát az Azure DNS. Hello tartomány hello tulajdonosaként a regisztráló kínál, a tartomány hello beállítás tooconfigure hello névkiszolgáló-címet (Ez azt jelenti, hogy hello Névkiszolgálói rekordokat). hello regisztráló ezeket a Névkiszolgálói rekordokat hello szülőtartományban, ebben az esetben ".net" tárolja. Hello world különböző pontjain található ügyfelek majd lehet irányított tooyour tartományhoz az Azure DNS-zóna tooresolve DNS-rekordokat a "contoso.net" tett kísérlet során.

## <a name="create-a-dns-zone"></a>DNS-zóna létrehozása

1. Jelentkezzen be toohello Azure-portálon
1. Hello központ menüben kattintson, majd **új > Hálózat >** , majd **DNS-zóna** tooopen hello hozzon létre DNS-zóna panelen.

    ![DNS-zóna](./media/dns-domain-delegation/dns.png)

1. A hello **hozzon létre DNS-zóna** panelen adja meg a következő értékek hello, majd kattintson a **létrehozása**:

   | **Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Name (Név)**|contoso.net|hello hello DNS-zóna neve|
   |**Előfizetés**|[Az Ön előfizetése]|Válasszon egy előfizetés toocreate hello Alkalmazásátjáró a.|
   |**Erőforráscsoport**|**Új létrehozása:** contosoRG|Hozzon létre egy erőforráscsoportot. az erőforráscsoport neve hello kiválasztott hello előfizetésen belül egyedinek kell lennie. További tudnivalók az erőforráscsoportokról, olvassa el a hello toolearn [erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) a cikk áttekintése.|
   |**Hely**|USA nyugati régiója||

> [!NOTE]
> hello erőforráscsoport hello erőforráscsoport toohello helyre hivatkozik, és nincs hatással van a hello DNS-zónát. mindig "globális" Hello DNS-zóna helyét, és nem jelenik meg.

## <a name="retrieve-name-servers"></a>Névkiszolgálók lekérdezése

A DNS-zóna tooAzure DNS delegálhatja, mielőtt először tooknow hello névkiszolgálói neveket a zóna. Minden zóna létrehozásakor az Azure DNS egy névkiszolgálói készletből választ ki egyet.

1. A hello DNS-zóna hello Azure-portálon létrehozott **Kedvencek** ablaktáblán kattintson a **összes erőforrás**. Kattintson a hello **contoso.net** hello DNS-zónát **összes erőforrás** panelen. Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja **contoso.net** a hello szűrő név szerint... mezőbe tooeasily hozzáférés hello Alkalmazásátjáró. 

1. Hello névkiszolgálók lekérése hello DNS-zóna panelen. Ebben a példában a "contoso.net" zónához hello rendelték névkiszolgálókat "ns1-01.azure-dns.com", "ns2-01.azure-DNS.NET", "ns3-01.azure-dns.org", és "ns4-01.azure-dns.info":

 ![DNS-névkiszolgáló](./media/dns-domain-delegation/viewzonens500.png)

Az Azure DNS automatikusan létrehozza a mérvadó Névkiszolgálói rekordokat, amelyek a zónához hozzárendelt névkiszolgálók hello tartalmazó.  toosee hello névkiszolgáló nevek Azure PowerShell vagy Azure parancssori felületen keresztül, egyszerűen szükséges tooretrieve ezeket a rekordokat.

hello következő példákban is lépéseit írják le hello tooretrieve hello névkiszolgálók az PowerShell és az Azure CLI Azure DNS-zóna.

### <a name="powershell"></a>PowerShell

```powershell
# hello record name "@" is used toorefer toorecords at hello top of hello zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

a következő példa hello hello válasz.

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

a következő példa hello hello válasz.

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-hello-domain"></a>Delegált hello tartomány

Most, hogy hello DNS-zóna jön létre, és el kell hello névkiszolgálókat, hello szülőtartomány kell toobe hello Azure DNS névkiszolgálóit frissült. Minden tartományregisztráló a saját DNS felügyeleti eszközök toochange hello névkiszolgálójának rekordjai egy tartományhoz tartozik. Hello regisztráló DNS kezelése lapon hello NS rekord szerkesztését, és cserélje le hello Névkiszolgálói rekordokat Azure DNS-ben létrehozott hello néhányat a meglévők közül.

Amikor delegálása a tartományi tooAzure DNS, az Azure DNS által nyújtott hello névkiszolgálói neveket kell használnia. Ajánlott minden toouse négy kiszolgálók nevei, függetlenül a tartomány nevét hello neve. Tartományok delegálását nem kell hello neve server name toouse hello a tartomány legfelső szintű tartományában.

Mivel a következő IP-címek megváltozhatnak a jövőben ne használjon "összetartó rekordokat" toopoint toohello Azure DNS neve kiszolgáló IP-címek. A saját zónájában történő, névkiszolgálói neveket használó delegálásokat – más néven „személyes névkiszolgálókat” – az Azure DNS jelenleg nem támogatja.

## <a name="verify-name-resolution-is-working"></a>A névfeloldás működésének ellenőrzése

Hello delegálás befejezése után ellenőrizheti, hogy névfeloldás működik-e olyan eszköz, például az "nslookup" tooquery hello SOA rekordot a zóna (amely szintén automatikusan létrejön hello zóna létrehozásakor) használatával.

Nem rendelkezik toospecify hello Azure DNS névkiszolgálóit, ha hello delegálás beállítása helyes, hello normál DNS-feloldási folyamat megkeresi hello névkiszolgálók automatikusan.

```
nslookup -type=SOA contoso.com
```

hello az alábbiakban látható egy példa egy válasz az előző parancs hello:

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.com
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="delegate-sub-domains-in-azure-dns"></a>Altartományok delegálása az Azure DNS-ben

Ha azt szeretné, hogy fel egy különálló gyermekzónát tooset, delegálhat egy altartomány Azure DNS-ben. Például, hogy beállítása és delegált "contoso.net" Azure DNS-beli tegyük fel, hogy tooset fel egy különálló gyermekzónát szeretne "partners.contoso.net".

1. Hozzon létre hello gyermek zóna "partners.contoso.net" Azure DNS-ben.
2. Kereshet a mérvadó Névkiszolgálói rekordokat hello hello gyermek zóna tooobtain hello üzemeltető névkiszolgálókat hello gyermekzónát az Azure DNS-ben.
3. Delegálja a gyermekzónát hello hello gyermekzónára toohello gyermekzónát a Névkiszolgálói rekordok konfigurálásával.

### <a name="create-a-dns-zone"></a>DNS-zóna létrehozása

1. Jelentkezzen be toohello Azure-portálon
1. Hello központ menüben kattintson, majd **új > Hálózat >** , majd **DNS-zóna** tooopen hello hozzon létre DNS-zóna panelen.

    ![DNS-zóna](./media/dns-domain-delegation/dns.png)

1. A hello **hozzon létre DNS-zóna** panelen adja meg a következő értékek hello, majd kattintson a **létrehozása**:

   | **Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Name (Név)**|partners.contoso.net|hello hello DNS-zóna neve|
   |**Előfizetés**|[Az Ön előfizetése]|Válasszon egy előfizetés toocreate hello Alkalmazásátjáró a.|
   |**Erőforráscsoport**|**Meglévő használata:** contosoRG|Hozzon létre egy erőforráscsoportot. az erőforráscsoport neve hello kiválasztott hello előfizetésen belül egyedinek kell lennie. További tudnivalók az erőforráscsoportokról, olvassa el a hello toolearn [erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) a cikk áttekintése.|
   |**Hely**|USA nyugati régiója||

> [!NOTE]
> hello erőforráscsoport hello erőforráscsoport toohello helyre hivatkozik, és nincs hatással van a hello DNS-zónát. mindig "globális" Hello DNS-zóna helyét, és nem jelenik meg.

### <a name="retrieve-name-servers"></a>Névkiszolgálók lekérdezése

1. A hello DNS-zóna hello Azure-portálon létrehozott **Kedvencek** ablaktáblán kattintson a **összes erőforrás**. Kattintson a hello **partners.contoso.net** hello DNS-zónát **összes erőforrás** panelen. Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja **partners.contoso.net** a hello szűrő név szerint... mezőbe tooeasily hozzáférés hello DNS-zónát.

1. Hello névkiszolgálók lekérése hello DNS-zóna panelen. Ebben a példában a "contoso.net" zónához hello rendelték névkiszolgálókat "ns1-01.azure-dns.com", "ns2-01.azure-DNS.NET", "ns3-01.azure-dns.org", és "ns4-01.azure-dns.info":

 ![DNS-névkiszolgáló](./media/dns-domain-delegation/viewzonens500.png)

Az Azure DNS automatikusan létrehozza a mérvadó Névkiszolgálói rekordokat, amelyek a zónához hozzárendelt névkiszolgálók hello tartalmazó.  toosee hello névkiszolgáló nevek Azure PowerShell vagy Azure parancssori felületen keresztül, egyszerűen szükséges tooretrieve ezeket a rekordokat.

### <a name="create-name-server-record-in-parent-zone"></a>Névkiszolgáló-rekord létrehozása a szülőzónában

1. Keresse meg a toohello **contoso.net** hello Azure-portálon DNS-zónát.
1. Kattintson a **+ Rekordhalmaz** gombra.
1. A hello **adja hozzá a rekordhalmaz** panelen adja meg a következő értékek hello, majd kattintson a **OK**:

   | **Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Name (Név)**|partners|hello hello gyermek DNS-zóna neve|
   |**Típus**|NS|A névkiszolgálók esetében használja az NS típust.|
   |**TTL**|1|Az idő toolive.|
   |**TTL mértékegysége**|Óra|Beállítja az időt toolive egység toohours|
   |**NÉVKISZOLGÁLÓ**|{névkiszolgálók a partners.contoso.net zónából}|Adja meg minden 4 hello névkiszolgálók partners.contoso.net zónából. |

   ![DNS-névkiszolgáló](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a>Altartományok delegálása az Azure DNS-ben más eszközökkel

hello alábbi példák megadják hello lépéseket toodelegate altartományok a PowerShell és a CLI Azure DNS-ben:

#### <a name="powershell"></a>PowerShell

hello a következő PowerShell-példa bemutatja ennek működését. Ugyanezek a lépések hello Azure-portálon keresztül hajtható végre, vagy keresztül hello platformfüggetlen Azure CLI hello.

```powershell
# Create hello parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve hello authoritative NS records from hello child zone as shown in hello next example. This contains hello name servers assigned toohello child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create hello corresponding NS record set in hello parent zone toocomplete hello delegation. hello record set name in hello parent zone matches hello child zone name, in this case "partners".
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

Használjon `nslookup` tooverify, hogy minden helyesen van beállítva hello hello gyermekzóna SOA típusú rekordjának megkeresésével.

```
nslookup -type=SOA partners.contoso.com
```

```
Server: ns1-08.azure-dns.com
Address: 208.76.47.8

partners.contoso.com
    primary name server = ns1-08.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
#!/bin/bash

# Create hello parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

Hello névkiszolgálóit hello beolvasása `partners.contoso.net` zóna hello kimenetből.

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Hozzon létre hello rekordhalmaz és minden névkiszolgáló rekordjait.

```azurecli
#!/bin/bash

# Create hello record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a>Az összes erőforrás törlése

toodelete összes erőforrás létrehozása ebben a cikkben teljes hello a következő lépéseket:

1. Az Azure-portálon hello **Kedvencek** ablaktáblán kattintson a **összes erőforrás**. Kattintson a hello **contosorg** erőforráscsoport hello az összes erőforrás panel. Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja **contosorg** a hello **Szűrés név alapján...** mezőbe tooeasily hozzáférés hello erőforráscsoportot.
1. A hello **contosorg** panelen kattintson a hello **törlése** gomb.
1. hello portal meg tootype hello nevét, amelyet az toodelete hello erőforrás csoport tooconfirm azt. Típus *contosorg* hello erőforráscsoport-nevet, majd kattintson **törlése**. Erőforráscsoport törlésével törli az összes erőforrás hello erőforráscsoporton belül, ezért mindig meg arról, hogy tooconfirm erőforráscsoport hello tartalmának törlése előtt. hello portal hello erőforráscsoporton belül található összes erőforrást törli, majd törli a hello erőforráscsoport magát. Ez a folyamat több percig is eltarthat.

## <a name="next-steps"></a>Következő lépések

[DNS-zónák kezelése](dns-operations-dnszones.md)

[DNS-rekordok kezelése](dns-operations-recordsets.md)
