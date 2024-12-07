Meine Eindrücke nach meinen GEsprächen zu AUtomation Potential im Engineering
Gespräche:
[[Tempton Automation Potential]]
[[Lekkerland]]
[[Kaufland Overview]]
[[Flossbach von Storch]]

Inspo: [[SAG Automation Process Overview]]



Ich hab mit Joshua, Andi, Carla, Sophia gesprochen

Meine Findings:

Potential in unterschiedlichen Bereichen und auf unterschiedliche Arten:
- Konkretes Potenzial:
	- Tempton
		- Doku der Legacy Lösung non-existent
		- 50% des Aufwands geht auf Verstehen und Erfassen
		- Legacy Lösung in SSIS Flows
		- ähnlich zu SAG (Flows statt Stored Procs, aber Flows in xml gestored)
- Vollautomatische Lösung ist aufwendig
- Semi-automatische Lösung sinnvoller Zwischenschritt
- semi-automatische Lösung sollte leicht in vollautomatische überführbar sein
- semi automatische Lösung gut möglich über build your own gpt oder evtl custom spaces bei perplexity
	- BYO GPT klappt am besten
	- Perplexity Spaces erlauben mehr Models, aber Kontext Documents werden nicht so gut ausgelesen und verwendet
- Vollautomatische Lösung ähnlich [[SAG Automation Process Overview]] erfordert viel präzises Wissen über Arbeitsabläufe und Edge Cases
	- Problem: Maximaler Impact bei früher Entwicklung, aber damit auch Slowdown und Edge Cases unbekannt
- Lesson:
	- Ich kann semi-automatic support tools easy bauen
	- viele PoCs können fast realtime impact haben
	- PoC erlauben mir einen Impact ohne direkt im Prozess drin zu sein, weil Anpassungen sehr schnell
	- wenn ich bei einem großen Engineering Projekt die Prozesse mitgestalte, dann auch automatisierbar
	- von außen Vollautomatisierung wahrscheinlich zu schwer zu schnellen Impact
- Versioning
	- Saubere Lösung erfordert Versionierung
	- Helper Tool, das yml ausliest und dann in currently open byo gpt einfügt kann helfen
- Konkretes