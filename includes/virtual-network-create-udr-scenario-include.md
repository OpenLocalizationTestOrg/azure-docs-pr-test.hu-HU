## <a name="scenario"></a>Forgatókönyv
toobetter bemutatják, hogyan toocreate udr-EK, ez a dokumentum használja az alábbi hello forgatókönyv.

![KÉPLEÍRÁS](./media/virtual-network-create-udr-scenario-include/figure1.png)

Ebben a forgatókönyvben a hello egy UDR hoz létre *első záró alhálózat* és egy másik UDR a hello *vissza záró alhálózat* , az alábbiak szerint: 

* **UDR-előtérbeli**. hello előtér UDR lesz alkalmazott toohello *előtér* alhálózatot, és egy útvonalat tartalmazza:    
  * **RouteToBackend**. Ez az útvonal elküld minden forgalom toohello háttér alhálózati toohello **FW1** virtuális gépet.
* **UDR-háttérrendszer**. hello háttér UDR lesz alkalmazott toohello *háttér* alhálózatot, és egy útvonalat tartalmaznak:    
  * **RouteToFrontend**. Ez az útvonal elküld minden forgalom toohello előtér alhálózati toohello **FW1** virtuális gépet.

Ezeket az útvonalakat hello kombinációja lesz győződjön meg arról, hogy az egyik alhálózat tooanother adatforgalmat lesz irányított toohello **FW1** virtuális gépet, mely a virtuális készülék használatos. Ezt a virtuális gépet is kell az IP-továbbítás tooturn, megkaphatja a forgalom tooensure tooother virtuális gépek felé.

