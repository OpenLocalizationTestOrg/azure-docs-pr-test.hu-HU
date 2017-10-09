Minden ügyfélszámítógép, amely a tooa VNet pont-pont használatával telepített ügyféltanúsítvánnyal kell rendelkeznie. hello ügyféltanúsítvány hello legfelső szintű tanúsítvány jön létre, és minden ügyfélszámítógépen telepítette. Ha nincs telepítve érvényes ügyféltanúsítványt, és hello-ügyfél megkísérli tooconnect toohello hálózatok, a hitelesítés meghiúsul.

Az egyes ügyfelek vagy létrehozhat egy egyedi tanúsítványt, vagy használhatja ugyanazt a tanúsítványt több ügyfél számára hello. hello előny toogenerating egyedi ügyfél-tanúsítványok hello képességét toorevoke a rendszer egy tanúsítványt. Ellenkező esetben, ha több ügyfél azonos ügyféltanúsítvány használatával hello és toorevoke kell azt, hogy toogenerate és új tanúsítványok telepítését, az összes hello adott tanúsítvány tooauthenticate használó ügyfelek.

Ügyféltanúsítványok hello a következő módszerek segítségével hozhat létre:

- **Vállalati tanúsítvány:**

  - Ha egy vállalati tanúsítvány megoldást használ, hello közös érték formátuma ügyfél tanúsítványt létrehozni "name@yourdomain.com", hello "tartománynév\felhasználónév" formátum helyett.
  - Ellenőrizze, hogy hello ügyféltanúsítvány hello "User" tanúsítványsablont, amely rendelkezik ügyfél-hitelesítési hello hello használata lista első eleme alapul, nem pedig intelligens kártyás bejelentkezés, stb. Ehhez kattintson duplán a hello ügyféltanúsítványt, és tekintse meg ellenőrizheti a hello tanúsítvány **Részletek > kibővített kulcshasználat**.

- **Önaláírt főtanúsítvány:** fontos lépéseket hello alábbi hello P2S tanúsítvány cikkek egyikében. Ellenkező esetben hoz létre hello ügyféltanúsítványok nem kompatibilis a P2S-kapcsolatok és ügyfelek tooconnect megkísérlésekor hibaüzenetet kap. a következő cikkek hello valamelyikével hello lépéseket kompatibilis ügyfél tanúsítvány jön létre: 

  * [Windows 10 PowerShell-utasításokat](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert): ezek az utasítások a Windows 10 és a PowerShell toogenerate tanúsítványt igényel. hello tanúsítványokat, amelyek akkor jönnek létre, telepítheti bármely támogatott P2S-ügyfélen.
  * [MakeCert utasításokat](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): MakeCert használja, ha még nem rendelkezik hozzáféréssel tooa Windows 10 toouse toogenerate tanúsítványok. MakeCert elavult, de továbbra is használhatja a MakeCert toogenerate tanúsítványokat. hello tanúsítványokat, amelyek akkor jönnek létre, telepítheti bármely támogatott P2S-ügyfélen.

  Ügyféltanúsítvány használatával utasításokat megelőző hello származó önaláírt főtanúsítvány létrehozásakor automatikusan telepítve van, hogy használja-e toogenerate hello számítógépen azt. Ha azt szeretné, hogy egy másik ügyfélszámítógépen ügyféltanúsítványt tooinstall, tooexport kell azt a .pfx fájlhoz, hello teljes tanúsítványlánc együtt. Ezzel létrehoz egy hello legfelső szintű tanúsítvány szükséges információkat a hello ügyfél toosuccessfully hitelesítéséhez tartalmazó .pfx fájlt. Lépéseket tooexport egy tanúsítványt, lásd: [tanúsítványok – ügyfél-tanúsítvány exportálása](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport).
