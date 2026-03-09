Hashed Password Cracking & Cryptanalysis

<p>
This project demonstrates the use of <b>John the Ripper</b> to perform cryptographic attacks against a SHA-1 hashed password dataset. By analyzing password distribution statistics and implementing multi-stage attacks—ranging from dictionary-based wordlists to custom rule mutations and masks—this lab simulates the real-world methodology used to recover plain-text credentials from leaked databases.
</p>

<h2>Utilities Used</h2>
<ul>
<li><b>John the Ripper (JtR):</b> Primary password recovery tool.</li>
<li><b>Hash Dataset:</b> 1,000 SHA-1 hashes (cp_leak.txt).</li>
<li><b>Wordlists:</b> <code>lower.lst</code> and <code>rockyou.txt</code>.</li>
<li><b>Kali Linux:</b> Security-focused OS for execution.</li>
</ul>

<h2>Lab Walk-through</h2>

<h3>Step 1: Initial Wordlist Attack</h3>
<p>
The first stage involves a standard dictionary attack. This compares the hashed entries in <code>cp_leak.txt</code> against the hashes of words found in the provided <code>lower.lst</code> file.
</p>
<p align="left">
<img src="https://imgur.com/5JJx4Av.png" height="70%" width="70%" alt="Wordlist Mode Execution"/>
</p>

<br />

<h3>Step 2: Incremental & Length-Specific Brute Force</h3>
<p>
To target "low-hanging fruit," I used <b>Incremental Mode</b>. Based on the provided statistics (showing 22 passwords with length 1 and 387 with length 3), I restricted the search parameters to specific lengths to maximize efficiency and minimize cracking time.
</p>
<p align="left">
<img src="https://imgur.com/5DPCZuX.png" height="70%" width="70%" alt="Incremental Mode"/>
<br />
<i>Refining the search using password length parameters:</i>
<br />
<img src="https://imgur.com/74vTEVf.png" height="70%" width="70%" alt="Length Parameters"/>
</p>

<br />

<h3>Step 3: Implementing Rule-Based Mutations</h3>
<p>
Simple dictionary attacks often fail against modified passwords. I applied JtR's internal rulesets to mutate the wordlist:
<ul>
<li><b>L33tspeak:</b> Swaps characters for numbers (e.g., 'e' to '3').</li>
<li><b>ShiftToggle:</b> Randomizes casing to catch variations like 'P@ssw0rd'.</li>
</ul>
</p>
<p align="left">
<strong>Executing L33tspeak Rules:</strong><br />
<img src="https://imgur.com/FgoEvMZ.png" height="70%" width="70%" alt="L33tspeak Rules"/>
<br /><br />
<strong>Executing ShiftToggle Rules:</strong><br />
<img src="https://imgur.com/x9VtWRn.png" height="70%" width="70%" alt="ShiftToggle Rules"/>
</p>

<br />

<h3>Step 4: Result Verification & Final Crack Count</h3>
<p>
After completing the various attack phases, I verified the results stored in the <code>john.pot</code> file. This step ensures that the cracked hashes meet the project requirement of 250+ recovered passwords.
</p>
<p align="left">
<img src="https://imgur.com/pu67YAc.png" height="70%" width="70%" alt="Final Crack Count"/>
</p>

<br />

<h2>💡 Key Findings</h2>
<ul>
<li><b>Efficiency:</b> By targeting short password lengths (1-3 chars) first, the majority of the crack count was achieved in under 5 minutes.</li>
<li><b>Rule Superiority:</b> Using <code>--rules</code> significantly increased the yield of the <code>lower.lst</code> wordlist without requiring a larger, more resource-intensive file.</li>
<li><b>Entropy:</b> The distribution of passwords suggests that even simple mutations can thwart basic automated attacks, highlighting the need for higher password complexity.</li>
</ul>
