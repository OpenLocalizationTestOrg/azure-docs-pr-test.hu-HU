1. A **Csatlakozás** elemre kattintva létrehoz és letölt egy Remote Desktop Protocol fájlt (.rdp-fájlt). Kattintson a **nyitott** toouse ezt a fájlt.
2. Egy figyelmeztetés fog megjelenni, hogy hello RDP-fájl közzétevője ismeretlen. Ez nem jelent problémát. Hello távoli asztal ablakában kattintson **Connect** toocontinue.
   
    ![Képernyőkép az ismeretlen közzétevőre vonatkozó figyelmeztetésről.](./media/virtual-machines-log-on-win-server/rdp-warn.png)
3. A hello **Windows biztonsági** ablak, írja be egy fiók hitelesítő adatainak hello hello virtuális gépen, és kattintson **OK**.
   
     **Helyi fiók** – Ez általában az hello helyi fiók felhasználónevét és jelszavát, amely létrehozásakor adott hello virtuális gép. Ebben az esetben hello tartomány hello hello virtuális gép nevét, és azt is meg kell adni *vmname*&#92; *felhasználónév*.  
   
    **Tartományhoz csatlakoztatott virtuális gép** – hello VM tartozik tooa tartomány, ha hello formátumban adja meg felhasználónevét hello *tartomány*&#92; *Felhasználónév*. hello fiókot is kell tooeither hello rendszergazdák csoport vagy távoli hozzáférési jogosultságokkal toohello VM rendelkezik.
   
    **Tartományvezérlő** - Ha hello virtuális gép egy tartományvezérlő, a hello írja be a felhasználónevet és a tartományhoz tartozó tartományi rendszergazdai fiók jelszavát.
4. Kattintson a **Igen** tooverify hello hello virtuális gép identitását, és a bejelentkezés befejezéséhez.
   
   ![Képernyőkép üzenetről hello VM hello identitásának ellenőrzésére.](./media/virtual-machines-log-on-win-server/cert-warning.png)

