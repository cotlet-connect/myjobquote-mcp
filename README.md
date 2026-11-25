# MyJobQuote API integration and MCP server: connect MyJobQuote to ChatGPT, Claude and any AI agent

**MyJobQuote API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your MyJobQuote account.** MyJobQuote has no public API for tradespeople and no official MCP server; its only API serves affiliates who send jobs in. Cotlet.dev connects your own signed-in MyJobQuote account to any AI agent, so you ask in plain words and the AI answers leads and messages customers for you.

## What you can ask

> Rank today's MyJobQuote leads by budget and town and tell me which 3 are worth the credits.

> List the jobs I bought this week with the customer's phone and email.

> Find leads I bought where the customer never answered and draft a dispute for each.

> How many credits did I spend this month, and how many jobs did I win from them?

> Draft a feedback request for each customer whose job I finished this month.

## What tradespeople ask their AI to do in MyJobQuote

From tradespeople' posts on Reddit, Facebook groups, review sites and the MyJobQuote help center (checked 25 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Pick leads before buying | "Rank today's MyJobQuote leads by budget, town and job type, and tell me which 3 are worth the credits." | Daily |
| Call list of bought jobs | "List the jobs I bought this week with the customer's phone and email, oldest first." | Daily |
| Dispute dud leads | "Find leads I bought where the customer never answered or the job was already done, and draft a dispute for each." | Weekly |
| Credits against jobs won | "How many credits did I spend this month and how many jobs did I win from them?" | Monthly |
| Spot repeat posts | "Flag new leads that look like the same job posted again this month." | Daily |
| Ask for feedback | "List jobs I finished this month and draft a feedback request for each customer. Send nothing yet." | Monthly |

Your AI asks you before it sends, buys, replies or changes anything. Payments stay in your hands.

## MyJobQuote API

MyJobQuote has no API, webhooks or data export for a tradesperson's own account; its API serves affiliates who send jobs in (checked 25 Nov 2026). With Cotlet.dev you build your own MyJobQuote API: show your AI agent a job once in your signed-in MyJobQuote account, save it as an endpoint and call it from n8n, Make, Zapier or your own code. How it works: https://cotlet.dev/integrations/api/myjobquote

Endpoints you can build in a few minutes (none of them exists until you build it):

- `new-leads`: Time, job, town, budget and credit price of each lead
- `purchased-jobs`: Customer, phone, email and job of each lead you bought
- `credits-spent`: Credits used and jobs bought for the period you name
- `open-disputes`: Disputed jobs and where each one stands
- `leads-to-dispute`: Bought leads with no answer or a job already gone
- `mark-completed`: The job status you name, saved after your OK

Call one with your Cotlet API key: `POST https://cotlet.dev/api/v1/custom/new-leads`. One call runs up to 90 seconds, writes wait for your OK, payments are blocked.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its MyJobQuote tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"myjobquote":{"command":"uvx","args":["myjobquote-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t myjobquote-mcp .
docker run -i --rm -e COTLET_API_KEY myjobquote-mcp
```

Tools: `myjobquote_guide`, `myjobquote_status`, `myjobquote_open`, `myjobquote_read`, `myjobquote_click`, `myjobquote_type`, `myjobquote_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install myjobquote-mcp        # or run it once: uvx myjobquote-mcp connect
myjobquote-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 myjobquote_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 myjobquote_agent.py status    # is MyJobQuote ready in your Cotlet.dev account
python3 myjobquote_agent.py guide     # MyJobQuote screens and sign-in link
python3 myjobquote_agent.py read      # open MyJobQuote, read the page, close the browser
python3 myjobquote_agent.py tools     # list Cotlet.dev tools
python3 myjobquote_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from MyJobQuote users

**Does MyJobQuote have an API?**

MyJobQuote has no public API for tradespeople (checked 25 Nov 2026). Its only API is in the affiliate programme, for publishers who send homeowner jobs in. Cotlet.dev works in your own MyJobQuote account, so your AI agent can read new leads and message customers.

**How do I connect MyJobQuote to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to MyJobQuote once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no MyJobQuote API key.

**Is there a way to use Claude with MyJobQuote?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own MyJobQuote account.

**Is there a MyJobQuote MCP server?**

MyJobQuote has no official MCP server. Cotlet.dev is a hosted MCP server that works in MyJobQuote for you, 24/7, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

**Can AI dispute bad MyJobQuote leads for me?**

Your AI finds bought leads where the customer never answered or the job was gone, drafts the dispute with the dates and sends it in your account after your OK. MyJobQuote decides each dispute.

**Can AI tell me which MyJobQuote leads to buy?**

Yes. It reads new leads in your account, ranks them by budget, town and job type, and buys only the ones you pick.

## Find it on

- [MyJobQuote on Cotlet.dev](https://cotlet.dev/integrations/mcp/myjobquote?utm_source=github&utm_medium=readme&utm_campaign=myjobquote&utm_content=find)
- [PyPI package myjobquote-mcp](https://pypi.org/project/myjobquote-mcp/)
- [Odel](https://odel.app/modules/cotlet/myjobquote)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/myjobquote/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-myjobquote-to-ai-agents-guide)

## Also works with

- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [hipages MCP](https://github.com/cotlet-connect/hipages-mcp)
- [ServiceSeeking MCP](https://github.com/cotlet-connect/serviceseeking-mcp)
- [Yelp for Business MCP](https://github.com/cotlet-connect/yelp-for-business-mcp)
- [Rosie MCP](https://github.com/cotlet-connect/rosie-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [YourTradebase MCP](https://github.com/cotlet-connect/yourtradebase-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [eLocal MCP](https://github.com/cotlet-connect/elocal-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/myjobquote?utm_source=github&utm_medium=readme&utm_campaign=myjobquote

Start free: https://cotlet.dev/signup?app=myjobquote&utm_source=github&utm_medium=readme&utm_campaign=myjobquote
