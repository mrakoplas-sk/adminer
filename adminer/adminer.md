ADMINER – MOJE ÚPRAVY A POSTUP AKTUALIZÁCIE
1. Pracovný adresár
C:\xampp\htdocs\adminer

Zdrojové súbory Admineru:

C:\xampp\htdocs\adminer\adminer

Výsledný skompilovaný Adminer:

C:\xampp\htdocs\adminer\adminer.php
2. Predvolený Limit
Súbor
adminer/include/adminer.inc.php
Nájsť
function selectLimitProcess(): int {
    return (isset($_GET["limit"]) ? intval($_GET["limit"]) : 50);
}
Zmeniť na
function selectLimitProcess(): int {
    return (isset($_GET["limit"]) ? intval($_GET["limit"]) : 20);
}
Výsledok

Predvolený Limit:

20
3. Predvolený Text length
Súbor
adminer/include/adminer.inc.php
Nájsť
function selectLengthProcess(): string {
    return (isset($_GET["text_length"]) ? "$_GET[text_length]" : "100");
}
Zmeniť na
function selectLengthProcess(): string {
    return (isset($_GET["text_length"]) ? "$_GET[text_length]" : "25");
}
Výsledok

Predvolený Text length:

25
4. Import automaticky otvorený
Súbor
adminer/select.inc.php
Nájsť
if (adminer()->selectImportPrint()) {

V tejto časti nájsť:

echo "<span id='import'" . ($_POST["import"] ? "" : " class='hidden'") . ">: ";
Zmeniť na
echo "<span id='import'" . ($_POST["import"] ? "" : " class=''") . ">: ";
Výsledok

Import je po otvorení Admineru automaticky rozbalený.

5. Export automaticky otvorený
Súbor
adminer/select.inc.php
Nájsť
if ($format) {
    print_fieldset("export", lang('Export') . " <span id='selected2'></span>");
Zmeniť

Z:

print_fieldset("export", lang('Export') . " <span id='selected2'></span>");

na:

echo "<fieldset><legend><a href='#fieldset-export' class='toggle'>" . lang('Export') . " <span id='selected2'></span></a></legend><div id='fieldset-export'>";
Celá časť
if ($format) {
    echo "<fieldset><legend><a href='#fieldset-export' class='toggle'>" . lang('Export') . " <span id='selected2'></span></a></legend><div id='fieldset-export'>";
    $output = adminer()->dumpOutput();
    echo ($output ? html_select("output", $output, $adminer_import["output"]) . " " : "");
    echo html_select("format", $format, $adminer_import["format"]);
    echo " <input type='submit' name='export' value='" . lang('Export') . "'>\n";
    echo "</div></fieldset>\n";
}
Výsledok

Export je automaticky otvorený.

6. Domov – vlastný odkaz
Súbor
adminer/include/design.inc.php

Do príslušnej časti menu/footeru bol pridaný:

$current_path = $_SERVER['REQUEST_URI'];


$path = parse_url($current_path, PHP_URL_PATH);


$up_one_level = rtrim(dirname($path), '/\\') . '/';


echo '<div id="menu">
    <a class="nav-link active" aria-current="page" href="'.$up_one_level.'">
        Domov
    </a>
';
Výsledok

V Admineri je odkaz:

Domov

ktorý ide o jednu úroveň adresára vyššie.

7. CodeMirror plugin

Plugin:

highlight-codemirror.php

Zdroj:

plugins/highlight-codemirror.php

Skopírovaný do:

adminer/adminer-plugins/highlight-codemirror.php
8. AI Markdown Export plugin

Plugin je uložený v:

adminer/adminer-plugins/

Načítava sa cez:

adminer/adminer-plugins.php

Obsah:

<?php


return array(
    new AdminerHighlightCodemirror(),
    new AdminerAiMdExport(),
);
Výsledok

Adminer načítava:

AdminerHighlightCodemirror
AdminerAiMdExport
9. AI Markdown Export – anglické texty

V plugine AdminerAiMdExport:

const T = {
    bar: 'Export for AI',
    copy: 'Copy MD',
    copied: 'Copied',
    save: 'Save .md',
    saved: 'Saved',
    failed: 'Failed',
    title: 'SQL Results',
    query: 'Query',
    result: 'Result',
    error: 'Error',
    warning: 'Warning',
    rows: 'rows',
};

Počet dotazov:

const count = document.createElement('span');
count.className = 'ai-md-count';
count.textContent = list.length + ' ' + (list.length === 1 ? 'query' : 'queries');

Výsledok:

1 query
2 queries
10 queries
10. Popis AI pluginu

Pôvodne:

'cs' => array('' => 'Zkopíruje výsledky dotazů jako Markdown pro AI'),

Zmenené na:

'en' => array('' => 'Copy query results as Markdown for AI'),
11. Git – moje nastavenie

Môj GitHub:

https://github.com/mrakoplas-sk/adminer

Git remote origin:

https://github.com/mrakoplas-sk/adminer.git

Originálny Adminer:

https://github.com/vrana/adminer

Git remote upstream:

https://github.com/vrana/adminer.git

Kontrola:

git remote -v

Musí ukázať:

origin    https://github.com/mrakoplas-sk/adminer.git
origin    https://github.com/mrakoplas-sk/adminer.git
upstream  https://github.com/vrana/adminer.git
upstream  https://github.com/vrana/adminer.git
12. Git vetva

Používame:

main

Nie:

master

Kontrola:

git branch

Výsledok:

* main

Push na GitHub:

git push origin main

Nie:

git push origin master
13. Kontrola stavu
cd C:\xampp\htdocs\adminer
git status

História:

git log --oneline -5
14. Uloženie vlastných zmien

Po vykonaní zmien:

git add .

Potom:

git commit -m "Custom Adminer modifications"

A odoslať na GitHub:

git push origin main
15. Aktualizácia Admineru po vydaní novej verzie

NIKDY nemusím začínať odznova kopírovaním celého Admineru.

Najprv:

cd C:\xampp\htdocs\adminer

Stiahnuť nové zmeny od autora:

git fetch upstream

Pozrieť nové commity:

git log --oneline HEAD..upstream/main

Originálny Adminer používa:

upstream/main

nie:

upstream/master
16. Aktualizácia na novú verziu

Pri novej verzii najprv:

git fetch upstream

Potom budeme zosúlaďovať:

moja main
     ↓
upstream/main

Git sa pokúsi ponechať moje vlastné úpravy:

Limit 20
Text length 25
Import otvorený
Export otvorený
Domov
pluginy
AI Markdown Export

Ak vznikne konflikt, nepokračovať naslepo.

Použiť:

git status

a vyriešiť konflikty podľa toho, čo zmenil nový Adminer.

17. Kompilácia Admineru

Po aktualizácii alebo vlastných zmenách treba Adminer skompilovať.

Používa sa:

compile.php

Výsledkom je:

adminer.php

Postup:

zdrojové súbory
      ↓
compile.php
      ↓
adminer.php

Potom otestovať:

Limit = 20
Text length = 25
Import otvorený
Export otvorený
Domov
CodeMirror
AI Markdown Export
18. Po úspešnej aktualizácii

Keď všetko funguje:

git status

Potom:

git add .

Commit:

git commit -m "Update Adminer and preserve custom modifications"

Push:

git push origin main
19. Rýchly checklist pri novej verzii
[ ] cd C:\xampp\htdocs\adminer


[ ] git status


[ ] git fetch upstream


[ ] git log --oneline HEAD..upstream/main


[ ] aktualizovať main z upstream/main


[ ] vyriešiť prípadné konflikty


[ ] skontrolovať Limit = 20


[ ] skontrolovať Text length = 25


[ ] skontrolovať Import = otvorený


[ ] skontrolovať Export = otvorený


[ ] skontrolovať Domov


[ ] skontrolovať CodeMirror plugin


[ ] skontrolovať AI Markdown plugin


[ ] spustiť compile.php


[ ] otestovať výsledný adminer.php


[ ] git status


[ ] git add .


[ ] git commit -m "Update Adminer and preserve custom modifications"


[ ] git push origin main
20. Kompletný zoznam mojich upravených súborov
adminer/include/adminer.inc.php
    - Limit 50 → 20
    - Text length 100 → 25


adminer/select.inc.php
    - Import automaticky otvorený
    - Export automaticky otvorený


adminer/include/design.inc.php
    - vlastný odkaz Domov


adminer/adminer-plugins.php
    - načítanie AdminerHighlightCodemirror
    - načítanie AdminerAiMdExport


adminer/adminer-plugins/highlight-codemirror.php
    - CodeMirror plugin


adminer/adminer-plugins/[AI Markdown plugin]
    - Export výsledkov SQL do Markdownu
    - anglické texty
    - query/queries
    - anglický popis pluginu
Git
origin
https://github.com/mrakoplas-sk/adminer.git


upstream
https://github.com/vrana/adminer.git


branch
main

Toto je celý postup, ktorý si môžeš uložiť napríklad ako ADMINER-UPRAVY.md a pri každej novej verzii podľa neho postupovať.