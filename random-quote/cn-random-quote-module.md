<%*
const baseName = "$文件名"; // 只要文件名不变，移动也不怕
const matches = app.vault.getMarkdownFiles().filter(f => f.basename === baseName);

if (matches.length === 0) { tR += `> ⚠️ 找不到文件：${baseName}`; return; }

const contentRaw = await app.vault.read(matches[0]);

// 1) 去掉 frontmatter（如果有）
const content = contentRaw.replace(/^---\n[\s\S]*?\n---\n/, "");

// 2) 只抓“列表项”作为候选（避免抽到表单/HTML/标题）
let candidates = content.split("\n")
  .map(l => l.trim())
  .filter(l =>
    /^([-*+]\s+|\d+\.\s+)/.test(l) // - / * / + / 1.
  )
  .map(l => l.replace(/^([-*+]\s+|\d+\.\s+)/, "").trim())
  .filter(l =>
    l.length >= 4 &&                 // 太短的不要（你可改成 2）
    !/^\d+$/.test(l) &&              // 纯数字不要
    !/^<.*>$/.test(l) &&             // 纯 HTML 行不要
    !l.includes("<progress")         // 进度条这种不要
  );

if (candidates.length === 0) {
  tR += `> ⚠️ 没找到可抽取的“列表项精句”。\n> 建议：把精句都用「- 」开头写成列表。\n>（比如：- 真正的自由来自边界）`;
  return;
}

const pick = candidates[Math.floor(Math.random() * candidates.length)];
let text = pick;
let author = "$文件名";

if (pick.includes(" -- ")) {
  const parts = pick.split(" -- ");
  text = parts[0].trim();
  author = parts.slice(1).join(" -- ").trim(); // 防止作者里也有 --
}

tR += `>【**今日一句**】\n\t ${text}\n— ${author}`;
%>
