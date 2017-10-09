---
title: "Oracle-adatbázishoz egy Azure virtuális gép aaaCreate |} Microsoft Docs"
description: "Gyorsan karban lehessen az Oracle-adatbázishoz 12c adatbázis mentése és az Azure környezetben futó."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/17/2017
ms.author: rclaus
ms.openlocfilehash: 83205154c3275d5f57b46c8acfb0cb4e5c68a412
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a>Oracle-adatbázishoz egy Azure virtuális gép létrehozása

Ez az útmutató részletek hello Azure CLI toodeploy hello az Azure virtuális gép használata [Oracle piactér gyűjtemény kép](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) a rendezés toocreate 12 c Oracle-adatbázishoz. Miután hello kiszolgáló van telepítve, kapcsolódni fog SSH-kapcsolaton keresztül a rendelés tooconfigure hello Oracle-adatbázishoz. 

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. 

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
## <a name="create-virtual-machine"></a>Virtuális gép létrehozása

egy virtuális gép (VM) toocreate hello használata [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot. 

hello alábbi példakód létrehozza a virtuális gépek nevű `myVM`. SSH-kulcsok, azt is hoz létre, ha még nem léteznek a kulcs alapértelmezett helye. toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

Hello virtuális gép létrehozása után az Azure CLI információkat a következő példa hasonló toohello jeleníti meg. Jegyezze fel a hello értékét `publicIpAddress`. A cím tooaccess hello virtuális gép használja.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/{snip}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="connect-toohello-vm"></a>Csatlakozás a virtuális gép toohello

az SSH-munkamenetet a virtuális gép, hello toocreate hello a következő parancsot használja. Cserélje le hello IP-cím hello `publicIpAddress` értéket a virtuális gép számára.

```bash 
ssh <publicIpAddress>
```

## <a name="create-hello-database"></a>Hello adatbázis létrehozása

hello Piactéri lemezképhez hello Oracle szoftver már telepítve van. Hozzon létre egy adatbázist az alábbiak szerint. 

1.  Váltás toohello *oracle* felügyelő, majd a naplózás inicializálásakor hello figyelő:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    hello hasonló toohello következő kimenete:

    ```bash
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written too/u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting too(ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of hello LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Start Date                23-MAR-2017 15:32:08
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Log File         /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening Endpoints Summary...
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))
    hello listener supports no services
    hello command completed successfully
    ```

2.  hello adatbázis létrehozása:

    ```bash
    dbca -silent \
           -createDatabase \
           -templateName General_Purpose.dbc \
           -gdbname cdb1 \
           -sid cdb1 \
           -responseFile NO_VALUE \
           -characterSet AL32UTF8 \
           -sysPassword OraPasswd1 \
           -systemPassword OraPasswd1 \
           -createAsContainerDatabase true \
           -numberOfPDBs 1 \
           -pdbName pdb1 \
           -pdbAdminPassword OraPasswd1 \
           -databaseType MULTIPURPOSE \
           -automaticMemoryManagement false \
           -storageType FS \
           -ignorePreReqs
    ```

    Néhány perc toocreate hello adatbázis vesz igénybe.

3. Oracle változók megadása

Csatlakozás előtt van szüksége a két tooset környezeti változók: *ORACLE_HOME* és *ORACLE_SID*.

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```
ORACLE_HOME és ORACLE_SID változók toohello .bashrc fájl is adhat hozzá. A környezeti változók hello a későbbi bejelentkezések volna mentse. Erősítse meg a következő hello utasítás hozzá vannak adva toohello `~/.bashrc` a választott szerkesztővel fájlt.

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a>Oracle EM Express kapcsolat

Grafikus felügyeleti eszköz használható tooexplore hello adatbázis, az Oracle EM Express beállítása. tooconnect tooOracle EM Express, először be kell állítania az Oracle hello port. 

1. Csatlakozás tooyour adatbázist sqlplus használatával:

    ```bash
    sqlplus / as sysdba
    ```

2. A csatlakozás után a EM expressz hello port 5502 beállítása

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. Nyissa meg hello tároló PDB1, ha nem már megnyitott, de az első hello állapotát:

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    hello hasonló toohello következő kimenete:

    ```bash
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. Ha a OPEN_MODE hello `PDB1` nincs OLVASÁSI ÍRNI, majd futtassa a hello következőket parancsok tooopen PDB1:

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

Tootype kell `quit` tooend hello sqlplus munkamenet és típus `exit` toologout hello oracle felhasználó.

## <a name="automate-database-startup-and-shutdown"></a>Adatbázis indítási és leállítási automatizálásához

Oracle-adatbázishoz hello alapértelmezés szerint automatikusan hello VM újraindításakor nem indul el. tooset hello Oracle adatbázis toostart fel automatikusan, először bejelentkezik rendszergazdaként. Ezt követően létrehozása, és egyes rendszer fájlok frissítése.

1. Jelentkezzen a legfelső szintű
    ```bash
    sudo su -
    ```

2.  A kedvenc szerkesztővel szerkessze a hello fájlt `/etc/oratab` , és módosítsa a hello alapértelmezett `N` túl`Y`:

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  Hozzon létre egy fájlt `/etc/init.d/dbora` és a Beillesztés hello a következő tartalmát:

    ```
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME toobe equivalent too$ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start hello Oracle databases:
        # hello following command assumes that hello Oracle sign-in
        # will not prompt hello user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop hello Oracle databases:
        # hello following command assumes that hello Oracle sign-in
        # will not prompt hello user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  A fájlok engedélyeinek módosítása *chmod* az alábbiak szerint:

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  Indítási és leállítási szimbolikus hivatkozásokat hoz létre a a következőképpen:

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  tootest a módosításokat, indítsa újra a virtuális gép hello:

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a>A hálózati kapcsolatot megnyitott portok

hello végső feladat tooconfigure van néhány külső végpontok száma. másolatot tooset hello Azure hálózati biztonsági csoportot, amely védi a virtuális gép hello, először lépjen ki a virtuális gép (kell rendelkeznie lett kezdődött SSH kívül ha újraindítás az előző lépésben) hello az SSH-munkamenetet. 

1.  tooopen hello végpont tooaccess hello Oracle-adatbázishoz távolról, használja a hálózati biztonsági csoport szabály létrehozása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) az alábbiak szerint: 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  tooopen hello végpont Oracle EM Express tooaccess távolról, használja a hálózati biztonsági csoport szabály létrehozása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) az alábbiak szerint:

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. Szükség esetén hello nyilvános IP-címet a virtuális gép újra az beszerzése [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show) az alábbiak szerint:

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  Csatlakoztassa a böngésző EM Express. Ügyeljen arra, hogy a böngésző kompatibilis EM expressz (Flash esetén): 

    ```
    https://<VM ip address or hostname>:5502/em
    ```

Hello segítségével bejelentkezhet **SYS** fiókot, és ellenőrizze a hello **SYSDBA csoport szerint** jelölőnégyzetet. Használjon hello jelszó **OraPasswd1** telepítése során beállított. 

![Képernyőfelvétel a hello Oracle OEM Express bejelentkezési oldal](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Miután befejezte az első Oracle adatbázis fel az Azure-on, és hello virtuális gép már nem szükséges, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások parancsot.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

Tudnivalók más [Oracle megoldások Azure](oracle-considerations.md). 

Próbálja meg hello [telepítése és konfigurálása Oracle automatikus tárhely-kezelés](configure-oracle-asm.md) oktatóanyag.
