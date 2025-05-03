---
title: "Understanding Cookie Attributes and Security"
date: 2025-03-19 17:56:00 +09:00
categories: [Development, Security]
author: skykhs3
image:
  path: /assets/img/posts/2025-03-19-cookie-attributes/cookie.webp
  alt: Cookie
  show_in_post: false
tags:
  - web security
  - cookies
  - http
  - web development
  - cybersecurity
  - browser security
description: Cookies have several attributes that control how they behave in the browser. Understanding these attributes is essential for security, session management, and cross-site request handling.
---

>Referenced from [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies){:target="_blank"}
{: .prompt-info }

<div markdown="1">
- Because the browser is smart, it automatically manages and stores cookies.
- When saving cookies in your browser, you can use the `Set-Cookie` response header to save cookies in your browser and control properties.
- Moreover, developers need to know how **backend frameworks automatically** set up `set-cookie`.

## 1. Overview

| **Attribute**             | **Description**                                        | **Example**                             |
| ------------------------- | ------------------------------------------------------ |
| **`Name=Value`**          | Defines the name and value of the cookie               | `session_id=abc123`                     |
| **`Expires` / `Max-Age`** | Specifies when the cookie should expire                | `Expires=Wed, 19 Jun 2025 12:00:00 GMT` |
| **`Domain`**              | Specifies which domain can access the cookie           | `Domain=.example.com`                   |
| **`Path`**                | Restricts the cookie to a specific URL path            | `Path=/account/`                        |
| **`Secure`**              | Ensures the cookie is sent only over HTTPS             | `Secure`                                |
| **`HttpOnly`**            | Prevents JavaScript from accessing the cookie          | `HttpOnly`                              |
| **`SameSite`**            | Controls how cookies are sent with cross-site requests | `SameSite=Strict`                       |

---

| **Attribute** | **Default Behavior (If Not Set)**            | **When Explicitly Set**                                             |
| ------------- | -------------------------------------------- | ------------------------------------------------------------------- |
| **Expires**   | Session cookie (deleted when browser closes) | Persistent until a specific date                                    |
| **Domain**    | Only available on the current domain         | Accessible on subdomains                                            |
| **Path**      | Current path and subdirectories              | Limited to a specific path                                          |
| **Secure**    | Sent over HTTP & HTTPS (security risk)       | Sent only over HTTPS (prevents MITM attacks)                        |
| **HttpOnly**  | JavaScript can access (`document.cookie`)    | JavaScript cannot access (prevents XSS)                             |
| **SameSite**  | `Lax` (CSRF protection enabled)              | `Strict` (stronger CSRF protection) or `None` (allows all requests) |

---

📌 **Secure Cookie Example:**
```http
Set-Cookie: session_id=xyz123; Secure; HttpOnly; SameSite=Lax; Expires=Wed, 19 Jun 2025 12:00:00 GMT
```
This setup ensures **CSRF/XSS protection, HTTPS security, and automatic session expiration.**  

---

## 2. `Name=Value` (Cookie Name and Value)
Every cookie consists of a **name-value pair** which stores data in the browser.

```http
Set-Cookie: session_id=xyz123
```
✅ Creates a cookie named `session_id` with a value of `xyz123`.

---

## 3. `Expires` / `Max-Age` (Cookie Expiration Time)

### 3.1. `Expires`
- Specifies a **fixed expiration date and time** in GMT/UTC format.
- Once the expiration date is reached, the browser automatically deletes the cookie.

```http
Set-Cookie: session_id=xyz123; Expires=Wed, 19 Jun 2025 12:00:00 GMT
```
✅ This cookie **expires on June 19, 2025, at 12:00 GMT**.

---

### 3.2. `Max-Age`
- Specifies the cookie's **lifespan in seconds** from the time it is set.
- `Max-Age=0` means the cookie expires **immediately**.

```http
Set-Cookie: session_id=xyz123; Max-Age=3600
```
✅ The cookie lasts **for 1 hour (3,600 seconds)** before expiring.

---

### 3.3. Difference Between `Expires` and `Max-Age`
- `Expires` sets an **absolute expiration date**.
- `Max-Age` sets a **relative expiration time** (in seconds).
- If both are present, **`Max-Age` takes precedence**.

---

### 3.4. If Not set
- The browser stores it until when the current session ends

---

## 4. `Domain` (Specifies Where the Cookie is Accessible)

###  4.1. `Domain` is NOT set
The cookie is only accessible from the domain that set it.

```http
Set-Cookie: auth_token=xyz123
```
✅ The cookie is available **only on `example.com`**.  
❌ It is **not accessible from `sub.example.com`**.

---

### 4.2. `Domain` is set
The cookie can be accessed by the specified domain and all its 

```http
Set-Cookie: auth_token=xyz123; Domain=.example.com
```
✅ The cookie is **available on `example.com`, `sub.example.com`, `blog.example.com`, etc.**

