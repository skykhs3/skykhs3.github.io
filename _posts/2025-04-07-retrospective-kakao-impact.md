---
title: "Retrospective of Kakao Impact"
date: 2025-04-07 22:00:00 +09:00
categories: [Retrospective]
post: skykhs3
math: true
mermaid: true
image:
  path: /assets/img/posts/2025-04-07-retrospective-kakao-impact/award.webp
  alt: Winning Technology Innovation Award
  show_in_post: true
tags:
  - tutorial
  - typescript
  - express
description: My team and I won the technology innovation award from Kakao Impact. Kakao Impact is a project aimed at solving social issues around us.
---

<div markdown="1">

## 1. Overview
   
We proposed the project called **"Solar-See"**. "Solar-See" is an AI-based game and mapping service developed to identify **non-metered solar panels** and improve the accuracy of **power demand forecasting**.

### Link
[**GitHub Repository**](https://github.com/techforimpact-archive/TFI_CAMPUS_KAIST_24fall_Solar-See){:target="_blank"}

[**Demo**](/assets/img/posts/2025-04-07-retrospective-kakao-impact/demo-small.mp4){:target="_blank"}

[**Presentation**](/assets/img/resume/solar_see_final_presentation.pdf){:target="_blank"}


## 2. My role and Responsibilities
I developed both the **frontend(React)** and **backend(Django)**, deployed the services(Nginx), and fine-tuned an **image segmentation model(Ultralytics YOLO11)**.

## 3. Architecture

### 3.1. Training Model

```mermaid
flowchart TD
    A[National Geographic Information Institute] -->|Aerial Photos| B[GPU Server]

```

### 3.2. Pre Processing
```mermaid
flowchart TD
    A[National Geographic Information Institute] -->|Aerial Photos| B[Trained AI Model]
    A[National Geographic Information Institute] -->|Metadata| C[Coordinate Converter]
    B[Trained AI Model] -->|Solar panel x,y pixel coordinates on photos| C[Coordinate Converter]
    C[Coordinate Converter] -->|Solar panel longitude, latitude| E[Database; SQLite]
```

### 3.3. Service
```mermaid
flowchart TD
    A[Client] <-->  B
    subgraph Personal Server 
        B[Nginx] <--> C[Bundled Frontend Files]
        B <--> D[Gunicorn]
        D <--> E[Django]
        E <--> F[SQLite]
        B <--> G[Aerial Images]
    end

```

## 3. Tech Stacks

We chose React and Django + Django REST Framework because our team is already familiar with these technologies. We saved over 50GB of images for training the model and developing the service.

### 3.1. Django REST Framework
**Pros:**
- Many AI modules are available in Python
- Built-in ORM (Django ORM) and built-in JSON conversion (serializers)
- Auto-generated admin panel.

**Cons:**
- Limited flexibility

### 3.2. Nginx
- Its asynchronous, event-driven architecture makes it highly efficient at handling concurrent connections. It's especially fast at serving static files.

### 3.2.1. High-level features (HTTP optimizations)
**1. gzip / brotli – Compression**

These compress your static files (like HTML, CSS, JS) before sending them to the browser.<br/>
Smaller file size = faster loading.<br/>
brotli is newer and usually compresses better than gzip.<br/>

**2. ETag – File version checker**<br/>
It’s like a unique ID for each version of a file.<br/>
If the file hasn’t changed, the browser can use the cached version instead of downloading again.<br/>

**3. Last-Modified – Timestamp of the file**<br/>
The browser asks: “Has this file been updated since last time?”<br/>
If not, the server just says "No need to re-download" (sends 304 Not Modified).<br/>

### 3.2.2. Low-level features (System-level speed boosts)
**1. sendfile – Skip the middle step**<br/>
Normally, files go from disk → memory → network.<br/>
With sendfile, the OS sends the file directly from disk to network.<br/>
This saves time and CPU.<br/>

**2. mmap – Map file into memory**<br/>
Makes file access much faster by mapping it directly into memory.<br/>
Useful when serving the same files repeatedly.<br/>

**3. zero-copy I/O – No copying at all**<br/>
It avoids copying data between parts of the system (like memory and disk).<br/>


### 3.3. SQLite
**Pros:**

- Django supports SQLite as the default database.

- It's lightweight and file-based, making it easy to share data.

**Cons:**

- Lacks concurrency support. Only one process can modify the database at a time.

- As a serverless RDBMS, it cannot be accessed directly from external networks.

### 3.4. Ultralytics
**Pros:**

- If the training data is well-labeled, fine-tuning can be done easily with just a few commands.
- This framework has good pre-trained models.

### 3.4.1. How to fine-turn model?
To be written...

## 4. Gunicorn?? It's weird for me.

This was my first time using Django, and **I initially didn’t understand why a separate component like Gunicorn was needed for deployment**.
In Node.js, I was used to creating standalone HTTP servers, so simply porting it behind Nginx was enough.

However, Django introduces a unique concept: the separation between the **WSGI (Web Server Gateway Interface) server** and the **WSGI(web server gateway interface) application**

### 4.1. WSGI Server, WSGI Application

- The WSGI application handles business logic.

- The WSGI server is responsible for multiprocessing and threading.

This separation of concerns is a good design principle. It allows the WSGI application to be swapped out easily (e.g., from WSGI to ASGI).


### 4.2.  Node.js vs Django + DRF

For example, if you want to use HTTP/2 in Node.js, you'd need to import the http2 module and change the code accordingly.
In Django, on the other hand, you just need to switch the WSGI server.

|                   | Node.js                     | Django                           |
| ----------------- | --------------------------- | -------------------------------- |
| Self-HTTP Server  | O (default built-in)        | X (external server required)     |
| Design Philosophy | Server + Runtime All-in-One | App Logic Only, Server is Needed |
| Deployment        | simple and low flexibility  | complex and high flexibility     |

### 4.3. Spring Boot Application vs Django


| Concept                    | Spring Boot Application                                   | Django                                                      |
| -------------------------- | --------------------------------------------------------- | ----------------------------------------------------------- |
| Web Application            | Spring Boot application                                   | Django project                                              |
| Web Server / HTTP Handler  | Embedded Tomcat (included by default)                     | External Gunicorn (requires separate setup)                 |
| Request Handling & Routing | Tomcat receives HTTP requests and forwards them to Spring | Gunicorn receives HTTP requests and forwards them to Django |
| Replaceable?               | Can be replaced with Jetty, Undertow, etc.                | Can be replaced with uWSGI, Daphne, etc.                    |


### 5. Points to improve
It ended with the development of MVP without considering the situation where there are many users. If there are many users, each service will be converted into a container and the following architecture will be considered.

```mermaid
flowchart TD
    A[Client Request] --> N[Nginx]

    N --> C1
    N --> C2
    N --> C3[Bundled Frontend Files]

    subgraph Container_1 [Uvicorn Container 1]
        C1[Uvicorn Instance 1]
        C1 --> W1[ASGI Worker]
        W1 --> D1[Django ASGI App]
    end

    subgraph Container_2 [Uvicorn Container 2]
        C2[Uvicorn Instance 2]
        C2 --> W2[ASGI Worker]
        W2 --> D2[Django ASGI App]
    end

    D1 --> E1[Database Server]
    D2 --> E1[Database Server]

```
