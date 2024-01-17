---
theme: seriph
background: /landing-bg.jpg
class: text-center
colorSchema: 'dark'
highlighter: shiki
lineNumbers: false
info: |
  ## Presented by Victor Miti at [#OGN56](https://www.meetup.com/oxford-geek-night/events/297539007/)
  Held in Oxford on Wednesday 17th January 2024.

  [Source](https://github.com/engineervix/zed-news-talk-ogn)
drawings:
  persist: false
transition: slide-left
title: How I built an automated podcast powered by Python, LLMs and 11ty
mdc: true
hideInToc: true
---

<div class="overlay">
  <h1>
    building <strong>zed news</strong> &mdash; an automated podcast powered by Python, LLMs and 11ty
  </h1>
</div>

<style>
  .overlay {
    background-color: rgba(0, 0, 0, 0.5); /* Dark background color with some transparency */
    padding: 20px;
  }
</style>

<div class="abs-br m-6 flex gap-2">
  <a href="https://zednews.pages.dev"
     target="_blank" 
     rel="noopener noreferrer" 
     alt="See it live" 
     title="Check it out"
     class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-green">
    <carbon-view />
  </a>
</div>

---
layout: default
transition: fade
---

```json
{
  "name": "Victor Miti",
  "country": "Zambia 🇿🇲",
  "likes": [
    "🛠️ building, breaking and fixing things",
    "🏃 running",
    "🍲 experimenting with food",
    "🎵 making music",
    "🐶 dogs"
  ],
  "work": {
    "org": "Torchbox",
    "url": "torchbox.com",
    "role": "developer"
  },
  "links": {
    "blog": "blog.victor.co.zm",
    "github": "@engineervix",
    "x": "@engineervix"
  }
}
```

---
layout: iframe
url: https://zednews.pages.dev
---


---
layout: image-left
hideInToc: true
image: https://source.unsplash.com/fqbGGMpKLwc/1080x1920
---

# Outline

<Toc maxDepth="1"></Toc>

---
layout: image-right
image: https://source.unsplash.com/TuAtSs8peoM/1080x1920
---

# Why did I build this?

- I'm generally terrible at keeping up with current affairs
- I was inspired by [Hackercast](https://camrobjones.com/hackercast/) -- an AI-generated podcast summary of Hacker News, by [Cameron Jones](https://camrobjones.com/)
- Opportunity to learn how to work with AI tools while solving a real problem

<!--
- time: 1.5 minutes 
- what led you to build this?
-->

---
layout: image-right
image: https://source.unsplash.com/hLit2zL-Dhk/1920x1080
transition: slide-up
---

# The approach

<v-clicks>

- fetch news from various sources using [feedparser](https://pypi.org/project/feedparser/), [requests](https://pypi.org/project/requests/) and [Beautiful Soup](https://pypi.org/project/beautifulsoup4/),
- use [Langchain](https://python.langchain.com/en/latest/index.html) to summarize the news articles and
- use [AWS Polly](https://aws.amazon.com/polly/) to convert text to speech.

</v-clicks>

<!-- 
- time: 2 minutes
- what's the rationale?
- outline the architecture
-->

---
layout: image-left
image: https://res.cloudinary.com/engineervix/image/upload/v1697545495/slidev/zed-news-talk/text-to-speech.jpg
transition: slide-up
---

## Why AWS Polly?

<br />

- reasonable pricing
- nice, straightforward API via [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html).
- many language options and features
- well documented
- plays nicely with [S3](https://aws.amazon.com/s3/), so I have less headaches about where to store the audio files.

---
layout: center
transition: fade
---

## How does it work?

<iframe src="https://giphy.com/embed/BM61CKd08ypJ6" width="800" height="450" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/channelfrederator-crumbs-joseph-herscher-jiwis-machines-BM61CKd08ypJ6">via GIPHY</a></p>

---
layout: center
transition: fade
---

<style>
  .larger-code code {
    font-size: 2.5em;
  }
</style>

<div class="larger-code">
```sh
00 14 * * 1-5 $HOME/SITES/zed-news/cron.sh
```
</div>

---
layout: default
transition: fade
hideInToc: true
---

```sh
# 1. cd to project directory
cd "${HOME}/SITES/zed-news"

# 2. Activate virtual environment
source "${HOME}/Env/zed-news/bin/activate"

# 3. git pull
git pull

# 4. Run podcast toolchain inside docker container
invoke up --build
docker-compose run --rm app invoke toolchain
invoke down

# 5. commit changes
today_iso=$(date --iso)
git add .
git commit --no-verify -m "chore: ✨ new episode 🎙️ » ${today_iso}"

# 6. push changes to remote
git push origin main
```

---
layout: image
image: https://res.cloudinary.com/engineervix/image/upload/v1697572084/slidev/zed-news-talk/healthchecks-io.png
---

---
layout: image-right
image: https://res.cloudinary.com/engineervix/image/upload/v1697571250/slidev/zed-news-talk/telegram-bot.png
transition: fade
hideInToc: true
---

```sh
# Notify Admin via Telegram
today_human_readable=$(date +"%a %d %b %Y")
apprise -vv -t "🎙️ New Episode » ${today_human_readable}" \
  -b "📻 Listen now at ${BASE_URL}/episode/${today_iso}/" \
  tgram://"${TELEGRAM_BOT_TOKEN}"
```


---
layout: image
image: https://res.cloudinary.com/engineervix/image/upload/v1697574721/slidev/zed-news-talk/001.png
transition: fade
---

---
layout: image
image: https://res.cloudinary.com/engineervix/image/upload/v1697574721/slidev/zed-news-talk/001-a.png
transition: fade
---


---
layout: image
image: https://res.cloudinary.com/engineervix/image/upload/v1697574721/slidev/zed-news-talk/002.png
transition: fade
---

---
layout: iframe
url: https://neon.tech/
transition: fade
---

---
layout: iframe
url: https://spacy.io/
transition: fade
---

---
layout: iframe
url: https://www.nltk.org/
transition: fade
---

---
layout: image-right
image: https://source.unsplash.com/49uySSA678U/1080x1920
transition: fade
---

<!-- https://unsplash.com/photos/red-letters-neon-light-49uySSA678U -->

## Which ORM should I use?

- peewee
- Pony
- SQLAlchemy
- SQLObject
- Tortoise

---
layout: iframe
url: https://tortoise.github.io/
transition: fade
---

---
layout: iframe
url: https://lucumr.pocoo.org/2016/10/30/i-dont-understand-asyncio/
transition: fade
---

---
layout: center
transition: fade
---

```python
from tortoise import fields
from tortoise.models import Model
from tortoise.validators import RegexValidator

from app.core.utilities import today


class Article(Model):
    """An article fetched from an online source"""

    # core fields based on fetched data
    source = fields.CharField(max_length=255, description="The source of the article")
    url = fields.CharField(max_length=2047, validators=[RegexValidator(r"^(http|https)://", 0)])
    title = fields.CharField(max_length=511)
    content = fields.TextField(description="The article content as fetched from the source")
    category = fields.CharField(max_length=255, required=False, null=True)

    # additional fields
    date = fields.DateField(default=today, description="The date the article was published")
    summary = fields.TextField(required=False, null=True, description="The AI generated summary of the article")
    episode = fields.ForeignKeyField("models.Episode", related_name="articles", null=True, required=False)

    class Meta:
        table_description = "Articles fetched from various sources"

    def __str__(self):
        return self.title
```

---
layout: image
image: https://res.cloudinary.com/engineervix/image/upload/v1697574721/slidev/zed-news-talk/003.png
transition: fade
---

<div class="abs-br m-6 flex gap-2">
  <a href="https://zed.up.railway.app"
     target="_blank" 
     rel="noopener noreferrer" 
     alt="See the code" 
     title="Show me the code"
     class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-black">
    <carbon-repo-source-code />
  </a>
</div>

---
layout: image
image: https://res.cloudinary.com/engineervix/image/upload/v1697574721/slidev/zed-news-talk/004.png
transition: fade
---

<div class="abs-br m-6 flex gap-2">
  <a href="https://zed.up.railway.app"
     target="_blank" 
     rel="noopener noreferrer" 
     alt="See the code" 
     title="Show me the code"
     class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-black">
    <carbon-repo-source-code />
  </a>
</div>

---
layout: image
image: https://res.cloudinary.com/engineervix/image/upload/v1697574721/slidev/zed-news-talk/005.png
transition: fade
---

<div class="abs-br m-6 flex gap-2">
  <a href="https://zed.up.railway.app"
     target="_blank" 
     rel="noopener noreferrer" 
     alt="See the code" 
     title="Show me the code"
     class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-black">
    <carbon-repo-source-code />
  </a>
</div>

---
layout: image
image: https://res.cloudinary.com/engineervix/image/upload/v1697574721/slidev/zed-news-talk/006.png
transition: fade
---

<div class="abs-br m-6 flex gap-2">
  <a href="https://zed.up.railway.app"
     target="_blank" 
     rel="noopener noreferrer" 
     alt="See the code" 
     title="Show me the code"
     class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-black">
    <carbon-repo-source-code />
  </a>
</div>

---
layout: image
image: https://res.cloudinary.com/engineervix/image/upload/v1697574721/slidev/zed-news-talk/007.png
transition: fade
---

<div class="abs-br m-6 flex gap-2">
  <a href="https://zed.up.railway.app"
     target="_blank" 
     rel="noopener noreferrer" 
     alt="See the code" 
     title="Show me the code"
     class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-black">
    <carbon-repo-source-code />
  </a>
</div>

---
layout: image
image: https://res.cloudinary.com/engineervix/image/upload/v1697574721/slidev/zed-news-talk/008.png
transition: fade
---

<div class="abs-br m-6 flex gap-2">
  <a href="https://zed.up.railway.app"
     target="_blank" 
     rel="noopener noreferrer" 
     alt="See the code" 
     title="Show me the code"
     class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-black">
    <carbon-repo-source-code />
  </a>
</div>

---
layout: image
image: https://res.cloudinary.com/engineervix/image/upload/v1697574721/slidev/zed-news-talk/toolchain.png
transition: fade
---

<div class="abs-br m-6 flex gap-2">
  <a href="https://zed.up.railway.app"
     target="_blank" 
     rel="noopener noreferrer" 
     alt="See the code" 
     title="Show me the code"
     class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-black">
    <carbon-repo-source-code />
  </a>
</div>

---
transition: slide-up
layout: image-right
image: https://res.cloudinary.com/engineervix/image/upload/v1697625801/slidev/zed-news-talk/undraw_prototyping_process_re_7a6p.svg
---

## The frontend

- [11ty.dev](https://www.11ty.dev/) -- a simpler static site generator
- Bootstrap 5
- [plyr.io](https://plyr.io/)
- ua-parser-js

<div class="abs-br m-6 flex gap-2">
  <a href="https://zed.up.railway.app"
     target="_blank" 
     rel="noopener noreferrer" 
     alt="See the code" 
     title="Show me the code"
     class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-black">
    <carbon-repo-source-code />
  </a>
</div>

---
transition: slide-up
layout: image-right
image: https://res.cloudinary.com/engineervix/image/upload/v1697623097/slidev/zed-news-talk/undraw_electricity_k2ft.svg
---


# Practical applications

- Create audio versions of articles / posts ([example](https://townhall.hashnode.com/whats-new-at-hashnode-feb-march-23))
- Use GPT to generate summaries of articles / posts

<!--
- what have you learned from the experience?
- how can it be applied elsewhere?
- what improvements could be made?
-->

---
layout: center
hideInToc: true
---


<h1><a href="https://github.com/engineervix/zed-news"><carbon-logo-github /> github.com/engineervix/zed-news</a></h1>
