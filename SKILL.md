---
name: glyph-theater
description: "Make tiny animated ASCII stories with classic characters, like the DevDay frog: a character wants something, goes for it its own way, something goes a little wrong, it reacts. Use when someone asks for ASCII art, character art, or small glyph animations and interactions."
---

# Glyph Theater: tiny ASCII stories

The output is **a small web page that plays by itself**: one ASCII character, one small object, one tiny story. Think of the frog waiting for a bug on the OpenAI DevDay 2026 page.
No big composed scenes. One character and one interaction are enough.

## Who uses it, and where
Ask where it will live first. That decides the size and what you hand over:
- **Easter eggs in a site or product**: loading, 404, empty states, "sent". Small, loops forever, never steals focus → size `small`, deliver a snippet that can be embedded in a page.
- **Holiday cards / mini sites**: open and watch, easy to share → size `card`, deliver a standalone page (publish it as an artifact if you can).
- **Social posts**: → capture frames with the script in Appendix D, then turn them into a GIF or video with render.py (Appendix A) or ffmpeg.
- **Portfolio or personal site decoration**: → size `wide` or `card`, always with light and dark themes.
If nobody says, make a `card`-size standalone page.

## The core logic (most important)

**Start from what the character wants, not from "make it move".** Write the story first; motion follows from it.

Every interaction is a four-beat story:
1. **Wants**: the character wants something (catch the bug, catch the ball, not get eaten, get the cup off the table)
2. **Does**: goes for it using its **nature** (the frog shoots its tongue, the cat pushes with a paw, the dog jumps, the gingerbread man runs)
3. **Twist**: the result is a little unexpected (the tongue is absurdly long, the ball is too big for the mouth, a snowflake lands on the nose, sometimes it fails)
4. **Reacts**: the reaction shows personality. Pick from the reaction list: **freeze** (stop for a second or two), **slowly turn to look at you**, **pretend nothing happened** (look away, carry on), **try again out of spite**, **proud** (small hop, squint), **hide, then peek back**. What's funny is the attitude, not the effect. **Shaking, jolting, "boom" are seasoning at most: once per story at most, and not every story needs one.** Overused, it just looks twitchy.

Before building, write the four beats in one sentence. If you can't, or there is no twist, pick another story.

**It also needs tension**: viewers have to know what they're waiting for, and there has to be a moment where they hold their breath. Before building, name where the tension comes from. If you can't, drop the story:
1. **Almost there**: the thing is right there, and it gets closer bit by bit (the cat pushing the cup to the edge, the dog crouching for the ball)
2. **You know first**: the danger is already coming and the character hasn't noticed (a snowball rolling up behind the reindeer, a fork closing in on the gingerbread man)
3. **Can it hold back**: temptation is right there and the character is resisting (the yarn ball twitching while the cat knits)

A vague wish like "wants to see a snowflake clearly" has no tension. Neither does the same result three times in a row (the snowflake melting three times). Move step by step toward one result.

## Checklist (run it once the story is written)
- **Nature**: is this something the character would naturally do? Viewers should get it without explanation. Forced actions (a reindeer switching on lights, a person juggling) feel random.
- **Pace is slow → stop → fast → stop**: notice slowly (eyes follow), stop and build up, strike suddenly (1–3 frames), stop and react. Even pacing never surprises.
- **Exaggerate one thing**: the action should go beyond the character's own body, like a tongue across half the frame or a hat sneezed into the sky.
- **Not always a success**: about 30% fail (miss, ball bounces off the head, gets stuck), so people want to watch again.
- **The character moves on its own**: nothing depends on clicking. Clicks and the mouse only "add something" (drop a bug, throw a ball) or get "noticed" (the gingerbread man runs when the mouse comes close).
- **One character, one small object, one action.** More gets messy.
- **It loops**: one round ends and the next starts naturally (a new bug flies in, a new cup appears).
- **Readable at a glance** (from early blind tests): use the way everyone writes an object, not your own shorthand (`[+]` as a gift failed); the object can't be much bigger than the character; when they interact, they have to touch.
- **Props must read without the title**: a lone `*` on white is not a firefly to anyone. If it doesn't read, switch to something people know at a glance (snowflake, snowball, a winged bug `c•ɔ`).
- **The twist must be big enough to see**: two eyes blinking in sync goes unnoticed.
- **Has tension**: you can say whether it's "almost there", "you know first" or "can it hold back".
- **Reactions don't repeat**: on one page, the stories shouldn't all end in a shake or a "boom".

## Workflow
1. **Pick a character** from the library. Keep its shape; change only the eyes, mouth and hands for expressions and actions. If the library doesn't have it, look on asciiart.eu first; if that fails, trace an open-source icon (Lucide, Tabler; see Appendix C). Never draw from scratch.
2. **Come up with 3–5 tiny stories**, each written as one four-beat sentence, and let the person pick. Use the examples below as reference: those are ones people picked.
3. **Once picked, write the script**: copy the engine in Appendix B. **Don't touch the engine; write only the script** (actors, anchors, which actions in each beat). Actions and reactions come from the vocabulary; don't write animation code from scratch. Only when the vocabulary really lacks a move, add it as a function (see "Adding a move" in Appendix B). Every actor needs at least `idle` and `blink`; if it uses reactions, add `blank` `camera` `away` `proud`.
4. **Self-check automatically**: run check.py from Appendix D. It reports errors, overlapping glyphs, anything resting off frame, too much accent and more than one shake. It also screenshots every "landing" (something arriving at an anchor). Fix until there are no warnings.
5. **Blind test**: look only at the frame strip check.py exports (no title, no description) and say in one sentence what happens. If you can hand work to a sub-agent, give it to one that hasn't seen the script. If the sentence is wrong or missing, the story isn't clear: change the prop, make the twist bigger, test again.
6. **Let the person pick**: put several stories side by side on one page, each card with "Keep / Skip".
7. **Collect and re-test**: add the ones people like to the examples and the ones they don't to "What failed". **After any change to the engine or this skill, run check.py on the example scripts in Appendix B again** to make sure nothing broke.

## Visuals
- **Black and white plus one accent color**: the character and everything else in ink (`#111` in light mode, `#f2f2f2` in dark). Each story keeps **one accent color**, two at most, on the thing the character wants: the bug, the ball, the cup, the carrot nose, the spell light, the Santa hat. The accent sits on one or two glyphs only (just the nose `>`, not the whole face). State changes (like being covered in soot) are shown by swapping glyphs or opacity, never a new color.
- Every page has a **light / dark toggle**: colors come from CSS variables `--ink` `--bg` `--acc`, never hard-coded; the accent stays the same in both modes.
- Mostly plain ASCII; no Japanese kana; a few common symbols like `°` `¬` `•` are fine.
- Monospace. The frame is laid out in cells (columns × rows); the engine sizes the font from the frame width, stories never use pixels. Characters rest on whole cells; while flying, drifting or falling they can sit between cells.
- No captions, no lines of code on screen.
- **No artist names or initials in anything you generate**: not in the picture, page text or card descriptions (strip initials like jgs, hjw, ldb, mrf from archive pieces first). Artists are credited only in this skill's library and the Credits at the end.
- For a paid or commercial product, contact the artists of archive pieces, or switch to traced open-source icons or your own drawings.

