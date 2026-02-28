# Setup

If you'd like to iterate and test your MCP server, you can do so in local development.

## Local Development

1. Create a `.dev.vars` file in your project root:

   Provide a development API token:

   ```
   DEV_DISABLE_OAUTH=true
   # This is your global api token
   DEV_CLOUDFLARE_API_TOKEN=your_development_api_token
   ```

2. Start the local development server:

   ```bash
   npx wrangler dev
   ```

   If `8976` is already in use on your machine, run on another port (for example `8977`):

   ```bash
   npx wrangler dev --port 8977
   ```

3. To test locally, open Inspector and connect to `http://localhost:8976/mcp` (or the custom port you chose).
   Once you follow the prompts, you'll be able to "List Tools".

4. OpenCode setup for local, no-OAuth mode:

   When `DEV_DISABLE_OAUTH=true`, `opencode mcp auth` is not needed for the local server.
   Configure `radar-local` as a local `mcp-remote` bridge instead of a direct remote URL.

   In `~/.config/opencode/opencode.json`:

   ```json
   {
   	"mcp": {
   		"radar-local": {
   			"type": "local",
   			"command": ["npx", "-y", "mcp-remote", "http://localhost:8977/mcp"]
   		}
   	}
   }
   ```

   Then run:

   ```bash
   opencode mcp list
   ```

   You should see `radar-local` as connected while `wrangler dev` is running.

## Deploying the Worker ( Cloudflare employees only )

Set secrets via Wrangler:

```bash
npx wrangler secret put CLOUDFLARE_CLIENT_ID -e <ENVIRONMENT>
npx wrangler secret put CLOUDFLARE_CLIENT_SECRET -e <ENVIRONMENT>
```

## Set up a KV namespace

Create the KV namespace:

```bash
npx wrangler kv namespace create "OAUTH_KV"
```

Then, update the Wrangler file with the generated KV namespace ID.

## Deploy & Test

Deploy the MCP server to make it available on your workers.dev domain:

```bash
npx wrangler deploy -e <ENVIRONMENT>
```

Test the remote server using [Inspector](https://modelcontextprotocol.io/docs/tools/inspector):

```bash
npx @modelcontextprotocol/inspector@latest
```
