# EZ05 - Vulnerability Management

by Cyber.SoHo

This is the list of all of the Course' materials into GitHub :

{% comment %}
==========================================================================
 HOW THIS PAGE BUILDS ITSELF
 (to reuse it in another course, change only the title lines above)

 Every file whose type is in "extensions" (SETTING 1, below this note)
 is listed automatically.

 1. NUMBERS make the bullets nest
      03. Title.html           ->  a bullet
      03.01. Flashcards.html   ->  nested under 03
      03.02. Lab.sql           ->  nested under 03
    When a number has no single main file (only 12.01, 12.02 ... or two
    files named "12. ..."), the bullet shows the number alone and all of
    its files are nested under it.
    Files without a number are listed as plain bullets.

 2. FOLDERS make the structure, as deep as they go
    A top-level folder becomes a heading. Every folder inside it becomes
    a folder line, with its own files and folders nested under it.
    Files in the root of the repository come first, without a heading.
    Every folder can be folded: a click on its name opens or closes it,
    the two buttons at the top open or close all of them, and the page
    remembers which folders each visitor left open. SETTING 3 decides
    whether folders start closed or open.

 3. ORDER is alphabetical by name, so keep two digits: 01, 02 ... 10.
    Folders are sorted the same way: name them "1 - Lectures",
    "2 - Exercises" to choose which one comes first.

 4. TO KEEP SOMETHING OFF THIS PAGE
    - Add the file name or folder name to "skip" (SETTING 2): it stays
      on the site but is not listed. Meant for logos, image folders
      and similar.
    - Or start the name with an underscore:   _draft.pdf     _Week 09/
      It is then not published at all. It can still be seen by anyone
      who opens the GitHub repository itself.

 5. LINKS open in a separate browser tab, so this page stays open
    behind them. SETTING 4 switches that off.

 6. MARKDOWN FILES, AND FILES WHOSE FIRST LINE IS ---
    GitHub Pages turns every .md file into a web page; the link on this
    page opens that web page. It also processes any other file whose
    first line is three dashes, which is common in YAML: if a second
    line of three dashes follows, everything above it is cut off.
    In all of those files, text inside double curly braces or inside
    curly-brace-percent tags is treated as template code: it disappears,
    or it stops the whole site from building.
    YAML: delete the first --- line. Markdown: put that text between
    Liquid raw tags. Or share the file inside a .zip.
==========================================================================
{% endcomment %}
{%- comment -%} SETTING 1 - file types to list {%- endcomment -%}
{%- assign extensions = ".pdf,.html,.md,.txt,.csv,.json,.yaml,.yml,.sql,.py,.sh,.ps1,.ps,.zip,.rar,.png,.jpg,.jpeg,.svg,.mp3,.mp4,.avi,.mkv" | split: "," -%}
{%- comment -%} SETTING 2 - file names or folder names to leave off the list {%- endcomment -%}
{%- assign skip = "README.md,cyber_soho.png" | split: "," -%}
{%- comment -%} SETTING 3 - how the folders look when the page opens: "closed" or "open" {%- endcomment -%}
{%- assign folders_start = "closed" -%}
{%- comment -%} SETTING 4 - where a clicked link opens: "new tab" or "same tab" {%- endcomment -%}
{%- assign links_open_in = "new tab" -%}

{%- assign no_items = "" | split: "" -%}
{%- assign digits = "0,1,2,3,4,5,6,7,8,9" | split: "," -%}
{%- comment -%} Two invisible separator characters: no file name contains them and they sort before everything else {%- endcomment -%}
{%- assign c1 = "%01" | url_decode -%}
{%- assign c2 = "%02" | url_decode -%}
{%- assign tab_mark = ' target="_blank" rel="noopener"' -%}
{%- if links_open_in == "same tab" -%}{%- assign tab_mark = "" -%}{%- endif -%}

