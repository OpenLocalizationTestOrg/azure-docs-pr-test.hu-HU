## <a name="scenario"></a>Forgatókönyv
toobetter bemutatják, hogyan toocreate NSG-ket, ez a dokumentum használja hello részletesen.

![VNet-forgatókönyv](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

Ebben a forgatókönyvben létrehoz egy NSG-t az egyes alhálózatokon lévő hello **TestVNet** virtuális hálózatban, folyamata az alábbiakban olvasható: 

* **NSG-előtérbeli**. hello előtér NSG lesz alkalmazott toohello *előtér* alhálózatot, és két szabályt tartalmaz:    
  * **RDP-szabály**. Ez a szabály lehetővé teszi az RDP-forgalmát toohello *előtér* alhálózat.
  * **webalkalmazás-szabály**. Ez a szabály lehetővé teszi a HTTP-forgalom toohello *előtér* alhálózat.
* **NSG-háttérrendszer**. hello háttér NSG lesz alkalmazott toohello *háttér* alhálózatot, és két szabályt tartalmaz:    
  * **SQL-szabály**. Ez a szabály lehetővé teszi, hogy csak a hello forgalmát SQL *előtér* alhálózat.
  * **webalkalmazás-szabály**. Ez a szabály letiltja az összes internetes forgalom kötött a hello *háttér* alhálózat.

hello ezek a szabályok kombinációját hozzon létre egy DMZ-szerű forgatókönyv, ahol hello háttér alhálózati csak az SQL hello előtér alhálózatból bejövő forgalom fogadására, és nincs hozzáférési toohello Internet, amíg hello előtér alhálózati kommunikálhatnak hello Internet rendelkezik, és csak a bejövő HTTP-kérelmek fogadásához.

