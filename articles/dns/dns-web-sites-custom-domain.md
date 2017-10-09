---
title: "a webes alkalmazás aaaCreate egyéni DNS-rekordok |} Microsoft Docs"
description: "Hogyan toocreate egyéni tartomány DNS Azure DNS-sel webalkalmazás rögzíti."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 6c16608c-4819-44e7-ab88-306cf4d6efe5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 070c808a55bab922eb624d99ae5c275d8eaa5aaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Egyéni tartomány DNS-rekordok webalkalmazás létrehozása

A webalkalmazások Azure DNS toohost egyéni tartományt is használhatja. Például egy Azure webalkalmazás létrehozásakor, és azt szeretné, hogy a felhasználók tooaccess azt által contoso.com, vagy a www.contoso.com teljes tartománynév használatával.

toodo, két bejegyzés toocreate van:

* Egy legfelső szintű "A" rekord mutató toocontoso.com
* Egy "CNAME" rekordja hello www nevét, amely toohello mutat egy rekord

Ne feledje, hogy az Azure-ban létrehoz egy A rekordot egy webalkalmazás, ha A rekord manuálisan frissíteni kell ha hello hello web app módosítások alapul szolgáló IP-cím hello.

## <a name="before-you-begin"></a>Előkészületek

Mielőtt elkezdené, először hozzon létre egy DNS-zóna Azure DNS-ben, és hello zóna delegálása a regisztráló tooAzure DNS a.

1. a DNS-zónák toocreate kövesse hello [hozzon létre egy DNS-zóna](dns-getstarted-create-dnszone.md).
2. toodelegate a DNS-tooAzure DNS, kövesse hello [DNS-tartomány delegálás](dns-domain-delegation.md).

Zóna létrehozása és delegálása azt tooAzure DNS, után is létrehozhat rögzíti az egyéni tartományhoz.

## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Az egyéni tartomány az A rekord létrehozása

Az A rekord használt toomap neve tooits IP-címnek. Az alábbi példa hello azt fogja rendeljen egy A rekordot tooan IPv4-cím:

### <a name="step-1"></a>1. lépés

Az A rekord létrehozása és hozzárendelése tooa változó $rs

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a>2. lépés

Adja hozzá az IPv4-alapú hello érték korábban létrehozott toohello rekordhalmaz "@" hozzárendelt hello $rs változó segítségével. hello hozzárendelt IPv4 érték lesz hello IP-címe a webalkalmazás.

toofind hello IP-cím egy webalkalmazás kövesse hello [egyéni tartománynév beállítása az Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a>3. lépés

Hello módosítások toohello rekordhalmaz véglegesítése. Használjon `Set-AzureRMDnsRecordSet` tooupload hello toohello rekordhalmaz tooAzure DNS módosítja:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Az egyéni tartomány CNAME rekord létrehozása

Ha a tartomány már az Azure DNS kezeli (lásd: [DNS-tartomány delegálás](dns-domain-delegation.md), hello példa toocreate contoso.azurewebsites.net CNAME rekord a következő hello is használhatja.

### <a name="step-1"></a>1. lépés

Nyissa meg a Powershellt, és hozzon létre egy új CNAME rekordkészlete, és rendelje hozzá a tooa változó $rs. A példában a "idő toolive" 600 másodperc DNS-zónában "a contoso.com" nevű rekordkészlet típus CNAME hoz létre.

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

a következő példa hello hello válasz.

```
Name              : www
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>2. lépés

Hello CNAME rekordhalmaz létrehozása után kell toocreate fog mutatni toohello web app alias értéke.

A korábban hozzárendelt változó "$rs" hello használatával toocreate hello alias hello PowerShell-parancsot a hello web app contoso.azurewebsites.net.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

a következő példa hello hello válasz.

```
    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>3. lépés

Hello segítségével hello változtatások véglegesítése a határidő `Set-AzureRMDnsRecordSet` parancsmagot:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

Ellenőrizheti hello rekord megfelelően lett létrehozva hello "www.contoso.com" nslookup, használatával lekérdezésével alább látható módon:

```
PS C:\> nslookup
Default Server:  Default
Address:  192.168.0.1

> www.contoso.com
Server:  default server
Address:  192.168.0.1

Non-authoritative answer:
Name:    <instance of web app service>.cloudapp.net
Address:  <ip of web app service>
Aliases:  www.contoso.com
contoso.azurewebsites.net
<instance of web app service>.vip.azurewebsites.windows.net
```

## <a name="create-an-awverify-record-for-web-apps"></a>A webalkalmazások egy "awverify" rekord létrehozása

Ha úgy dönt, az A rekord toouse a webalkalmazás, haladjon végig a hitelesítési folyamat tooensure meg saját hello egyéni tartományt. Az ellenőrzési lépés "awverify" nevű különleges CNAME rekord létrehozásával történik. Ez a szakasz csak a tooA rekordok vonatkozik.

### <a name="step-1"></a>1. lépés

Hello "awverify" rekordot kell létrehozni. Hello az alábbi példában szereplő létrehozunk hello "aweverify" rekord contoso.com tooverify tulajdonosát hello mutató egyéni tartományhoz.

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

a következő példa hello hello válasz.

```
Name              : awverify
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>2. lépés

"Awverify" hello rekordhalmaz létrehozása után rendelje hozzá a hello CNAME-rekordhalmazt alias. Hello az alábbi példában a Microsoft hello CNAMe rekordkészlete alias tooawverify.contoso.azurewebsites.net rendeli.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

a következő példa hello hello válasz.

```
    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>3. lépés

Hello segítségével hello változtatások véglegesítése a határidő `Set-AzureRMDnsRecordSet cmdlet`, ahogy az alábbi hello parancsot.

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a>Következő lépések

Hello kövesse [beállítása egy egyéni tartománynevet, az App Service](../app-service-web/web-sites-custom-domain-name.md) tooconfigure a webes alkalmazás toouse egyéni tartományt.
