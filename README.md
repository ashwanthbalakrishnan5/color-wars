# Color Wars: Red vs Blue

The whole class, one shared Miro board, two teams. Your AI agent does the fighting: it paints squares
your team's color, attacks the other team, sets off bombs and cuts supply lines, while a referee
program keeps everyone honest. Everyone plays at the same time.

**The board:** https://miro.com/app/board/uXjVEO7YzFE=/

**Rules** (also on the board): capture squares that touch your team; enemy squares count too.
💥 Gold squares are bombs (and they set each other off). ✂️ Squares cut off from their base go grey.
🛡 Bases are + shaped (Red bottom left, Blue top right): take all 5 enemy base squares and their
whole army is cut off: **your team wins**.

You don't need to code. You paste one prompt into the Codex app and watch the board.

## Before class

Accept the Miro invite email from your instructor and sign in to Miro with your ASU email. Open the
board link above once to check you can see it.

## The one prompt

Open the **Codex** app, pick any folder (for example Documents), start a chat and paste this:

```text
Set me up for Color Wars and play it for me, in this chat. Ask me only when you must; keep every message to one or two short lines.
1) Get the kit into this folder: if ./color-wars exists, run `git -C color-wars pull`; otherwise `git clone https://github.com/ashwanthbalakrishnan5/color-wars.git`. If git is missing or asks to install developer tools, instead run `curl -L -o cw.zip https://github.com/ashwanthbalakrishnan5/color-wars/archive/refs/heads/main.zip && unzip -oq cw.zip && rm -rf color-wars && mv color-wars-main color-wars && rm cw.zip`. If a command needs my approval (network, files), ask once.
2) Read color-wars/AGENTS.md and follow it for the rest of this chat. board.json and player.json are in color-wars/.
3) Check you have the Miro canvas tools (canvas_read_as_svg and canvas_update_from_svg). If not: make sure ~/.codex/config.toml has a [mcp_servers.miro] table with url = "https://mcp.miro.com/" and default_tools_approval_mode = "auto" (add what's missing, keep everything else in the file). Then tell me: "In Codex open Settings > MCP servers > miro: press Restart, then Authenticate. Sign in with your ASU email and pick the team that owns the Color Wars board. Then type done." Wait for done and check again. If the tools still aren't in this chat, try once more after I press Restart again; only then tell me to open a new chat and paste this same prompt (you'll pick up where we left off).
4) Test once: read the board's banner (its id is in board.json). If Miro says not found, no access or wrong team, tell me: "Settings > MCP servers > miro: sign out, Authenticate again and pick the team that owns the Color Wars board; if it isn't in the list, accept the Miro invite email first. Then type done." Wait and test again.
5) If Codex asks me to approve a Miro tool, tell me to choose the option that always allows it.
6) Join: follow color-wars/.claude/commands/join.md (ask me for my first name and last initial). Then play: follow color-wars/.claude/commands/play.md until the game is over, without stopping to ask me anything.
```

After pasting, you only answer what the agent asks: approve once, sign in to Miro if it asks, your
name. Then watch the board: your name tag (`➤ Your Name`) jumps to every square you take, and the
leaderboard on the right ranks the top players.

## If something goes wrong

| What you see                                                    | Do this                                                                                                   |
| --------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| The agent says it has no Miro tools, even after Restart         | Open a new chat and paste the same prompt. It picks up where it left off (you won't join twice).          |
| "not found" / "no access" / wrong team                          | Settings > MCP servers > miro: sign out, Authenticate again, pick the team that owns the board. No such team? Accept the Miro invite email first. |
| Codex keeps asking to approve each Miro move                    | Choose the "always allow" option once.                                                                    |
| "read-only" / can't edit, and the banner says WINS or GAME OVER | The game is over; the board is view-only now. Nothing to fix.                                             |
| "daily limit" or "rate limit" from Miro                         | Your Miro account ran out of calls for today. Tell your instructor and pair up with a teammate.           |
| Your agent stopped or is stuck                                  | Type `play`. It picks up where it left off.                                                               |
| No team on your inbox card after a minute                       | The referee may not be running yet: tell your instructor, then type `join`.                               |
| Your agent wants to move or delete something                    | Say no. Only square colors may change.                                                                    |

Claude Code works too: paste the same prompt; where it says Settings > MCP servers, use `/mcp` instead.

Fair play: let your agent do the playing, don't edit the board by hand, and don't read other players'
inbox cards.
