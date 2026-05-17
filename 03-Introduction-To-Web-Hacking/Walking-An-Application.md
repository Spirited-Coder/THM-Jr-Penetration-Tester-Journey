# Walking An Application
## Objective: To learn how to manually review a web application for security issues using only browsers developer tools.

## Tools Used: View Page Source, Inspector, Debugger, Network

## Methodology:
### A. Exploring The Website: 
Manually exploring the website can help us map the whole web application and discover features that could potentially be vulnerable. Exploring the website can be as simple as spotting a login page to manually reviewing the javascript files.

### B. Viewing the Page Source:
* **Process:** I inspected the raw HTML and CSS code which was sent by the server to the browser.
* **Targets:** 
	* **HTML Comments (!-- --):** I scanned for notes left by developers in the raw source file which can sometimes contain sensitive information like default credentials or hidden directories.
	* **Hidden Inputs (`<input type="hidden"..`>):** I checked for hidden input field present in the source code. Knowing the functionality of these hidden field can help attackers in tampering parameters and get unauthorized access to sensitive data.

### C. Developer Tools - Inspector:
* **Process:** Unlike the raw source code, the inspector tab helps me to see the live, dynamically updated Document Object Model(DOM) after the javascript has been executed.
* **Targets:** I used the live feature of the inspector tab to uncover hidden items (elements with properties like: `style="display: none;"` or `visibility: hidden;`).]

### D. Developer Tools - Debugger:
* **Process:** This panel in the Developer Tools is used for debugging JavaScript. I extracted the JavaScript files and used the debugger to audit them.
* **Targets:** I used the debugger to analyse how the frontend works, inspect and modify javascript files and bypass client-side validations.

### E. Developer Tools - Network:
* **Process:** The network tab is used to capture and display all the activity between the server and the browser
* **Targets:** I used the network tab to analyze all the requests and responses between the browser and the servers and detecting sensitive data leaks and security misconfigurations.

## Exploitation:
### A. Robots.txt
* **Process:** I navigated directly to `http://<target_ip>/robots.txt`.
* **Purpose:** It is a plain text file placed in the root of a website that instructs web crawlers (like Googlebot) which pages or directories should not be indexed.
* **Security Risks:** I mapped out disallowed paths, which routinely expose sensitive interfaces like `/admin`, `/backup`, `/dev-login`, or internal configuration portals.

### B. Sitemap.xml
* **Process:** I navigated directly to `http://<target_ip>/sitemap.xml`.
* **Purpose:** It is a structured XML file (usually at https://example.com/sitemap.xml) that lists all or important URLs of a website to help search engines crawl it efficiently.
* **Security Risks:** Reviewing this file manually often exposes old, legacy, or unlinked endpoints that the development team forgot to delete but left active on the live production server.

## Remediation:
To secure a web application against manual client-side walking, organizations must adopt a "zero trust in the client" architecture:
1. **Strip Comments in Production:** Build pipelines must automatically minify code and strip out all developer comments (`<!-- ... -->`) and console logging statements (`console.log()`) before deploying code to production.
2. **Move Validation to the Backend:** Never rely on frontend JavaScript, hidden fields, or CSS hidden elements to enforce security boundaries. All authorization checks must happen securely on the server-side.
3. **Secure Sensitive Configurations:** Never store sensitive paths inside `robots.txt` if they require absolute secrecy; instead, enforce strong, server-side authentication controls (like IP whitelisting or strict login forms) on those administrative endpoints.
