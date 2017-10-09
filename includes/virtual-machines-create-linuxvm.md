
1. Jelentkezzen be tooyour hello lépésekkel felsorolt Azure-előfizetés [tooAzure csatlakoztatja a hello Azure CLI 1.0](../articles/xplat-cli-connect.md).

2. Gondoskodjon arról, hogy Ön hello klasszikus telepítési módban az alábbiak szerint:

    ```azurecli
    azure config mode asm
    ```

3. Megtudhatja, hello Linux lemezképet, amelyet a rendelkezésre álló lemezképeinek hello tooload az alábbiak szerint:

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    Windows-parancsablakokban a grep helyett használja a **find** kifejezést.
   
4. Használjon `azure vm create` toocreate hello Linux lemezképpel hello előző listában egy virtuális Gépet. Ezzel a lépéssel létrehoz egy felhőszolgáltatást és egy tárfiókot. A virtuális gép tooan meglévő felhőszolgáltatás is kapcsolódhat egy `-c` lehetőséget. Hozzon létre egy SSH-végpont toolog toohello Linux virtuális gép hello `-e` lehetőséget. hello alábbi példa létrehoz egy nevű virtuális gép `myVM` hello segítségével `Ubuntu-14_04_4-LTS` hello lemezképet `West US` helyét, és hozzáad egy felhasználói nevet `ops`:
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    hello hasonló toohello a következő példa a kimenetre:

    ```azurecli
    info:    Executing command vm create
    + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB
    + Looking up cloud service
    info:    cloud service myVM not found.
    + Creating cloud service
    + Retrieving storage accounts
    + Creating VM
    info:    vm create command OK
    ```
   
   > [!NOTE]
   > A Linux virtuális gép esetén meg kell adnia hello `-e` beállítást `vm create`. Nincs SSH lehetséges tooenable hello virtuális gép létrehozása után. Az SSH további tudnivalókért olvassa el [hogyan tooUse Azure Linux SSH](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

5. Hello segítségével ellenőrizheti a hello VM hello attribútumait `azure vm show` parancsot. hello alábbi példa felsorolja hello nevű virtuális gép adatai `myVM`:

    ```azurecli   
    azure vm show myVM
    ```

6. Indítsa el a virtuális Gépet a hello `azure vm start` parancsot a következőképpen:

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a>Következő lépések
Ezek Azure CLI 1.0 virtuális gép parancsok tudnivalókért olvassa el a hello [Using hello Azure CLI 1.0 hello klasszikus telepítés API](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

