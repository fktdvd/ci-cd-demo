Értem, nézzük, hogyan lehet a README-t áttekinthetően, egységesen formázni Markdown-ban, hogy a kódblokkok, listák és alfejezetek szépen jelenjenek meg. Íme egy átfogó, formázott változat:

# CI/CD Demo – Docker + Jenkins + Python + Pytest

Ez a README teljes áttekintést ad a CI/CD demo projektről, amely Python kódot és teszteket tartalmaz, valamint Jenkins pipeline-t Docker környezetben.

---

## 📁 Projekt struktúra

ci-cd-demo/
├── Dockerfile
├── Jenkinsfile
├── README.md
├── requirements.txt
├── src/
│   ├── init.py
│   └── app.py
└── tests/
└── test_app.py

---

## 🔧 Telepítés

### 1. Docker telepítése Mac-en

Töltsd le és telepítsd a Docker Desktopot:  
[https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)

Indítás után ellenőrizd:

```bash
docker ps

Ha hibaüzenetet kapsz (Cannot connect to the Docker daemon), indítsd újra a Docker Desktopot.

⸻

🐍 Lokális teszt futtatása Python segítségével

A projekt root könyvtárából:

python -m pytest -v .

	•	-v → verbose output, minden teszt részletesen látszik
	•	. → a root könyvtárat jelenti, ahol a src/ és tests/ mappák találhatók

⸻

🐳 Lokális Docker teszt futtatás
	1.	Docker image építése:

docker build -t ci-demo .

	2.	Interaktív belépés a konténerbe:

docker run -it --rm ci-demo bash

	•	Most a /app könyvtárban vagy
	•	Ellenőrizheted a fájlokat: ls -R

	3.	Tesztek futtatása a konténerben:

python -m pytest -v .

	•	Ha a src modult nem találja, ellenőrizd a WORKDIR-t és a PYTHONPATH-ot:

export PYTHONPATH=/app:$PYTHONPATH
python -m pytest -v .


⸻

1️⃣ Docker image buildelése

docker build -t ci-demo .

	•	-t ci-demo → az image neve és tag-je
	•	. → a Dockerfile könyvtár helye
	•	Ez létrehozza a ci-demo image-t lokálisan

⸻

2️⃣ Lokális image-ek listázása

docker images

Vagy újabb formátumban:

docker image ls

	•	Látható a REPOSITORY, TAG, IMAGE ID, CREATED és SIZE
	•	Példa:

REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
ci-demo      latest    abc123def456   2 minutes ago  123MB

Fontos: docker ps vagy docker ps -a nem listázza az image-eket, csak a konténereket.

⸻

3️⃣ Új konténer indítása az image-ből

docker run -it --rm ci-demo bash

	•	-it → interaktív mód, terminál elérhető
	•	--rm → a konténer kilépéskor törlődik
	•	ci-demo → a futtatandó image neve
	•	bash → shell indítása a konténerben

Ellenőrzés a konténerben

ls -R
python -m pytest -v .

	•	Itt ellenőrizheted a projekt fájlokat és futtathatod a teszteket

⸻

4️⃣ Futtó / leállt konténerek listázása

docker ps       # csak a futó konténerek
docker ps -a    # minden konténer, futó és leállt

	•	A konténerek listája független a buildelt image-ek listájától
	•	Új docker run mindig létrehoz egy új példányt az image-ből

⸻

🏗️ Jenkins indítása Dockerben
	1.	Korábbi container leállítása (ha van):

docker stop jenkins_demo
docker rm jenkins_demo

	2.	Jenkins indítása Dockerben:

docker run -u root -d \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --name jenkins_demo \
  jenkins/jenkins:lts-docker

	•	-v /var/run/docker.sock:/var/run/docker.sock → Jenkins a host Docker daemon-t használja
	•	-u root → root jogosultság a socket használatához

	3.	Jenkins web UI:
http://localhost:8080￼

	•	Initial admin password lekérése:

docker logs jenkins_demo

Vagy containerbe lépve:

docker exec -it jenkins_demo bash
cat /var/jenkins_home/secrets/initialAdminPassword

	•	Telepítsd a javasolt pluginokat: Install suggested plugins

⸻

4️⃣ Tesztelés Jenkinsben

Interaktív shell a Jenkins containerben:

docker exec -it jenkins_demo bash

	•	Például futtathatsz Docker parancsokat:

docker ps
python -m pytest -v .


⸻

🟦 Jenkins pipeline futtatása
	1.	Pipeline létrehozása UI-ból:

	•	Jenkins → New Item → Pipeline → név: ci-cd-demo
	•	Pipeline definition: Pipeline script from SCM → Git repo URL → Branch: main → Script Path: Jenkinsfile
	•	Mentés → Build Now

	2.	Pipeline futtatása: UI-ból Build Now gomb

⸻

🔹 Megjegyzés
	•	Lokális fejlesztéshez push trigger nem szükséges, manuális build elég
	•	Verbose pytest output (-v) segít a Jenkins konzolban a hibák és teszteredmények részletes követésében
	•	Docker + Jenkins kombináció izolált, gyors és hordozható, tanuláshoz ideális

Ez a formázott változat **egységesen kódblokkokkal, listákkal, szekciókkal** van szerkesztve, így áttekinthető a Markdown megjelenítőben (pl. VS Code, GitHub).