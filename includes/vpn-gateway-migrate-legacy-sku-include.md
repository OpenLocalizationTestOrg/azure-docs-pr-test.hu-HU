> [!NOTE]
> * hello VPN-átjáró nyilvános IP-cím módosul egy régi SKU tooa való áttelepítés új Termékváltozat.
> * Hagyományos VPN-átjárók toohello nem telepíthető át új SKU. Hagyományos VPN átjáró is csak használja az örökölt (régi) termékváltozatok hello.
> 

Nem méretezhető át az Azure VPN-átjárók közötti régi termékváltozatok hello, és új SKU-családok hello. Ha van VPN-átjárók hello Resource Manager üzembe helyezési modellel, az SKU hello hello régebbi verzióját használja, áttelepítheti a toohello új SKU. toomigrate, hello meglévő VPN-átjáró törlése a virtuális hálózat, majd hozzon létre egy újat.

A migrálási munkafolyamat:

1. Távolítsa el a kapcsolatok toohello virtuális hálózati átjáró.
2. Hello régi VPN-átjáró törlése.
3. Hello új VPN-átjáró létrehozásához.
4. A helyszíni VPN-eszközök frissítése hello új VPN átjáró IP-címet (a pont-pont kapcsolatok).
5. Hello átjáró IP cím értéke toothis átjáró csatlakozó VNet – VNet helyi hálózati átjárók frissítése.
6. Töltse le az új ügyfél VPN-konfigurációs csomagokat a csatlakozó virtuális hálózati toohello a VPN-átjárón keresztül P2S-ügyfeleket.
7. Hozza létre újra a hello kapcsolatok toohello virtuális hálózati átjáró.