## Examples (picked by people; the accent goes to what's in brackets)
- **Cat pounces on a bug** (bug): wants the bug → stares `°.°`, crouches `>.<`, raises a paw `\( o.o )/` → sometimes misses → hit `^.^`, miss `;.;`. Click empty space to drop a bug.
- **Cat pushes the cup off the table** (cup): wants the cup on the floor → push, look back at you, stop, push again → hesitates a long time before the last push → the cup spins down and smashes; the cat looks at you `-.-` as if nothing happened.
- **Gingerbread man runs** (buttons): doesn't want to be eaten → when the mouse or a fork `---E` comes close he's terrified `(O.O)` and runs off frame → later peeks in from the edge `(¬.¬)` → it's safe, he walks back.
- **Dog catches the ball** (ball): wants the ball → stares, crouches, jumps → catches it but the ball is too big and gets stuck `V(  O  )V`, tail still wagging; 30% of the time the ball bounces off its head → freeze `. .`, turn to you `• •`, look away as if nothing happened.
- **Wizard makes a star** (the star): a thought bubble `. o *` shows he wants a star → sparks gather on the staff, he holds it → pop, and out comes a bug that circles his head → he glares `¬¬`, then turns to you.
- **Santa and the chimney** (Santa hat; tension: you know first): smoke is already rising from the chimney → he peeks left and right and dives in anyway → a beat later he launches out, falls back, freezes, looks at you. The chimney carries a background color (`mask:true`) so it hides the part of Santa that's inside.
- **Cat knits a scarf** (yarn ball; tension: can it hold back): knits head down `-.-`, needles `\/` `/\`, the scarf grows row by row `vvv`, a dotted line runs to the yarn ball `@` → the ball moves, the cat stares `°.°` and holds back, knits on → it moves again, the cat squeezes `>.<`, then swipes → the ball rolls off frame and the scarf unravels row by row → the cat freezes, looks at you, knits air.
- **Fish steals the bait** (worm): off-library character using the well-known one-liner `><(((º>` / `<º)))><` → swims, spots the worm on the hook, creeps up, stops, nibbles twice → the line yanks up: 70% it escapes with the worm and blows bubbles, 30% it's pulled up, drops back, eyes `x`.
- **Peeking over the wall** (hat + `!`): fingers first `ooO` `Ooo`, then the hat, looks left and right → another head pops up on the other side, staring back → both go wide-eyed with `!` → both duck down.
- **Knight on high alert** (snail): something small crawls in from afar `_@)` → alert, shield up `[#]`, holds for a long time → the plume `!` turns `?`: it's only a snail → the plume droops `!`→`,`→`.`.
- **Reindeer and the snowball** (red nose; tension: you know first): the reindeer looks up `° °`, lost in the falling snow → behind it a small snowball `o` rolls in and grows `O` → `.-.` → three rows, four rows; halfway its eyes slowly come back and you think it'll notice, but it looks up again → hit: it becomes a snow pile with only the red nose blinking → one shake clears the snow, the antlers still wear some `.*.`, it freezes `. .`, turns to you `• •`, pretends nothing happened. 30% of the time it hops up for a snowflake right then and the ball rolls underneath; it never knows. Light snow keeps falling behind (faded).

What failed (don't do this again):
- Reindeer switching on lights, a person juggling, a gift box that opens on click, a gingerbread man that gets bitten on click: not the character's nature, or entirely click-driven, so it feels random.
- A baby who wants to sleep: the "baby" piece doesn't look like a baby. If the shape doesn't read, no story can save it; replace the piece first.
- Reindeer and a firefly: a `*` on white doesn't read as a firefly, and the twist (blinking together) was too subtle to see.
- Reindeer sneezing (a bug or snowflake lands on the nose → boom): the reaction is another jolt plus burst, the same as the snowman sneeze; every story ending in a jolt, shake or "boom" looks twitchy.
- Reindeer and snowflakes (they melt on the warm nose before landing): no tension. The wish is vague, so viewers don't know what to wait for, and the snow melting three times is the same result three times.
- Tilde `~` scarves and a cat "wrapped in yarn" `~(^.^)~`: viewers couldn't tell what the tildes were. Use shapes that read as the thing (`vvv` for knitting).

## Character library
### Cat · front
Source: asciiart.eu · archive
```
 /\_/\
( o.o )
 > ^ <
```
### Cat · squinting, with tail
Source: asciiart.eu · archive
```
  /\_/\  (
 ( ^.^ ) _)
   \"/  (
 ( | | )
(__d b__)
```
### Dog · floppy ears
Source: asciiart.eu · Hayley Jane Wakenshaw
```
,-.___,-.
\_/_ _\_/
  )O_O(
 { (_) }
  `-^-'
```
### Dog · sitting
Source: asciiart.eu · archive
```
  /^ ^\
 / 0 0 \
 V\ Y /V
  / - \
 /    |
V__) ||
```
### Dog · walking, side view
Source: asciiart.eu · Hayley Jane Wakenshaw
```
   __
o-''|\_____/)
 \_/|_)     )
    \  __  /
    (_/ (_/
```
### Dog · lying down
Source: asciiart.eu · Joan G. Stark
```
           __
      (___()'`;
      /,    /`
      \\"--\\
```
### Coffee and cereal
Source: asciiart.eu · Hayley Jane Wakenshaw
```
  ;)( ;
 :----:     o8Oo./
C|====| ._o8o8o8Oo_.
 |    |  \========/
 `----'   `------'
```
### Christmas tree
Source: asciiart.eu · Joan G. Stark
```
          *
         /.\
        /..'\
        /'.'\
       /.''.'\
       /.'.'.\
"'""""/'.''.'.\""'"'"
      ^^^[_]^^^
```
### Small snowman
Source: asciiart.eu · Joan G. Stark
```
         _
       _[_]_
        (")
    `--( : )--'
      (  :  )
    ""`-...-'""
```
### Snowman with hat
Source: asciiart.eu · Laura Brown
```
     __
   _|==|_
    ('')___/
>--(`^^')
  (`^'^'`)
  `======'
```
### Santa
Source: asciiart.eu · Max Strandberg
```
  .-""-.
 /,..___\
() {_____}
  (/-@-@-\)
  {`-=^=-'}
  {  `-'  }
   {     }
    `---'
```
### Wreath
Source: asciiart.eu · Joan G. Stark
```
     _...._
   .::o:::::.
  .:::'''':o:.
  :o:_    _:::
  `:(_>()<_):'
    `'//\\''
     //  \\
    /'    `\
```
### Person · peeking over a wall
Source: asciiart.eu · archive
```
         ____
       _(____)_
___ooO_(_o__o_)_Ooo___
```
### Person · baby (not recommended: it does not read as a baby)
Source: asciiart.eu · Glory Py Yip
```
       __
      (  )
       )(
    ,;/  \:.
   ( `.__,' )
    `-.__,-'
     ((__))
      `--'
```
### Person · bride and groom
Source: asciiart.eu · Joan G. Stark
```
           _
    mMm  _[_]_
   /(_)\  (_)
  //)^(\\//:\\
 /(/&@&\)\|~|/
/ /-~`~-\ |||
`/       \|||
`--------'-'--
```
### Person · king
Source: asciiart.eu · Joan G. Stark
```
      _.+._
    (^\/^\/^)
     \@*@*@/
     {_____}
    ///"""\\\
    (/6   6\)
     ||=^=||
     \\\\///
      \\///
       `)/
```
### Person · knight
Source: asciiart.eu · Joan G. Stark
```
       !
      .-.
    __|=|__
   (_/`-`\_)
   //\___/\\
   <>/   \<>
    \|_._|/
     <_I_>
      |||
     /_|_\
```
### Person · wizard
Source: asciiart.eu · H P Barmario
```
     __/\__
. _  \\''//
-( )-/_||_\
 .'. \_()_/
  |   | . \
  |   | .  \
 .'. ,\_____'.
```
### Person · throwing a ball
Source: asciiart.eu · archive
```
  .
    .
. O _
_/=/
  |
_/ \_
```
### Person · skating
Source: asciiart.eu · Joan G. Stark
```
      _
     { ) __
 ,---'_ .---'
       \ \___
       //'---,\
       \\
       =='
```
### Person · beach volleyball
Source: asciiart.eu · llizard
```
            /    (%)
            \O_
              \|     O_\
              /\/   _/\
              \       /\
                     |  \
```
### Star
Source: asciiart.eu · archive
```
   ,
__/ \__
\     /
/_   _\
  \ /
   '
```
### Gingerbread man
Source: traced from Tabler(MIT)
```
   .-.
  (o.o)
 .-) (-.
 '-.:.-'
   / \
  (_/ \_)
```
### Stocking
Source: traced from Tabler(MIT)
```
  .----.
  |====|
  |    |
  |    |
 .'    |
(      /
 '----'
```
### Candy cane
Source: traced from Lucide(ISC)
```
   .--.
  //  \\
  ''  |/|
      |/|
      |/|
      '-'
```
### Reindeer
Source: asciiart.eu · archive
```
\|/    \|/
  \    /
   \_/  ___   ___
   o o-'   '''   '
    O -.         |\
        | |'''| |
         ||   | |
         ||    ||
         "     "
```
### Ornament light
Source: asciiart.eu · Joan G. Stark
```
  _Y_
 [___]
 /:' \
|::   |
\::.  /
 \::./
  '='
```
### Stocking · archive version
Source: asciiart.eu · Susie Oviatt
```
 @@@@@@@
 `::::::
  ::::::
.::::::::
`:::::::'
```
### Gift box
Source: traced from Lucide(ISC)
```
   .-.   .-.
  (   `.'   )
 .-----|-----.
 '-----|-----'
  |    |    |
  |    |    |
  '----'----'
```
### Coffee cup
Source: traced from Lucide(ISC)
```
   ) ) )
 .------.
 |      |-.
 |      |_/
 '.____.'
```

## Appendix A: stills, GIFs, plain text (optional)
The interactive page is the main output. For a still or a GIF, screenshot the best beat in a browser, or write the character's states as a `.art` file and render PNG, GIF and plain text with render.py below. Colors follow the same "black and white plus one accent".

`.art` format: `@params` at the top (`@bg` background, `@fg` text color, `@c.name` palette, `@cell` cell height, `@cols` `@rows` canvas, `@pad` padding, `@fps` frame interval), then the art itself; color with `⟦name|text⟧`; a line with only `===` starts a new frame. Usage: `python3 render.py file.art`.

```python
#!/usr/bin/env python3
"""Char-art renderer: lays a .art file onto a monospace grid and outputs PNG (first frame), GIF (when there are several frames) and plain TXT.
@bg background / @fg default text color / @c.name palette (use ⟦name|text⟧ in the body) / @cell cell height px / @pad padding in cells / @fps frame interval ms / @weight regular|bold / @cols @rows fixed canvas (content is centered; @center: no turns it off)
Separate frames with a line containing only ===. Usage: python3 render.py in.art [out_dir]
"""
import sys, os, re, unicodedata, subprocess
from PIL import Image, ImageDraw, ImageFont

def font_path(weight):
    f = 'NotoSansCJK-Bold.ttc' if weight == 'bold' else 'NotoSansCJK-Regular.ttc'
    for d in ('/usr/share/fonts/opentype/noto', '/System/Library/Fonts', os.path.expanduser('~/Library/Fonts')):
        p = os.path.join(d, f)
        if os.path.exists(p): return p
    return subprocess.run(['fc-match', '-f', '%{file}', 'Noto Sans Mono CJK JP'], capture_output=True, text=True).stdout

def face_index(path, family='Noto Sans Mono CJK JP'):
    for i in range(12):
        try:
            f = ImageFont.truetype(path, 20, index=i)
            if f.getname()[0] == family: return i
        except Exception: break
    return 0

def cw(ch):
    if ch == '\t': return 4
    return 2 if unicodedata.east_asian_width(ch) in ('W', 'F') else 1

TOK = re.compile(r'⟦([\w.-]+)\|(.*?)⟧')
def parse_line(line, fg):
    out, i = [], 0
    for m in TOK.finditer(line):
        out += [(c, fg) for c in line[i:m.start()]]
        out += [(c, m.group(1)) for c in m.group(2)]
        i = m.end()
    out += [(c, fg) for c in line[i:]]
    return out

def load(path):
    meta, body = {}, []
    for raw in open(path, encoding='utf-8').read().split('\n'):
        if raw.startswith('@') and ':' in raw and not body:
            k, v = raw[1:].split(':', 1); meta[k.strip()] = v.strip()
        else: body.append(raw)
    frames, cur = [], []
    for l in body:
        if l.strip() == '===': frames.append(cur); cur = []
        else: cur.append(l)
    frames.append(cur)
    frames = [[l.rstrip() for l in f] for f in frames]
    lead = min((next((i for i, l in enumerate(f) if l.strip()), len(f)) for f in frames), default=0)
    frames = [f[lead:] for f in frames]
    for f in frames:
        while f and f[-1] == '': f.pop()
    return meta, frames

