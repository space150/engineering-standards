[back to table of contents](readme.md)

(*this document is part of the space150 Engineering Standards & Guidelines*)

# Secure Coding

## Overview

Security must be at the forefont of every project we develop at space150. Whether the application is processing financial transactions or simply displaying some marketing copy, security should be treated with equal thoughtfulness and priority. Our client's reputations are on the line with every project we deploy. 

The following section outlines our minimum standards for secure coding practices. Beyond the guidelines here, our clients may have additional specific security gates and reviews that will be considered in addition (not a replacement) to our base standards. Our goals for secure coding practices align with OWASP's software security principles: *The goal of software security is to maintain the confidentiality, integrity, and availability of information resources in order to enable successful business operations.*

## Steps for Secure Coding and Peer Reviews

- **Keep credentials secure** Check that environment variables, addresses and configuration settings that would expose a security vector are not exposed directly in the code repository. Credentials should only be stored in a Bitwarden vault or suite of our client's choosing.

- **Clean up debug messages and comments** During development it is useful to write messages to the console for debugging and testing purposes. Scan your codebase prior to merging into `main` and remove instances of these tests.

- **Ensure error handling is setup correctly** It is very important that our applications handle errors properly. Depending on the language you may have to do more or less work to catch error types and throw a proper response.
    ### **Dos and Dont's**
    - Do log useful information on the server for troubleshooting errors in the future. Ensure that the information logged does not pose a security risk
    - Do throw 500 exceptions when an error occurs, and ensure your application has a 500 application error handling page configured
    - Don't `console.log` to log potentially insecure information in a production system

- **Verify TLS enforcement** For any web applications these days it is important to enforce `https` for all requests and ensure that non-secure requests are handled appropriately.

- **Scan 3rd party libraries for vulnerabilities** We often rely on a lot of node packages in our work. Check your project to ensure the dependencies do not have any security vulnerabilities when you select and install packages.

- Review the [OWASP Secure Coding Practices checklist](https://owasp.org/www-pdf-archive/OWASP_SCP_Quick_Reference_Guide_v2.pdf) for applicable items to the application you are developing and ensure you understand how the application is protecting against the potential vulnerability.

    **Example**: Regarding input validation, many frameworks we build on top of do not allow injection to allow an attacker to retrieve data by injecting a query into an input. Ensure that you know how it is protected and review your project for any vulnerability. 

- **Utilize Pull Request and Peer Reviews** Sole development on a project can be one of the biggest security vectors as we can become blind to our own issues. Always request a peer review of your project and updates to projects.

- **Keep your firewalls active, and antivirus up-to-date** It's a scary world out there; it's not enough to rely on attackers ignoring your IP address by chance. They will find you; it is only a matter of time.