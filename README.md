# SonarQube with Docker

This document explains how to set up and run SonarQube using Docker to analyze the code quality of a .NET project and integrate it into a basic workflow.

---

## **Prerequisites**

- Docker and Docker Compose installed on your machine.
- .NET project configured with unit tests.
- Internet connection.

---

## **1. Setting Up Docker for SonarQube**

SonarQube requires two containers: one for the SonarQube server and another for the PostgreSQL database. We'll use Docker Compose to simplify the setup.

### **Step 1: Create the `docker-compose.yml` file**

Create a `docker-compose.yml` file with the following content:

```yaml
version: '3.8'

services:
  sonarqube:
    image: sonarqube:community
    container_name: sonarqube
    ports:
      - "9000:9000"
    environment:
      - SONAR_JDBC_URL=jdbc:postgresql://sonarqube_db:5432/sonar
      - SONAR_JDBC_USERNAME=sonar
      - SONAR_JDBC_PASSWORD=sonar
    depends_on:
      - sonarqube_db

  sonarqube_db:
    image: postgres:14
    container_name: sonarqube_db
    environment:
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=sonar
      - POSTGRES_DB=sonar
    volumes:
      - sonarqube_db_data:/var/lib/postgresql/data

volumes:
  sonarqube_db_data:
```

---

## **2. Running SonarQube with Docker**

### **Step 1: Start the containers**

Run the following command in the folder where the `docker-compose.yml` file is located:

```bash
docker-compose up -d
```

This command:
- Downloads the necessary images for SonarQube and PostgreSQL.
- Creates and runs the containers in the background.

### **Step 2: Verify that SonarQube is running**

1. Open your browser and go to [http://localhost:9000](http://localhost:9000).
2. Log in with the default credentials:
   - Username: `admin`
   - Password: `admin`
3. Change the default password to a new one.

---

## **3. Configuring SonarQube for Your Project**

### **Step 1: Create a new project**

1. In the SonarQube dashboard, go to "Projects" > "Create Project".
2. Enter a name for your project (e.g., `CSharp-UnitTesting-Basics`).
3. Generate an access token and save it. You'll need it later.

### **Step 2: Configure the analysis**

Create a `sonar-project.properties` file in the root of your repository with the following content:

```properties
sonar.projectKey=CSharp-UnitTesting-Basics
sonar.host.url=http://localhost:9000
sonar.login=YOUR_GENERATED_TOKEN

sonar.sources=UnitTestingBasics
sonar.tests=Tests
sonar.test.inclusions=Tests/**/*.cs
sonar.language=cs
sonar.dotnet.visualstudio.solution.file=UnitTestingBasics.sln
```

Replace `YOUR_GENERATED_TOKEN` with the token you generated earlier.

---

## **4. Analyzing Your .NET Project**

We'll use the SonarScanner to analyze the code.

### **Step 1: Install the SonarScanner**

1. Install the global tool:

   ```bash
   dotnet tool install --global dotnet-sonarscanner
   ```

2. Verify that it's installed correctly:

   ```bash
   dotnet sonarscanner --version
   ```

### **Step 2: Run the analysis**

1. Start the analysis:

   ```bash
   dotnet sonarscanner begin /k:"CSharp-UnitTesting-Basics" /d:sonar.login="YOUR_GENERATED_TOKEN" /d:sonar.host.url="http://localhost:9000"
   ```

2. Build your solution:

   ```bash
   dotnet build UnitTestingBasics.sln
   ```

3. Run tests with coverage:

   ```bash
   dotnet test --collect:"Code Coverage"
   ```

4. Finish the analysis and send the data to SonarQube:

   ```bash
   dotnet sonarscanner end /d:sonar.login="YOUR_GENERATED_TOKEN"
   ```

---

## **5. Automating with GitHub Actions**

You can integrate this workflow into GitHub Actions to automatically run the analysis on every change to the `main` branch.

### **Workflow File: `.github/workflows/sonar.yml`**

```yaml
name: SonarQube Analysis

on:
  push:
    branches:
      - main

jobs:
  sonarQube:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: 7.0

      - name: Install SonarScanner for .NET
        run: dotnet tool install --global dotnet-sonarscanner

      - name: SonarQube Begin
        run: dotnet sonarscanner begin /k:"CSharp-UnitTesting-Basics" /d:sonar.login="${{ secrets.SONAR_TOKEN }}" /d:sonar.host.url="http://localhost:9000"

      - name: Build Solution
        run: dotnet build UnitTestingBasics.sln

      - name: Run Tests with Coverage
        run: dotnet test --collect:"Code Coverage"

      - name: SonarQube End
        run: dotnet sonarscanner end /d:sonar.login="${{ secrets.SONAR_TOKEN }}"
```

### **Set the Token in GitHub**

1. Go to your GitHub repository.
2. Navigate to "Settings" > "Secrets and variables" > "Actions".
3. Create a new secret called `SONAR_TOKEN` and paste the token generated in SonarQube.

---

## **6. Viewing Results**

1. After running the analysis, check the results in the SonarQube dashboard.
2. Observe metrics such as:
   - Test coverage.
   - Bugs, vulnerabilities, and code smells.
   - Code duplication.

