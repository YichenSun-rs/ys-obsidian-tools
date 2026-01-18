<%*
const baseName = "$FILENAME"; 
// As long as the file name stays the same, moving the file will not break this.

const matches = app.vault.getMarkdownFiles()
  .filter(f => f.basename === baseName);

if (matches.length === 0) { 
  tR += `> ⚠️ File not found: ${baseName}`; 
  return; 
}

const rawContent = await app.vault.read(matches[0]);

// 1) Remove frontmatter if it exists
const content = rawContent.replace(/^---\n[\s\S]*?\n---\n/, "");

// 2) Only take list items as quote candidates
//    This avoids forms / HTML / headers
let candidates = content.split("\n")
  .map(l => l.trim())
  .filter(l =>
    /^([-*+]\s+|\d+\.\s+)/.test(l)   // - / * / + / 1.
  )
  .map(l => l.replace(/^([-*+]\s+|\d+\.\s+)/, "").trim())
  .filter(l =>
    l.length >= 4 &&                 // avoid very short lines
    !/^\d+$/.test(l) &&              // avoid pure numbers
    !/^<.*>$/.test(l) &&             // avoid pure HTML
    !l.includes("<progress")         // avoid progress bar lines
  );

if (candidates.length === 0) {
  tR += `> ⚠️ No valid quote found.\n> Please write quotes as list items, e.g.:\n> - Freedom comes from boundaries.`;
  return;
}

// Pick one randomly
const pick = candidates[Math.floor(Math.random() * candidates.length)];

// Support "Quote -- Author" format
let text = pick;
let author = baseName;

if (pick.includes(" -- ")) {
  const parts = pick.split(" -- ");
  text = parts[0].trim();
  author = parts.slice(1).join(" -- ").trim(); // in case author also contains --
}

// Output
tR += `>【**Quote of the Day**】\n\t ${text}\n— ${author}`;
%>
