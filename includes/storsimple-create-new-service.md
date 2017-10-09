<!--author=alkohli last changed:01/14/2016-->


#### <a name="toocreate-a-new-service"></a>egy új szolgáltatás toocreate
1. A Microsoft-fiók hitelesítő adatait használva bejelentkezés toohello klasszikus Azure portálra az URL-címen: [https://manage.windowsazure.com/](https://manage.windowsazure.com/).
2. Hello a klasszikus Azure portálon, kattintson **új** > **adatszolgáltatások** > **StorSimple Manager** > **gyors Hozzon létre**.
3. Az hello hello űrlap jelenik meg, a következő:
   
   1. Adjon egy egyedi **nevet** a szolgáltatásnak. Ez egy rövid nevet, amely az tooidentify hello szolgáltatást használja. hello neve lehet 2 és 50 karakter közötti lehet betűket, számokat és kötőjeleket tartalmazhat. hello nevét kell kezdődnie, és betűvel vagy számmal végződhet.
   2. Adjon meg egy **helyet** a szolgáltatáshoz. Általánosságban elmondható válassza ki egy hely legközelebbi toohello földrajzi régióban toodeploy, ahová az eszközét. Érdemes toofactor hello következőket is: 
      
      * Ha meglévő alkalmazások, hogy is szeretné toodeploy a StorSimple eszközt az Azure-ban, akkor válassza azt az adatközpontot.
      * A StorSimple Manager és az Azure Storage szolgáltatás helye eltérő is lehet. Ebben az esetben áll szükséges toocreate hello StorSimple Manager és az Azure-tárfiókot külön-külön. toocreate egy Azure storage-fiókot, nyissa meg toohello Azure Storage szolgáltatást a klasszikus Azure portálon hello, és kövesse hello [hozzon létre egy Azure Storage-fiók](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account). Miután létrehozta a fiókot, vegye fel toohello StorSimple Manager szolgáltatás hello utasításait követve [hello szolgáltatást egy új tárfiók konfigurálása](../articles/storsimple/storsimple-deployment-walkthrough.md#configure-a-new-storage-account-for-the-service).
   3. Válasszon egy **előfizetés** hello legördülő listából. hello előfizetés számlázási fiók csatolt tooyour. Ez a mező nem jelenik meg abban az esetben, ha csak egy előfizetéssel rendelkezik.
   4. Válassza ki **hozzon létre egy új tárfiókot** tooautomatically hello szolgáltatásban hozzon létre egy tárfiókot. Ez a tárfiók egy speciális névvel jön létre, pl.: „storsimplebwv8c6dcnf”. Ha máshová szeretné menteni az adatokat, akkor törölje a pipát a jelölőnégyzetből. 
   5. Kattintson a **StorSimple Manager létrehozása** toocreate hello szolgáltatást.
   
   ![StorSimple Manager létrehozása](./media/storsimple-create-new-service/HCS_CreateAService-include.png)
   
   Irányított toohello fogja **szolgáltatás** kezdőlapja. hello szolgáltatás létrehozása eltarthat néhány percig. Miután hello szolgáltatás sikeresen létrehozva, akkor rendszer értesítést küld, és hello hello szolgáltatás állapotát a túl változik**aktív**.
   
   ![Szolgáltatás létrehozása](./media/storsimple-create-new-service/HCS_StorSimpleManagerServicePage-include.png)

![Videó elérhető](./media/storsimple-create-new-service/Video_icon.png)**Videó elérhető**

videó toowatch hogyan toocreate egy új StorSimple Manager szolgáltatás kattintson [Itt](https://azure.microsoft.com/documentation/videos/create-a-storsimple-manager-service/).

