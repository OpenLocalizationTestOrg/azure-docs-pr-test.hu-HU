### <a name="gwipnoconnection"></a>toomodify hello helyi hálózati átjáró IP-cím - átjáró kapcsolat

Hello példa toomodify a helyi hálózati átjáró, amely nem rendelkezik egy gateway-kapcsolatot használjon. Ha módosítja ezt az értéket, módosíthatja is hello címelőtagok: hello ugyanannyi időt vesz igénybe.

1. A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **konfigurációs**.
2. A hello **IP-cím** módosítsa hello IP-címet.
3. Kattintson a **mentése** toosave hello beállításait.

### <a name="gwipwithconnection"></a>toomodify hello helyi hálózati átjáró átjáró IP-cím - meglévő gateway-kapcsolatot

toomodify a helyi hálózati átjáró-kapcsolattal, toofirst eltávolítása hello kapcsolat szükséges. Hello kapcsolat eltávolítása után módosítja a hello átjáró IP-címet, és hozza létre újra egy új kapcsolatot. Módosíthatja is hello címelőtagok: hello ugyanannyi időt vesz igénybe. Ez némi állásidőt jelent a VPN-kapcsolata számára. Ha módosítja a hello átjáró IP-címe, nincs szükség a toodelete hello VPN-átjáró. Csak akkor kell tooremove hello kapcsolat.
 
#### <a name="1-remove-hello-connection"></a>1. Távolítsa el a hello kapcsolatot.

1. A helyi hálózati átjáró erőforrás hello hello **beállítások** kattintson **kapcsolatok**.
2. Kattintson a hello **...**  hello sor hello kapcsolat, majd kattintson a **törlése**.
3. Kattintson a **mentése** toosave a beállításokat.

#### <a name="2-modify-hello-ip-address"></a>2. Hello IP-cím módosításához.

Módosíthatja is hello címelőtagok: hello ugyanannyi időt vesz igénybe.

1. A hello **IP-cím** módosítsa hello IP-címet.
2. Kattintson a **mentése** toosave hello beállításait.

#### <a name="3-recreate-hello-connection"></a>3. Hozza létre újra a hello kapcsolatot.

1. Keresse meg a virtuális hálózati átjáró toohello a Vnethez tartozó. (Már nem hello helyi hálózati átjáró van hátra.)
2. A virtuális hálózati átjáró hello a hello **beállítások** kattintson **kapcsolatok**.
3. Kattintson a hello **+ Hozzáadás** tooopen hello **kapcsolat hozzáadása a** panelen.
4. Hozza létre újra a kapcsolatot.
5. Kattintson a **OK** toocreate hello kapcsolat.
