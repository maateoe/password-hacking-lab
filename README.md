<h1>Password Cracking & Hash Analysis Project</h1>

<p>
This project demonstrates a systematic, multi-vector attack against a leaked database of 1,000 SHA-1 password hashes. Moving beyond "script-kiddie" brute forcing, this lab focuses on <b>statistical profiling</b> and <b>rule-based mutation</b> to maximize cracking efficiency.
</p>

<h2>Utilities Used</h2>
<ul>
<li><b>John the Ripper (JtR) Jumbo Version:</b> Advanced password recovery suite.</li>
<li><b>SHA-1 Hashing Algorithm:</b> The target cryptographic hash function.</li>
<li><b>Python/Bash Scripting:</b> Used for dataset analysis and environment prep.</li>
<li><b>Custom Rule Syntax:</b> For targeted pattern matching.</li>
</ul>

<h2>Attack Methodology & Pre-Computation</h2>
<p>
Before launching the attack, a statistical analysis of <code>cp_leak.txt</code> was performed. Understanding the distribution of password lengths allows for a <b>"Shortest-First"</b> strategy, which yields the highest success rate in the least amount of time.
</p>

<table border="1">
<tr>
<th>Length (# chars)</th>
<th>Count (# passwords)</th>
<th>Attack Priority</th>
</tr>
<tr>
<td>1-3 Characters</td>
<td>409</td>
<td><b>Critical (Instant)</b></td>
</tr>
<tr>
<td>4-6 Characters</td>
<td>239</td>
<td>High (Minutes)</td>
</tr>
<tr>
<td>7+ Characters</td>
<td>352</td>
<td>Medium (Varies)</td>
</tr>
</table>

<br />

<h2>Lab Walk-through</h2>

<h3>Step 1: Environmental Preparation & Permissions</h3>
<p>
Security tools often require specific user permissions to interact with sensitive files. I began by reclaiming ownership of the leak file to ensure JtR could read the hashes without permission errors.
</p>
<p align="left">
<img src="https://imgur.com/5JJx4Av.png" height="70%" width="70%" alt="Permission Config"/>
</p>

<br />

<h3>Step 2: Incremental Optimization (Targeting Lengths 1-3)</h3>
<p>
Utilizing the <b>Incremental ASCII</b> mode, I targeted the 40% of the dataset that consisted of 3 characters or less. This "low-hanging fruit" phase provides immediate results and clears the .pot file for more complex attacks.
</p>
<p align="left">
<img src="https://imgur.com/74vTEVf.png" height="70%" width="70%" alt="Incremental Length Attack"/>
</p>

<br />

<h3>Step 3: Advanced Masking (Bypassing Outdated Documentation)</h3>
<p>
A key challenge in this lab was the failure of standard documentation syntax (e.g., <code>?v</code> for vowels). I successfully implemented a <b>Regex-based Mask Attack</b> using <code>[aeiou]</code> to target English-language patterns.
</p>
<p align="left">
<code>john --mask='[aeiou]?l?l?l' cp_leak.txt</code>
<br />
<br />
<img src="https://imgur.com/FgoEvMZ.png" height="70%" width="70%" alt="Mask Attack Syntax"/>
</p>

<br />

<h3>Step 4: Rule-Based Mutation (L33tspeak & ShiftToggle)</h3>
<p>
To defeat users who use "standard" passwords with minor variations, I utilized the <code>john.conf</code> rulesets. This mutates the <code>lower.lst</code> wordlist entries in real-time.
</p>
<ul>
<li><b>L33tspeak Rule:</b> Automatically swaps 'A' for '4' and 'E' for '3'.</li>
<li><b>ShiftToggle Rule:</b> Tests every case combination (e.g., 'admin', 'Admin', 'aDmin').</li>
</ul>
<p align="left">
<img src="https://imgur.com/x9VtWRn.png" height="70%" width="70%" alt="Rule Mutations"/>
</p>

<br />

<h3>Step 5: Audit & Result Verification</h3>
<p>
The final audit confirms the recovery of plain-text credentials.
</p>
<p align="left">
<strong>Total Passwords Cracked:</strong> 250+ (Requirement Met)
<br />
<img src="https://imgur.com/pu67YAc.png" height="70%" width="70%" alt="Final Result Audit"/>
</p>

<br />

<h2>🛡️ Security Recommendations (Remediation)</h2>
<p>
Based on the success of this attack, the following remediations are recommended:
</p>
<ol>
<li><b>Deprecate SHA-1:</b> SHA-1 is cryptographically broken and susceptible to collision attacks. Migrate to <b>Argon2</b> or <b>bcrypt</b>.</li>
<li><b>Implement Salting:</b> All hashes in this lab were unsalted, allowing for identical passwords to have identical hashes. Salting would prevent the use of pre-computed tables.</li>
<li><b>Minimum Length Policy:</b> Over 40% of passwords were under 4 characters. Enforcing a 12-character minimum would exponentially increase the "time-to-crack."</li>
</ol>
