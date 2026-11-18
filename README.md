# artex-and-friends

Daily feed of internet-exposed **agentic offensive frameworks**: AI-driven autonomous penetration-testing consoles and agent frameworks that can be (and are) used to attack other systems.
The feed is updated automatically every day from Shodan. One list per framework.

## Frameworks

| Framework | Key | What it is | Detection |
|---|---|---|---|
| ARTEX | `artex` | Autonomous penetration testing console (Chinese / Korean / English UI, also rebranded e.g. as AntiFox, TGSEC) | page title `ARTEX`, `自主渗透测试控制台`, `자율 침투 테스트 콘솔` |
| Cairn | `cairn` | Agent framework web UI (uvicorn on port 8000, or behind nginx) | Shodan product `Cairn`, TLS certificate `Cairn Platform` |
| CyberStrikeAI | `cyberstrikeai` | AI-driven offensive testing platform | favicon hash `-1349384020` |

## Files

| File | Content |
|---|---|
| `live/<key>.txt` | IPs **currently** exposing the framework (seen in one of the last 2 daily runs). **Use this for blocking / alerting.** |
| `live/all.txt` | all frameworks together |
| `lists/<key>.txt` | **history**: every IP ever seen for that framework (only grows) |
| `lists/all.txt` | history, all frameworks |
| `data/<key>.csv` | live hosts with details: ip, port, first/last seen, page title, product/version, org, ASN, country, matching query |
| `data/stats.json` | summary of the latest run: live / new / gone / total, top countries, orgs, ports, versions |
| `data/history.csv` | one row per day and framework: live, new, gone, total ever seen |
| `reports/<date>.md` | daily report: per-query counts, new and gone IPs, new page titles |

All `.txt` lists are one IP per line, sorted. Raw URLs work directly in a SIEM, firewall or EDL, e.g.

```
https://raw.githubusercontent.com/brunomarx/artex-and-friends/main/live/artex.txt
https://raw.githubusercontent.com/brunomarx/artex-and-friends/main/live/all.txt
```

## How it works

1. Every day each framework's Shodan queries run.
2. Results are de-duplicated by ip:port; known false positives (e.g. unrelated companies called "Artex") are filtered out by page title.
3. An IP leaves `live/` after it is missing from 2 runs in a row; it stays in `lists/` forever.

## Caveats

- Data comes from Shodan's crawl, which can be days old. An IP in the list was serving the framework when Shodan saw it.
- Exposure is not proof of malicious use: some hosts are researchers, red teams, honeypots or test installs. Use the lists for hunting, alerting and context; check before taking action against an individual IP.
- Cloud IPs get reused. Prefer `live/` over `lists/` for blocking.

False positive or a framework to add? Open an issue.
