 # Troubleshooting RTS API System Prompt Issues

This is related to the [[Klüh Voicebot Project Overview]]
Almost direct copy of repo doesnt yield the expected success.
For more details read [[Klüh - Onboarding Call Sven]] 

This guide addresses the issue where the Real-Time Speech (RTS) API sometimes ignores the system prompt for tool workflow, causing responses to use foundation model knowledge instead of retrieved context.

## Recommendations (Ordered by Priority)

### 1. System Message Configuration Verification
**Confidence Score: 95%**

Check in `app/backend/rtmt.py`:
- Verify system_message is not None
- Ensure system message is being passed correctly in session updates
- Make system message more explicit about requiring tool usage

Example system message enhancement:

`system_message = """You MUST use the search tool for any factual information.`
`Do not rely on your general knowledge.`
`Always cite sources from the search results."""`


### 2. Tool Registration Validation 
**Confidence Score: 90%**

Check in `app/backend/ragtools.py`:
- Verify search tool schema is properly defined
- Ensure tool descriptions are explicit about mandatory usage
- Confirm tools are registered in RTMiddleTier initialization

Key areas to verify:

'''python
search_tool_schema = {
"type": "function",
"name": "search",
"description": "REQUIRED: Search the knowledge base before answering..."
# ... rest of schema
}
'''


### 3. WebSocket Communication Check
**Confidence Score: 85%**

Monitor in `app/backend/rtmt.py`:
- Confirm tools are included in session configuration
- Verify tool_choice is set to "auto"
- Check if session updates are being acknowledged

Debug points:

'''python
session["tool_choice"] = "auto" if len(self.tools) > 0 else "none"
session["tools"] = [tool.schema for tool in self.tools.values()]
'''


### 4. Tool Processing Debug
**Confidence Score: 80%**

Add logging in `app/backend/rtmt.py`:

'''python
logger.debug(f"Tool call received: {item['name']}")
logger.debug(f"Tool arguments: {args}")
logger.debug(f"Tool result: {result.to_text()}")
'''

Monitor:
- Tool call frequency
- Tool result processing
- Response integration

### 5. Search Implementation Review
**Confidence Score: 75%**

Check in `app/backend/ragtools.py`:
- Verify search results format
- Monitor search result quality
- Confirm content formatting

Key function:

'''python
async def search_tool(search_client: SearchClient, ...):
\# Add logging
logger.debug(f"Search query: {args['query']}")
logger.debug(f"Search results count: {len(results)}")
'''

## Required Information for Better Diagnosis

To provide more targeted solutions, please provide:

1. Current system prompt configuration
2. Example conversations where the issue occurs
3. Search tool response samples
4. Console error messages
5. Azure OpenAI deployment configuration

## Notes

- Confidence scores are based on common patterns in similar implementations
- Start with highest confidence recommendations first
- Document results of each attempted fix
- Multiple issues might need to be addressed simultaneously

## Next Steps

1. Implement logging at recommended points
2. Test with simplified prompts first
3. Gradually increase complexity while monitoring behavior
4. Document any patterns in when the system prompt is ignored
