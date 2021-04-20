🔰Muhammaddzaky🔰
Hi there 👋

`ghtop` provides a number of views of all current public activity from all users across the entire GitHub platform. (Note that GitHub delays all events by five minutes.)

<img width="850" src="https://user-images.githubusercontent.com/56260/101270865-3f033780-3732-11eb-8dcc-97caf7cc58e6.png" style="max-width: 850px">
<img width="650" src="https://user-images.githubusercontent.com/1483922/105071141-0c3ea580-5a39-11eb-8808-34952c0bf26d.gif" style="max-width: 650px">

## Install

@@ -14,26 +14,27 @@ Either `pip install ghtop` or `conda install -c fastai ghtop`.

Run `ghtop -h` to view the help:

```bash
```
$ ghtop -h
usage: ghtop [-h] [--include_bots] [--types TYPES] [--filt {user,repo,org}] [--filtval FILTVAL]
             {tail,quad,users,simple}
usage: ghtop [-h] [--include_bots] [--types TYPES] [--pause PAUSE] [--filt {users,repo,org}] [--filtval FILTVAL] {tail,quad,users,simple}
positional arguments:
  {tail,quad,users,simple}  Operation mode to run
optional arguments:
  -h, --help                show this help message and exit
  --include_bots            Include bots (there is a lot of them!) (default: False)
  --types TYPES             Comma-separated types of event to include (e.g PushEvent)
  --filt {user,repo,org}    Filtering method
  --include_bots            Include bots (there's a lot of them!) (default: False)
  --types TYPES             Comma-separated types of event to include (e.g PushEvent) (default: )
  --pause PAUSE             Number of seconds to pause between requests to the GitHub api (default: 0.4)
  --filt {users,repo,org}   Filtering method
  --filtval FILTVAL         Value to filter by (for `repo` use format `owner/repo`)
```

There are 4 views you can choose: `ghtop simple`, `ghtop tail`, `ghtop quad`, or `ghtop users`. Each are shown and described below. All views have the following options:

- `--include_bots`: By default events that appear to be from bots are excluded. Add this flag to include them
- `--types TYPES`: Optional comma-separated list of event types to include (defaults to all types). For a full list of types, see the GitHub [event types docs](https://docs.github.com/en/free-pro-team@latest/developers/webhooks-and-events/github-event-types)
- `--pause PAUSE`: Number of seconds to pause between requests to the GitHub api (default: 0.4).  It is helpful to adjust this number if you want to get events more or less frequently.  For example, if you are filtering all events by an org, then you likely want to pause for a longer period of time than the default.
- `--filt` and `--filtval`: Optionally filter events to just those from one of: `user`, `repo`, or `org`, depending on `filt`. `filtval` is the value to filter by. See the [GitHub docs](https://docs.github.com/en/free-pro-team@latest/rest/reference/activity#list-public-events) for details on the public event API calls used.

**Important note**: while running, `ghtop` will make about 5 API calls per second. GitHub has a quota of 5000 calls per hour. When there are 1000 calls left, `ghtop` will show a warning on every call.
@@ -42,25 +43,25 @@ There are 4 views you can choose: `ghtop simple`, `ghtop tail`, `ghtop quad`, or

A simple dump to your console of all events as they happen.

<img src="https://user-images.githubusercontent.com/346999/101861674-79e7df80-3b25-11eb-92d3-f888843f4aa2.png" width="500" style="max-width: 500px">
<img src="https://user-images.githubusercontent.com/1483922/105074536-56298a80-5a3d-11eb-8e8c-32ba33e09405.png" width="650" style="max-width: 650px">

### ghtop tail

Like `simple`, but removes most bots, and only includes releases, issues and PRs (open, close, and comment events). A summary of the frequency of push events is also shown at the bottom of the screen.
Like `simple`, but only includes releases, issues and PRs (open, close, and comment events). A summary of the frequency of different kind of events along with sparklines are shown at the top of the screen.

<img src="https://user-images.githubusercontent.com/346999/101861658-69376980-3b25-11eb-96ef-9d68f075abf7.png" width="700" style="max-width: 700px">
<img src="https://user-images.githubusercontent.com/1483922/105074448-398d5280-5a3d-11eb-9603-3def521d87e5.png" width="650" style="max-width: 650px">

### ghtop quad

The same information as `tail`, but in a split window showing separately PRs, issues, pushes, and releases. This view does not remove bot activity.
The same information as `tail`, but in a split window showing separately PRs, issues, pushes, and releases.

<img src="https://user-images.githubusercontent.com/346999/101861560-2ecdcc80-3b25-11eb-9fba-25382b2df65f.png" width="900" style="max-width: 900px">
<img src="https://user-images.githubusercontent.com/1483922/105074862-cb955b00-5a3d-11eb-99c5-3125bb98910b.png" width="650" style="max-width: 650px">

### ghtop users

A summary of activity for the most active current users.

<img src="https://user-images.githubusercontent.com/346999/101861612-4b6a0480-3b25-11eb-8124-19bb2434c27e.png" width="500" style="max-width: 500px">
<img src="https://user-images.githubusercontent.com/1483922/105075363-8887b780-5a3e-11eb-9f7a-627268ac465f.png" width="650" style="max-width: 650px">

----

 84  ghtop/all_rich.py 
@@ -0,0 +1,84 @@
from rich import print as pr
from rich.align import *
from rich.bar import *
from rich.color import *
from rich.columns import *
from rich.console import *
from rich.emoji import *
from rich.highlighter import *
from rich.live import *
from rich.logging import *
from rich.markdown import *
from rich.markup import *
from rich.measure import *
from rich.padding import *
from rich.panel import *
from rich.pretty import *
from rich.progress_bar import *
from rich.progress import *
from rich.prompt import *
from rich.protocol import *
from rich.rule import *
from rich.segment import *
from rich.spinner import *
from rich.status import *
from rich.style import *
from rich.styled import *
from rich.syntax import *
from rich.table import *
from rich.text import *
from rich.theme import *
from rich.traceback import *
from fastcore.all import *

@delegates(Style)
def text(s, maxlen=None, **kwargs):
    "Create a styled `Text` object"
    if maxlen: s = truncstr(s, maxlen=maxlen)
    return Text(s, style=Style(**kwargs))

@delegates(Style)
def segment(s, maxlen=None, space=' ', **kwargs):
    "Create a styled `Segment` object"
    if maxlen: s = truncstr(s, maxlen=maxlen, space=space)
    return Segment(s, style=Style(**kwargs))

class Segments(list):
    def __init__(self, options): self.w = options.max_width

    @property
    def chars(self): return sum(o.cell_length for o in self)
    def txtlen(self, pct): return min((self.w-self.chars)*pct, 999)

    @delegates(segment)
    def add(self, x, maxlen=None, pct=None, **kwargs):
        if pct: maxlen = math.ceil(self.txtlen(pct))
        self.append(segment(x, maxlen=maxlen, **kwargs))

@delegates(Table)
def _grid(box=None, padding=0, collapse_padding=True, pad_edge=False, expand=False, show_header=False, show_edge=False, **kwargs):
    return Table(padding=padding, pad_edge=pad_edge, expand=expand, collapse_padding=collapse_padding,
                 box=box, show_header=show_header, show_edge=show_edge, **kwargs)

@delegates(_grid)
def grid(items, expand=True, no_wrap=True, **kwargs):
    g = _grid(expand=expand, **kwargs)
    for c in items[0]: g.add_column(no_wrap=no_wrap, justify='center')
    for i in items: g.add_row(*i)
    return g

Color = str_enum('Color', *ANSI_COLOR_NAMES)

class Deque(deque):
    def __rich__(self): return RenderGroup(*(filter(None, self)))

@delegates()
class FixedPanel(Panel, GetAttr):
    _default='renderable'
    def __init__(self, height, **kwargs):
        super().__init__(Deque([' ']*height, maxlen=height), **kwargs)

@delegates(Style)
def add(self, s:str, **kwargs):
    "Add styled `s` to panel"
    self.append(text(s, **kwargs))
  161  ghtop/ghtop.py 
@@ -1,6 +1,6 @@


__all__ = ['term', 'logfile', 'github_auth_device', 'limit_cb', 'api', 'Events', 'print_event', 'tail_events',
__all__ = ['get_sparklines', 'ETYPES', 'term', 'tdim', 'limit_cb', 'Events', 'print_event', 'pct_comp', 'tail_events',
           'watch_users', 'quad_logs', 'simple', 'main']


@@ -14,25 +14,27 @@
from fastcore.foundation import *
from fastcore.script import *
from ghapi.all import *
from .richext import *
from .all_rich import (Console, Color, FixedPanel, box, Segments, Live,
                            grid, ConsoleOptions, Progress, BarColumn, Spinner, Table)

ETYPES=PushEvent,PullRequestEvent,IssuesEvent,ReleaseEvent

term = Terminal()
logfile = Path("log.txt")
def get_sparklines():
    s1 = ESpark('Push', 'magenta', [PushEvent], mx=30)
    s2 = ESpark('PR', 'yellow', [PullRequestEvent, PullRequestReviewCommentEvent, PullRequestReviewEvent], mx=8)
    s3 = ESpark('Issues', 'green', [IssueCommentEvent,IssuesEvent], mx=6)
    s4 = ESpark('Releases', 'blue', [ReleaseEvent], mx=0.4)
    s5 = ESpark('All Events', 'orange', mx=45)

    return Stats([s1,s2,s3,s4,s5], store=5, span=5, spn_lbl='5/s', show_freq=True)

def github_auth_device(wb='', n_polls=9999):
    "Authenticate with GitHub, polling up to `n_polls` times to wait for completion"
    auth = GhDeviceAuth()
    print(f"First copy your one-time code: {term.yellow}{auth.user_code}{term.normal}")
    print(f"Then visit {auth.verification_uri} in your browser, and paste the code when prompted.")
    if not wb: wb = input("Shall we try to open the link for you? [y/n] ")
    if wb[0].lower()=='y': auth.open_browser()

    print("Waiting for authorization...", end='')
    token = auth.wait(lambda: print('.', end=''), n_polls=n_polls)
    if not token: return print('Authentication not complete!')
    print("Authenticated with GitHub")
    return token
term = Terminal()

tdim = L(os.popen('stty size', 'r').read().split())
if not tdim: theight,twidth = 15,15
else: theight,twidth = tdim.map(lambda x: max(int(x)-4, 15))


def _exit(msg):
@@ -46,18 +48,6 @@ def limit_cb(rem,quota):
    if rem < 1000: print(f"{w}\nRemaining calls: {rem} out of {quota}\n{w}", file=sys.stderr)


def _get_api():
    path = Path.home()/".ghtop_token"
    if path.is_file():
        try: token = path.read_text().strip()
        except: _exit("Error reading token")
    else: token = github_auth_device()
    path.write_text(token)
    return GhApi(limit_cb=limit_cb, token=token)

api = _get_api()


Events = dict(
    IssuesEvent_closed=('⭐', 'closed', noop),
    IssuesEvent_opened=('📫', 'opened', noop),
@@ -78,79 +68,120 @@ def _to_log(e):
    elif e.type == "ReleaseEvent": return f'🚀 {login} released {e.payload.release.tag_name} of {repo}'


def print_event(e, commits_counter):
def print_event(e, counter):
    res = _to_log(e)
    if res: print(res)
    elif e.type == "PushEvent": [commits_counter.update() for c in e.payload.commits]
    elif counter and e.type == "PushEvent": [counter.update() for c in e.payload.commits]
    elif e.type == "SecurityAdvisoryEvent": print(term.blink("SECURITY ADVISORY"))


def tail_events(evt):
    "Print events from `fetch_events` along with a counter of push events"
    manager = enlighten.get_manager()
    commits = manager.counter(desc='Commits', unit='commits', color='green')
    for ev in evt: print_event(ev, commits)
def pct_comp(api): return int(((5000-int(api.limit_rem)) / 5000) * 100)


def _pr_row(*its): print(f"{its[0]: <30} {its[1]: <6} {its[2]: <5} {its[3]: <6} {its[4]: <7}")
def watch_users(evts):
def tail_events(evt, api):
    "Print events from `fetch_events` along with a counter of push events"
    p = FixedPanel(theight, box=box.HORIZONTALS, title='ghtop')
    s = get_sparklines()
    g = grid([[s], [p]])
    with Live(g):
        for e in evt:
            s.add_events(e)
            s.update_prog(pct_comp(api))
            p.append(e)
            g = grid([[s], [p]])


def _user_grid():
    g = Table.grid(expand=True)
    g.add_column(justify="left")
    for i in range(4): g.add_column(justify="center")
    g.add_row("", "", "", "", "")
    g.add_row("User", "Events", "PRs", "Issues", "Pushes")
    return g


def watch_users(evts, api):
    "Print a table of the users with the most events"
    users,users_events = defaultdict(int),defaultdict(lambda: defaultdict(int))
    while True:
        for x in islice(evts, 10):
            users[x.actor.login] += 1
            users_events[x.actor.login][x.type] += 1

        print(term.clear())
        _pr_row("User", "Events", "PRs", "Issues", "Pushes")
        sorted_users = sorted(users.items(), key=lambda o: (o[1],o[0]), reverse=True)
        for u in sorted_users[:20]:
            _pr_row(*u, *itemgetter('PullRequestEvent','IssuesEvent','PushEvent')(users_events[u[0]]))

    with Live() as live:
        s = get_sparklines()
        while True:
            for x in islice(evts, 10):
                users[x.actor.login] += 1
                users_events[x.actor.login][x.type] += 1
                s.add_events(x)

def _push_to_log(e): return f"{e.actor.login} pushed {len(e.payload.commits)} commits to repo {e.repo.name}"
def _logwin(title,color): return Log(title=title,border_color=2,color=color)
            ig = _user_grid()
            sorted_users = sorted(users.items(), key=lambda o: (o[1],o[0]), reverse=True)
            for u in sorted_users[:theight]:
                data = (*u, *itemgetter('PullRequestEvent','IssuesEvent','PushEvent')(users_events[u[0]]))
                ig.add_row(*L(data).map(str))

def quad_logs(evts):
    "Print 4 panels, showing most recent issues, commits, PRs, and releases"
    term.enter_fullscreen()
    ui = HSplit(VSplit(_logwin('Issues',        color=7), _logwin('Commits' , color=3)),
                VSplit(_logwin('Pull Requests', color=4), _logwin('Releases', color=5)))
            s.update_prog(pct_comp(api))
            g = grid([[s], [ig]])
            live.update(g)

    issues,commits,prs,releases = all_items = ui.items[0].items+ui.items[1].items
    for o in all_items: o.append(" ")

    d = dict(PushEvent=commits, IssuesEvent=issues, IssueCommentEvent=issues, PullRequestEvent=prs, ReleaseEvent=releases)
    while True:
        for x in islice(evts, 10):
            f = [_to_log,_push_to_log][x.type == 'PushEvent']
            if x.type in d: d[x.type].append(f(x)[:95])
        ui.display()
def _panelDict2Grid(pd):
    ispush,ispr,isiss,isrel = pd.values()
    return grid([[ispush,ispr],[isiss,isrel]], width=twidth)


def simple(evts):
def quad_logs(evts, api):
    "Print 4 panels, showing most recent issues, commits, PRs, and releases"
    pd = {o:FixedPanel(height=(theight//2)-1,
                       width=(twidth//2)-1,
                       box=box.HORIZONTALS,
                       title=camel2words(remove_suffix(o.__name__,'Event'))) for o in ETYPES}
    p = _panelDict2Grid(pd)
    s = get_sparklines()
    g = grid([[s], [p]])
    with Live(g):
        for e in evts:
            s.add_events(e)
            s.update_prog(pct_comp(api))
            typ = type(e)
            if typ in pd: pd[typ].append(e)
            p = _panelDict2Grid(pd)
            g = grid([[s], [p]])


def simple(evts, api):
    for ev in evts: print(f"{ev.actor.login} {ev.type} {ev.repo.name}")


def _get_token():
    path = Path.home()/".ghtop_token"
    if path.is_file():
        try: return path.read_text().strip()
        except: _exit("Error reading token")
    else: token = github_auth_device()
    path.write_text(token)
    return token


def _signal_handler(sig, frame):
    if sig != signal.SIGINT: return
    print(term.exit_fullscreen(),term.clear(),term.normal)
    sys.exit(0)

_funcs = dict(tail=tail_events, quad=quad_logs, users=watch_users, simple=simple)
_filts = str_enum('_filts', 'user', 'repo', 'org')
_filts = str_enum('_filts', 'users', 'repo', 'org')
_OpModes = str_enum('_OpModes', *_funcs)

@call_parse
def main(mode:         Param("Operation mode to run", _OpModes),
         include_bots: Param("Include bots (there's a lot of them!)", store_true)=False,
         types:        Param("Comma-separated types of event to include (e.g PushEvent)", str)='',
         pause:        Param("Number of seconds to pause between requests to the GitHub api", float)=0.4,
         filt:         Param("Filtering method", _filts)=None,
         filtval:      Param("Value to filter by (for `repo` use format `owner/repo`)", str)=None):
    signal.signal(signal.SIGINT, _signal_handler)
    types = types.split(',') if types else None
    if filt and not filtval: _exit("Must pass `filter_value` if passing `filter_type`")
    if filtval and not filt: _exit("Must pass `filter_type` if passing `filter_value`")
    kwargs = {filt:filtval} if filt else {}
    evts = api.fetch_events(types=types, incl_bot=include_bots, **kwargs)
    _funcs[mode](evts) 
    api = GhApi(limit_cb=limit_cb, token=_get_token())
    evts = api.fetch_events(types=types, incl_bot=include_bots, pause=float(pause), **kwargs)
    _funcs[mode](evts, api) 
 131  ghtop/richext.py 
@@ -0,0 +1,131 @@


__all__ = ['console', 'EProg', 'ESpark', 'SpkMap', 'Stats', 'colors', 'colors2']


import time,random
from collections import defaultdict
from typing import List
from collections import deque, OrderedDict, namedtuple
from .all_rich import (Console, Color, FixedPanel, box, Segments, Live,
                            grid, ConsoleOptions, Progress, BarColumn, Spinner)
from ghapi.event import *
from fastcore.all import *
console = Console()



class EProg:
    "Progress bar with a heading `hdg`."
    def __init__(self, hdg='Quota', width=10):
        self.prog = Progress(BarColumn(bar_width=width), "[progress.percentage]{task.percentage:>3.0f}%")
        self.task = self.prog.add_task("",total=100, visible=False)
        store_attr()
    def update(self, completed): self.prog.update(self.task, completed=completed)
    def __rich_console__(self, console: Console, options: ConsoleOptions):
        self.prog.update(self.task, visible=True)
        yield grid([["Quota"], [self.prog.get_renderable()]], width=self.width+2, expand=False)



class ESpark(EventTimer):
    "An `EventTimer` that displays a sparkline with a heading `nm`."
    def __init__(self, nm:str, color:str, ghevts=None, store=5, span=.2, mn=0, mx=None, stacked=True, show_freq=False):
        super().__init__(store=store, span=span)
        self.ghevts=L(ghevts)
        store_attr('nm,color,store,span,mn,mx,stacked,show_freq')

    def _spark(self):
        data = L(list(self.hist)+[self.freq] if self.show_freq else self.hist)
        num = f'{self.freq:.1f}' if self.freq < 10 else f'{self.freq:.0f}'
        return f"[{self.color}]{num} {sparkline(data, mn=self.mn, mx=self.mx)}[/]"

    def upd_hist(self, store, span): super().__init__(store=store, span=span)

    def _nm(self): return f"[{self.color}] {self.nm}[/]"

    def __rich_console__(self, console: Console, options: ConsoleOptions):
        yield grid([[self._nm()], [self._spark()]]) if self.stacked else f'{self._nm()}  {self._spark()}'

    def add_events(self, evts):
        evts = L([evts]) if isinstance(evts, dict) else L(evts)
        if self.ghevts: evts.map(lambda e: self.add(1) if type(e) in L(self.ghevts) else noop)
        else: self.add(len(evts))

    __repr__ = basic_repr('nm,color,ghevts,store,span,stacked,show_freq,ylim')


class SpkMap:
    "A Group of `ESpark` instances."
    def __init__(self, spks:List[ESpark]): store_attr()

    @property
    def evcounts(self): return dict([(s.nm, s.events) for s in self.spks])

    def update_params(self, store:int=None, span:float=None, stacked:bool=None, show_freq:bool=None):
        for s in self.spks:
            s.upd_hist(store=ifnone(store,s.store), span=ifnone(span,s.span))
            s.stacked = ifnone(stacked,s.stacked)
            s.show_freq = ifnone(show_freq,s.show_freq)

    def add_events(self, evts:GhEvent):
        "Update `SpkMap` sparkline historgrams with events."
        evts = L([evts]) if isinstance(evts, dict) else L(evts)
        for s in self.spks: s.add_events(evts)

    def __rich_console__(self, console: Console, options: ConsoleOptions): yield grid([self.spks])
    __repr__ = basic_repr('spks')




class Stats(SpkMap):
    "Renders a group of `ESpark` along with a spinner and progress bar that are dynamically sized."
    def __init__(self, spks:List[ESpark], store=None, span=None, stacked=None, show_freq=None, max_width=console.width-5, spin:str='earth', spn_lbl="/min"):
        super().__init__(spks)
        self.update_params(store=store, span=span, stacked=stacked, show_freq=show_freq)
        store_attr()
        self.spn = Spinner(spin)
        self.slen = len(spks) * max(15, store*2)
        self.plen = max(store, 10) # max(max_width-self.slen-15, 15)
        self.progbar = EProg(width=self.plen)

    def get_spk(self): return grid([self.spks], width=min(console.width-15, self.slen), expand=False)

    def get_spinner(self): return grid([[self.spn], [self.spn_lbl]])

    def update_prog(self, pct_complete:int=None): self.progbar.update(pct_complete) if pct_complete else noop()

    def __rich_console__(self, console: Console, options: ConsoleOptions):
        yield grid([[self.get_spinner(), self.get_spk(), grid([[self.progbar]], width=self.plen+5) ]], width=self.max_width)



@patch
def __rich_console__(self:GhEvent, console, options):
    res = Segments(options)
    kw = {'color': colors[self.type]}
    res.add(f'{self.emoji}  ')
    res.add(self.actor.login, pct=0.25, bold=True, **kw)
    res.add(self.description, pct=0.5, **kw)
    res.add(self.repo.name, pct=0.5 if self.text else 1, space = ': ' if self.text else '', italic=True, **kw)
    if self.text:
        clean_text = self.text.replace('\n', ' ').replace('\n', ' ')
        res.add (f'"{clean_text}"', pct=1, space='', **kw)
    res.add('\n')
    return res


colors = dict(
    PushEvent=None, CreateEvent=Color.red, IssueCommentEvent=Color.green, WatchEvent=Color.yellow,
    PullRequestEvent=Color.blue, PullRequestReviewEvent=Color.magenta, PullRequestReviewCommentEvent=Color.cyan,
    DeleteEvent=Color.bright_red, ForkEvent=Color.bright_green, IssuesEvent=Color.bright_magenta,
    ReleaseEvent=Color.bright_blue, MemberEvent=Color.bright_yellow, CommitCommentEvent=Color.bright_cyan,
    GollumEvent=Color.white, PublicEvent=Color.turquoise4)

colors2 = dict(
    PushEvent=None, CreateEvent=Color.dodger_blue1, IssueCommentEvent=Color.tan, WatchEvent=Color.steel_blue1,
    PullRequestEvent=Color.deep_pink1, PullRequestReviewEvent=Color.slate_blue1, PullRequestReviewCommentEvent=Color.tan,
    DeleteEvent=Color.light_pink1, ForkEvent=Color.orange1, IssuesEvent=Color.medium_violet_red,
    ReleaseEvent=Color.green1, MemberEvent=Color.orchid1, CommitCommentEvent=Color.tan,
    GollumEvent=Color.sea_green1, PublicEvent=Color.magenta2) ![75478502_2021549974655084_2879397195498539273_n](https://user-images.githubusercontent.com/58392246/115397328-2cd0f400-a210-11eb-853d-54bdae2543d3.jpg)
![106667439_2021550104655071_4702985018161248845_n](https://user-images.githubusercontent.com/58392246/115397335-2e022100-a210-11eb-95fc-07043f153fdb.jpg)
![106677748_2021550017988413_2673436044332224579_n](https://user-images.githubusercontent.com/58392246/115397341-2e9ab780-a210-11eb-8299-b235e9a7f704.jpg)
![107592966_2021549971321751_8827747987111937987_n](https://user-images.githubusercontent.com/58392246/115397347-2f334e00-a210-11eb-8b46-f36b731d676d.jpg)
![107745745_2021550127988402_6939238531202695701_n](https://user-images.githubusercontent.com/58392246/115397350-2fcbe480-a210-11eb-8e9c-f94ba5b55c6b.jpg)


