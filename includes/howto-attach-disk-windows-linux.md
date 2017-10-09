


## <a name="attach-an-empty-disk"></a>Üres lemez csatlakoztatása
Üres lemez csatolása egy egyszerű módon tooadd az adatok lemezre, mivel Azure hello .vhd fájl létrehozza, és azt hello tárfiók tárolja.

1. Kattintson a **virtuális gépek (klasszikus)**, és ezután válasszon hello megfelelő méretű.

2. Hello-beállítások menüjében kattintson **lemezek**.

   ![Egy új üres lemez csatolása](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. A hello parancssávon kattintson **új Attach**.  
    Hello **új lemez csatolása** párbeszédpanel jelenik meg.

    ![Új lemez csatolása](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    Adja meg a következő információ hello:
    - A **Fájlnév**fogadja el a hello alapértelmezett nevet, vagy írja be a hello .vhd fájl egy másikat. hello adatlemez egy automatikusan létrehozott nevet használja, akkor is, ha beírja hello .vhd fájl egy másik nevet.
    - Jelölje be hello **típus** hello adatok lemez. Az összes virtuális gép támogatja a normál lemezeket. Több virtuális gép is támogatja a premium lemezek.
    - Jelölje be hello **méret (GB)** hello adatok lemez.
    - A **állomás-gyorsítótárazása**, válasszon none, vagy csak olvasható.
    - Kattintson az OK toofinish.

4. Miután hello adatlemez létrehozni és csatolni, hello VM hello lemezek szakaszában szerepel.

   ![Sikeresen csatolta az új és üres adatlemez](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> Adatlemez hozzáadása után a virtuális gép toohello toolog szükségesek, és hello lemez inicializálása, hogy használatba vehető.

## <a name="how-to-attach-an-existing-disk"></a>Hogyan: meglévő lemez csatolása
Meglévő lemez csatlakoztatása esetén rendelkeznie kell egy tárfiókban elérhető .vhd-vel. Használjon hello [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) parancsmag tooupload hello .vhd fájl toohello tárfiók. Miután létrehozott és hello .vhd fájl feltöltése, csatolhatók tooa virtuális gép.

1. Kattintson a **virtuális gépek (klasszikus)**, és ezután hello az válassza ki a megfelelő virtuális gép.

2. Hello-beállítások menüjében kattintson **lemezek**.

3. A hello parancssávon kattintson **Csatolás meglévő**.

    ![Adatlemez csatolása](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. Kattintson a **hely**. hello rendelkezésre álló tár fiókokat jeleníti meg. Ezután válassza ki a megfelelő tárolási fiók felsorolt.

    ![Adja meg a storage-fiók](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. A **tárfiók** rendelkezik egy vagy több olyan tárolók, amelyek tartalmazzák a meghajtók (VHD-k). Válassza ki a megfelelő tárolót hello felsorolt.

    ![Adja meg a virtuális gépek windows tároló](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. Hello **VHD-k** panel hello tárolóban tárolt hello lemezmeghajtók sorolja fel. Kattintson hello lemezek közül, majd válassza ki.

    ![Adja meg a lemezképet a virtuális gépek windows](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. Hello **meglévő lemez csatolása** panel újra, hello helyen lévő hello tárfiókot, a tároló és a kijelölt merevlemez (vhd) tooadd toohello virtuális gépet tartalmazó megjeleníti.

  Állítsa be **állomás-gyorsítótárazása** toonone vagy olvasási csak, majd kattintson az OK gombra.

    ![Sikeresen csatolta adatlemez](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