---

## 5. `Path` (Restricting Cookie Usage to Specific Paths)
### 5.1. `Path` is NOT set
Path is defaulted to the path of the request URL that set the cookie (e.g. `/login/`), and applies to that path and its subpaths (e.g. `/login/reset`).

---

### 5.2. `Path` is set

The cookie is **restricted to that specific path**.

```http
Set-Cookie: user_token=abc123; Path=/account/
```
✅ The cookie is available **only for `/account/` and its subdirectories**.  
❌ It will **not be sent with requests to `/home/` or `/profile/`**.

---

## 6. `Secure` (Restricting Cookies to HTTPS Only)

| Situation         | Cookie Stored?(Response) | Cookie Sent?(Request) |
| ----------------- | ------------------------ | --------------------- |
| HTTP + Secure     | ❌ No                     | ❌ No                  |
| HTTP + no Secure  | ✅ Yes                    | ✅ Yes                 |
| HTTPS + Secure    | ✅ Yes                    | ✅ Yes                 |
| HTTPS + no Secure | ✅ Yes                    | ✅ Yes                 |

```http
Set-Cookie: session_id=xyz123; Secure
```
✅ The cookie **is only transmitted over HTTPS**.  
❌ **It is not sent over HTTP** (better security).

---

## 7. `HttpOnly` (Preventing JavaScript Access to Cookies)

### 7.1. `HttpOnly` is Not set
JavaScript can **read and modify** the cookie using `document.cookie` (security risk!).

---

### 7.2. `HttpOnly` is set
JavaScript **cannot access the cookie** (prevents XSS attacks).
```http
Set-Cookie: session_id=xyz123; HttpOnly
```
✅ Now, **JavaScript cannot access `session_id`**, but the browser will still send it in HTTP requests.

---

### 7.3. Why is `HttpOnly` important?
- Prevents **Cross-Site Scripting (XSS) attacks** where malicious scripts steal cookies.
- Ensures cookies **can only be accessed by the server**.

---

## 8. `SameSite` (Preventing CSRF Attacks)
- `SameSite` controls **whether cookies are sent with cross-site requests**.
- Cookies will automatically be included in the request if they are of the same origin.

| **SameSite Value**  | **Behavior**                                                                       |
| ------------------- | ---------------------------------------------------------------------------------- |
| **`Strict`**        | Cookie is **only sent for same-site requests** (most secure).                      |
| **`Lax`** (Default) | Cookie is **sent when users click a link but not with other cross-site requests**. |
| **`None`**          | Cookie is **sent with all requests, even from other sites** (must be `Secure`).    |

---

### 8.1. (`Lax` == `SameSite` is Not set):
```http
Set-Cookie: session_id=xyz123; SameSite=Lax
```
✅ **Cross-site automatic requests (like `fetch`,`<img>`,`<iframe>`,`XMLHttpRequest`) will not include the cookie**, preventing CSRF attacks.  
✅ **If a user clicks a link(`<a href>`), the cookie will be sent**.

---

### 8.2. (`Strict` == maximum security):
```http
Set-Cookie: session_id=xyz123; SameSite=Strict
```
✅ **The cookie is never sent with cross-site requests**, even if the user clicks a link.

---

### 8.3. (`None` == allows all requests):
```http
Set-Cookie: session_id=xyz123; SameSite=None; Secure
```
✅ The cookie **is sent with all requests**, but `Secure` is **required** (must be HTTPS).

---

## 9. `fetch`

>Referenced from [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#including_credentials){:target="_blank"}
{: .prompt-info }

In many cases, developers use `fetch` function with the credential option to request a server. For security, it is important to consider the appropriate credential option.


```js
fetch('https://api.example.com/profile', {
  credentials: 'include'
});
```
 
| **Credentials**             | **Behavior**                                                                  |
| --------------------------- | ----------------------------------------------------------------------------- |
| **`omit`**                  | never send credentials in the request or include credentials in the response. |
| **`same-origin`** (Default) | only send and include credentials for same-origin requests.                   |
| **`include`**               | always include credentials, even cross-origin.                                |

---

> However, the sameSite attribute of cookie take precedence over the credential option of `fetch`
{: .prompt-warning}

| Condition                      | GET Request                  | POST Request                 |
| ------------------------------ | ---------------------------- | ---------------------------- |
| same-origin                    | ✅ Cookies sent automatically | ✅ Cookies sent automatically |
| cross-origin + SameSite=Lax    | ✅ (conditionally allowed)    | ❌ Blocked                    |
| cross-origin + SameSite=None   | ✅ (credentials: 'include')   | ✅ (credentials: 'include')   |
| cross-origin + SameSite=Strict | ❌ Blocked                    | ❌ Blocked                    |

---

</div>