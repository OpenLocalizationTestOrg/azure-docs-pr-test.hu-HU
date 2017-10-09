
További információ a lemezekkel kapcsolatban: [A lemezek és virtuális merevlemezek ismertetése](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

<a id="attachempty"></a>

## <a name="attach-an-empty-disk"></a>Üres lemez csatlakoztatása
1. Nyissa meg az Azure CLI 1.0 és [csatlakozás Azure-előfizetés tooyour](../articles/xplat-cli-connect.md). Bizonyosodjon meg róla, Azure szolgáltatásfelügyelet módban van-e (`azure config mode asm`).
2. Adja meg `azure vm disk attach-new` toocreate és egy új lemezt csatolni a hello a következő példában látható módon. Cserélje le *myVM* hello nevű a Linux virtuális gép, és adja meg a hello lemez hello méretét GB, ami *100GB* ebben a példában:

    ```azurecli
    azure vm disk attach-new myVM 100
    ```

3. Hello adatlemez létrehozni és csatolni, után hello kimenete felsorolt `azure vm disk list <virtual-machine-name>` a hello a következő példában látható módon:
   
    ```azurecli
    azure vm disk list TestVM
    ```

    hello hasonló toohello a következő példa a kimenetre:

    ```bash
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        myVM-2645b8030676c8f8.vhd  Linux
     data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

<a id="attachexisting"></a>

## <a name="attach-an-existing-disk"></a>Meglévő lemez csatlakoztatása
Meglévő lemez csatlakoztatása esetén rendelkeznie kell egy tárfiókban elérhető .vhd-vel.

1. Nyissa meg az Azure CLI 1.0 és [csatlakozás Azure-előfizetés tooyour](../articles/xplat-cli-connect.md). Bizonyosodjon meg róla, Azure szolgáltatásfelügyelet módban van-e (`azure config mode asm`).
2. Ellenőrizze, hogy ha hello VHD kívánt tooattach már feltöltött tooyour Azure-előfizetés:
   
    ```azurecli
    azure vm disk list
    ```

    hello hasonló toohello a következő példa a kimenetre:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
     data:    Name                                          OS
     data:    --------------------------------------------  -----
     data:    myTestVhd                                     Linux
     data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
     data:    TestVM-ubuntuVMasm-0-201508060040530369
     info:    vm disk list command OK
    ```

3. Ha nem találja a hello lemez adott meg toouse, előfordulhat, hogy töltse fel a helyi virtuális merevlemez tooyour előfizetés használatával `azure vm disk create` vagy `azure vm disk upload`. Példa `disk create` lenne, mint például a következő hello:
   
    ```azurecli
    azure vm disk create myVhd .\TempDisk\test.VHD -l "East US" -o Linux
    ```

    hello hasonló toohello a következő példa a kimenetre:

    ```azurecli
    info:    Executing command vm disk create
    + Retrieving storage accounts
    info:    VHD size : 10 GB
    info:    Uploading 10485760.5 KB
    Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
    info:    Finishing computing MD5 hash, 16% is complete.
    info:    https://mystorageaccount.blob.core.windows.net/disks/myVHD.vhd was
    uploaded successfully
    info:    vm disk create command OK
    ```
   
   Is használhatja `azure vm disk upload` tooupload egy virtuális merevlemez tooa bizonyos tárolási fiókot. Olvasási hello kapcsolatos parancsok toomanage az Azure virtuális gép adatlemezek [keresztül Itt](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

4. Most csatlakoztatása hello szükséges VHD tooyour virtuális gépet:
   
    ```azurecli
    azure vm disk attach myVM myVhd
    ```
   
   Győződjön meg arról, hogy tooreplace *myVM* hello nevet, a virtuális gépet, és *myVHD* rendelkező a kívánt virtuális Merevlemezt.

5. Ellenőrizheti hello lemez virtuális géphez csatolt toohello `azure vm disk list <virtual-machine-name>`:
   
    ```azurecli
    azure vm disk list myVM
    ```

    hello hasonló toohello a következő példa a kimenetre:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    1    10        test.VHD
     data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

> [!NOTE]
> Adatlemez hozzáadása után kell toolog toohello virtuális gépen kell és hello lemez inicializálása, hello virtuális gépek által használható hello lemez tárolás (lásd a következő hello lépéseit további információt hogyan toodo inicializálása hello lemez).
> 
> 

