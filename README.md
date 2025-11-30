# **Enterprise-Grade Docker Compose Environment for Oracle ADB Free 26ai & JasperReports Server 8.2**

A fully engineered, production-style container environment that
seamlessly integrates **Oracle Autonomous Database Free (26ai)** with
**JasperReports Server 8.2**.\
Designed for developers, data engineers, and reporting teams who require
a reliable, scalable, and secure local environment for building
enterprise applications, analytics, or APEX-driven reporting solutions.

------------------------------------------------------------------------

## 🎯 **Purpose of This Environment**

This setup provides:

-   A complete **local Oracle ADB experience** (SQL, REST, Mongo APIs).
-   A fully functional **JasperReports Server** for report design,
    scheduling, and export.
-   Persistent storage across database and report servers.
-   A stable structure suitable for **APEX**, **ORDS**, and **enterprise
    reporting workflows**.
-   Clean, organized configuration following DevOps best practices.

------------------------------------------------------------------------

## 🏗️ **Architecture Overview**

    +-------------------------+         +----------------------------+
    |  Oracle ADB Free 26ai   | <-----> | JasperReports Server 8.2   |
    |   - ATP workload        |         |   - Reporting engine       |
    |   - SQL / REST / Mongo  |         |   - Web UI                 |
    |   - Persistent volumes  |         |   - Scheduled jobs         |
    +-------------------------+         +----------------------------+

           Shared External Docker Network (oracle-adb-network)

------------------------------------------------------------------------

## 🗄️ **1. Oracle ADB Free 26ai Service**

A fully containerized version of Oracle's Autonomous Database (Free
tier), ideal for development, learning, and integration.

### **Features**

-   Autonomous Transaction Processing (ATP) workload
-   Secure wallet-based authentication
-   Exposed ports for SQL, REST, and Mongo APIs
-   Full data persistence using named Docker volumes
-   Auto-restart for long-running development sessions

### **Exposed Ports**

  Host    Container   Purpose
  ------- ----------- ----------------------------
  1521    1522        SQL\*Net (Oracle Database)
  1522    1522        Alternate SQL port
  8443    8443        HTTPS Admin
  27017   27017       Mongo-compatible API
  8888    8888        Utility services

### **Environment Variables**

-   `WORKLOAD_TYPE=ATP`
-   `WALLET_PASSWORD=Malek7788Wallet`
-   `ADMIN_PASSWORD=Malek7788Adb`

> ⚠️ **Always change these before production use.**

### **Persistent Volumes**

-   `oracle-adb-data` → Database storage
-   `oracle-adb-logs` → Diagnostic logs
-   `/home/eng-malek/oracle_wallet` → Oracle Wallet mount

------------------------------------------------------------------------

## 🗃️ **2. JasperReports Server 8.2 Service**

A powerful enterprise reporting engine capable of generating **PDF,
Excel, Word, HTML** and more --- fully integrated with databases via
JDBC or REST.

### **Features**

-   Web-based report designer
-   Role-based access control
-   Advanced scheduling system
-   Integration with Oracle ADB
-   Export to multiple formats
-   REST API for automation

### **Typical Ports**

  Host   Container   Purpose
  ------ ----------- -------------------------------------
  8080   8080        JasperReports Web UI
  5432   5432        Internal DB (depends on base image)

> If your exact container uses different ports, I can update them for
> you.

------------------------------------------------------------------------

## 🔗 **Docker Network**

Both containers communicate through an external, pre-created Docker
network:

``` yaml
networks:
  oracle-adb-network:
    external: true
```

Create it (if not already created):

``` bash
docker network create oracle-adb-network
```

------------------------------------------------------------------------

## 💾 **Persistent Volumes Setup**

For clean and reliable data storage:

``` bash
docker volume create oracle-adb-data
docker volume create oracle-adb-logs
docker volume create jasperreports_data
```

------------------------------------------------------------------------

## ▶️ **How to Use**

### **Start all services**

``` bash
docker-compose up -d
```

### **Stop and remove containers**

``` bash
docker-compose down
```

### **Tail logs**

``` bash
docker logs -f oracle-adb-26ai
```

------------------------------------------------------------------------

## 📌 **Recommended Folder Structure**

    project-root/
    │
    ├── docker-compose.yaml
    ├── README.md
    └── wallet/  (Oracle Wallet)

------------------------------------------------------------------------

## 📝 **Best Practices**

-   Use **strong passwords** and rotate wallet files regularly.
-   Never expose database ports directly on production networks.
-   Use Nginx or Traefik if exposing JasperReports externally.
-   Keep volumes managed and monitored.

------------------------------------------------------------------------

## 👤 **Author**

**Eng. Malek Al-edresi**
Oracle APEX & Database Developer
AI Vector Search • Flutter • DevOps Tools