{%- comment -%} ---- 1. Collect everything in the repository: pages that GitHub Pages builds, then ordinary files ---- {%- endcomment -%}
{%- assign found = no_items -%}
{%- assign page_paths = no_items -%}
{%- for p in site.pages -%}
  {%- assign path = p.path | prepend: "/" | replace: "//", "/" -%}
  {%- assign page_paths = page_paths | push: path -%}
  {%- if p.path == page.path -%}{%- continue -%}{%- endif -%}
  {%- assign item = path | append: c1 | append: p.url | append: c1 | append: "page" -%}
  {%- assign found = found | push: item -%}
{%- endfor -%}
{%- for file in site.static_files -%}
  {%- if page_paths contains file.path -%}{%- continue -%}{%- endif -%}
  {%- assign item = file.path | append: c1 | append: file.path | append: c1 | append: "file" -%}
  {%- assign found = found | push: item -%}
{%- endfor -%}

{%- comment -%} ---- 2. Keep the listed types, drop what is in "skip", work out each link, sort by folder and name ---- {%- endcomment -%}
{%- assign records = no_items -%}
{%- for item in found -%}
  {%- assign parts = item | split: c1 -%}
  {%- assign crumbs = parts[0] | split: "/" -%}
  {%- assign name = crumbs | last -%}
  {%- unless name contains "." -%}{%- continue -%}{%- endunless -%}
  {%- assign ext = name | split: "." | last | downcase | prepend: "." -%}
  {%- unless extensions contains ext -%}{%- continue -%}{%- endunless -%}
  {%- assign hide = false -%}
  {%- for crumb in crumbs -%}
    {%- if skip contains crumb -%}{%- assign hide = true -%}{%- break -%}{%- endif -%}
  {%- endfor -%}
  {%- if hide -%}{%- continue -%}{%- endif -%}
  {%- comment -%} A % # or ? inside a name would otherwise break its link {%- endcomment -%}
  {%- if parts[2] == "page" -%}
    {%- assign url = parts[1] | replace: "#", "%23" | replace: "?", "%3F" | relative_url -%}
  {%- else -%}
    {%- assign url = parts[1] | replace: "%", "%25" | replace: "#", "%23" | replace: "?", "%3F" | relative_url -%}
  {%- endif -%}
  {%- assign record = parts[0] | append: c1 | append: url -%}
  {%- assign records = records | push: record -%}
{%- endfor -%}
{%- assign records = records | sort_natural -%}

{%- comment -%} ---- 3. For each file work out its folder, its number and its link; note every folder on the way ---- {%- endcomment -%}
{%- assign folders = no_items -%}
{%- assign units = no_items -%}
{%- assign kinds = no_items -%}
{%- assign entries = no_items -%}
{%- assign tree = no_items -%}
{%- assign last_folder = c1 -%}
{%- for record in records -%}
  {%- assign parts = record | split: c1 -%}
  {%- assign crumbs = parts[0] | split: "/" -%}
  {%- assign name = crumbs | last -%}
  {%- assign crumbs = crumbs | pop -%}
  {%- assign folder = crumbs | join: "/" -%}
  {%- assign pieces = name | split: "." -%}
  {%- assign ext = pieces | last -%}
  {%- assign pieces = pieces | pop -%}
  {%- assign stem = pieces | join: "." -%}
  {%- assign lead = pieces[0] | strip -%}
  {%- assign second = pieces[1] | strip -%}
  {%- comment -%} Take the digits out: if nothing is left, the piece was a number {%- endcomment -%}
  {%- assign lead_left = lead -%}
  {%- assign second_left = second -%}
  {%- for d in digits -%}
    {%- assign lead_left = lead_left | remove: d -%}
    {%- assign second_left = second_left | remove: d -%}
  {%- endfor -%}
  {%- assign unit = "" -%}
  {%- assign kind = "" -%}
  {%- if lead != "" and lead_left == "" -%}
    {%- assign unit = lead -%}
    {%- assign kind = "main" -%}
    {%- if second != "" and second_left == "" -%}
      {%- assign kind = "sub" -%}
    {%- endif -%}
  {%- endif -%}
  {%- capture entry -%}
    <a href="{{ parts[1] | escape }}"{{ tab_mark }}>{{ stem | escape }}</a> <small class="file-type">{{ ext | upcase }}</small>
  {%- endcapture -%}
  {%- assign folders = folders | push: folder -%}
  {%- assign units = units | push: unit -%}
  {%- assign kinds = kinds | push: kind -%}
  {%- assign entries = entries | push: entry -%}
  {%- comment -%} New folder: remember it and every folder above it, as keys that sort parents right before their contents {%- endcomment -%}
  {%- if folder != last_folder -%}
    {%- assign last_folder = folder -%}
    {%- assign key = "" -%}
    {%- if folder == "" -%}{%- assign tree = tree | push: key -%}{%- endif -%}
    {%- for crumb in crumbs -%}
      {%- if crumb == "" -%}{%- continue -%}{%- endif -%}
      {%- assign small = crumb | downcase -%}
      {%- assign key = key | append: c2 | append: small | append: c1 | append: crumb -%}
      {%- assign tree = tree | push: key -%}
    {%- endfor -%}
  {%- endif -%}
{%- endfor -%}
{%- assign tree = tree | uniq | sort -%}
{%- assign file_folders = folders | uniq -%}
{%- assign deepest = tree | last -%}
{%- assign open_mark = "" -%}
{%- if folders_start == "open" -%}{%- assign open_mark = " open" -%}{%- endif -%}

