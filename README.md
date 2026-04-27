<h1>Password Cracking & Hash Analysis Project</h1>

<p>
This project demonstrates a systematic, multi-vector attack against a leaked database of 1,000 SHA-1 password hashes. Moving beyond standard brute-forcing, this lab focuses on <b>statistical profiling</b> and <b>rule-based mutation</b> to maximize cracking efficiency.
</p>

<h2>Utilities Used</h2>
<ul>
<li><b>John the Ripper (JtR) Jumbo Version:</b> Advanced password recovery suite.</li>
<li><b>SHA-1 Hashing Algorithm:</b> The target cryptographic hash function.</li>
<li><b>RockYou Wordlist:</b> Industry-standard dictionary file for credential recovery.</li>
<li><b>Kali Linux:</b> Security-focused OS for execution and environment management.</li>
</ul>

<h2>Attack Methodology & Statistics</h2>
<p>
Before launching the attack, I analyzed the distribution of the <code>cp_leak.txt</code> dataset. Understanding password length distribution allows for a <b>"Shortest-First"</b> strategy, which yields the highest success rate in the least amount of time.
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
<td><b>Critical (Instant Recovery)</b></td>
</tr>
<tr>
<td>4-6 Characters</td>
<td>239</td>
<td>High (Minutes)</td>
</tr>
<tr>
<td>7+ Characters</td>
<td>352</td>
<td>Medium (Extended Brute-Force)</td>
</tr>
</table>

<br />

<h2>Lab Walk-through</h2>

<h3>Step 1: Incremental Length Attack (1-8 Characters)</h3>
<p>
Using the <code>--incremental</code> mode, I targeted the "low-hanging fruit" identified in my statistical analysis. By restricting the <code>min-length</code> and <code>max-length</code>, I recovered the first batch of credentials in seconds.
</p>
<p align="left">
<img src="https://imgur.com/5DPCZuX.png" height="70%" width="70%" alt="Incremental Attack Setup"/>
<br />
<i>Evidence of successful real-time recovery:</i>
<br />
<img src="https://imgur.com/74vTEVf.png" height="70%" width="70%" alt="Active Cracking Results"/>
</p>

<br />

<h3>Step 2: Dictionary Attack with Rule Mutations</h3>
<p>
To defeat users who use common words with minor variations (l33tspeak or case changes), I utilized JtR rulesets. This mutates the <b>RockYou</b> wordlist in real-time, catching variations like <code>P@ssw0rd</code> or <code>p4ssword</code>.
</p>
<ul>
<li><b>L33tspeak Rule:</b> Swaps characters for numbers (e.g., 'e' to '3').</li>
<li><b>ShiftToggle Rule:</b> Tests every case combination (e.g., 'admin', 'Admin').</li>
</ul>
<p align="left">
<img src="https://imgur.com/x9VtWRn.png" height="70%" width="70%" alt="Rule-Based Mutation Execution"/>
</p>

<br />

<h3>Step 3: Verification of Results</h3>
<p>
After completing the attack vectors, I performed an audit of the <code>john.pot</code> file to verify the total count of recovered plain-text credentials.
</p>
<p align="left">
<strong>Success Metric:</strong> 254/1000 Passwords Cracked.
<br />
<img src="https://imgur.com/pu67YAc.png" height="50%" width="50%" alt="Final Result Count"/>
</p>

<br />

<h2>Security Remediation & Recommendations</h2>
<p>
A security project is not complete without a mitigation plan. Based on the vulnerability of this dataset, I recommend the following:
</p>
<ol>
<li><b>Deprecate SHA-1:</b> This algorithm is cryptographically broken. Migrate to <b>Argon2id</b> or <b>bcrypt</b> which are designed to resist high-speed GPU cracking.</li>
<li><b>Mandatory Salting:</b> The lack of salts in this dataset allowed for identical hashes. Implementing a unique salt per user prevents the use of pre-computed Rainbow Tables.</li>
<li><b>Entropy Enforcement:</b> Over 40% of the dataset was under 4 characters. Security policy should enforce a <b>12-character minimum</b> to exponentially increase the time-to-crack.</li>
</ol>
