<!--author=SharS last changed: 9/17/15-->

#### <a name="to-mount-initialize-and-format-a-volume"></a>Kötet csatlakoztatása, inicializálása és formázása
1. Indítsa el a Microsoft iSCSI-kezdeményezőt.
2. Az **iSCSI-kezdeményező tulajdonságai** ablakban a **Felderítés** lapon kattintson a **Kapu felderítése** elemre.
3. A **Tárolókapu felderítése** párbeszédablakban adja meg az iSCSI-kompatibilis hálózati adaptert, majd kattintson az **OK** gombra. 
4. Az **iSCSI-kezdeményező tulajdonságai** ablakban a **Tárolók** lapon keresse meg a **Felderített tárolók** elemet. Az eszköznek **Inaktív** állapotúnak kell lennie.
5. Válassza ki a céleszközt, majd kattintson a **Csatlakozás** gombra. Az eszköz csatlakoztatása után az állapotnak **Csatlakoztatva** állapotra kell módosulnia. (További információ a Microsoft iSCSI-kezdeményezőről: [A Microsoft iSCSI-kezdeményező telepítése és konfigurálása][1]).
6. A Windows-gazdagépen nyomja le a Windows és az X billentyűt, majd kattintson a **Futtatás** parancsra. 
7. A **Futtatás** párbeszédpanelen írja be a **Diskmgmt.msc** karakterláncot. Kattintson az **OK** gombra, és megjelenik a **Lemezkezelés**párbeszédablak. A jobb oldali panel a gazdagépen lévő köteteket jeleníti meg.
8. A **Lemezkezelés** ablakban a csatlakoztatott kötetek az alábbi ábrán látható módon jelennek meg. Kattintson a jobb gombbal a felderített kötetre (kattintson a lemez nevére), majd kattintson az **Online** elemre.
   
     ![Kötet formázásának inicializálása](./media/storsimple-mount-initialize-format-volume/HCS_InitializeFormatVolume-include.png) 
9. Kattintson a jobb gombbal a kötetre (kattintson a lemez nevére), majd kattintson az **Inicializálás** elemre.
10. Egyszerű kötet formázásához hajtsa végre az alábbi lépéseket:
    
    1. Válassza ki a kötetet, kattintson rá a jobb gombbal (kattintson a jobb oldali területre), majd kattintson az **Új egyszerű kötet** lehetőségre.
    2. Az Új egyszerű kötet varázslóban adja meg a kötet méretét és a meghajtó betűjelét, majd konfigurálja a kötetet NTFS fájlrendszerűként.
    3. Adjon meg 64 KB-os lemezfoglalásiegység-méretet. Ez a lemezfoglalásiegység-méret jól működik a StorSimple megoldásban használt deduplikációs algoritmusokkal.
    4. Hajtson végre egy gyorsformázást.

![Videó elérhető](./media/storsimple-mount-initialize-format-volume/Video_icon.png)**Videó elérhető**

A StorSimple-kötet csatlakoztatását, inicializálását és formázását bemutató videó megtekintéséhez kattintson [ide](https://azure.microsoft.com/documentation/videos/mount-initialize-and-format-a-storsimple-volume/).

<!--Link references-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx
