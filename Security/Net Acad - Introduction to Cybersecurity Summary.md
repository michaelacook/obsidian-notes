# Introduction to Cybersecurity – Notes by Section

## Module 1 – The World of Cybersecurity

### 1.1 The World of Cybersecurity

- **Definition & scope** – Cybersecurity is a continuous effort to protect individuals, organizations and governments from digital attacks by safeguarding networked systems and data from unauthorized use or harm.  
- **Responsibilities at different levels** –
  - **Personal** – protect your identity, data and devices by limiting the information you share and by using safe practices.  
  - **Organizational** – everyone must safeguard company reputation, data and customers; security involves policies, awareness and technical controls.  
  - **Government/National** – national security and economic stability rely on protecting infrastructure and data.
- **Offline vs. online identity** –
  - **Offline identity** is your real‑life persona (name, address, credentials) and must be protected to prevent impersonation.  
  - **Online identity** comprises usernames and your digital footprint; avoid sharing personal details (full name, address, email) when creating usernames and choose appropriate names that do not hint at passwords.  
  - Personal data examples include name, social security number, driver’s‑license number and date of birth; cybercriminals can use these to impersonate you.  
- **Education, employment and financial data** – Academic records and employment/financial records contain sensitive details such as contact information, tax returns and payroll; if not safeguarded these can be abused.  
- **Oversharing risk** – Photos and updates shared online can circulate globally; even private photos may be stored on servers worldwide.
- **Smart devices and advertising** – IoT devices and fitness trackers collect personal and health data; companies use this information for targeted advertising, often trading privacy for convenience.
- **Cybercriminal motives** – Most attackers seek financial gain; they may impersonate family to request money or steal frequent‑flyer miles and login credentials.
- **Identity theft examples** – Medical identity theft and banking fraud highlight the importance of safeguarding personal information.
- **Data collectors** – ISPs, advertisers and search engines log your online activities; this data is valuable and often sold for targeted marketing.

### 1.2 Organizational Data

- **Traditional organizational data** includes:
  - **Transactional data** – transactions such as buying, selling and employment.  
  - **Intellectual property** – patents, trade secrets and proprietary information.  
  - **Financial data** – income statements, balance sheets and banking details.  
- **IoT & big data** – IoT devices collect and share data at scale, creating massive data sets; big data is now an industry focused on analyzing this information.  
- **McCumber Cube (The Cube)** – a three‑dimensional model for evaluating information security:  
  - **Foundational principles** – confidentiality, integrity and availability.  
  - **States of information** – processing, storage and transmission.  
  - **Security measures** – awareness/training, technology and policy/procedure.  
- **Phishing indicators** – suspicious sender addresses, incorrect domains, malicious links, poor grammar and low‑quality graphics.  
- **Case studies** – Persirai botnet exploited IP cameras to perform distributed denial‑of‑service (DDoS) attacks; Equifax breach exploited a vulnerability exposing sensitive data and led to phishing campaigns.
- **Consequences of security breaches** – damage to reputation and brand, online vandalism, theft of data and intellectual property, financial loss and legal liabilities; security breaches are inevitable, so organizations must plan to minimize impact.

### 1.4 Cyber Attackers

- **Types of attackers**:
  - **Script kiddies** – amateurs using existing tools to hack.  
  - **Hackers** – classified into **white hat** (ethical, find and fix vulnerabilities), **black hat** (malicious, exploit vulnerabilities for gain) and **gray hat** (operate between legal and illegal).  
  - **Organized attackers** – criminal organizations, hacktivists, terrorists and state‑sponsored groups.
- **Internal vs. external threats** – Internal threats arise from employees or partners mishandling data, connecting infected devices or misusing privileges. External threats come from outsiders exploiting network vulnerabilities or using social engineering.
- **Insider threats** – Example: a former employee retained access and stole data; insider threats can cause severe damage.

### 1.5 Cyberwarfare

