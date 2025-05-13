# 🐳 Dockerized Multi-Tor Proxy Suite  
*A scalable, containerized solution for hosting multiple isolated Tor proxy instances*  

---

## 🔍 Overview  
This project provides a clean and efficient **Docker Compose configuration** to deploy multiple Tor proxy services simultaneously. Each instance runs in isolation with its own circuit and is exposed on a unique port, enabling parallel traffic routing through distinct Tor exit nodes.  

Ideal for developers, ethical hackers, and privacy advocates requiring distributed Tor-based networking capabilities.  

---

## 🧩 Key Features  
- **10 Pre-configured Tor Proxies**  
  Each proxy runs on a dedicated port (9051–9060) with no configuration overhead.  
- **Effortless Scalability**  
  Add new proxies in seconds by duplicating service templates in `docker-compose.yml`.  
- **Isolated Circuits**  
  Independent Tor sessions per instance for true traffic separation.  
- **Docker-First Design**  
  Lightweight, portable, and compatible with Linux, macOS, and Windows (via WSL).  

---

## 📦 Service Mapping  
| Service | Host Port | Container Port | Proxy URL              |  
|---------|-----------|----------------|------------------------|  
| tor1    | 9051      | 9050           | `socks5://localhost:9051` |  
| tor2    | 9052      | 9050           | `socks5://localhost:9052` |  
| tor3    | 9053      | 9050           | `socks5://localhost:9053` |  
| tor4    | 9054      | 9050           | `socks5://localhost:9054` |  
| tor5    | 9055      | 9050           | `socks5://localhost:9055` |  
| tor6    | 9056      | 9050           | `socks5://localhost:9056` |  
| tor7    | 9057      | 9050           | `socks5://localhost:9057` |  
| tor8    | 9058      | 9050           | `socks5://localhost:9058` |  
| tor9    | 9059      | 9050           | `socks5://localhost:9059` |  
| tor10   | 9060      | 9050           | `socks5://localhost:9060` |  

---

## 🚀 Getting Started  

### Prerequisites  
- [Docker](https://docs.docker.com/get-docker/) & Docker Compose installed  
- Minimum system resources:  
  - 2 GB RAM (adjust for scale)  
  - 1 CPU core per 5 proxies recommended  

### Installation  
1. Clone the repository:  
   ```bash  
   git clone https://github.com/your-username/multi-tor-proxy.git  
   cd multi-tor-proxy  
   ```  

2. Launch the services:  
   ```bash  
   docker-compose up -d  
   ```  

3. Verify containers are running:  
   ```bash  
   docker-compose ps  
   ```  

---

## 🛠️ Usage  
### Routing Traffic  
Configure applications to use Tor proxies via their respective ports. For example:  
- **cURL via tor1**:  
  ```bash  
  curl --socks5-hostname localhost:9051 https://check.torproject.org  
  ```  
- **Browser Configuration**: Set SOCKS5 proxy to `localhost:9051`, `localhost:9052`, etc.  

### Adding More Proxies  
Edit `docker-compose.yml` to extend the fleet:  
```yaml  
tor11:  
  image: dperson/torproxy:latest  
  ports:  
    - "9061:9050"  
```  
> 🔁 Update port numbers sequentially to avoid conflicts.  

---

## 🐍 📦 Programmatic Usage in Python  
### Requirements  
```bash  
pip install requests[socks]  
```  

### Example: GET Request via Tor Proxy  
```python
import requests

# Configure proxy (e.g., tor1 on port 9051)
proxies = {
    'http': 'socks5h://localhost:9051',
    'https': 'socks5h://localhost:9051'
}

# Test request
try:
    response = requests.get(
        'https://ifconfig.me/ip',  # Shows your current IP
        proxies=proxies,
        timeout=10
    )
    print("IP via Tor:", response.text.strip())
except requests.exceptions.RequestException as e:
    print("Error:", e)
```

### Notes  
- Replace `socks5h` with `socks5` if DNS leaks are not a concern.  
- Use different ports (9052–9060) for parallel sessions.  
- For POST requests, simply replace `requests.get()` with `requests.post()`.  

---

## 🧱 📦 Programmatic Usage in Node.js  
### Requirements  
```bash  
npm install axios socks-proxy-agent
```  

### Example: GET Request via Tor Proxy  
```javascript
const axios = require('axios');
const { SocksProxyAgent } = require('socks-proxy-agent');

// Configure agent (e.g., tor2 on port 9052)
const agent = new SocksProxyAgent({
  hostname: 'localhost',
  port: 9052,
  type: 5 // SOCKS protocol version
});

// Test request
(async () => {
  try {
    const response = await axios.get('https://ifconfig.me/ip', { 
      httpsAgent: agent,
      httpAgent: agent
    });
    console.log("IP via Tor:", response.data.trim());
  } catch (error) {
    console.error("Error:", error.message);
  }
})();
```

### Notes  
- For POST requests, use `axios.post()` with the same agent configuration.  
- Change the port number to use different Tor instances (9053–9060).  
- Ensure your Node.js version supports ES6 modules if needed.  

---

## 🧹 Maintenance  

### Stopping Services  
```bash  
docker-compose down  
```  

### Logs & Monitoring  
View logs for a specific proxy:  
```bash  
docker-compose logs -f tor1  
```  

---

## ⚠️ Notes & Best Practices  
- **Initialization Time**: Tor instances may take 15–30 seconds to bootstrap.  
- **Resource Management**: Scale proxies based on available system resources.  
- **Security**: Avoid exposing proxies to public networks without authentication.  
- **Anonymity**: Tor alone does not guarantee anonymity—use additional safeguards (e.g., VMs, firewalls).  

---

## 🤝 Contributing  
Contributions are welcome! To propose enhancements:  
1. Fork the repository  
2. Create a feature branch (`git checkout -b feature/new-proxy-template`)  
3. Commit changes and submit a PR  

---

## 📄 License  
MIT License  
See [LICENSE](LICENSE) for details.  

---  
*Built with ❤️ for privacy, scalability, and simplicity.*  
