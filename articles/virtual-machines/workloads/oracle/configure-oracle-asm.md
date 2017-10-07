---
title: "egy Azure Linux virtuális gép mentése Oracle ASM aaaSet |} Microsoft Docs"
description: "Gyorsan karban lehessen Oracle ASM be és az Azure környezetben futna."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/19/2017
ms.author: rclaus
ms.openlocfilehash: d6a7046638e919876477d46943faabcb1872acac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a>Oracle címterület-kezelés beállítása az Azure Linux virtuális gépen  

Az Azure virtuális gépek adjon meg egy teljes mértékben konfigurálhatók és rugalmas számítási környezet. Ez az oktatóanyag ismerteti alapszintű Azure-beli virtuális gép telepítési hello telepítése és konfigurálása az Oracle automatikus tárolási kezelési (ASM) együtt.  Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Hozzon létre, és csatlakozzon az Oracle adatbázis VM tooan
> * Telepítse és konfigurálja az Oracle automatikus tárolók kezelése
> * Telepítse és konfigurálja az Oracle rács infrastruktúra
> * Oracle ASM telepítés inicializálása
> * A címterület-kezelés által felügyelt az Oracle-adatbázis létrehozása


[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="prepare-hello-environment"></a>Hello környezet előkészítése

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

egy erőforráscsoport, toocreate hello használata [az csoport létrehozása](/cli/azure/group#create) parancsot. Egy Azure erőforráscsoport egy olyan logikai tároló, amelyre erőforrások telepítése és kezelése. Ebben a példában az erőforráscsoport neve *myResourceGroup* a hello *eastus* régióban.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-vm"></a>Virtuális gép létrehozása

a virtuális gépek toocreate hello Oracle adatbázis-lemezképen alapuló és toouse Oracle ASM konfigurálják, hello használata [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot. 

hello alábbi példa létrehoz egy 50 GB négy csatolt adatlemezekkel rendelkező Standard_DS2_v2 méretű myVM nevű virtuális gép. Ha még nem léteznek hello alapértelmezett kulcshelyen, SSH-kulcsok is létrehoz.  toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.  

   ```azurecli-interactive
   az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50 50 50 50
   ```

Hello virtuális gép létrehozása után az Azure CLI információkat a következő példa hasonló toohello jeleníti meg. Jegyezze fel a hello értékét `publicIpAddress`. A cím tooaccess hello virtuális gép használja.

   ```azurecli
   {
     "fqdns": "",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
     "location": "eastus",
     "macAddress": "00-0D-3A-36-2F-56",
     "powerState": "VM running",
     "privateIpAddress": "10.0.0.4",
     "publicIpAddress": "13.64.104.241",
     "resourceGroup": "myResourceGroup"
   }
   ```

### <a name="connect-toohello-vm"></a>Csatlakozás a virtuális gép toohello

az SSH-munkamenetet toocreate hello VM és további beállításokat, a következő parancs hello használata. Cserélje le hello IP-cím hello `publicIpAddress` értéket a virtuális gép számára.

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a>Oracle ASM telepítése

tooinstall Oracle ASM, teljes hello a következő lépéseket. 

Oracle ASM telepítésével kapcsolatos további információkért lásd: [Oracle ASMLib letölti az Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).  

1. A címterület-kezelési telepítésének sorrendje toocontinue legfelső szintű toologin lesz szüksége:

   ```bash
   sudo su -
   ```
   
2. A parancsok további tooinstall Oracle ASM összetevőket:

   ```bash
    yum list | grep oracleasm 
    yum -y install kmod-oracleasm.x86_64 
    yum -y install oracleasm-support.x86_64 
    wget http://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
   ```

3. Győződjön meg arról, hogy telepítve van-e az Oracle ASM:

   ```bash
   rpm -qa |grep oracleasm
   ```

    hello a parancs kimenetében hello összetevők a következő:

    ```bash
   oracleasm-support-2.1.10-4.el6.x86_64
   kmod-oracleasm-2.0.8-15.el6_9.x86_64
   oracleasmlib-2.0.12-1.el6.x86_64
    ```

4. A címterület-kezelés megfelelően igényel meghatározott felhasználókra és szerepkörökre a rendelés toofunction. a következő parancsok hello hello működéséhez szükséges felhasználói fiókok és csoportok létrehozása: 

   ```bash
    groupadd -g 54345 asmadmin 
    groupadd -g 54346 asmdba 
    groupadd -g 54347 asmoper 
    useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid 
    usermod -g oinstall -G dba,asmdba,asmadmin oracle
   ```

5. Ellenőrizze a felhasználók és csoportok létrehozott megfelelően:

   ```bash
   id grid
   ```

    hello a parancs kimenetében hello következő felhasználókat és csoportokat:

    ```bash
    uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
    ```
 
6. Hozzon létre egy mappát felhasználói *rács* és hello tulajdonos módosítása:

   ```bash
   mkdir /u01/app/grid 
   chown grid:oinstall /u01/app/grid
   ```

## <a name="set-up-oracle-asm"></a>Oracle címterület-kezelés beállítása

Ebben az oktatóanyagban hello alapértelmezett felhasználói van *rács* és hello alapértelmezett csoport *asmadmin*. Győződjön meg arról, hogy hello *oracle* felhasználói hello asmadmin csoport része. az Oracle ASM telepítése, a következő lépéseket teljes hello mentése tooset:

1. Hello Oracle címterület-kezelési könyvtár illesztőprogram beállítása magában foglalja a hello alapértelmezett felhasználói (grid) és az alapértelmezett csoportot (asmadmin) valamint a rendszerindító meghajtó toostart hello konfigurálása (válassza ki a y), és a rendszerindító lemezek tooscan (y kiválasztása). Tooanswer hello kér a parancs a következő hello lesz szüksége:

   ```bash
   /usr/sbin/oracleasm configure -i
   ```

   a parancs kimenetének hello alábbihoz hasonló toohello a következő, a megválaszolt kérdések toobe leállítása.

    ```bash
   Configuring hello Oracle ASM library driver.

   This will configure hello on-boot properties of hello Oracle ASM library
   driver. hello following questions will determine whether hello driver is
   loaded on boot and what permissions it will have. hello current values
   will be shown in brackets ('[]'). Hitting <ENTER> without typing an
   answer will keep that current value. Ctrl-C will abort.

   Default user tooown hello driver interface []: grid
   Default group tooown hello driver interface []: asmadmin
   Start Oracle ASM library driver on boot (y/n) [n]: y
   Scan for Oracle ASM disks on boot (y/n) [y]: y
   Writing Oracle ASM library driver configuration: done
   ```

2. Hello lemez konfiguráció megtekintése:
   ```bash
   cat /proc/partitions
   ```

   a parancs kimenetének hello alábbihoz hasonló toohello a rendelkezésre álló lemezek tőzsdei követően

   ```bash
   8       16   14680064 sdb
   8       17   14678976 sdb1
   8        0   52428800 sda
   8        1     512000 sda1
   8        2   51915776 sda2
   8       48   52428800 sdd
   8       64   52428800 sde
   8       80   52428800 sdf
   8       32   52428800 sdc
   11       0       1152 sr0
   ```

3. Lemez formázása */dev/sdc* hello futtassa a következő parancsot, és a fogadó hello kérdéseket:
   - *n*Új partíció
   - *p* elsődleges partíció
   - *1* tooselect hello első partíció
   - nyomja le az ENTER `enter` a hello alapértelmezett első palack
   - nyomja le az ENTER `enter` a hello alapértelmezett utolsó palack
   - nyomja le az ENTER *w* toowrite hello módosítások toohello partíciós tábla  

   ```bash
   fdisk /dev/sdc
   ```
   
   Használja a fenti hello választ, hello fdisk parancs kimenetét hello alábbihoz hasonló hello:

   ```bash
   Device contains not a valid DOS partition table, or Sun, SGI or OSF disklabel
   Building a new DOS disklabel with disk identifier 0xf865c6ca.
   Changes will remain in memory only, until you decide toowrite them.
   After that, of course, hello previous content won't be recoverable.

   Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

   hello device presents a logical sector size that is smaller than
   hello physical sector size. Aligning tooa physical sector (or optimal
   I/O) size boundary is recommended, or performance may be impacted.

   WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
           switch off hello mode (command 'c') and change display units to
           sectors (command 'u').

   Command (m for help): n
   Command action
     e   extended
     p   primary partition (1-4)
   p
   Partition number (1-4): 1
   First cylinder (1-6527, default 1):
   Using default value 1
   Last cylinder, +cylinders or +size{K,M,G} (1-6527, default 6527):
   Using default value 6527

   Command (m for help): w
   hello partition table has been altered!

   Calling ioctl() toore-read partition table.
   Syncing disks.
   ```

4. Ismétlődő hello fdisk parancsot megelőző `/dev/sdd`, `/dev/sde`, és `/dev/sdf`.

5. Ellenőrizze a lemezkonfigurációt hello:

   ```bash
   cat /proc/partitions
   ```

   hello parancs kimenetében hello hello hasonlóan kell kinéznie:

   ```bash
   major minor  #blocks  name

     8       16   14680064 sdb
     8       17   14678976 sdb1
     8       32   52428800 sdc
     8       33   52428096 sdc1
     8       48   52428800 sdd
     8       49   52428096 sdd1
     8       64   52428800 sde
     8       65   52428096 sde1
     8       80   52428800 sdf
     8       81   52428096 sdf1
     8        0   52428800 sda
     8        1     512000 sda1
     8        2   51915776 sda2
     11       0    1048575 sr0
   ```

6. Hello Oracle címterület-kezelési szolgáltatás állapotának ellenőrzéséhez és hello Oracle címterület-kezelés szolgáltatás elindítása:

   ```bash
   service oracleasm status 
   service oracleasm start
   ```

   hello parancs kimenetében hello hello hasonlóan kell kinéznie:
   
   ```bash
   Checking if ASM is loaded: no
   Checking if /dev/oracleasm is mounted: no
   Initializing hello Oracle ASMLib driver:                     [  OK  ]
   Scanning hello system for Oracle ASMLib disks:               [  OK  ]
   ```

7. Oracle ASM lemezek létrehozásához:

   ```bash
   service oracleasm createdisk ASMSP /dev/sdc1 
   service oracleasm createdisk DATA /dev/sdd1 
   service oracleasm createdisk DATA1 /dev/sde1 
   service oracleasm createdisk FRA /dev/sdf1
   ```    

   hello parancs kimenetében hello hello hasonlóan kell kinéznie:

   ```bash
   Marking disk "ASMSP" as an ASM disk:                       [  OK  ]
   Marking disk "DATA" as an ASM disk:                        [  OK  ]
   Marking disk "DATA1" as an ASM disk:                       [  OK  ]
   Marking disk "FRA" as an ASM disk:                         [  OK  ]
   ```

8. Oracle ASM lemezek listáját:

   ```bash
   service oracleasm listdisks
   ```   

   hello hello parancs kimenetében ki a következő Oracle ASM lemezek hello:

   ```bash
    ASMSP
    DATA
    DATA1
    FRA
   ```

9. Módosíthatja a hello gyökérszintű, oracle és rács felhasználók hello jelszavát. **Jegyezze fel ezeket az új jelszót a** módon használja őket később hello telepítése során.

   ```bash
   passwd oracle 
   passwd grid 
   passwd root
   ```

10. Hello mappa engedély módosítása:

   ```bash
   chmod -R 775 /opt 
   chown grid:oinstall /opt 
   chown oracle:oinstall /dev/sdc1 
   chown oracle:oinstall /dev/sdd1 
   chown oracle:oinstall /dev/sde1 
   chown oracle:oinstall /dev/sdf1 
   chmod 600 /dev/sdc1 
   chmod 600 /dev/sdd1 
   chmod 600 /dev/sde1 
   chmod 600 /dev/sdf1
   ```

## <a name="download-and-prepare-oracle-grid-infrastructure"></a>Töltse le és Oracle rács infrastruktúra előkészítése

toodownload és hello Oracle rács infrastruktúra szoftver, a következő lépéseket teljes hello előkészítése:

1. Oracle rács infrastruktúra letöltését hello [Oracle ASM letöltési oldala](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html). 

   Című hello letöltés alatt **Oracle adatbázis 12c kiadás 1 rács infrastruktúra (12.1.0.2.0) a Linux x86-64**, töltse le a két hello .zip fájlokat.

2. Hello .zip fájlokat tooyour ügyfélszámítógép letöltése után biztonságos másolási protokoll (SCP) toocopy hello fájlok tooyour VM is használhatja:

   ```bash
   scp *.zip <publicIpAddress>:.
   ```

3. SSH vissza az Oracle Azure-ban a rendelés toomove hello .zip fájlokat hello az / opt mappát. Majd módosíthatja hello fájlok hello tulajdonosa:

   ```bash
   ssh <publicIPAddress>
   sudo mv ./*.zip /opt
   cd /opt
   sudo chown grid:oinstall linuxamd64_12102_grid_1of2.zip
   sudo chown grid:oinstall linuxamd64_12102_grid_2of2.zip
   ```

4. Hello fájlokat csomagolja ki. (Telepítés hello Linux csomagolja ki eszköz Ha még nincs telepítve.)
   
   ```bash
   sudo yum install unzip
   sudo unzip linuxamd64_12102_grid_1of2.zip
   sudo unzip linuxamd64_12102_grid_2of2.zip
   ```

5. Engedély módosítása:
   
   ```bash
   sudo chown -R grid:oinstall /opt/grid
   ```

6. Frissítés konfigurált lapozóterület. Az Oracle rács összetevők legalább 6,8 GB swap terület tooinstall rács kell. hello alapértelmezett lapozófájl-kapacitás az Azure-ban Oracle Linux képek mérete csak 2048MB. Tooincrease kell `ResourceDisk.SwapSizeMB` a hello `/etc/waagent.conf` fájlt, és indítsa újra a hello WALinuxAgent szolgáltatást ahhoz, hogy a frissített hello beállítások tootake hatást. Mivel a csak olvasható fájlba, toochange fájl engedélyek tooenable írási hozzáférés szükséges.

   ```bash
   sudo chmod 777 /etc/waagent.conf  
   vi /etc/waagent.conf
   ```
   
   Keresse meg `ResourceDisk.SwapSizeMB` , és módosítsa a hello érték túl**8192**. Szüksége lesz a toopress `insert` tooenter beszúrási módban hello értékének típusa **8192** , és nyomja le az `esc` tooreturn toocommand mód. toowrite hello módosításokat, és kilép hello fájlt, írja be a `:wq` nyomja le az ENTER `enter`.
   
   > [!NOTE]
   > Erősen ajánlott, hogy mindig használjon `WALinuxAgent` tooconfigure lapozófájl, hogy mindig létrejön a hello helyi elmúló (lemez ideiglenes) a legjobb teljesítmény érdekében. További információkért lásd: [hogyan tooadd egy felcserélés Linux Azure virtuális gépek fájlt](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).

## <a name="prepare-your-local-client-and-vm-toorun-x11"></a>A helyi ügyfél és a virtuális gép toorun x11 előkészítése
Grafikus felület toocomplete hello telepítési és konfigurációs Oracle ASM konfigurálása szükséges. Használunk hello x11 protokoll toofacilitate ehhez a telepítéshez. Ha egy ügyfél rendszer (Mac vagy Linux), amely már rendelkezik X11 használ képességek engedélyezve és konfigurálva - kihagyhatja ezt a konfigurációt, és kizárólagos tooWindows gépek beállítása. 

1. [Töltse le a PuTTY](http://www.putty.org/) és [Xming letöltése](https://xming.en.softonic.com/) tooyour Windows rendszerű számítógépen. Mindkét alkalmazást, a folytatás előtt hello alapértelmezett értékekkel hello telepítésének toocomplete kell.

2. PuTTY telepítése után nyisson meg egy parancssort, váltson át hello PuTTY mappát (például, C:\Program Files\PuTTY), és futtassa `puttygen.exe` a rendezés toogenerate egy kulcsot.

3. A PuTTY Megosztottelérésikulcs-készítő:
   
   1. Hozzon létre egy kulcsot hello kiválasztásával `Generate` gombra.
   2. Hello kulcs (Ctrl + C) hello tartalom másolása.
   3. Jelölje be hello `Save private key` gombra.
   4. Egy hozzáférési kóddal hello kulcs védelme hello figyelmeztetést figyelmen kívül, majd válassza ki `OK`.

   ![Képernyőkép a PuTTY Megosztottelérésikulcs-készítő](./media/oracle-asm/puttykeygen.png)

4. A virtuális gépen, futtassa az alábbi parancsokat:

   ```bash
   sudo su - grid
   mkdir .ssh 
   cd .ssh
   ```

5. Hozzon létre egy fájlt `authorized_keys`. Hello kulcs tartalmát hello illessze be ezt a fájlt, és mentse hello fájlt.

   > [!NOTE]
   > hello kulcs tartalmaznia kell a hello karakterlánc `ssh-rsa`. Hello hello kulcs tartalmát is, egy egyszerű szövegsor kell lennie.
   >  

6. Indítsa el a PuTTY az ügyfélrendszeren. A hello **kategória** ablaktáblában lépjen túl**kapcsolat** > **SSH** > **Auth**. A hello **hitelesítéshez titkos kulcsfájl** mezőbe keresse meg a korábban létrehozott toohello kulcs.

   ![Képernyőfelvétel a hello SSH hitelesítés beállításai](./media/oracle-asm/setprivatekey.png)

7. A hello **kategória** ablaktáblában lépjen túl**kapcsolat** > **SSH** > **X11**. Jelölje be hello **engedélyezése X11 továbbítási** jelölőnégyzetet.

   ![Képernyőfelvétel a hello SSH X11 továbbítási beállítások](./media/oracle-asm/enablex11.png)

8. A hello **kategória** ablaktáblában lépjen túl**munkamenet**. Adja meg az Oracle ASM virtuális gép `<publicIPaddress>` hello állomás neve párbeszédpanelen töltse ki az új `Saved Session` nevet, majd kattintson a `Save`.  Ha menteni, kattintson a `open` tooconnect tooyour Oracle ASM virtuális gépet.  hello első csatlakozáskor akkor figyelmeztetést kap hello távoli rendszer nem gyorsítótárazza a beállításjegyzékben. Kattintson a `yes` tooadd, és továbbra is.

   ![Képernyőfelvétel a hello PuTTY munkamenet-beállításokkal](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a>Oracle-rács infrastruktúra telepítése

tooinstall Oracle rács infrastruktúra, teljes hello a következő lépéseket:

1. Jelentkezzen be a **rács**. (A jelszó megadása nélkül képes toosign kell lennie.) 

   > [!NOTE]
   > Ha Windows fut, győződjön meg arról Xming elindította hello telepítés megkezdése előtt.

   ```bash
   cd /opt/grid
   ./runInstaller
   ```

   Megnyílik a Oracle rács infrastruktúra 12c kiadás 1 telepítőjét. (Hello telepítő toostart néhány percig is eltarthat.)

2. A hello **telepítési lehetőség kiválasztása** lapon, hogy melyik **telepítése és konfigurálása Oracle rács infrastruktúra egy önálló kiszolgáló**.

   ![Képernyőfelvétel a hello telepítő telepítési lehetőség kiválasztása lap](./media/oracle-asm/install01.png)

3. A hello **termék nyelvek kiválasztása** oldalon **angol** vagy hello nyelv van kiválasztva.  Kattintson a `next` gombra.

4. A hello **ASM lemez csoport létrehozása** lap:
   - Adjon meg egy nevet hello lemez csoporthoz.
   - A **redundancia**, jelölje be **külső**.
   - A **foglalásiegység-méret**, jelölje be **4**.
   - A **lemezek hozzáadása a**, jelölje be **ORCLASMSP**.
   - Kattintson a `next` gombra.

5. A hello **adja meg a címterület-kezelés jelszó** lapra, jelölje be hello **használja ugyanazt a jelszót a fiókokhoz** lehetőséget, és adjon meg egy jelszót.

   ![Hello telepítő ASM jelszót adjon meg oldalát bemutató képernyőkép](./media/oracle-asm/install04.png)

6. A hello **felügyeleti beállítások megadása** lapon hello beállítás tooconfigure EM felhő vezérlő rendelkezik. Ez a beállítás kihagyjuk - kattintson `next` toocontinue. 

7. A hello **jogosultsággal rendelkező operációs rendszer csoportjának** lapján hello alapértelmezett beállításokat használja. Kattintson a `next` toocontinue.

8. A hello **adja meg a telepítési hely** lapján hello alapértelmezett beállításokat használja. Kattintson a `next` toocontinue.

9. A hello **létrehozása** lapon, módosítsa a készlet Directory hello túl`/u01/app/grid/oraInventory`. Kattintson a `next` toocontinue.

   ![Képernyőfelvétel a hello telepítő létrehozása lap](./media/oracle-asm/install08.png)

10. A hello **legfelső szintű parancsfájl végrehajtásának konfigurációs** lapra, jelölje be hello **automatikusan a parancsfájlokat futtasson** jelölőnégyzetet. Ezután válassza ki a hello **"Gyökér" felhasználó hitelesítő adatainak használata** lehetőséget, és adja meg a hello gyökér szintű felhasználó jelszavát.

    ![Hello telepítő legfelső szintű parancsfájl végrehajtásának konfigurációs oldalát bemutató képernyőkép](./media/oracle-asm/install09.png)

11. A hello **előfeltételek ellenőrzésének végre** lapon hello aktuális beállítása sikertelen lesz, és hibákat. Ez az várt viselkedése. Válassza a(z) `Fix & Check Again` lehetőséget.

12. A hello **javítás parancsfájl** párbeszédpanel, kattintson a `OK`.

13. A hello **összegzés** lapon tekintse át a beállításokat, és kattintson a `Install`.

    ![Képernyőfelvétel a hello telepítő összegző lapja](./media/oracle-asm/install12.png)

14. Egy figyelmeztető párbeszédpanel figyelmezteti konfigurációs parancsfájlokat kell toobe futtatnia szintű jogosultságokkal rendelkező felhasználóként. Kattintson a `Yes` toocontinue.

15. A hello **Befejezés** kattintson `Close` toofinish hello telepítését.

## <a name="set-up-your-oracle-asm-installation"></a>Állítsa be az Oracle ASM telepítése

az Oracle ASM telepítése, a következő lépéseket teljes hello mentése tooset:

1. Győződjön meg arról, hogy továbbra is felhasználóként van bejelentkezve **rács**, a X11 a munkamenet. Előfordulhat, hogy toohit `enter` toorevive hello terminál. Majd indítsa el az Oracle automatikus tárolási felügyeleti konfiguráció Segéd hello:

   ```bash
   cd /u01/app/grid/product/12.1.0/grid/bin
   ./asmca
   ```

   Oracle címterület-kezelési konfigurációs Segéd nyílik meg.

2. A hello **a címterület-kezelés konfigurálása: lemez csoportok** párbeszédpanelen kattintson hello `Create` gombra, majd `Show Advanced Options`.

3. A hello **lemezcsoport létrehozása** párbeszédpanel:

   - Adja meg a csoport Lemeznév hello **adatok**.
   - A **tag lemez kiválasztása**, jelölje be **ORCL_DATA** és **ORCL_DATA1**.
   - A **foglalásiegység-méret**, jelölje be **4**.
   - Kattintson a `ok` toocreate hello lemezcsoport.
   - Kattintson a `ok` tooclose hello ablak.

   ![Képernyőfelvétel a hello lemez csoport létrehozása párbeszédpanel](./media/oracle-asm/asm02.png)

4. A hello **a címterület-kezelés konfigurálása: lemez csoportok** párbeszédpanelen kattintson hello `Create` gombra, majd `Show Advanced Options`.

5. A hello **lemezcsoport létrehozása** párbeszédpanel:

   - Adja meg a csoport Lemeznév hello **FRA**.
   - A **redundancia**, jelölje be **(nincs) külső**.
   - A **tag lemez kiválasztása**, jelölje be **ORCL_FRA**.
   - A **foglalásiegység-méret**, jelölje be **4**.
   - Kattintson a `ok` toocreate hello lemezcsoport.
   - Kattintson a `ok` tooclose hello ablak.

   ![Képernyőfelvétel a hello lemez csoport létrehozása párbeszédpanel](./media/oracle-asm/asm04.png)

6. Válassza ki **kilépési** tooclose címterület-kezelési konfigurációs Segéd.

   ![Képernyőfelvétel a hello konfigurálása ASM: lemez csoportok párbeszédpanel a Kilépés gombra](./media/oracle-asm/asm05.png)

## <a name="create-hello-database"></a>Hello adatbázis létrehozása

hello Azure Piactéri lemezképhez hello Oracle adatbázis-szoftver már telepítve van. toocreate egy adatbázis teljes hello a következő lépéseket:

1. Felhasználók toohello Oracle felügyelő váltson, és majd a naplózás hello figyelő inicializálása:

   ```bash
   su - oracle
   cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
   ./dbca
   ```
   Adatbázis-konfigurációs Segéd nyílik meg.

2. A hello **adatbázis-művelet** kattintson `Create Database`.

3. A hello **létrehozási mód** lap:

   - Adja meg a hello adatbázis nevét.
   - A **tárolótípus**, győződjön meg arról **automatikus tárhely-kezelési (ASM)** van kiválasztva.
   - A **adatbázis helye**, hello alapértelmezett ASM hely javasolt.
   - A **gyors helyreállítás terület**, hello alapértelmezett ASM hely javasolt.
   - Adja meg egy **rendszergazdai jelszó** és **jelszó megerősítése**.
   - Győződjön meg arról `create as container database` van kiválasztva.
   - Írja be a `pluggable database name` érték.

4. A hello **összegzés** lapon tekintse át a beállításokat, és kattintson a `Finish` toocreate hello adatbázis.

   ![Képernyőfelvétel a hello összegző lapja](./media/oracle-asm/createdb03.png)

5. hello adatbázis létrehozását. A hello **Befejezés** oldalon lehetősége van a hello lehetőséget toounlock további fiókokat toouse ezt az adatbázist, és módosíthatja a hello jelszavát. Ha a toodo tette, válassza ki a **jelszókezelés** – ellenkező esetben kattintson a `close`.

## <a name="delete-hello-vm"></a>Hello virtuális gép törlése

Sikeresen konfigurálta hello Oracle DB lemezkép hello Azure piactér Oracle automatikus tárolók kezelése.  Ha már nincs szüksége a virtuális gép, használhatja a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

[Oktatóanyag: Oracle DataGuard konfigurálása](configure-oracle-dataguard.md)

[Oktatóanyag: Oracle GoldenGate konfigurálása](Configure-oracle-golden-gate.md)

Tekintse át [az Oracle DB tervezővel](oracle-design.md)