- **Definition** – The use of technology to penetrate another nation’s computer systems to cause damage or disruption.  
- **Stuxnet example** – A worm distributed via USB that targeted industrial control systems; it used zero‑day exploits, modular code and targeted specific programmable logic controllers (PLCs) to sabotage operations rather than steal data.  
- **Goals of cyberwarfare** – Steal defense secrets, disrupt critical infrastructure (such as power grids), and destabilize nations; requires dedicated cybersecurity professionals to protect citizens and infrastructure.

## Module 2 – Attacks, Concepts and Techniques

### 2.1 Analyzing a Cyber Attack

- **Malware types**:
  - **Spyware** – tracks user activity and logs keystrokes.  
  - **Adware** – displays unwanted advertisements or pop‑ups.  
  - **Backdoor** – bypasses authentication to access systems or files.  
  - **Ransomware** – encrypts data and demands payment for decryption.  
  - **Scareware** – uses scare tactics to trick users into installing malware.  
  - **Rootkits** – modify the operating system to create backdoors and hide malicious processes.  
  - **Viruses** – self‑replicating programs that attach to executable files and require user interaction to spread.  
  - **Trojan horses** – disguise malicious code as legitimate software.  
  - **Worms** – self‑replicating programs that spread through networks without user intervention.  
- **Indicators of infection** – high CPU usage, freezing/crashing, slow browsing, unknown files/processes, modified files and unsolicited emails sent from your account.

### 2.2 Methods of Infiltration

- **Social engineering** – manipulation to elicit confidential information. Techniques include:
  - **Pretexting** – creating a fabricated scenario to trick a victim into providing information.  
  - **Tailgating** – following an authorized person into a secured area without authentication.  
  - **Quid pro quo** – offering something in exchange for information.
- **Denial-of-Service (DoS)** – overloads a network or sends malformed packets to crash a system.
- **Distributed DoS (DDoS)** – uses a botnet of infected hosts (zombies) controlled by a handler to launch large‑scale attacks.
- **Botnets** – networks of compromised devices used to spread malware or conduct DDoS; firewalls and filtering help block malicious traffic.
- **On‑path attacks (MitM/MitMo)** – attackers intercept communications between devices; MitMo targets mobile devices and can capture two‑factor authentication codes.
- **SEO poisoning** – manipulates search engine results to direct users to malicious sites.
- **Wi‑Fi password cracking** – attackers may use brute force, network sniffing or social engineering to obtain wireless passwords.
- **Password attacks** – password spraying (using common passwords across many accounts), dictionary attacks (common words), brute‑force (all combinations), rainbow table attacks (pre‑computed hashes) and traffic interception; storing passwords hashed and encrypted mitigates risk.
- **Advanced persistent threats (APTs)** – long‑term, sophisticated intrusions by well‑funded actors aiming to infiltrate and remain undetected.

### 2.3 Security Vulnerability and Exploits

- **Vulnerability & exploit** – A vulnerability is a defect in software or hardware; an exploit is the program or code that takes advantage of that defect.
- **Hardware vulnerabilities** – Issues like **Rowhammer** (memory manipulation by repeatedly accessing memory rows), and CPU flaws **Meltdown** and **Spectre**, allow attackers to read privileged memory; good physical security and malware protection reduce risk.
- **Software vulnerabilities** – include:
  - **Buffer overflow** – writing outside a memory buffer causing crashes or code execution.  
  - **Non‑validated input** – improperly sanitized input leading to allocation or injection issues.  
  - **Race conditions** – out‑of‑order events causing unexpected behavior.  
  - **Weak security practices** – poorly designed or implemented security algorithms.  
  - **Access control problems** – improperly managed permissions allow unauthorized access.  
- **Mitigation** – Apply software updates, patches and configuration changes; companies use vulnerability research and penetration testing (e.g., Google’s Project Zero) to discover and fix flaws.

