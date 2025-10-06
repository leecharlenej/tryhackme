# Yara
Applicatgions and Yara language for threat itnelligence, forensics and threat hunting.

## Content:
[Task 1: Introduction](#task-1-introduction)
<br>[Task 2: What is Yara](#task-2-what-is-yara)
<br>[Task 3: Deploy](#task-3-deploy)
<br>[Task 4: Introduction to Yara Rules](#task-4-introduction-to-yara-rules)
<br>[Task 5: Expanding on Yara Rules](#task-5-expanding-on-yara-rules)
<br>[Task 6: Yara Modules](#task-6-yara-modules)
<br>[Task 7: Other tools and Yara](#task-7-other-tools-and-yara)
<br>[Task 8: Using LOKI and its Yara rule set](#task-8-using-loki-and-its-yara-rule-set)
<br>[Task 9: Creating Yara rules with yarGen]()
<br>[Task 10: Valhalla]()
<br>[To note](#to-note)

## Task 1: Introduction
Pre-requisite:
<ul>
<li>Basic Linux: E.g. installing software and commands for general navigation of system.</li>
</ul>

## Task 2: What is Yara
| Topic | Description |
|-------|-------------|
| Yara can identify info based on | Binary and textual patterns. E.g. hexadecimal and strings contained in a file. |
| Rules | Used to label these patterns.<br>Yara rules are written to determine if file is malicious or not, based upon features/ patterns it presents.<br>Strings = Fundamental component of programming languages. Applications use strings to store data such as text.<br>E.g. Can write Yara rule to search for "hello world" string in every program on our OS. |
| Why does malware use strings? | Malware uses strings to store textual data.<br><br>E.g.<ul><li><b>Type:</b> Ransomware - <b>Data:</b> 12t9YDPgwueZ9NyMgw519p7AA8isjr6SMw - <b>Description:</b> Bitcoin Wallet for ransom payments.</li><li><b>Type:</b> Botnet - <b>Data:</b> 12.34.56.7 - <b>Description:</b> IP address of C2 server.</li></ul> |

## Task 3: Deploy
THM: In-browser VM instance (vs. via SSH)

## Task 4: Introduction to Yara Rules
| Topic | Description |
|-------|-------------|
| Yara rule | Language used for rules is simple to pick up but hard to master. Rule is only as effective as your understanding of patterns you want to search for. |
| Yara command requires | 2 arguments to be valid: <ol><li>Rule file we create</li><li>Name of file, directory or process ID to use rule for. |
| Standard file extension for all Yara rules | .yar |
| Content of Yara rule file | In below's example:<ul><li><b>Name:</b> myfirstrule</li><li><b>Condition:</b> true</li></ul> |

| No. | Step | Command |
|-----|------|---------|
| 1. | Create a file to apply Yara rule. | `touch somefile` |
| 2. | Create a new file and name it "myfirstrule.yar" | `touch myfirstrule.yar` |
| 3. | Open up "myfirstrule.yar" using text editor, input snipper and save. <br><br>Rule checks to see if file/ directory/ PID specified exists via `condition:true`. If file exists, then given output of `examplerule`. | `nano myfirstrule.yar`<br><br>`rule examplerule {condition: true}`<br><br>`CTRL+O`<br>`CTRL+X`|
| 4. | To run Yara rule on file.<br><br>Output:<ul><li><b>If exists:</b> `examplerule somefile`.</li><li><b>If not:</b> Error message.</li></ul><br><br>When rule matches, output is (`rule_name filename`. Can add flags for more details such as `-s`/ `--print-strings` to print strings matched, `-m` to print meta, etc. | `yara my firstrule.yar somefile` |

## Task 5: Expanding on Yara Rules
| Topic | Description |
|-------|-------------|
| Meta | Reserved for descriptive info by rule author. E.g. Can use `desc` (description) to summarize what your rule checks for.<br><br>Anything within this section does not influence the rule itself. Similar to commenting code, useful to summarize your rule. |
| Strings<ol><li>Define keyword `strings`, where string we want to search for is stored in variable `$hello_world`.<li>Need condition to make rule valid. Here, we use variable's name to amke the string the condition.</li></ol>| Can use strings to search for specific text/ hexadecimal in files or programs.<br><br>E.g. Want to search directory for all files containing "Hello World".<br>`rule helloworld_checker{`<br>`strings:`<br>`$hello_world = "Hello World!"`<br><br>`condition:`<br>`$hello_world`<br>`}`<br><br>Want to account for all upper and lower cases:<br>`rule helloworld_checker{`<br>`strings:`<br>`$hello_world = "Hello World!"`<br>`$hello_world_lowercase = "hello world"`<br>`$hello_world_uppercase = "HELLO WOLRD"`<br><br>`condition:`<br>`any of them`<br>` |
| Conditions | E.g. `true`, `any of them`, `<=`, `>=`, `!=`<br><br>E.g.: (1) Search for "Hello World!" string. (2) Only say rule matches if there are less than or equal to 10 occurrences. <br>`rule helloworld_checker{`<br>`strings:`<br>`$hello_world = "Hello World!"`<br><br>`condition:`<br>`$hello_world <= 10`<br>`}` |
|Combining keywords |  `and`, `not`, `or`<br><br>E.g. To check if file has string + of a certain size. If match,`helloworld_textfile_checker mytextfile.txt`. If doesn't, no output.<br>`rule helloworld_checker{`<br>`strings:`<br>`$hello_world = "Hello World!"`<br><br>`condition:`<br>`$hello_world and filesize < 10KB`<br>`}`|
| Weight| |
| Cheatsheet for Yara rule's anatomy | [Click](https://miro.medium.com/v2/resize:fit:875/1*gThGNPenpT-AS-gjr8JCtA.png)|

## Task 6: Yara Modules
Frameworks (E.g. Below.) improve technicality of Yara rules.

| Library | Description |
|---------|-------------|
| Cuckoo | Cuckoo Sandbox is an automated malware analysis environment. This module allows you to generate Yara rules based on behaviours discovered from Cuckoo Sandbox. As environment executes malware, can create rules on specific behaviours such as runtime strings etc. |
| Python PE | Python's PE module allows you to create Yara rules from various sections and elements of Windows PE structure; this structure is standard formatting of all executables and DLL files on Windows, including the programming libraries that are used.<br>Examining PE file's contents is essential in malware analysis; behaviours such as cryptography or worming can be largely identified without reverse engineering or sample execution. |

## Task 7: Other tools and Yara
<ul>
<li>Know how to create custom Yara rules is useful.</li>
<li>BUT don't have to create many rules from scratch to begin using Yara; many GitHub resources and open-source tools (and commercial products) that can be used to leverage Yara in hunt operations and/ or incident response engagements.</li>
</ul>

| Tool | Description |
|------|-------------|
| LOKI | Free open-source IOC scanner that can be used on both Windows and Linux systems. Detection is based on 4 methods: (1) File name IOC check (2) Yara rule check (3) Hash check (4) C2 back connect check (5) Additional checks that LOKI can be used for.<br><br>To run: `python3 loki.py -h` |
| THOR | THOR Lite is a free multi-platform IOC and Yara scanner and have precompiled versions for Windows, Linux, macOS. A nice feature is its scan throttling to limit exhuasting CPU resources.<br><br>THOR is for corporate customers.<br><br>To run: `./thor-lite-linux-64 -h`|
| FENRIR | A bash script which is a simple bash IOC checker that runs on any system capable of running bash (Including Windows.). Updated version was created to address issue where requirements must be met for them to function.<br><br>To run: `./fenrir.sh` |
| YAYA (Yet Another Yara Automation) | Open-source tool that runs only on Linux to help researchers manage multiple YARA rule repo. Starts by importing a set of high-quality YARA rules and then, let researchers add their own rules, disable specific rulesets and run file scans. |

## Task 8: Using LOKI and its Yara rule set
<ul>
<li>Security analyst: Need to research various threat intelligence reports, blog postings etc. and gather info on latest tactics and techniques used in the wild, past or present.</li>
<li>These readings usually will share IOCs (E.g. Hashes, IP addresses, domain names.) so that rules can be created to detect these threats in your environment, along with Yara rules.</li>
<li>BUT might also encounter something unknown and current security stack of tools can't/ didn't detect.<li>
<li>So, use tools like Loki BUT also need to add own rules based on threat intelligence gathers or findings from incident response engagement (forensics).</li>
</ul>

<b>Using LOKI</b>
| Action | Command |
|--------|---------|
| THM: To locate LOKI (In tools folder). | `cd tools`<br>`cd loki` |
| To see options available for LOKI | `python loki.py -h` |
| To update LOKI; add the signature-base directory which Loki uses to scan for known evil. | `python loki.py --update` |
| To see the different Yara files used by LOKI, navigate to the `yara` directory. | `cd signature-base`<br>`cd yara`<br>`ls` |
| To run LOKI, go to the suspicious file directory. `.` represents the current directory. LOKI will scan directory currently in. | `python ../../tools/Loki/loki.py -p .` |

<b>THM Questions</b>
| Question | Answer |
|----------|--------|
| Scan file 1. Does Loki detect this file as suspicious/malicious or benign? | Go the the directory of file 1.<br>`python ../../tools/Loki/loki.py -p .`<br><br><i>Suspicious</i> |
| What Yara rule did it match on?| `MATCH: webshell_metaslsoft`<br><br><i>webshell_metaslsoft</i> |
| What does Loki classify this file as? | `DESCRIPTION: Web Shell - file metaslsoft.php`<br><br><i>Web Shell</i> |
| Based on the output, what string within the Yara rule did it match on? | `MATCHES: Str1: $buff .= "<tr><td><a href=\\"?d=".$pwd."\\">[ $folder ]</a></td><td>LINK</t`<br><br><i>Str1</i>|
| What is the name and version of this hack tool? | `FIRST_BYTES: 3c3f7068700a2f2a0a09623337346b20322e320a / <?php/*b374k 2.2`<br><br><i>b374k 2.2</i> |
| Inspect the actual Yara file that flagged file 1. Within this rule, how many strings are there to flag this file? | `grep -RIn 'webshell' . -l`<br>`nano thor-webshells.yar`<br>`FN+F6` (Don't do `CTRL+W`; closes browser window.)<br><br>or<br><br>`grep -n -A12 "metaslsoft" thor-webshells.yar`<br><br><i>1</i>|
| Scan file 2. Does Loki detect this file as suspicious/malicious or benign? | Go the the directory of file 2.<br><br>`python ../../tools/Loki/loki.py -p .`<br><br>`[RESULT] SYSTEM SEEMS TO BE CLEAN.`<br><br><i>Benign</i>|
| Inspect file 2. What is the name and version of this web shell? | `nano 1ndex.php`<br><br>`b374k shell 3.2.3`<br><br><i>b374k 3.2.3</i> |