<!--author=alkohli last changed: 9/17/15-->

#### <a name="tooadd-a-storage-account-in-storsimple-8000-series-update-10"></a>tooadd egy tárfiókot, a StorSimple 8000 Series 1.0-s frissítésében
1. A hello StorSimple Manager szolgáltatás kezdőlapján válasza ki a szolgáltatást, és kattintson rá duplán. A rendszer ekkor toohello **gyors üzembe helyezés** lap. Jelölje be hello **konfigurálása** lap.
2. Kattintson a **Tárfiók hozzáadása/szerkesztése** elemre.
3. A hello **Tárfiók hozzáadása/szerkesztése** párbeszédpanel, kattintson a **új hozzáadása**.
4. A hello **szolgáltató** mezőjét, jelölje be hello kívánt felhőszolgáltatót. hello támogatott szolgáltatók a következők: Azure, Amazon S3, Amazon S3 with RRS, HP és OpenStack. Adja meg a hello hitelesítő adatokat és a felhőbeli szolgáltatók hello tárfiók társított hello helyét. hello mezők hitelesítő adatok megadására lesz hello megadott felhőbeli szolgáltatás szolgáltatója különböző függően. 
   
   * Ha az Azure felhőszolgáltatást választotta, adja meg a hello **neve** és elsődleges hello **hozzáférési kulcs** a Microsoft Azure-tárfiók. Azure-fiókot hello hely adatok automatikusan kitöltődnek.
     
        ![Azure-tárfiók hozzáadása](./media/storsimple-configure-new-storage-account-u1/AddAzureStorageaccount-include.png)
   * Ha az Amazon S3 vagy az Amazon S3 with RRS szolgáltatót választotta, adjon meg egy rövid **tárfióknevet**, **elérési kulcsot** és **titkos kulcsot**. Az Amazon S3 és az Amazon S3 with rrs Szolgáltatót hello alábbi helyek támogatottak:
     
     * USA szabvány
     * USA nyugati régiója (Oregon)
     * USA nyugati régiója (Észak-Kalifornia)
     * EU (Írország)
     * Ázsia és a Csendes-óceáni térség (Szingapúr)
     * Ázsia és a Csendes-óceáni térség (Sydney)
     * Ázsia és a Csendes-óceáni térség (Tokió)
     * Dél-Amerika (Sao Paulo)
       
       ![Amazon-tárfiók hozzáadása](./media/storsimple-configure-new-storage-account-u1/AddAmazonStorageaccount-include.png)
   * Ha a HP felhőszolgáltatót választotta, adjon meg egy rövid **tárhelyfióknevet**, **bérlőazonosítót**, **felhasználónevet** és **jelszót**. A HP esetén hello alábbi helyek támogatottak:
     
     * USA keleti régiója
     * USA nyugati régiója
       
       ![HP-tárfiók hozzáadása](./media/storsimple-configure-new-storage-account-u1/AddHPStorageaccount-include.png)
   * Ha az **Openstack** felhőszolgáltatót választotta, adjon meg egy **állomásnevet**, **elérési kulcsot** és **titkos kulcsot**.
     
     > [!NOTE]
     > Minden hello felhőszolgáltatóknál, az Azure kivételével egy rövid nevet is megadhat. Különböző rövid névvel, és hozzon létre több tárfiókot hello megegyezik a hitelesítő adatok beállítása.
     > 
     > 
     
        ![Openstack-tárfiók hozzáadása](./media/storsimple-configure-new-storage-account-u1/AddOpenstackStorageaccount-include.png)
5. Válassza ki **SSL-mód engedélyezése** toocreate biztonságos csatornát a eszköz- és hello felhő közötti hálózati kommunikációhoz. Törölje a jelet hello **SSL-mód engedélyezése** jelölőnégyzet csak akkor, ha magánfelhőben tevékenykedik.
   
   > [!NOTE]
   > Ha a HP szolgáltatót választotta, akkor az SSL-mód mindig engedélyezve van.
   > 
   > 
6. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png). Értesítést fog kapni hello tárfiók sikeres létrehozása után.
7. az újonnan létrehozott tárfiók hello jelenik meg a hello **konfigurálása** lapon az **tárfiókok**. Kattintson a **mentése** toosave hello új tárfiókot. Ha a rendszer megerősítést kér, kattintson az **OK** gombra.

