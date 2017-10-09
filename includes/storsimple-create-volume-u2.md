<!--author=alkohli last changed: 08/16/2016-->

#### <a name="toocreate-a-volume"></a>a kötet toocreate
1. Hello eszközön **gyors üzembe helyezés** kattintson **kötet hozzáadása** toostart hello kötet hozzáadása varázslóban.
2. Hello hozzá egy kötet varázslóban a **alapbeállítások**:
   
   1. Adja meg a kötet **Nevét**.
   2. Hello a legördülő listán válassza ki a hello **használat típusa** a köteten. Helyi garanciákat, kis késleltetést és magasabb teljesítményt igénylő munkaterheléseknél válasszon **Helyileg rögzített** kötetet. Minden más adathoz válasszon **Rétegzett** kötetet. Ha archív adatokhoz használja ezt a kötetet, jelölje be a **Kötet használata ritkábban használt archív adatokhoz** beállítást. 
      
       Egy helyileg rögzített kötet kiosztása, és biztosítja, hogy hello hello kötet elsődleges adatai helyi toohello eszköz marad, és nem kerülnek toohello felhő.  Ha helyileg rögzített kötetet hoz létre, hello eszköz kérdezze le a hello helyi rétegeken elérhető tárhelyet tooprovision hello kötet hello a kért méret. hello működését egy helyileg rögzített kötet létrehozásához magukban foglalhatják kiömlést hello eszköz toohello felhő adatait, és előfordulhat, hogy hosszú idő hello toocreate hello kötet. hello teljes ideje hello kiosztott kötetre, rendelkezésre álló hálózati sávszélességet és az eszközökön hello adatok hello méretétől függ. 
      
       A rétegzett kötetek kiosztása dinamikus, ezért gyorsan létrehozhatók. Kiválasztása **kötet használata ritkábban használt archív adatokhoz** a rétegzett kötetet archív adatokhoz módosítások hello deduplikációs adattömbméret a kötethez tartozó szánt too512 KB. Ha ez a mező nincs bejelölve, a hello megfelelő rétegzett kötet 64 KB adattömbméretet használ. Egy nagyobb deduplikációs adattömbméret lehetővé teszi, hogy a nagy mennyiségű archiválási adatok toohello felhő hello eszköz tooexpedite hello átvitelét.
   3. Adja meg a hello **kiosztott kapacitást** a köteten. Jegyezze fel a rendelkezésre álló hello kapacitás a kiválasztott hello kötettípus alapján. a megadott hello kötet mérete nem haladhatja meg a hello rendelkezésre álló területet.
      
       Megadhat helyileg rögzített kötetekhez mentése too8.5 TB, rétegzett kötetekhez too200 TB hello 8100-as eszközön be. Hello nagyobb 8600 eszköz a helyileg rögzített kötetekhez too22.5 TB mentése vagy mentése too500 TB rétegzett kötetek oszthat. Mivel hello eszközön helyi terület szükséges toohost hello használata a rétegzett kötetek készlete, a helyileg rögzített kötetek létrehozása hatással van hello lemezterület rétegzett kötetek. Ha tehát helyileg rögzített kötetet hoz létre, a rétegzett kötetek létrehozásához rendelkezésre álló terület csökken. Hasonlóképpen ha rétegzett kötetet hoz létre, hello helyileg rögzített kötetek létrehozásához rendelkezésre álló terület csökken.
      
       Ha a 8100-as eszközön helyileg rögzített kötet 8.5 TB-os (megengedett maximális méretet), majd már kipróbálta összes hello helyi szabad lemezterület a hello eszköz. Nem fogja tudni toocreate a rétegzett kötet, és újabb verziók esetében is vannak a pont nincs helyi terület a hello eszköz toohost hello munkakészletének hello többszintű kötet. A meglévő rétegzett kötetek is hatással vannak a hello szabad lemezterület. Ha például egy 8100-as eszközhöz már tartozik körülbelül 106 TB rétegzett kötet, akkor csak 4 TB érhető el a gyors helyi kötetekhez.
      
       hello következő kép bemutatja hello **alapbeállítások** párbeszédpanelt, amely egy helyileg rögzített kötet.
      
        ![Helyi kötet hozzáadása](./media/storsimple-create-volume-u2/add-local-volume-include.png)
      
       hello következő kép bemutatja hello **alapbeállítások** egy rétegzett kötet párbeszédpanel.
      
        ![Helyi kötet hozzáadása](./media/storsimple-create-volume-u2/add-tiered-volume-include.png)
   
   1. Hello nyíl ikonra ![nyíl ikon](./media/storsimple-create-volume-u2/HCS_ArrowIcon-include.png) toogo toohello következő oldalra.
3. A hello **további beállításokat** párbeszédpanelen adja hozzá egy új hozzáférés-vezérlési rekordot (ACR):
   
   1. Adja meg az ACR **nevét**.
   2. A **iSCSI-kezdeményező neve**, adja meg a hello iSCSI minősített nevét (IQN) Windows-gazdagépen. Ha nem rendelkezik hello IQN, nyissa meg túl[Get hello egy Windows Server-állomás IQN-Nevének](#get-the-iqn-of-a-windows-server-host).
   3. A **alapértelmezett biztonsági mentés ehhez a kötethez?**, jelölje be hello **engedélyezése** jelölőnégyzetet. hello alapértelmezett biztonsági mentés létrehoz egy házirendet, amely mindennap 22:30-kor (az eszköz ideje), és létrehoz egy felhő-pillanatfelvételt a kötetről.
      
      > [!NOTE]
      > Hello biztonsági mentés itt engedélyezése után nem lehet visszaállítani. Tooedit hello kötet toomodify kell ezt a beállítást.
      > 
      > 
      
      ![Kötet hozzáadása](./media/storsimple-create-volume-u2/AddVolumeAdditionalSettings1.png)
4. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-create-volume-u2/HCS_CheckIcon-include.png). A kötet jön létre a megadott hello beállításait.