## Module 3 – Protecting Your Data and Privacy

### 3.1 Protecting Your Devices and Network

- **Securing new devices** –
  - Turn on the firewall and enable antivirus/antispyware.  
  - Regularly update operating system and browser, install patches and set security settings to medium or higher.  
  - Configure password protection and encrypt sensitive files.  
  - For IoT devices, isolate them on a separate network due to infrequent updates.  
- **Wireless network security** – Change default SSID and admin password; enable WPA2 encryption; keep firmware updated; consider wired connections and use a VPN.
- **Public Wi‑Fi** – Use a VPN, disable sharing and Bluetooth, and avoid sensitive transactions on untrusted networks.
- **Password best practices** –
  - Use unique, strong passwords for each account; avoid reuse and common patterns.  
  - NIST guidelines: 8–64 characters, avoid composition rules, allow spaces, no periodic expiration; do not use password hints.  
  - Use passphrases (long memorable sentences) and a password manager.

### 3.2 Data Maintenance

- **Encryption** – Converts information into a secret form so only those with a key can read it; EFS (Windows) can encrypt individual files by enabling encryption in file properties.
- **Backups** – Keep multiple backups: locally (external drives), off‑site (NAS) and cloud; cloud backups protect against device failure and disasters.
- **Data deletion** – Deleting files or emptying the recycle bin only marks data as free; to permanently erase, use secure deletion tools or physically destroy storage; also remove cloud copies.

### 3.3 Who Owns Your Data?

- **Terms of Service (ToS)** – A legally binding contract between you and a provider outlining rules, rights and obligations.
- **Data use & privacy** – Providers collect, use and share your data; many claim the right to host, modify, distribute or create derivative works from the content you upload. Default privacy settings often allow anyone to view your information until you change them.
- **Questions before signing up** – Read ToS carefully; understand what rights you keep; know how to request your data; learn what the provider can do with your data; determine what happens to your data if you close your account.
- **Protecting your data** – Adjust privacy settings; limit what you share; review provider’s security policy; regularly change passwords; use strong passwords and two‑factor authentication.

### 3.4 Safeguarding Your Online Privacy

- **Two‑factor authentication (2FA)** – Popular services like Google, Facebook and Apple use 2FA. Beyond your username and password/PIN, a second token is required: physical object (credit card, phone or fob), biometric scan (fingerprint, facial or voice recognition) or a verification code sent via SMS/email.  
  - Be cautious: 2FA reduces risk but is still susceptible to phishing and malware.
- **Open Authorization (OAuth)** – A protocol allowing you to use existing credentials (e.g., Facebook or Google) to log into third‑party applications without exposing your password.  
- **Social sharing** – Share as little personal information as possible; adjust privacy settings so only known contacts can see your activity; oversharing makes it easier for attackers to profile and exploit you.
- **Spoofing & internet monitoring** – Forged emails can cause data breaches. Anyone with physical access to your device or router can view your browsing history and emails. Use caution when clicking links and verify senders.
- **Email & browser privacy** –
  - Enable private browsing modes (InPrivate, Incognito, Private Tab, etc.). Private browsing disables cookies, removes temporary files and clears history when closed.  
  - Private browsing reduces tracking but does not prevent all forms of fingerprinting; routers and intermediaries can still log your activity. Always follow good security practices when emailing or browsing.

## Module 4 – Protecting the Organization

### 4.1 Cybersecurity Devices and Technologies

