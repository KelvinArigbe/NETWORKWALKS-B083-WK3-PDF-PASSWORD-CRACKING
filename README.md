# NetworkWalks — Week 3: PDF Password Hash Extraction & Dictionary Testing

**Author:** Kelvin Arigbe  
**Cohort:** B083  
**Programme:** NetworkWalks Cybersecurity  
**Week:** 03  
**Focus:** PDF password-hash extraction, John the Ripper (JtR), Johnny GUI, and dictionary-based password auditing.

> **Authorization and safety:** Only test PDF files you created or are explicitly authorized to assess. This lab uses a deliberately weak, self-created sample and a tiny wordlist. Do not use these steps against third-party documents, accounts, or data.

## 1. Objective

Understand how password-protected PDFs expose password-verification data for offline auditing, how a PDF hash is extracted in a John-compatible format, and how John the Ripper tests candidate passwords from a dictionary.

This exercise practises:
- Confirming that a PDF is password protected.
- Extracting a PDF hash with `pdf2john`.
- Preparing a small, controlled wordlist.
- Running John the Ripper from the command line.
- Reviewing the result with `john --show` and optionally Johnny.
- Verifying a recovered lab password against the original test PDF.
- Explaining the limits of dictionary attacks and the importance of strong passwords.

## 2. Workflow

1. Create a non-sensitive PDF and protect it with a weak lab-only password.
2. Confirm the PDF prompts for a password.
3. Extract a John-compatible hash locally using `pdf2john`.
4. Run John against the hash with the provided small lab wordlist.
5. Review the result with `john --show`.
6. Verify the password by opening your own sample PDF.
7. Optionally repeat using Johnny, the graphical interface for John.

## 3. Tools

| Tool | Purpose |
|---|---|
| John the Ripper Jumbo | Offline password auditing and dictionary testing |
| `pdf2john` helper | Extract a John-compatible hash from a protected PDF |
| Johnny | Graphical front-end for John the Ripper |
| Python 3 | Create/maintain lab helper files |
| LibreOffice or another PDF creator | Create your own protected test PDF |
| NetworkWalks Hash Calculator | Optional course demonstration tool; use only with your own test document |

Official project: https://www.openwall.com/john/  
Documentation: https://www.openwall.com/john/doc/

## 4. Create a test PDF

Create a short document containing non-sensitive text, export it as `sample/lab-sample.pdf`, and apply a password you choose solely for this exercise. Add that same lab-only candidate to `wordlists/lab-wordlist.txt`.

Do not use a personal, work, or reused password. The PDF is intentionally kept out of Git so that this repository contains no protected document or recovered secret.

## 5. Extract the PDF hash

Use the `pdf2john` helper supplied with your John the Ripper **Jumbo** installation. The helper's filename and path vary by operating system and package.

Linux/Kali example (adjust the helper path to your installation):

```bash
python3 /path/to/pdf2john.py sample/lab-sample.pdf > output/pdf.hash
```

Windows PowerShell example (adjust the path and script name to the helper in your Jumbo package):

```powershell
python .\run\pdf2john.py .\sample\lab-sample.pdf | Out-File -Encoding ascii .\output\pdf.hash
```

Check that the output file is non-empty:

```bash
ls -l output/pdf.hash
```

Treat extracted hashes as sensitive. Do not commit or publish the hash, especially if it relates to a real document.

## 6. Prepare the controlled wordlist

The repository includes a tiny illustrative wordlist at `wordlists/lab-wordlist.txt`. Add the weak lab-only password chosen for your own PDF. Keep the list small: the learning goal is to understand the process, not to run large-scale cracking.

## 7. Run John the Ripper

```bash
john --wordlist=wordlists/lab-wordlist.txt output/pdf.hash
john --show output/pdf.hash
```

If John rejects the extracted hash, check that you used the helper from a compatible Jumbo installation and that its output was written without formatting changes. Consult the John documentation for version-specific hash formats.

## 8. Johnny GUI (optional)

1. Open Johnny and confirm it can locate the John executable.
2. Load the hash file generated from your own protected test PDF.
3. Start a new dictionary attack.
4. Select the small lab wordlist.
5. Start the test and review the status/results.
6. Use the result only to verify the password for your own sample PDF.

Johnny's labels and menus can vary by version. Do not load real user credential dumps or third-party files.

## 9. Verify

Open `sample/lab-sample.pdf` and enter the lab password reported by John. Record whether the document opens. A successful match only shows that this candidate existed in the supplied wordlist; it does not mean dictionary attacks can recover every password.

## 10. Troubleshooting

| Symptom | Checks |
|---|---|
| `pdf2john` not found | Confirm a Jumbo build is installed and locate its helper script. |
| John rejects the hash | Re-extract using the matching helper and inspect the generated format. |
| No password found | Confirm the hash belongs to your sample PDF and the lab candidate is in the wordlist. |
| Johnny cannot find John | Set the John executable path in Johnny's settings. |
| PDF does not open | Confirm you are testing the same PDF and enter the recovered lab password carefully. |

## 11. Security observations

- Dictionary attacks test likely candidate passwords against an extracted hash; they do not automatically decrypt every protected document.
- Long, unique, unpredictable passwords are harder to guess than common words and predictable substitutions.
- PDF encryption method and configuration also affect security.
- Extracted hashes and recovered passwords should be handled as sensitive information.
- Use a password manager to generate and store unique passwords.

## 12. Evidence checklist

Add screenshots from your own lab to `evidence/` after checking them for secrets and personal information:

- [ ] John/Johnny installation and executable configuration.
- [ ] Your test PDF showing a password prompt (do not show the password).
- [ ] Hash extraction process (redact the hash value).
- [ ] John dictionary test running against the lab hash.
- [ ] `john --show` result (redact the recovered password).
- [ ] Successful opening of your own test PDF (redact the password).

See `evidence/README.md` for naming and redaction guidance.

## 13. What I learned

This lab connects file protection, password-hash extraction, and offline password auditing into one repeatable workflow. I practised both command-line and GUI-based use of John the Ripper, interpreted its output, and documented the limits of dictionary-based testing. The key takeaway is that password strength and uniqueness directly affect resistance to guessing attacks.

## 14. References

- John the Ripper: https://www.openwall.com/john/
- John the Ripper documentation: https://www.openwall.com/john/doc/
- NetworkWalks Hash Calculator: https://networkwalks.com/hash-calculator/

---

**Project:** NetworkWalks Cybersecurity — Week 03  
**Author:** Kelvin Arigbe  
**Cohort:** B083
