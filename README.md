# CPaaS

## Uputstvo za probni deploy sa db u dockeru

### Sadrzaj

- Napravi se osnovni dir za projekat npr. ```cpaas```
- Iskopira se u njega .env fajl
   
   *(trenutno je .env isti kao onaj koji je javno dostupan u jasmin-web-panel repou ali cemo kasnije da ga menjamo)*
- U njemu se klonira ovaj repo, bulk-sms-jasmin, sa dir i fajlovima potrebnim za doker kontejner:
    ```
    git clone https://github.com/vladanan/bulk-sms-jasmin.git
    ```
- Zatim se u njemu kloniraju:
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

- nakon toga u osnovnom folderu ```docker compose up -d``` bi trebalo da obavi sve i da se u dockeru vide aktivni kontejneri

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


