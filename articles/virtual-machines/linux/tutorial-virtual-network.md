---
title: "aaaAzure virtuális hálózatok és a Linux virtuális gépek |} Microsoft Docs"
description: "Útmutató – hello Azure CLI Azure virtuális hálózatok és a Linux virtuális gépek kezelése"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 57e6bd4de16f0e31d53dc67bf50dc5730d43712b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-hello-azure-cli"></a>Azure virtuális hálózatok és a Linux virtuális gépek kezelése hello Azure parancssori felület

Az Azure virtuális gépek Azure hálózatkezelés belső és külső hálózati kommunikációhoz használni. Ez az oktatóanyag végigvezeti a két virtuális gép telepítése és konfigurálása ezen a virtuális gépek Azure hálózatkezelés. Ebben az oktatóanyagban hello példák feltételezik, hogy, hogy hello virtuális gépeken üzemeltet egy adatbázis-háttér webalkalmazás azonban egy alkalmazás nem lett telepítve a hello oktatóanyag. Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Telepíthet egy virtuális hálózatot
> * Hozzon létre egy alhálózatot a virtuális hálózaton belül
> * Virtuális gépek tooa alhálózati csatolása
> * Virtuális gép nyilvános IP-címeinek kezelése
> * Bejövő internet forgalmának biztonságossá tétele
> * Virtuális gép tooVM forgalmának biztonságossá tétele


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="vm-networking-overview"></a>VM-hálózat – áttekintés

Egy Azure virtuális hálózatot a virtuális gépek között a biztonságos hálózati kapcsolatok engedélyezéséhez hello internetes és az egyéb Azure-szolgáltatások például az Azure SQL-adatbázis. Virtuális hálózatok logikai szegmensekben – ún alhálózatok osztható. Alhálózatok használt toocontrol hálózati folyamata, és egy biztonsági határként. A virtuális gép telepítésekor a virtuális hálózati adaptert, amely csatolt tooa alhálózati általában tartalmazza.

## <a name="deploy-virtual-network"></a>Virtuális hálózati telepítése

Ebben az oktatóanyagban egy virtuális hálózaton, két alhálózattal jön létre. A webalkalmazás üzemeltetéséhez előtér-alhálózatot, és egy adatbázis-kiszolgálót futtató háttér-alhálózatot.

Virtuális hálózat létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myRGNetwork* hello eastus helyen.

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a>Virtuális hálózat létrehozása