- **Security appliances** – Tools used to protect an organization’s network. Categories include routers (interconnect network segments and perform basic filtering), firewalls (deep packet inspection, security policies), intrusion prevention systems (IPS), virtual private networks (VPNs), antimalware/antivirus systems and other devices like web/email security appliances and decryption devices.
- **Router vs. firewall vs. VPN vs. antimalware** – Integrated services routers (ISRs) perform routing, filtering, IPS, encryption and VPN tunneling; next‑generation firewalls (e.g., Cisco Firepower) add advanced analytics; VPN clients (Cisco AnyConnect) provide secure tunnels for remote workers; Advanced Malware Protection (AMP) monitors files and detects malicious behavior.
- **Firewalls** – Control communications allowed into or out of a device/network. Types include:
  - **Network layer firewalls** – filter by source/destination IP addresses.  
  - **Transport layer firewalls** – filter by port numbers and connection states.  
  - **Application layer firewalls** – filter by application or service.  
  - **Context‑aware firewalls** – use user, device, role and threat context to filter traffic.  
  - **Proxy servers** – filter web content requests such as URLs and domain names.  
  - **Reverse proxy servers** – placed in front of web servers to protect and load‑balance requests.  
  - **NAT firewalls** – hide internal IP addresses.  
  - **Host‑based firewalls** – filter ports and system service calls on a single host.
- **Port scanning** – Examines open ports on a device to identify running services. Tools like Nmap reveal which ports are open, closed or filtered. An “open” state means the service is accessible and can be exploited; port scanning should only be done with permission.
- **Intrusion Detection vs. Intrusion Prevention Systems** –
  - **IDS** monitors network traffic for signatures; logs and alerts administrators but does not block traffic. It is often placed offline to reduce latency.  
  - **IPS** actively blocks or denies traffic based on a positive match; Snort/Sourcefire is an example.  
  - **Real‑time detection** – Active scanning (firewall and IDS/IPS) and anomaly detection are required to detect attacks (like DDoS) in real time.
- **Protecting against malware** – Advanced malware detection solutions (e.g., Cisco’s AMP Threat Grid) analyze and correlate millions of files to identify APTs. They support incident response and threat intelligence teams by providing actionable data.
- **Security best practices** – According to NIST and professional guidelines, organizations should:
  - Perform risk assessments.  
  - Create and enforce security policies.  
  - Enforce physical security (restrict access to network closets and server rooms).  
  - Conduct background checks for employees.  
  - Perform and test backups regularly.  
  - Maintain patches and updates on all devices.  
  - Employ access controls and strong user authentication.  
  - Test incident response plans.  
  - Implement network monitoring and analytics.  
  - Deploy next‑generation routers, firewalls and endpoint security solutions.  
  - Train employees and encrypt all sensitive data.

### 4.2 Behavior Approach to Cybersecurity

- **Behavior‑based security** – A threat detection approach that analyzes normal patterns of user and network behavior; anomalies suggest potential attacks.  
  - **Honeypots** lure attackers into controlled environments to analyze their behavior and strengthen defenses.  
  - **Cisco’s Cyber Threat Defense** uses behavior indicators and multiple security technologies to provide visibility and context in attacks.
- **NetFlow** – A technology that collects flow data from switches, routers and firewalls; this data is analyzed to create baseline behaviors and detect anomalies.
- **Penetration testing (pen test)** – A five‑step process to evaluate security:
  1. **Planning/Footprinting** – gather information about the target (reconnaissance).  
  2. **Scanning** – identify open ports and vulnerabilities via port/vulnerability scanning and enumeration.  
  3. **Gaining access** – exploit identified vulnerabilities using malware, social engineering, Wi‑Fi cracking and physical intrusion.  
  4. **Maintaining access** – use backdoors or rootkits to remain undetected while assessing data.  
  5. **Analysis and reporting** – recommend remediation measures and improvements.  
  - Pen tests should be authorized and executed ethically to improve defenses.
- **Impact reduction** – When a breach occurs, organizations should:
  - Communicate the issue internally and externally to create transparency.  
  - Be sincere and accountable; provide details about what happened and what data was compromised.  
  - Investigate the cause (often by hiring forensic experts).  
  - Apply lessons learned to prevent similar breaches; check for and remove backdoors.  
  - Educate employees, partners and clients to prevent future incidents.
