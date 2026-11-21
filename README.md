
# NetworkWalks — Week 3: PDF Password Hash Extraction & Dictionary Attacks

**Author:** Kelvin Arigbe  
**Cohort:** B083  
**Programme:** NetworkWalks Cybersecurity  
**Week:** 03  
**Focus:** PDF password-hash extraction, John the Ripper (JtR), and dictionary attacks.

> **Authorization and safety:** Only test PDF files you create yourself or have explicit permission to assess. Do not use this workflow against documents belonging to other people.

## 1. Objective

Understand how password-protected PDF files store password-verification data and how security professionals can assess password strength using John the Ripper.

This exercise practises:

- Confirming that a PDF is password protected.
- Extracting a PDF hash with `pdf2john`.
- Preparing a small, controlled wordlist.
- Running John the Ripper from the command line.
- Reviewing results with `john --show` and optionally Johnny GUI.
- Verifying a recovered lab password against the original test PDF.
- Explaining the limits of dictionary attacks and the importance of strong passwords.

## 2. Tools and Environment

| Tool | Purpose |
|---|---|
| Kali Linux | Lab operating system |
| John the Ripper | Password auditing |
| pdf2john | Extracting PDF password hashes |
| Johnny | Optional graphical interface for John |
| Terminal | Executing commands |
| Test PDF | Self-created password-protected document |

## 3. Lab Preparation

Create a dedicated workspace:

```bash
mkdir -p ~/networkwalks-week3/{sample,hashes,wordlists,output}
cd ~/networkwalks-week3
```

Create a password-protected PDF that you own. Use a temporary lab password that is not used for any real account.

Confirm that the PDF is password protected by attempting to open it.

## 4. Check John the Ripper Installation

Run:

```bash
john --version
```

If John is not installed:

```bash
sudo apt update
sudo apt install john -y
```

Check the installation:

```bash
john --list=formats | grep -i pdf
```

PDF support depends on the installed John build.

## 5. Extract the PDF Hash

Locate the `pdf2john` utility. Depending on your installation, it may be named `pdf2john` or `pdf2john.pl`.

Example:

```bash
pdf2john sample/test.pdf > hashes/pdf_hash.txt
```

If the utility is not in your PATH, locate it:

```bash
find /usr/share/john -iname '*pdf2john*' 2>/dev/null
```

Use the path returned by the command to run the utility.

Inspect the output:

```bash
cat hashes/pdf_hash.txt
```

The extracted hash is sensitive lab material. Do not publish it or commit it to GitHub.

## 6. Prepare a Controlled Wordlist

Create a small wordlist containing only the temporary password you assigned to your test PDF and a few deliberately incorrect examples.

```bash
nano wordlists/lab-wordlist.txt
```

Save the file, then inspect it:

```bash
cat wordlists/lab-wordlist.txt
```

Do not use leaked password databases or passwords belonging to other people.

## 7. Run John the Ripper

Run a dictionary attack against your extracted lab hash:

```bash
john --wordlist=wordlists/lab-wordlist.txt hashes/pdf_hash.txt
```

Check the results:

```bash
john --show hashes/pdf_hash.txt
```

If the password is present in your controlled wordlist and the PDF format is supported, John may recover it.

If the password is not present, the dictionary attack may fail. That is an important result: dictionary attacks depend on the candidates available.

## 8. Optional: Johnny GUI

Johnny provides a graphical interface for John the Ripper.

General workflow:

1. Open Johnny.
2. Load the extracted PDF hash file.
3. Select the wordlist.
4. Start the password audit.
5. Review the result.

The exact interface and available options depend on the installed version.

## 9. Verify the Result

If John reports a recovered password:

1. Open your original test PDF.
2. Enter the recovered lab password.
3. Confirm that the document opens successfully.
4. Compare the result with John's `--show` output.

Do not claim successful recovery unless you have verified it against your own test PDF.

## 10. Troubleshooting

| Problem | Possible explanation |
|---|---|
| `john: command not found` | John is not installed or is not in PATH |
| `pdf2john: command not found` | Utility is located elsewhere or has a different name |
| No password recovered | Candidate password is not in the wordlist |
| Unsupported hash format | Installed John build may lack the required PDF format |
| Empty hash file | Extraction command or PDF path may be incorrect |
| Permission denied | Check file permissions and directory access |

## 11. Security Observations

- Password-protected documents should use strong, unique passwords.
- Short and predictable passwords are more vulnerable to dictionary attacks.
- A dictionary attack only tests the candidates supplied to it.
- A failed dictionary attack does not prove that a password is secure.
- Sensitive hashes and recovered passwords should be protected.
- Organizations should establish policies for document encryption and password management.

## 12. Evidence Checklist

Add your own screenshots to the `evidence/` directory.

Suggested evidence:

- [ ] John the Ripper installation and version.
- [ ] Password-protected test PDF.
- [ ] Successful PDF hash extraction.
- [ ] Controlled wordlist.
- [ ] John the Ripper dictionary attack.
- [ ] `john --show` output.
- [ ] Successful verification of the test PDF.

Before uploading screenshots, redact passwords, extracted hashes, personal information, and unrelated documents.

## 13. Lessons Learned

This exercise demonstrates how password-protected PDF files can be assessed through hash extraction and controlled dictionary attacks.

It also highlights why password complexity, uniqueness, and secure document-handling practices matter.

The outcome of a dictionary attack depends on the quality of the candidate wordlist and the password being tested.

## 14. References

- [John the Ripper — Official Website](https://www.openwall.com/john/)
- [John the Ripper — Documentation](https://www.openwall.com/john/doc/)
- [Kali Linux Tools — John](https://www.kali.org/tools/john/)

---

**NetworkWalks | Week 03 | B083 | Kelvin Arigbe**
