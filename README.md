# AI Engineer Neo4j Memory MCP Demo
This is a short demo of the Neo4j  Memory MCP server. 

This file descibes how to configure Claude Desktop with agentic memory courtesy of the Neo4j Memory MCP Server.


## Set Up

### Install uv Python Package Manager
The Neo4j Memory MCP Server is written in Python and managed by uv. Follow the directions on [this page](https://docs.astral.sh/uv/getting-started/installation/) to properly install.

### Neo4j Database

Create a Neo4j database instance by either 
* Navigating to the [Neo4j Aura Console](https://console.neo4j.io/) to create a cloud hosted Neo4j Aura instance 
* Downloading [Neo4j Desktop](https://neo4j.com/download) and creating a local Neo4j instance 

### Claude Desktop
Download [Claude Desktop](https://claude.ai/download)
* navigate to Settings -> Developer -> Edit Config
* Add the Neo4j Memory MCP server to the config file

*You may replace the config values with your own database credentials*

```json
{
  "mcpServers": {
    "neo4j-memory-store": {
      "command": "uvx",
      "args": [ "mcp-neo4j-memory@0.1.4" ],
      "env": {
        "NEO4J_URL": "bolt://localhost:7687",
        "NEO4J_USERNAME": "neo4j",
        "NEO4J_PASSWORD": "password",
        "NEO4J_DATABASE": "neo4j"
      }
    }
  }
}
```

* Navigate to Settings -> General -> Claude Settings -> Configure
* In the personal preferences section - paste this text:

```markdown
Follow these steps for each interaction:

1. Memory Retrieval:
   - Always begin your chat by saying only "Remembering..." and retrieve all relevant information from your knowledge graph
   - Always refer to your knowledge graph as your "memory"

2. Memory Update:
   - If any new information was gathered during the interaction, update your memory as follows:
     a) Create entities for recurring organizations, people, and significant events
     b) Connect them to the current entities using relations
     b) Store facts about them as observations
```

This text is a modified version of [the system prompt ](https://github.com/modelcontextprotocol/servers/tree/main/src/memory#system-prompt)Anthropic recommends for their memory MCP server.


## Demo

Begin a conversation with Claude Desktop. 

Each interaction should be automatically logged in the Neo4j database you've configured. 

These memories may be used between different conversations with Claude and even accessed by other clients such as Cursor or Windsurf, if they are configured with the same Neo4j instance. 