- **Risk management** – A formal process to continuously identify and assess risk, weighing threat impact against the cost of controls. Steps include:
  - **Frame the risk** – identify threats (processes, products, attacks, service failures, reputation damage, legal liabilities, intellectual property loss).  
  - **Assess the risk** – determine severity and prioritize threats using quantitative or qualitative analysis.  
  - **Respond to the risk** – develop a plan to eliminate, mitigate, transfer or accept the risk.  
  - **Monitor the risk** – continuously review and monitor accepted or reduced risks.  
  - Never spend more on controls than the value of the asset being protected.

### 4.3 Cisco’s Approach to Cybersecurity

- **Cisco’s CSIRT** – A Computer Security Incident Response Team providing proactive threat assessment, mitigation planning, incident trend analysis and security architecture review. They collaborate with international organizations like FIRST, NSIE, DSIE and DNS‑OARC. Public CSIRTs (e.g., CERT/SEI) assist organizations in developing incident management capabilities.
- **Security playbook** – A collection of repeatable queries and processes for incident detection and response. It should describe how to identify and automate responses to common threats, define inbound/outbound traffic, provide summaries (trends, statistics) and correlate events across data sources.
- **Tools for incident detection and prevention** –
  - **SIEM (Security Information and Event Management)** – collects and analyzes security alerts, logs and data from devices to detect attacks.  
  - **DLP (Data Loss Prevention)** – monitors and protects data in use, in motion and at rest to prevent sensitive information from leaving the network.  
  - **ISE and TrustSec** – Cisco solutions enforcing role‑based access control policies.
- **Key differences** – An **IPS** blocks or denies traffic when signatures match; an **IDS** logs detections; a **DLP** stops data exfiltration; a **SIEM** collects and analyzes log data.

## Module 5 – Will Your Future Be in Cybersecurity?

### 5.1 Legal and Ethical Issues

- **Legal issues** – Cybersecurity professionals possess skills similar to attackers but must operate within the law. Personal hacking (e.g., accessing someone’s computer) can be traced and lead to prosecution.  
  - Businesses must comply with national cybersecurity laws; employees breaking laws may expose companies to liability and personal penalties.  
  - When unsure if an action is legal, assume it is illegal and consult legal or HR staff.  
  - International cybersecurity law is evolving; cyberspace lacks geographic boundaries, and attributing attacks is difficult.
- **Ethical issues** – Use a decision framework: Is the action legal? Does it comply with company policy? Is it favorable for the organization and stakeholders? Would it be acceptable if everyone did it? Does it reflect positively in public? Answer “Yes” to all before acting.  
  - Withholding information (e.g., hiding a colleague’s involvement in a breach) may not be illegal but is unethical and could harm the organization; always report findings and seek guidance.
- **Corporate ethical codes** – Professional organizations like the Information Systems Security Association (ISSA) publish codes of ethics; Cisco provides a Code of Business Conduct and an ethics team to guide decisions.

### 5.2 Education and Careers

- **Become a cybersecurity guru** – There are many roles beyond entry‑level security analyst; explore job options based on interests and skills.
- **Professional certifications** – Certifications validate skills and can advance a career:
  - **MTA Security Fundamentals** – targeted at students and career changers.  
  - **Palo Alto Networks Certified Cybersecurity Associate** – entry‑level certificate.  
  - **ISACA CSX Cybersecurity Fundamentals** – for recent graduates and career changers; does not expire.  
  - **CompTIA Security+** – entry‑level certification meeting U.S. government requirements.  
  - **EC‑Council Certified Ethical Hacker (CEH)** – tests knowledge of exploiting vulnerabilities lawfully.  
  - **ISC2 CISSP** – widely recognized certification requiring at least five years of industry experience.  
  - **Cisco Certified CyberOps Associate** – validates skills for security operations center analysts.
- **Career pathways** – Tools like CyberSeek provide data on supply and demand in the cybersecurity job market, salaries and required skills.

