```dataviewjs
const months = ["January","February","March","April","May","June","July","August","September","October","November","December"];
const dayNames = ["Sun","Mon","Tue","Wed","Thu","Fri","Sat"];
const folder = dv.current().file.folder;
const fp = folder.split("/");
const year = parseInt(fp[1]);
const mi = months.indexOf(fp[2]);
const day = parseInt(dv.current().file.name);
const curT = new Date(year, mi, day).getTime();
function notePath(d) {
    return `Notes/${d.getFullYear()}/${months[d.getMonth()]}/${String(d.getDate()).padStart(2,'0')} ${dayNames[d.getDay()]}`;
}
const yPath = notePath(new Date(year, mi, day - 1));
const tPath = notePath(new Date(year, mi, day + 1));
const yExists = !!dv.page(yPath);
const tExists = !!dv.page(tPath);
let prev = null, next = null;
for (const p of dv.pages('"Notes"')) {
    if (!/^\d{2} \w{3}$/.test(p.file.name)) continue;
    const pp = p.file.folder.split("/");
    if (pp.length !== 3 || pp[0] !== "Notes") continue;
    const t = new Date(parseInt(pp[1]), months.indexOf(pp[2]), parseInt(p.file.name)).getTime();
    if (t < curT && (!prev || t > prev.t)) prev = { path: `${p.file.folder}/${p.file.name}`, t };
    if (t > curT && (!next || t < next.t)) next = { path: `${p.file.folder}/${p.file.name}`, t };
}
let nav = "*Navigation*\n<< ";
if (!yExists && prev) nav += `[[${prev.path}|← ${prev.path.split("/").pop()}]] `;
nav += `[[${yPath}|Eile]] | [[${tPath}|Homme]]`;
if (!tExists && next) nav += ` [[${next.path}|${next.path.split("/").pop()} →]]`;
nav += " >>";
dv.paragraph(nav);
```

# 📝 
- 

## Vaja teha:
<%*
const months = ["January","February","March","April","May","June","July","August","September","October","November","December"];
const dailyNoteRegex = /^Notes\/(\d{4})\/(\w+)\/(\d{2}) \w{3}\.md$/;

// Parse current note's date
const fp = tp.file.folder(true).split("/");
const curT = new Date(parseInt(fp[1]), months.indexOf(fp[2]), parseInt(tp.file.title)).getTime();

// Find the most recent daily note before this one
let prevFile = null;
let prevTime = 0;
for (const file of app.vault.getFiles()) {
    const match = file.path.match(dailyNoteRegex);
    if (!match) continue;
    const t = new Date(parseInt(match[1]), months.indexOf(match[2]), parseInt(match[3])).getTime();
    if (t < curT && t > prevTime) {
        prevFile = file;
        prevTime = t;
    }
}

let hasTodos = false;
if (prevFile) {
    const content = await app.vault.read(prevFile);
    const todos = content.match(/^- \[ \] .+$/gm);
    if (todos && todos.length > 0) {
        tR += todos.join('\n');
        hasTodos = true;
    }
}
if (!hasTodos) {
    tR += '- [ ] ';
}
-%>

---

[[Home]]