This file is to store my ideas for possible workflow improvements, most of them heavily LLM dependent.

# Ideas
Rough ideas
- Repo Visualizer
- Custom Prompt Library
- Run on save - run a script in vscode/cursor on saving a specific type of file
- Meeting Notes Assistant - check meeting notes for a number of tasks and provide a list of tasks to iterate over
## Repo Visualizer
- Input: Repo (URL or directory)
	- Output: drawio file with xml for draw.io graph
	- Requires identification of general repo structure
	- Visualize it
	- then iterate over them
	- visualize each part
	- then combine all partial visual representations
## Custom Prompt Library
- Issue: During calls I love using Perplexity and Cursor to get a better understanding of the topic at hand. Writing my instructions while taking notes and listening is exhausting. If I had a set of prompts ready that only require a small additional input, I could make better use of the meetings
- In this context I'd love a transcription tool that runs undected preferably
- Idea:
	- Perplexity space with a txt with all my custom prompts
	- any thread in this space reads the txts first, finds the fitting prompt and executes it
## Run on save
- Issue: closely related to the custom prompt library
- When in a call i want to take notes in vscode/cursor
- When saving the notes I want to trigger workflows that provide me with a bunch of information
- Imagine this scenario:
	- During the meeting i get information about a tool they use, their architecture and 
- Example workflow:
	- When saving an md file
		- calculate change size
		- the bigger the change, the bigger the size
		- if the size crosses some threshold, make it pretty, format it, add links (for obisdian)
	- Solution ideas:
		- I can run custom python scripts on save
		- i can create a function to calculate the change size
		- i can create a call to an llm to make the file pretty
		- if the change size is big enough, trigger the call
		- also check for some kind of keyword like \\pretty, if it exists, run the second part independent of the change size and remove the keyword
		- pass the saved file to the script obviously
	- Issues and ideas:
		- to get the diff between save and previous save, i need a backup of the previous save. 
		- I can create the backup as part of the script
		- the backup should be looked for, if it doesnt exist the change size is big so it should be made pretty (see: [[Change Significance Score]])
		- after being made pretty it should be saved to backup for later comparison
		- i need a way to undo the prettifying and revert to the latest save maybe (not core functionality)
- I can use the extension "Run on Save" for saves and I can use the [[Git Post-Commit Hook]] for commit handling like 
## Meeting Notes Assistant
when finishing meeting notes, analyze them for documents to have ready
- for every technology named, make sure a file outlining the information about the technology exists in a concise report
	- if it doesnt exist, create the report
- analyze for to dos and tickets that need to be tracked/created
	- offer relevant ticket info like title, description, story points and so on