---
title: "Azure-adatbázis a PostgreSQL adatkapcsolattárak |} Microsoft Docs"
description: "Ez a cikk több szalagtárak és illesztőprogramokat, amelyek a fejlesztők használhatják, amikor az alkalmazások tooconnect és lekérdezés az Azure Database kódolásának PostgreSQL ismerteti."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/15/2017
ms.openlocfilehash: 1f7234499d8abe37f8de9008e3158765b1fb0bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connection-libraries-for-azure-database-for-postgresql"></a>Adatkapcsolattárak PostgreSQL Azure-adatbázis
Ez a témakör a szalagtárak és illesztőprogramoknak a fejlesztők a programozási alkalmazások tooconnect és lekérdezés az Azure Database PostgreSQL sorolja fel.

## <a name="client-interfaces"></a>Ügyfél felületek
A legtöbb nyelvi ügyfél szalagtárak tooconnect tooPostgreSQL kiszolgáló külső projektek, és egymástól függetlenül küld. Ezek a Windows, Linux és Mac rendszerek támogatottak. Egy hello népszerű ügyfél illesztőprogramok tartalmazza:

| **Nyelv** | **Ügyféloldali felület** | **További információ** | **Letöltés** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | DB API 2.0-kompatibilis | [Letöltés](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://php.net/manual/en/book.pgsql.php) | Adatbázis bővítmény | [Telepítés](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [A védelmi npm csomag](https://www.npmjs.com/package/pg) | Tiszta JavaScript nem blokkoló ügyfél | [Telepítés](https://www.npmjs.com/package/pg) |
| Java | [JDBC](http://jdbc.postgresql.org/) | 4 típus JDBC-illesztőt | [Letöltés](https://jdbc.postgresql.org/download.html)  |
| Ruby | [A védelmi gem](https://deveiate.org/code/pg/) | Ruby felület | [Letöltés](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Indítás | [Csomag pq](https://godoc.org/github.com/lib/pq) | Tiszta Ugrás postgres illesztőprogram | [Telepítés](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](http://www.npgsql.org/) | Az ADO.NET-szolgáltató | [Letöltés](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | ODBC-illesztő | [Letöltés](http://www.postgresql.org/ftp/odbc/versions/) |
| C# | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | C nyelvi elsődleges felület | Tartalmazza |
| C++ | [libpqxx](http://pqxx.org/) | Új stílusú C++ felület | [Letöltés](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Következő lépések
Olvassa el ezen quickstarts hogyan tooconnect és lekérdezés Azure adatbázist PostgreSQL a választott nyelv használata:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [.NET (C#)](./connect-csharp.md)