def render(path, outdir=None):
    meta, frames = load(path)
    name = os.path.splitext(os.path.basename(path))[0]
    outdir = outdir or os.path.dirname(os.path.abspath(path))
    pal = {k[2:]: v for k, v in meta.items() if k.startswith('c.')}
    bg = meta.get('bg', '#0b0b0b'); pal['fg'] = meta.get('fg', '#f2f2f2')
    H = int(meta.get('cell', 22)); W = H / 2; pad = int(meta.get('pad', 2)); S = 2
    fp = font_path(meta.get('weight', 'regular'))
    font = ImageFont.truetype(fp, int(H * 0.86 * S), index=face_index(fp))
    parsed = [[parse_line(l, 'fg') for l in f] for f in frames]
    cols = max([sum(cw(c) for c, _ in l) for f in parsed for l in f] + [int(meta.get('cols', 0))])
    rows = max([len(f) for f in parsed] + [int(meta.get('rows', 0))])
    bw = max(sum(cw(c) for c, _ in l) for f in parsed for l in f) if any(parsed) else 0
    bh = max(len(f) for f in parsed)
    ox = (cols - bw) // 2 if meta.get('center', 'yes') == 'yes' else 0
    oy = (rows - bh) // 2 if meta.get('center', 'yes') == 'yes' else 0
    Wpx = int((cols + pad * 2) * W * S); Hpx = int((rows + pad * 2) * H * S)
    imgs = []
    for f in parsed:
        im = Image.new('RGB', (Wpx, Hpx), bg); d = ImageDraw.Draw(im)
        for r, line in enumerate(f):
            x = pad + ox
            for ch, col in line:
                w = cw(ch)
                if not ch.isspace():
                    cx = (x + w / 2) * W * S; cy = (pad + oy + r + 0.5) * H * S
                    d.text((cx, cy), ch, font=font, fill=pal.get(col, col), anchor='mm')
                x += w
        imgs.append(im)
    imgs[0].save(os.path.join(outdir, name + '.png'))
    if len(imgs) > 1:
        imgs[0].save(os.path.join(outdir, name + '.gif'), save_all=True, append_images=imgs[1:],
                     duration=int(meta.get('fps', 500)), loop=0)
    plain = '\n'.join(TOK.sub(lambda m: m.group(2), l) for l in frames[0])
    open(os.path.join(outdir, name + '.txt'), 'w', encoding='utf-8').write(plain + '\n')
    return name, len(imgs), cols, rows

if __name__ == '__main__':
    print(render(sys.argv[1], sys.argv[2] if len(sys.argv) > 2 else None))
```

## Appendix B: interaction engine v2 (copy it whole, edit only the script)
Page layout: a grid with one card per story; each card has "Keep / Skip", plus "Copy my picks" and "Light / dark" at the bottom so the person can choose. Use the same layout even for a single story.
All positions are in cells: `[col, row]`. The engine measures glyph width and line height; stories never use pixels.

```html
<!DOCTYPE html><html lang="en"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<title>Tiny stories</title><style>
:root{--bg:#fff;--ink:#111;--page:#f4f4f2;--card:#fff;--sub:#888}
body.dark{--bg:#0c0f14;--ink:#f2f2f2;--page:#05070a;--card:#0c0f14;--sub:#8a919c}
body{margin:0;background:var(--page);font-family:-apple-system,system-ui,sans-serif;color:var(--ink)}
main{padding:28px 28px 90px;max-width:1240px;margin:auto}h1{font-size:22px;margin:0 0 6px}p.h{color:var(--sub);font-size:13px;margin:0 0 18px;line-height:1.7}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(420px,1fr));gap:16px}
.card{background:var(--card);border-radius:14px;box-shadow:0 1px 2px rgba(0,0,0,.05),0 6px 18px rgba(0,0,0,.04);overflow:hidden}
.st{position:relative;min-height:220px;overflow:hidden;cursor:pointer;background:var(--bg)}
.box{overflow:hidden}
.cap{padding:10px 14px 12px;font-size:13px}.cap b{display:block;margin-bottom:2px}.cap span{color:var(--sub);font-size:12px}
.chips{display:flex;gap:6px;margin-top:8px}.chip{border:1px solid var(--sub);background:transparent;color:var(--ink);border-radius:999px;padding:4px 12px;font-size:12px;cursor:pointer}.chip.on{background:var(--ink);color:var(--bg)}
.g{position:absolute;white-space:pre;font:600 1em/1.15 "SF Mono",ui-monospace,Menlo,"DejaVu Sans Mono",monospace;color:var(--ink);pointer-events:none}
.bar{position:fixed;left:0;right:0;bottom:0;background:color-mix(in srgb,var(--page) 90%,transparent);backdrop-filter:blur(8px);padding:12px 28px;display:flex;gap:14px;align-items:center}
.bar button{border:0;background:var(--ink);color:var(--bg);border-radius:10px;padding:10px 20px;font:inherit;font-size:14px;cursor:pointer}.bar span{font-size:12px;color:var(--sub)}
#theme{background:transparent;color:var(--ink);border:1px solid var(--sub)}
</style></head><body><main><h1>Tiny stories</h1><p class="h">One line about what to look at.</p>
<div class="grid" id="g"></div></main>
<div class="bar"><button id="copy">Copy my picks</button><button id="theme">Light / dark</button><span id="st"></span></div>
<script>
// ===== Engine: copy as is, don't edit =====
// Everything is in grid cells: x = column, y = row. The engine measures glyph width and line height; stories never use pixels.
const SIZES={card:{cols:36,rows:14},wide:{cols:72,rows:12},small:{cols:26,rows:8}};
const Q=new URLSearchParams(location.search);const SPEED=+(Q.get('speed')||1);
const esc=s=>s.replace(/&/g,'&amp;').replace(/</g,'&lt;');
const md=(s,c)=>esc(s).replace(/⟦(\w)\|(.*?)⟧/g,(m,k,t)=>`<span style="color:${(c&&c[k])||'var(--acc)'}">${t}</span>`);
const plain=s=>s.replace(/⟦\w\|(.*?)⟧/g,'$1');
let NOW=0;const TW=[];
function tween(ms,fn){return new Promise(res=>TW.push({t0:NOW,ms:Math.max(ms,1),fn,res}))}
const sleep=ms=>tween(ms,()=>{});
const EASE={inout:p=>p<.5?2*p*p:1-Math.pow(-2*p+2,2)/2,out:p=>1-(1-p)*(1-p),in:p=>p*p};
// Paths return [x progress, y progress, x offset (cells), y offset (cells)]
const PATH={
 slide:p=>[EASE.inout(p),EASE.inout(p),0,0],                       // push, slide
 walk:p=>[p,p,0,-Math.abs(Math.sin(p*Math.PI*6))*.25],              // walk: a little bob
 drift:p=>[p,p,Math.sin(p*9)*1.4*(1-p),0],                          // drift: snowflakes, feathers
 fly:p=>[EASE.inout(p),EASE.inout(p),Math.sin(p*14)*1.2*(1-p),Math.cos(p*11)*.8*(1-p)], // fly: bugs
 settle:p=>[Math.pow(p,2.5),p,Math.sin(p*9)*(1-p),0],                  // drift down, move in at the end: avoids bodies in the way
 fall:p=>[p,EASE.in(p),0,0],                                        // fall: speeds up
 hop:(p,h)=>[p,p,0,-4*(h||1)*p*(1-p)],                              // small hop
 arc:(p,h)=>[p,p,0,-4*(h||4)*p*(1-p)],                              // arc: a throw
};
const STORIES=[];
function story(def){STORIES.push(def)}

