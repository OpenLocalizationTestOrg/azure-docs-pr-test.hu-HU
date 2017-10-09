### <a name="is-custom-ipsecike-policy-supported-on-all-azure-vpn-gateway-skus"></a>Minden Azure VPN-átjáróhoz tartozó termékváltozat támogatja az egyéni IPsec/IKE-házirendet?
Az egyéni IPsec/IKE-házirendet az Azure **VpnGw1, VpnGw2, VpnGw3, Standard** és a **Nagy teljesítményű** VPN-átjárók támogatják. Az **alapszintű** SKU NEM támogatott.

### <a name="how-many-policies-can-i-specify-on-a-connection"></a>Hány házirendeket adhatok meg egy kapcsolathoz?
Egy adott kapcsolathoz csak ***egy*** házirendet adhat meg.

### <a name="can-i-specify-a-partial-policy-on-a-connection-eg-only-ike-algorithms-but-not-ipsec"></a>Megadhatok részleges házirendet egy kapcsolathoz? (Például csak IKE-algoritmusokat IPsec nélkül)
Nem, minden algoritmust és paramétert meg kell adnia mind az IKE (Elsődleges mód), mind az IPsec (Gyors mód) esetében. A részleges házirend-megadás nem engedélyezett.

### <a name="what-are-hello-algorithms-and-key-strengths-supported-in-hello-custom-policy"></a>Mik azok a hello algoritmusok és a kulcs szintjeiről egyéni házirendek hello támogatott?
hello az alábbi táblázat hello támogatott titkosítási algoritmusok és a kulcs szintjeiről konfigurálható hello ügyfelek. Minden mezőhöz választania kell egy lehetőséget.

| **IPsec/IKEv2**  | **Beállítások**                                                                   |
| ---              | ---                                                                           |
| IKEv2-titkosítás | AES256, AES192, AES128, DES3, DES                                             |
| IKEv2-integritás  | SHA384, MD5, SHA1, SHA256                                                     |
| DH-csoport         | DHGroup24, ECP384, ECP256, DHGroup14 (DHGroup2048), DHGroup2, DHGroup1, Nincs |
| IPsec-titkosítás | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, Nincs      |
| IPsec-integritás  | GCMAES256, GCMAES192, GCMAES128, SHA-256, SHA1, MD5                            |
| PFS-csoport        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Nincs                              |
| Gyorsmódú biztonsági társítás élettartama   | Másodperc (egész szám; **min. 300**/alapértelmezett érték: 27000 másodperc)<br>KB (egész szám; **min. 1024**/alapértelmezett érték: 102400000 KB)           |
| Forgalomválasztó | UsePolicyBasedTrafficSelectors ($True/$False; alapértelmezett: $False)                 |
|                  |                                                                               |

