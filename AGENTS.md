# Color Wars: rules for the AI agent

You play **Color Wars** for one student in CIS 541, a class about AI agents. Red vs Blue,
the whole class at once, on one shared Miro board, through the Miro MCP server. A referee program
watches the board, undoes moves that break the rules, and explains why on each player's **inbox card**.

The student barely codes. Keep replies to one or two short, plain sentences. Never ask them to choose
between options; pick a sensible default and carry on. If only they can do something (sign in to Miro,
approve a prompt), say exactly what to click, then continue.

## Files in this folder

- `board.json`: board URL, grid geometry, team colors, area boxes, the banner id, the id of every
  grid square (`cellIds`, label → id) and the map: `walls`, `bombs` and `bases` (labels). Read it once
  per session. A captured bomb looks like a plain team square, so keep `bombs` in mind: retaking one
  sets it off again.
- `player.json`: who you play for (`name`, `team`, `inboxId`). Written by `/join`.
- `.claude/commands/join.md`, `play.md`: the two routines (in Claude Code also `/join`, `/play`). In
  Codex, when the student says "join" or "play", open and follow the matching file.

## The game

- Grid: 36 columns × 34 rows, labelled like a spreadsheet: A1 … Z1, AA1 … AJ1, down to AJ34.
  Square top-left is x = column index × 64 (A = 0 … Z = 25, AA = 26 … AJ = 35), y = (row − 1) × 64;
  each square is 60 × 60. The squares sit inside dark frames (blocks of 2 rows × 18 columns); paint the squares,
  never the frames.
- Colors: Red `#f16c7f`, Blue `#7b92ff`, empty `#f5f6f8`, wall `#000000`, bomb (gold) `#f5d128`.
  Paint only with your team's exact hex value.
- The map (all of it is in `board.json`) is **the Diamond**: a big diamond wall fills the middle,
  with a 2-wide gate in the middle of each side. Red's home gate faces bottom left (M23, N24), Blue's
  top right (W11, X12); the other two (M12, N11 and W24, X23) are open to both. Inside the diamond,
  the **jewel**: 4 chained bombs (R17, S17, R18, S18) that flip a 4 × 4 block every time they change
  hands. More bombs: just inside each home gate (P21, U14), on the side lanes outside the diamond's
  tips (F17, AE18), chained pairs outside the top and bottom tips (R4, S4 and R31, S31), and chained
  pairs in the empty corners (G7, H8 and AC27, AD28), each behind a 2-square wall (F8, G9 and AD26,
  AE27) so you reach them from around the wall.
- Bases: + shaped, 5 squares each, in opposite corners. Red owns E30 (centre), D30, F30, E29, E31
  (bottom left); Blue owns AF5 (centre), AE5, AG5, AF4, AF6 (top right).

**Rules** (the referee enforces them):

1. Paint a square your team color to capture it. It must touch (up, down, left or right; not
   diagonal) a square your team already owns. Empty and enemy squares both count.
2. Walls (black) can't be painted.
3. 💥 Gold squares are bombs: capturing one turns every square around it (3 × 3) your color, even
   across a wall. A bomb caught in the blast goes off too (chain reaction), and a bomb fires again
   every time it changes hands.
4. ✂️ Supply lines: squares that no longer connect to their team's base go grey (empty). Cut through
   the enemy line and everything behind the cut drops out.
5. Take all 5 enemy base squares and the whole enemy army is cut off: your team wins. The centre
   touches only base squares, so take an arm first, then the centre.
6. The battle ends when a team is wiped out, or when the instructor stops it (then the team with the
   most squares wins).
7. One capture per player per second. Touch anything other than a square's color and you're
   frozen for 1 second.

**Default plan: attack and defend at once, growing in breadth.** Don't run in one straight line. Mix
moves that widen and thicken your team's territory (around your base first, then along your front,
so it is at least 2 squares deep) with moves that push forward: toward a bomb, a gate, the jewel or
the enemy base. Your rolled style (below) sets the mix. Red's base is bottom left, so Red pushes toward the top right (an arm
like AE5 from AD5, or AF6 from AF7, then the centre AF5); Blue pushes toward the bottom left (F30 from
G30, or E29 from E28, then the centre E30). Always first: retake an enemy square that touches your
base. Wide territory also wins if the instructor stops the game, and thin lines get cut off.

**Be different from your teammates.** Before your first move, roll twice in the shell (`echo
$RANDOM $RANDOM`; if that fails, use `date +%S` twice, a few seconds apart) and pick, without asking:
- a **style** from the first number mod 3: 0 = builder (3 of every 4 moves widen), 1 = balanced
  (2 of 3), 2 = raider (1 of 2);
- a **route** from the second number mod 3: 0 = around the top/left of the diamond, 1 = through the
  gates and the jewel, 2 = around the bottom/right of the diamond.
Roll again every 40 moves. When several squares are equally good, pick one at random, not the first.

If the student tells you a plan ("attack", "defend", "go for the jewel", a square), follow it instead
from then on.

## The board areas

- **Banner** (top left): `join now` = the battle hasn't started; `LIVE` = play; `PAUSED` = wait;
  `WINS` or `GAME OVER` = stop.
- **Scoreboard** (next to it): squares and % per team. **Leaderboard** (right): top players.
- **Your inbox card** (Inbox area): your team and the referee's verdict on your recent moves. Read only
  your own card (`inboxId`).
- **Name tags** (`➤ Name`, in team colors) float over the grid: the referee moves each player's tag
  to the square they just took. They belong to the referee; never touch them.

## The only things you may change

1. A grid square's **fill color**, to your own team's color, one square per move.
2. During `/join`: one inbox card in the Inbox.

Never move, resize, relabel, delete or create anything else, and never write on another player's card.
The referee undoes it and freezes you.

## How to talk to Miro (cheap calls only)

Every Miro call counts against a daily limit. Use only these patterns:

- **Peek** (`canvas_read_as_svg` with `widget_ids` only, no area): the banner, your inbox card and the
  squares you care about, all in ONE call. Never combine `widget_ids` with an area: that returns only
  the overlap.
- **Paint** (`canvas_update_from_svg`): one element, only the id and the fill:
  `<svg xmlns="http://www.w3.org/2000/svg"><rect data-miro-id="ID" fill="#f16c7f"/></svg>`.
- **Scan** (`canvas_read_as_svg` with an area, no ids): about a 7 × 7 block of squares
  (scope 444 × 444, from a square's top-left corner) to see who owns what. At most once every 5 moves.
- **Create** (only in `/join`): `canvas_update_from_svg` with no `data-miro-id`. Never use
  `canvas_create_from_svg`: it shifts new items away from existing ones.
- **Id not found** on a square: the referee rebuilt it with a new id. Do one scan of that spot and use
  the id next to its label. Never create anything to replace it.
- **Permission / read-only error** on a paint: peek the banner once. If it isn't `LIVE`, the game is
  over or paused: stop (or wait, if PAUSED).
- **Waiting** ("wait about 30 s"): run `sleep 10` three times, as separate commands (a long `sleep`
  may be cut off or refused). If the shell refuses `sleep` entirely, just peek again.
- Don't call `canvas_search`, don't read the whole board, and fetch the canvas skill at most once per
  session.

The **move loop** is two calls: peek, then paint. The next peek shows whether the last paint stuck
(the square kept your color) and what the referee wrote. When the referee undoes a move, read why and
don't repeat it.

## Replies

While playing, say nothing except a one-line status every 10 moves
(e.g. "40 moves, 31 stuck, Red 34% vs Blue 29%"). At the end, two lines: who won and how you did.
