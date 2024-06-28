# Agartha - LFI, RCE, SQLi, Auth, HTTP to JS
Agartha, specializes in dynamic payload analysis and access control assessment. It adeptly identifies vulnerabilities related to injection attacks, and authentication/authorization issues. The dynamic payload generator crafts extensive wordlists for various injection vectors, including SQL Injection, Local File Inclusion  (LFI), and Remote Code Execution. Furthermore, Agartha constructs a comprehensive user access matrix, revealing potential access violations and privilege escalation paths. It also assists in performing HTTP 403 bypass checks, shedding light on authorization weaknesses. Additionally, it can convert HTTP requests to JavaScript code to help digging up XSS issues more.

In summary:

- **Payload Generator**: It dynamically constructs comprehensive wordlists for injection attacks, incorporating various encoding and escaping characters to enhance the effectiveness of security testing. These wordlists cover critical vulnerabilities such as SQL Injection, Local File Inclusion (LFI), and Remote Code Execution, making them indispensable for robust security testing.
	- **Local File Inclusion, Path Traversal** helps identifying vulnerabilities that allow attackers to access files on the server's filesystem.
	- **Command Injection, Remote Code Execution** aims to detects potential command injection points, enabling robust testing for code execution vulnerabilities.
	- **SQL Injection** assists to uncover SQL Injection vulnerabilities, including Stacked Queries, Boolean-Based, Union-Based, and Time-Based.
- **Auth Matrix**: By constructing a comprehensive access matrix, the tool reveals potential access violations and privilege escalation paths. This feature enhances security posture by addressing authentication and authorization issues. 
	- You can use web **'Spider'** feature to generate sitemap/URL list. It will populate visible links automatically and the result will totally depend on the user's header.
- **403 Bypass**: It aims to tackle common access restrictions, such as HTTP 403 Forbidden responses. It utilizes techniques like URL manipulation and request header modification to bypass implemented limitations.
- **Copy as JavaScript**: It converts Http requests to JavaScript code for further XSS exploitation and more.<br/><br/>

Here is a small tutorial how to use.

## Installation
You should download 'Jython' file and set your environment first:
- Burp Menu > Extender > Options > Python Environment > Locate Jython standalone jar file (tested in Jython v2.7.3).

You can install Agartha through official store: 
- Burp Menu > Extender > BApp Store > Agartha

Or for manual installation:
- Burp Menu > Extender > Extensions > Add > Extension Type: Python > Extension file(.py): Select 'Agartha.py' file

After all, you will see 'Agartha' tab in the main window and it will be also registered the right click, under: 
- 'Extensions > Agartha - LFI, RCE, SQLi, Auth, HTTP to JS', with three sub-menus:
	- 'Auth Matrix'
 	- '403 Bypass'
	- 'Copy as JavaScript'<br/><br/>


## Local File Inclusion / Path Traversal
It supports both Unix and Windows file syntaxes, enabling dynamic wordlist generation for any desired path. Additionally, it can attempt to bypass Web Application Firewall (WAF) implementations, with various encodings and other techniques.
- **'Depth'** is representation of how deep the wordlist should be. You can generate wordlists 'till' or 'equal to' this value.
- **'Waf Bypass'** asks for if you want to include all bypass features; like null bytes, different encoding, etc.

<img width="1000" alt="Directory Traversal/Local File Inclusion wordlist" src="https://github.com/volkandindar/agartha/assets/50321735/b457e6c2-0829-4959-84aa-9116886b99f7"><br/><br/>



## Command Injection / Remote Code Execution
It generates dynamic wordlists for command execution based on the supplied command. It combines various separators and terminators for both Unix and Windows environments.
- **'URL Encoding'** encodes dictionary output.

<img width="1000" alt="Remote Code Execution wordlist" src="https://github.com/volkandindar/agartha/assets/50321735/d28c12c9-c6fb-4509-9299-888f3f048c12"><br/><br/>

## SQL Injection
It generates payloads for various types of SQL injection attacks, including Stacked Queries, Boolean-Based, Union-Based, and Time-Based. It doesn’t require any user inputs; you simply select the desired SQL attack types and databases, and it generates a wordlist with different combinations.
- **'URL Encoding'** encodes dictionary output.
- **'Waf Bypass'** asks for if you want to include all bypass features; like null bytes, different encoding, etc.
- **'Union-Based'** ask for how deep the payload should be. The default value is 5.
- And the rest is related with databases and attack types.

