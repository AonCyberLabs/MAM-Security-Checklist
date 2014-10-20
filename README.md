
<html>

<head>
<h3>Mobile Application Management (MAM) Security Checklist</h3>
</head>

<body>
<p>
The following checklist is intended to be used as a baseline for assessing, designing, and testing the security of a MAM solution and is thus aptly suited to assist three major stakeholders in the BYOD space: buyers, builders, and breakers. This list was constructed from our experience and research assessing a variety of MAM solutions in the marketplace today. This research was presented at Blackhat USA 2014 and the slides can be downloaded <a href="https://github.com/GDSSecurity/Presentations/blob/master/Unwrapping%20the%20Truth%20-%20Analysis%20of%20Mobile%20App%20Wrapping/us-14-Gutierrez-Unwrapping%20the%20Truth%20-%20Analysis%20of%20Mobile%20App%20Wrapping.pdf?raw=true">here</a>. This should not be considered an exhaustive list, but rather a compilation of major test cases that can be used to determine the security posture of a MAM solution. As always, contributions/recommendations from the community are welcome in an effort to further expand this checklist. It should be noted that several of the test cases require significant reverse engineering and advanced mobile application testing skills. Therefore, it is recommended that organizations consult skilled security professionals to perform/verify the test cases.
</p>

<table>
  <tr>
    <th colspan="2">Implementation of MAM Secure Container Authentication</th>
  </tr>
  <tr>
    <td>1</td>
    <td>Stored encrypted data should be protected using key material that is only accessible upon successful offline or online authentication</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Online authentication should aim to retrieve the key material from the MAM admin server. This provides the ability to enforce passcode lockout attempts</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Online key material or any Content Encryption Key (CEK) derived from the online key material should never persist on the disk</td>
  </tr>
  <tr>
    <td>4</td>
    <td>If the policy is configured to only allow online authentication, the MAM agents or managed apps should not be susceptible to any offline attacks due to storing offline authentication hashes</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Key material must be deleted from memory upon entering an unauthenticated state (app lock, session timeouts, sign off, etc.)</td>
  </tr>
  <tr>
    <td>6</td>
    <td>Incorrect passcode attempts beyond the configured threshold should result in the deletion of all device key material and stored application data</td>
  </tr>
  <tr>
    <td>7</td>
    <td>Strict incorrect passcode attempts policy should be configured by default</td>
  </tr>
  <tr>
    <td>8</td>
    <td>Offline authentication must utilize a strong key derivation function (e.g. PBKDF2) that is configured with sufficient work factor (e.g. 20,000 minimum) to generate the Key Encryption Key (KEK). The work factor should be increased over time to account for hardware advancements</td>
  </tr>
  <tr>
    <td>9</td>
    <td>Ensure a secure random salt of sufficient length is used by the key derivation function to prevent attacks involving pre-computed keys</td>
  </tr>
  <tr>
    <td>10</td>
    <td>Offline authentication passcode should not be the same password as the employees primary Active Directory or SSO password</td>
  </tr>
  <tr>
    <td>11</td>
    <td>Offline passcode complexity policies should be sufficiently strong in order to mitigate offline passcode brute force attempts (Minimum of five characters with numbers and special characters recommended)</td>
  </tr>
  <tr>
    <td>12</td>
    <td>The key derivation routines used for offline passcode validation and symmetric key derivation should be the same. As in a malicious user should not be able to perform a faster brute force on one routine over the other  </td>
  </tr>
  <tr>
    <td>13</td>
    <td>MAM agent and wrapped applications should have a reasonably short session timeout in order to force the user to re-authenticate</td>
  </tr>
  <tr>
    <td>14</td>
    <td>Validate that the offline/online authentication routines within the wrapped applications match that of the MAM agent</td>
  </tr>
  <tr>
    <td>15</td>
    <td>If MAM Server authentication tokens and/or client certificates must be stored, they should be encrypted using the CEK and made available to the application only after the user successfully enters their passcode</td>
  </tr>
  <tr>
    <td>16</td>
    <td>If application requires online access to function, organization should consider enforcing online authentication policies for the application</td>
  </tr>
  <tr>
    <th colspan="2">Implementation of MAM Secure Container Cryptography</th>
  </tr>
  <tr>
    <td>17</td>
    <td>Identify the secure random generator library and routine used throughout the solution and ensure it is not vulnerable to known issues</td>
  </tr>
  <tr>
    <td>18</td>
    <td>Validate that the cryptography used to protect any sensitive data is utilizing FIPS compliant library (if solution claims they are FIPS compliant)</td>
  </tr>
  <tr>
    <td>19</td>
    <td>The solution should not utilize an insecure mode of encryption</td>
  </tr>
  <tr>
    <td>20</td>
    <td>Solution should utilize secure random IV/nonce of sufficient length when encrypting data</td>
  </tr>
  <tr>
    <td>21</td>
    <td>Ensure encrypted data stored is not susceptible to tampering by performing an HMAC of the ciphertext and validating the value prior to decryption</td>
  </tr>
  <tr>
    <td>22</td>
    <td>Beware of using string objects to hold sensitive binary data (Cryptographic Keys, IVs, Salts) as it could lead to unexpected char-set encoding and a loss of entropy </td>
  </tr>
  <tr>
    <td>23</td>
    <td>The solution should avoid utilizing database encryption solutions such as SQLCipher to encrypt/decrypt application data as it will cause the symmetric key to be stored as a string</td>
  </tr>
  <tr>
    <td>24</td>
    <td>The solution should avoid setting passcode or cryptography keys on the heap as it will cause the key to persist in memory for an extended period of time. The data will only be removed from memory once the garbage collection is performed.</td>
  </tr>
  <tr>
    <td>25</td>
    <td>Verify third party cryptography library is not out of date and vulnerable to any vulnerabilities that apply to the MAM solution</td>
  </tr>
  <tr>
    <th colspan="2">Completeness of MAM Wrapping</th>
  </tr>
  <tr>
    <td>26</td>
    <td>Verify that common APIs that persist data on the file system will be adequately wrapped and encrypted by the solution </td>
  </tr>
  <tr>
    <td>27</td>
    <td>Ensure that the wrapped applications are using the same FIPS compliant library to perform cryptography</td>
  </tr>
  <tr>
    <td>28</td>
    <td>Wrapped application should not be utilizing device level data protection as the primary form of encryption since it requires a passcode being set on the device</td>
  </tr>
  <tr>
    <td>29</td>
    <td>Ensure data stored within Webviews are adequately wrapped and data will be encrypted by the solution (HTML5 storage, Cached Pages, Cookies, etc)</td>
  </tr>
  <tr>
    <td>30</td>
    <td>Phone numbers or email addresses rendered within Webviews (Typically created into hyperlinks) should contain security controls to prevent leakage to unmanaged applications</td>
  </tr>
  <tr>
    <td>31</td>
    <td>Verify that native or low level APIs (NDK, C functions) are also wrapped and data will be encrypted by the solution</td>
  </tr>
  <tr>
    <td>32</td>
    <td>Files opened by a wrapped application should not lead to unencrypted storage of cached documents</td>
  </tr>
  <tr>
    <td>33</td>
    <td>Cached HTTP response data should not be written to the file system unencrypted</td>
  </tr>
  <tr>
    <td>34</td>
    <td>Persistent HTTP cookies should not be written to the file system unencrypted</td>
  </tr>
  <tr>
    <td>35</td>
    <td>Data written to the system pasteboard by a wrapped application should be encrypted</td>
  </tr>
  <tr>
    <td>36</td>
    <td>Data written using cloud based APIs should be encrypted (iCloud, Backup API, etc)</td>
  </tr>
  <tr>
    <td>37</td>
    <td>Snapshot Caching when sending applications to the background should be actively prevented within wrapped applications</td>
  </tr>
  <tr>
    <td>38</td>
    <td>Filenames should be encrypted by the applications</td>
  </tr>
  <tr>
    <td>39</td>
    <td>Files opened by a wrapped applications (Open-in) should remove any plaintext copies made into its application container</td>
  </tr>
  <tr>
    <td>40</td>
    <td>Persistent cookies should not be stored in plaintext within wrapped applications</td>
  </tr>
  <tr>
    <td>41</td>
    <td>Any device level key storage (Keychains) should not be stored within MAM data encrypted since the device level encryption relies on device level passcode policies.</td>
  </tr>
  <tr>
    <td>42</td>
    <td>The wrapping solution should be able to handle language specific introspection/reflection if used on a wrapped API</td>
  </tr>
  <tr>
    <td>43</td>
    <td>The MAM agent should try to encrypt all data except values that would be required prior to authentication.</td>
  </tr>
  <tr>
    <td>44</td>
    <td>Solution should provide restrictions to prevent wrapped apps from sending data to other systems from exposed system APIS (e.g. AirDrop APIs)</td>
  </tr>
  <tr>
    <th colspan="2">Implementation of the MAM Inter-Process Communication</th>
  </tr>
  <tr>
    <td>45</td>
    <td>IPC entry points within the MAM agents and wrapped applications should validate the calling application to ensure it is trusted</td>
  </tr>
  <tr>
    <td>46</td>
    <td>IPC messages should not be susceptible to forgery by third party applications</td>
  </tr>
  <tr>
    <td>47</td>
    <td>Data passed within through IPC between the MAM agent and wrapped applications should not be susceptible to be read by third party applications</td>
  </tr>
  <tr>
    <td>48</td>
    <td>Verify how keys are exchanged or generated within managed/wrapped apps if encryption is performed (e.g. URL Schemes, Content Providers, Intents, etc.). The key exchange process should contain authorization checks to ensure it cannot be invoked or sniffed by third party applications</td>
  </tr>
  <tr>
    <th colspan="2">Effectiveness of Client Side Security Controls</th>
  </tr>
  <tr>
    <td>49</td>
    <td>Security policies sent from the MAM server to the MAM mobile agent should be digitally signed to prevent trivial modification over the network by an employee</td>
  </tr>
  <tr>
    <td>50</td>
    <td>Security policies should not be stored in plaintext on the device to prevent trivial modification by an employee</td>
  </tr>
  <tr>
    <td>51</td>
    <td>Solution should provide restrictions on any IPC calls wrapped apps can make into an unmanaged application</td>
  </tr>
  <tr>
    <td>52</td>
    <td>Solution should incorporate obfuscation of client-side code to increase the difficulty in reverse engineering.</td>
  </tr>
  <tr>
    <td>53</td>
    <td>Jailbreak detection should not be susceptible to trivial bypasses. For example, using the xCon Cydia application or by simply writing a hook for a “isJailbroken” method. It is recommended that jailbreak detection be written in low level code and placed inline across various methods in the application.</td>
  </tr>
  <tr>
    <th colspan="2">Effectiveness of Remote Lock and Wipes</th>
  </tr>
  <tr>
    <td>54</td>
    <td>Ensure key material is wiped on device "lock" states (session timeouts, server locks, excessive invalid pass codes, presented with pin code login screen, etc).</td>
  </tr>
  <tr>
    <td>55</td>
    <td>Server invoked data wipes should cause all data to be removed wipes from the MAM Agent (This includes encrypted documents, encrypted key material, offline passcode validation data, etc)</td>
  </tr>
  <tr>
    <td>56</td>
    <td>Server invoked data wipes should cause all data to be removed wipes from the Wrapped Applications (This includes encrypted documents, encrypted key material, offline passcode validation data, etc)</td>
  </tr>
  <tr>
    <td>57</td>
    <td>Server invoked security commands should be invoked on the device immediately. The user should not need to interact with the application in order to invoke a polling request</td>
  </tr>
</table>

<br>

<h2 align="center">License</h2>
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">MAM Security Checklist</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/GDSSecurity/MAM-Security-Checklist" property="cc:attributionName" rel="cc:attributionURL">Gotham Digital Science</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

</body>
</html>
