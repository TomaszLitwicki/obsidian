## Modele językowe LOKALNE
[Ollama.com]()
Polski LLM Bielki
`ollama run SpeakLeash/bielik-7b-instruct-v0.1-gguf:Q4_K_S`
## MCP
MCP - Model Context Protocol
*Idea - zapewnić łatwą integrację między modelami a zewnętrznymi aplikacjami. Przenosi odpowiedzialność w integracji z zewnętrznymi narzędziami na dostawców narzędzi (a nie twórców LLM)*
Wywoływanie zewnętrznych funkcji lub zewnętrznych narzędzi
Function Calling / Tool Calling
[więcej info z OpenAI](https://platform.openai.com/docs/guides/function-calling)
Własny serwer MCP zgodnie z instrukcją:
[Model Context Protocol](https://modelcontextprotocol.io/docs/develop/build-server#testing-your-server-with-claude-for-desktop)
## Prompt Engineering
sposób pisania prompt'u, aby uzyskać spodziewany/potrzebny wynik 
META PROMPTING - wykorzystanie AI do poprawienia prompt'u
## Structure Model Outputs
#parsowanie  — tłumaczenie surowego tekstu (ciągu znaków) na strukturę, którą komputer może logicznie zrozumieć.
[Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs)
## Prompt Management
Zarządzanie promptami
Dla dużych komercyjnych projektów
- https://langfuse.com
- https://www.langchain.com/langsmith
Dla małych projektów narzędzie od OpenAI
- [Reużywalne prompty](https://platform.openai.com/docs/guides/prompt-engineering#reusable-prompts)
- [Tworzenie takiego promptu](https://platform.openai.com/chat/edit?models=gpt-5)
do którego można dodawać zmienne i narzędzia i reużywać go w aplikacji. Przechowywany po stronie AI i można go pobrać po ID.
Zmienne w podwójnych {{nawiasach klamrowych}}
## Nauka z AI
https://www.perplexity.ai/ weryfikacja odpowiedzi
https://notebooklm.google/ 


