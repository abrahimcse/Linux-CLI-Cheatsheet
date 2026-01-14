# VIM EDITOR – STANDARD SINGLE-PAGE CHEAT SHEET

────────────────────────────────────────────
1. MODES (MOST IMPORTANT)
────────────────────────────────────────────
Normal Mode  → Command execution (DEFAULT)
Insert Mode  → Text typing
Visual Mode  → Text selection

Esc          → Always return to Normal mode

────────────────────────────────────────────
2. BASIC CURSOR MOVEMENT (NO ARROW KEYS)
────────────────────────────────────────────
h   → left
l   → right
k   → up
j   → down

w   → next word
b   → previous word
0   → beginning of line
$   → end of line

gg  → top of file
G   → bottom of file
:n  → go to line number n

────────────────────────────────────────────
3. INSERT MODE (TEXT ENTRY)
────────────────────────────────────────────
i   → insert before cursor
I   → insert at line start
a   → insert after cursor
A   → insert at line end
o   → new line below
O   → new line above

────────────────────────────────────────────
4. DELETE / CUT (MOST USED)
────────────────────────────────────────────
x    → delete character
dd   → delete entire line
D    → delete from cursor to line end

dw   → delete word
daw  → delete whole word (with space)
d0   → delete to line start
d$   → delete to line end

ndd  → delete n lines
dgg  → delete to top of file
dG   → delete to bottom of file

────────────────────────────────────────────
5. COPY (YANK) & PASTE
────────────────────────────────────────────
yy   → copy line
yw   → copy word
y$   → copy to line end

p    → paste after cursor
P    → paste before cursor

────────────────────────────────────────────
6. VISUAL MODE (SELECTION)
────────────────────────────────────────────
v    → character selection
V    → line selection

y    → copy selection
d    → delete selection

────────────────────────────────────────────
7. UNDO / REDO
────────────────────────────────────────────
u          → undo
Ctrl + r   → redo

────────────────────────────────────────────
8. SEARCH
────────────────────────────────────────────
/text   → search forward
?text   → search backward
n       → next match
N       → previous match

────────────────────────────────────────────
9. SAVE & EXIT (EXAM CRITICAL)
────────────────────────────────────────────
:w     → save
:q     → quit
:wq    → save and quit
:q!    → force quit (no save)

────────────────────────────────────────────
10. CORE VIM LOGIC (REMEMBER THIS)
────────────────────────────────────────────
COMMAND = ACTION + MOVEMENT

Examples:
d$   → delete to end of line
yG   → copy to end of file
dw   → delete word
