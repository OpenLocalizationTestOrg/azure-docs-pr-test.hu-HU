1. Hello portálon a **összes erőforrás**, kattintson a **+ Hozzáadás**. 
2. A hello **mindent** panel keresési mezőbe, írja be **helyi hálózati átjáró**, majd kattintson az toosearch. Ez visszaad egy listát. Kattintson a **helyi hálózati átjáró** tooopen hello panelen, majd kattintson az **létrehozása** tooopen hello **helyi hálózati átjáró létrehozása** panelen.

  ![helyi hálózati átjáró létrehozása](./media/vpn-gateway-add-lng-s2s-rm-portal-include/createlng.png)

3. A hello **létrehozás helyi hálózati átjáró panel**, adja meg a helyi hálózati átjáró hello értékeket.

  - **Név:** Adja meg a helyi hálózati átjáróobjektum nevét.
  - **IP-cím:** Ez az hello VPN-eszköz, amelyet az Azure tooconnect hello nyilvános IP-címét. Adjon meg egy érvényes nyilvános IP-címet. hello IP-cím nem lehet NAT mögött, és az Azure-ban elérhető toobe rendelkezik. Ha most nincs hello IP-cím, hello a képernyőfelvételen látható hello értékeket is használhat, de lesz toogo vissza kell, és cserélje le a helyőrző IP-cím hello nyilvános IP-címe a VPN-eszköz. Ellenkező esetben Azure nem lesz képes tooconnect.
  - **Címtér** toohello címtartományai hivatkozik hello hálózat, amely a helyi hálózati jelöli. Több címtartományt is felvehet. Győződjön meg arról, hogy az itt megadott hello tartományok nem átfedésben más hálózatokkal való tooconnect kívánt tartományokat. Azure hello címtartományt, hogy megadja a toohello a helyszíni VPN eszköz IP-címet fogja átirányítani. *Itt használja saját értékeket, nem hello hello képernyőfelvételen látható módon értékek*.
  - **Előfizetés:** győződjön meg arról, hogy a megfelelő előfizetés jelenik hello.
  - **Erőforráscsoport:** , amelyet az toouse válassza hello erőforráscsoportot. Létrehozhat egy új erőforráscsoportot, vagy kiválaszthat egy korábban létrehozottat.
  - **Hely:** adja meg, hogy ez az objektum létrejön hello helyét. Érdemes lehet tooselect hello ugyanarra a helyre, amely a virtuális hálózat található, de nem szükséges toodo így.

4. Hello értékek megadása után kattintson **létrehozása** hello panel toocreate hello helyi hálózati átjáró hello alján.
