<!--author=alkohli last changed: 07/19/2017-->

#### <a name="toocreate-a-volume"></a>a kötet toocreate
1. A hello táblázatos felsorolása hello hello eszközök **eszközök** panelen válassza ki azt az eszközt. Kattintson a **+ Kötet hozzáadása** gombra.

    ![Új kötet hozzáadása](./media/storsimple-8000-create-volume-u2/step5createvol1.png)

2. A hello **kötet hozzáadása** panel:
   
   1. Hello **kiválasztására szolgáló** mező automatikusan feltöltődik értékkel az aktuális eszközzel.

   2. Hello legördülő listájából válassza ki egy kötetet tooadd kell hello kötettároló. 

   3.  Adja meg a kötet **Nevét**. A kötet nem nevezhető át, hello kötet létrehozása után.

   4. Hello a legördülő listán válassza ki a hello **típus** a köteten. Helyi garanciákat, kis késleltetést és magasabb teljesítményt igénylő munkaterheléseknél válasszon **Helyileg rögzített** kötetet. Minden más adathoz válasszon **Rétegzett** kötetet. Ha archív adatokhoz használja ezt a kötetet, jelölje be a **Kötet használata ritkábban használt archív adatokhoz** beállítást.
      
       A rétegzett kötetek kiosztása dinamikus, ezért gyorsan létrehozhatók. Kiválasztása **kötet használata ritkábban használt archív adatokhoz** a rétegzett kötetet archív adatokhoz módosítások hello deduplikációs adattömbméret a kötethez tartozó szánt too512 KB. Ha ez a mező nincs bejelölve, a hello megfelelő rétegzett kötet 64 KB adattömbméretet használ. Egy nagyobb deduplikációs adattömbméret lehetővé teszi, hogy a nagy mennyiségű archiválási adatok toohello felhő hello eszköz tooexpedite hello átvitelét.
       
       Egy helyileg rögzített kötet kiosztása, és biztosítja, hogy hello hello kötet elsődleges adatai helyi toohello eszköz marad, és nem kerülnek toohello felhő.  Ha helyileg rögzített kötetet hoz létre, hello eszköz kérdezze le a hello helyi rétegeken elérhető tárhelyet tooprovision hello kötet hello a kért méret. hello működését egy helyileg rögzített kötet létrehozásához magukban foglalhatják kiömlést hello eszköz toohello felhő adatait, és előfordulhat, hogy hosszú idő hello toocreate hello kötet. hello teljes ideje hello kiosztott kötetre, rendelkezésre álló hálózati sávszélességet és az eszközökön hello adatok hello méretétől függ.

   5. Adja meg a hello **kiosztott kapacitást** a köteten. Jegyezze fel a rendelkezésre álló hello kapacitás a kiválasztott hello kötettípus alapján. a megadott hello kötet mérete nem haladhatja meg a hello rendelkezésre álló területet.
      
       Megadhat helyileg rögzített kötetekhez mentése too8.5 TB, rétegzett kötetekhez too200 TB hello 8100-as eszközön be. Hello nagyobb 8600 eszköz a helyileg rögzített kötetekhez too22.5 TB mentése vagy mentése too500 TB rétegzett kötetek oszthat. Mivel hello eszközön helyi terület szükséges toohost hello használata a rétegzett kötetek készlete, a helyileg rögzített kötetek létrehozása hatással van hello lemezterület rétegzett kötetek. Ha tehát helyileg rögzített kötetet hoz létre, a rétegzett kötetek létrehozásához rendelkezésre álló terület lecsökken. Hasonlóképpen ha rétegzett kötetet hoz létre, hello helyileg rögzített kötetek létrehozásához rendelkezésre álló terület csökken.
      
       Ha a 8100-as eszközön helyileg rögzített kötet 8.5 TB-os (megengedett maximális méretet), majd már kipróbálta összes hello helyi szabad lemezterület a hello eszköz. Nem hozható létre további rétegzett köteteket ettől kezdve, mivel nincs helyi terület hello eszköz toohost hello munkakészletének hello rétegzett kötet. A meglévő rétegzett kötetek is hatással vannak a hello szabad lemezterület. Ha például egy 8100-as eszközhöz már tartozik körülbelül 106 TB rétegzett kötet, akkor csak 4 TB érhető el a gyors helyi kötetekhez.

    6. A hello **csatlakozó állomások** , majd kattintson a hello nyílra. 

        ![Csatlakoztatott gazdagépek](./media/storsimple-8000-create-volume-u2/step5createvol2.png)

    7. A hello **csatlakozó állomások** panelen válassza ki egy meglévő ACR, vagy adja hozzá egy új ACR hello lépések végrehajtásával:

       1. Adja meg az ACR **nevét**.
       2. A **iSCSI-kezdeményező neve**, adja meg a hello iSCSI minősített nevét (IQN) Windows-gazdagépen. Ha nem rendelkezik hello IQN, nyissa meg túl[Get hello egy Windows Server-állomás IQN-Nevének](#get-the-iqn-of-a-windows-server-host).

    9. Kattintson a **Create** (Létrehozás) gombra. A kötet jön létre a megadott hello beállításait.

        ![Kattintson a Létrehozás gombra](./media/storsimple-8000-create-volume-u2/step5createvol3.png)

        > [!NOTE]
        > Vegye figyelembe, hogy az itt létrehozott hello kötet nem védett. A kötet tootake ütemezett biztonsági toocreate és társítása biztonsági mentési házirendek lesz szüksége. 

