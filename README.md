## Task 3  : Perform a Basic Vulnerability Scan on Your PC
---
## ✅ **Outcome: Vulnerability Scan Setup Using Nessus Essentials (v10.8.4)**

### 🎯 **Objective:**

To perform a **basic vulnerability scan** on the local system using **Nessus Essentials**, and identify potential weaknesses using free community tools.

---

### 🛠️ **Steps Taken:**

1. **Downloaded Nessus Essentials**

   * Used `curl` to download Nessus installer for Ubuntu:

     ```bash
     curl --request GET \
     --url 'https://www.tenable.com/downloads/api/v2/pages/nessus/files/Nessus-10.8.4-ubuntu1604_amd64.deb' \
     --output 'Nessus-10.8.4-ubuntu1604_amd64.deb'
     ```

2. **Installed Nessus**

   * Installed using:

     ```bash
     sudo dpkg -i Nessus-10.8.4-ubuntu1604_amd64.deb
     sudo apt --fix-broken install
     ```

3. **Started Nessus Service**

   * Started and enabled the service:

     ```bash
     sudo systemctl start nessusd
     sudo systemctl enable nessusd
     ```

4. **Faced Plugin Registration Issue**

   * Nessus showed:
     `Did not get a 200 OK response from the server: HTTP/1.1 400 Bad Request`
   * Root Cause: **Plugin feed registration failed** due to:

     * Invalid or reused activation code
     * Plugin download blocked or broken
     * Feed fetch error from Tenable’s server

5. **Debugging and Mitigation**

   * Unregistered Nessus and cleared cache:

     ```bash
     sudo /opt/nessus/sbin/nessuscli fetch --unregister
     sudo rm -rf /opt/nessus/var/nessus/*
     ```

   * Retried registration manually:

     ```bash
     sudo /opt/nessus/sbin/nessuscli fetch --register <YOUR-CODE> --os ubuntu1604 --accept-eula
     ```

6. **Validated Internet and Plugin Access**

   * Checked server response:

     ```bash
     curl -I https://plugins.nessus.org
     ```
   * Ensured system had internet access and outbound HTTPS was not blocked.

---

### 📌 **Key Takeaways:**

* Proper activation and plugin downloads are **mandatory** for scan functionality.
* Internet access and **firewall/proxy settings** can directly affect Nessus's ability to fetch updates.
* Manual commands like `nessuscli fetch` and clearing cache are useful for troubleshooting.
* Plugin feed error (HTTP 400) is often linked to **invalid requests, stale cache, or reused codes**.
