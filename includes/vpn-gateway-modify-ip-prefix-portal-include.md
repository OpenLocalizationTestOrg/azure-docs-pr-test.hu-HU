### <a name="noconnection"></a>toomodify helyi hálózati átjáró IP-cím előtagokat - átjáró kapcsolat

#### <a name="tooadd-additional-address-prefixes"></a>tooadd további címelőtagokat:

1. A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **konfigurációs**.
2. Hello IP-címterület hozzáadása a hello *újabb címtartomány felvétele* mezőbe.
3. Kattintson a **mentése** toosave a beállításokat.

#### <a name="tooremove-address-prefixes"></a>tooremove címelőtagok:

1. A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **konfigurációs**.
2. Kattintson a hello **"..."** hello előtag tartalmazó hello sor tooremove szeretné.
3. Kattintson a **eltávolítása**.
4. Kattintson a **mentése** toosave a beállításokat.

### <a name="withconnection"></a>toomodify helyi hálózati átjáró IP-cím előtagokat - meglévő gateway-kapcsolatot

Ha átjáró kapcsolat és szeretné, hogy tooadd, vagy távolítsa el a hello a helyi hálózati átjáróban tárolt IP-címelőtagokat, akkor toodo hello lépések sorrendben. Ez némi állásidőt jelent a VPN-kapcsolata számára. Ha módosítja az IP-cím előtagokat, nincs szükség a toodelete hello VPN-átjáró. Csak akkor kell tooremove hello kapcsolat.

#### <a name="1-remove-hello-connection"></a>1. Távolítsa el a hello kapcsolatot.

1. A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **kapcsolatok**.
2. Kattintson a hello **...**  hello sor minden egyes kapcsolathoz, majd kattintson a **törlése**.
3. Kattintson a **mentése** toosave a beállításokat.

#### <a name="2-modify-hello-address-prefixes"></a>2. Módosítsa a címelőtagokat hello.

tooadd további címelőtagokat:

1. A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **konfigurációs**.
2. Hello IP-címterület hozzáadása.
3. Kattintson a **mentése** toosave a beállításokat.

tooremove címelőtagok:

1. A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **konfigurációs**.
2. Kattintson a hello **...**  hello előtag tartalmazó hello sor tooremove szeretné.
3. Kattintson a **eltávolítása**.
4. Kattintson a **mentése** toosave a beállításokat.

#### <a name="3-recreate-hello-connection"></a>3. Hozza létre újra a hello kapcsolatot.

1. Keresse meg a virtuális hálózati átjáró toohello a Vnethez tartozó. (Már nem hello helyi hálózati átjáró van hátra.)
2. A virtuális hálózati átjáró hello a hello **beállítások** kattintson **kapcsolatok**.
3. Kattintson a hello **+ Hozzáadás** tooopen hello **kapcsolat hozzáadása a** panelen.
4. Hozza létre újra a kapcsolatot.
5. Kattintson a **OK** toocreate hello kapcsolat.
