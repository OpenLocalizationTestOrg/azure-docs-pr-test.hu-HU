### <a name="record-names"></a>Rekordnevek

Az Azure DNS-ben a rekordok relatív nevek használatával vannak meghatározva. A *teljesen minősített* tartománynevét (FQDN) tartalmazza hello zóna nevét, mivel egy *relatív* név azonban nem. Például hello relatív rekordnév a "www" a "contoso.com" hello zóna elnevezi hello teljesen minősített rekord "www.contoso.com".

Egy *legfelső pontján* rekord a DNS-rekord hello gyökérkönyvtárában (vagy *legfelső pontján*) a DNS-zónák. Például hello DNS zóna "contoso.com", egy csúcs-rekordot is rendelkezik hello teljesen minősített neve "contoso.com" (elnevezése egy *csupasz* tartományhoz).  Szabály szerint hello relatív neve "@" van használt toorepresent csúcs-rekordokat.

### <a name="record-types"></a>Rekordtípusok

Minden DNS-rekord rendelkezik névvel és típussal. Rekordok különféle típusokba toohello adatokat tartalmaznak szerint vannak rendszerezve. hello leggyakoribb típus az "A" rekord, amely egy IPv4-címet nevének tooan. Egy másik közös típus, az "MX" rekord, amely neve tooa levelezési kiszolgálóhoz.

Az Azure DNS minden gyakori DNS-rekord típust támogat: A, AAAA, CNAME, MX, NS, PTR, SOA, SRV és TXT. Vegye figyelembe, hogy az [SPF-rekordok TXT-rekordok használatával vannak jelölve](../articles/dns/dns-zones-records.md#spf-records).

### <a name="record-sets"></a>Rekordhalmazok

Néha kell toocreate egynél több DNS-rekord egy adott nevű és típusú. Tegyük fel, hogy hello "www.contoso.com" webhely két különböző IP-címek üzemelteti. hello webhelyhez két különböző A-rekordra összes IP-címe. Példa egy rekordhalmazra:

    www.contoso.com.        3600    IN    A    134.170.185.46
    www.contoso.com.        3600    IN    A    134.170.188.221

Az Azure DNS minden DNS-rekordot a *rekordhalmazok* használatával kezel. Rekordhalmaz (más néven a *erőforrás* rekordhalmaz) DNS-rekordok zónában, amelyek azonos nevet és -hello hello hello gyűjteménye ugyanarra a típusra. A legtöbb rekordhalmaz egyetlen rekordot tartalmaz. Azonban például hello a fenti példák rekord csoport több mint egy bejegyzést tartalmaz, nem ritka.

Például tegyük fel, hogy már létrehozott egy A rekordot "www" a "contoso.com" hello zóna, mutató toohello IP cím "134.170.185.46" (hello első rekordra fenti).  toocreate hello második rekordot kell felvenni, amely toohello meglévő rekord jegyezze be, nem pedig egy további rekordhalmaz létrehozása.

hello SOA és CNAME-rekordtípus jelentenek ez alól. hello DNS-szabványokból nem teszi lehetővé több rekordokat hello ezek a típusok neve megegyezik, ezért ezek rekordhalmazok is csak egyetlen rekordot tartalmaz.
