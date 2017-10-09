---
title: "aaaImport és exportálás a tartományzóna fájl használata az Azure CLI 1.0 DNS tooAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooimport és exportálás a DNS zóna fájl tooAzure DNS Azure CLI 1.0 használatával"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: f5797782-3005-4663-a488-ac0089809010
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 4c3163395e151e9934c730349b828c612491016f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-a-dns-zone-file-using-hello-azure-cli-10"></a>Importálni és exportálni egy DNS-zónafájlját hello Azure CLI 1.0 használatával 

Ez a cikk bemutatja, hogyan hogyan tooimport és exportálási DNS zóna fájlok az Azure DNS használatával hello Azure CLI 1.0.

## <a name="introduction-toodns-zone-migration"></a>Bevezetés tooDNS zóna áttelepítése

A DNS-zónafájlját egy szövegfájlt, amely az összes hello zónában tartománynévrendszer (DNS) rekord részleteit tartalmazza. Ez azt jelenti, szabványos formátumban, így a megfelelő DNS-rekordok átvitele a DNS-rendszerek között. Egy zóna fájllal egy gyors, megbízható, és kényelmesen tootransfer egy DNS-zónát, vagy abból az Azure DNS-ben.

Az Azure DNS támogatja, importálása és exportálása a zóna fájlok hello Azure parancssori felület (CLI) használatával. Zóna fájl importálás **nem** jelenleg támogatott Azure PowerShell vagy hello Azure-portálon keresztül.

