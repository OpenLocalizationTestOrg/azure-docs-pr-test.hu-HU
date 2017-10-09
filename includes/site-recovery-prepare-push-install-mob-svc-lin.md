### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Felkészülés leküldéses telepítésre Linux-kiszolgálón

1. Győződjön meg arról, hogy van-e hálózati kapcsolat hello Linux számítógép- és hello folyamatkiszolgáló között.
2. Hozzon létre egy fiókot, hogy hello folyamatkiszolgáló tooaccess hello számítógép használható. hello fióknak kell lennie egy **legfelső szintű** forráskiszolgálón hello Linux felhasználói. (Ez a fiók csak hello ügyfélleküldéses telepítés és használjon frissítéseket.)
3. Ellenőrizze, hogy hello forrás Linux-kiszolgáló rendelkezik bejegyzéseit, amelyek az összes hálózati adapter társított hello helyi állomásnevével tooIP címek leképezése hello/Etc/Hosts fájlt.
4. Hello legújabb openssh openssh-kiszolgáló és openssl-csomagok telepítése, amelyet az tooreplicate hello számítógépen.
5. Ügyeljen arra, hogy a Secure Shell (SSH) engedélyezve legyen, és a 22-es porton fusson.
6. Hello sshd_config fájl SFTP-alrendszer- és jelszóalapú hitelesítés engedélyezése:
  1.  Jelentkezzen be **gyökér** szintű felhasználóként.
  2.  A hello fájl/etc/ssh/sshd_config fájl keresése hello. sor, amelyek kezdődik **PasswordAuthentication**.
  3.  Állítsa vissza a hello sor, és módosítsa hello túl**Igen**.
  4.  Keresés hello kezdődő sort **alrendszer** és hello sor törölje.

     ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)
  5. Indítsa újra a hello **sshd** szolgáltatás.

7. A CSPSConfigtool hello fiók hozzáadása.
    1.  Jelentkezzen be tooyour konfigurációs kiszolgáló.
    2.  Nyissa meg a következőt: **cspsconfigtool.exe**. (Érhető el gyors hello asztalon és hello %ProgramData%\home\svsystems\bin mappában.)
    3.  A hello **fiókok kezelése** lapra, majd **fiók hozzáadása**.
    4.  Vegye fel a létrehozott hello fiókot. 
    5.  Adja meg a hello hitelesítő adatait használhatja, ha egy számítógép replikáció engedélyezése.
