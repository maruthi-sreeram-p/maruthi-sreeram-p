# Maruthi Sreeram
 
Java backend engineer in progress. I build systems, break them on purpose, and write down what failed.
 
```diff
+ target:  Java Backend Engineer / Software Development Engineer
+ streak:  38 consecutive commit days (Sep 4 - Oct 11, 2026)
```
 
---
 
## /whoami
 
```yaml
name:      Maruthi Sreeram
role:      Java Backend Developer
education: B.Tech Computer Science, class of 2027
focus:     [backend engineering, distributed systems, microservices, event-driven architecture, AI integration]
 
stack:
  language:   [Java, Python, JavaScript, TypeScript]
  backend:    [Spring Boot, Spring Security, Spring Data JPA, Hibernate, Spring AI, Maven]
  data:       [MySQL, PostgreSQL, MongoDB, Redis]
  messaging:  [Kafka, RabbitMQ]
  tooling:    [Docker, Git, Postman, Linux, IntelliJ IDEA]
  frontend:   [React, Vite]
 
learning:  distributed systems, system design, scalable backend architecture
principle: "Understand the system. Build it. Break it. Improve it."
```
 
---
 
## /registry
 
Four services, ordered by how hard they were to get right. Bars show relative difficulty.
 
```text
distributed-kv-store   ##########  consensus has to stay correct while nodes fail
careerflux             #########.  widest system: 12 backend modules, ingestion to matching
nexus-ai               ########..  four roles, stock forecasting, an LLM with hard safety limits
medicare-tracker       ######....  three services, role-based JWT, server-side dose logic
```
 
