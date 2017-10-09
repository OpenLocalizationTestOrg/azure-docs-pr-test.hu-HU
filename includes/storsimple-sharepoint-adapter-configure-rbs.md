<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> Módosítások toohello StorSimple Adapter SharePoint RBS konfiguráció meghozásakor akkor kell bejelentkeznie toohello tartományi rendszergazdák csoportba tartozó felhasználói fiókkal. Emellett ugyanaz, mint a központi adminisztrációs gazdagép hello futó böngészővel hello konfigurálása lapon kell elérnie.
> 
> 

#### <a name="tooconfigure-rbs"></a>tooconfigure RBS
1. Nyissa meg hello SharePoint központi felügyelet lapját, és keresse meg a túl**rendszerbeállítások**. 
2. A hello **Azure StorSimple** kattintson **konfigurálása a StorSimple-Adapter**.
   
    ![StorSimple Adapter hello konfigurálása](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. A hello **konfigurálása a StorSimple-Adapter** lap:
   
   1. Győződjön meg arról, hogy hello **szerkesztési útvonal engedélyezéséhez** jelölőnégyzet be van jelölve.
   2. Hello mezőbe írja be a hello univerzális elnevezési konvenció (UNC) szerinti elérési útját hello BLOB-tároló.
      
      > [!NOTE]
      > hello BLOB tároló kötet hello StorSimple eszközön konfigurált iSCSI-köteten kell elhelyezni.

   3. Kattintson a hello **engedélyezése** egyes hello tartalom-adatbázisokra, amelyet az tooconfigure távoli tárolás gombra.
      
      > [!NOTE]
      > hello BLOB-tárolónak kell lennie megosztott és elérhető-e az összes webes előtér-(WFE) kiszolgáló által, és hello felhasználói fiókot, amelyet a SharePoint-kiszolgálófarm hello beállított hozzáférési toohello megosztást kell rendelkeznie.
      
      ![Hello RBS szolgáltató engedélyezése](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      Engedélyezése vagy letiltása RBS, akkor a következő üzenet hello is látható.
      
      ![Konfigurálja a StorSimple Adapter engedélyezése letiltása](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. Kattintson a hello **frissítés** gombok tooapply hello beállítása. Amikor rákattint hello **frissítés** gomb, hello RBS konfiguráció állapota frissülni fog minden WFE-kiszolgálón, és a farm egésze hello lesz, RBS engedélyezve van. hello a következő üzenet jelenik meg.
      
      ![Csatoló konfigurációs üzenet](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > RBS nagyon sok (több mint 200) adatbázisok SharePoint-farm beállításakor, hello SharePoint központi felügyelet weblap előfordulhat, hogy túllépi az időkorlátot. Ha ez történik, frissítse a hello lapot. Ez nem befolyásolja a hello konfigurációs folyamatát.

4. Hello konfigurációjának ellenőrzése:
   
   1. Jelentkezzen be a SharePoint központi felügyeleti webhely toohello, és keresse meg a toohello **konfigurálása a StorSimple-Adapter** lap.
   2. Ellenőrizze a hello konfigurációs részletek toomake meg arról, hogy ezek megegyeznek-e a megadott hello-beállítások. 
5. Győződjön meg arról, hogy RBS megfelelően működik-e:
   
   1. Töltse fel a dokumentum tooSharePoint. 
   2. Keresse meg a konfigurált toohello UNC elérési utat. Győződjön meg arról, hogy létrejött-e hello RBS könyvtárszerkezetét és feltöltött hello objektumot tartalmaz.
6. (Választható) Használhatja a Microsoft RBS hello `Migrate()` részét képező SharePoint toomigrate meglévő BLOB tartalom toohello StorSimple eszköz PowerShell-parancsmagot. További információkért lásd: [tartalmat át a virtuális gépbe vagy onnan a SharePoint 2013 RBS] [ 6] vagy [tartalmat át a virtuális gépbe vagy onnan RBS (SharePoint Foundation 2010)] [7].
7. (Választható) A teszt telepítésekre ellenőrizheti, hogy hello Blobok került kívül hello tartalom-adatbázist az alábbiak szerint: 
   
   1. Indítsa el az SQL Management Studiót.
   2. Hello ListBlobsInDB_2010.sql vagy ListBlobsInDB_2013.sql lekérdezés, az alábbiak szerint futtatni.
      
      ```
      **ListBlobsInDB_2013.sql**
      
        USE WSS_Content
        GO
      
        SELECT DocStreams.DocId,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               DocStreams.RbsId,
               TimeLastModified
      
        FROM DocStreams
      
             INNER JOIN AllDocs ON DocStreams.DocId = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      
      **ListBlobsInDB_2010.sql**
      
        USE WSS_Content
        GO
      
        SELECT AllDocStreams.Id,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               RbsId,
               TimeLastModified
        FROM AllDocStreams
      
             INNER JOIN AllDocs ON AllDocStreams.Id = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      ```
      
      Ha RBS megfelelően lett konfigurálva, NULL értéket meg kell jelennie hello SizeOfContentInDB oszlop lett feltöltve, és sikeresen externalized RBS az objektumhoz.
8. (Választható) Miután RBS konfigurál, és helyezze át az összes BLOB tartalom toohello StorSimple eszköz áthelyezheti hello tartalom-adatbázist toohello eszköz. Ha úgy dönt, hogy toomove hello tartalom-adatbázist, ajánlott konfigurált hello tartalom-adatbázist tároló hello eszköz elsődleges kötetként. Használja majd, létrehozni, SQL Server ajánlott eljárásokat toomigrate hello tartalom-adatbázist toohello StorSimple eszközt. 
   
   > [!NOTE]
   > Toohello eszköz áthelyezése hello tartalom-adatbázist csak támogatott hello StorSimple 8000 series (ez nem támogatott hello 5000 vagy a 7000-es adatsorozathoz).
   
   Ha hello tartalom-adatbázis és a Blobok hello StorSimple eszközön külön köteteken tárolja, azt javasoljuk a hello konfigurálása ugyanazon kötettároló. Ez biztosítja, hogy azok készül biztonsági másolat együtt.
   
   > [!WARNING]
   > Ha RBS nincs engedélyezve, hello tartalom-adatbázist toohello StorSimple eszköz áthelyezése nem ajánlott. Ez a nem tesztelt konfigurálása során.
   
9. Nyissa meg toohello következő lépés: [szemétgyűjtés konfigurálása](#configure-garbage-collection).

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