<img width="1000" alt="SQL Injection wordlist" src="https://github.com/volkandindar/agartha/assets/50321735/51a010b6-4d9a-4dc9-a634-b353f6b30b95"><br/><br/>

## Authorization Matrix / User Access Table
This part focuses on analyzing user session and URL relationships to identify access violations. The tool systematically visits all URLs associated with pre-defined user sessions and populates a table with HTTP responses. Essentially, it creates an access matrix, which aids in identifying authentication and authorization issues. Ultimately, this process reveals which users can access specific page contents.
- You can right click on any request ('Extensions > Agartha > Authorization Matrix') to define **user sessions**.
- Next, you need to provide **URL addresses** the user (Http header/session owner) can visit. You can use internal 'SiteMap' generator feature or supply any manual list. 
- And then, you can use **'Add User'** button to add the user sessions.
- Now, it is ready for execution with only clicking **'Run'** button, and it will fill the table. 

<img width="1000" alt="Authorization Matrix" src="https://github.com/volkandindar/agartha/assets/50321735/6f89e22c-e29c-413d-96d8-c2a8d7ac39d4">


A little bit more details:
1. What's username for the session you provide. You can add up to 4 different users and each user will have a different color to make it more readable.
	- 'Add User' for adding user sessions to matrix.
	- You can change Http request method between 'GET', 'POST' or 'Dynamic', which bases on proxy history.
	- 'Reset' button clear all contents.
	- 'Run' button execute the task and the result will show user access matrix.
	- 'Warnings' indicates possible issues in different colors.
	- 'Spider (SiteMap)' button generates URL list automatically and the result totally depends on the user's header/session. Visible URLs will be populated in next textbox and you can still modify it.
	- 'Crawl Depth' is defination for how many sub-links (max depth) 'SiteMap' spider should go and detect links.
2. It is the field for request headers and all URLs will be visited over the session defined in here.
3. URL addresses that user can visit. You can create this list with manual effort or use **'SiteMap'** generator feature. You need to provide visitable URL lists for each users.
4. All URLs you supply will be in here and they will be visited with the corresponding user sessions.
5. No authentication column. All cookies, tokens and possible session parameters will be removed form Http calls.
6. The rest of columns belong to users you created respectively and each of them has a unique color which indicates the URL owners.  
7. Cell titles show Http 'response codes:response lengths' for each user sessions.
8. Just click the cell you want to examine and Http details will be shown in the bottom.

Please note that potential session terminators (such as logoff, sign-out, etc.) and specific file types (such as CSS, images, JavaScript, etc.) will be filtered out from both the 'Spider' and the user's URL list.

<img width="1000" alt="User Access Table Details" src="https://github.com/volkandindar/agartha/assets/50321735/a48e60d4-d7f1-4c12-ad02-f1989c175730">

After clicking 'RUN', the tool will fill user and URL matrix with different colors. Besides the user colors, you will see orange, yellow and red cells. The URL address does not belong to the user, and if the cell color is:
- **Red**, because the response returns 'HTTP 200' and same content length, with authentication/authorization concerns
- **Orange**, because the response returns 'HTTP 200' but different content length, with authentication/authorization concerns
- **Yellow**, because the response returns 'HTTP 302' with authentication/authorization concerns

The task at hand involves a bulk process, and it is worth to mention which HTTP request methods will be used. The tool provides three different options for performing HTTP calls:
- **GET**, All requests are sent using the GET method.
- **POST**, All requests are sent using the POST method.
- **Dynamic**, The request method depends on the proxy history.If no info exists, the base header method will be default.<br/><br/>

## 403 Bypass
HTTP 403 Forbidden status code indicates that the server understands the request but refuses to authorize it. Essentially, it means, ‘I recognize who you are, but you lack permission to access this resource.’ This status often points to issues like ‘insufficient permissions’, ‘authentication required’, ‘IP restrictions’, etc.

The tool addresses the common access forbidden error by employing various techniques, such as URL manipulation and request header modification. These strategies aim to bypass access restrictions and retrieve the desired content.

