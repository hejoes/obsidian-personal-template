```dataviewjs
const today = dv.date("today");
const months = ["January", "February", "March", "April", "May", "June",
                "July", "August", "September", "October", "November", "December"];
const dayNames = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
const dayAbbr = dayNames[today.weekday % 7];
const dateStr = String(today.day).padStart(2, '0');
const todayPath = `Notes/${today.year}/${months[today.month - 1]}/${dateStr} ${dayAbbr}`;

if (dv.page(todayPath)) {
    dv.paragraph(`**[[${todayPath}|Tänane päev]]**`);
} else {
    const bold = dv.container.createEl("strong");
    const link = bold.createEl("a", { text: "Tänane päev", cls: "internal-link" });
    link.style.cursor = "pointer";
    link.addEventListener("click", (e) => {
        e.preventDefault();
        app.commands.executeCommandById("periodic-notes:open-daily-note");
    });
}
```

## Teha vaja:

```dataviewjs
const today = dv.date("today");
const months = ["January", "February", "March", "April", "May", "June",
                "July", "August", "September", "October", "November", "December"];
const dayNames = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
const dayAbbr = dayNames[today.weekday % 7];
const dateStr = String(today.day).padStart(2, '0');
const todayPath = `Notes/${today.year}/${months[today.month - 1]}/${dateStr} ${dayAbbr}`;

let page = dv.page(todayPath);
if (!page) {
    // No today's note — find the most recent daily note
    let latest = null;
    const todayT = new Date(today.year, today.month - 1, today.day).getTime();
    for (const p of dv.pages('"Notes"')) {
        if (!/^\d{2} \w{3}$/.test(p.file.name)) continue;
        const pp = p.file.folder.split("/");
        if (pp.length !== 3 || pp[0] !== "Notes") continue;
        const t = new Date(parseInt(pp[1]), months.indexOf(pp[2]), parseInt(p.file.name)).getTime();
        if (t < todayT && (!latest || t > latest.t)) latest = { page: p, t };
    }
    if (latest) page = latest.page;
}
if (page) {
    const tasks = page.file.tasks.where(t => !t.completed);
    if (tasks.length > 0) {
        if (page.file.path !== todayPath) {
            dv.paragraph(`*From [[${page.file.folder}/${page.file.name}|${page.file.name}]]:*`);
        }
        dv.taskList(tasks, false);
    } else {
        dv.paragraph("*All done for today!*");
    }
} else {
    dv.paragraph("*No daily notes yet*");
}
```

## Järgmise 7 päeva üritused

```dataviewjs
const ics = app.plugins.getPlugin('ics');
if (!ics) {
    dv.paragraph("*ICS plugin not installed*");
} else {
    try {
        const CACHE_KEY = 'ics-events-cache';
        const CACHE_DURATION = 60 * 60 * 1000; // 1 hour in ms

        let allEvents = [];
        let cached = null;

        // Try to load from cache
        try {
            cached = JSON.parse(localStorage.getItem(CACHE_KEY));
        } catch (e) {}

        const now = Date.now();
        if (cached && cached.timestamp && (now - cached.timestamp) < CACHE_DURATION) {
            // Use cached data
            allEvents = cached.events.map(e => ({ ...e, date: new Date(e.date) }));
        } else {
            // Fetch fresh data
            const today = new Date();
            for (let i = 0; i < 7; i++) {
                const date = new Date(today);
                date.setDate(today.getDate() + i);
                const events = await ics.getEvents(date);
                if (events && events.length > 0) {
                    events.forEach(e => {
                        allEvents.push({ ...e, date: date.toISOString() });
                    });
                }
            }
            // Save to cache
            localStorage.setItem(CACHE_KEY, JSON.stringify({ timestamp: now, events: allEvents }));
            // Convert dates back
            allEvents = allEvents.map(e => ({ ...e, date: new Date(e.date) }));
        }

        if (allEvents.length === 0) {
            dv.paragraph("*No upcoming events*");
        } else {
            allEvents.sort((a, b) => (a.utime || 0) - (b.utime || 0));
            const items = allEvents.slice(0, 10).map(e => {
                const dateStr = e.date.toLocaleDateString('et-EE', { weekday: 'short', day: 'numeric', month: 'short' });
                const time = e.time || '';
                const location = e.location ? ` @ ${e.location}` : '';
                return `**${dateStr}** ${time} — ${e.summary}${location}`;
            });
            dv.list(items);
        }
    } catch (err) {
        dv.paragraph("*Error loading events: " + err.message + "*");
    }
}
```

## Lingid

- [[SRP & SRE Team Important information]]
- [[Random info/Interesting charts & facts|Charts & Facts]] — [+ Uus random info fail](obsidian://new?vault=Hensu&file=Random%20info%2FNew%20Info)

---

> [!abstract]- Inimesed — [+ Uus inimene](obsidian://new?vault=Hensu&file=Muu%2FInimesed%2FNew%20Person)
> ```dataview
> LIST
> FROM "Muu/Inimesed"
> SORT file.mtime DESC
> ```

> [!abstract]- Kristlus
> ```dataview
> LIST
> FROM "Kristlus"
> SORT file.mtime DESC
> ```

> [!abstract]- Raamatud
> ```dataview
> LIST
> FROM "Raamatud"
> SORT file.mtime DESC
> ```

> [!abstract]- Kalender
> ```dataviewjs
> const today = dv.date("today");
> const months = ["January", "February", "March", "April", "May", "June",
>                 "July", "August", "September", "October", "November", "December"];
> const dayNames = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
>
> function renderMonth(year, month) {
>     const monthName = months[month - 1];
>     const daysInMonth = new Date(year, month, 0).getDate();
>     const firstDay = new Date(year, month - 1, 1).getDay();
>     let cal = `### ${monthName} ${year}\n\n`;
>     cal += "| " + dayNames.join(" | ") + " |\n";
>     cal += "|:---:|:---:|:---:|:---:|:---:|:---:|:---:|\n";
>     let row = "|";
>     for (let i = 0; i < firstDay; i++) row += "   |";
>     for (let day = 1; day <= daysInMonth; day++) {
>         const dayOfWeek = (firstDay + day - 1) % 7;
>         const dateStr = String(day).padStart(2, '0');
>         const dayAbbr = dayNames[new Date(year, month - 1, day).getDay()].substring(0, 3);
>         const notePath = `Notes/${year}/${monthName}/${dateStr} ${dayAbbr}`;
>         const noteExists = dv.page(notePath);
>         const isToday = (day === today.day && month === today.month && year === today.year);
>         if (isToday) {
>             row += noteExists ? ` **[[${notePath}\\|${day}]]** |` : ` **${day}** |`;
>         } else {
>             row += noteExists ? ` [[${notePath}\\|${day}]] |` : ` ${day} |`;
>         }
>         if (dayOfWeek === 6 && day < daysInMonth) row += "\n|";
>     }
>     const lastDay = (firstDay + daysInMonth - 1) % 7;
>     for (let i = lastDay; i < 6; i++) row += "   |";
>     return cal + row;
> }
> dv.paragraph(renderMonth(today.year, today.month));
> let prevMonth = today.month - 1;
> let prevYear = today.year;
> if (prevMonth < 1) { prevMonth = 12; prevYear--; }
> dv.paragraph("\n---\n");
> dv.paragraph(renderMonth(prevYear, prevMonth));
> ```

---