function actor(S,name,def){
 const e=document.createElement('div');e.className='g';S.box.appendChild(e);
 if(def.z)e.style.zIndex=def.z;if(def.color)e.style.color=def.color;if(def.mask)e.style.background='var(--bg)';
 const a={name,def,poses:def.poses,anchors:def.anchors||{},pin:def.pin||[0,0],x:0,y:0,ox:0,oy:0,pose:null,vis:true,moving:0,e,
  set(p){if(!(p in a.poses))throw Error(`"${name}" has no pose "${p}"`);if(a.pose!==p){a.pose=p;e.innerHTML=md(a.poses[p],def.acc)}return a},
  draw(){e.style.left=(a.x+a.ox)*S.cw+'px';e.style.top=(a.y+a.oy)*S.lh+'px';e.style.visibility=a.vis?'visible':'hidden'}};
 return a}

// Target: [col,row] / {dx,dy} (relative to itself) / 'actor.anchor' / ['actor.anchor',dx,dy]
function pt(S,a,t){let x,y;
 if(typeof t==='string'||typeof t[0]==='string'){const [ref,dx=0,dy=0]=typeof t==='string'?[t]:t;const [n,k]=ref.split('.');const b=S.A[n];
  if(!b)throw Error(`no actor "${n}"`);const an=k?b.anchors[k]:[0,0];if(!an)throw Error(`"${n}" has no anchor "${k}"`);x=b.x+an[0]+dx;y=b.y+an[1]+dy}
 else if(!Array.isArray(t)){x=a.x+a.pin[0]+(t.dx||0);y=a.y+a.pin[1]+(t.dy||0)}
 else [x,y]=t;
 return [x-a.pin[0],y-a.pin[1]]}

// Reaction vocabulary. Shaking is just one of them: at most once per story.
const REACT={
 freeze:async(S,a)=>{a.set('blank');await sleep(1400)},                         // freeze
 turn:async(S,a)=>{await sleep(350);a.set('camera');await sleep(1300)},          // slowly turn to look at you
 pretend:async(S,a)=>{a.set('away');await sleep(1200);a.set('idle');await sleep(500)}, // pretend nothing happened
 proud:async(S,a)=>{a.set('proud');await run(S,['move',a.name,{dy:-.7},170,'slide']);await run(S,['move',a.name,{dy:.7},170,'fall']);await sleep(1100)}, // proud
 shake:async(S,a)=>{S.shakes++;await tween(320,p=>{a.ox=p<1?Math.sin(p*40)*.25:0});a.ox=0}, // shake (once at most)
};
const NEEDS={freeze:['blank'],turn:['camera'],pretend:['away','idle'],proud:['proud']};

async function run(S,b){
 if(typeof b==='function')return b(S);
 const [op,...r]=b;const A=n=>{const a=S.A[n];if(!a)throw Error(`no actor "${n}"`);return a};
 switch(op){
  case 'wait':return sleep(r[0]);
  case 'pose':{const a=A(r[0]);a.set(r[1]);a.vis=true;return}
  case 'hide':A(r[0]).vis=false;return;
  case 'place':{const a=A(r[0]);[a.x,a.y]=pt(S,a,r[1]);if(r[2])a.set(r[2]);a.vis=true;return}
  case 'move':{const [n,to,ms,style='slide',h]=r;const a=A(n);const x0=a.x,y0=a.y;const [x1,y1]=pt(S,a,to);a.vis=true;a.moving++;
   await tween(ms,p=>{const q=PATH[style](p,h);a.x=x0+(x1-x0)*q[0]+q[2];a.y=y0+(y1-y0)*q[1]+q[3]});a.x=x1;a.y=y1;a.moving--;
   if(typeof to==='string'||typeof to[0]==='string')S.mark(`${n} → ${typeof to==='string'?to:to[0]}`);return}
  case 'nudge':return run(S,['move',r[0],{dx:r[1],dy:r[2]||0},r[3]||260,'slide']);
  case 'frames':{const [n,ps,every=150,total]=r;const a=A(n);a.vis=true;const ms=total||ps.length*every;
   await tween(ms,p=>{const i=Math.floor(p*ms/every);a.set(ps[total?i%ps.length:Math.min(i,ps.length-1)])});return}
  case 'blink':{const a=A(r[0]);const p=a.pose;a.set(r[1]||'blink');await sleep(140);a.set(p);return}
  case 'react':return REACT[r[1]](S,A(r[0]));
  case 'par':return Promise.all(r.map(x=>run(S,x)));
  case 'seq':for(const x of r)await run(S,x);return;
  case 'maybe':{const yes=Math.random()<r[0];S.mark(yes?'took the twist branch':'took the usual branch');for(const x of (yes?r[1]:(r[2]||[])))await run(S,x);return}
  case 'mark':S.mark(r[0]);return;
  default:throw Error('unknown action '+op)}}

function lint(S){const d=S.def;
 const walk=b=>{if(!Array.isArray(b))return;if(b[0]==='react'){const a=d.cast[b[1]];(NEEDS[b[2]]||[]).forEach(p=>{if(!a||!(p in a.poses))S.warn(`reaction "${b[2]}" needs "${b[1]}" to have pose "${p}"`)})}b.forEach(walk)};
 walk(d.beats);const n=JSON.stringify(d.beats).split('"shake"').length-1;if(n>1)S.warn(`script shakes ${n} times, max 1`)}

function mount(st,def){
 const {cols,rows}=SIZES[def.size||'card'];
 const probe=document.createElement('div');probe.className='g';probe.style.fontSize='100px';probe.style.visibility='hidden';probe.textContent='MMMMMMMMMM\nM';st.appendChild(probe);
 const r=probe.getBoundingClientRect();probe.remove();const rw=r.width/1000,rl=r.height/200;
 const fs=Math.min((st.clientWidth-24)/(cols*rw),40);
 const box=document.createElement('div');box.className='box';box.style.cssText=`position:relative;margin:12px auto;font-size:${fs}px;width:${cols*rw*fs}px;height:${rows*rl*fs}px`;st.appendChild(box);
 const S={def,st,box,cols,rows,cw:rw*fs,lh:rl*fs,A:{},amb:[],shakes:0,warns:new Set(),
  warn(m){if(!S.warns.has(m)){S.warns.add(m);console.warn(def.title+': '+m)}},
  mark(l){(window.__marks=window.__marks||[]).push({story:def.title,label:l,t:NOW})}};
 for(const [n,d] of Object.entries(def.cast))S.A[n]=actor(S,n,d);
 if(def.ambient==='snow')for(let i=0;i<8;i++){const e=document.createElement('div');e.className='g';e.style.opacity=.4;e.textContent=i%3?'*':'.';box.appendChild(e);S.amb.push({e,x:Math.random()*cols,y:Math.random()*rows,v:.0015+Math.random()*.0015,p:Math.random()*6})}
 lint(S);
 S.reset=()=>{for(const a of Object.values(S.A)){const d=a.def;a.ox=a.oy=0;a.set(d.pose||Object.keys(d.poses)[0]);[a.x,a.y]=d.at||[0,0];a.vis=!d.hidden}};
 if(def.click)st.onclick=()=>run(S,['seq',...def.click]).catch(e=>console.error(e));
 (async()=>{while(true){S.reset();S.shakes=0;await run(S,['seq',...def.beats]);if(S.shakes>1)S.warn(`shook ${S.shakes} times`);await sleep(def.gap||1400)}})().catch(e=>{console.error(def.title,e)});
 return S}

