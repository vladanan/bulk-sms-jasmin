# CPaaS

## Uputstvo za probni deploy u dockeru

### Sadrzaj

- Napravi se osnovni dir za projekat npr. ```cpaas```
- Iskopira se u njega .env fajl
  * ako se radi sa db u dokeru onda ostaje kakav jeste
  * ako se radi sa lokalnom db onda se u DEVDB_URL i PRODB_URL stavlja connection string za lokalnu db
   *(trenutno je ovaj .env fajl slican kao onaj koji je javno dostupan u jasmin-web-panel repou ali cemo kasnije da ga menjamo)*
- U istom dir se klonira ovaj repo, bulk-sms-jasmin, sa dir i fajlovima potrebnim za doker kontejner:
    ```
    git clone https://github.com/vladanan/bulk-sms-jasmin.git
    ```
    * nakon kloniranja ako se koristi lokalna db potrebno je izmeniti fajl ```docker-compose.local.db.yml``` tako da se u ```sms_logger``` odeljku izmene podaci u skladu sa pristupnim podacima za lokalnu db:

      ```
      DB_HOST: ${DB_HOST:-local_db_url}

      DB_DATABASE: ${DB_DATABASE:-jasmin} # ovo ostaje isto
      
      DB_TABLE: ${DB_TABLE:-submit_log} # ovo ostaje isto
      
      DB_USER: ${DB_USER:-local_db_user}
      
      DB_PASS: ${DB_PASS:-local_db_lozinka}
      ```

- Zatim se u taj dir kloniraju i:
  * jasmin:
    ```
    git clone https://github.com/jookies/jasmin.git
    ```
  * jasmin-web-panel:
    ```
    git clone https://github.com/101t/jasmin-web-panel.git
    ```
  
  (dok ih ne forkujemo ili promenimo)

### Aktivacija

- nakon toga doker komanda u osnovnom folderu  bi trebalo da obavi sve i da se u dockeru vide aktivni kontejneri:
    * ako se radi sa db u dokeru: ```docker compose up -d```
    * ako se radi sa lokalnom db: ```docker-compose -f docker-compose.local.db.yml up -d```

### Testiranje

- RESTful api:

  * ping test:
    ```
    curl --location 'http://127.0.0.1:8080/ping' --header 'Content-Type: application/json'
    ```
  * odgovor apija:
    ```
    {
        "data": "Jasmin/PONG"
    }
    ```

  * balance check:
    ```
    curl --location 'http://127.0.0.1:8080/secure/balance' --header 'Authorization: Basic YWRtaW46c2VjdXJl'
    ```
    ```
    {
      "message": "Authentication failure for username:admin"
    }
    ```

- HTTP api:

  * Checking account balance:
    ```
    curl --location 'http://127.0.0.1:1401/balance?username=jasmin_user&password=jasmin_pass'
    ```
    ```
    "Authentication failure for username:jasmin_user"
    ```

  * Checking rate price:
    ```
    curl --location 'http://127.0.0.1:1401/rate?to=235423523532&from=3542352352&coding=0&username=admin&password=secret&content=Hello%20world%20!'
    ```
    ```
    "Authentication failure for username:admin"
    ```

- Web panel

  * url:
    ```
    http://localhost:8999/
    ```
  * user: ```admin```, pass: ```secret```