| # | service | why it ranks here | stack | source |
|---|---------|-------------------|-------|--------|
| 1 | distributed-kv-store | Leader election, replicated logs and quorum commits must hold under node failure | Java, Spring Boot, Raft | TODO add repo link |
| 2 | careerflux | Source intelligence, job ingestion with dedup, deterministic matching, security, migrations | Spring Boot, React, PostgreSQL, Kafka (optional), Spring AI | [careerFlux](https://github.com/maruthi-sreeram-p/careerFlux) |
| 3 | nexus-ai | Four role-based workflows plus an AI layer that may explain prescriptions but never prescribe | Spring Boot 3, Spring Security, JWT, React, Gemini | [medcare-nexus-ai](https://github.com/maruthi-sreeram-p/medcare-nexus-ai) |
| 4 | medicare-tracker | Prescription slots, dose status computed on the server, a separate AI service | Spring Boot, MySQL, React, FastAPI | [online-medication-and-prescription-tracker](https://github.com/maruthi-sreeram-p/online-medication-and-prescription-tracker) |
 
---
 
## /services
 
<details open>
<summary><b>01 &middot; distributed-kv-store</b> &mdash; consensus under failure</summary>
**problem**
A single-node store loses data when it crashes. This one replicates every write across nodes and keeps serving when a node dies.
 
**topology** (3-node example)
 
```text
client --> leader --append--> log
              |-----------> follower A --ack-->|
              |-----------> follower B --ack-->|
          majority ack  =>  commit  =>  apply to store
 
leader stops sending heartbeats  =>  follower times out  =>  election  =>  new leader
```
 
**decision**
A write is acknowledged only after a majority of nodes have it (quorum commit).
 
**trade-off**
No majority, no progress. The store favors consistency over availability, so a partitioned minority stops accepting writes.
 
**run**
 
```bash
# TODO add the command that starts the cluster
```
 
**verify**
 
```text
# TODO add the steps: kill the leader, show the election, show a write still replicating
```
 
</details>
<details>
<summary><b>02 &middot; careerflux</b> &mdash; widest system</summary>
**problem**
Job seekers cannot tell which employers matter for a target career, whether those sources may be read, or why a role fits. CareerFlux finds the sources, checks access policy, monitors them for change, and explains every match.
 
**topology**
 
```text
ATS sources --> ingestion --> normalize --> dedupe --> enrich --> jobs
                (optional Kafka bus)                                |
resume --> AI extract --> proposals --> human review --> profile ---+
                                                                    v
                                    deterministic matcher (5 weighted sub-scores)
                                                                    v
                                         ranked jobs + strengths + gaps
```
 
**decision**
AI proposes, humans decide. Matching and eligibility stay in deterministic Java. AI is optional and falls back to rule-based logic when it is off.
 
**trade-off**
A rule-based matcher adapts less than a learned ranker. In return, every score is explainable and testable.
 
**run**
 
```bash
export CAREERFLUX_JWT_SECRET="$(openssl rand -base64 48)"
cd backend  && mvn spring-boot:run -Dspring-boot.run.profiles=dev,demo   # :8080
cd frontend && npm install && npm run dev                                # :5173
```
 
**verify**
 
```bash
cd backend && mvn test
```
 
Design notes: `docs/architecture.md`
 
</details>
<details>
<summary><b>03 &middot; nexus-ai</b> &mdash; role-based healthcare platform with a guarded AI layer</summary>
**problem**
Medication adherence, prescriptions and pharmacy stock live in separate places, and AI help in healthcare needs hard limits. Nexus AI covers physicians, patients, pharmacy staff and admins in one platform, in six languages.
 
**topology**
 
```text
React (Redux, MUI) --> Spring Boot (Spring Security 6, JWT)
                           |-- JPA / Hibernate --> H2 | MySQL
                           '-- GeminiAiService --> Google Gemini API
 
roles: physician | patient | pharmacy | admin
```
 
**decision**
The AI explains existing prescriptions only. It does not diagnose new conditions or change dosages.
 
**trade-off**
A narrower assistant than an open-ended chatbot, in exchange for safe behavior in a medical setting.
 
**run**
 
```bash
cd backend  && mvn spring-boot:run     # :8088, Swagger UI at /swagger-ui.html
cd frontend && npm install && npm run dev   # :3000
```
 
**verify**
Open `http://localhost:8088/swagger-ui.html` and call the same endpoint as two different roles.
 
</details>
<details>
<summary><b>04 &middot; medicare-tracker</b> &mdash; prescriptions and dose tracking</summary>
**problem**
Patients miss doses. MediCare gives doctors, patients, staff and admins role-based dashboards, prescriptions split into morning, afternoon and night slots, and a chatbot that reads the patient's prescription data.
 
**topology**
 
```text
React :3000 --> Spring Boot :8321 --> MySQL :3306 (11 tables)
                     '--> FastAPI :8000 --> Groq (Llama 3.1 8B)
 
auth: JWT + BCrypt     roles: doctor | patient | staff | admin
```
 
**decision**
Dose status (UPCOMING, PENDING, ON TIME, LATE, MISSED) is computed from the backend clock, not the client's.
 
**trade-off**
The server clock becomes the source of truth, so time zones and clock drift have to be handled on the server.
 
**run**
 
```bash
# three terminals, MySQL running first
mvn spring-boot:run                        # backend
uvicorn main:app --reload --port 8000      # AI service
npm install && npm start                   # frontend
```
 
**verify**
TODO add a request and response that shows a dose moving from PENDING to LATE.
 
</details>
---
 
## /labs
 
Practice labs, not products. Each one is a set of Spring Boot apps with planted bugs, built to practice root-cause analysis instead of symptom fixing.
 
| lab | what it trains | size | start |
|-----|----------------|------|-------|
| [java-debugging-playground](https://github.com/maruthi-sreeram-p/java-debugging-playground) | MVC, DI, JPA, transactions, security, JWT, Redis, Kafka, RabbitMQ | 20 graded projects, 133 planted defects | `cd 05-mysql-integration-debugging && docker compose up -d` |
| [spring-boot-debugging-laboratory](https://github.com/maruthi-sreeram-p/spring-boot-debugging-laboratory) | Production-style bugs: locking, caching, messaging, auth, concurrency | 15 apps, difficulty 2/10 to 10/10 | `docker compose -f infra/docker-compose.yml up -d mysql redis` |
 
```text
method:  reproduce first -> form a theory -> change one thing at a time
log:     symptom -> hypothesis -> what ruled it out -> what confirmed it
rule:    read the answer key last
```
 
---
 
## /contact
 
| | |
|---|---|
| portfolio | [maruthi-sreeram.vercel.app](https://maruthi-sreeram.vercel.app/) |
| linkedin | [maruthi-sreeram-b57680331](https://www.linkedin.com/in/maruthi-sreeram-b57680331/) |
| leetcode | [PMaruthisreeram](https://leetcode.com/u/PMaruthisreeram/) |
| github | [maruthi-sreeram-p](https://github.com/maruthi-sreeram-p) |
| email | maruthisreeram864@gmail.com |
 
<sub>last updated 2026-10-11</sub>
 
