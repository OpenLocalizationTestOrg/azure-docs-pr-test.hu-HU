<!--author=SharS last changed: 9/17/15-->

#### <a name="toomount-initialize-and-format-a-volume"></a>toomount, inicializálja és formázza a kötetet
1. Indítsa el a hello Microsoft iSCSI-kezdeményező.
2. A hello **iSCSI-kezdeményező tulajdonságai** ablakban a hello **felderítési** lapra, majd **kapu felderítése**.
3. A hello **tárolókapu felderítése** párbeszédpanelen adja meg az iSCSI-kompatibilis hálózati adaptert hello IP-címét, és kattintson **OK**. 
4. A hello **iSCSI-kezdeményező tulajdonságai** ablakban a hello **célok** lapján keresse meg a hello **Felderített tárolók**. hello eszköz állapotúnak kell lennie **inaktív**.
5. Válasszon hello céleszközt, majd kattintson **Connect**. Hello eszköz csatlakoztatása után hello kell állapotmódosítási túl**csatlakoztatva**. (Hello Microsoft iSCSI-kezdeményező használatával kapcsolatos további információkért lásd: [telepítése és konfigurálása a Microsoft iSCSI-kezdeményező][1]).
6. A Windows-gazdagépen, nyomja meg a hello Windows billentyű + X, és kattintson **futtatása**. 
7. A hello **futtatása** párbeszédpanelen írja be **Diskmgmt.msc**. Kattintson a **OK**, és hello **Lemezkezelés** párbeszédpanel jelenik meg. hello jobb oldali ablaktáblán jelennek meg hello kötetek a gazdagépen.
8. A hello **Lemezkezelés** ablakban hello csatlakoztatott kötetek fog megjelenni a hello a következő ábrán látható módon. Kattintson a jobb gombbal a hello felderített kötetre (kattintson a hello lemez nevére), és kattintson a **Online**.
   
     ![Kötet formázásának inicializálása](./media/storsimple-mount-initialize-format-volume/HCS_InitializeFormatVolume-include.png) 
9. Kattintson a jobb gombbal a hello kötetre (kattintson a hello lemez nevére) újra, és kattintson a **inicializálása**.
10. tooformat egyszerű kötet, hajtsa végre a következő lépéseket hello:
    
    1. Hello kötet kiválasztásához kattintson a jobb gombbal az (kattintson a jobb oldali területre hello), és kattintson a **új egyszerű kötet**.
    2. Hello új egyszerű kötet varázslóban hello kötet méretét és a meghajtó betűjelét adja meg, és az NTFS fájlrendszer hello kötet konfigurálása.
    3. Adjon meg 64 KB-os lemezfoglalásiegység-méretet. A foglalásiegység-méret jól működik hello StorSimple megoldásban használt hello deduplikációs algoritmusokkal.
    4. Hajtson végre egy gyorsformázást.

![Videó elérhető](./media/storsimple-mount-initialize-format-volume/Video_icon.png)**Videó elérhető**

a videó bemutatja, hogyan toomount, inicializálása és formátum a StorSimple-kötet, kattintson az toowatch [Itt](https://azure.microsoft.com/documentation/videos/mount-initialize-and-format-a-storsimple-volume/).

<!--Link references-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx
