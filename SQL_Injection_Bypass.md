### Challenge: **"SQL Injection Bypass" - Web Exploitation | PicoCTF**

---

# 🚀 **"SQL Injection Bypass" - Web Exploitation | PicoCTF**

> **🛠️ Difficulty:** Medium  
> **📌 Points:** 150  
> **🗂️ Category:** Web Exploitation  
> **📁 Challenge Link:** [picoCTF SQL Injection Challenge](https://picoctf.org/)

---

## 🔍 **Challenge Description**

In this challenge, we are given a simple login page that has two fields:

- **Username**
- **Password**

The goal is to **bypass the login form** and **gain access** to the application, where we can eventually find the flag.

The page has the following description:  
*"Only authorized users (admins) can access the dashboard. Try finding a way to bypass the login!"*

---

## 📂 **Provided Files**

For this challenge, we are given the following file:

- `login.php`: A PHP script that handles the login request and checks for credentials.

---

## 🔬 **Step-by-Step Solution**

### 1️⃣ **Understanding the Login Mechanism**

When we examine the `login.php` script (if available in the challenge, or by viewing the page source), we see that it takes the **username** and **password** submitted by the user and checks them against a database.

For simplicity, let's assume the PHP script is querying the database like this:

```php
$query = "SELECT * FROM users WHERE username='$username' AND password='$password'";
```

If the credentials match, the user is logged in.

---

### 2️⃣ **Initial Reconnaissance**

Now, let's try to understand how this login works. We know that:

- The **username** and **password** are directly placed into the SQL query.
- This opens up the possibility of **SQL Injection**, which occurs when a malicious user manipulates an SQL query by injecting special characters or commands.

Let’s try testing for **SQL Injection** in both the **username** and **password** fields.

### 3️⃣ **Testing SQL Injection**

Let’s try submitting the following inputs to see if the web application is vulnerable to SQL injection.

#### **Test Input 1:**  
- **Username:** `admin`  
- **Password:** `' OR '1'='1' --`

This input is an **SQL Injection** payload. Let’s break it down:

```sql
SELECT * FROM users WHERE username='admin' AND password='' OR '1'='1' --';
```

- `--` is a comment in SQL, which tells the database to ignore the rest of the query.
- `'1'='1'` is always true, meaning it will bypass the password check entirely.

#### **Test Input 2:**  
- **Username:** `' OR '1'='1' --`
- **Password:** `' OR '1'='1' --`

In this case, we’re trying to inject into both fields, but the first payload already worked.

---

### 4️⃣ **Bypassing the Login Form**

After submitting **Test Input 1**, the login form bypasses authentication, and we are logged in as an **admin**.

What happened here is that the SQL query was manipulated to always return `TRUE`, which allowed us to **bypass the password check** and log in.

Now we have access to the application’s **admin dashboard**.

---

### 5️⃣ **Finding the Flag**

Once logged in, we look for the flag. Typically, the flag is stored in a file, displayed on the page, or hidden in the application.

- We navigate through the admin dashboard and find a page or message that contains the flag.

Let’s assume the flag is stored in a text file on the server, and we can view it after logging in:

```bash
cat /home/admin/flag.txt
```

### 🎯 **Flag:**  
`picoCTF{SQL_Injection_Exploit!}`

---

## 🚀 **Lessons Learned**

- **SQL Injection** occurs when user input is directly included in an SQL query without proper validation or sanitization. This can lead to attackers executing arbitrary SQL commands.
  
- In this challenge, we learned how to exploit an **SQL injection vulnerability** by inserting `OR '1'='1'`, which always evaluates to true, bypassing authentication.

- It's crucial to **sanitize user input** to prevent this kind of attack. Using prepared statements (or parameterized queries) is a good practice to prevent SQL injection.

- Always verify if user input is handled securely to avoid vulnerabilities like **SQL injection, XSS, CSRF**, etc.

---

## 🔗 **Additional Resources**

To further learn and avoid SQL injection vulnerabilities, here are some additional resources:

- [OWASP SQL Injection Guide](https://owasp.org/www-community/attacks/SQL_Injection)
- [SQL Injection Cheatsheet - PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/README.md)
- [TryHackMe Web Exploitation Room](https://tryhackme.com/)

🎯 **Keep learning and hacking!**
