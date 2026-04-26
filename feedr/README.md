# feedr

A terminal RSS/Atom feed reader with a web interface.

## Setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install requests pyyaml beautifulsoup4
```

## Usage

```bash
# subscribe to feeds
python -m feedr add "https://blog.rust-lang.org/feed.xml"
python -m feedr add "https://hnrss.org/newest?count=10"

# or discover feeds from a website
python -m feedr discover "https://xkcd.com"
python -m feedr discover "https://xkcd.com" --add   # subscribe automatically

# fetch new entries
python -m feedr fetch

# browse entries
python -m feedr list
python -m feedr list --unread
python -m feedr list --feed 1 --limit 20

# read an entry
python -m feedr read 3

# manage feeds
python -m feedr feeds
python -m feedr remove 2

# mark as read
python -m feedr mark-read --feed 1
python -m feedr mark-read --all

# stats
python -m feedr stats

# export
python -m feedr export opml
python -m feedr export json -o entries.json

# web interface
python -m feedr serve
python -m feedr serve --port 9000
```

Then open http://127.0.0.1:8080 in a browser.

## Project structure

```
feedr/
  __main__.py    entry point
  cli.py         argument parsing and subcommands
  config.py      YAML configuration with defaults
  db.py          SQLite schema and queries
  discovery.py   find RSS/Atom feeds from a webpage URL
  fetcher.py     HTTP requests with conditional GET (ETag/Last-Modified)
  formatting.py  terminal output, JSON/OPML export, HTML stripping
  parser.py      RSS 2.0 and Atom XML parsing, date normalization
  web.py         web interface (entries, feeds, stats)
```

## Configuration

Optional. Create `~/.feedr/config.yml`:

```yaml
db_path: /path/to/feedr.db
fetch_timeout: 30
max_entries_per_list: 100
```

Without a config file, the database is created as `feedr.db` in the current directory.