{%- comment -%} ---- 4. Print: walk the folders in order, opening and closing lists as the depth changes ---- {%- endcomment -%}
<div class="course-materials">
{%- if records.size == 0 -%}
  <p><em>No materials have been posted yet.</em></p>
{%- endif -%}
{%- if tree.size > 0 and deepest != "" -%}
  <p class="fold-buttons" hidden><button type="button" data-fold="open">Unfold all</button> <button type="button" data-fold="closed">Fold all</button></p>
{%- endif -%}
{%- assign open = 0 -%}
{%- for key in tree -%}
  {%- comment -%} Read the folder, its name and its depth back from the key {%- endcomment -%}
  {%- assign folder = "" -%}
  {%- assign title = "" -%}
  {%- assign depth = 0 -%}
  {%- assign steps = key | split: c2 -%}
  {%- for step in steps -%}
    {%- if step == "" -%}{%- continue -%}{%- endif -%}
    {%- assign title = step | split: c1 | last -%}
    {%- assign folder = folder | append: "/" | append: title -%}
    {%- assign depth = depth | plus: 1 -%}
  {%- endfor -%}
  {%- comment -%} Close the folders that are finished, deepest first {%- endcomment -%}
  {%- for n in (depth..open) -%}
    {%- assign level = open | minus: forloop.index0 -%}
    {%- if level > 1 -%}</ul></details></li>{%- elsif level == 1 -%}</ul></details>{%- endif -%}
  {%- endfor -%}
  {%- comment -%} Open this one: plain list for the root, foldable heading for a top-level folder, foldable folder line below that {%- endcomment -%}
  {%- if depth == 0 -%}
    <ul>
  {%- elsif depth == 1 -%}
    <details class="top"{{ open_mark }} data-folder="{{ folder | escape }}"><summary><h2>{{ title | escape }}</h2></summary><ul>
  {%- else -%}
    <li class="folder"><details{{ open_mark }} data-folder="{{ folder | escape }}"><summary>{{ title | escape }}</summary><ul>
  {%- endif -%}
  {%- assign open = depth -%}
  {%- if file_folders contains folder -%}
    {%- comment -%} The files that sit directly in this folder {%- endcomment -%}
    {%- assign here = no_items -%}
    {%- for f in folders -%}
      {%- if f == folder -%}{%- assign here = here | push: forloop.index0 -%}{%- endif -%}
    {%- endfor -%}
    {%- assign shown = no_items -%}
    {%- for i in here -%}
      {%- assign unit = units[i] -%}
      {%- if unit == "" -%}
        <li>{{ entries[i] }}</li>
        {%- continue -%}
      {%- endif -%}
      {%- if shown contains unit -%}{%- continue -%}{%- endif -%}
      {%- assign shown = shown | push: unit -%}
      {%- comment -%} Collect every file of this folder that carries the same number {%- endcomment -%}
      {%- assign members = no_items -%}
      {%- assign mains = no_items -%}
      {%- for j in here -%}
        {%- if units[j] == unit -%}
          {%- assign members = members | push: j -%}
          {%- if kinds[j] == "main" -%}
            {%- assign mains = mains | push: j -%}
          {%- endif -%}
        {%- endif -%}
      {%- endfor -%}
      {%- if members.size == 1 -%}
        <li>{{ entries[i] }}</li>
      {%- elsif mains.size == 1 -%}
        {%- assign m = mains[0] -%}
        <li>{{ entries[m] }}<ul>
        {%- for j in members -%}
          {%- if j != m -%}<li>{{ entries[j] }}</li>{%- endif -%}
        {%- endfor -%}
        </ul></li>
      {%- else -%}
        <li><strong>{{ unit }}</strong><ul>
        {%- for j in members -%}
          <li>{{ entries[j] }}</li>
        {%- endfor -%}
        </ul></li>
      {%- endif -%}
    {%- endfor -%}
  {%- endif -%}
  {%- if depth == 0 -%}</ul>{%- endif -%}
{%- endfor -%}
{%- for n in (1..open) -%}
  {%- assign level = open | minus: forloop.index0 -%}
  {%- if level > 1 -%}</ul></details></li>{%- else -%}</ul></details>{%- endif -%}
{%- endfor -%}
</div>

