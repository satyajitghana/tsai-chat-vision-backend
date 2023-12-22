# TSAI Chat - Vision

```bash
pip install -r requirements.txt
```

```bash
python main.py
```

## Usage

```python
from openai import OpenAI

openai_api_key = "EMPTY"
openai_api_base = "http://localhost:8000/v1"

client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)

response = client.chat.completions.create(
  model="llava-hf/bakLlava-v1-hf",
  messages=[
    {
      "role": "user",
      "content": [
        {"type": "text", "text": "What’s in this image?"},
        {
          "type": "image_url",
          "image_url": {
            "url": "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg",
          },
        },
      ],
    }
  ],
  max_tokens=300,
  stream=True
)

for chunk in response:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")
```

## NOTES

The model being used in this is `llava-hf/bakLlava-v1-hf` which is a mistral backbone model of llava

You can also use `llava-hf/llava-1.5-7b-hf`

```
model_id = "llava-hf/llava-1.5-7b-hf"
quantization_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_compute_dtype=torch.float16
)
```

## Reverse Proxy

Make sure to point your domain to the instance ip first

Install Caddy

```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

```
sudo setcap CAP_NET_BIND_SERVICE=+eip $(which caddy)
```

Run this command in the folder with `Caddyfile`

```
caddy stop
caddy start
```