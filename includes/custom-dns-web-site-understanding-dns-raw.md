Tartománynévrendszer (DNS) hello hello használt toolocate erőforrásokat interneten. Például amikor web app címet adjon meg a böngészőben, vagy egy hivatkozásra egy weblap, az DNS-tootranslate hello tartományhoz tartozó IP-címet. hello IP-cím rendezésére a hasonlít egy utca, házszám, de nem nagyon emberi leíró. Például sokkal könnyebb tooremember egy DNS-nevet, például a **contoso.com** , mint az tooremember például 192.168.1.88 vagy 2001:0:4137:1f67:24a2:3888:9cce:fea3 IP-címet.

DNS-rendszerében hello alapuló *rekordok*. Rekordok társítson egy adott *neve*, például a **contoso.com**, IP-címet vagy egy másik DNS-nevét. Egy alkalmazás, például egy webes böngésző, egy nevet a DNS-ben, keres hello rekord keresése, és használja a függetlenül mutat tooas hello cím. Ha a hello érték azt pontok toois IP-címet, hello böngésző ezt az értéket fogja használni. Mutat tooanother DNS-nevét, majd hello alkalmazásnak toodo feloldási újra. Végső soron minden névfeloldás véget ér az IP-címet.

Az App Service egy webalkalmazás létrehozásakor egy DNS-név automatikusan toohello webalkalmazás. Ez a név veszi hello formája  **&lt;yourwebappname&gt;. azurewebsites.net**. Nincs is egy virtuális IP-cím használható DNS létrehozása rögzíti, amikor, létrehozhat rögzíti, hogy pont toohello **. azurewebsites.net**, vagy kijelölhet toohello IP-címet.

> [!NOTE]
> a webalkalmazás hello IP-címe változik, ha törölje és hozza létre újra a webes alkalmazást, vagy módosítsa a hello App Service-csomag mód túl**szabad** túl beállítása után**alapvető**, **megosztott**, vagy **szabványos**.
> 
> 

Van még több típusú bejegyzés – mindegyiket a saját funkciók és -korlátozások, de a web Apps csak ügyelünk körülbelül két, *A* és *CNAME* rögzíti.

### <a name="address-record-a-record"></a>Cím rekordot (A)
Az A rekord rendeli hozzá egy tartományhoz, például a **contoso.com** vagy **www.contoso.com**, *vagy helyettesítő tartomány* például  **\*. contoso.com**, tooan IP-címet. Egy webalkalmazást az App Service hello esetben hello vagy virtuális IP-címe hello szolgáltatás vagy egy adott IP-cím, a webalkalmazás vásárolta.

egy A rekordot egy olyan CNAME rekordot keresztül hello fő előnyei a következők:

* Egy legfelső szintű tartomány például hozzárendelhető **contoso.com** tooan IP-cím; sok regisztráló szervezetek engedélyezése csak az A rekordok használatával
* Akkor használja, mint a helyettesítő karakter, egy vagy több bejegyzése  **\*. contoso.com**, amely volna tanúsítványigénylések több altartományok például **mail.contoso.com**,  **blogs.contoso.com**, vagy **www.contso.com**.

> [!NOTE]
> Mivel az A rekord le van képezve tooa statikus IP-cím, nem tudják automatikusan feloldani módosítások toohello IP-címét a webes alkalmazást. IP-címet A rekordok való használatra biztosított a beállításokat az egyéni tartomány nevét a webalkalmazás; azonban előfordulhat, hogy módosítsa ezt az értéket, törölje és hozza létre újra a webes alkalmazás vagy az alkalmazásszolgáltatási csomag mód tooback hello túl módosítani**szabad**.
> 
> 

### <a name="alias-record-cname-record"></a>Aliasrekordot (CNAME rekord)
A CNAME rekord rendeli hozzá a *adott* DNS-nevével, például a **mail.contoso.com** vagy **www.contoso.com**, tooanother (kanonikus) tartomány nevét. Az App Service Web Apps hello esetben hello kanonikus tartománynév megadása hello  **&lt;yourwebappname >. azurewebsites.net** webalkalmazás tartománynevet. Létrehozása után hello CNAME létrehoz egy aliast hello  **&lt;yourwebappname >. azurewebsites.net** tartomány nevét. hello CNAME bejegyzés megoldja toohello IP-címét a  **&lt;yourwebappname >. azurewebsites.net** tartománynév automatikusan, így ha hello IP-cím hello webalkalmazás megváltozik, nem kell tootake bármely művelet.

> [!NOTE]
> Néhány tartomány regisztráló szervezetek csak teszi toomap altartományok, mint egy olyan CNAME rekordot használatakor **www.contoso.com**, és nem neveket, többek között a **contoso.com**. A regisztráló által biztosított hello dokumentációjában talál további információt a CNAME-rekordot, <a href="http://en.wikipedia.org/wiki/CNAME_record">Wikipedia bejegyzés a CNAME rekord hello</a>, vagy hello <a href="http://tools.ietf.org/html/rfc1035">IETF tartománynév - megvalósítása és meghatározása</a> a dokumentum.
> 
> 

### <a name="web-app-dns-specifics"></a>Webes alkalmazás DNS rögzítésen
Az A rekord használatával a webalkalmazásokkal meg toofirst hozzon létre egy TXT rekordot a következő hello:

* **Hello gyökértartományban** -rekordot A DNS TXT  **@**  túl  **&lt;yourwebappname&gt;. azurewebsites.net**.
* **Az adott altartomány** -A DNS-neve  **&lt;altartomány >** túl**&lt;yourwebappname&gt;. azurewebsites.net**. Például **blogok** Ha hello rekord **blogs.contoso.com**.
* **A hello helyettesítő sub-dodmains** -rekordot A DNS TXT x túl  **&lt;yourwebappname&gt;. azurewebsites.net**.

A TXT-rekord használt tooverify, hogy Ön a tulajdonosa toouse próbált hello tartományban. Továbbá ez a virtuális IP-cím toohello webalkalmazás mutató toocreating egy A rekordot.

Hello IP-cím található, és **. azurewebsites.net** végrehajtásával a webalkalmazás nevének hello lépéseket követve:

1. A böngészőben nyissa meg a hello [Azure Portal](https://portal.azure.com).
2. A hello **Web Apps** panelen kattintson a webalkalmazás hello nevére, majd válassza ki **egyéni tartományok** hello lap hello lista aljáról.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. A hello **egyéni tartományok** panelen láthatja hello virtuális IP-címet. Mentse ezt az információt, a DNS-rekordok létrehozásához használandó
   
    ![](./media/custom-dns-web-site/virtual-ip-address.png)
   
   > [!NOTE]
   > Nem használható egyéni tartománynevek egy **szabad** webalkalmazás, és túl frissítse App Service-csomag hello**megosztott**, **alapvető**, **szabványos**, vagy **prémium** réteg. További információk az App Service hello terv tartozó tarifacsomagok, beleértve, hogyan toochange hello webalkalmazás tarifacsomag című [hogyan tooscale webalkalmazások](../articles/app-service-web/web-sites-scale.md).
   > 
   > 

