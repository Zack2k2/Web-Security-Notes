<h3>Examples of nmap command</h3>

`nmap -v scanme.nmap.org`

verbose scan tcp

`nmap -sS -O scanme.nmap.org`

does a stealth scan and OS detection

`nmap -sT -T3 -O scanme.nmap.org/24`

-T paranoid|sneaky|polite|normal|aggressive|insane (Set a timing template)
           While the fine-grained timing controls discussed in the previous section are powerful and effective, some people find them confusing. Moreover,
           choosing the appropriate values can sometimes take more time than the scan you are trying to optimize. Fortunately, Nmap offers a simpler approach,
           with six timing templates. You can specify them with the -T option and their number (0–5) or their name. The template names are paranoid (0),
           sneaky (1), polite (2), normal (3), aggressive (4), and insane (5). The first two are for IDS evasion. Polite mode slows down the scan to use less
           bandwidth and target machine resources. Normal mode is the default and so -T3 does nothing. Aggressive mode speeds scans up by making the assumption
           that you are on a reasonably fast and reliable network. Finally insane mode assumes that you are on an extraordinarily fast network or are willing to
           sacrifice some accuracy for speed.


