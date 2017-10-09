Tartománynévrendszer (DNS) hello használt toolocate dolgokat hello az interneten. Például amikor adjon meg egy címet a böngészőben, vagy egy hivatkozásra egy weblap, az DNS-tootranslate hello tartományhoz tartozó IP-címet. hello IP-cím rendezésére a hasonlít egy utca, házszám, de nem nagyon emberi leíró. Például sokkal könnyebb tooremember egy DNS-nevet, például a **contoso.com** , mint az tooremember például 192.168.1.88 vagy 2001:0:4137:1f67:24a2:3888:9cce:fea3 IP-címet.

DNS-rendszerében hello alapuló *rekordok*. Rekordok társítson egy adott *neve*, például a **contoso.com**, IP-címet vagy egy másik DNS-nevét. Egy alkalmazás, például egy webes böngésző, egy nevet a DNS-ben, keres hello rekord keresése, és használja a függetlenül mutat tooas hello cím. Ha a hello érték azt pontok toois IP-címet, hello böngésző ezt az értéket fogja használni. Mutat tooanother DNS-nevét, majd hello alkalmazásnak toodo feloldási újra. Végső soron minden névfeloldás véget ér az IP-címet.

Amikor egy Azure-webhelyet hoz létre, a DNS-név automatikusan toohello hely. Ez a név veszi hello formája  **&lt;yoursitename&gt;. azurewebsites.net**. Ha a webhely egy Azure Traffic Manager-végpontot, a webhely keresztül érhető el majd hello  **&lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** tartomány.

> [!NOTE]
> Ha a webhely egy Traffic Manager-végpont van konfigurálva, szüksége lesz a hello **. trafficmanager.net** címet a DNS-rekordok létrehozásához.
> 
> Csak használhatja a CNAME-rekordok a Traffic Managerrel
> 
> 

Van még több típusú bejegyzés – mindegyiket a saját funkciók és -korlátozások, de a titkosításra konfigurált webhelyek tooas Traffic Manager-végpontot, csak ügyelünk egy; *CNAME* rögzíti.

### <a name="cname-or-alias-record"></a>CNAME- vagy aliasnév rekord
A CNAME rekord rendeli hozzá a *adott* DNS-nevével, például a **mail.contoso.com** vagy **www.contoso.com**, tooanother (kanonikus) tartomány nevét. A Traffic Manager használata Azure Websitesra hello esetben hello kanonikus tartománynév megadása hello  **&lt;myapp >. trafficmanager.net** a Traffic Manager-profil tartománynevére. Létrehozása után hello CNAME létrehoz egy aliast hello  **&lt;myapp >. trafficmanager.net** tartomány nevét. hello CNAME bejegyzés megoldja toohello IP-címét a  **&lt;myapp >. trafficmanager.net** tartománynév automatikusan, így ha megváltozik a hello IP-cím hello webhely, nem kell tootake bármilyen műveletet.

Forgalom a Traffic Manager megérkeznek, majd hello forgalom tooyour webhelyén, a terheléselosztás hello módszer van konfigurálva irányítja. Ez a teljes mértékben transzparens toovisitors tooyour webhelyet. Hello egyéni tartománynév a böngészőbe csak akkor jelenik meg.

> [!NOTE]
> Néhány tartomány regisztráló szervezetek csak teszi toomap altartományok, mint egy olyan CNAME rekordot használatakor **www.contoso.com**, és nem neveket, többek között a **contoso.com**. A regisztráló által biztosított hello dokumentációjában talál további információt a CNAME-rekordot, <a href="http://en.wikipedia.org/wiki/CNAME_record">Wikipedia bejegyzés a CNAME rekord hello</a>, vagy hello <a href="http://tools.ietf.org/html/rfc1035">IETF tartománynév - megvalósítása és meghatározása</a> a dokumentum.
> 
> 

