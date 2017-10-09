Ha már nincs szüksége, amely csatolt tooa virtuális gép (VM) adatlemezt, könnyen leválasztás. Ha Ön a lemezt leválasztani a virtuális gép hello, hello lemez nem raktárból azt. Ha újra toouse hello meglévő hello a lemezen lévő adatokat, akkor is csatlakoztassa újra toohello azonos virtuális gép vagy egy másikra.  

> [!NOTE]
> Az Azure-ban a virtuális gépek különböző lemeztípusokat használnak – operációsrendszer-lemezt, helyi ideiglenes lemezt és opcionális adatlemezeket. További információ: [A lemezek és virtuális merevlemezek ismertetése](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Operációsrendszer-lemez nem választható le, kivéve, ha a virtuális gép hello is törli.

## <a name="find-hello-disk"></a>Hello lemez található
Akkor is a lemezt leválasztani a virtuális gép kell toofind kimenő hello logikai egység száma, amely hello lemez toobe leválasztott azonosítója. az toodo, kövesse az alábbi lépéseket:

1. Nyissa meg az Azure CLI és [csatlakozás Azure-előfizetés tooyour](../articles/xplat-cli-connect.md). Bizonyosodjon meg róla, Azure szolgáltatásfelügyelet módban van-e (`azure config mode asm`).
2. Annak meghatározása, hogy mely lemezek csatolt tooyour VM-et hello alábbi példa felsorolja hello nevű virtuális gép lemezeinek `myVM`:

    ```azurecli
    azure vm disk list myVM
    ```

    hello hasonló toohello a következő példa a kimenetre:

    ```azurecli
    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
      data:    Lun  Size(GB)  Blob-Name                         OS
      data:    ---  --------  --------------------------------  -----
      data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
      data:    0    30        myDataDisk.vhd
      info:    vm disk list command OK
    ```

3. Vegye figyelembe a logikai egység hello vagy hello **logikaiegység-szám** , amelyet az toodetach hello lemezhez.

## <a name="remove-operating-system-references-toohello-disk"></a>Operációs rendszer hivatkozások toohello lemez eltávolítása
Mielőtt hello lemez leválasztása a hello Linux Vendég, meg kell győződnie arról, hogy minden partíció hello lemezen nincsenek használatban. Győződjön meg arról, hogy hello operációs rendszer nem próbálkozik tooremount őket a rendszer újraindítása után. Ezeket a lépéseket a visszavonás vélhetőleg létrehozta azon mikor hello konfigurációs [csatolása](../articles/virtual-machines/linux/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) hello lemez.

1. Használjon hello `lsscsi` parancs toodiscover hello lemez azonosítója. Az `lsscsi` a `yum install lsscsi` (Red Hat-alapú disztribúciókon) vagy az `apt-get install lsscsi` (Debian-alapú disztribúciókon) használatával telepíthető. Hello LUN számot a keresett hello lemezazonosítót található. hello utolsó hello rekordot sorokban értéke hello logikai Egységet. Az alábbi példa hello `lsscsi`, LUN 0 leképezhető túl  */dev/sdc*

    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```

2. Használjon `fdisk -l <disk>` hello lemez toobe leválasztott társított toodiscover hello partíciókat. hello következő példa bemutatja a hello kimeneti `/dev/sdc`:

    ```bash
    Disk /dev/sdc: 1098.4 GB, 1098437885952 bytes, 2145386496 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x5a1d2a1a
    
        Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048  2145386495  1072692224   83  Linux
    ```

3. Mindegyik partíció felsorolt hello lemez leválasztása. hello alábbi példa leválasztja `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

4. Használjon hello `blkid` parancs toodiscovery hello UUID-k minden partíció esetében. hello hasonló toohello a következő példa a kimenetre:

    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

5. Távolítsa el a bejegyzéseket hello **/etc/fstab** tartozó hello eszköz elérési utak vagy UUID-k hello lemez toobe leválasztott tartozó összes partíció fájlt.  Ebben a példában a bejegyzések a következők lehetnek:

    ```sh  
   UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
   ```

    vagy
   
   ```sh   
   /dev/sdc1   /datadrive   ext4   defaults   1   2
   ```

## <a name="detach-hello-disk"></a>Hello lemez leválasztása
Hello logikai egység száma hello lemez és eltávolított hello operációs rendszer hivatkozások megtalálta most készen áll a toodetach azt:

1. Hello kijelölt lemezt leválasztani a virtuális gép hello hello parancs futtatásával `azure vm disk detach
   <virtual-machine-name> <LUN>`. hello alábbi példa leválasztja LUN `0` hello nevű virtuális gép a `myVM`:
   
    ```azurecli
    azure vm disk detach myVM 0
    ```

2. Ha hello lemez leválasztott állapotban a kapott a következő futtatásával ellenőrizheti `azure vm disk list` újra. a következő példa ellenőrzések hello hello nevű virtuális gép `myVM`:
   
    ```azurecli
    azure vm disk list myVM
    ```

    a kimeneti hello van a hasonló toohello következő példának, amely bemutatja a hello adatok lemez már csatlakoztatva van:

    ```azurecli
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
     info:    vm disk list command OK
    ```

lemez leválasztása hello tárolási megmarad, de már nem csatlakoztatott tooa virtuális gépet.