hello Azure CLI 1.0 használt szolgáltatások kezeléséhez az Azure platformfüggetlen parancssori eszköz. Használható a hello Windows, Mac és Linux platformokon a hello [Azure letöltési oldalon](https://azure.microsoft.com/downloads/). Az importálás és exportálás a zóna fájlokat, mert hello leggyakoribb neve server szoftver, többplatformos támogatást fontos [kötési](https://www.isc.org/downloads/bind/), jellemzően futó Linux.

> [!NOTE]
> Jelenleg az Azure CLI hello két verziója. CLI1.0 Node.js alapul, és rendelkezik az "azure" kezdve parancsok.
> CLI2.0 "az" kezdve parancsok rendelkezik, és a Python alapul. Amíg a zóna fájl importálása verziójával is támogatott, azt javasoljuk, hello CLI1.0 parancsok, ezen az oldalon leírtak szerint.

## <a name="obtain-your-existing-dns-zone-file"></a>A meglévő DNS-zónafájlját beszerzése

A DNS-zónafájlját importál Azure DNS-ben, meg kell tooobtain hello zóna fájl egy példányát. fájl forrásában hello attól függ, ahol hello DNS-zóna jelenleg üzemel.

* Ha a DNS-zóna (például a tartományregisztráló, dedikált DNS-szolgáltató vagy alternatív felhőszolgáltatóként) egy partner szolgáltatás működteti, a service hello képességét toodownload hello DNS-zónafájlját kell biztosítania.
* Ha a Windows DNS-üzemelteti a DNS-zónát, hello alapértelmezett mappa hello zóna fájlok: **%systemroot%\system32\dns**. hello teljes elérési útja tooeach zóna fájl is látható hello **általános** hello DNS-konzol fülre.
* Ha a DNS-zóna BIND használatával, hello hely hello zónafájl zónák meg van adva hello BIND konfigurációs fájl **named.conf**.

> [!NOTE]
> GoDaddy letöltésének zóna fájlok formátuma némileg nem szabványos. Toocorrect ez előtt kell a zóna fájlok importálása az Azure DNS-ben.
>
> Teljesen minősített nevek hello RDATA minden DNS-rekord a DNS-nevek vannak megadva, de nem rendelkeznek a záró "." Ez azt jelenti, hogy azok más relatív nevek DNS rendszer értelmezi. Szüksége tooedit hello zóna fájl tooappend hello leáll "." tootheir neve előtt az Azure DNS importálja azokat.
>
> Például hello CNAME rekordot a "www 3600 CNAME contoso.com" úgy kell módosítani túl "www 3600 IN CNAME contoso.com."
> (az a záró ".").

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Egy DNS-zóna fájlt importálja az Azure DNS

Új zóna az Azure DNS-zóna fájlok importálása hoz, ha egy nem létezik. Ha hello zóna már létezik, hello rekordhalmazok hello zóna fájlban kell egyesíthető hello meglévő rekordhalmazok.

### <a name="merge-behavior"></a>Viselkedés egyesítése

* Alapértelmezés szerint a meglévő és új rekordhalmazok egyesítődnek. Egyesített rekordhalmaz belüli azonos rekordokat deszerializálni duplikált.
* Másik lehetőségként hello megadásával `--force` lehetőségre, a meglévő rekordhalmazok új rekordhalmazok az importálási folyamat cserél hello. Meglévő rekordhalmazok, amely nem rendelkezik a megfelelő rekordhalmazt hello importált zónafájl nem lesznek eltávolítva.
* Rekordhalmazok egyesítésekor hello idő toolive (TTL) elérésű, korábban létező rekordkészletek szerepel. Ha `--force` van használva hello hello új rekordhalmaz Élettartammal szolgál.
* Kezdő hatóság (SOA) paraméterek (kivéve `host`) fájlból hello importált zóna, függetlenül attól, hogy mindig készít `--force` szolgál. Ehhez hasonlóan hello névkiszolgáló-rekord hello zóna felső pontja állítsa be, a hello TTL mindig használatban van hello importált zóna fájlból.
* Az importált CNAME rekord nem váltják ki egy meglévő CNAME jegyezze fel az azonos név, kivéve, ha hello hello `--force` paraméter meg van adva.
* Ha ütközés lép fel egy olyan CNAME rekordot, és egy másik rekord között azonos nevet, de különböző hello adja (függetlenül attól, amely meglévő vagy új), hello meglévő rekord marad. Ez nem függ a hello használata `--force`.

### <a name="additional-information-about-importing"></a>További információt a importálása

hello következő megjegyzések tartalmazzák hello zóna további technikai részleteiért importálási folyamat.

* Hello `$TTL` direktíva nem kötelező, és támogatja azt. Ha nem `$TTL` irányelv kap, egy explicit TTL nélkül importálásának tooa alapértelmezett élettartam 3600 másodperc. Ha két rögzíti a hello azonos rekordhalmaz különböző TTLs megadásához hello alacsonyabb értéket használja.
* Hello `$ORIGIN` direktíva nem kötelező, és támogatja azt. Ha nem `$ORIGIN` van beállítva, alapértelmezett hello használt értéke hello zóna neve, ahogy hello parancssorban (plusz hello leáll ".").
* Hello `$INCLUDE` és `$GENERATE` nem támogatott.
* Ezek a rekordok típusok támogatottak: A, AAAA, CNAME, MX, NS, SOA, SRV és TXT.
* hello SOA-rekord automatikusan létrejön Azure DNS-zóna létrehozásakor. Egy zóna fájl importálásakor összes SOA típusú paramétert kell venni hello zónafájl *kivéve* hello `host` paraméter. Ez a paraméter Azure DNS által nyújtott hello értéket használja. Ennek az az oka ennek a paraméternek toohello elsődleges névkiszolgáló Azure DNS által nyújtott kell hivatkoznia.
* hello neve server rekordhalmaz hello zóna csúcsán is automatikusan hozza létre Azure DNS hello zóna létrehozásakor. Csak hello a rekordhalmaz Élettartammal importálva van. Ezeket a rekordokat az Azure DNS által nyújtott hello névkiszolgálói neveket tartalmaz. hello erőforrásrekord-adatokat nem írják hello értékek hello importált zóna fájlban találhatók.
* Nyilvános előzetes Azure DNS TXT rekordok csak egyetlen-karakterlánc támogatja. Karakterláncsoros TXT-rekordok vannak összefűzött és csonkolt too255 karakter lehet.

### <a name="cli-format-and-values"></a>Parancssori felület formátuma és értékei

hello hello Azure CLI parancs tooimport DNS-zóna formátuma:

```azurecli
azure network dns zone import [options] <resource group> <zone name> <zone file name>
```

Értékek:

* `<resource group>`van hello zóna Azure DNS-ben hello hello erőforráscsoport nevét.
* `<zone name>`hello zóna hello neve van.
* `<zone file name>`hello elérési útja vagy neve hello zóna fájl toobe importálva van.

Ha egy ilyen nevű zónát hello erőforráscsoporthoz tartozik, nem létezik, az Ön létrejön. Ha hello zóna már létezik, hello importált rekordhalmazok egyesítve lesznek az meglévő rekordhalmazok. toooverwrite hello meglévő rekordhalmazok, használja a hello `--force` lehetőséget.

a zóna fájl ténylegesen importálása, hello használata nélkül tooverify hello formátumát `--parse-only` lehetőséget.

### <a name="step-1-import-a-zone-file"></a>1. lépés Zóna fájl importálása

egy zóna fájl hello zóna tooimport **contoso.com**.

1. Jelentkezzen be tooyour Azure-előfizetés hello Azure CLI 1.0 használatával.

    ```azurecli
    azure login
    ```

2. Válassza ki a kívánt toocreate az új DNS-zónát hello előfizetést.

    ```azurecli
    azure account set <subscription name>
    ```

3. Az Azure DNS egy olyan Azure csak erőforrás-kezelő szolgáltatás, így hello Azure CLI kapcsolt tooResource kezelő módban kell lennie.

    ```azurecli
    azure config mode arm
    ```

4. Hello Azure DNS-szolgáltatás használata előtt regisztrálnia kell az előfizetési toouse hello Microsoft.Network erőforrás-szolgáltató. (Ez a műveletet egyszer kell elvégezni az egyes előfizetésekhez.)

    ```azurecli
    azure provider register Microsoft.Network
    ```

5. Ha Ön nem rendelkezik ilyennel, szükség toocreate egy erőforrás-kezelő erőforráscsoportot.

    ```azurecli
    azure group create myresourcegroup westeurope
    ```

6. tooimport hello zóna **contoso.com** hello fájlból **contoso.com.txt** be egy új DNS-zóna hello erőforráscsoportban **myresourcegroup**, hello paranccsal `azure network dns zone import`.<BR>Ez a parancs hello zóna fájlt, és elemzéséhez. hello parancs végrehajtása parancsokat a hello Azure DNS-szolgáltatás toocreate hello zónát, és minden hello rekordhalmazok hello zónában. hello parancs jelentések folyamatban hello konzolablakban, továbbá az esetleges hibákat vagy figyelmeztetéseket. Rekordhalmazok sorozat jönnek létre, mert szükség lehet néhány perc tooimport nagy zónafájl.

    ```azurecli
    azure network dns zone import myresourcegroup contoso.com contoso.com.txt
    ```

### <a name="step-2-verify-hello-zone"></a>2. lépés Ellenőrizze a hello zóna

tooverify hello DNS-zóna hello fájl importálása után használhatja hello a következő módszerek egyikét:

* Hello rögzíti az Azure parancssori felület parancs a következő hello segítségével jeleníthetők meg:

    ```azurecli
    azure network dns record-set list myresourcegroup contoso.com
    ```

* Hello rekordok hello PowerShell parancsmag segítségével listázhatja `Get-AzureRmDnsRecordSet`.
* Használhat `nslookup` tooverify névfeloldás hello rögzíti. Mivel hello zóna még nem delegált, explicit módon kell toospecify hello megfelelő Azure DNS névkiszolgálóit. hello következő minta bemutatja, hogyan tooretrieve hello névkiszolgálói neveket hozzárendelve toohello zóna. Informatikai is bemutatja, hogyan tooquery hello "www" történő rögzítéséhez `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up hello DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/.../resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>3. lépés DNS-delegálás frissítése

Miután ellenőrizte, hogy helyesen lettek importálva hello zóna, tooupdate hello DNS delegálás toopoint toohello van szüksége az Azure DNS-névhez kiszolgálók. További információkért lásd: hello cikk [hello DNS-delegálás frissítése](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Exportálja egy DNS-zónafájlját az Azure DNS-ben

hello hello Azure CLI parancs tooimport DNS-zóna formátuma:

```azurecli
azure network dns zone export [options] <resource group> <zone name> <zone file name>
```

Értékek:

* `<resource group>`van hello zóna Azure DNS-ben hello hello erőforráscsoport nevét.
* `<zone name>`hello zóna hello neve van.
* `<zone file name>`hello zóna fájl toobe exportált hello/útvonalnév van.

Hello zóna importálás, először a toosign, válassza ki az előfizetés, hello Azure CLI toouse Resource Manager módra konfigurálni.

### <a name="tooexport-a-zone-file"></a>egy zóna fájl tooexport

1. Jelentkezzen be tooyour Azure-előfizetés hello Azure parancssori felület használatával.

    ```azurecli
    azure login
    ```

2. Válassza ki a kívánt toocreate a DNS-zóna hello előfizetést.

    ```azurecli
    azure account set <subscription name>
    ```

3. Az Azure DNS egy olyan Azure csak erőforrás-kezelő szolgáltatás. hello Azure CLI kapcsolt tooResource kezelő módban kell lennie.

    ```azurecli
    azure config mode arm
    ```

4. tooexport hello Azure DNS-zónával **contoso.com** erőforráscsoportban **myresourcegroup** toohello fájl **contoso.com.txt** (hello aktuális mappában), futtassa a `azure network dns zone export`. Ez a parancs hívások hello Azure DNS szolgáltatást tooenumerate rekordkészletek hello zónában, és hello eredmények tooa BIND-kompatibilis zóna-fájl exportálását.

    ```azurecli
    azure network dns zone export myresourcegroup contoso.com contoso.com.txt
    ```