Velünk hello [az hálózati vnet létrehozása](/cli/azure/network/vnet#create) parancs toocreate egy virtuális hálózatot. Ebben a példában hello hálózati nevű *mvVnet* , és egy címelőtagot *10.0.0.0/16*. Egy alhálózat is létrejön, melynek a neve *mySubnetFrontEnd* és a előtaggal *10.0.1.0/24*. Az oktatóanyag későbbi részében egy előtér-virtuális Géphez csatlakoztatott toothis alhálózat. 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a>Hozzon létre az alhálózatot

Egy új alhálózatot kerül toohello virtuális hálózat hello [az alhálózaton virtuális hálózat létrehozása](/cli/azure/network/vnet/subnet#create) parancsot. Ebben a példában hello alhálózati nevű *mySubnetBackEnd* , és egy címelőtagot *10.0.2.0/24*. Ez az alhálózat összes háttérszolgáltatások használatos.

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

Ezen a ponton a hálózati létrehozott és szegmentált két alhálózat, egy az előtér-szolgáltatásokat, és egy másikat a háttér-szolgáltatásaihoz. Hello a következő szakaszban a virtuális gépek jönnek létre, és toothese alhálózatok csatlakoztatva.

## <a name="understand-public-ip-address"></a>Nyilvános IP-cím ismertetése

A nyilvános IP-cím lehetővé teszi, hogy Azure-erőforrások toobe elérhető-e hello internet. Ebben a szakaszban hello oktatóanyag egy virtuális gép létrehozása toodemonstrate hogyan toowork nyilvános IP-címek.

### <a name="allocation-method"></a>Lefoglalási módszer

Egy nyilvános IP-címet, dinamikus vagy statikus oszthat ki. Alapértelmezés szerint dinamikusan lefoglalt nyilvános IP-cím. Dinamikus IP-címek van kiadva, ha egy virtuális gép felszabadítása. Ennek következtében hello IP-cím toochange bármely, amely tartalmazza a virtuális gép felszabadítás művelet során.

hello elosztási módszert beállítható toostatic, amely biztosítja, hogy hello IP-címet meg kell maradnia hozzárendelt tooa VM, megszüntetett állapot esetén. A statikusan lefoglalt IP-cím használata esetén nem adható meg hello IP-címet saját magát. Ehelyett az történik a rendelkezésre álló címek készletét.

### <a name="dynamic-allocation"></a>Dinamikus lefoglalását

Virtuális gép létrehozásakor a hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancs hello alapértelmezett nyilvános IP-cím címkiosztási módszerét dinamikus. A következő példa hello a virtuális gép létrehozása egy dinamikus IP-címmel. 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontEndVM \
  --vnet-name myVnet \
  --subnet mySubnetFrontEnd \
  --nsg myNSGFrontEnd \
  --public-ip-address myFrontEndIP \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="static-allocation"></a>Statikus foglalási

A virtuális gépek hello létrehozásakor [az virtuális gép létrehozása](/cli/azure/vm#create) paranccsal, hello tartalmazza `--public-ip-address-allocation static` argumentum tooassign egy statikus nyilvános IP-cím. Ez a művelet nem mutatja be ebben az oktatóanyagban azonban hello a következő szakaszban a dinamikusan kiosztott IP-cím módosított tooa statikusan lefoglalt címet. 

### <a name="change-allocation-method"></a>Elosztási módszer váltása

hello IP-cím címkiosztási módszerét hello módosítható [az hálózati nyilvános ip-frissítés](/cli/azure/network/public-ip#update) parancsot. Ebben a példában hello IP-cím címkiosztási módszerét az előtér-VM megváltozott hello toostatic.

Első lépésként felszabadítani hello virtuális gép.

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

Használjon hello [az hálózati nyilvános ip-frissítés](/cli/azure/network/public-ip#update) tooupdate hello elosztási módszert parancsot. Ebben az esetben hello `--allocation-method` túl beállítása*statikus*.

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

Indítsa el a virtuális gép hello.

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a>Nyilvános IP-cím

Gyakran, egy virtuális Gépet nem kell toobe keresztül elérhető hello internet. a virtuális gép, egy nyilvános IP-címet, hello használata nélkül toocreate `--public-ip-address ""` dupla idézőjelek között egy üres számú argumentum. Ez a konfiguráció az oktatóanyag későbbi részében bemutatott

## <a name="secure-network-traffic"></a>Biztonságos hálózati adatforgalom

A hálózati biztonsági csoport (NSG) felsorolja azokat a szabályokat, amelyek engedélyezik vagy megtagadják a hálózati forgalom tooresources csatlakoztatott tooAzure virtuális hálózatot (VNet). Az NSG-k társított toosubnets vagy az egyes hálózati adapterek lehetnek. Ha egy NSG-t egy adott hálózati csatoló kapcsolódik, érvényes hello társított virtuális gép. Amikor egy NSG társított tooa alhálózati, hello szabályokat alkalmazni tooall erőforrások csatlakoztatott toohello alhálózat. 

### <a name="network-security-group-rules"></a>Hálózati biztonsági csoportszabályok

NSG-szabályok határozza meg, amelyben forgalom engedélyezett vagy megtagadott hálózati portok. hello szabályok is tartalmazza a forrás és cél IP-címtartományok, így egyes rendszerek vagy az alhálózatok közötti forgalmat szabályozott. NSG-szabályok terjednie a prioritással (1 – és 4096). Szabályok a prioritásuk szerinti sorrendben hello értékeli ki a rendszer. A 100 prioritással kiértékeli előtt szabály 200 prioritással.

Minden NSG tartalmaz egy alapértelmezett szabálykészletet. hello alapértelmezett szabályokat nem lehet törölni, de a legalacsonyabb prioritású hello hozzárendeli őket, mert azok felülbírálhatja hello létrehozott szabályok.

- **Virtuális hálózati** - származó forgalmat, és a befejezési egy virtuális hálózat bejövő és kimenő irányban is.
- **Internet** – a kimenő forgalom engedélyezve van, de a bejövő forgalmat blokkol.
- **Terheléselosztó** -engedélyezése Azure load terheléselosztó tooprobe hello állapotát a virtuális gépek és a szerepkörpéldányok. Ha nem használ elosztott terhelésű készlet, ez a szabály lehet felülbírálni.

### <a name="create-network-security-groups"></a>Hálózati biztonsági csoportok létrehozása

Hálózati biztonsági csoport is létrehozható, hello azonos időben is a virtuális gépek hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot. Annak során, hello NSG hello virtuális gépek hálózati illesztő társítva, és egy NSG-szabály automatikusan létrehozott tooallow adatforgalmat porton *22* forrásból. Ebben az oktatóanyagban a korábbi előtér-NSG volt az automatikusan létrehozott hello hello előtér-virtuális gép. Az NSG-szabályok is 22-es port számára létrehozott automatikus volt. 

Bizonyos esetekben lehet hasznos toopre-hozzon létre egy NSG-t, például amikor alapértelmezett SSH szabályokat nem lehet létrehozni, vagy ha hello NSG csatolt tooa alhálózaton kell lennie. 

Használjon hello [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) parancs toocreate hálózati biztonsági csoport.

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

Hello NSG tooa hálózati adapter közötti társítás, helyett hozzárendelnek egy alhálózathoz. Ebben a konfigurációban a virtuális gép, amely csatolt toohello örökli hello NSG-szabályok.

Hello meglévő nevű alhálózat frissítése *mySubnetBackEnd* rendelkező új NSG hello.

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

Most létrehoz egy virtuális gépet, amely csatolt toohello *mySubnetBackEnd*. Figyelje meg, hogy hello `--nsg` argumentum értéke üres dupla idézőjelek között. Az NSG nem kell létrehozni hello VM toobe. virtuális gép hello csatolt toohello háttér-alhálózathoz, előre létrehozott, a háttér NSG hello védi. Az NSG toohello VM vonatkozik. Emellett az információkban a hello `--public-ip-address` argumentum értéke üres dupla idézőjelek között. Ez a konfiguráció nélkül egy nyilvános IP-cím egy virtuális Gépet hoz létre. 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackEndVM \
  --vnet-name myVnet \
  --subnet mySubnetBackEnd \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="secure-incoming-traffic"></a>Biztonságos bejövő forgalom

Hello előtér-virtuális gép létrehozása után az NSG-szabályok létrehozásának tooallow bejövő forgalom 22-es porton. Ez a szabály lehetővé teszi, hogy az SSH-kapcsolatok toohello virtuális gép. Ebben a példában a forgalmat is engedélyezni kell a porton *80*. Ez a konfiguráció lehetővé teszi, hogy egy webes alkalmazás toobe hello VM elérhetők.

Használjon hello [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) parancs toocreate port szabály *80*.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGFrontEnd \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

hello előtér-virtuális gép jelenleg csak elérhető porton *22* és port *80*. Minden bejövő forgalom hello hálózati biztonsági csoport blokkolva van. Akkor lehet hasznos toovisualize hello NSG-szabályok konfigurációi. Visszatérési hello NSG-szabály konfiguráció hello [az hálózati szabálylistában](/cli/azure/network/nsg/rule#list) parancsot. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGFrontEnd --output table
```

Kimenet:

```azurecli-interactive 
Access    DestinationAddressPrefix      DestinationPortRange  Direction    Name                 Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -----------------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                                               22  Inbound      default-allow-ssh        1000  Tcp         Succeeded            myRGNetwork      *                      *
Allow     *                                               80  Inbound      http                      200  Tcp         Succeeded            myRGNetwork      *                      *
```

### <a name="secure-vm-toovm-traffic"></a>Virtuális gép tooVM forgalmának biztonságossá tétele

Virtuális gépek közötti hálózati biztonsági csoportszabályok is alkalmazhat. Ebben a példában az előtér-virtuális gép van szüksége a toocommunicate hello hello háttér-VM porton *22* és *3306*. Ez a konfiguráció lehetővé teszi, hogy az SSH-kapcsolatok az előtér-VM hello és is annak engedélyezése, hogy egy alkalmazás hello előtér-VM toocommunicate a háttér-MySQL-adatbázis. Az összes többi forgalom hello előtér- és a virtuális gépek között le kell tiltani.

Használjon hello [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) parancs toocreate egy szabály a 22-es portot. Figyelje meg, hogy hello `--source-address-prefix` argumentum meghatározza a érték *10.0.1.0/24*. Ez a konfiguráció biztosítja, hogy az csak a hello előtér-alhálózatból forgalom engedélyezve van-e keresztül hello NSG.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

Most adja hozzá egy forgalomra vonatkozó szabály MySQL 3306 porton.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

Végül, mert NSG-ket egy alapértelmezett szabály engedélyezése minden forgalomnak hello a virtuális gépek közötti azonos VNet szabály hozható létre hello háttér-NSG-k tooblock minden forgalmat. Itt láthatja, hogy hello `--priority` értéket kap *300*, amelyek verziója alacsonyabb, hogy mindkét hello NSG-t és a MySQL szabályokat. Ez a konfiguráció biztosítja, hogy SSH és a MySQL-forgalmat a rendszer nem tagadja hello NSG keresztül.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

hello háttér-virtuális gép jelenleg csak elérhető porton *22* és port *3306* hello előtér-alhálózatról. Minden bejövő forgalom hello hálózati biztonsági csoport blokkolva van. Akkor lehet hasznos toovisualize hello NSG-szabályok konfigurációi. Visszatérési hello NSG-szabály konfiguráció hello [az hálózati szabálylistában](/cli/azure/network/nsg/rule#list) parancsot. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGBackEnd --output table
```

Kimenet:

```azurecli-interactive 
Access    DestinationAddressPrefix    DestinationPortRange    Direction    Name       Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                           22                      Inbound      SSH             100  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Allow     *                           3306                    Inbound      MySQL           200  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Deny      *                           *                       Inbound      denyAll         300  Tcp         Succeeded            myRGNetwork      *                      *
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban létre, és biztonságos kapcsolódó toovirtual gépként Azure hálózatokhoz. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Telepíthet egy virtuális hálózatot
> * Hozzon létre egy alhálózatot a virtuális hálózaton belül
> * Virtuális gépek tooa alhálózati csatolása
> * Virtuális gép nyilvános IP-címeinek kezelése
> * Bejövő internet forgalmának biztonságossá tétele
> * Virtuális gép tooVM forgalmának biztonságossá tétele

Előzetes toohello oktatóanyag következő toolearn kapcsolatos adatok az Azure backup használatával virtuális gépek védelme. 

> [!div class="nextstepaction"]
> [Készítsen biztonsági másolatot a Linux virtuális gépek Azure-ban](./tutorial-backup-vms.md)