// Every frame: advance time, draw, and check (overlap, off frame, too much accent)
function check(S){const occ=new Map();let accN=0;const touch=(S.def.touch||[]).map(p=>p.slice().sort().join('×'));
 for(const a of Object.values(S.A)){if(!a.vis)continue;const raw=a.poses[a.pose];accN+=(raw.match(/⟦\w\|(.*?)⟧/g)||[]).reduce((s,m)=>s+plain(m).trim().length,0);
  plain(raw).split('\n').forEach((line,j)=>{[...line].forEach((ch,i)=>{if(ch===' ')return;const c=Math.round(a.x+a.ox)+i,rr=Math.round(a.y+a.oy)+j;
   if(!a.moving&&(c<0||c>=S.cols||rr<0||rr>=S.rows))S.warn(`off frame: "${a.name}" rests outside the frame`);
   const k=c+','+rr;const o=occ.get(k);if(o&&o!==a.name){const pair=[o,a.name].sort().join('×');if(!touch.includes(pair))S.warn(`overlap: "${o}" and "${a.name}" (${a.name} pose ${a.pose}, col ${c} row ${rr})`)}else occ.set(k,a.name)})})}
 if(accN>6)S.warn(`accent used on ${accN} glyphs at once, too many (keep it ≤ 6)`)}
let LAST=performance.now(),FR=0;const ALL=[];
function loop(){const t=performance.now();const dt=Math.min(t-LAST,60)*SPEED;LAST=t;NOW+=dt;FR++;
 for(const w of TW.slice()){const p=Math.min(1,(NOW-w.t0)/w.ms);w.fn(p);if(p>=1){TW.splice(TW.indexOf(w),1);w.res()}}
 for(const S of ALL){for(const f of S.amb){f.p+=.002*dt;f.y+=f.v*dt;if(f.y>S.rows){f.y=-1;f.x=Math.random()*S.cols}f.e.style.left=(f.x+Math.sin(f.p)*.6)*S.cw+'px';f.e.style.top=f.y*S.lh+'px'}
  for(const a of Object.values(S.A))a.draw();if(FR%3==0)check(S)}
 window.__report=ALL.map(S=>({story:S.def.title,warnings:[...S.warns]}));requestAnimationFrame(loop)}

// ===== Script: edit only this part =====
// Example 1: Dog catches the ball (accent on the ball only; tension: almost there)
const DOG=(eyes,mouth='⟦m|Y⟧',legs='V__) ||')=>["  /^ ^\\"," / "+eyes+" \\"," V\\ "+mouth+" /V","  / - \\"," /    |",legs].join('\n');
story({title:'Dog catches the ball',acc:'#f2b705',
 hint:'The dog wants the ball. It stares, crouches, jumps and catches it, but the ball is too big and gets stuck in its mouth. Sometimes it bounces off its head.',
 touch:[['dog','ball']],
 cast:{dog:{at:[10,7],anchors:{mouth:[4,2]},acc:{m:'var(--ink)',b:'var(--acc)'},
   poses:{idle:DOG('0 0'),look:DOG('° °'),crouch:'\n'+DOG('> <'),jump:DOG('O O','⟦m|o⟧',' V    V'),stuck:DOG('^ ^','⟦b|(O)⟧'),stuck2:DOG('^ ^','⟦b|(O)⟧','V__) ||~'),
          blank:DOG('. .'),camera:DOG('• •'),away:DOG('0 0',undefined,'V__) || ').replace(' / 0 0 \\',' /0 0  \\'),proud:DOG('^ ^','⟦b|(O)⟧'),blink:DOG('- -')}},
  ball:{at:[35,9],hidden:true,poses:{o:'⟦a|O⟧'}}},
 beats:[
  ['wait',1000],['blink','dog'],['wait',900],
  ['place','ball',[34,10]],['pose','dog','look'],
  ['par',['move','ball',['dog.mouth',6,-3],1500,'arc',4],['seq',['wait',800],['pose','dog','crouch']]],   // stare, crouch (slow → stop)
  ['pose','dog','jump'],['move','dog',{dy:-2},160,'slide'],['move','ball',['dog.mouth',0,0],140,'slide'], // jump (fast)
  ['maybe',.3,
   [['move','ball',['dog.mouth',7,-3],320,'slide'],['par',['move','ball',[31,13],700,'fall'],['seq',['pose','dog','blank'],['move','dog',{dy:2},220,'fall']]],
    ['react','dog','freeze'],['react','dog','turn'],['react','dog','pretend'],['hide','ball']],
   [['hide','ball'],['pose','dog','stuck'],['move','dog',{dy:2},220,'fall'],['frames','dog',['stuck','stuck2'],200,2600],['pose','dog','idle'],['wait',600]]],
 ]});

