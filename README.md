# Dark Web Site with Tor and Docker Compose

This project lets you host a static website as a Tor hidden service (a .onion site) using Docker Compose, Nginx, and Tor.

## Features
- Serves a static website via Nginx
- Exposes the site as a Tor hidden service (.onion address)
- Simple, portable, and easy to set up

## Project Structure
```
.
├── docker-compose.yml         # Docker Compose configuration
├── tor/
│   ├── torrc                 # Tor configuration file
│   └── ...                   # (Tor will create hidden_service/ here if you bind-mount)
└── web/
    ├── index.html            # Your website's main page
    └── default.conf          # Nginx config
```

## Prerequisites
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/)
- (Optional) [Tor Browser](https://www.torproject.org/download/) to access your .onion site

## Usage
1. **Clone or copy this repository.**
2. **Edit `web/index.html`** to customize your website.
3. **Start the services:**
   ```powershell
   docker-compose up -d
   ```
4. **Find your .onion address:**
   ```powershell
   docker exec darkweb_tor cat /var/lib/tor/hidden_service/hostname
   ```
   Open this address in Tor Browser.

## How it Works
- The Nginx container serves your static site.
- The Tor container runs Tor, configured to proxy requests to the Nginx container and expose a hidden service.
- The `.onion` address is generated automatically and printed in the `hostname` file.

## Custom Onion Address
- By default, the .onion address is random.
- For a custom (vanity) address, you must generate a special key using tools like [mkp224o](https://github.com/cathugger/mkp224o). This is advanced and computationally expensive.

## Troubleshooting
- If the Tor container fails to start, check permissions and logs:
  ```powershell
  docker logs darkweb_tor
  ```
- Do **not** bind-mount the `hidden_service` directory from Windows; let Tor manage it inside the container to avoid permission issues.

## Security Notice
- This setup is for educational/testing purposes. For production, review Tor and Docker security best practices.

## License
MIT
