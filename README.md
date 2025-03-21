# Python Email Verification Script

This Python-based email verification script is designed to help you 
validate email addresses by analyzing various technical aspects of 
email systems. It performs an in-depth assessment to determine whether 
an email address is likely valid, invalid, or problematic, logging the 
results for future reference. While the tool provides valuable insights 
into email address validity, it's important to note that due to the 
complex and dynamic nature of email systems, it is never 100% accurate. 

This script is a robust internal validation tool and can offer insights 
into the technical setup of email addresses and domains. However, it 
should not be solely relied upon for verifying whether an email address 
is actively in use, especially for critical applications. To ensure 
higher accuracy, it's recommended to use this tool alongside other 
verification methods.

## Addressing Windows Defender False Positive Detection

Windows Defender may flag this Email Verification Script as "Trojan:Script/Wacatac.B!ml." This is a false positive and commonly occurs with Python scripts that perform network-related operations.
Why This Happens

Antivirus software, including Windows Defender, relies on heuristic analysis to detect potential threats. This script includes several features that can trigger false positives:

   - Network connections – The script connects to SMTP servers, DNS servers, and other email-related services.
   - Email processing functionality – Handling email addresses can resemble behaviors seen in spam-related malware.
   - Socket operations – Direct socket connections are used for communication, a technique also seen in various types of software.
   - ultiple server connection attempts – The script tests various email servers, which may resemble scanning behavior to security software.

### Current Status

I am actively investigating this issue to find potential workarounds or solutions. If you encounter any issues, feel free to open an issue or contribute to the discussion.

## Key Features and Functions

1. Email Format Validation:
   The script first checks whether the email address adheres to a 
   standard format (e.g., user@domain.com). Invalid emails are logged 
   with an error message.

2. Domain Checks:
   - MX Records: Verifies whether the domain associated with the email 
     has valid MX (Mail Exchange) records, indicating that it is capable 
     of receiving emails.
   - Disposable Email Detection: Checks if the email comes from a known 
     disposable email provider (e.g., mailinator.com, tempmail.com), 
     typically used for temporary emails.
   - Blacklist Checks: Looks up the domain in a predefined list of 
     blacklisted domains, which could indicate potential spam or 
     malicious activity.

3. SPF and DKIM Validation:
   - SPF (Sender Policy Framework): Checks if the domain has a valid SPF 
     record, helping verify that the email was sent from a trusted server.
   - DKIM (DomainKeys Identified Mail): Checks for DKIM records, which 
     verify the authenticity of the email's sender and prevent email 
     tampering.

4. SMTP Checks:
   - SMTP Connection Test: Attempts to connect to the SMTP server for 
     the domain on multiple ports (25, 587, 465) to see if the email 
     server is responsive and capable of handling the email address.
   - SMTP VRFY Command: Tries to verify whether the email address exists 
     on the mail server. However, many servers have this command disabled, 
     meaning this check may not always be accurate.

5. Catch-All Email Detection:
   - Detects if a domain is configured with a catch-all email address 
     (i.e., it accepts all emails for any address on that domain), using 
     a fake email address.

6. IMAP and POP3 SSL Checks:
   - Tests if the domain's mail server supports secure IMAP and POP3 
     connections on common SSL ports (993 for IMAP, 995 for POP3), 
     providing clues about the server's configuration and security.

7. Logging:
   - All results are stored in a SQLite database with details such as:
     - Timestamp
     - Email address and domain
     - MX record and port used
     - SPF and DKIM status
     - SMTP, SMTP VRFY, and blacklist status
     - IMAP/POP3 availability
     - Additional information (SMTP banner, MX server IP, etc.)
     - Confidence score (0-100)
     - Validation time (execution time in seconds)
   - This database can be accessed and viewed from within the application
     using the 'show log' command.

## What to Expect

1. Accuracy Limitations:
   - No Guarantees: The accuracy of email verification is never 100%. 
     Some mail servers may block commands (like VRFY or RCPT), making it 
     impossible to verify the existence of an email address. Anti-spam 
     measures could also interfere with the process.
   - Catch-All Detection: Even if an email doesn't explicitly exist, it 
     could still be accepted if the domain has a catch-all email configured.
   - Blacklist Information: The blacklist check relies on a predefined 
     list and may not catch all blacklisted domains. For critical 
     applications, consider using a more comprehensive and up-to-date 
     blacklist database.

2. Dynamic Email Systems:
   - Email server configurations can change frequently, and the script 
     does not guarantee real-time accuracy. It's recommended to verify 
     email addresses through multiple sources for precise accuracy.

3. Performance Considerations:
   - Validation time can vary significantly based on server response time,
     network conditions, and security measures in place. Some email providers
     intentionally slow down verification attempts as an anti-spam measure.
   - The tool tracks and displays validation time for each email check,
     helping you identify problematic or slow servers.

## Use Cases for Internal Use

This email verification tool can be used internally for various purposes, 
including:

1. Email List Cleaning:
   - Reduces bounce rates and improves deliverability by ensuring you're 
     sending emails to valid addresses for marketing campaigns, mailing 
     lists, or CRM systems.

2. Security Audits:
   - Checks if an organization's email servers are properly configured 
     with SPF, DKIM, and MX records, helping prevent phishing and spoofing 
     attacks.

3. Spam Prevention:
   - Filters out potentially malicious or spam-related addresses by 
     checking disposable or blacklisted domains.

4. Catch-All Identification:
   - Identifies catch-all email configurations to determine if a domain 
     accepts all emails, or if more granular checks are required.

5. Compliance and Reporting:
   - Maintains a database log of all email validation attempts for auditing email 
     systems and ensuring compliance with certain regulations or internal 
     standards.

6. Performance Monitoring:
   - Tracks validation time for different email providers, helping you identify
     slow or problematic servers that might affect your email operations.

## Command Reference

The script supports several commands:

- 'help'      - Display help message
- 'show log'  - Display verification history from the database
- 'clear log' - Delete all content from the log database
- 'clear'     - Clear the terminal window
- 'read more' - Learn more about features, functions, and use cases
- 'who am i'  - Display current user information
- 'exit'      - Quit the program

You can also enter one or more email addresses separated by commas to check their validity.
For example: test@example.com, user@domain.com
