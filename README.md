# SOC-SIMULATION
<p>SOC Automation is the process of automating security monitoring, detection, investigation, and response workflows using Security Orchestration, Automation, and Response (SOAR) platforms, SIEM tools, and machine learning techniques.</p>
<b>Key Objectives:</b>
<ul>
<li>Reduce response time (MTTR) — Quickly detect and mitigate threats.</li>
<li>Improve accuracy — Reduce false positives through automated correlation.</li>
<li>Increase efficiency — Handle large volumes of alerts without analyst burnout.</li>
<li>Standardize responses — Ensure consistent and policy-compliant actions.</li>
</ul>

<!-- START OF INTRODUCTION -->
<h2>INTRODUCTION</h2>
<ul><li><b>Project Title:</b> Automated Security Operations Center (SOC) Workflow using Wazuh, Shuffle, and TheHive</li>
<li><b>Context:</b> This project demonstrates the implementation of a full-cycle automated incident response workflow, integrating leading open-source security tools. This design fulfills the requirements for a modern, efficient Security Operations Center (SOC).</li>
<li><b>Primary Goal:</b> To reduce the Mean Time To Respond (MTTR) to security incidents by leveraging Security Orchestration, Automation, and Response (SOAR) capabilities. This allows for rapid alert triage, enrichment, and initial containment, freeing human analysts to focus on complex investigation and threat hunting.</li></ul>
<!-- END OF INTRODUCTION -->

<!-- START OF SYSTEM ARCHITECTURE OVERVIEW -->
<h2>SYSTEM ARCHITECTURE OVERVIEW</h2>
<p>The system is a classic three-tiered architecture consisting of Detection, Orchestration, and Case Management components, all linked via the Internet, as designed in the Figure 1.</p>
<table>
  <th>Component</th>
  <th>Role</th>
  <th>Specific Tool</th>
  <tr>
    <td>Detection & Monitoring (SIEM/HIDS)</td>
    <td>Collects endpoint events, analyzes logs, and generates alerts based on rules.</td>
    <td>Wazuh Manager</td>
  </tr>
  <tr>
    <td>Orchestration & Automation (SOAR)</td>
    <td>Acts as the central "brain," ingesting alerts and coordinating automated actions across tools.</td>
    <td>Shuffle</td>
  </tr>
  <tr>
    <td>Incident & Case Management (SIRP)</td>
    <td>Centralizes alerts, facilitates human analyst investigation, and documents the incident lifecycle.</td>
    <td>TheHive</td>
  </tr>
  <tr>
    <td>Endpoint</td>
    <td>Generates events and executes response actions.</td>
    <td>Windows 10 Client (Wazuh Agent)</td>
  </tr>
</table>
<!-- END OF SYSTEM ARCHITECTURE OVERVIEW -->

<!-- START OF DETAILED WORKFLOW DESCRIPTION (8 STEPS) -->
<h2>DETAILED WORKFLOW DESCRIPTION (8 STEPS)</h2>
<div align="center">
  <img width="782" height="740" alt="image"  src="https://github.com/user-attachments/assets/5e979b21-28ee-4c83-8ca9-6321c02dc9d4" />
</div>
<div> 
  <br>The following describes the logical flow of a high-severity security alert through the automated system, corresponding to the numbered steps in the diagram:
<ol> 
   <br>
  <li><b>Event Collection and Ingestion (1. Send Events & 2. Receive Events):</b></li>
    <ul>
      <li>The Wazuh Agent installed on the Windows 10 Client monitors system activity (logs, file integrity, process execution) and forwards these security events over the network (via the Router and Internet) to the Wazuh Manager.</li>
    </ul>
 <br>
  <li><b>Alert Generation and Forwarding (3. Send Alerts):</b></li>
    <ul>
      <li>The Wazuh Manager processes the incoming events. When an event matches a high-severity rule (e.g., a known malicious command is executed), it triggers a security Alert.</li>
      <li>This alert is immediately sent to Shuffle, which acts as the SOAR platform, often via a Webhook.</li>
    </ul>
 <br>
  <li><b>Alert Enrichment (4. Enrich IOC's):</b></li>
    <ul>
      <li>Shuffle ingests the raw Wazuh alert and triggers an automation playbook.</li>
      <li>The playbook extracts Indicators of Compromise (IOCs), such as file hashes or IP addresses, and queries external Threat Intelligence platforms (e.g., VirusTotal, AbuseIPDB) via the Internet to enrich the alert data with reputation scores and context.</li>
    </ul>
 <br>
  <li><b>Case Creation and Notification (5. Send Alerts & 6. Send Emails):</b></li>
    <ul>
      <li>Based on the enriched data, Shuffle automatically creates a new Alert (which often converts into a Case) in TheHive, providing the human SOC Analyst with all collected evidence in one central location.</li>
      <li>Concurrently, Shuffle sends an email notification to the SOC Analyst containing a summary of the incident and a link to the new case in TheHive.</li>
    </ul>
 <br>
  <li><b>Human Triage and Response Decision (7. Send & Receive Emails):</b></li>
    <ul>
      <li>The SOC Analyst reviews the enriched case in TheHive and the email. Crucially, the email often contains a call to action, such as an embedded button or link that allows the analyst to select a Response Action (e.g., "Isolate Host," "Close False Positive").</li>
      <li>The analyst sends this Response Action back to Shuffle via the Internet/Email interface.</li>
    </ul>
 <br>
  <li><b>Action Orchestration and Execution (8. Send Response Action & 8. Perform Response Action):</b></li>
    <ul>
      <li>Shuffle receives the analyst's chosen response action.</li>
      <li>It translates this high-level action into a specific command and sends a Response Action instruction to the Wazuh Manager.</li>
      <li>The Wazuh Manager then uses its Active Response capability to command the Wazuh Agent on the Windows 10 Client to Perform the Response Action (e.g., blocking an IP in the firewall, terminating a process, or isolating the host from the network).</li>
    </ul>
</ol>
</div>
<!-- END OF DETAILED WORKFLOW DESCRIPTION (8 STEPS) -->

<h2>FLOWCHART</h2>
<div align = "center">
<img width="966" height="462" alt="image" src="https://github.com/user-attachments/assets/91895aeb-8c09-4e3b-bd6e-14ac84a80a92" />
</div>