// Example 2: Reindeer and the snowball (accent on the nose only; light snow behind; tension: you know first)
// The snowball grows by handing off between five actors (b1…b5). Each pins its bottom-left corner, so the ball stays on the ground.
const DEER=e=>["\\|/    \\|/","  \\    /","   \\_/  ___   ___","   "+e+"-'   '''   '","    ⟦a|O⟧ -.         |\\","        | |'''| |","         ||   | |","         ||    ||",'         "     "'].join('\n');
const PILE=n=>["        .  *   .","     .-~~~~~~~~~-.","   .'             '.","  (   ⟦a|"+n+"⟧             )"," (                   )","(_____________________)"].join('\n');
story({title:'Reindeer and the snowball',acc:'#e5383b',ambient:'snow',
 hint:'The reindeer is lost in the falling snow. Behind it a snowball rolls closer and grows, and it never notices.',
 touch:[['deer','b5']],
 cast:{deer:{at:[2,5],anchors:{top:[0,0]},poses:{idle:DEER('o o'),up:DEER('° °'),away:DEER('oo '),blank:DEER('. .'),camera:DEER('• •'),blink:DEER('- -')}},
  b1:{hidden:true,pin:[0,0],poses:{a:'o'}},
  b2:{hidden:true,pin:[0,0],poses:{a:'O'}},
  b3:{hidden:true,pin:[0,1],poses:{a:".-.\n'-'"}},
  b4:{hidden:true,pin:[0,2],poses:{a:" .--.\n(    )\n '--'"}},
  b5:{hidden:true,pin:[0,3],poses:{a:" .---.\n(     )\n(     )\n '---'"}},
  pile:{hidden:true,at:[0,8],poses:{on:PILE('O'),off:PILE('o')}},
  cap:{hidden:true,poses:{a:".*.    .*."}}},
 beats:[
  ['wait',700],['pose','deer','up'],['wait',900],
  // slow: a small snowball rolls in from far away while the reindeer watches the snow
  ['place','b1',[35,13]],
  ['par',
   ['seq',['move','b1',[31,13],2200,'slide'],['hide','b1'],['place','b2',[31,13]],['move','b2',[28,13],1600,'slide']],
   ['seq',['wait',1400],['blink','deer'],['wait',900],['pose','deer','away']]],
  // stop: its eyes slowly come back… you think it's about to notice
  ['pose','deer','idle'],['wait',900],['pose','deer','up'],['wait',500],
  // fast: it grows as it rolls
  ['hide','b2'],['place','b3',[28,13]],['move','b3',[26,13],700,'fall'],
  ['hide','b3'],['place','b4',[26,13]],['move','b4',[24,13],450,'fall'],
  ['hide','b4'],['place','b5',[24,13]],
  ['maybe',.3,
   // 30%: it hops up for a snowflake right then, the ball rolls underneath, and it never knows
   [['par',['move','deer',{dy:-5},360,'slide'],['seq',['move','b5',[-8,13],900,'slide'],['hide','b5']]],['move','deer',{dy:5},260,'fall'],
    ['wait',300],['pose','deer','camera'],['wait',1400],['pose','deer','up'],['wait',800]],
   // 70%: hit, buried in a snow pile with only the red nose blinking (this uses the one shake)
   [['move','b5',[20,13],180,'slide'],['hide','b5'],['hide','deer'],['place','pile',[0,8],'on'],
    ['wait',900],['frames','pile',['off','on'],500,3000],['wait',500],
    ['react','pile','shake'],['hide','pile'],['pose','deer','blank'],['place','cap',['deer.top',0,-1]],
    ['wait',1200],['react','deer','turn'],['react','deer','pretend'],['hide','cap']]],
 ]});

// ===== Page: copy as is, don't edit =====
const g=document.getElementById('g'),sel={};
STORIES.forEach((d,i)=>{const c=document.createElement('div');c.className='card';c.innerHTML=`<div class="st" style="--acc:${d.acc}"></div><div class="cap"><b>${i+1}. ${d.title}</b><span>${d.hint}</span><div class="chips"></div></div>`;g.appendChild(c);
 const cs=c.querySelector('.chips');['Keep','Skip'].forEach(l=>{const b=document.createElement('button');b.className='chip';b.textContent=l;b.onclick=()=>{cs.querySelectorAll('.chip').forEach(z=>z.classList.toggle('on',z===b));sel[i]=l};cs.appendChild(b)})});
requestAnimationFrame(()=>{document.querySelectorAll('.st').forEach((st,i)=>ALL.push(mount(st,STORIES[i])));loop()});
document.getElementById('copy').onclick=async()=>{const s='Stories:\n'+STORIES.map((d,i)=>`${i+1}. ${d.title}: ${sel[i]||'not picked'}`).join('\n');try{await navigator.clipboard.writeText(s);document.getElementById('st').textContent='Copied. Paste it back to me.'}catch(e){prompt('Copy this',s)}};
document.getElementById('theme').onclick=()=>document.body.classList.toggle('dark');
</script></body></html>
```

### Writing a script
Inside `story({...})`:
- `title`, `hint` (card text, one or two sentences of the four beats), `acc` (the single accent color), `size` (`card` 36×14 / `wide` 72×12 / `small` 26×8; default `card`)
- `cast`: each actor is `{at:[col,row], poses:{name:art}, anchors:{anchor:[col,row]}, pin:[col,row], hidden, acc, mask}`
  - The first pose is the starting one. In the art, `⟦a|text⟧` puts the accent on one or two glyphs; when one actor needs two colors, use `acc:{m:'var(--ink)',b:'var(--acc)'}` with `⟦m|…⟧` `⟦b|…⟧`.
  - `anchors`: the points on an actor that things touch (nose, mouth, paw, top of head), relative to its top-left. To land something there, write `'dog.mouth'` or `['dog.mouth', col offset, row offset]`; never compute coordinates by hand.
  - `pin`: which cell of the moving thing lines up with the target (the bug `c•ɔ` uses its middle: `pin:[1,0]`; a snowball rolling on the ground uses its bottom-left).
  - `mask:true`: draws a background behind the actor so it hides what's behind it (chimney, laptop).
- `touch`: actor pairs that are allowed to overlap, e.g. `[['dog','ball']]`. Any other overlap is reported.
- `ambient:'snow'`: faint snow in the background, ignored by the checks.
- `click`: a script added on click (only to "add something", never the main story).
- `beats`: the script itself, one action per line.

**Action vocabulary** (use only these in `beats`):
| Action | Syntax | Used for |
|---|---|---|
| Stop | `['wait', ms]` | build-up, looking, hesitating ("stop" is half the rhythm) |
| Change pose | `['pose', actor, pose]` | expressions, postures |
| Blink | `['blink', actor]` | life while idle (needs a `blink` pose) |
| Put somewhere | `['place', actor, target, pose?]` | bring something in |
| Move | `['move', actor, target, ms, path, height?]` | walk, drift, fly, fall, hop, throw |
| Nudge | `['nudge', actor, cols, rows?]` | pushing one cell at a time |
| Frames | `['frames', actor, [poses…], ms per frame, total?]` | wings, melting, wagging, a blinking nose; without a total it plays once |
| Hide | `['hide', actor]` | |
| Together | `['par', action, action…]` | the snowball rolls while the reindeer watches the snow |
| In order | `['seq', action…]` | used inside `par` |
| 30% twist | `['maybe', 0.3, [twist script], [usual script]]` | fails, gets stuck, the occasional success |
| React | `['react', actor, reaction]` | see below |

**Paths**: `slide` push, slide; `walk` walk with a little bob; `drift` float; `settle` drift down and only move in at the end (avoids bodies in the way); `fly` a bug's wobble; `fall` speeds up (also for "rolls faster and faster"); `hop` small hop; `arc` a throw.

**Reactions** (each needs the matching pose on the actor):
| Reaction | Syntax | Needs pose |
|---|---|---|
| Freeze | `freeze` | `blank` |
| Slowly turn to look at you | `turn` | `camera` |
| Pretend nothing happened | `pretend` | `away`, `idle` |
| Proud | `proud` | `proud` |
| Shake (once per story at most) | `shake` | none |
Common combos: on failure `freeze → turn → pretend`; on success `proud`. "Try again out of spite" means writing the action again, faster the second time.

**Adding a move**: when the vocabulary truly lacks one (yarn unraveling, a screen filling with `gggg`), put a function `async S=>{…}` in `beats`; it can use `S.A.actor`, `sleep(ms)` and `tween(ms, p=>{…})`. Anything used twice or more goes into the engine's vocabulary.

Notes:
- The engine measures glyph width and line height and sizes the font from the column count; stories never use pixels.
- To make something "grow" (a snowball getting bigger), hand off between actors of different sizes: hide one, place the next at the same point, pin the corner that touches the ground.
- When several things must **line up on the same text row** (a wall and the hands on it, two heads peeking), make them one pose of one actor instead of separate overlapping actors.
- Build art as arrays `[line1, line2, …].join('\n')`, not template strings: library pieces often contain backticks `` ` `` and triple quotes `'''` that break the code. Write `\` as `\\` inside strings.
- When the same thing comes back (a second snowflake), give `place` a pose, or it keeps the look it ended with last time.

Notes on stories and characters:
- Human faces are often only a few cells, with little room for expression: tell human stories through action and other people's reactions (sudden eye contact, `!` `?` above the head), not subtle faces.
- **Faceless objects as the lead** (a tree, a cup, a star): show feelings with whole-body motion (shake, jump, bounce); a dot flashing on it can read as "happy".
- **The glyphs themselves can be the joke**: a character face-planting on the keyboard fills the screen with `gggg`, soot turns it into `#`. That humor only exists in character art, so look for it first.
- **Everyday topics are played by animals** (overtime, exams, moving): pick an animal from the library and give it a new prop (a laptop, an exam paper). It's safer than drawing a person.
- **The lead can't be tiny**: at least a quarter of the frame height. If a piece is small (a two-line duck), use a bigger font size.
- **Off-library characters: look for a one-liner first**: many animals have a well-known one-line form (fish `><(((º>`, duck `>(.)__`): small, readable, easy to animate.
- **Props are recognized by how they move**: an `@` on its own means nothing; rolling and trailing a line of dots, it's a yarn ball. Keep props to 1–5 glyphs.

