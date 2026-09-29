# 📰 Crow RSS Reader Card

A multi-feed RSS reader card for [Home Assistant](https://www.home-assistant.io/). It merges any number of feeds into one list with the newest articles first, shows a thumbnail and summary for each article, can scroll continuously like a news ticker, and can open articles in your browser or in a reader inside the card. You can choose the Classic look with your own colours or a liquid-glass look in light or dark. Optional AI features add Ask, a spoken news briefing, same-story grouping, topics and a weekly recap. Everything can be set up without writing any YAML.

> ✨ **AI features are optional.** Nothing AI-powered runs until you turn on AI features and choose a conversation agent in the editor (see [AI Features Setup](#-ai-features-setup-optional) below). Without an agent, the card works fully as an RSS reader.

---

## ✨ Features

### Feeds
- **Multiple feeds in one list**: add as many RSS or Atom feed URLs as you like. Articles from every feed are merged and sorted newest first.
- **Each article shows** its thumbnail (when the feed has one), headline, a short summary, the date and the name of the source feed.
- **Auto-refresh** on an interval you choose (1–1440 minutes, 30 by default), and a **refresh button** in the header to update on demand.
- **Max articles** sets how many articles are shown.
- **Instant load**: the last set of articles is kept on the device, so the card appears straight away when you open the dashboard, then refreshes in the background. One slow feed doesn't hold up the rest.

### Reading
- **Open in Browser** opens the full article in a new tab.
- **Read in Card** opens a reader sheet inside the card with the headline, source, date and summary, and an **Open** button for the full article.

### Continuous auto-scroll
Turn it on and the articles scroll continuously and loop, like a news ticker. There are three speeds: 🐢 Slow, 🐇 Medium and ⚡ Fast.

### Appearance
- **Style**:
  - **Classic** uses your own colours for everything.
  - **Glass** is a frosted, see-through surface with soft highlights. The header keeps your header colours, and everything else follows the light or dark theme.
- **Theme** for the Glass card and its reader: Auto (follows Home Assistant), Light or Dark.
- **Glass** slider, from clear to frosted.
- **Header**:
  - **Coloured** fills the header with your header colours.
  - **Plain** shows a large bold title on the card itself with a round refresh button.
- **Colours** for the header, header text, background, headlines, meta text and summaries. They're adjusted automatically so they stay readable in both light and dark themes.
- **Colour presets**: Classic (default), Amber, Ocean, Berry and Graphite. One tap applies a preset, and you can then fine-tune any colour.

### AI features (optional)
You need a Home Assistant conversation agent for these. Once AI is turned on, a **⋯ More** button appears in the header next to refresh. It opens a menu titled **News**. Nothing on the card itself mentions AI, and any problems show as plain messages such as "Busy right now".
- **Ask**: ask a question about the latest headlines, or tap a suggestion such as "What are the top stories right now?". The card searches every loaded headline for your question's keywords, so answers cover more than just the newest stories. Answers only use the headlines the card has loaded. The related stories are listed under the answer so you can open them, and you can send any answer to **Announce**.
- **Announce**: the AI writes a short spoken briefing of the top four or five stories, and the card plays it on the speakers you tick. Speakers are grouped by area, and unavailable speakers and TVs are hidden. Nothing is ticked when Announce opens, so you choose the speakers each time. It uses Home Assistant's text-to-speech, and it also works with Music Assistant speakers.
- **Same-story grouping**: when several feeds cover the same event, they become one article with a **"3 sources"** badge. Tap the badge to see every version and open any of them. Results are cached for 24 hours, so a refresh with no new articles doesn't use the AI.
- **This week**: the headlines this device has loaded over the last 7 days, shown as totals, the busiest day and the top sources, plus an AI summary of the week's main stories. Every tile and bar is tappable and narrows the whole view: the numbers, the bars, the main stories and a list of matching headlines.
  - Tap **busiest day** or a day bar to see only that day, then tap **busiest hour** or an hour bar to narrow to one hour.
  - Tap **most headlines**, a source bar or a source in **Sources** to see only that source. With a source picked, tap **latest headline** to open it, or its **rank** (for example "1st of 4 sources") to switch to another source.
  - Filters combine, for example GB News on Tuesday. Each filter shows as a blue pill at the top: tap its ✕ to remove it, or use the back arrow to return to the whole week.
  - Tap **headlines** to see the full list for the current filter, and tap any headline to open it.
  - With Topics on, a **Topics** section shows the week's top topics. Tap one to filter by topic too, for example World stories from GB News on Tuesday.
  - You can also send the summary to **Announce**.
- **Topics**: each headline is tagged with one topic: Politics, World, Business, Technology, Science, Health, Sport, Entertainment, Crime, Weather or Other.
  - Topic pills with counts appear above the list. Tap one to show only that topic, and tap it again or tap **All** to go back.
  - Each article's date line ends with its topic.
  - **Hide topics** in the editor leaves the ticked topics, for example Sport, out of the list and the pills.
  - Each headline is tagged once and kept on the device for about 9 days, so only new headlines are sent to the agent.
- **Fetch full feeds**: most feeds only return their latest 10 stories through the feed service. With this on, the card also reads each feed directly through a public CORS proxy, so Ask and This week see every story the feed publishes. It runs in the background and never slows down the list.

Each feature has its own toggle in the editor. The AI only sees headlines, sources, dates and short summaries. Feed text is treated as data, never as instructions, and every AI answer is escaped before it's shown.

---

## Configuration

Add the card from the card picker, then add your feed URLs in the built-in visual editor. You don't need to write any YAML. The README lists every YAML option.

---

## 🤖 AI Features Setup (Optional)

AI features stay off until you turn them on and choose a conversation agent. **Google Gemini** is the recommended and best-tested agent:

### Step 1 — Enable the Generative Language API

1. Go to [console.cloud.google.com](https://console.cloud.google.com) and sign in
2. Create a new project (or select an existing one)
3. Go to **APIs & Services → Library**
4. Search for **Generative Language API** and click **Enable**

> ⚠️ Don't skip this step. An API key won't work until the Generative Language API is enabled; it will return errors straight away.

### Step 2 — Create an API Key

1. In Google Cloud Console go to **APIs & Services → Credentials**
2. Click **+ Create Credentials → API key** and copy the key

### Step 3 — Add Google Generative AI to Home Assistant

1. In Home Assistant go to **Settings → Devices & Services → + Add Integration**
2. Search for **Google Generative AI** and select it
3. Paste your API key and click Submit
4. The recommended model settings work fine. If you choose a model yourself, pick a **current Flash model**, because Google retires older models regularly (`gemini-2.0-flash` was shut down in June 2026).

### Step 4 — Configure the Card

In the card's visual editor, open **AI Features**, turn on **Enable AI features**, and choose your Google AI agent under **Conversation agent**.

### Rate limits

Free-tier limits vary by model and change over time, so check Google AI Studio for your current quota. The card only calls the agent when you open an AI feature, ask a question, or (with grouping on) when new articles arrive, and it caches answers, so you're unlikely to reach the limit in normal use. If you do see a quota message, it resets the next day.

If the agent can't answer, the card shows the reason in plain English and a **Try again** button. It retries once automatically first.

---

## 🌐 How Feeds Are Fetched

Browsers can't read most RSS feeds directly from a dashboard because of cross-origin restrictions, so the card fetches each feed through the free **[rss2json](https://rss2json.com)** service. It then merges and sorts the results itself.

- Your feed URLs are sent to rss2json each time the card refreshes.
- The free service limits how often you can fetch feeds and how many items it returns for each one, so very long feeds may be cut short.
- Some feeds, such as private or authenticated feeds, or ones that block the service, may not load. In that case the card shows **Error loading feeds**.
- The AI features can't read full article pages, only the headline, summary and content the feed provides.
- **Fetch full feeds** (AI features only, on by default) also requests each feed through a public CORS proxy (allorigins.win, falling back to corsproxy.io). Results are cached for 30 minutes. Turn it off in the editor if you'd rather your feed URLs weren't sent to these services.
- **This week** keeps a list of headlines in your browser's local storage for 7 days. Each device builds its own list, and clearing site data resets it. Feeds only publish their latest stories, so a new list starts from the first day the card loads them. Until it covers a full week, This week notes the date it starts from.
