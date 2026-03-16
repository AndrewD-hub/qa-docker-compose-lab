Tema Docker Compose – Mediu de Testare Multi-Serviciu

# 1. Configurația mediului

Am creat un mediu multi-container folosind Docker Compose cu trei servicii:

- **db** – PostgreSQL (postgres:16-alpine)
- **cache** – Redis (redis:7-alpine)
- **adminer** – interfață web pentru administrarea bazei de date

Pentru baza de date au fost configurate variabilele de mediu:
- POSTGRES_USER=qa_user
- POSTGRES_PASSWORD=qa_pass
- POSTGRES_DB=qa_db

Porturi mapate:
- 5432 → PostgreSQL
- 8888 → Adminer

De asemenea, a fost definit un *volum persistent* pentru stocarea datelor bazei de date.

# 2. Orchestrarea serviciilor

Serviciul `adminer` pornește doar după ce serviciul `db` este healthy, folosind:

- `healthcheck` cu `pg_isready`
- `depends_on` cu `condition: service_healthy`

Pornirea mediului s-a făcut cu:
	**docker compose up -d**

Verificarea containerelor:
	**docker compose ps**
	
# 3. Validarea funcționalității

Am accesat interfața Adminer la: http://localhost:8888

Datele de conectare:

- System: PostgreSQL
- Server: db
- Username: qa_user
- Password: qa_pass
- Database: qa_db

După conectare am rulat comanda SQL:
	**CREATE TABLE tests (id INT);**
Tabelul a fost creat cu succes, demonstrând că mediul funcționează corect.

# 4. Resetarea mediului

Pentru a opri și curăța complet mediul am folosit comanda:
	**docker compose down -v**
Aceasta oprește containerele, șterge rețeaua creată de Docker Compose și elimină volumele persistente.

În fluxul QA această comandă este importantă deoarece permite resetarea completă a mediului de testare și eliminarea datelor rămase din testele anterioare.