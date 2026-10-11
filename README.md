<p align="center">
<img src="./assets/hero.svg" alt="Maruthi Sreeram, Java backend developer." width="100%">
<img src="./assets/marquee.svg" alt="Focus areas: distributed systems, microservices, event-driven architecture, system design, AI integration, secure APIs, root-cause debugging" width="100%">
</p>

<br>

<p align="center"><img src="./assets/h-01.svg" alt="01 About" width="100%"></p>

<p align="center">
Every app has a hidden half: the servers that store your data, check who you are and keep running when something breaks.<br>
<b>That hidden half is what I build.</b> Java and Spring Boot on the backend, B.Tech Computer Science, class of 2027,<br>
aiming for a Software Development Engineer role.
</p>

<p align="center"><img src="./assets/about.svg" alt="Headline numbers, four strengths (distributed systems, debugging by method, secure by design, AI with guardrails) and my loop: understand, build, break, improve." width="100%"></p>

<br>

<p align="center"><img src="./assets/h-02.svg" alt="02 Tech stack" width="100%"></p>

<p align="center"><img src="./assets/stack.svg" alt="Languages: Java, Python, JavaScript, TypeScript. Backend and security: Spring Boot, Spring Security, Hibernate, FastAPI, JWT. Data and messaging: MySQL, PostgreSQL, MongoDB, Redis, Kafka, RabbitMQ. Tooling: Docker, Git, GitHub, Postman, Linux, IntelliJ IDEA. Frontend: React, Vite. Currently learning distributed systems, system design and scalable systems." width="100%"></p>

<br>

<p align="center"><img src="./assets/h-03.svg" alt="03 Projects" width="100%"></p>

<p align="center"><img src="./assets/proj-1.svg" alt="CareerFlux: job boards are ingested, cleaned and stored; a resume is read by optional AI and approved by a human; a deterministic matcher then ranks jobs with strengths and gaps." width="100%"></p>

<p align="center">
<a href="https://github.com/maruthi-sreeram-p/careerFlux"><b>Source code</b></a>
&nbsp;·&nbsp;
<a href="https://github.com/maruthi-sreeram-p/careerFlux/blob/main/docs/architecture.md">Architecture notes</a>
</p>

<details>
<summary><b>Run CareerFlux</b> (Java 21, Maven 3.9, Node 20)</summary>

```bash
export CAREERFLUX_JWT_SECRET="$(openssl rand -base64 48)"
cd backend  && mvn spring-boot:run -Dspring-boot.run.profiles=dev,demo   # API on :8080
cd frontend && npm install && npm run dev                                # UI  on :5173
```

Run the test suite: `cd backend && mvn test`

</details>

<br>

<p align="center"><img src="./assets/proj-2.svg" alt="Nexus AI: four roles use a React app; a Spring Boot API with Spring Security and JWT checks the role, talks to the database, and passes AI requests through an explain-only guardrail to Gemini." width="100%"></p>

<p align="center">
<a href="https://github.com/maruthi-sreeram-p/medcare-nexus-ai"><b>Source code</b></a>
</p>

<details>
<summary><b>Run Nexus AI</b> (Java 21, Node 20)</summary>

```bash
cd backend  && mvn spring-boot:run          # API on :8088, Swagger UI at /swagger-ui.html
cd frontend && npm install && npm run dev   # UI  on :3000
```

Open `http://localhost:8088/swagger-ui.html` and call the same endpoint as two different roles to see access control at work.

</details>

<br>

<p align="center"><img src="./assets/proj-3.svg" alt="MediCare Tracker: a React app talks to a Spring Boot API backed by MySQL, plus a separate FastAPI service that calls an LLM. Dose status moves from upcoming to pending to on time, late or missed, decided by the server clock." width="100%"></p>

<p align="center">
<a href="https://github.com/maruthi-sreeram-p/online-medication-and-prescription-tracker"><b>Source code</b></a>
</p>

<details>
<summary><b>Run MediCare Tracker</b> (three terminals, MySQL running first)</summary>

```bash
mvn spring-boot:run                        # backend, :8321
uvicorn main:app --reload --port 8000      # AI service, :8000
npm install && npm start                   # frontend, :3000
```

</details>

<!-- TODO(maruthi): add a "verify" line for MediCare, e.g. one request/response showing a dose moving from PENDING to LATE. -->

<br>

<p align="center"><img src="./assets/proj-4.svg" alt="Distributed key-value store on Raft: a leader copies each write to followers and a majority confirms it. When the leader crashes, a follower times out, becomes a candidate, wins a majority vote and becomes the new leader." width="100%"></p>

<!-- TODO(maruthi): add the repo link and the one command that starts the cluster, then uncomment:

<p align="center">
<a href="https://github.com/maruthi-sreeram-p/REPO-NAME"><b>Source code</b></a>
</p>

<details>
<summary><b>Run the 3-node cluster</b></summary>

```bash
# command that starts the cluster
# steps: kill the leader, watch the election, confirm a write still replicates
```

</details>
-->

<br>

<p align="center"><img src="./assets/h-04.svg" alt="04 Activity" width="100%"></p>

<p align="center"><img src="./assets/activity.svg" alt="Commit activity heatmap for the last 26 weeks with my current streak, yearly total, busiest day and commits per month." width="100%"></p>

<p align="center"><sub>Drawn from my real GitHub contributions and refreshed daily by a GitHub Action.</sub></p>

<br>

<p align="center"><img src="./assets/h-05.svg" alt="05 Debugging labs" width="100%"></p>

<p align="center"><img src="./assets/labs.svg" alt="Two debugging labs. java-debugging-playground: 20 graded projects with 133 planted defects. spring-boot-debugging-laboratory: 15 Spring Boot apps from easy to brutal. Method: reproduce the bug, form a theory, change one thing, confirm the cause." width="100%"></p>

<p align="center">
<a href="https://github.com/maruthi-sreeram-p/java-debugging-playground"><b>java-debugging-playground</b></a>
&nbsp;·&nbsp;
<a href="https://github.com/maruthi-sreeram-p/spring-boot-debugging-laboratory"><b>spring-boot-debugging-laboratory</b></a>
</p>

<details>
<summary><b>Start a lab</b></summary>

```bash
# java-debugging-playground (JDK 17+, Docker from project 05)
cd 05-mysql-integration-debugging && docker compose up -d

# spring-boot-debugging-laboratory
docker compose -f infra/docker-compose.yml up -d mysql redis
```

</details>

<br>

<p align="center"><img src="./assets/h-06.svg" alt="06 Contact" width="100%"></p>

<p align="center">
<a href="https://maruthi-sreeram.vercel.app/"><img src="./assets/pill-portfolio.svg" alt="Portfolio" width="156"></a>
<a href="https://www.linkedin.com/in/maruthi-sreeram-b57680331/"><img src="./assets/pill-linkedin.svg" alt="LinkedIn" width="156"></a>
<a href="https://leetcode.com/u/PMaruthisreeram/"><img src="./assets/pill-leetcode.svg" alt="LeetCode" width="156"></a>
<a href="https://github.com/maruthi-sreeram-p"><img src="./assets/pill-github.svg" alt="GitHub" width="156"></a>
<a href="mailto:maruthisreeram864@gmail.com"><img src="./assets/pill-email.svg" alt="Email" width="156"></a>
</p>

<p align="center"><img src="./assets/footer.svg" alt="Understand the system. Build it. Break it. Improve it." width="100%"></p>
