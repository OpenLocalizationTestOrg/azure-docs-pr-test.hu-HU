Ha a virtuális gép (VM) az Azure-ban rendszerindító vagy a lemez hibát tapasztal, szükség lehet a tooperform hibaelhárítási hello virtuális merevlemezen magát. Ilyenek például a sikertelen frissítés, amely megakadályozza, hogy a virtuális gép hello sikeresen indítását lenne. Ez a cikk ismerteti, hogyan toouse az Azure portál tooconnect a virtuális merevlemez tooanother VM toofix ki a hibákat, majd hozza újra létre az eredeti virtuális gép.

## <a name="recovery-process-overview"></a>Helyreállítási folyamat áttekintése
hibaelhárítási folyamatának hello a következőképpen történik:

1. Törölje a virtuális Gépet, amely kapcsolatban felmerült problémák hello, de megőrizni hello virtuális merevlemezeket.
2. Csatolja, és a csatlakoztatási hello virtuális merevlemez tooanother VM hibaelhárításhoz.
3. Csatlakoztassa a VM hibaelhárítási toohello. Szerkesztheti a fájlokat, vagy futtassa a eszközöket toofix hibák hello eredeti virtuális merevlemez.
4. Válassza le a lemezképet, és válassza le hello virtuális merevlemezt a virtuális gép hibakeresési hello.
5. Hozzon létre egy virtuális Gépet hello eredeti virtuális merevlemez használatával.

## <a name="delete-hello-original-vm"></a>Törlés hello az eredeti virtuális gép
A virtuális merevlemezek és a virtuális gépek az Azure-erőforrások két különböző típusa. A virtuális merevlemez hello operációs rendszer, alkalmazások és konfigurációk tárolására. virtuális gép hello, amely meghatározza, hogy hello vagy a hely és az erőforrások, például egy virtuális merevlemezt vagy virtuális hálózati kártya (NIC) hivatkozik, amely csak metaadatokat. Mindegyik virtuális merevlemezt lekérdezi a címbérlet létrehozásakor kell, hogy a lemez csatlakoztatott tooa virtuális gép. Bár adatlemezt csatolni, és leválasztott hello virtuális gép futása közben is, hello operációsrendszer-lemez nem választható le, kivéve, ha a virtuális gép erőforrásához hello törlődik. hello bérleti tooassociate hello OS lemez tooa virtuális gép továbbra is, akkor is, ha ezt a virtuális Gépet felszabadított és leállított állapotban van.

első lépés toorecovering hello a virtuális gép toodelete hello VM erőforrás magát. Virtuális gép törlése hello hello virtuális merevlemezek elhagyja a tárfiókban lévő. Hello a virtuális gép törlődik, miután hello virtuális merevlemez tooanother VM tootroubleshoot csatolja, és hello hibák. 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com). 
2. A hello hello bal oldali menüben kattintson **virtuális gépek (klasszikus)**.
3. Jelölje be hello virtuális Gépet, amely hello problémát tapasztal, kattintson a **lemezek**, majd keresse meg hello hello virtuális merevlemez nevét. 
4. Válassza ki az operációs rendszer hello virtuális merevlemez, és ellenőrizze a hello **hely** tooidentify hello tárfiókot, amely tartalmazza a virtuális merevlemezt. A következő példa hello, hello karakterlánc előtt közvetlenül ". blob.core.windows.net" hello tárfiók neve van.

    ```
    https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd
    ```

    ![a virtuális gép helyével kapcsolatos hello kép](./media/virtual-machines-classic-recovery-disks-portal/vm-location.png)

5. Kattintson a jobb gombbal a virtuális gép hello, és válassza ki **törlése**. Győződjön meg arról, hogy hello lemezek nem választott hello virtuális gép törlésekor.
6. Hozzon létre egy új helyreállítási virtuális gépet. A virtuális gép lehet hello azonos régióban és erőforrás csoportban (Felhőszolgáltatás) hello probléma virtuális gép.
7. Hello helyreállítási virtuális Gépet, majd válassza ki és **lemezek** > **meglévő csatolása**.
8. tooselect a meglévő virtuális merevlemez kattintson **VHD-fájl**:

    ![Meglévő VHD keresése](./media/virtual-machines-classic-recovery-disks-portal/select-vhd-location.png)

9. Válassza ki a tárfiók hello > VHD-tároló > hello virtuális merevlemez, kattintson a hello **válasszon** tooconfirm gomb szabadon választott.

    ![Meglévő VHD kiválasztása](./media/virtual-machines-classic-recovery-disks-portal/select-vhd.png)

10. Válassza ki a most kijelölt virtuális merevlemez a **OK** tooattach hello létező virtuális merevlemezt.
11. Néhány másodperc múlva hello **lemezek** a virtuális gép ablaktábla a meglévő virtuális merevlemez csatlakoztatva adatlemezt számára:

    ![Adatlemezként csatlakoztatott meglévő virtuális merevlemez](./media/virtual-machines-classic-recovery-disks-portal/attached-disk.png)

## <a name="fix-issues-on-hello-original-virtual-hard-disk"></a>Hello eredeti virtuális merevlemez kapcsolatos problémák megoldása
Ha hello meglévő virtuálismerevlemez-fájl csatlakoztatva van, minden karbantartási és hibaelhárítási lépéseket, igény szerint most elvégezhet. Egyszer foglalkoztak hello problémák, folytassa a következő lépéseket hello.

## <a name="unmount-and-detach-hello-original-virtual-hard-disk"></a>Válassza le a lemezképet, és válassza le hello eredeti virtuális merevlemezt
Amennyiben az esetleges hibákat fakadó problémák megoldásával leválasztása, és válassza le hello meglévő virtuális merevlemezt a hibaelhárítási virtuális gépről. A virtuális merevlemez más virtuális gép együtt nem használható, amíg hello bérleti hello virtuális merevlemez toohello VM hibaelhárítási csatlakozó.  

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com). 
2. A hello hello bal oldali menüben válassza **virtuális gépek (klasszikus)**.
3. Keresse meg a hello helyreállítási virtuális Gépet. Válassza ki a lemezt, kattintson a jobb gombbal hello lemezre, majd **leválasztási**.

## <a name="create-a-vm-from-hello-original-hard-disk"></a>Hozzon létre egy virtuális Gépet hello eredeti merevlemez

toocreate egy virtuális Gépet az eredeti virtuális merevlemez használata [a klasszikus Azure portálon](https://manage.windowsazure.com).

1. Jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com).
2. Hello hello portál alsó részén, jelölje ki a **új** > **számítási** > **virtuális gép** > **a gyűjteménye** .
3. A hello **kép kiválasztása** szakaszban jelölje be **a lemezek**, és ezután válasszon hello eredeti virtuális merevlemezt. Ellenőrizze a hello helyére vonatkozó információkat. Ez a hello régió, ahol hello VM kell telepíteni. Válassza ki a hello Tovább gombra.
4. A hello **virtuálisgép-konfiguráció** szakaszt, írja be a nevet a virtuális gép hello és hello VM kiválasztásához.
