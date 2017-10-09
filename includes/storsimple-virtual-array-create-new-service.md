#### <a name="toocreate-a-new-service"></a>egy új szolgáltatás toocreate

1.  A Microsoft-fiók hitelesítő adatait használva bejelentkezés toohello Azure portálra az URL-címen: <https://portal.azure.com/>. Ha hello eszközt kormányzati portál telepítésével, jelentkezzen be: <https://portal.azure.us/>

2.  Hello Azure-portálon, kattintson **+ új** &gt; **tárolási** &gt; **StorSimple virtuális adatsorozat**.

    ![Új szolgáltatás létrehozása](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  A hello **StorSimple Device Manager** panel, amelyen megnyílik, hello a következő:

    1.  Adjon egy egyedi **Erőforrásnevet** a szolgáltatásnak. hello erőforrásnév egy rövid nevet, amely tooidentify hello szolgáltatást használja. hello neve lehet 2 és 50 karakter közötti lehet betűket, számokat és kötőjeleket tartalmazhat. hello nevét kell kezdődnie, és betűvel vagy számmal végződhet.

    2.  Válasszon egy **előfizetés** hello legördülő listából. hello előfizetés számlázási fiók csatolt tooyour. Ez a mező nem jelenik meg abban az esetben, ha csak egy előfizetéssel rendelkezik.

    3.  A **erőforráscsoport**, válasszon egy meglévő, vagy hozzon létre egy új csoportot. További információk: [Azure-erőforráscsoportok](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).

    4.  Adjon meg egy **helyet** a szolgáltatáshoz. Lásd: [Azure-régiókat](https://azure.microsoft.com/regions/#services) további információt arról, hogy mely szolgáltatások érhetőek melyik régióban. Válasszon általánosságban egy **hely** legközelebbi toohello földrajzi régió ahová toodeploy az eszközt. Érdemes toofactor hello következőket is:

        -   Ha meglévő alkalmazások, hogy is szeretné toodeploy a StorSimple eszközt az Azure-ban, azt javasoljuk, hogy használja-e azt az adatközpontot.

        -   A StorSimple Device Manager és az Azure storage két külön helyen lehet. Ebben az esetben áll szükséges toocreate hello StorSimple Device Manager és az Azure-tárfiókot külön-külön. toocreate egy Azure storage-fiókot, nyissa meg toohello Azure Storage szolgáltatást a hello Azure-portálon, és kövesse hello [hozzon létre egy Azure Storage-fiók](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account). Miután létrehozta a fiókot, vegye fel toohello StorSimple Device Manager szolgáltatás hello utasításait követve [hello szolgáltatást egy új tárfiók konfigurálása](https://azure.microsoft.com/en-us/documentation/articles/storsimple-deployment-walkthrough/#configure-a-new-storage-account-for-the-service).

        -   Hello hello kormányzati portál virtuális eszköz üzembe helyezése, ha hello StorSimple Device Manager szolgáltatás nem áll rendelkezésre Velünk Iowa és Velünk Virginia helyeket.

    5.  Válassza ki **hozzon létre egy új Azure-tárfiók** tooautomatically hello szolgáltatásban hozzon létre egy tárfiókot. Adjon meg egy **tárfióknév**. Ha máshová szeretné menteni az adatokat, akkor törölje a pipát a jelölőnégyzetből.

    6.  Ellenőrizze **PIN-kód toodashboard** Ha azt szeretné, egy Gyorshivatkozás toothis szolgáltatás az irányítópulton.

    7.  Kattintson a **létrehozása** toocreate hello StorSimple Device Manager.

        ![Új szolgáltatás létrehozása](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

Biztos, hogy irányított toohello **szolgáltatás** kezdőlapja. hello szolgáltatás létrehozása néhány percet vesz igénybe. Miután hello szolgáltatás sikeresen létrehozva, akkor rendszer értesítést küld, és hello hello szolgáltatás állapotát a túl változik**aktív**.


