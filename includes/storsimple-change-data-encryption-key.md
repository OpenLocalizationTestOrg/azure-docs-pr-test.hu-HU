<!--author=SharS last changed: 12/01/15-->

### <a name="step-1-authorize-a-device-toochange-hello-service-data-encryption-key-in-hello-azure-classic-portal"></a>1. lépés: Engedélyezze a eszköz toochange hello szolgáltatásadat-titkosítási kulcs a klasszikus Azure portálon hello
Általában hello eszköz-rendszergazdai kérni fogja, hogy hello szolgáltatás-rendszergazda engedélyezi egy eszköz toochange szolgáltatás titkosítási kulcsokat. hello szolgáltatás-rendszergazda majd engedélyeztetéséhez használandó hello toochange hello eszközkulcs.

Ez a lépés a klasszikus Azure portálon hello történik. hello szolgáltatás-rendszergazda jogosult jogosult toobe hello eszközök megjelenített listáját egy eszközt is kijelölhet. hello eszköz nem engedélyezett toostart hello szolgáltatásadat-titkosítási kulcs módosítása folyamat.

#### <a name="which-devices-can-be-authorized-toochange-service-data-encryption-keys"></a>Mely eszközök engedélyezett toochange szolgáltatás titkosítási kulcsokat?
Egy eszköz hello ahhoz, azok engedélyezett tooinitiate szolgáltatás titkosítási kulcs adatváltozásokat a következő feltételeknek kell megfelelniük:

* hello eszköz jogosult szolgáltatás adatok titkosítás kulcsváltozás engedélyezése az online toobe kell lennie.
* Engedélyezheti a hello ugyanarra az eszközre újra 30 másodperc után ha hello kulcs módosítása nem lett inicializálva.
* Engedélyezheti egy másik eszközön, feltéve, hogy hello kulcsváltozás hello korábban eszköz nem lett inicializálva. Miután hello új eszköz engedélyezve van-e, hello régi eszköz hello módosítása nem kezdeményezhetik.
* Egy eszköz nem lehet engedélyezni, amíg folyamatban van hello hello szolgáltatás adatok adattitkosítási kulcshoz kapcsolódó kulcsváltást.
* Ha néhány hello szolgáltatásban regisztrált hello eszközök rendelkezik tanúsítványváltást hello titkosítási míg mások számára nem engedélyezhető a eszköz. Ilyen esetekben hello jogosult tartoznak hello gazdarendszerhez hello szolgáltatás titkosítási kulcs változását befejeződött.

> [!NOTE]
> Hello a klasszikus Azure portálon, a StorSimple virtuális eszköz csak akkor jelennek meg hello listája, amely jogosult toostart hello kulcsváltozás.
> 
> 

Egy eszköz tooinitiate hello szolgáltatás titkosítási kulcs változását helyszerepkörre, és hajtsa végre a következő lépéseket tooselect hello.

#### <a name="tooauthorize-a-device-toochange-hello-key"></a>tooauthorize egy toochange hello eszközkulcs
1. A hello szolgáltatás irányítópult-oldalon, kattintson **módosítás szolgáltatásadat-titkosítási kulcs**.
   
    ![Szolgáltatás-titkosítási kulcs módosítása](./media/storsimple-change-data-encryption-key/HCS_ChangeServiceDataEncryptionKey-include.png)
2. A hello **módosítás szolgáltatásadat-titkosítási kulcs** párbeszédpanelen válassza ki, és engedélyezi egy eszköz tooinitiate hello szolgáltatás titkosítási kulcs változását. hello legördülő lista rendelkezik hello jogosult eszközöket is engedélyezhető.
3. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-change-data-encryption-key/HCS_CheckIcon-include.png).

### <a name="step-2-use-windows-powershell-for-storsimple-tooinitiate-hello-service-data-encryption-key-change"></a>2. lépés: A Windows Powershellt használja a StorSimple tooinitiate hello szolgáltatás titkosítási kulcs változását
Ebben a lépésben történik hello Windows PowerShell, a StorSimple illesztőfelülete hello a StorSimple eszköz engedélyezett.

> [!NOTE]
> Nincsenek műveletek is elvégezhetők hello a StorSimple Manager szolgáltatás a klasszikus Azure portálon hello kulcshoz kapcsolódó kulcsváltás után befejezéséig.
> 
> 

Ha hello eszköz soros konzoljához tooconnect toohello Windows PowerShell-felületet használ, hajtsa végre a lépéseket követve hello.

#### <a name="tooinitiate-hello-service-data-encryption-key-change"></a>tooinitiate hello szolgáltatásadat-titkosítási kulcs módosítása
1. Válassza ki az 1. lehetőség toolog teljes hozzáféréssel.
2. Hello parancsot, írja be a parancssorba:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Miután hello parancsmag sikeresen befejeződött, kap egy új szolgáltatásadat-titkosítási kulcs. Másolja ki és mentse a kulcs használható a 3. lépésben a folyamat során. Ez a kulcs lesz tooupdate használt összes hello fennmaradó hello StorSimple Manager szolgáltatásban regisztrált eszközöket.
   
   > [!NOTE]
   > Ez a folyamat engedélyezése a StorSimple eszköz négy órán belül kell kezdeményezni.
   > 
   > 
   
   Ez az új kulcs küldi el a rendszer toohello szolgáltatás leküldött toobe tooall hello regisztrált eszközök hello szolgáltatásban. Riasztás megjelenik hello szolgáltatás irányítópultját. hello szolgáltatás le fogja tiltani az összes hello művelet hello regisztrált eszközökön, és eszköz-rendszergazdai hello majd kell tooupdate hello szolgáltatásadat-titkosítási kulcs a hello, más eszközök. Azonban hello i/o műveletek (gazdagép toohello felhő adatok küldése) fog nem működni.
   
   Ha még egyetlen eszközt regisztrált tooyour szolgáltatás, hello helyettesítő folyamat befejeződött, és hello a következő lépést kihagyhatja. Ha több eszközök regisztrált tooyour szolgáltatás, folytassa a toostep 3.

### <a name="step-3-update-hello-service-data-encryption-key-on-other-storsimple-devices"></a>3. lépés: Frissítés hello szolgáltatásadat-titkosítási kulcs a más StorSimple eszközökhöz
Hello Windows PowerShell felületet a StorSimple eszköz ezeket a lépéseket kell elvégezni, ha több eszközök regisztrált tooyour StorSimple Manager szolgáltatás. 2. lépésben beszerzett hello kulcs: a Windows Powershellt használja a StorSimple tooinitiate hello szolgáltatás titkosítási kulcs változását összes hello StorSimple eszköz regisztrálva a StorSimple Manager szolgáltatás hello fennmaradó használt tooupdate kell lennie.

Hajtsa végre a hello követő lépéseket tooupdate hello szolgáltatásadat-titkosítási az eszközön.

#### <a name="tooupdate-hello-service-data-encryption-key"></a>tooupdate hello szolgáltatásadat-titkosítási kulcs
1. StorSimple tooconnect toohello konzol a Windows Powershellt használja. Válassza ki az 1. lehetőség toolog teljes hozzáféréssel.
2. Hello parancsot, írja be a parancssorba:
   
    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Adja meg a hello szolgáltatásadat-titkosítási kulcs beolvasott [2. lépés: a Windows Powershellt használja a StorSimple tooinitiate hello szolgáltatás titkosítási kulcs változását](#to-initiate-the-service-data-encryption-key-change).

