#### <a name="toocreate-public-endpoints-on-hello-cloud-appliance"></a>nyilvános végpontok toocreate hello felhő készüléken

1. Bejelentkezés toohello Azure-portálon.
2. Nyissa meg túl**virtuális gépek**, majd válassza ki, és kattintson a hello virtuális gépet, amely a felhő készülék használatos.
    
3. A virtuális gép mindkét kell toocreate egy hálózati biztonsági csoport (NSG) szabály toocontrol hello az forgalom áramlását. Hajtsa végre a következő lépéseket toocreate egy NSG-szabály hello.
    1. Válassza a **Hálózati biztonsági csoport** lehetőséget.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. Kattintson a hello alapértelmezett hálózati biztonsági csoportot, amely számára jelenik meg.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. Válassza a **Bejövő biztonsági szabályok** elemet.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. Kattintson a **+ Hozzáadás** toocreate egy bejövő biztonsági szabály.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        Hello Hozzáadás bejövő biztonsági szabály panelen:

        1. A hello **neve**, hello végpont típusa hello következő nevet: WinRMHttps.
        
        2. A hello **prioritás**, jelölje be több kisebb, mint 1000 (amely hello prioritását hello alapértelmezett szabály). Nagyobb hello érték, hello alacsonyabb prioritású.

        3. Set hello **forrás** túl**bármely**.

        4. A hello **szolgáltatás**, jelölje be **WinRM**. Hello **protokoll** automatikusan értéke túl**TCP** és hello **porttartomány** értéke túl**5986-os**.

        5. Kattintson a **OK** toocreate hello szabály.

            ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. Az utolsó lépés a hálózati biztonsági csoport egy alhálózatot vagy egy adott hálózati illesztőn tooassociate. Hajtsa végre a következő lépéseket tooassociate hello egy alhálózatot a hálózati biztonsági csoport.
    1. Nyissa meg túl**alhálózatok**.
    2. Kattintson a **+ Társítás** gombra.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. Válassza ki a virtuális hálózatot, majd válassza ki a megfelelő alhálózati hello.
    4. Kattintson a **OK** toocreate hello szabály.

        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

Hello szabály létrehozása után megtekintheti az részletek toodetermine hello nyilvános virtuális IP-cím (VIP) címét. Jegyezze fel ezt az IP-címet.


