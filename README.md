# APIMyLlama V2 Documentation

## Overview

[![APIMyLlama Video](https://img.youtube.com/vi/x_MSmGX3Vmc/hqdefault.jpg)](https://www.youtube.com/embed/x_MSmGX3Vmc)

APIMyLlama is a server application that provides an interface to interact with the Ollama API, a powerful AI tool to run LLMs. It allows users to run this alongside Ollama to easily distribute API keys to create amazing things.

## Installation

### Ollama Setup
If you already have Ollama set up with the `ollama serve` command and your desired model, you can skip this step. Otherwise, install [Ollama](https://ollama.com/download) for your operating system. Once installed, open a terminal and run:

```bash
ollama pull tinyllama
```

This installs Meta's Llama3 LLM. You can use any model with the API, but for this example, we will use this model. After the installation is complete, run:

```bash
ollama serve
```

Now your Ollama server is set up.

### Hosting the API

Install [Node.JS](https://nodejs.org/en/download/package-manager) on your server, then clone the Git repository:

```bash
git clone https://github.com/Gimer-Studios/APIMyLlama.git
cd APIMyLlama
npm install
node APIMyLlama.js
```

After cloning, navigate to the `APIMyLlama` directory and install dependencies by running `npm install`. Then, start the application:

```bash
node APIMyLlama.js
```

On startup, it will prompt you for a port number and the Ollama server URL.

### Running Ollama on All Interfaces (For Multi-System Use)

#### Linux:
Edit `/etc/systemd/system/ollama.service` and add:

```
Environment="OLLAMA_HOST=0.0.0.0"
```

Alternatively, run:

```bash
OLLAMA_HOST=0.0.0.0 ollama serve
```

## Commands

### API Key Management
- `generatekey` - Generate an API key.
- `listkey` - List all API keys.
- `removekey <API_KEY>` - Remove a specific API key.
- `addkey <API_KEY>` - Manually add an API key.
- `changeport <SERVER_PORT>` - Change the server port.
- `changeollamaurl <OLLAMA_SERVER_URL>` - Change the Ollama server URL.
- `ratelimit <API_KEY> <RATE_LIMIT>` - Set request limits per minute.
- `activatekey <API_KEY>` - Activate a previously deactivated key.
- `deactivatekey <API_KEY>` - Deactivate a key.
- `listactivekeys` - List all active keys.
- `listinactivekeys` - List all inactive keys.

### Webhooks
- `addwebhook <WEBHOOK_URL>` - Add a webhook for notifications.
- `listwebhooks` - List all configured webhooks.
- `deletewebhook <WEBHOOK_ID>` - Remove a webhook.

### API Key Descriptions
- `addkeydescription <API_KEY>` - Add a description to an API key.
- `listkeydescription <API_KEY>` - View a key's description.
- `getkeyinfo <API_KEY>` - Get detailed information about a key.

### Batch Operations
- `generatekeys <NUMBER>` - Generate multiple API keys at once.
- `regeneratekey <API_KEY>` - Regenerate an API key.
- `activateallkeys` - Activate all API keys.
- `deactivateallkeys` - Deactivate all API keys.

## Working with the API

### Installation Packages

#### Node.JS (NPM)
```bash
cd PROJECT_NAME
npm install apimyllama-node-package
```

#### Python (PIP)
```bash
cd PROJECT_NAME
pip install apimyllama
```

#### Java (Gradle)
```bash
repositories {
    mavenCentral()
    maven { url 'https://www.jitpack.io' }
}

dependencies {
    implementation 'com.github.Gimer-Studios:APIMyLlama-Java-Package:V2.0.5'
}
```

#### Java (Maven)
```xml
<repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://www.jitpack.io</url>
    </repository>
</repositories>

<dependency>
    <groupId>com.github.Gimer-Studios</groupId>
    <artifactId>APIMyLlama-Java-Package</artifactId>
    <version>V2.0.5</version>
</dependency>
```

#### Rust (Cargo)
```toml
[dependencies]
apimyllama = "2.0.7"
tokio = { version = "1", features = ["full"] }
```

## Example API Requests


### Curl Example
```
curl -X POST "{DOMAIN/IP}/generate" \
     -H "Content-Type: application/json" \
     -d '{
           "apikey": "{API_KEY}",
           "prompt": "Hello",
           "model": "tinyllama"
         }' | tr -d '\\'

```


### Node.JS
```javascript
const apiMyLlamaNodePackage = require('apimyllama-node-package');

const apikey = 'API_KEY';
const prompt = 'Hello!';
const model = 'llama3';
const ip = 'SERVER_IP';
const port = 'SERVER_PORT';
const stream = false;

apiMyLlamaNodePackage.generate(apikey, prompt, model, ip, port, stream)
  .then(response => console.log(response))
  .catch(error => console.error(error));
```

### Python3
<code>python3 ai.py --api_key="YOUR_KEY" --m="tinyllama" "Why is the sky black?"</code>
```python
import subprocess
import shlex
import re
import json
import argparse

def extract_response(api_key, model, prompt):
    payload = json.dumps({
        "apikey": api_key,
        "prompt": prompt,
        "model": model
    })

    curl_command = f'curl -X POST https://gpt.code-x.my/generate -H "Content-Type: application/json" -d {shlex.quote(payload)}'

    try:
        result = subprocess.run(curl_command, shell=True, capture_output=True, text=True)

        if result.returncode != 0:
            print(f"Error occurred: {result.stderr.strip()}")
            return

        cleaned_output = result.stdout.replace("\\", "")
        json_objects = re.findall(r'\{.*?\}', cleaned_output)

        responses = []
        for obj in json_objects:
            try:
                data = json.loads(obj)
                if 'response' in data:
                    responses.append(data['response'])
            except json.JSONDecodeError:
                continue

        print(''.join(responses))

    except Exception as e:
        print(f"An error occurred: {e}")

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Send a prompt to the API and extract the response.")
    parser.add_argument("--api_key", required=True, help="The API key for authentication.")
    parser.add_argument("--m", "--model", dest="model", required=True, help="The model to use (e.g., tinyllama).")
    parser.add_argument("prompt", help="The prompt to send to the API.")  # Ensure this is treated as a positional argument

    args = parser.parse_args()

    extract_response(args.api_key, args.model, args.prompt)
```

### Java
```java
import com.gimerstudios.apimyllama.ApiMyLlama;

public class TestAPIMyLlama {
    public static void main(String[] args) {
        String serverIp = "SERVER_IP";
        int serverPort = SERVER_PORT;
        String apiKey = "API_KEY";
        String prompt = "Hello!";
        String model = "llama3";
        boolean stream = false;

        ApiMyLlama apiMyLlama = new ApiMyLlama(serverIp, serverPort);
        try {
            String response = apiMyLlama.generate(apiKey, prompt, model, stream);
            System.out.println("Generate Response: " + response);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Autorun on boot

To set up your Node.js application, `APIMyLlama.js`, to auto-run on boot, I'll provide you with tailored steps based on your provided path (`/{YOUR_PATH}/APIMyLlama/`). I'll assume you're using a Linux-based OS (Ubuntu/Debian). If you're using a different operating system, let me know!

### **Linux (Ubuntu/Debian)**

#### **Using `systemd`**

1. **Create a service file**  
   Open a terminal and run:
   ```sh
   sudo nano /etc/systemd/system/apimyllama.service
   ```

2. **Add the following content**  
   Replace the paths with your actual paths, ensuring that `/usr/bin/node` points to your Node.js installation. If you have a specific Node.js version manager like `nvm`, adjust accordingly.

   ```ini
   [Unit]
   Description=APIMyLlama Node.js Application
   After=network.target

   [Service]
   ExecStart=/usr/bin/node {YOUR_FULL_PATH}/APIMyLlama/APIMyLlama.js
   WorkingDirectory={YOUR_FULL_PATH}/APIMyLlama
   Restart=always
   User=root
   Group=root
   Environment=NODE_ENV=production
   StandardOutput=syslog
   StandardError=syslog
   SyslogIdentifier=apimyllama

   [Install]
   WantedBy=multi-user.target
   ```

3. **Enable and start the service**  
   Reload `systemd` to recognize the new service:
   ```sh
   sudo systemctl daemon-reload
   sudo systemctl enable apimyllama
   sudo systemctl start apimyllama
   ```

4. **Check the service status**  
   To verify that the service is running correctly:
   ```sh
   sudo systemctl status apimyllama
   ```

   ## FAQ

#### 1. Why am I getting the module not found error?

You most likely forgot to run the 'npm install' command after cloning the repository.

#### 2. Why can't I use the API outside my network?

You probably didn't port foward. And if you did your router may have not intialized the changes yet or applied them.

#### 3. Ollama Serve command error "Error: listen tcp 127.0.0.1:11434: bind: Only one usage of each socket address (protocol/network address/port) is normally permitted."

If you get this error just close the Ollama app through the system tray on Windows. And if your on Linux just use systemctl to stop the Ollama process. Once done you can try running the ollama serve command again.

#### 4. error: 'Error making request to Ollama API'

If you have a custom port set for your Ollama server this is a simple fix. Just run the 'changeollamaurl <YOUR_OLLAMA_SERVER_URL>' and change it to the url your Ollama server is running on. By default it is "http://localhost:11434" but if you changed it you will need to do this. You can also fix this problem through changing the port in the ollamaURL.conf file.

## Authors

- [@gimerstudios - Original](https://github.com/Gimer-Studios) -
- [@Exrienz](https://github.com/exrienz/)

