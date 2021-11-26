
Quick reference
--

-	**Maintained by**:  
	[MriiSoft LLC](https://www.mriisoft.com/company)

Environment Variables
--

### `DB_USERNAME`
This environment variable is required for you to use the PostgreSQL image. It must not be empty or undefined.

### `DB_PASSWORD`
This environment variable sets the superuser password for PostgreSQL. The default superuser is defined by the `DB_USERNAME` environment variable.

### `JWT_USERNAME`
This environment variable is required for you to use the OBackup image. It must not be empty or undefined.

### `JWT_USERNAME`
This environment variable sets the superuser password for OBackup. The default superuser is defined by the `JWT_USERNAME` environment variable.

Quickstart
--

```shell
version: '3.8'

services:

  api:
    image: mriisoft/obackup-api:latest
    environment:
      - ASPNETCORE_ENVIRONMENT=Production
      - ASPNETCORE_URLS=https://+:4431

      - WEBAPI_URL=https://localhost:4431
      - WEBCLIENT_URL=https://localhost:4432
      
      # disable cors policy, default: false;
      - DISABLE_CORS=true
      
      # database type:
      ## 1 - PostgreSQL
      ## 2 - SQLServer
      - DB_TYPE=1
      
      # database host
      - DB_HOST=postgres-db
      
      # database port, only for PostgreSQL
      - DB_PORT=5432
      
      - DB_USERNAME=postgres
      - DB_PASSWORD=admin
      
      - JWT_USERNAME=admin
      - JWT_PASSWORD=admin
    ports:
      - "4431:4431"
    volumes:
      - ./obackup-docker-data/obackup:/opt/MriiSoft/OBackup
    depends_on:
      - "postgres-db"      

  webclient:
    image: mriisoft/obackup-ui:latest
    depends_on:
      - api
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:4432

      - WEBAPI_URL=https://localhost:4431
      - WEBCLIENT_URL=https://localhost:4432

    ports:
      - "4432:4432"
    volumes:
      - ./obackup-docker-data/obackup:/opt/MriiSoft/OBackup
    depends_on:
      - "api"      
      
  postgres-db:
    image: postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: admin
    ports:
      - "5432:5432"
    volumes:
      - ./obackup-docker-data/postgres-data:/var/lib/postgresql/data       

```

# License

View [license information](https://www.mriisoft.com/license-agreement/) for the software contained in this image.

For getting Free License for 10 users you can register on [MriiSoft license portal](https://portal.mriisoft.com/).

As with all Docker images, these likely also contain other software which may be under other licenses (such as Postgres, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.