## Appendix C: tracing (when the library has nothing)
1. Take an SVG icon from Lucide (`npm pack lucide-static`) or Tabler (`npm pack @tabler/icons`); both licenses allow commercial use.
2. Sample the paths into polylines with svgpathtools and pick a glyph per cell by stroke direction: horizontal `-` `_`, vertical `|`, diagonal `/` `\`, curves `( )`, corners `.` `'`. Scale to about 6–7 rows.
3. The trace only sets proportions; then fix it cell by cell: round corners become `.` `'`, remove extra glyphs, don't use three rows if two will do. Side views don't draw the top.
4. Compare side by side with the reference; use it only once it looks right.

## Appendix D: self-check script check.py
Usage: `python3 check.py file:///path/index.html 40 chk` (runs 40 seconds, results in the chk folder). Needs `pip install playwright pillow` and `playwright install chromium`.
It plays at 2× speed; prints errors and each story's warnings (overlap, off frame, too much accent, more than one shake, missing reaction poses); screenshots every "landing" (`m*.png`, to see whether things landed in the right place); and exports a **title-free frame strip** per story, `blind_*.png`, for the blind test.

```python
# Self-check: errors, overlap, off frame, accent, shake count; screenshots at each mark; a title-free frame strip for the blind test
import sys,json,time,os
from playwright.sync_api import sync_playwright
from PIL import Image
url=sys.argv[1];secs=float(sys.argv[2]) if len(sys.argv)>2 else 40;out=sys.argv[3] if len(sys.argv)>3 else 'chk'
os.makedirs(out,exist_ok=True)
with sync_playwright() as p:
    try:b=p.chromium.launch()
    except Exception:b=p.chromium.launch(executable_path='/usr/bin/google-chrome')
    pg=b.new_page(viewport={'width':1400,'height':900})
    errs=[];pg.on('pageerror',lambda e:errs.append(str(e)));pg.on('console',lambda c:errs.append(c.text) if c.type=='error' else None)
    pg.goto(url+('&' if '?' in url else '?')+'speed=2');pg.wait_for_timeout(500)
    sts=pg.query_selector_all('.st');n=len(sts);frames={i:[] for i in range(n)};marks=[];seen=0;t0=time.time();k=0
    titles=pg.evaluate('STORIES.map(s=>s.title)')
    while time.time()-t0<secs:
        ms=pg.evaluate('window.__marks||[]')
        for m in ms[seen:]:
            i=titles.index(m['story']);f=f'{out}/m{len(marks)}.png';sts[i].screenshot(path=f);marks.append((m['story'],m['label'],f))
        seen=len(ms)
        if k%4==0:
            for i in range(n):f=f'{out}/s{i}_{len(frames[i])}.png';sts[i].screenshot(path=f);frames[i].append(f)
        k+=1;pg.wait_for_timeout(250)
    rep=pg.evaluate('window.__report');b.close()
print('errors:',errs[:5] or 'none')
for r in rep:print(r['story'],'→',r['warnings'] or 'ok')
for i in range(n):
    ims=[Image.open(f) for f in frames[i]][:24];w,h=ims[0].size;s=.45;W,H=int(w*s),int(h*s);cols=6
    sheet=Image.new('RGB',(W*cols,H*((len(ims)+cols-1)//cols)),'#ccc')
    for j,im in enumerate(ims):sheet.paste(im.resize((W,H)),((j%cols)*W,(j//cols)*H))
    sheet.save(f'{out}/blind_{i}.png')
json.dump(marks,open(f'{out}/marks.json','w'),ensure_ascii=False)
print('marks:',[(a,b) for a,b,_ in marks][:30])
```

What the script can't catch, check by eye:
- In the landing screenshots, did things land **where they should** (on the eyes instead of the nose is invisible to the script)?
- Look at light and dark once each.
- Blind strip: can you tell the four beats without the title, and say where the tension comes from?

## Credits
Many characters in the library are by ASCII artists, collected on asciiart.eu. Thanks to:
- Joan G. Stark (Spunk): Dog · lying down, Christmas tree, Small snowman, Wreath, Bride and groom, King, Knight, Skating, Ornament light
- Hayley Jane Wakenshaw (Flump): Dog · floppy ears, Dog · walking, Coffee and cereal
- Laura Brown (SheDragon): Snowman with hat
- Max Strandberg: Santa
- Glory Py Yip (Glory Moon): Baby
- H P Barmario (Morfina): Wizard
- llizard: Beach volleyball
- Susie Oviatt: Stocking · archive version
- Archive pieces with unknown artists: Cat · front, Cat · squinting, Dog · sitting, Peeking person, Throwing a ball, Star, Reindeer
- Open-source icons (tracing base): Lucide (ISC): Gift box, Coffee cup, Candy cane; Tabler Icons (MIT): Gingerbread man, Stocking
- Inspiration: the character visuals of OpenAI DevDay 2025 / 2026 (Studio Dumbar). Only the method is borrowed, not its characters.

These credits stay in the skill and never go into any generated picture or page.