<style>
  .course-materials .file-type { font-size: .7em; letter-spacing: .04em; padding: 0 .4em; border: 1px solid; border-radius: 3px; opacity: .6; vertical-align: middle; }
  .course-materials summary { cursor: pointer; list-style: none; }
  .course-materials summary::-webkit-details-marker { display: none; }
  .course-materials summary h2 { margin: 14px 0 8px; }
  .course-materials summary h2::before, .course-materials .folder summary::before { content: "\1F4C1\00A0"; }
  .course-materials details[open] > summary h2::before, .course-materials .folder > details[open] > summary::before { content: "\1F4C2\00A0"; }
  .course-materials .folder { list-style: none; }
  .course-materials .folder > details > summary { font-weight: 600; margin-left: -1.6em; }
  .course-materials .fold-buttons { text-align: right; margin: 0 0 .4em; }
  .course-materials .fold-buttons button { font: inherit; font-size: .8em; padding: .15em .7em; border: 1px solid; border-radius: 4px; background: none; color: inherit; opacity: .7; cursor: pointer; }
</style>

<script>
{% raw %}
(function () {
  var box = document.querySelector(".course-materials");
  if (!box) { return; }
  var folders = Array.prototype.slice.call(box.querySelectorAll("details"));
  var buttons = box.querySelector(".fold-buttons");
  if (folders.length === 0 || !buttons) { return; }
  var key = "open-folders:" + location.pathname;
  function remember() {
    var open = folders.filter(function (d) { return d.open; }).map(function (d) { return d.getAttribute("data-folder"); });
    try { localStorage.setItem(key, JSON.stringify(open)); } catch (e) {}
  }
  try {
    var saved = JSON.parse(localStorage.getItem(key));
    if (Array.isArray(saved)) {
      folders.forEach(function (d) { d.open = saved.indexOf(d.getAttribute("data-folder")) !== -1; });
    }
  } catch (e) {}
  folders.forEach(function (d) { d.addEventListener("toggle", remember); });
  buttons.hidden = false;
  Array.prototype.forEach.call(buttons.querySelectorAll("button"), function (b) {
    b.addEventListener("click", function () {
      var open = b.getAttribute("data-fold") === "open";
      folders.forEach(function (d) { d.open = open; });
      remember();
    });
  });
})();
{% endraw %}
</script>
