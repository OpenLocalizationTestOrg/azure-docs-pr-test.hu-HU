<!--author=alkohli last changed:02/10/2017-->


#### <a name="toocreate-a-new-service"></a>egy új szolgáltatás toocreate

1. Használja a Microsoft-fiók hitelesítő adatait toolog toohello [Azure-portálon](https://portal.azure.com/).

2. Hello Azure-portálon, kattintson  **+**  majd hello piactéren **láthatja az összes**.

    ![StorSimple-eszközkezelő létrehozása](./media/storsimple-8000-create-new-service/createssdevman1.png)

    _StorSimple fizikai eszközök_ keresése. Válassza ki, majd kattintson a **StorSimple fizikaieszköz-sorozat** lehetőségre, majd kattintson a **Létrehozás** gombra. Másik lehetőségként kattintson hello Azure-portálon  **+**  majd a **tárolási**, kattintson a **StorSimple fizikai eszköz adatsorozat**.

    ![StorSimple-eszközkezelő létrehozása](./media/storsimple-8000-create-new-service/createssdevman11.png)

3. A hello **StorSimple Device Manager** panelen hello a következő lépéseket:
   
   1. Adjon egy egyedi **Erőforrásnevet** a szolgáltatásnak. Ez a név egy rövid nevet, amely tooidentify hello szolgáltatást használja. hello neve lehet 2 és 50 karakter közötti lehet betűket, számokat és kötőjeleket tartalmazhat. hello nevét kell kezdődnie, és betűvel vagy számmal végződhet.

   2. Válasszon egy **előfizetés** hello legördülő listából. hello előfizetés számlázási fiók csatolt tooyour. Ez a mező nem jelenik meg abban az esetben, ha csak egy előfizetéssel rendelkezik.

   3. Egy **Erőforráscsoporthoz** választhatja a **Meglévő használata** vagy az **Új létrehozása** lehetőséget. További információk: [Azure-erőforráscsoportok](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).
   
   4. Adjon meg egy **helyet** a szolgáltatáshoz. Általánosságban elmondható válassza ki egy hely legközelebbi toohello földrajzi régióban toodeploy, ahová az eszközét. A következő szempontok hello toofactor is érdemes lehet: 
      
      * Ha meglévő alkalmazások, hogy is szeretné toodeploy a StorSimple eszközt az Azure-ban, akkor válassza azt az adatközpontot.
      * A StorSimple-eszközkezelő és az Azure Storage szolgáltatás helye eltérő is lehet. Ebben az esetben áll szükséges toocreate hello StorSimple Device Manager és az Azure-tárfiókot külön-külön. toocreate egy Azure storage-fiókot, nyissa meg toohello Azure Storage szolgáltatást a hello Azure-portálon, és kövesse hello [hozzon létre egy Azure Storage-fiók](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account). Miután létrehozta a fiókot, vegye fel toohello StorSimple Device Manager szolgáltatás hello utasításait követve [hello szolgáltatást egy új tárfiók konfigurálása](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#configure-a-new-storage-account-for-the-service).

   5. Válassza ki **hozzon létre egy új tárfiókot** tooautomatically hello szolgáltatásban hozzon létre egy tárfiókot. Adja meg a tárfiók nevét. Ha máshová szeretné menteni az adatokat, akkor törölje a pipát a jelölőnégyzetből.

   6. Ellenőrizze **PIN-kód toodashboard** Ha azt szeretné, egy Gyorshivatkozás toothis szolgáltatás az irányítópulton.
      
   7. Kattintson a **létrehozása** toocreate hello StorSimple Device Manager.

       ![StorSimple-eszközkezelő létrehozása](./media/storsimple-8000-create-new-service/createssdevman2.png)
   
hello szolgáltatás létrehozása néhány percet vesz igénybe. Hello szolgáltatás sikeres létrehozása után megjelenik egy értesítés, és megnyílik hello új szolgáltatás panelre.
   
![StorSimple-eszközkezelő létrehozása](./media/storsimple-8000-create-new-service/createssdevman5.png)