> [!IMPORTANT]
> 1. DHGroup2048 & PFS2048 vannak, a Diffie-Hellman csoport azonos hello **14** IKE-és IPsec PFS. Ellenőrizze a [Diffie-Hellman csoport](#DH) hello végezze el a leképezéseket.
> 2. GCMAES algoritmusok, meg kell adnia GCMAES algoritmus és a kulcs hosszának hello IPsec titkosításhoz és sértetlenségét.
> 3. IKEv2 alapmódú biztonsági Társítás élettartama hello Azure VPN gatewayek a 28 800 másodperc van rögzítve.
> 4. A gyorsmódú biztonsági társítás élettartama paraméter megadása opcionális. Ha nem ad meg értéket, a rendszer az alapértelmezett értékeket használja: 27 000 másodperc (7,5 óra) és 102400000 KB (102 GB).
> 5. UsePolicyBasedTrafficSelector megadása hello kapcsolaton beállítás paraméter. Tekintse meg a következő GYIK hello "UsePolicyBasedTrafficSelectors" eleme

### <a name="does-everything-need-toomatch-between-hello-azure-vpn-gateway-policy-and-my-on-premises-vpn-device-configurations"></a>Nem minden szükséges toomatch hello Azure VPN gateway házirend és a helyszíni VPN eszközkonfigurációk között?
A helyszíni VPN-eszköz konfigurációjában meg kell egyeznie, vagy tartalmaz a következő algoritmusokat hello és paraméterek meg hello Azure IPsec/IKE házirend:

* IKE titkosítási algoritmus
* IKE integritási algoritmus
* DH-csoport
* IPsec titkosítási algoritmus
* IPsec integritási algoritmus
* PFS-csoport
* Forgalomválasztó (*)

hello SA élettartama csak a helyi specifikációk, nem kell toomatch.

Ha engedélyezi a **UsePolicyBasedTrafficSelectors**, a VPN-eszköz van definiálva minden kiegészítve a helyi hálózati (helyi hálózati átjáró) előtagok hello és a forgalom választók megfelelő hello tooensure van szüksége Azure-beli virtuális hálózat előtagokat, bármely közöttiként helyett. Például ha a helyszíni hálózati előtagok 10.1.0.0/16 és 10.2.0.0/16, és a virtuális hálózati előtagok 192.168.0.0/16 és 172.16.0.0/16, kell a következő forgalmat választók toospecify hello:
* 10.1.0.0/16 <====> 192.168.0.0/16
* 10.1.0.0/16 <====> 172.16.0.0/16
* 10.2.0.0/16 <====> 192.168.0.0/16
* 10.2.0.0/16 <====> 172.16.0.0/16

Tekintse meg a túl[csatlakozás több helyszíni házirendalapú VPN-eszközök](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) nyújt további tájékoztatást toouse ezt a beállítást.

### <a name ="DH"></a>Mely Diffie-Hellman csoportok támogatottak?
hello az alábbi táblázat hello támogatott Diffie-Hellman csoport (DHGroup) IKE-és IPsec (PFSGroup):

| **Diffie-Hellman csoport**  | **DH-csoport**              | **PFS-csoport** | **A kulcs hossza** |
| ---                       | ---                      | ---          | ---            |
| 1                         | DHGroup1                 | PFS1         | 768 bites MODP   |
| 2                         | DHGroup2                 | PFS2         | 1024 bites MODP  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | 2048 bites MODP  |
| 19                        | ECP256                   | ECP256       | 256 bites ECP    |
| 20                        | ECP384                   | ECP284       | 384 bites ECP    |
| 24                        | DHGroup24                | PFS24        | 2048 bites MODP  |
|                           |                          |              |                |

Tekintse meg a túl[RFC3526](https://tools.ietf.org/html/rfc3526) és [RFC5114](https://tools.ietf.org/html/rfc5114) további részleteket.

### <a name="does-hello-custom-policy-replace-hello-default-ipsecike-policy-sets-for-azure-vpn-gateways"></a>Egyéni házirend hello vált hello alapértelmezett IPsec/IKE házirend beállítása az Azure VPN gatewayek?
Igen, ha egyéni házirendet a kapcsolat van megadva, Azure VPN gateway csak használandó hello házirend hello kapcsolaton, mind az internetes KULCSCSERE kezdeményező és a válaszoló IKE.

### <a name="if-i-remove-a-custom-ipsecike-policy-does-hello-connection-become-unprotected"></a>Ha egy egyéni IPsec/IKE házirend eltávolításához nem hello kapcsolat válnak nem védett?
Nem, hello kapcsolat továbbra is védeni kívánja IPsec/IKE. Kapcsolat hello egyéni házirendet eltávolítja, ha a hello Azure VPN gateway visszaállítja-e a háttérben toohello [alapértelmezett IPsec/IKE javaslatok listájának](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) , és indítsa újra hello IKE kézfogást újra a helyszíni VPN-eszköz.

### <a name="would-adding-or-updating-an-ipsecike-policy-disrupt-my-vpn-connection"></a>Az IPsec/IKE-házirend hozzáadása vagy frissítése megszakítja a VPN-kapcsolatot?
Igen, akkor a kis szüneteltetése (néhány másodpercet) hello Azure VPN gateway fog szakadjon el hello meglévő kapcsolat le és indítsa újra a hello IKE kézfogás toore-hello új titkosítási algoritmusok és a paraméterek hello IPsec-alagút létrehozásához. Ellenőrizze, hogy a helyszíni VPN-eszköz is konfigurálva hello megfelelő algoritmusok és a kulcs szintjeiről toominimize hello megszakítása.

### <a name="can-i-use-different-policies-on-different-connections"></a>Használhatok különböző házirendeket különböző kapcsolatokhoz?
Igen. Az egyéni házirendeket kapcsolatonként hozza létre. A különböző kapcsolatokhoz különböző IPsec/IKE-házirendeket hozhat létre és alkalmazhat. Másik lehetőségként tooapply egy részét kapcsolatok egyéni házirendeket. fennmaradó kiépítettektől eltérő hello hello Azure alapértelmezett IPsec/IKE házirend beállítása fogja használni.

### <a name="can-i-use-hello-custom-policy-on-vnet-to-vnet-connection-as-well"></a>Használható a VNet – VNet-kapcsolatot, valamint hello egyéni házirendet?
Igen, mind az IPsec létesítmények közötti kapcsolataihoz, mind a virtuális hálózatok közötti kapcsolatokhoz alkalmazhat egyéni házirendet.

### <a name="do-i-need-toospecify-hello-same-policy-on-both-vnet-to-vnet-connection-resources"></a>Van toospecify hello mindkét VNet – VNet-kapcsolatot erőforrások ugyanabban a házirendben?
Igen. A virtuális hálózatok közötti alagút két kapcsolati erőforrásból áll az Azure-ban, amelyek a két különböző irányba mutatnak. Mindkét kapcsolati erőforrások tooensure kell othereise hello VNet – VNet-kapcsolatot nem fog létrehozni ugyanabban a házirendben, hello.

### <a name="does-custom-ipsecike-policy-work-on-expressroute-connection"></a>Működik az egyéni IPsec/IKE-házirend az ExpressRoute-kapcsolatokkal?
Nem. IPsec/IKE házirend csak akkor működik a S2S VPN- és VNet – VNet kapcsolatokhoz hello Azure VPN gatewayek keresztül.