It is worth to mention two different usage cases:
1. In scenarios related to **Authentication Issues**, consider removing all session identifiers. Then you can test whether any sources become accessible publicly. This approach helps identify unauthentication accesses and ensures that sensitive information remains protected.
2. For **Privilege Escalation/Authorization** testing, retain session identifiers but limit their use to specific user roles. For instance, you can utilize a regular user’s session while substituting an administrative URL. This controlled approach allows you to assess whether privileged sources are accessible without proper roles.

There are 2 ways you can send HTTP requests to the tool.
1. You can load requests from proxy history by clicking the ‘Load Requests’ button. Doing so will automatically remove all session identifiers, making it suitable for attack **Case 1**. Please note that potential session terminators (such as logoff, sign-out, etc.) and specific file types (such as CSS, images, JavaScript, etc.) will be also filtered out.
2. You can send individual requests by right-clicking. Session identifiers will be retained, making this approach suitable for attack **Case 2**.

<img width="1000" alt="Sending individual requests" src="https://github.com/volkandindar/agartha/assets/50321735/54b567a0-6b69-43f4-b727-f01709f4cc79">

The page we aim to access belongs to a privileged user group, and we retain our session identifiers to verify if Privilege Escalation is feasible.
<br/><br/>
Simply clicking the 'RUN' button will execute the task.

The figure below illustrates that a URL may have an access issue, with the ‘Red’ color indicating a warning.

<img width="1000" alt="Attempt details" src="https://github.com/volkandindar/agartha/assets/50321735/b7c81258-aa11-42dc-87c6-c25b1047056c">

1. Load requests from the proxy history by selecting the target hostname and clicking the ‘Load Requests’ button.
2. URL and Header details
3. Request attempts and results
4. HTTP requests and responses

Note: The number of attempts depends on the specific target URL itself.
<br/><br/>
## Copy as JavaScript
The feature allows for converting HTTP requests to JavaScript code, which can be valuable for digging up further XSS issues and bypassing header restrictions.

To access it, right click any Http request and 'Extensions > Agartha > Copy as JavaScript'.

<img width="1000" alt="Copy as JavaScript" src="https://github.com/volkandindar/agartha/assets/50321735/771fd1db-c2ba-4a32-8b17-ef5cc53fa5bd">

It will automatically save it to your clipboard with some remarks. For example:
```
Http request with minimum header paramaters in JavaScript:
	<script>
		var xhr=new XMLHttpRequest();
		xhr.open('GET','http://dvwa.local/vulnerabilities/xss_r/?name=XSS');
		xhr.withCredentials=true;
		xhr.send();
	</script>

Http request with all header paramaters (except cookies, tokens, etc) in JavaScript, you may need to remove unnecessary fields:
	<script>
		var xhr=new XMLHttpRequest();
		xhr.open('GET','http://dvwa.local/vulnerabilities/xss_r/?name=XSS');
		xhr.withCredentials=true;
		xhr.setRequestHeader('Host',' dvwa.local');
		xhr.setRequestHeader('User-Agent',' Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:127.0) Gecko/20100101 Firefox/127.0');
		xhr.setRequestHeader('Accept',' text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8');
		xhr.setRequestHeader('Accept-Language',' en-US,en;q=0.5');
		xhr.setRequestHeader('Accept-Encoding',' gzip, deflate, br');
		xhr.setRequestHeader('DNT',' 1');
		xhr.setRequestHeader('Sec-GPC',' 1');
		xhr.setRequestHeader('Connection',' keep-alive');
		xhr.setRequestHeader('Referer',' http://dvwa.local/vulnerabilities/xss_r/');
		xhr.setRequestHeader('Upgrade-Insecure-Requests',' 1');
		xhr.setRequestHeader('Priority',' u=1');
		xhr.send();
	</script>

For redirection, please also add this code before '</script>' tag:
	xhr.onreadystatechange=function(){if (this.status===302){var location=this.getResponseHeader('Location');return ajax.call(this,location);}};
```
Please note that the JavaScript code will be executed within the original user session, and many header fields will be automatically populated by browsers. In certain cases, the server may require specific header fields to be mandatory; for instance, some requests might fail with an incorrect ‘Content-Type’. Therefore, you may need to adjust the code accordingly.
<br/><br/>
[Another tutorial link](https://www.linkedin.com/pulse/agartha-lfi-rce-auth-sqli-http-js-volkan-dindar)
